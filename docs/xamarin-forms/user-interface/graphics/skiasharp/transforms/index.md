---
title: Transformations SkiaSharp
description: En savoir plus sur les transformations pour afficher les graphiques de SkiaSharp
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 668488ab7efae66f1777e9ae6ded1f725833fe16
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="skiasharp-transforms"></a>Transformations SkiaSharp

_En savoir plus sur les transformations pour afficher les graphiques de SkiaSharp_

SkiaSharp prend en charge les transformations de graphique traditionnelle qui sont implémentées en tant que méthodes de le [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) objet. Mathématiquement, les transformations modifier les coordonnées et tailles que vous spécifiez dans `SKCanvas` fonctions de dessin, comme les objets graphiques sont rendus. Les transformations sont souvent pratiques pour dessiner des graphiques répétitives ou pour l’animation. Certaines techniques &mdash; telles que la rotation des fichiers bitmap ou texte &mdash; ne sont pas possibles sans l’utilisation de transformations.

Les transformations SkiaSharp prennent en charge les opérations suivantes :

- *Traduire* MAJ les coordonnées d’un emplacement à un autre
- *Mise à l’échelle* pour augmenter ou diminuer les coordonnées et tailles
- *Faire pivoter* pour faire pivoter les coordonnées autour d’un point
- *Incliner* décaler coordonne horizontalement ou verticalement afin que d’un rectangle devient un parallélogramme

Ils sont appelés *affine* transforme. Transformations affines conservent toujours des lignes parallèles et ne provoquent jamais une coordonnée ou une taille de devenir infinie. Un carré n’est jamais transformé en autre chose qu’un parallélogramme et un cercle n’est jamais transformé en autre chose qu’une ellipse.

SkiaSharp prend également en charge les transformations non affines (également appelé *projectives* ou *perspective* transforme) basé sur une matrice de transformation de 3-par-3 standard. Une transformation non affines permet un carré être transformés en un quadrilatère convexe (quatre côtés figure avec tous les angles intérieurs inférieur à 180 degrés). Transformations affines non peuvent entraîner des coordonnées ou les tailles de devenir infinie, mais ils sont essentielles pour les effets 3D.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Différences entre SkiaSharp et les transformations de Xamarin.Forms

Xamarin.Forms prend également en charge les transformations qui sont semblables à celles des SkiaSharp. Le Xamarin.Forms [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) classe définit les propriétés de transformation suivantes :

- `TranslationX` et `TranslationY`
- `Scale`
- `Rotation`, `RotationX` et `RotationY`

Le `RotationX` et `RotationY` propriétés sont des transformations de perspective qui créent des effets quasi-3D.

Il existe plusieurs différences importantes entre SkiaSharp transformations et les transformations de Xamarin.Forms :

La première différence est que SkiaSharp sont appliquées à l’intégralité de `SKCanvas` tandis que les transformations de Xamarin.Forms sont appliquées à une personne de l’objet `VisualElement` dérivés. (Vous pouvez appliquer les transformations de Xamarin.Forms pour la `SKCanvasView` de l’objet lui-même, car `SKCanvasView` dérive `VisualElement`, mais dans ce `SKCanvasView`, appliquent des transformations SkiaSkarp.)

Les transformations SkiaSharp sont par rapport au coin supérieur gauche de la `SKCanvas` tandis que les transformations de Xamarin.Forms sont par rapport au coin supérieur gauche de la `VisualElement` auquel ils sont appliqués. Cette différence est importante lorsque vous appliquez la mise à l’échelle et rotation transforme car ces transformations sont toujours par rapport à un moment donné.

La très grande différence est que les transformations de SKiaSharp sont *méthodes* tandis que les transformations de Xamarin.Forms sont *propriétés*. Il s’agit d’une différence de sémantique au-delà de la différence de syntaxe : SkiaSharp transformations effectuent une opération alors que les transformations Xamarin.Forms de définir un état. SkiaSharp les transformations s’appliquent aux objets de graphiques dessinés par la suite, mais pas vers les objets graphiques dessinés avant la transformation est appliquée. En revanche, une transformation Xamarin.Forms s’applique à un élément précédemment rendu dès que la propriété est définie. Les transformations SkiaSharp sont cumulatifs que les méthodes sont appelées ; Transformations de Xamarin.Forms sont remplacées lorsque la propriété est définie avec une autre valeur.

Tous les exemples de programmes dans cette section apparaissent sous l’en-tête **transforme** dans la page d’accueil de la [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programme, puis, dans le [ **Transforme** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms) dossier de la solution.

## <a name="the-translate-transformtranslatemd"></a>[La transformation de traduction](translate.md)

Découvrez comment utiliser la transformation de traduction décaler SkiaSharp graphics.

## <a name="the-scale-transformscalemd"></a>[La transformation d’échelle](scale.md)

Découvrir la transformation d’échelle SkiaSharp mise à l’échelle des objets de différentes tailles.

## <a name="the-rotate-transformrotatemd"></a>[La transformation de rotation](rotate.md)

Explorer les effets et les animations possibles avec la transformation de rotation SkiaSharp.

## <a name="the-skew-transformskewmd"></a>[La transformation d’inclinaison](skew.md)

Découvrez comment la transformation d’inclinaison peut créer des objets graphiques inclinées dans SkiaSharp.

## <a name="matrix-transformsmatrixmd"></a>[Transformations de matrice](matrix.md)

Explorer plus en détail les transformations SkiaSharp avec la matrice de transformation polyvalente.

## <a name="touch-manipulationstouchmd"></a>[Manipulations tactiles](touch.md)

Matrice d’utilisation se transforme pour implémenter des manipulations tactile pour faire glisser, à l’échelle et la rotation.

## <a name="non-affine-transformsnon-affinemd"></a>[Transformations non affines](non-affine.md)

Aller au-delà de l’oridinary avec des effets de transformation non affines.

## <a name="3d-rotation3d-rotationmd"></a>[Rotation 3D](3d-rotation.md)

Transformations non affines permet de faire pivoter des objets 2D dans un espace 3D.


## <a name="related-links"></a>Liens associés

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
