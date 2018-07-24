---
title: Polices dans Xamarin.Forms
description: Cet article explique comment spécifier des informations de police sur les contrôles qui affichent du texte dans les applications Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e6635bc13214a5a4e728fa3e71db86a8ea1c39d6
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202953"
---
# <a name="fonts-in-xamarinforms"></a>Polices dans Xamarin.Forms

Cet article décrit comment Xamarin.Forms vous permet de spécifier les attributs de police (y compris les poids et taille) sur les contrôles qui affichent du texte. Informations de police peuvent être [spécifiée dans le code](#Setting_Font_in_Code) ou [spécifié dans XAML](#Setting_Font_in_Xaml).
Il est également possible d’utiliser un [police personnalisée](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Définition de police dans le Code

Utilisez les trois propriétés liées à la police de tous les contrôles qui affichent du texte :

- **FontFamily** &ndash; le `string` nom de la police.
- **FontSize** &ndash; la taille de police comme un `double`.
- **Attributs de la police** &ndash; une chaîne qui spécifie les informations de style comme *italique* et **gras** (à l’aide de la `FontAttributes` énumération en c#).

Ce code montre comment créer une étiquette et de spécifier la taille de police et le poids à afficher :

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Taille de police

Le `FontSize` propriété peut être définie sur une valeur double, par exemple :

```csharp
label.FontSize = 24;
```

Vous pouvez également utiliser le `NamedSize` énumération qui possède quatre options intégrées ; Xamarin.Forms choisit la taille optimale pour chaque plateforme.

-  **Micro**
-  **Petit**
-  **Moyenne**
-  **Grand**

Le `NamedSize` énumération ne peut être utilisé partout où une `FontSize` peut être spécifié à l’aide de la `Device.GetNamedSize` méthode pour convertir la valeur en un `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Attributs de police

Styles de police comme **gras** et *italique* peuvent être définies sur le `FontAttributes` propriété. Les valeurs suivantes sont actuellement prises en charge :

-  **Aucun**
-  **Gras**
-  **Italique**

Le `FontAttribute` énumération peut être utilisée comme suit (vous pouvez spécifier un seul attribut ou `OR` utilisez-les) :

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>Définition des informations de police par plateforme

Vous pouvez également le `Device.RuntimePlatform` propriété peut être utilisée pour définir des noms de polices différentes sur chaque plateforme, comme illustré dans ce code :

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Est une bonne source d’informations de police pour iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Définition de la police dans XAML

Xamarin.Forms contrôle ce texte d’affichage ont toutes un `Font` propriété qui peut être définie dans XAML. Le plus simple pour définir la police dans XAML consiste à utiliser les valeurs d’énumération taille nommée, comme illustré dans cet exemple :

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Il existe un convertisseur intégré pour le `Font` propriété qui permet à tous les paramètres de police à être exprimé comme une valeur de chaîne dans XAML. Les exemples suivants montrent comment vous pouvez spécifier des attributs de police et de tailles dans XAML :

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Pour spécifier plusieurs `Font` combiner des paramètres, les paramètres requis dans un seul `Font` chaîne d’attribut. La chaîne d’attribut de police doit être mis en forme en tant que `"[font-face],[attributes],[size]"`. L’ordre des paramètres est important, tous les paramètres sont facultatifs et plusieurs `attributes` peut être spécifié, par exemple :

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) peut également être utilisé dans XAML pour restituer une police différente sur chaque plateforme. L’exemple ci-dessous utilise un type de police personnalisée sur iOS (<span style="font-family:MarkerFelt-Thin">MarkerFelt-fin</span>) et spécifie uniquement taille/attributs sur d’autres plateformes :

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Lorsque vous spécifiez un type de police personnalisée, il est toujours judicieux d’utiliser `OnPlatform`, comme il est difficile de trouver une police qui n’est disponible sur toutes les plateformes.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>À l’aide d’une police personnalisée

À l’aide d’une police autres que les polices intégrées nécessite du codage spécifique à la plateforme. Cette capture d’écran montre la police personnalisée **Lobster** de [polices d’open source de Google](https://www.google.com/fonts) restitué à l’aide de Xamarin.Forms.

 [![Une police personnalisée sur iOS et Android](fonts-images/custom-sml.png "personnalisé polices exemple")](fonts-images/custom.png#lightbox "exemple des polices personnalisées")

Les étapes requises pour chaque plateforme sont décrites ci-dessous. En incluant des fichiers de police personnalisée avec une application, veillez à vérifier que les licences de la police autorise pour la distribution.

### <a name="ios"></a>iOS

Il est possible d’afficher une police personnalisée, tout d’abord s’assurer qu’elle est chargée, puis référence par nom à l’aide de Xamarin.Forms `Font` méthodes.
Suivez les instructions de [ce billet de blog](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Ajouter le fichier de police avec **Action de génération : BundleResource**, et
2. Mise à jour le **Info.plist** fichier (**polices fournies par l’application**, ou `UIAppFonts`, key), puis
3. Reporter en nom partout où vous définissez une police dans Xamarin.Forms !

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms pour Android peut référencer une police personnalisée qui a été ajoutée au projet en suivant une norme d’affectation de noms spécifique. Tout d’abord ajouter le fichier de police pour le **actifs** dossier dans le projet d’application et un jeu *Action de génération : AndroidAsset*. Puis utilisez le chemin d’accès complet et *nom de la police* séparés par un signe dièse (#) comme nom de police dans Xamarin.Forms, comme le montre l’extrait de code ci-dessous :

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms pour les plateformes Windows peut référencer une police personnalisée qui a été ajoutée au projet en suivant une norme d’affectation de noms spécifique. Tout d’abord ajouter le fichier de police pour le **/Assets/polices/** dossier dans le projet d’application et le jeu le <span class="UIItem">Build Action : contenu</span>. Puis utilisez le nom de chemin d’accès et de la police complète, suivi d’un signe dièse (#) et le <span class="UIItem">nom de la police</span>, comme le montre l’extrait de code ci-dessous :

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Notez que le nom de fichier de police et le nom de police peuvent être différentes. Pour découvrir le nom de la police sur Windows, cliquez sur le fichier .ttf et sélectionnez **aperçu**. Le nom de la police peut alors être déterminé à partir de la fenêtre d’aperçu.

Le code commun pour l’application est maintenant complet. Le code du numéroteur téléphonique spécifique à la plateforme sera désormais implémenté comme [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Vous pouvez également utiliser [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) dans XAML pour restituer une police personnalisée :

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Xamarin.Forms fournit des paramètres par défaut simple pour vous permettre de taille de texte facilement pour toutes les plateformes prises en charge. Il vous permet également de spécifier le type de police et la taille &ndash; même différemment pour chaque plateforme &ndash; lorsqu’un contrôle plus précis est requis.

Informations de la police peuvent également être spécifiées dans XAML à l’aide des attributs de police correctement mis en forme.

## <a name="related-links"></a>Liens associés

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
