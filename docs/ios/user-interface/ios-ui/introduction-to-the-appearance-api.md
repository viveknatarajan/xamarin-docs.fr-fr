---
title: API d’apparence dans Xamarin.iOS
description: iOS vous permet d’appliquer des paramètres de propriété visual au niveau d’une classe statique et non sur des objets spécifiques afin que la modification s’applique à toutes les instances de ce contrôle dans l’application.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0dd9832a2e4dd0803f92d6e3923fe178252211f4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103567"
---
# <a name="appearance-api-in-xamarinios"></a>API d’apparence dans Xamarin.iOS

_iOS vous permet d’appliquer des paramètres de propriété visual au niveau d’une classe statique et non sur des objets spécifiques afin que la modification s’applique à toutes les instances de ce contrôle dans l’application._

Cette fonctionnalité est exposée dans Xamarin.iOS via statique `Appearance` propriété sur tous les contrôles UIKit qui prennent en charge. Apparence visuelle (telles que les propriétés en tant qu’image d’arrière-plan et de couleur de teinte) peut donc être facilement personnalisé pour donner à votre application une apparence cohérente. L’API d’apparence a été introduit dans iOS 5 et bien que certaines parties de celui-ci sont déconseillés dans iOS 9 il est toujours un bon moyen pour obtenir certains styles et les effets de thèmes dans les applications Xamarin.iOS.

## <a name="overview"></a>Vue d'ensemble

iOS permet de que vous personnaliser l’apparence de nombreux contrôles UIKit pour rendre les contrôles standards sont conformes à la personnalisation que vous souhaitez appliquer à votre application.

Il existe deux façons différentes d’appliquer une apparence personnalisée :

- **Directement sur une instance de contrôle** – vous pouvez définir la couleur de teinte, image d’arrière-plan et position du titre (comme ainsi que d’autres attributs) sur de nombreux contrôles, y compris des barres d’outils, des barres de navigation, des boutons et des curseurs.

- **Définir les valeurs par défaut sur la propriété statique apparence** – attributs personnalisables pour chaque contrôle sont exposées via la `Appearance` propriété statique. Les personnalisations que vous s’appliquent à ces propriétés peuvent être utilisées en tant que la valeur par défaut pour n’importe quel contrôle de ce type est créé après que la propriété est définie.

L’exemple d’application apparence montre les trois méthodes, comme indiqué dans ces captures d’écran :

 [![](introduction-to-the-appearance-api-images/appearance01.png "L’exemple d’application apparence montre les trois méthodes")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

À compter d’iOS 8, le proxy de l’apparence a été étendu aux TraitCollections.
 `AppearanceForTraitCollection` peut être utilisé pour définir l’apparence par défaut sur une collection d’une caractéristique particulière. Vous trouverez plus d’informations à ce sujet dans le [Introduction aux Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) guide.


## <a name="setting-appearance-properties"></a>Définition des propriétés d’apparence

Dans le premier écran, la classe d’apparence statique est utilisée pour définir le style les boutons et les éléments de couleur jaune/orange comme suit :

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Les styles de l’élément verte sont définies comme suit, dans le `ViewDidLoad` méthode qui remplace les valeurs par défaut et le *apparence* classe statique :

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>À l’aide de UIAppearance dans Xamarin.Forms

L’API d’apparence peut être utile quand [appliquer un style de l’application iOS](~/xamarin-forms/platform/ios/theme.md#uiappearance) dans les solutions Xamarin.Forms. Quelques lignes dans le `AppDelegate` classe permettent d’implémenter un modèle de couleurs spécifique sans avoir à créer un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>UIAppearance et des thèmes personnalisés

iOS permet de nombreux attributs visuels de l’utilisateur des contrôles d’interface être « thème » à l’aide de la *UIAppearance* API pour forcer toutes les instances d’un contrôle particulier doit avoir la même apparence. Cela est exposé comme une propriété Appearance sur nombreuses classes de contrôle d’interface utilisateur, et non sur des instances individuelles du contrôle. Définition d’une propriété d’affichage sur statiques `Appearance` propriété affecte tous les contrôles de ce type dans votre application.

Pour mieux comprendre le concept, prenons un exemple.

Pour modifier un spécifique `UISegmentedControl` pour avoir une teinte Magenta, nous devez référencer le contrôle spécifique sur notre écran comme celui-ci dans `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Vous pouvez également définir la valeur dans le panneau Propriétés du concepteur : 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Teinte du panneau Propriétés")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

L’image ci-dessous montre que cela affecte la teinte uniquement du contrôle nommé « sg1 ».

 [![](introduction-to-the-appearance-api-images/image53.png "Définition de la teinte de contrôle individuel")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Pour définir de nombreux contrôles de cette façon serait complètement inefficace, donc nous pouvons définir à la place de la méthode statique `Appearance` propriété sur la classe elle-même. Ceci est illustré dans le code ci-dessous :

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Maintenant, l’Image ci-dessous illustre les deux contrôles segmentés avec l’apparence de la valeur Magenta :

 [![](introduction-to-the-appearance-api-images/image54.png "Définition de la teinte de contrôle d’apparence")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` propriétés qui doivent être définies très tôt dans le cycle de vie d’application, comme dans l’AppDelegate `FinishedLaunching` événement, ou dans un ViewController avant que les contrôles concernés sont affichés.


Reportez-vous à la [Introduction à l’API d’apparence](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) pour des informations plus détaillées.


## <a name="related-links"></a>Liens associés

- [Apparence (exemple)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [Référence sur les protocoles de UIAppearance](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Apparence dans Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
