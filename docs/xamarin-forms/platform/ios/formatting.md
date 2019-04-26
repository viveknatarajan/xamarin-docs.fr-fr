---
title: Ajout de mise en forme spécifique à iOS
description: Cet article explique comment définir l’apparence de spécifique à iOS sans utiliser un convertisseur personnalisé Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946391"
---
# <a name="adding-ios-specific-formatting"></a>Ajout de mise en forme spécifique à iOS

Pour définir spécifique à iOS mise en forme consiste à créer un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) pour un contrôle et les styles spécifiques à la plateforme de jeu et les couleurs pour chaque plateforme.

Autres options pour contrôler la manière dont les apparence de votre application iOS de Xamarin.Forms sont les suivantes :

* Configuration d’afficher les options dans [ **Info.plist**](#info-plist)
* Définir des styles de contrôle via le [ `UIAppearance` API](#uiappearance)

Ces méthodes sont décrites ci-dessous.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Personnalisation du fichier Info.plist

Le **Info.plist** fichier vous permet de configurer certains aspects de renderering d’une application iOS, par exemple comment (et si) la barre d’état s’affiche.

Par exemple, le [exemple Todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/) utilise le code suivant pour définir la barre de couleur et la couleur du texte de navigation sur toutes les plateformes :

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Le résultat est indiqué dans l’extrait de l’écran ci-dessous. Notez que les éléments de barre d’état seront affichent en noirs (cela ne peut pas être défini dans Xamarin.Forms, car c’est une fonctionnalité spécifique à la plateforme).

![](theme-images/status-default-sml.png "iOS thèmes")

Dans l’idéal, la barre d’état serait également blanche - quelque chose nous pouvons faire directement dans le projet iOS. Ajoutez les entrées suivantes à la **Info.plist** pour forcer la barre d’état de couleur blanche :

![](theme-images/info-plist.png "iOS entrées du fichier Info.plist")

ou modifier le correspondantes **Info.plist** fichier directement à inclure :

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Maintenant lorsque l’application est exécutée, la barre de navigation est verte et que son texte blanc (en raison d’une mise en forme de Xamarin.Forms) *et* le texte de barre d’état est également blanc grâce à la configuration spécifique à iOS :

![](theme-images/status-white-sml.png "iOS thèmes")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

Le [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) peut être utilisé pour définir les propriétés visuelles sur de nombreux contrôles iOS *sans* devoir créer un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Ajout d’une seule ligne de code pour le **AppDelegate.cs** `FinishedLaunching` méthode pouvez appliquer un style tous les contrôles d’un type donné en utilisant leurs `Appearance` propriété. Le code suivant contient deux exemples - appliquer un style dans le monde entier de l’onglet de la barre et basculez le contrôle :

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

Par défaut, l’icône de barre d’onglet sélectionné dans un [`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
serait bleu :

![](theme-images/tabbar-default.png "Par défaut d’icône de barre d’onglet dans TabbedPage iOS")

Pour modifier ce comportement, définissez le `UITabBar.Appearance` propriété :

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Ceci provoque l’onglet sélectionné être en vert :

![](theme-images/tabbar-custom.png "Icône de barre d’onglet dans TabbedPage d’iOS vert")

À l’aide de cette API vous permet de personnaliser l’apparence de la Xamarin.Forms `TabbedPage` sur iOS avec très peu de code. Reportez-vous à la [Recipe (Recette) Personnaliser les onglets](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) pour plus d’informations sur l’utilisation d’un convertisseur personnalisé pour définir une police spécifique pour l’onglet.

### <a name="uiswitch"></a>UISwitch

Le `Switch` contrôle est un autre exemple styles peut leur être facilement :

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Ces captures d’écran de deux affichent la valeur par défaut `UISwitch` contrôle sur la gauche et la version personnalisée (paramètre `Appearance`) sur la droite dans le [exemple Todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "UISwitch couleur par défaut") ![](theme-images/switch-custom.png "personnalisé UISwitch couleur")

### <a name="other-controls"></a>Autres contrôles

Plusieurs contrôles d’interface utilisateur iOS peuvent avoir leurs couleurs par défaut et autres attributs à l’aide de la [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Liens associés

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Personnaliser des onglets](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
