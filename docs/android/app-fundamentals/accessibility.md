---
title: "Accessibilité sur Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 2dea77b4c52db0c032aba9bde471e76eb36ba3ad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="accessibility-on-android"></a>Accessibilité sur Android

Cette page décrit comment utiliser les API d’accessibilité Android pour générer des applications en fonction de la [liste de vérification d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).
Reportez-vous à la [iOS accessibilité](~/ios/app-fundamentals/accessibility.md) et [accessibilité du système d’exploitation X](~/mac/app-fundamentals/accessibility.md) pages pour les autres API de la plateforme.


## <a name="describing-ui-elements"></a>Décrivant les éléments d’interface utilisateur

Android fournit un `ContentDescription` propriété utilisé par l’API de lecture écran afin de fournir une description accessible de l’objectif du contrôle.

La description du contenu peut être définie dans c# ou dans le fichier de disposition AXML.

**C#**

La description peut être définie dans du code pour n’importe quelle chaîne (ou une ressource de chaîne) :

```csharp
saveButton.ContentDescription = "Save data";
```

**Mise en page AXML**

En XML, utilisez des dispositions le `android:contentDescription` attribut :

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Utilisez l’indicateur pour TextView

Pour `EditText` et `TextView` contrôles d’entrée de données, utilisez la `Hint` propriété afin de fournir une description de quel entrée est attendue (au lieu de `ContentDescription`).
Lorsque du texte a été entré, le texte proprement dit est « lu » au lieu de l’indicateur.

**C#**

Définir le `Hint` propriété dans le code :

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**Mise en page AXML**

En XML, utilisez les fichiers de mise en page le `android:hint` attribut :

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>Les liens LabelFor champs d’entrée avec étiquettes

Pour associer une étiquette avec un contrôle d’entrée de données, utilisez le `LabelFor` propriété

**C#**

En c#, vous devez définir le `LabelFor` décrit la propriété à l’ID de ressource du contrôle ce ce contenu (en général, cette propriété est définie sur une étiquette et fait référence à d’autres contrôles d’entrée) :

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**Mise en page AXML**

Dans la mise en forme XML utilisent le `android:labelFor` propriété à référencer l’identificateur d’un autre contrôle :

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Annoncer pour l’accessibilité

Utilisez la `AnnounceForAccessibility` méthode sur n’importe quel afficher le contrôle de communiquer une modification d’état ou les événements aux utilisateurs lors de l’accessibilité est activée. Cette méthode n’est pas requise pour la plupart des opérations où la narration intégrée fournit des commentaires suffisantes, mais doit être utilisée lorsque des informations supplémentaires sont utiles pour l’utilisateur.

Le code suivant montre un appel de l’exemple simple `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Modification des paramètres de Focus

Navigation accessible s’appuie sur les contrôles ayant le focus à l’aide de l’utilisateur à comprendre quelles opérations sont disponibles. Android fournit un `Focusable` propriété pour laquelle vous pouvez marquer des contrôles comme spécifiquement en mesure de recevoir le focus lors de la navigation.

**C#**

Pour empêcher un contrôle à partir de l’obtention du focus avec c#, définissez la `Focusable` propriété `false`:

```csharp
label.Focusable = false;
```

**Mise en page AXML**

Dans la disposition des fichiers XML ensemble la `android:focusable` attribut :

```xml
<android:focusable="false" />
```

Vous pouvez également contrôler la commande le focus avec la `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` attributs, généralement définies dans la mise en page AXML. Utiliser ces attributs pour s’assurer de que l’utilisateur peut parcourir facilement les contrôles sur l’écran.


## <a name="accessibility-and-localization"></a>Accessibilité et la localisation

Dans les exemples ci-dessus, la description de l’indicateur et le contenu sont directement défini comme la valeur d’affichage. Il est préférable d’utiliser des valeurs dans une **Strings.xml** fichier, comme ceci :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

À l’aide du texte à partir d’un fichier de chaînes ci-dessous dans c# et les fichiers de mise en page AXML :

**C#**

Au lieu d’utiliser des littéraux de chaîne dans le code, rechercher les valeurs traduites à partir de fichiers de chaînes avec `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

Dans la mise en forme XML, les attributs d’accessibilité comme `hint` et `contentDescription` peut être défini sur un identificateur de chaîne :

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

L’avantage de stocker du texte dans un fichier distinct est que plusieurs traductions de langue du fichier peuvent être fournies dans votre application. Consultez le [guide de localisation Android](~/android/app-fundamentals/localization.md) pour savoir comment ajouter des fichiers de la chaîne localisée à un projet d’application.


## <a name="testing-accessibility"></a>Test de l’accessibilité

Suivez [suit](http://developer.android.com/training/accessibility/testing.html#how-to) pour activer TalkBack et Explorer par tactile tester l’accessibilité sur les appareils Android.

Il se pouvez que vous deviez installer [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) à partir de Google Play si elle n’apparaît pas dans **Paramètres > accessibilité**.


## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [API d’accessibilité Android](http://developer.android.com/guide/topics/ui/accessibility/index.html)
