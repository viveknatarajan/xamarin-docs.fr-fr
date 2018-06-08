---
title: Localisation de droite à gauche
description: Localisation de droite à gauche ajoute la prise en charge de la direction du flux de droite à gauche aux applications de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 94cf937a810e84ead0bdc1f69933968b42090a6f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846430"
---
# <a name="right-to-left-localization"></a>Localisation de droite à gauche

_Localisation de droite à gauche ajoute la prise en charge de la direction du flux de droite à gauche aux applications de Xamarin.Forms._

> [!NOTE]
> Localisation de droite à gauche requiert l’utilisation d’API 17 ou une version ultérieure sur Android et iOS 9 ou version ultérieure.

Sens du flux est la direction dans laquelle les éléments d’interface utilisateur sur la page sont analysés par le œil. Certains langages, tels que l’arabe et hébreu, requièrent que les éléments d’interface utilisateur sont disposés dans un sens de flux de droite à gauche. Cela peut être obtenue en définissant le [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété. Cette propriété obtient ou définit le sens dans le flux d’éléments de l’interface utilisateur dans tout élément parent qui contrôle leur disposition et doit être défini à un de la [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) valeurs d’énumération :

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Définition de la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) sur un élément généralement définit l’alignement à droite, l’ordre de lecture de droite à gauche et la disposition du contrôle de flux à partir de droite à gauche :

[![TodoItemPage en chiffres arabes avec un sens de flux de droite à gauche](rtl-images/TodoItemPage-Arabic.png "TodoItemPage en chiffres arabes avec un sens de flux de droite à gauche")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage en chiffres arabes avec un sens de flux de droite à gauche")

> [!TIP]
> Vous ne devez définir le [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété sur la mise en page initiale. Modification de cette valeur lors de l’exécution entraîne un processus coûteux de disposition qui affectent les performances.

La valeur par défaut [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) valeur de propriété pour un élément sans un parent est [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), tandis que la valeur par défaut `FlowDirection` pour un élément avec un parent est [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Par conséquent, un élément hérite le `FlowDirection` valeur de la propriété de son parent dans l’arborescence d’éléments visuels et tout élément pouvez remplacer la valeur qu’il obtient à partir de son parent.

> [!TIP]
> Lors de la localisation d’une application pour des langues de droite à gauche, définissez la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété sur une disposition de page ou de la racine. Conséquence, tous les éléments contenus dans la page, ou la présentation racine, de réagir de façon appropriée à la direction du flux.

## <a name="respecting-device-flow-direction"></a>En respectant la direction du flux du périphérique

En respectant le sens du flux du périphérique basé sur le langage sélectionné et la région est un choix explicite développeur et n’est pas automatique. Il peut être obtenue en définissant le [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété sur une page ou la mise en page de la racine, à la `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) valeur :

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Tous les éléments enfants de la page ou la mise en page de la racine, par défaut puis héritent la [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) valeur.

## <a name="platform-setup"></a>Programme d’installation de plateforme

Le programme d’installation de plateforme spécifique est requis pour activer les paramètres régionaux de droite à gauche.

### <a name="ios"></a>iOS

Les paramètres régionaux de droite à gauche requis doivent être ajouté comme un langage pris en charge pour les éléments de tableau pour le `CFBundleLocalizations` clé **Info.plist**. L’exemple suivant montre l’arabe après avoir été ajouté à la baie pour le `CFBundleLocalizations` clé :

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Les langues prises en charge de info.plist](rtl-images/ios-locales.png "Info.plist les langues prises en charge")

Pour plus d’informations, consultez [principes fondamentaux de la localisation dans iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Localisation de droite à gauche peut être testée en modifiant la langue et région sur le simulateur de périphérique/paramètres régionaux de droite à gauche qui a été spécifié dans **Info.plist**.

> [!WARNING]
> Notez que lorsque vous modifiez la langue et région à une langue de droite à gauche sur iOS, tout [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) vues lève une exception si vous n’incluez pas les ressources requises pour les paramètres régionaux. Par exemple, lorsque vous testez une application en chiffres arabes qui a un `DatePicker`, vérifiez que **mideast** est sélectionné dans le **internationalisation** section de la **Build iOS** volet.

### <a name="android"></a>Android

L’application **AndroidManifest.xml** fichier doit être mis à jour afin que les `<uses-sdk>` les collections de nœuds le `android:minSdkVersion` 17, l’attribut et la `<application>` les collections de nœuds le `android:supportsRtl` attribut `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Localisation de droite à gauche peut être testée en modifiant l’appareil/émulateur pour utiliser la langue de droite à gauche, ou en activant **sens de la disposition de droite à gauche Force** dans **Paramètres > Options pour développeurs**.

### <a name="universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP)

Les ressources linguistiques requises doivent être spécifiés dans le `<Resources>` nœud de la **Package.appxmanifest** fichier. L’exemple suivant montre l’arabe ayant été ajouté à la `<Resources>` nœud :

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

En outre, UWP nécessite que culture de l’application par défaut est explicitement définie dans la bibliothèque .NET Standard. Cela peut être accompli en définissant le `NeutralResourcesLanguage` attribut `AssemblyInfo.cs`, ou dans une autre classe, à la culture par défaut :

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Localisation de droite à gauche peut être testée en modifiant la langue et région sur l’appareil pour les paramètres régionaux de droite à gauche appropriés.

## <a name="limitations"></a>Limitations

Localisation de droite à gauche Xamarin.Forms possède actuellement un nombre de limitations :

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) emplacement du bouton, élément de la barre d’outils emplacement et les animations de transition sont contrôlée par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) ne retourne pas de sens de balayage.
- [`Image`](xref:Xamarin.Forms.Image) ne retourne pas de contenu visuel.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) et [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) l’orientation est contrôlée par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- [`WebView`](xref:Xamarin.Forms.WebView) contenu qui ne respecte pas la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- A `TextDirection` propriété doit être ajouté, pour contrôler l’alignement du texte.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) l’orientation est contrôlée par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) alignement du texte est contrôlé par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) mouvements et alignement ne sont pas inversés.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) l’orientation est contrôlée par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) la sélection élective est contrôlée par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) alignement du texte est contrôlé par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété n’est pas héritée par [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) enfants.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) alignement du texte est contrôlé par les paramètres régionaux de périphérique, plutôt que la [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) propriété.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>De droite à gauche langue prise en charge avec Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Droite à gauche Xamarin.Forms 3.0 prend en charge, par [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Liens connexes

- [TodoLocalizedRTL exemple d’application](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
