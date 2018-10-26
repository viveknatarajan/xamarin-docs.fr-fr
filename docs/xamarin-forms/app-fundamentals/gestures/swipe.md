---
title: Ajout d’un module de reconnaissance de mouvement de balayage
description: Cet article explique comment reconnaître un mouvement de balayage qui se produisent sur une vue.
ms.prod: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
ms.openlocfilehash: 95e95d8849824cd2dc31c2019627cc5adbbefeec
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131145"
---
# <a name="adding-a-swipe-gesture-recognizer"></a>Ajout d’un module de reconnaissance de mouvement de balayage

_Un mouvement de balayage se produit lorsqu’un doigt est déplacé sur l’écran dans le sens horizontal ou vertical et est souvent utilisé pour lancer la navigation via le contenu. Les exemples de code dans cet article sont extraites de la [mouvement de balayage](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/) exemple._

Pour rendre un [ `View` ](xref:Xamarin.Forms.View) reconnaître un mouvement de balayage, créez un [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) de l’instance, définissez le [ `Direction` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) propriété à un [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) valeur d’énumération (`Left`, `Right`, `Up`, ou `Down`), définissez éventuellement le [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) propriété, gérez le [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) événement, et ajoutez la reconnaissance de mouvement de nouveau à la [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) collection sur la vue. Le code suivant montre l’exemple un `SwipeGestureRecognizer` attaché à un [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Voici l’équivalent C# code :

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Le [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) classe inclut également un [ `Threshold` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) propriété, ce qui peut éventuellement être définie pour une `uint` valeur qui représente la distance de balayage minimale qui doit être atteint pour un balayez pour être reconnus, en unités indépendantes du périphérique. La valeur par défaut de cette propriété est 100, ce qui signifie que n’importe quel où les balayages qui sont que moins de 100 unités indépendantes du périphérique seront ignorées.

## <a name="recognizing-the-swipe-direction"></a>Reconnaissant la direction de balayage

Dans les exemples ci-dessus, le [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) propriété est définie sur une valeur à partir d’un seul le [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) énumération. Toutefois, il est également possible de définir cette propriété à plusieurs valeurs à partir de la `SwipeDirection` énumération, afin que le [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) événement est déclenché en réponse à un balayage dans plusieurs directions. Toutefois, la contrainte est qu’un seul [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) peut seulement reconnaître où les balayages qui se produisent sur le même axe. Par conséquent, où les balayages qui se produisent sur l’axe horizontal peuvent être reconnus en définissant le `Direction` propriété `Left` et `Right`:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

De même, où les balayages qui se produisent sur l’axe vertical peuvent être reconnus en définissant le [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) propriété `Up` et `Down`:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

Vous pouvez également un [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) pour chaque passage de direction peut être créée afin qu’il reconnaisse où les balayages dans chaque direction :

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Right" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Up" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Down" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Voici l’équivalent C# code :

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;
var rightSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Right };
rightSwipeGesture.Swiped += OnSwiped;
var upSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up };
upSwipeGesture.Swiped += OnSwiped;
var downSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Down };
downSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
boxView.GestureRecognizers.Add(rightSwipeGesture);
boxView.GestureRecognizers.Add(upSwipeGesture);
boxView.GestureRecognizers.Add(downSwipeGesture);
```

> [!NOTE]
> Dans les exemples ci-dessus, le même gestionnaire d’événements répond à la [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) déclenchement des événements. Toutefois, chaque [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) instance peut utiliser un autre gestionnaire d’événements si nécessaire.

## <a name="responding-to-the-swipe"></a>Réponse au balayage

Un gestionnaire d’événements pour le [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) événement est illustré dans l’exemple suivant :

```csharp
void OnSwiped(object sender, SwipedEventArgs e)
{
    switch (e.Direction)
    {
        case SwipeDirection.Left:
            // Handle the swipe
            break;
        case SwipeDirection.Right:
            // Handle the swipe
            break;
        case SwipeDirection.Up:
            // Handle the swipe
            break;
        case SwipeDirection.Down:
            // Handle the swipe
            break;
    }
}
```

Le [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs) peut être examiné pour déterminer la direction du balayage, avec une logique personnalisée de réponse pour le passage en fonction des besoins. La direction du balayage peut être obtenue à partir de la [ `Direction` ](xref:Xamarin.Forms.SwipedEventArgs.Direction) propriété des arguments d’événement, qui sera défini à une des valeurs de la [ `SwipeDirection` ](xref:Xamarin.Forms.SwipeDirection) énumération. En outre, les arguments d’événement ont également un [ `Parameter` ](xref:Xamarin.Forms.SwipedEventArgs.Parameter) propriété qui sera définie à la valeur de la [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) propriété, s’il est défini.

## <a name="using-commands"></a>À l’aide de commandes

Le [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) classe inclut également [ `Command` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) et [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) propriétés. Ces propriétés sont généralement utilisées dans les applications qui utilisent le modèle Model-View-ViewModel (MVVM). Le `Command` propriété définit le `ICommand` à appeler lorsqu’un mouvement de balayage est reconnu, avec le `CommandParameter` définissant un objet à passer à la propriété le `ICommand.` l’exemple de code suivant montre comment lier le `Command` propriété à un `ICommand` défini dans le modèle de vue dont l’instance est défini comme la page [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext):

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

Le code XAML équivalent est :

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` est une propriété de type `ICommand` défini dans l’instance de modèle de vue qui est défini comme la page [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Lorsqu’un mouvement de balayage est reconnu, le `Execute` méthode de la `SwipeCommand` objet sera exécuté. L’argument pour le `Execute` méthode est la valeur de la [ `CommandParameter` ](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) propriété. Pour plus d’informations sur les commandes, consultez [l’Interface de commande](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="creating-a-swipe-container"></a>Création d’un conteneur de balayage

Le `SwipeContainer` (classe), ce qui est illustré dans l’exemple de code suivant, est une classe de reconnaissance de balayage généralisée être wrappé autour d’un [ `View` ](xref:Xamarin.Forms.View) pour effectuer la reconnaissance de mouvement de balayage :

```csharp
public class SwipeContainer : ContentView
{
    public event EventHandler<SwipedEventArgs> Swipe;

    public SwipeContainer()
    {
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Left));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Right));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Up));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Down));
    }

    SwipeGestureRecognizer GetSwipeGestureRecognizer(SwipeDirection direction)
    {
        var swipe = new SwipeGestureRecognizer { Direction = direction };
        swipe.Swiped += (sender, e) => Swipe?.Invoke(this, e);
        return swipe;
    }
}
```

Le `SwipeContainer` classe crée [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) objets pour toutes les directions de balayage de quatre et l’attache `Swipe` gestionnaires d’événements. Ces gestionnaires d’événements appeler le `Swipe` événement défini par le `SwipeContainer`.

L’exemple de code XAML suivant montre le `SwipeContainer` classe habillage un [ `BoxView` ](xref:Xamarin.Forms.BoxView):

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

Le code suivant montre l’exemple comment la `SwipeContainer` encapsule un [ `BoxView` ](xref:Xamarin.Forms.BoxView) dans un C# page :

```csharp
public class SwipeContainerPageCS : ContentPage
{
    public SwipeContainerPageCS()
    {
        var boxView = new BoxView { Color = Color.Teal, ... };
        var swipeContainer = new SwipeContainer { Content = boxView, ... };
        swipeContainer.Swipe += (sender, e) =>
        {
          // Handle the swipe
        };

        Content = new StackLayout
        {
            Children = { swipeContainer }
        };
    }
}
```

Lorsque le [ `BoxView` ](xref:Xamarin.Forms.BoxView) reçoit un mouvement de balayage, le [ `Swiped` ](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) événement dans le [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) est déclenché. Ceci est géré par le `SwipeContainer` (classe), qui se déclenche son propre `Swipe` événement. Cela `Swipe` événement est géré sur la page. Le [ `SwipedEventArgs` ](xref:Xamarin.Forms.SwipedEventArgs) peut ensuite être examiné pour déterminer la direction du balayage, avec une logique personnalisée de réponse pour le passage en fonction des besoins.

## <a name="related-links"></a>Liens connexes

- [Mouvement de balayage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/SwipeGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)
