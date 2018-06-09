---
title: Résumé du chapitre 21. Transformations
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 21. Transformations'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b8f24a500c0d1a8359641c62e88779b795b0577a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240555"
---
# <a name="summary-of-chapter-21-transforms"></a>Résumé du chapitre 21. Transformations

Xamarin.Forms s’affiche sur l’écran dans un emplacement et une taille déterminée par son parent, qui est généralement un `Layout` ou `Layout<View>` dérivé. Le *transformer* est une fonctionnalité de Xamarin.Forms que vous pouvez modifier cet emplacement, la taille ou orientation même.

Xamarin.Forms prend en charge trois types de transformations :

- *Traduction* &mdash; déplacer un élément horizontalement ou verticalement
- *Mise à l’échelle* &mdash; modifier la taille d’un élément
- *Rotation* &mdash; activer un élément autour d’un point ou un axe

Dans Xamarin.Forms, la mise à l’échelle est Isotrope ; elle affecte la largeur et la hauteur uniformément. Rotation est pris en charge à la fois sur la surface à deux dimensions de l’écran et dans l’espace 3D. Il y a aucune transformation d’inclinaison (ou amène) et aucune transformation de matrice généralisé.

Les transformations sont pris en charge avec huit propriétés de type `double` définie par le `VisualElement` classe :

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

