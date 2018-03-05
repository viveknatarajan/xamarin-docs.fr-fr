---
title: "Animations personnalisées"
description: "La classe de l’Animation est le bloc de construction de toutes les animations de Xamarin.Forms, avec les méthodes d’extension dans la classe ViewExtensions création d’un ou plusieurs objets d’Animation. Cet article montre comment utiliser la classe de l’Animation pour créer et d’annuler des animations, de synchroniser plusieurs animations et créer des animations personnalisées animer des propriétés qui ne sont pas animées par les méthodes d’animation existante."
ms.topic: article
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 42ef3e6c82763831b5114f3de7603bba8f59eac6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="custom-animations"></a>Animations personnalisées

_La classe de l’Animation est le bloc de construction de toutes les animations de Xamarin.Forms, avec les méthodes d’extension dans la classe ViewExtensions création d’un ou plusieurs objets d’Animation. Cet article montre comment utiliser la classe de l’Animation pour créer et d’annuler des animations, de synchroniser plusieurs animations et créer des animations personnalisées animer des propriétés qui ne sont pas animées par les méthodes d’animation existante._


Un nombre de paramètres doit être spécifié lorsque vous créez un `Animation` objet, y compris les valeurs de début et de fin de la propriété animée et un rappel qui modifie la valeur de la propriété. Un `Animation` objet peut également conserver une collection d’animations enfants qui peuvent être exécutés et synchronisé. Pour plus d’informations, consultez [Animations enfants](#child).

Exécution d’une animation créée avec le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) (classe), ce qui peut ou ne peut pas inclure les animations enfants, s’effectue en appelant le [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) (méthode). Cette méthode spécifie la durée de l’animation, entre autres choses, un rappel qui détermine s’il faut répéter l’animation.

## <a name="creating-an-animation"></a>Création d’une Animation

Lorsque vous créez un [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) de l’objet, en règle générale, un minimum de trois paramètres sont requis, comme illustré dans l’exemple de code suivant :

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Ce code définit une animation de la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance à partir d’une valeur de 1 à la valeur 2. La valeur animée, qui est dérivée en Xamarin.Forms, qui est passée au rappel spécifié comme premier argument, où il est utilisé pour modifier la valeur de la `Scale` propriété.

L’animation est démarrée avec un appel à la [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) méthode, comme illustré dans l’exemple de code suivant :

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Notez que la [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) méthode ne retourne pas une `Task` objet. Au lieu de cela, les notifications sont fournies par le biais des méthodes de rappel.

Les arguments suivants sont spécifiés dans le `Commit` méthode :

- Le premier argument (*propriétaire*) identifie le propriétaire de l’animation. Cela peut être l’élément visuel à laquelle l’animation est appliquée, ou un autre élément visuel, telles que la page.
- Le deuxième argument (*nom*) identifie l’animation avec un nom. Le nom est combiné avec le propriétaire pour identifier de façon unique l’animation. Cette identification unique peut ensuite être utilisée pour déterminer si l’animation est en cours d’exécution ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), ou pour l’annuler ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- Le troisième argument (*taux*) indique le nombre de millisecondes entre chaque appel à la méthode de rappel définie dans le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) constructeur
- Le quatrième argument (*longueur*) indique la durée de l’animation, en millisecondes.
- Le cinquième argument (*accélération*) définit la fonction d’accélération à utiliser dans l’animation. Vous pouvez également la fonction d’accélération peut être spécifiée en tant qu’argument à la [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) constructeur. Pour plus d’informations sur les fonctions d’accélération, consultez [fonctions d’accélération](~/xamarin-forms/user-interface/animation/easing.md).
- Sixième argument (*terminé*) est un rappel qui sera exécuté une fois l’animation terminée. Ce rappel accepte deux arguments, avec le premier argument indiquant une valeur finale et le second argument étant un `bool` qui a la valeur `true` si l’animation a été annulée. Vous pouvez également le *terminé* rappel peut être spécifié en tant qu’argument à la [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) constructeur. Toutefois, avec une seule animation si *terminé* rappels sont spécifiés dans les deux le `Animation` constructeur et la `Commit` (méthode), seulement le rappel spécifié dans le `Commit` méthode sera exécutée.
- Septième argument (*répétez*) est un rappel qui permet l’animation doit être répétée. Elle est appelée à la fin de l’animation et en retournant `true` indique que l’animation doit être répétée.

