---
title: Polices
description: "Définition des polices dans Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 771e1607bc4e6be8f0991e159b5d34f6d4ea9c02
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="fonts"></a>Polices

Cet article décrit comment Xamarin.Forms vous permet de spécifier les attributs de police (y compris la taille et la pondération) sur des contrôles qui affichent du texte. Informations sur la police peuvent être [spécifiées dans le code](#Setting_Font_in_Code) ou [spécifié en Xaml](#Setting_Font_in_Xaml).
Il est également possible d’utiliser un [police personnalisée](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Définir la police dans le Code

Utilisez les trois propriétés liées à la police de tous les contrôles qui affichent du texte :

- **FontFamily** &ndash; le `string` nom de la police.
- **FontSize** &ndash; la taille de police comme un `double`.
- **Attributs de la police** &ndash; une chaîne qui spécifie les informations de style comme *italique* et **gras** (à l’aide de la `FontAttributes` énumération en c#).

Ce code montre comment créer une étiquette et spécifier la taille de police et le poids à afficher :

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Taille de police

Le `FontSize` propriété peut être définie à une valeur double, par exemple :

```csharp
label.FontSize = 24;
```

Vous pouvez également utiliser le `NamedSize` énumération qui possède quatre options intégrées ; Xamarin.Forms choisit la taille optimale pour chaque plateforme.

-  **Micro**
-  **Small**
-  **Medium**
-  **Large**


Le `NamedSize` énumération ne peut être utilisé partout où un `FontSize` peut être spécifié à l’aide de la `Device.GetNamedSize` méthode pour convertir la valeur en un `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Attributs de police

Styles de police comme **gras** et *italique* peut être défini sur le `FontAttributes` propriété. Les valeurs suivantes sont actuellement prises en charge :

-  **Aucun**
-  **Gras**
-  **Italic**

Le `FontAttribute` énumération peut être utilisée comme suit (vous pouvez spécifier un seul attribut ou `OR` ensemble) :

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

Certains contrôles Xamarin.Forms (tel que `Label`) prennent également en charge les différents attributs de police au sein d’une chaîne à l’aide de la `FormattedString` classe. A `FormattedString` se compose d’une ou plusieurs `Span`s, chacune d’elles peut avoir sa propre mise en forme des attributs.

La `Span` classe possède les attributs suivants :

* **Texte** &ndash; la valeur à afficher
* **FontFamily** &ndash; le nom de police
* **FontSize** &ndash; la taille de police
* **Attributs de la police** &ndash; telles que les informations de style *italique* et **en gras**
* **ForegroundColor** &ndash; couleur du texte
* **BackgroundColor** &ndash; couleur d’arrière-plan

Un exemple de création et affichage d’un `FormattedString` ci-dessous &ndash; Notez qu’il est attribué aux étiquettes de le `FormattedText` propriété et non le `Text` propriété.

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>Définition des informations de police par la plateforme

Vous pouvez également le `Device.RuntimePlatform` propriété peut être utilisée pour définir des noms de police différente sur chaque plateforme, comme illustré dans ce code :

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Une bonne source d’informations sur la police pour iOS est [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Définition de la police en Xaml

Xamarin.Forms contrôle ce texte d’affichage ont tous un `Font` propriété qui peut être définie en Xaml. La façon la plus simple pour définir la police en Xaml est d’utiliser les valeurs d’énumération de taille nommée, comme indiqué dans cet exemple :

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Il existe un convertisseur intégré pour le `Font` propriété qui permet à tous les paramètres de police être exprimé comme une valeur de chaîne en Xaml. Les exemples suivants montrent comment vous pouvez spécifier des attributs de police et les tailles en Xaml :

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Pour spécifier plusieurs `Font` combiner des paramètres, les paramètres requis dans une chaîne d’attribut de police. La chaîne d’attribut de police doit être mis en forme en tant que `"[font-face],[attributes],[size]"`. L’ordre des paramètres est important, tous les paramètres sont facultatifs et plusieurs `attributes` peut être spécifié, par exemple :

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

La `FormattedString` classe peut également être utilisée en XAML, comme indiqué ici :

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) peut également être utilisés en XAML pour restituer une police différente sur chaque plateforme. L’exemple ci-dessous utilise un type de police personnalisée sur iOS (<span style="font-family:MarkerFelt-Thin">MarkerFelt-fin</span>) et spécifie uniquement/attributs de taille sur d’autres plateformes :

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Lorsque vous spécifiez un type de police personnalisée, il est toujours judicieux d’utiliser `OnPlatform`, car il est difficile de trouver une police n’est disponible sur toutes les plateformes.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>À l’aide d’une police personnalisée

À l’aide d’une police autre que les polices intégrés requiert certains codage spécifique à la plateforme. Cette capture d’écran montre la police personnalisée **correspondance** de [les polices open source de Google](https://www.google.com/fonts) rendus sur iOS, Android et Windows Phone à l’aide de Xamarin.Forms.

 [![Une police personnalisée sur iOS et Android](fonts-images/custom-sml.png "exemple des polices personnalisées")](fonts-images/custom.png#lightbox "exemple des polices personnalisées")

Les étapes requises pour chaque plateforme sont décrites ci-dessous. En incluant des fichiers de police personnalisée à une application, veillez à vérifier que les licences de la police autorise pour la distribution.

### <a name="ios"></a>iOS

Il est possible d’afficher une police personnalisée par tout d’abord s’assurer qu’il est chargé, puis référence par nom à l’aide de la Xamarin.Forms `Font` méthodes.
Suivez les instructions de [ce billet de blog](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Ajouter le fichier de polices avec **Action de génération : BundleResource**, et
2. Mise à jour la **Info.plist** fichier (**polices fournies par application**, ou `UIAppFonts`, key), puis
3. Faire par nom partout où vous définissez une police dans Xamarin.Forms !

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms pour Android peut faire référence à une police personnalisée qui a été ajoutée au projet en suivant une norme d’affectation de noms spécifique. Tout d’abord ajouter le fichier de polices pour le **actifs** dossier dans le projet d’application et le jeu *Action de génération : AndroidAsset*. Puis utilisez le chemin d’accès complet et *nom de la police* séparés par un signe dièse (#) en tant que le nom de la police dans Xamarin.Forms, comme l’extrait de code ci-dessous montre :

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms pour les plateformes Windows peut faire référence à une police personnalisée qui a été ajoutée au projet en suivant une norme d’affectation de noms spécifique. Tout d’abord ajouter le fichier de polices pour le **/Assets/polices/** dossier dans le projet d’application et le jeu le <span class="UIItem">Build Action : contenu</span>. Puis utilisez le nom de chemin d’accès et de la police complète, suivi par un signe dièse (#) et le <span class="UIItem">nom de la police</span>, comme l’extrait de code ci-dessous montre :

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Notez que le nom de fichier de police et le nom de la police peuvent être différents. Pour découvrir le nom de la police sur Windows, cliquez sur le fichier .ttf et sélectionnez **aperçu**. Le nom de la police peut ensuite être déterminé à partir de la fenêtre d’aperçu.

Le code commun pour l’application est maintenant complet. Le code du numéroteur téléphonique spécifique à la plateforme sera désormais implémenté comme [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Vous pouvez également utiliser [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) en XAML pour restituer une police personnalisée :

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Xamarin.Forms fournit des paramètres par défaut simple pour vous permettre de taille texte facilement pour toutes les plateformes prises en charge. Il vous permet également de spécifier le type de police et taille &ndash; même différemment pour chaque plateforme &ndash; lorsqu’un contrôle plus précis est requis. Le `FormattedString` classe peut être utilisée pour construire une chaîne contenant les spécifications de police différente à l’aide de la `Span` classe.

Informations sur la police peuvent également être spécifiées en Xaml à l’aide de des attributs de police correctement mis en forme ou le `FormattedString` élément avec `Span` enfants.


## <a name="related-links"></a>Liens associés

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
