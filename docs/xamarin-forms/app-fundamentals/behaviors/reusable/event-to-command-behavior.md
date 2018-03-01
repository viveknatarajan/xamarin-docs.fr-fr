---
title: "EventToCommandBehavior réutilisable"
description: "Comportements peuvent être utilisés pour associer des commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes. Cet article décrit à l’aide d’un comportement de Xamarin.Forms pour appeler une commande lorsqu’un événement est déclenché."
ms.topic: article
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2727d83e55e305af1372ece35bdf22abfc653fe7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="reusable-eventtocommandbehavior"></a>EventToCommandBehavior réutilisable

_Comportements peuvent être utilisés pour associer des commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes. Cet article décrit à l’aide d’un comportement de Xamarin.Forms pour appeler une commande lorsqu’un événement est déclenché._

## <a name="overview"></a>Vue d'ensemble

Le `EventToCommandBehavior` classe est un comportement personnalisé Xamarin.Forms réutilisable qui exécute une commande en réponse à *tout* déclenchement des événements. Par défaut, les arguments d’événement pour l’événement sera passé à la commande et peut être éventuellement converti par un [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) mise en œuvre.

Les propriétés de comportement suivantes doivent être définies pour utiliser le comportement :

- **Nom_événement** : le nom de l’événement le comportement écoute.
- **Commande** : **ICommand** doit être exécuté. Le comportement s’attend à trouver le `ICommand` d’instance sur le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) du contrôle attaché, qui peut être hérité d’un élément parent.

Les comportement facultatif propriétés suivantes peuvent également être définies :

- **CommandParameter** : un `object` qui sera passé à la commande.
- **Convertisseur** : un [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implémentation qui modifiera le format des données d’événement argument comme il est passé entre *source* et *cible*par le moteur de liaison.

## <a name="creating-the-behavior"></a>Création d’un comportement

Le `EventToCommandBehavior` classe dérive de la `BehaviorBase<T>` (classe), qui à son tour dérive de la [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) classe. L’objectif de la `BehaviorBase<T>` classe consiste à fournir une classe de base pour tous les comportements de Xamarin.Forms qui nécessitent le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) du comportement à définir le contrôle attaché. Cela garantit que le comportement peut lier et exécuter le `ICommand` spécifié par le `Command` propriété lorsque le comportement est consommé.

Le `BehaviorBase<T>` classe fournit un substituable [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) méthode qui définit la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) du comportement et un substituable [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)méthode nettoie les `BindingContext`. En outre, la classe stocke une référence au contrôle attaché dans le `AssociatedObject` propriété.

### <a name="implementing-bindable-properties"></a>Implémentation des propriétés pouvant être liées

Le `EventToCommandBehavior` classe définit quatre [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instances, qui s’exécutent à un utilisateur défini par la commande lorsqu’un événement est déclenché. Ces propriétés s’affichent dans l’exemple de code suivant :

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

Lorsque le `EventToCommandBehavior` classe est consommé, le `Command` propriété doit être lié aux données d’une `ICommand` à exécuter en réponse au déclenchement d’événement qui est défini dans le `EventName` propriété. Le comportement s’attend à trouver le `ICommand` sur la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) du contrôle attaché.

Par défaut, les arguments d’événement pour l’événement passera à la commande. Ces données peuvent être converties éventuellement lorsqu’elle est transmise entre *source* et *cible* par le moteur de liaison, en spécifiant un [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) mise en œuvre en tant que le `Converter` valeur de propriété. Sinon, un paramètre peut être passé à la commande en spécifiant le `CommandParameter` valeur de propriété.

### <a name="implementing-the-overrides"></a>Mise en œuvre les remplacements

Le `EventToCommandBehavior` substitue le [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) et [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthodes de la `BehaviorBase<T>` classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

Le [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) méthode exécute le programme d’installation en appelant le `RegisterEvent` méthode, en passant la valeur de la `EventName` propriété en tant que paramètre. Le [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthode effectue un nettoyage en appelant le `DeregisterEvent` méthode, en passant la valeur de la `EventName` propriété en tant que paramètre.

### <a name="implementing-the-behavior-functionality"></a>Implémentation de la fonctionnalité de comportement

L’objectif du comportement consiste à exécuter la commande définie par le `Command` propriété dans la réponse au déclenchement d’événement défini par le `EventName` propriété. La fonctionnalité de comportement principale est indiquée dans l’exemple de code suivant :

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }       

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

Le `RegisterEvent` méthode est exécutée en réponse à la `EventToCommandBehavior` qui est attaché à un contrôle et il reçoit la valeur de la `EventName` propriété en tant que paramètre. La méthode essaie ensuite de trouver l’événement défini dans le `EventName` propriété, sur le contrôle attaché. Si l’événement peut se trouver, le `OnEvent` (méthode) est inscrit pour être la méthode de gestionnaire pour l’événement.

Le `OnEvent` méthode est exécutée en réponse au déclenchement des événements qui est défini dans le `EventName` propriété. Si le `Command` propriété fait référence à un élément valide `ICommand`, la méthode tente de récupérer un paramètre à passer à la `ICommand` comme suit :

- Si le `CommandParameter` propriété définit un paramètre, elle est récupérée.
- Sinon, si le `Converter` propriété définit un [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) mise en œuvre, le convertisseur est exécuté et convertit les données d’événement argument comme il est passé entre *source* et *cible* par le moteur de liaison.
- Dans le cas contraire, les arguments d’événement sont supposées pour être le paramètre.

Les données liées `ICommand` est ensuite exécutée, en passant le paramètre à la commande, à condition que la [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) méthode renvoie `true`.

Bien que ne pas indiqué ici, le `EventToCommandBehavior` inclut également un `DeregisterEvent` méthode qui est exécutée par le [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) (méthode). Le `DeregisterEvent` méthode est utilisée pour localiser et annuler l’inscription de l’événement défini dans le `EventName` propriété, pour effectuer un nettoyage des fuites de mémoire potentielle.

## <a name="consuming-the-behavior"></a>Utilisation du comportement

Le `EventToCommandBehavior` classe peut être attachée à la [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) collection d’un contrôle, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

Le `Command` propriété du comportement est lié aux données le `OutputAgeCommand` propriété ViewModel associé, tandis que la `Converter` est définie sur le `SelectedItemConverter` de l’instance, qui retourne le [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)de la [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) à partir de la [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/).

Lors de l’exécution, le comportement doit répondre à l’interaction avec le contrôle. Lorsqu’un élément est sélectionné dans le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), le [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) événement se déclenche, qui s’exécute le `OutputAgeCommand` dans ViewModel. À son tour cela met à jour le ViewModel `SelectedItemText` propriété qui le [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) lie à, comme indiqué dans les captures d’écran suivants :

[ ![](event-to-command-behavior-images/screenshots-sml.png "Exemple d’Application avec EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png "exemple d’Application avec EventToCommandBehavior")

L’avantage d’utiliser ce comportement pour exécuter une commande lorsqu’un événement est déclenché, est que les commandes peuvent être associées aux contrôles qui n’ont pas été conçus pour interagir avec les commandes. En outre, cela supprime le code de gestion des événements-maquette à partir des fichiers de code-behind.

## <a name="summary"></a>Récapitulatif

Cet article ne présente à l’aide d’un comportement de Xamarin.Forms pour appeler une commande lorsqu’un événement est déclenché. Comportements peuvent être utilisés pour associer des commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes.


## <a name="related-links"></a>Liens associés

- [Comportement de EventToCommand (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Behavior<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
