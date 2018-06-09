---
title: Ajout de mise en forme spécifiques à iOS
description: Cet article explique comment définir des e/s spécifiques apparence sans utiliser un convertisseur personnalisé Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 74a3cdc340cb09e8adf15ed0dd09315c985d18b5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243529"
---
# <a name="adding-ios-specific-formatting"></a>Ajout de mise en forme spécifiques à iOS

Pour définir des e/s spécifiques de mise en forme consiste à créer un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) pour un contrôle et des styles spécifiques à la plateforme de jeu et des couleurs pour chaque plateforme.

Autres options pour contrôler la manière dont les apparence de votre application iOS Xamarin.Forms sont les suivantes :

* Configuration d’afficher les options dans [ **Info.plist**](#info-plist)
* La définition de styles de contrôle via le [ `UIAppearance` API](#uiappearance)

Ces méthodes sont décrites ci-dessous.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Personnalisation du fichier Info.plist.

Le **Info.plist** fichier vous permet de configurer certains aspects de renderering d’une application iOS, telles que comment (et si) la barre d’état est indiquée.

Par exemple, le [Todo exemple](https://developer.xamarin.com/samples/xamarin-forms/Todo/) utilise le code suivant pour définir la couleur et la couleur du texte de la barre de navigation sur toutes les plates-formes :

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Le résultat est illustré dans l’extrait de l’écran ci-dessous. Notez que les éléments de barre d’état sont noirs (cela ne peut pas être définie dans Xamarin.Forms, car il s’agit d’une fonctionnalité spécifique à la plateforme).

![](theme-images/status-default-sml.png "iOS thèmes")

Dans l’idéal, la barre d’état serait également blanche - quelque chose nous pouvons effectuer directement dans le projet iOS. Ajoutez les entrées suivantes pour le **Info.plist** pour forcer la barre d’état de couleur blanche :

![](theme-images/info-plist.png "iOS Info.plist entrées")

ou modifier correspondant **Info.plist** fichier directement à inclure :

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Maintenant, lorsque l’application est exécutée, la barre de navigation est verte et son texte est blanc (en raison d’une mise en forme de Xamarin.Forms) *et* le texte de la barre d’état est également blanc grâce à une configuration spécifique à iOS :

![](theme-images/status-white-sml.png "iOS thèmes")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

Le [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) peut être utilisé pour définir les propriétés visuelles sur de nombreux contrôles iOS *sans* avoir à créer un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Ajout d’une seule ligne de code pour le **AppDelegate.cs** `FinishedLaunching` (méthode), appliquez le style tous les contrôles d’un type donné à l’aide de leurs `Appearance` propriété. Le code suivant contient deux exemples - global de l’onglet conception de styles de la barre et passer le contrôle :

**AppDelegate.cs** dans le projet iOS

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

Par défaut, l’icône de barre d’onglet sélectionné dans un [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) serait bleue :

![](theme-images/tabbar-default.png "Par défaut iOS icône de barre d’onglet dans TabbedPage")

Pour modifier ce comportement, définissez la `UITabBar.Appearance` propriété :

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Cela entraîne l’onglet sélectionné en vert :

![](theme-images/tabbar-custom.png "Icône de barre d’onglet dans TabbedPage d’iOS vert")

À l’aide de cette API vous permet de personnaliser l’apparence de la Xamarin.Forms `TabbedPage` sur iOS avec très peu de code. Reportez-vous à la [recette de personnaliser les onglets](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) pour plus d’informations sur l’utilisation d’un convertisseur personnalisé pour définir une police spécifique pour l’onglet.

### <a name="uiswitch"></a>UISwitch

Le `Switch` contrôle est un autre exemple qui peut être facilement style :

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Ces captures d’écran de deux affichent la valeur par défaut `UISwitch` contrôle sur la gauche et la version personnalisée (paramètre `Appearance`) sur la droite dans le [Todo exemple](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Par défaut UISwitch couleur") ![ ] (theme-images/switch-custom.png "personnalisé UISwitch couleur")

### <a name="other-controls"></a>Autres contrôles

Plusieurs contrôles d’interface utilisateur iOS peuvent avoir leurs couleurs par défaut et autres attributs à l’aide de la [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Liens associés

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Personnaliser des onglets](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