Toutes ces propriétés sont soutenues par les propriétés pouvant être liées. Ils peuvent être la cible de liaison de données et un style. [**Chapitre 22. Animation** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) montre comment ces propriétés peuvent être animées, mais certains exemples dans ce chapitre montrent comment vous pouvez animer à l’aide de la Xamarin.Forms [timer](~/xamarin-forms/platform/device.md#Device_StartTimer).

Transformer les propriétés affectent uniquement la manière dont l’élément est rendu et faire *pas* affectent la façon dont l’élément est perçu dans la disposition.

## <a name="the-translation-transform"></a>La transformation de traduction

Des valeurs différentes de zéro de la [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) et [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) propriétés déplacer un élément horizontalement ou verticalement.

Le [ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) programme vous permet de faire des essais avec ces propriétés avec deux `Slider` éléments qui contrôlent la `TranslationX` et `TranslationY` propriétés d’un `Frame`. La transformation affecte également tous les enfants de ce `Frame`.

### <a name="text-effects"></a>Effets de texte

Une utilisation courante des propriétés de la traduction est pour décaler légèrement le rendu de texte. Cela est illustré dans le [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) exemple :

[![Capture d’écran de triple des Offsets de texte](images/ch21fg03-small.png "texte Offsets")](images/ch21fg03-large.png#lightbox "Offsets du texte")

Un autre effet consiste à rendre plusieurs copies d’un `Label` ressemble à un bloc 3D, comme illustré dans le [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) exemple.

### <a name="jumps-and-animations"></a>Liens et les animations

Le [ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) exemple utilise la traduction pour déplacer un `Button` chaque fois qu’il est activé par un clic, mais l’objectif principal est de démontrer que la `Button` reçoit l’entrée d’utilisateur à l’emplacement où le bouton est restitué.

Le [ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) exemple est semblable, mais utilise une horloge pour animer la `Button` d’un point à un autre.

## <a name="the-scale-transform"></a>La transformation d’échelle

Le [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) transformation peut augmenter ou diminuer la taille de rendu de l’élément. La valeur par défaut est 1. La valeur 0 entraîne l’élément invisible. Les valeurs négatives entraînent l’élément semblent être pivotée de 180 degrés. Le `Scale` propriété n’affecte pas la `Width` ou `Height` propriétés de l’élément. Ces valeurs restent les mêmes.

Vous pouvez faire des essais avec le `Scale` à l’aide de la propriété du [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) exemple.

Le [ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) illustre la différence entre l’animation le `Scale` propriété d’un `Button` et l’animation le `FontSize` propriété. Le `FontSize` propriété affecte la `Button` est perçu dans disposition ; le `Scale` propriété n’a pas.

Le [ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) exemple calcule un `Scale` propriété qui est appliquée à un `Label` élément afin de faciliter autant que possible tout en adaptant toujours dans la page.

### <a name="anchoring-the-scale"></a>Ancrage de l’échelle

Les éléments mis à l’échelle dans les trois exemples précédents ont tous les augmentation ou de diminution de la taille par rapport au centre de l’élément. En d’autres termes, l’élément augmente ou diminue la taille identique dans toutes les directions. Seul le point au centre de l’élément reste dans le même emplacement lors de la mise à l’échelle.

Vous pouvez modifier le centre de la mise à l’échelle en définissant le [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) et [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) propriétés. Ces propriétés sont par rapport à l’élément lui-même. Pour `AnchorX`, la valeur 0 fait référence au côté gauche de l’élément et 1 fait référence au côté droit. De même pour `AnchorY`, 0 est en haut et 1 est bas. Les deux propriétés ont des valeurs par défaut de 0.5, qui est le centre.

Le [ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) exemple vous permet de faire des essais avec le `AnchorX` et `AnchorY` propriétés, ainsi que les `Scale` propriété.

Sur iOS, à l’aide de valeurs non définies par défaut de `AnchorX` et `AnchorY` propriétés est généralement pas compatible avec les modifications de l’orientation de téléphone.

## <a name="the-rotation-transform"></a>La transformation de rotation

Le [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété est spécifiée en degrés et indique une rotation autour d’un point de l’élément défini par `AnchorX` et `AnchorY`. Le [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) vous permet de faire des essais avec ces trois propriétés.

### <a name="rotated-text-effects"></a>Effets de texte pivoté

Le [ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) exemple illustre les calculs mathématiques nécessaires pour dessiner un cercle à l’aide de 64 minuscule paysage `BoxView` éléments.

Le [ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) exemple affiche plusieurs `Label` éléments avec la même chaîne de texte pivoté apparaît comme spokes.

Le [ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) exemple affiche une chaîne de texte qui apparaît à la ligne dans un cercle.

### <a name="an-analog-clock"></a>Une horloge analogique

Le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque contient un [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) classe qui calcule les angles de la configuration automatique d’une horloge. Pour éviter les dépendances de plateforme dans ViewModel, la classe utilise `Task.Delay` au lieu d’un minuteur pour la recherche d’un nouveau `DateTime` valeur.

Également inclus dans **Xamarin.FormsBook.Toolkit** est un [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) classe qui implémente `IValueConverter` et sert à arrondir un angle deuxième à la seconde près.

Le [ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) utilise rotation trois `BoxView` éléments pour dessiner une horloge analogique.

Le [ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) utilise `BoxView` pour les graphiques plus étendues, y compris les graduations marque autour de l’image de l’horloge et remet que faire pivoter une peu la distance à partir de leurs extrémités :

[![Capture d’écran de triple d’horloge de BoxView](images/ch21fg17-small.png "cadran analogique")](images/ch21fg17-large.png#lightbox "cadran analogique")

En outre un [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) classe dans **Xamarin.FormsBook.Toolkit** provoque l’aiguille d’extraire un peu avant de passer en avance, puis replacez dans sa position correcte.

### <a name="vertical-sliders"></a>Curseurs verticales ?

Le [ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) exemple montre que `Slider` éléments peut pivoter de 90 degrés et fonctionnent toujours. Toutefois, il est difficile de placer ces paysage `Slider` éléments, car dans la disposition, ils toujours semblent horizontal.

## <a name="3d-ish-rotations"></a>Rotations de problèmes de 3D

Le [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) propriété s’affiche pour faire pivoter un élément autour d’un axe des abscisses 3D afin que le haut et le bas de l’élément semblent déplacer vers ou hors de la visionneuse. De même, la [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) semble faire pivoter un élément autour de l’axe des ordonnées pour rendre les côtés gauche et droit de l’élément semble déplacer vers ou hors de la visionneuse.

Le `AnchorX` propriété affecte `RotationY` mais pas `RotationX`. Le `AnchorY` propriété affecte `RotationX` mais pas `RotationY`. Vous pouvez faire des essais avec le [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) exemple pour Explorer les interactions de ces propriétés.

Le système de coordonnées 3D impliqué par Xamarin.Forms est gauche. Si vous pointez l’index de la main gauche dans le sens d’augmentation de X coordonnées coordonnées (à droite) et votre doigt intermédiaire dans le sens d’augmentation Y (points vers le bas), puis votre curseur dans le sens d’augmentation des coordonnées Z (hors de l’écran).

En outre, pour tous les trois axes, si vous pointez votre curseur gauche dans le sens d’augmentation des valeurs, puis la courbe de vos doigts indique la direction de rotation pour faire pivoter les valeurs positives.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 21 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Exemples de chapitre 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
