---
title: Animations simples
description: "La classe ViewExtensions fournit des méthodes d’extension qui peuvent être utilisés pour construire des animations simples. Cet article décrit la création et l’annulation des animations à l’aide de la classe ViewExtensions."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: fb7ca216978e4c890349a44b07d5a383e9ca2384
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="simple-animations"></a>Animations simples

_La classe ViewExtensions fournit des méthodes d’extension qui peuvent être utilisés pour construire des animations simples. Cet article décrit la création et l’annulation des animations à l’aide de la classe ViewExtensions._


Le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe fournit les méthodes d’extension qui peuvent être utilisés pour créer des animations simples suivantes :

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Anime la [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) et [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) propriétés d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Anime la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) s’applique une augmentation incrémentielle animée ou la diminution de la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Anime la [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) s’applique une augmentation incrémentielle animée ou la diminution de la [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Anime la [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Anime la [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Anime la [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) propriété d’un [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).

Par défaut, chaque animation prendra 250 millisecondes. Toutefois, une durée de chaque animation peut être spécifiée lors de la création de l’animation.

Le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe inclut également un [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) méthode qui peut être utilisé pour annuler toutes les animations.

> [!NOTE]
> Le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe fournit un [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) méthode d’extension. Toutefois, cette méthode est destinée à être utilisée par des dispositions pour animer les transitions entre les États de mise en page qui contiennent la taille et de position des modifications. Par conséquent, il ne doit être utilisé par [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) sous-classes.

Les méthodes d’extension de l’animation dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe sont asynchrones et retour une `Task<bool>` objet. La valeur de retour est `false` si l’animation terminée, et `true` si l’animation est annulée. Par conséquent, les méthodes de l’animation doivent généralement être utilisées avec le `await` opérateur, qui permet de déterminer facilement si une animation est terminée. En outre, il devient possible de créer des animations séquentielles avec des méthodes d’animation suivante l’exécution une fois la méthode précédente est terminée. Pour plus d’informations, consultez [composée des Animations](#compound).

S’il est nécessaire pour permettre une animation terminée en arrière-plan, puis le `await` opérateur peut être omis. Dans ce scénario, les méthodes d’extension animation retournera rapidement après le lancement de l’animation, l’animation qui se produisent en arrière-plan. Cette opération peut être exploitée lors de la création des animations composites. Pour plus d’informations, consultez [Animations Composite](#composite).

Pour plus d’informations sur la `await` (opérateur), consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Animations uniques

Chaque méthode d’extension dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) implémente une opération unique d’animation qui modifie progressivement une propriété d’une valeur à une autre valeur sur une période de temps. Cette section traite de chaque opération de l’animation.

### <a name="rotation"></a>Rotation

L’exemple de code suivant illustre l’utilisation la [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode pour animer la [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Ce code s’anime la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance en appliquant une rotation jusqu'à 360 degrés plus de 2 secondes (2 000 millisecondes). Le [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode obtient actuel [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété valeur pour le début de l’animation, puis fait pivoter de cette valeur pour le premier argument (360). Une fois l’animation est terminé, l’image [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété est réinitialisée à 0. Cela garantit que le `Rotation` propriété ne reste à 360 une fois l’animation s’achève, ce qui empêcherait rotations supplémentaires.

Les captures d’écran suivantes illustrent la rotation en cours d’exécution sur chaque plateforme :

![](simple-images/rotateto.png "Animation de la rotation")

### <a name="relative-rotation"></a>Rotation relative

L’exemple de code suivant montre comment utiliser le [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode pour augmenter ou diminuer de manière incrémentielle le [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

Ce code s’anime la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance en appliquant une rotation de 360 degrés à partir de sa position de départ sur 2 secondes (2 000 millisecondes). Le [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode obtient actuel [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété valeur pour le début de l’animation, puis fait pivoter de cette valeur à la valeur plus son premier argument (360). Cela garantit que chaque animation sera toujours une rotation de 360 degrés à partir de la position de départ. Par conséquent, si une animation est appelée alors qu’une animation est déjà en cours d’exécution, il démarrera à partir de la position actuelle et peut mettre fin à une position qui n’est pas un incrément de 360 degrés.

Les captures d’écran suivantes illustrent la rotation relative en cours d’exécution sur chaque plateforme :

![](simple-images/relrotateto.png "Animation de la Rotation relatif")

### <a name="scaling"></a>Mise à l'échelle

L’exemple de code suivant illustre l’utilisation la [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) méthode pour animer la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

Ce code s’anime la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance en passant à deux fois sa taille supérieure à 2 secondes (2 000 millisecondes). Le [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) méthode obtient actuel [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) la valeur de propriété (valeur par défaut de 1) pour le début de l’animation et les échelles puis à partir de cette valeur pour le premier argument (2). Cela a pour effet de développer la taille de l’image à deux fois sa taille.

Les captures d’écran suivantes illustrent la mise à l’échelle en cours d’exécution sur chaque plateforme :

![](simple-images/scaleto.png "Mise à l’échelle d’Animation")

### <a name="relative-scaling"></a>Rapport de mise à l’échelle

L’exemple de code suivant illustre l’utilisation la [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode pour animer la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

Ce code s’anime la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance en passant à deux fois sa taille supérieure à 2 secondes (2 000 millisecondes). Le [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode obtient actuel [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) valeur pour le début de l’animation et les échelles puis à partir de cette valeur permet son premier argument (2), ainsi que la valeur de la propriété. Cela garantit que chaque animation sera toujours une mise à l’échelle de 2 à partir de la position de départ.

### <a name="scaling-and-rotation-with-anchors"></a>Mise à l’échelle et Rotation avec les points d’ancrage

Le [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) et [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) propriétés définies au centre de mise à l’échelle ou la rotation de la [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) et [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriétés. Par conséquent, leurs valeurs affectent également le [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) et [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) méthodes.

Étant donné un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) qui a été placé dans le centre d’une disposition, l’exemple de code suivant montre comment faire pivoter l’image autour du centre de la mise en page en définissant son [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) propriété :

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Pour faire pivoter le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance autour du centre de la mise en page, le [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) et [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) propriétés doivent être définies sur les valeurs par rapport à la largeur et la hauteur de la `Image`. Dans cet exemple, le centre de la `Image` est défini comme étant au centre de la mise en page et donc la valeur par défaut `AnchorX` la valeur 0,5 n’entraîne pas la modification. Toutefois, le `AnchorY` propriété est redéfinie pour être une valeur à partir du haut de la `Image` au point central de la disposition. Cela garantit que le `Image` effectue une rotation complète de 360 degrés autour du point central de la mise en page, comme indiqué dans les captures d’écran suivants :

![](simple-images/rotate-anchors.png "Animation de rotation avec les points d’ancrage")

### <a name="translation"></a>Traduction

L’exemple de code suivant montre comment utiliser le [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode pour animer la [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) et [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) propriétés d’un [ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Ce code s’anime la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance en traduisant il horizontalement et verticalement sur 1 seconde (1000 millisecondes). Le [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode traduit simultanément l’image 100 pixels à gauche et à 100 pixels vers le haut. Il s’agit, car les premier et le second arguments sont les deux nombres négatifs. En fournissant des nombres positifs traduit l’image vers la droite et vers le bas.

Les captures d’écran suivantes illustrent la traduction en cours d’exécution sur chaque plateforme :

![](simple-images/translateto.png "Translation Animation")

> [!NOTE]
> Si un élément est disposé initialement hors de l’écran et ensuite traduit à l’écran, après conversion disposition d’entrée de l’élément reste hors de l’écran et l’utilisateur ne peut pas interagir avec lui. Par conséquent, il est recommandé qu’une vue doit être disposée à sa dernière position et ensuite tous les traductions effectuées requis.

### <a name="fading"></a>Suppression

L’exemple de code suivant illustre l’utilisation la [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode pour animer la [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Ce code s’anime la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance par elle du fondu dans plus de 4 secondes (4000 millisecondes). Le [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode obtient actuel [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) valeur de propriété pour le début de l’animation et puis fondus dans à partir de cette valeur pour le premier argument (1).

Les captures d’écran suivantes illustrent l’atténuation en cours d’exécution sur chaque plateforme :

![](simple-images/fadeto.png "Animation d’atténuation")

<a name="compound" />

## <a name="compound-animations"></a>Animations composées

Une animation composée est une combinaison séquentielle d’animations et peuvent être créée avec le `await` (opérateur), comme illustré dans l’exemple de code suivant :

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

Dans cet exemple, le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) est traduite plus de 6 secondes (de 6 000 millisecondes). La traduction de la `Image` utilise cinq animations, avec la `await` opérateur indiquant que chaque animation s’exécute de manière séquentielle. Par conséquent, méthodes d’animation suivante exécutent une fois la méthode précédente est terminée.

<a name="composite" />

## <a name="composite-animations"></a>Animations composite

Une animation composite est une combinaison d’animations où deux ou plusieurs des animations de s’exécuter simultanément. Animations composites peuvent être créées par le mélange d’animations attendues et non attendue, comme illustré dans l’exemple de code suivant :

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

Dans cet exemple, le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) est mis à l’échelle et pivoter simultanément de plus de 4 secondes (4000 millisecondes). La mise à l’échelle de le `Image` utilise deux animations séquentielles qui se sont produites en même temps que la rotation. Le [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode s’exécute sans un `await` opérateur et retourne immédiatement, avec la première [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) puis à partir de l’animation. Le `await` opérateur sur le premier `ScaleTo` appel de méthode diffère de la seconde `ScaleTo` jusqu'à ce que le premier appel de la méthode `ScaleTo` appel de méthode est terminée. À ce stade le `RotateTo` animation représente la moitié terminée de manière et le `Image` sera pivotée de 180 degrés. Au cours des 2 dernières secondes (2 000 millisecondes), le deuxième `ScaleTo` animation et `RotateTo` animation les deux se.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Exécution simultanée de plusieurs méthodes asynchrones

Le `static` `Task.WhenAny` et `Task.WhenAll` méthodes sont utilisées pour exécuter plusieurs méthodes asynchrones simultanément et par conséquent peut être utilisés pour créer des animations composites. Ces deux méthodes retournent un `Task` de l’objet et d’accepter une collection de méthodes que chaque retour un `Task` objet. Le `Task.WhenAny` méthode se termine quand n’importe quelle méthode dans sa collection termine l’exécution, comme illustré dans l’exemple de code suivant :

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

Dans cet exemple, le `Task.WhenAny` appel de méthode contient deux tâches. La première tâche fait pivoter l’image de plus de 4 secondes (4000 millisecondes), et la seconde tâche met à l’échelle l’image de plus de 2 secondes (2 000 millisecondes). Lorsque la seconde tâche est terminée, le `Task.WhenAny` appel de méthode. Toutefois, même si le [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) (méthode) est en cours d’exécution, le deuxième [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) méthode peut commencer.

Le `Task.WhenAll` méthode se termine lorsque toutes les méthodes dans sa collection s’est terminé, comme illustré dans l’exemple de code suivant :

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

Dans cet exemple, le `Task.WhenAll` appel de méthode contient trois tâches, chacun d’eux exécute plus de 10 minutes. Chaque `Task` effectue un nombre différent de 360 degrés rotations – 307 rotations pour [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 rotations pour [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)et 199 rotations pour [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). Ces valeurs sont des nombres premiers, par conséquent vous être assuré que les rotations ne sont pas synchronisées et par conséquent, ne provoquera pas répétée.

Les captures d’écran suivantes illustrent les rotations plusieurs en cours d’exécution sur chaque plateforme :

![](simple-images/multiple-rotations.png "Animation composite")

## <a name="canceling-animations"></a>L’annulation d’Animations

Une application peut annuler une ou plusieurs des animations avec un appel à la `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) méthode, comme illustré dans l’exemple de code suivant :

```csharp
ViewExtensions.CancelAnimations (image);
```

Cette opération va annuler immédiatement toutes les animations en cours d’exécution sur le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance.

## <a name="summary"></a>Récapitulatif

Création et l’annulation des animations à l’aide de cet article ne présente le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe. Cette classe fournit des méthodes d’extension qui peuvent être utilisés pour construire des animations simples pour faire pivoter, mettre à l’échelle, traduisent et fondu [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) instances.


## <a name="related-links"></a>Liens associés

- [Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)
- [Animation de base (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