L’effet global consiste à créer une animation qui augmente la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété d’un [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) comprise entre 1 et 2, plus 2 secondes (2 000 millisecondes), à l’aide de la [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) fonction d’accélération. Chaque fois que l’animation terminée, son `Scale` propriété est réinitialisée à 1 et de l’animation se répète.

> [!NOTE]
> **Remarque**: animations simultanées, qui s’exécutent indépendamment les uns des autres peuvent être construites en créant un `Animation` de l’objet de chaque animation, puis en appelant le `Commit` méthode sur chaque animation.

<a name="child" />

### <a name="child-animations"></a>Animations enfants

Le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe prend également en charge les animations enfants, ce qui implique la création d’un `Animation` objet pour les autres `Animation` objets sont ajoutés. Cela permet une série d’animations doit exécuter et synchronisé. L’exemple de code suivant illustre la création et exécution des animations enfants :

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

L’exemple de code peut également être écrites plus concise, comme cela est indiqué dans l’exemple de code suivant :

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Dans les deux exemples de code, un parent [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) objet est créé, auquel supplémentaires `Animation` objets sont ensuite ajoutés. Les deux premiers arguments à la [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) méthode spécifier la date de début et de fin de l’animation enfant. Les valeurs d’argument doivent être comprise entre 0 et 1 et représentent la période relative au sein de l’animation parent que l’animation enfant spécifié sera active. Par conséquent, dans cet exemple la `scaleUpAnimation` seront actives pour la première moitié de l’animation, la `scaleDownAnimation` seront actives pour la seconde moitié de l’animation et le `rotateAnimation` sera active pendant toute la durée.

L’effet global est que l’animation se produit plus de 4 secondes (4000 millisecondes). Le `scaleUpAnimation` anime la [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété à partir de 1 à 2, plus de 2 secondes. Le `scaleDownAnimation` puis anime le `Scale` propriété à partir de 2 à 1, plus de 2 secondes. Alors que les deux animations à l’échelle sont produisent, le `rotateAnimation` anime la [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) propriété comprise entre 0 et 360, plus de 4 secondes. Notez que les animations de mise à l’échelle utilisent également des fonctions d’accélération. Le [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) provoque la fonction d’accélération le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) initialement réduction avant d’obtenir un plus grand et le [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) fonction d’accélération provoque la `Image` devienne plus petit que sa taille réelle vers la fin de l’animation terminée.

Il existe plusieurs différences entre un [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) objet qui utilise des animations enfants et l’autre ne :

- Lorsque vous utilisez des animations enfants, le *terminé* rappel sur une animation enfant indique quand l’enfant a terminé et le *terminé* rappel passé à la `Commit` méthode indique quand le toute l’animation terminée.
- Lorsque vous utilisez des animations enfants, retour `true` à partir de la *répétez* rappel sur le `Commit` méthode n’entraîne pas l’animation, mais continue à s’exécuter sans les nouvelles valeurs de l’animation.
- Lorsque vous incluez une fonction d’accélération dans le `Commit` (méthode) et la fonction d’accélération retourne une valeur supérieure à 1, l’animation va être interrompue. Si la fonction d’accélération retourne une valeur inférieure à 0, la valeur est ancrée à 0. Pour utiliser une fonction d’accélération qui retourne une valeur inférieure à 0 ou supérieur à 1, il doit spécifié dans un des animations enfants, plutôt que dans le `Commit` (méthode).

Le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe inclut également [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) méthodes qui peuvent être utilisés pour ajouter des animations enfants à un parent `Animation` objet. Toutefois, leur *commencer* et *Terminer* valeurs des arguments ne sont pas limitées à 0 et 1, mais seule la partie de l’animation enfant qui correspond à une plage de 0 à 1 sera active. Par exemple, si un `WithConcurrent` appel de méthode définit une animation enfant qui cible un [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) propriété à partir de 1 à 6, mais avec *commencer* et *Terminer* valeurs de -2 et 3, le *commencer* valeur-2 correspond à un `Scale` la valeur 1 et le *Terminer* la valeur 3 correspond à un `Scale` valeur 6. Étant donné que les valeurs en dehors de la plage de 0 à 1 ne jouent aucune partie d’une animation, la `Scale` propriété sera uniquement animée de 3 à 6.

