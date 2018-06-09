---
title: Résumé du chapitre 22. Animation
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé chapitre 22. Animation'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b25fed9a86b82e56cb3b2bf5e3276c8ff63f4e35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241967"
---
# <a name="summary-of-chapter-22-animation"></a>Résumé du chapitre 22. Animation

Vous avez vu que vous pouvez créer vos propres animations à l’aide de la minuterie Xamarin.Forms ou `Task.Delay`, mais il est généralement facile à l’aide des fonctionnalités d’animation fournies par Xamarin.Forms. Trois classes implémentent ces animations :

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/), l’approche de haut niveau
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/), plus souple, mais il est plus difficile
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/), le plus polyvalente plus bas niveau approche

En règle générale, les animations cibler les propriétés qui sont sauvegardées par des propriétés pouvant être liées. Cela n’est pas obligatoire, mais ce sont les seules propriétés réagissent de façon dynamique aux modifications.

Il n’existe aucune interface XAML pour ces animations, mais vous pouvez intégrer des animations dans XAML à l’aide des techniques présentées dans [ **chapitre 23. Les déclencheurs et les comportements**](chapter23.md).

## <a name="exploring-basic-animations"></a>Exploration des animations de base

Les fonctions d’animation de base sont les méthodes d’extension trouvées dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe. Ces méthodes s’appliquent à tout objet qui dérive de `VisualElement`. Les animations de la plus simple ciblent les propriétés décrites dans les transformations [ `Chapter 21. Transforms` ](chapter21.md).

