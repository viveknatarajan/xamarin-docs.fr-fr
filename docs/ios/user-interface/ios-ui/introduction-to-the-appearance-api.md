---
title: Apparence API
description: "iOS vous permet d’appliquer des paramètres de propriété visuelle à un niveau de la classe statique plutôt que sur des objets spécifiques afin que la modification s’applique à toutes les instances de ce contrôle dans l’application."
ms.topic: article
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f35256529d6d72a3f5e563dc88b9d5883a9724d4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="appearance-api"></a>Apparence API

_iOS vous permet d’appliquer des paramètres de propriété visuelle à un niveau de la classe statique plutôt que sur des objets spécifiques afin que la modification s’applique à toutes les instances de ce contrôle dans l’application._

Cette fonctionnalité est exposée dans Xamarin.iOS via statique `Appearance` propriété sur tous les contrôles de UIKit qui prennent en charge. Apparence visuelle (comme les propriétés en tant qu’image d’arrière-plan et de couleur de teinte) peut par conséquent être facilement personnalisé pour permettre à votre application une apparence cohérente. L’API d’apparence introduite dans iOS 5, tandis que certaines parties de celui-ci ont été déconseillées dans iOS 9 il est toujours un bon moyen d’obtenir certains styles et les effets des thèmes dans les applications Xamarin.iOS.

## <a name="overview"></a>Vue d'ensemble

iOS permet de que vous personnalisez l’apparence de nombreux contrôles UIKit pour rendre les contrôles standard sont conformes à la personnalisation que vous souhaitez appliquer à votre application.

Il existe deux façons différentes d’appliquer une apparence personnalisée :

- **Directement sur une instance de contrôle** – vous pouvez définir la couleur de teinte, image d’arrière-plan et position du titre (ainsi que les autres attributs) sur de nombreux contrôles, y compris les barres d’outils, des barres de navigation, des boutons et des curseurs.

- **Définir les valeurs par défaut sur la propriété statique apparence** – attributs personnalisables pour chaque contrôle sont exposées la `Appearance` propriété statique. Toutes les personnalisations que vous s’appliquent à ces propriétés peuvent être utilisées en tant que la valeur par défaut pour tous les contrôles de ce type est créé après que la propriété est définie.

L’exemple d’application apparence montre les trois méthodes, comme indiqué dans les captures d’écran :

 [![](introduction-to-the-appearance-api-images/appearance01.png "L’exemple d’application apparence montre les trois méthodes")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

À partir d’iOS 8, le proxy de l’apparence a été étendu pour TraitCollections.
 `AppearanceForTraitCollection` peut être utilisé pour définir l’apparence par défaut sur une collection d’une caractéristique particulière. Vous pouvez en savoir plus à ce sujet dans le [Introduction à des plans conceptuels](~/ios/user-interface/storyboards/unified-storyboards.md) guide.


## <a name="setting-appearance-properties"></a>Définition des propriétés d’apparence

Dans le premier écran, la classe d’apparence statique est utilisée pour définir le style les boutons et les éléments jaune/orange comme suit :

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

Les styles d’élément verte sont définies comme suit, dans le `ViewDidLoad` méthode qui remplace les valeurs par défaut et le *apparence* classe statique :

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

L’API d’apparence peuvent être utiles quand [styles de l’application iOS](~/xamarin-forms/platform/ios/theme.md#uiappearance) dans les solutions de Xamarin.Forms. Quelques lignes dans le `AppDelegate` classe permettent d’implémenter un jeu de couleurs spécifique sans avoir à créer un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>UIAppearance et des thèmes personnalisés

iOS permet de nombreux attributs visuels de l’utilisateur à être « thème » à l’aide de contrôles d’interface le *UIAppearance* API pour forcer toutes les instances d’un contrôle particulier à avoir la même apparence. Cela est exposée comme une propriété d’apparence dans de nombreuses classes de contrôle d’interface utilisateur, et non sur des instances individuelles du contrôle. Définition d’une propriété d’affichage sur statiques `Appearance` propriété affecte tous les contrôles de ce type dans votre application.

Pour mieux comprendre le concept, prenons un exemple.

Pour modifier un spécifique `UISegmentedControl` pour avoir une teinte Magenta, nous devez référencer le contrôle spécifique sur notre écran comme suit dans `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Vous pouvez également définir la valeur dans la zone de propriétés du concepteur : 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Teinte de remplissage de propriétés")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

L’image ci-dessous illustre que cela affecte la teinte uniquement du contrôle nommé 'sg1'.

 [![](introduction-to-the-appearance-api-images/image53.png "Définition de la teinte de chacun des contrôles")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Pour définir de nombreux contrôles de cette façon serait complètement inefficace, donc nous pouvons définir à la place de la méthode statique `Appearance` propriété sur la classe elle-même. Ceci est illustré dans le code ci-dessous :

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Maintenant, l’Image ci-dessous illustre les deux contrôles segmentés avec l’apparence définie à Magenta :

 [![](introduction-to-the-appearance-api-images/image54.png "Définition de la teinte de contrôle d’apparence")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` propriétés qui doivent être définies tôt dans le cycle de vie d’application, comme dans le AppDelegate `FinishedLaunching` événement, ou dans un ViewController avant que les contrôles concernés sont affichés.


Reportez-vous à la [Introduction à l’API d’apparence](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) pour plus d’informations.


## <a name="related-links"></a>Liens associés

- [Apparence (exemple)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [Protocole de UIAppearance référence](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Apparence dans Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
