---
title: EventToCommandBehavior réutilisable
description: Comportements peuvent être utilisés pour associer des commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes. Cet article montre à l’aide d’un comportement de Xamarin.Forms pour appeler une commande lorsqu’un événement est déclenché.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3cc568298ba3710f7d4f092acbb2139bebe5976b
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675287"
---
# <a name="reusable-eventtocommandbehavior"></a>EventToCommandBehavior réutilisable

_Comportements peuvent être utilisés pour associer des commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes. Cet article montre à l’aide d’un comportement de Xamarin.Forms pour appeler une commande lorsqu’un événement est déclenché._

## <a name="overview"></a>Vue d'ensemble

Le `EventToCommandBehavior` classe est un comportement personnalisé Xamarin.Forms réutilisable qui exécute une commande en réponse à *n’importe quel* déclenchement des événements. Par défaut, les arguments d’événement pour l’événement est transmis à la commande et peut être éventuellement converti par un [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implémentation.

Les propriétés de comportement suivantes doivent être définies pour utiliser le comportement :

- **EventName** – le nom de l’événement le comportement est à l’écoute.
- **Commande** : `ICommand` doit être exécuté. Le comportement s’attend à trouver le `ICommand` d’instance sur le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du contrôle attaché, qui peut être hérité d’un élément parent.

Les comportement facultatif propriétés suivantes peuvent également être définies :

- **CommandParameter** : un `object` qui sera passé à la commande.
- **Convertisseur** : un [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implémentation qui va changer le format des données d’événement argument comme il est passé entre *source* et *cible*par le moteur de liaison.

## <a name="creating-the-behavior"></a>Création d’un comportement

Le `EventToCommandBehavior` classe dérive de la `BehaviorBase<T>` classe qui dérive à son tour le [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) classe. L’objectif de la `BehaviorBase<T>` classe est de fournir une classe de base pour tous les comportements de Xamarin.Forms qui nécessitent le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du comportement à définir le contrôle attaché. Cela garantit que le comportement peut être liés à et exécuter le `ICommand` spécifié par le `Command` propriété lorsque le comportement est consommé.

Le `BehaviorBase<T>` classe fournit un substituable [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) méthode qui définit la [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du comportement et un substituable [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))méthode nettoie les `BindingContext`. En outre, la classe stocke une référence au contrôle attaché dans le `AssociatedObject` propriété.

### <a name="implementing-bindable-properties"></a>Implémentation des propriétés pouvant être liées

Le `EventToCommandBehavior` classe définit quatre [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) commande définie par les instances, qui s’exécutent à un utilisateur lorsqu’un événement est déclenché. Ces propriétés s’affichent dans l’exemple de code suivant :

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

Lorsque le `EventToCommandBehavior` classe est consommée, le `Command` propriété doit être lié aux données d’une `ICommand` doit être exécuté en réponse au déclenchement des événements qui est défini dans le `EventName` propriété. Le comportement s’attend à trouver le `ICommand` sur le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) du contrôle attaché.

Par défaut, les arguments d’événement pour l’événement seront passées à la commande. Ces données peuvent être éventuellement converties en tant qu’elle est transmise entre *source* et *cible* par le moteur de liaison, en spécifiant un [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) mise en œuvre en tant que le `Converter` valeur de propriété. Sinon, un paramètre peut être passé à la commande en spécifiant le `CommandParameter` valeur de propriété.

### <a name="implementing-the-overrides"></a>Implémenter les remplacements

Le `EventToCommandBehavior` substitue le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) et [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthodes de la `BehaviorBase<T>` classe, comme indiqué dans l’exemple de code suivant :

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

Le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) méthode exécute le programme d’installation en appelant le `RegisterEvent` méthode, en passant la valeur de la `EventName` propriété en tant que paramètre. Le [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthode exécute un nettoyage en appelant le `DeregisterEvent` méthode, en passant la valeur de la `EventName` propriété en tant que paramètre.

### <a name="implementing-the-behavior-functionality"></a>Implémentation de la fonctionnalité de comportement

L’objectif du comportement consiste à exécuter la commande définie par le `Command` propriété dans la réponse pour le déclenchement des événements qui est défini par le `EventName` propriété. La fonctionnalité de comportement principale est illustrée dans l’exemple de code suivant :

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

Le `RegisterEvent` méthode est exécutée en réponse à la `EventToCommandBehavior` qui est attaché à un contrôle et il reçoit la valeur de la `EventName` propriété en tant que paramètre. La méthode tente ensuite de localiser l’événement défini dans le `EventName` propriété, sur le contrôle attaché. Si l’événement peut être localisé, le `OnEvent` (méthode) n’est inscrit pour être la méthode de gestionnaire pour l’événement.

Le `OnEvent` méthode est exécutée en réponse au déclenchement des événements qui est défini dans le `EventName` propriété. Condition que le `Command` propriété fait référence à un élément valide `ICommand`, la méthode tente de récupérer un paramètre à passer à la `ICommand` comme suit :

- Si le `CommandParameter` propriété définit un paramètre, il sera récupéré.
- Sinon, si le `Converter` propriété définit un [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implémentation, le convertisseur est exécutée et convertit les données d’événement argument comme il est passé entre *source* et *cible* par le moteur de liaison.
- Sinon, les arguments d’événement sont supposées pour être le paramètre.

Les données liées `ICommand` est ensuite exécutée, en passant le paramètre à la commande, à condition que le [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object)) retourne de la méthode `true`.

Bien que non illustrée ici, le `EventToCommandBehavior` inclut également un `DeregisterEvent` méthode qui est exécutée par le [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) (méthode). Le `DeregisterEvent` méthode est utilisée pour localiser et annuler l’inscription de l’événement défini dans le `EventName` propriété, pour nettoyer les fuites de mémoire potentielle.

## <a name="consuming-the-behavior"></a>Utilisation du comportement

Le `EventToCommandBehavior` classe peut être attachée à la [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) collection d’un contrôle, comme illustré dans l’exemple de code XAML suivant :

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

Le `Command` propriété du comportement est lié aux données le `OutputAgeCommand` propriété du ViewModel associé, tandis que le `Converter` propriété est définie sur le `SelectedItemConverter` de l’instance, qui retourne le [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)de la [ `ListView` ](xref:Xamarin.Forms.ListView) à partir de la [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs).

Lors de l’exécution, le comportement répondra à l’interaction avec le contrôle. Lorsqu’un élément est sélectionné dans le [ `ListView` ](xref:Xamarin.Forms.ListView), le [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) événement se déclenche, qui s’exécute le `OutputAgeCommand` dans le ViewModel. À son tour est mis à jour le ViewModel `SelectedItemText` propriété qui le [ `Label` ](xref:Xamarin.Forms.Label) lie à, comme indiqué dans les captures d’écran suivante :

[![](event-to-command-behavior-images/screenshots-sml.png "Exemple d’Application avec EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "exemple d’Application avec EventToCommandBehavior")

L’avantage d’utiliser ce comportement pour exécuter une commande lorsqu’un événement se déclenche, est que les commandes peuvent être associés à des contrôles qui n’ont pas été conçus pour interagir avec les commandes. En outre, cela supprime le code de gestion des événements-standard à partir de fichiers code-behind.

## <a name="summary"></a>Récapitulatif

Cet article a montré à l’aide d’un comportement de Xamarin.Forms pour appeler une commande lorsqu’un événement est déclenché. Comportements peuvent être utilisés pour associer des commandes avec des contrôles qui n’ont pas été conçues pour interagir avec les commandes.


## <a name="related-links"></a>Liens associés

- [Comportement EventToCommand (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Comportement](xref:Xamarin.Forms.Behavior)
- [Comportement&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