## <a name="canceling-an-animation"></a>L’annulation d’une Animation

Une application peut annuler une animation avec un appel à la [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) méthode d’extension, comme illustré dans l’exemple de code suivant :

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Notez que les animations sont identifiées de manière unique par une combinaison de propriétaire de l’animation et le nom de l’animation. Par conséquent, le propriétaire et le nom spécifié lorsque l’animation en cours d’exécution doit être spécifié pour annuler l’animation. Par conséquent, l’exemple de code va annuler immédiatement l’animation nommée `SimpleAnimation` qui appartient à la page.

## <a name="creating-a-custom-animation"></a>Création d’une Animation personnalisée

Les exemples présentés ici jusqu'à présent ont montré que les animations qui peuvent également être obtenues avec les méthodes dans les [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe. Toutefois, l’avantage de la [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe est qu’il a accès à la méthode de rappel, qui est exécutée lorsque la valeur animée change. Ainsi, le rappel d’implémenter toute animation souhaitée. Par exemple, l’exemple de code suivant réalise une animation le [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) propriété d’une page en lui affectant [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) valeurs créées par le [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)(méthode), avec des valeurs de teinte comprises entre 0 et 1 :

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

L’animation fournit l’apparence d’avancer l’arrière-plan de la page via les couleurs de l’arc-en-ciel.

Pour plus d’exemples pour créer des animations complexes, y compris une animation de courbe Bézier, consultez [chapitre 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) de [créer des applications mobiles avec Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Création d’une méthode d’Extension de l’Animation personnalisée

Les méthodes d’extension dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe animer une propriété à partir de sa valeur actuelle à une valeur spécifiée. Cela rend difficile à créer, par exemple, un `ColorTo` méthode animation qui peut être utilisé pour animer une couleur à partir d’une valeur à une autre, car :

- La seule [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) propriété définie par le [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) classe est [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), qui n’est pas toujours souhaité `Color` propriété à animer.
- Fréquence à laquelle la valeur actuelle d’un [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) propriété est [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), qui n’est pas une couleur réelle, et qui ne peut pas être utilisé dans les calculs d’interpolation.

La solution à ce problème est de ne pas avoir le `ColorTo` méthode cibler un particulier [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) propriété. Au lieu de cela, il peut être écrit avec une méthode de rappel qui transmet l’interpolée `Color` valeur à l’appelant. En outre, la méthode prend start et end `Color` arguments.

Le `ColorTo` méthode peut être implémentée comme une méthode d’extension qui utilise le [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) méthode dans le [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) classe afin de fournir ses fonctionnalités. C’est parce que le `Animate` méthode peut être utilisée pour les propriétés cibles qui ne sont pas de type `double`, comme illustré dans l’exemple de code suivant :

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

Le [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) méthode requiert un *transformer* argument, qui est une méthode de rappel. L’entrée de ce rappel est toujours un `double` comprises entre 0 et 1. Par conséquent, le `ColorTo` méthode définit sa propre transformation `Func` qui accepte un `double` comprises entre 0 et 1 et qui retourne un [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) valeur correspondant à cette valeur. Le `Color` la valeur est calculée par l’interpolation le [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), et [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) les valeurs des deux fournis `Color` arguments. Le `Color` valeur est ensuite passée à la méthode de rappel pour l’application à une propriété particulière.

Cette approche permet la `ColorTo` méthode pour animer les [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) propriété, comme illustré dans l’exemple de code suivant :

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

Dans cet exemple de code, la `ColorTo` réalise une animation de la méthode la [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) et [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) propriétés d’un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), le `BackgroundColor`propriété d’une page et le [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) propriété d’un [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/).

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe pour créer et annuler des animations, synchroniser plusieurs animations et créer des animations personnalisées animer des propriétés qui ne sont pas animées par l’animation existante méthodes. La `Animation` classe est le bloc de construction de toutes les animations de Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [Animations personnalisées (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animation](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
