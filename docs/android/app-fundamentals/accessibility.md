---
title: Accessibilité sur Android
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 4eb4a97a346f3906c925dc9e324ed9378af0b560
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116170"
---
# <a name="accessibility-on-android"></a>Accessibilité sur Android

Cette page décrit comment utiliser les API d’accessibilité Android pour créer des applications en fonction de la [liste de contrôle d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md).
Reportez-vous à la [iOS accessibilité](~/ios/app-fundamentals/accessibility.md) et [l’accessibilité du système d’exploitation X](~/mac/app-fundamentals/accessibility.md) pages pour les autres API de la plateforme.


## <a name="describing-ui-elements"></a>Décrivant les éléments d’interface utilisateur

Android fournit un `ContentDescription` propriété qui est utilisée par les API de lecture écran afin de fournir une description accessible de l’objectif du contrôle.

La description du contenu peut être définie dans le C# ou dans le fichier de disposition AXML.

**C#**

La description peut être définie dans le code à n’importe quelle chaîne (ou une ressource de chaîne) :

```csharp
saveButton.ContentDescription = "Save data";
```

**Mise en page AXML**

En XML dispositions, utilisez le `android:contentDescription` attribut :

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Utilisez l’indicateur pour TextView

Pour `EditText` et `TextView` contrôles d’entrée de données, utilisez le `Hint` propriété afin de fournir une description de l’entrée qu’est attendue (au lieu de `ContentDescription`).
Lorsque du texte a été saisi, le texte lui-même sera « lue » au lieu de l’indicateur.

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

Pour associer une étiquette à un contrôle d’entrée de données, utilisez le `LabelFor` propriété

**C#**

Dans C#, définissez le `LabelFor` décrit la propriété à l’ID de ressource du contrôle ce ce contenu (en général, cette propriété est définie sur une étiquette et fait référence à un autre contrôle d’entrée) :

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**Mise en page AXML**

Dans la mise en page XML utilisent le `android:labelFor` propriété à référencer l’identificateur d’un autre contrôle :

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

Utilisez le `AnnounceForAccessibility` permet d’afficher (méthode) sur n’importe quel contrôle de communiquer un changement d’état ou des événements aux utilisateurs lors de l’accessibilité est activée. Cette méthode n’est pas requise pour la plupart des opérations où la narration intégrée fournit des commentaires suffisantes, mais doit être utilisée dans lequel des informations supplémentaires peut s’avérer utiles pour l’utilisateur.

Le code ci-dessous montre un appel d’exemple simple `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Modification des paramètres de Focus

Navigation accessible s’appuie sur les contrôles ayant le focus à l’aide de l’utilisateur à comprendre quelles opérations sont disponibles. Android fournit un `Focusable` propriété que pouvez marquer des contrôles comme spécifiquement en mesure de recevoir le focus lors de la navigation.

**C#**

Pour empêcher d’un contrôle le focus avec C#, définissez le `Focusable` propriété `false`:

```csharp
label.Focusable = false;
```

**Mise en page AXML**

Dans la mise en page ensemble de fichiers XML le `android:focusable` attribut :

```xml
<android:focusable="false" />
```

Vous pouvez contrôler également l’ordre de focus avec la `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` attributs, généralement définies dans la disposition AXML. Utilisez ces attributs pour garantir que l’utilisateur peut naviguer facilement dans les contrôles sur l’écran.


## <a name="accessibility-and-localization"></a>Accessibilité et la localisation

Dans les exemples ci-dessus, la description de l’indicateur et le contenu sont directement défini la valeur d’affichage. Il est préférable d’utiliser des valeurs dans un **Strings.xml** fichier, comme ceci :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

En utilisant le texte à partir d’un fichier de chaînes est illustrée ci-dessous C# et les fichiers de disposition AXML :

**C#**

Au lieu d’utiliser des littéraux de chaîne dans le code, rechercher les valeurs traduites à partir des fichiers de chaînes avec `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

Dans la mise en forme XML telles que les attributs d’accessibilité `hint` et `contentDescription` peut être définie sur un identificateur de chaîne :

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

Suivez [suit](http://developer.android.com/training/accessibility/testing.html#how-to) pour activer TalkBack et Explorer par Touch tester l’accessibilité sur les appareils Android.

Vous devrez peut-être installer [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) à partir de Google Play si elle n’apparaît pas dans **Paramètres > accessibilité**.


## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [API d’accessibilité Android](http://developer.android.com/guide/topics/ui/accessibility/index.html)
