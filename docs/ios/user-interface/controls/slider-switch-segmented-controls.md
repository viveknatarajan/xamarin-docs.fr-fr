---
title: Curseurs, les commutateurs et les contrôles segmentés dans Xamarin.iOS
description: Ce document présente les diapositives, les commutateurs et les contrôles segmentés dans Xamarin.iOS, décrivant la façon de les manipuler par programme et dans le concepteur iOS.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 09a5d9e76c41eba4e16cab041daa67d3a5d8a584
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790027"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Curseurs, les commutateurs et les contrôles segmentés dans Xamarin.iOS

<a name="Sliders" />

## <a name="sliders"></a>Contrôles Slider

Le contrôle slider permet la sélection simple d’une valeur numérique dans une plage. Par défaut, le contrôle à une valeur comprise entre 0 et 1, mais ces limites peuvent être personnalisés.

 [![](slider-switch-segmented-controls-images/image25a.png "Curseur")](slider-switch-segmented-controls-images/image25a.png#lightbox)

La capture d’écran suivante montre les propriétés qui sont modifiables dans le concepteur :

 [![](slider-switch-segmented-controls-images/image26a.png "Propriétés de curseur")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Vous pouvez définir ces valeurs dans le code comme indiqué ci-dessous, y compris le câblage d’un gestionnaire pour afficher la valeur actuellement sélectionnée dans un `UILabel` contrôle :

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Vous pouvez également personnaliser l’apparence visuelle du curseur par paramètre

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Le curseur personnalisé ressemble à ceci :

 [![](slider-switch-segmented-controls-images/image27a.png "Curseur personnalisé")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Il y a actuellement un [bogue](http://stackoverflow.com/a/19496179) à l’origine de la `ThumbTint` pour ne pas afficher au moment de l’exécution comme prévu. Vous pouvez ajouter la ligne suivante de code **avant** le code ci-dessus comme solution de contournement. [[Source](http://stackoverflow.com/a/21396794)] :
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Vous pouvez utiliser n’importe quelle image, tel qu’il sera remplacée, mais assurez-vous qu’il est placé _dans_ le répertoire des ressources et est appelée dans votre code.

<a name="Switch" />

## <a name="switch"></a>Basculer

e/s utilise le `UISwitch` comme entrée d’une valeur booléenne qui peut être représenté par une case sur d’autres plateformes. L’utilisateur peut manipuler le contrôle en déplaçant le *thumb* entre les **désactivez-les** positions.

 [![](slider-switch-segmented-controls-images/image28a.png "Commutateur")](slider-switch-segmented-controls-images/image28a.png#lightbox)

L’apparence du commutateur peut être personnalisé dans le **propriétés remplissage** du concepteur, ce qui vous permettra de contrôler l’état par défaut, **désactivez-les teinte** couleurs et une **on/off Image**. Ceci est illustré dans l’image ci-dessous :

 [![](slider-switch-segmented-controls-images/image29a.png "Propriétés d’un commutateur")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Les propriétés du commutateur peuvent également être définies dans le code, par exemple le code suivant affiche un commutateur avec la valeur par défaut `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Contrôles segmentés

Un contrôle segmenté est organisée permet d’autoriser les utilisateurs à interagir avec un petit nombre d’options. Il est disposé horizontalement et chaque segment fonctionne comme un bouton distinct. Lorsque vous utilisez le concepteur, le contrôle segmenté se trouve sous **boîte à outils > contrôles**et doit ressembler à l’image suivante :

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Contrôle segmenté")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Une fonctionnalité unique du concepteur permet de chaque segment d’être sélectionnés individuellement sur l’aire de conception, comme illustré ci-dessous :

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Contrôle segmenté")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Ainsi, la zone de propriétés à utiliser pour contrôler plus précisément les propriétés de chaque segment. Vous pouvez afficher les propriétés modifiables dans la capture d’écran ci-dessous :

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Contrôle segmenté")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Il convient de noter que le Style du contrôle segmenté a été déconseillé dans iOS7, et par conséquent, réglage des options pour ce dans une application iOS7 aura aucun effet.

## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
- [Contrôleur de l’alerte](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