Le [ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) montre comment la `Clicked` Gestionnaire d’événements pour un `Button` peut appeler le [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode d’extension pour lancer le bouton dans un cercle.

Le `RotateTo` de la méthode change la `Rotation` propriété de la `Button` de 0 à 360 au cours de la seconde un quart (par défaut). Si le `Button` drainé, toutefois, elle ne fait rien, car le `Rotation` propriété se trouve déjà 360 degrés.

### <a name="setting-the-animation-duration"></a>Définition de la durée de l’animation

Le deuxième argument de `RotateTo` est la durée en millisecondes. Si défini sur une valeur élevée, en touchant le `Button` pendant une animation démarre une nouvelle possibilité à compter de l’animation à l’angle actuel.

### <a name="relative-animations"></a>Animations relatives

Le `RelRotateTo` méthode effectue une rotation relative en ajoutant une valeur spécifiée à la valeur existante. Cette méthode permet du `Button` à drainer plusieurs fois et chaque heure augmente le `Rotation` propriété à 360 degrés.

### <a name="awaiting-animations"></a>En attente d’animations

Toutes les méthodes d’animation dans `ViewExtensions` retourner `Task<bool>` objets. Cela signifie que vous pouvez définir une série d’animations séquentielles à l’aide de `ContinueWith` ou `await`. Le `bool` fin valeur de retour est `false` si l’animation s’est terminée sans interruption ou `true` si elle a été annulée par le [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) (méthode), ce qui annule toutes les animations initiées par le autre méthode dans `ViewExtensions` qui sont définies sur le même élément.

### <a name="composite-animations"></a>Animations composite

Vous pouvez combiner des animations attendues et non attendue pour créer des animations composites. Il s’agit des animations dans `ViewExtensions` qui ciblent le `TranslatonX`, `TranslationY`, et `Scale` propriétés de la transformation :

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

Notez que `TranslateTo` affecte potentiellement tous deux le `TranslationX` et `TranslationY` propriétés.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll et Task.WhenAny

Il est également possible de gérer des animations simultanées à l’aide de [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), qui signale le moment où plusieurs tâches de tous les tests terminés, et [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), ce qui indique quand la première de plusieurs tâches a terminé.

### <a name="rotation-and-anchors"></a>Rotation et points d’ancrage

Lors de l’appel du `ScaleTo`, `RelScaleTo`, `RotateTo`, et `RelRotateTo` méthodes, vous pouvez définir le [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) et [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) propriétés pour indiquer le Centre de rotation et de mise à l’échelle.

Le [ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) illustre cette technique par révolution un `Button` autour du centre de la page.

### <a name="easing-functions"></a>Fonctions d’accélération

Animations sont généralement linéaires à partir d’une valeur de début pour une valeur de fin. Fonctions d’accélération peuvent entraîner des animations accélérer ou ralentir leur cours. Le dernier argument facultatif pour les méthodes de l’animation est de type [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), une classe qui définit le 11 champs en lecture seule statiques de type `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/), la valeur par défaut
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/), [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), et [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/), [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), et [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) et [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) et [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

Le `In` suffixe indique que l’effet est au début de l’animation, `Out` signifie à la fin, et `InOut` signifie qu’il est au début et à la fin de l’animation.

Le [ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) exemple illustre l’utilisation de fonctions d’accélération.

### <a name="your-own-easing-functions"></a>Vos propres fonctions d’accélération

Vous pouvez également définir vous propres fonctions d’accélération en passant un [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) à la [ `Easing` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/). `Easing` définit également une conversion implicite de `Func<double, double>` à `Easing`. L’argument de la fonction d’accélération est toujours dans la plage de 0 à 1, l’animation au fil de façon linéaire à partir du début à la fin. La fonction *généralement* retourne une valeur dans la plage de 0 à 1, mais peut être brièvement négatif ou supérieur à 1 (comme c’est le cas avec les `SpringIn` et `SpringOut` fonctions) ou peut altérer les règles si vous savez ce que vous faites.

Le [ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) exemple illustre une fonction d’accélération personnalisée, et [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) montre un autre.

Le [ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) exemple illustre également une fonction d’accélération personnalisée et également une technique de la modification de la `AnchorX` et `AnchorY` propriétés dans une séquence d’animations de rotation.

Le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque a un [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) la fonction de classe qui utilise une accélération personnalisée à jiggle un bouton lorsqu’il est activé. Le [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) exemple montre comment il.

### <a name="entrance-animations"></a>Animations d’entrée

Un seul type populaires d’animation se produit lorsqu’une page s’affiche pour la première fois. Une animation de ce type peut être démarrée le [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) la substitution de la page. Pour ces animations, il est préférable de configurer le code XAML pour la façon dont vous souhaitez que la page apparaisse *après* l’animation, initialiser et animer la disposition à partir du code.

Le [ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) exemple utilise le [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode d’extension pour augmenter progressivement le contenu de la page.

Le [ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) exemple utilise le [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode d’extension à la diapositive dans le contenu de la page sur les côtés.

Le [ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) exemple utilise le [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode d’extension pour animer la `RotationY` propriété. A [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode est également disponible.

### <a name="forever-animations"></a>Animations indéfiniment

À l’inverse, « forever » les animations s’exécutent jusqu'à ce que le programme est arrêté. Ils sont généralement destinés à des fins de démonstration.

Le [ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) exemple utilise [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) animation à deux parties de texte en fondu.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) affiche un palindrome et puis séquentiellement fait pivoter les lettres individuelles de 180 degrés afin qu’elles soient toutes envers. Ensuite, la chaîne entière est retournée à 180 degrés pour lire le même que la chaîne d’origine.

Le [ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) exemple fait pivoter un simple `BoxView` hélicoptère lors de la révolution il autour du centre de l’écran.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) tourne `BoxView` rayons autour du centre de l’écran, puis fait pivoter chaque membre spoke lui-même pour créer des modèles intéressants :

[![Capture d’écran de triple de rotation rayons](images/ch22fg21-small.png "rotation rayons")](images/ch22fg21-large.png#lightbox "rotation rayons")

Toutefois, augmentant progressivement le `Rotation` propriété d’un élément peut ne pas fonctionne à long terme, comme le [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) illustre.

Le [ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) exemple utilise [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), et [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) pour donner l’impression que si une image bitmap est rotation dans un espace 3D.

### <a name="animating-the-bounds-property"></a>Animation de la propriété de limites

La seule méthode d’extension dans `ViewExtensions` pas encore illustré est [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), qui anime efficacement en lecture seule [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) propriété en appelant le [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) (méthode). Cette méthode est généralement appelée par `Layout` dérivés comme sera traitée dans [ **chapitre 26. CustomLayouts**](chapter26.md).

Le `LayoutTo` méthode doit être limitée à des usages spéciaux. Le [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) programme l’utilise pour compresser et décompresser un `BoxView` comme il rebondit sur les côtés d’une page.

Le [ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) exemple utilise `LayoutTo` pour déplacer les vignettes dans une implémentation de la classique puzzle 15-16 qui affiche une image brouillée plutôt que les vignettes numérotées :

[![Capture d’écran de triple de Xamarin Xuzzle](images/ch22fg26-small.png "jeu de Puzzle Xuzzle")](images/ch22fg26-large.png#lightbox "jeu de Puzzle Xuzzle")

### <a name="your-own-awaitable-animations"></a>Vos propres animations pouvant être attendues

Le [ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) exemple crée une animation pouvant être attendue. La classe essentielle qui peut retourner un `Task` objet à partir de la méthode et le signal de fin de l’animation est [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/).

## <a name="deeper-into-animations"></a>Profond dans des animations

Le système d’animation Xamarin.Forms peut être déroutant. Outre la `Easing` catégorie, le système d’animation comporte la `ViewExtensions`, `Animation`, et `AnimationExtension` classification des caractéristiques.

### <a name="viewextensions-class"></a>Classe de ViewExtensions

Vous avez déjà vu [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/). Il définit neuf méthodes qui retournent `Task<bool>` et [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/). Sept de la cible de neuf méthodes propriétés de la transformation. Les deux autres sont [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), les destinations de la [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) propriété, et [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), qui appelle la [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) (méthode).

### <a name="animation-class"></a>Classe d’animation

Le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) classe a un [constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) avec cinq arguments pour définir le rappel et les méthodes terminés et les paramètres de l’animation.

Animations enfants peuvent être ajoutées avec [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), et et surcharge de [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

L’objet d’animation est puis démarré avec un appel à la [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) (méthode).

### <a name="animationextensions-class"></a>Classe de AnimationExtensions

Le [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) classe contient essentiellement des méthodes d’extension. Il existe plusieurs versions d’un `Animate` (méthode) et générique [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) (méthode) est si flexible qu’il s’agit bien de la fonction d’animation uniquement que vous avez besoin.

## <a name="working-with-the-animation-class"></a>Utilisation de la classe d’Animation

Le [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) illustre les [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe avec plusieurs animations différents.

### <a name="child-animations"></a>Animations enfants

Le [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) exemple illustre également l’enfant d’animations qui utilisent (très similaire) [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) et [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) méthodes.

### <a name="beyond-the-high-level-animation-methods"></a>Au-delà des méthodes de haut niveau d’animation

Le [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) exemple montre également comment effectuer les animations qui vont au-delà de la propriétés ciblés par la `ViewExtensions` méthodes. Dans l’exemple, une série de périodes d’obtenir plus de temps ; dans un autre exemple, un `BackgroundColor` propriété est animée.

### <a name="more-of-your-own-awaitable-methods"></a>Plusieurs de vos propres méthodes pouvant être attendus

Le [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) méthode `ViewExtensions` ne fonctionne pas avec le [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) (fonction). Il s’arrête lorsque la sortie accélération obtient supérieure à 1.

Le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque contient un [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) classe avec [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) et [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) les méthodes d’extension qui n’ont pas ce problème, ainsi que [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) et [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) méthodes pour annuler celles animations.

Le [ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) illustre les `TranslateXTo` (méthode).

Le `MoreExtensions` classe contient également un [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) méthode d’extension qui combine la translation X et Y, et un [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) (méthode).

### <a name="implementing-a-bezier-animation"></a>Implémentation d’une animation de Bézier

Il est également possible de développer une animation qui déplace un élément le long du chemin d’accès d’une spline de Bézier. Le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque contient un [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) structure qui encapsule une spline de Bézier et un [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) énumération pour contrôler l’orientation.

Le [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) classe contient un [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) méthode d’extension et un [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) (méthode).

Le [ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) exemple montre une animation d’un élément le long d’un chemin d’accès Beizer.

## <a name="working-with-animationextensions"></a>Utilisation de AnimationExtensions

Un seul type d’animation manquant à partir de la collection standard est une animation en couleur. Le problème est qu’il n’existe aucun moyen de droite pour l’interpolation entre deux `Color` valeurs. Il est possible d’interpolation des valeurs RVB, mais uniquement comme étant valides interpolation les valeurs TSL.

Pour cette raison, le [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) classe dans le [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) bibliothèque contient deux `Color` méthodes d’animation : [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)et [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Il existe également deux méthodes d’annulation : [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) et [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Les deux méthodes utilisent [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), qui effectue l’animation en appelant le générique étendu [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) méthode dans [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/).

Le [ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) exemple illustre l’utilisation de ces deux types d’animations couleur.

## <a name="structuring-your-animations"></a>Structuration de vos animations

Il est parfois utile exprimer des animations en XAML et de les utiliser conjointement avec MVVM. Ce sujet est abordé dans le chapitre suivant, [ **chapitre 23. Les déclencheurs et les comportements**](chapter23.md).



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 22 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Exemples de chapitre 22](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animation](~/xamarin-forms/user-interface/animation/index.md)
