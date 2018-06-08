---
title: Comportements de Xamarin.Forms
description: Xamarin.Forms comportements sont créées en dérivant le comportement ou un comportement<T> classe. Cet article montre comment créer et utiliser les comportements de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3a86e7713620eff90db995941eb35df7bc393a76
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848289"
---
# <a name="xamarinforms-behaviors"></a>Comportements de Xamarin.Forms

_Xamarin.Forms comportements sont créées en dérivant le comportement ou un comportement<T> classe. Cet article montre comment créer et utiliser les comportements de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

Le processus de création d’un comportement Xamarin.Forms est comme suit :

1. Créez une classe qui hérite de la [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) ou [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) (classe), où `T` est le type du contrôle auquel le comportement doit s’appliquer.
1. Remplacer la [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) méthode pour effectuer aucune autre configuration.
1. Remplacer la [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthode pour effectuer tout nettoyage nécessaire.
1. Implémenter les fonctionnalités principales du comportement.

Cela entraîne la structure illustrée dans l’exemple de code suivant :

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

Le [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) méthode est déclenchée immédiatement après le comportement est attaché à un contrôle. Cette méthode reçoit une référence au contrôle auquel il est connecté et peut servir à inscrire des gestionnaires d’événements ou effectuer d’autres paramètres qui est requis pour prendre en charge la fonctionnalité de comportement. Par exemple, vous pouvez vous abonner à un événement sur un contrôle. La fonctionnalité de comportement serait, être implémentée dans le Gestionnaire d’événements pour l’événement.

Le [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthode est déclenchée lorsque le comportement est supprimé du contrôle. Cette méthode reçoit une référence au contrôle auquel il est connecté et est utilisé pour effectuer le nettoyage nécessaire. Par exemple, vous pourriez annuler l’abonnement à un événement sur un contrôle pour éviter les fuites de mémoire.

Le comportement peut ensuite être consommé par l’attachement à la [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) collection du contrôle approprié.

## <a name="creating-a-xamarinforms-behavior"></a>Création d’un comportement de Xamarin.Forms

L’exemple d’application montre un `NumericValidationBehavior`, qui met en surbrillance la valeur entrée par l’utilisateur dans un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôler en rouge, si elle n’est pas un `double`. Le comportement est indiqué dans l’exemple de code suivant :

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Le `NumericValidationBehavior` dérive le [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) (classe), où `T` est un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Le [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) méthode inscrit un gestionnaire d’événements pour le [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) événement, avec la [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthode annule l’inscription du `TextChanged`fuites d’événement pour éviter de la mémoire. Les fonctionnalités principales du comportement sont fournie par le `OnEntryTextChanged` (méthode), qui analyse la valeur entrée par l’utilisateur dans le `Entry`et définit les [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) propriété rouge si la valeur n’est pas un `double`.

> [!NOTE]
> Xamarin.Forms ne définit pas le `BindingContext` d’un comportement, étant donné que les comportements peuvent être partagés et appliquées à plusieurs contrôles avec des styles.

## <a name="consuming-a-xamarinforms-behavior"></a>Utilisation d’un comportement de Xamarin.Forms

Chaque contrôle Xamarin.Forms a un [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) collection, à laquelle un ou plusieurs comportements peuvent être ajoutés, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

L’équivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) en c# est indiqué dans l’exemple de code suivant :

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Lors de l’exécution le comportement doit répondre à l’interaction avec le contrôle, en fonction de l’implémentation de comportement. Les captures d’écran suivantes illustrent le comportement de répondre à l’entrée non valide :

[![](creating-images/screenshots-sml.png "Exemple d’Application avec un comportement Xamarin.Forms")](creating-images/screenshots.png#lightbox "exemple d’Application avec le comportement de Xamarin.Forms")

> [!NOTE]
> Comportements sont écrites pour un type de contrôle spécifique (ou d’une superclasse qui permettre s’appliquent à de nombreux contrôles), et elles doivent uniquement être ajoutés à un contrôle compatible. Toute tentative d’attachement d’un comportement à un contrôle incompatible entraîne la levée d’une exception.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Utilisation d’un comportement Xamarin.Forms avec un Style

Comportements peuvent également être utilisés par un style explicit ou implicite. Toutefois, en créant un style qui définit les [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) propriété d’un contrôle n’est pas possible car la propriété est en lecture seule. La solution consiste à ajouter une propriété jointe à la classe de comportement qui contrôle l’ajout et le comportement de suppression. Le processus est comme suit :

1. Ajoutez une propriété jointe à la classe de comportement qui permet de contrôler l’ajout ou la suppression du comportement pour le contrôle auquel le comportement est attaché. Assurez-vous que la propriété jointe enregistre un `propertyChanged` délégué qui sera exécutée lorsque la valeur de la propriété change.
1. Créer un `static` accesseur Get et Set pour la propriété jointe.
1. Implémenter la logique dans le `propertyChanged` délégué à ajouter et supprimer le comportement.

L’exemple de code suivant montre une propriété jointe qui contrôle l’ajout et suppression de la `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

Le `NumericValidationBehavior` classe contient une propriété jointe nommée `AttachBehavior` avec un `static` getter et setter, qui contrôle l’ajout ou la suppression du comportement du contrôle auquel il est attaché. Registres de la propriété jointe la `OnAttachBehaviorChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. Cette méthode ajoute ou supprime le comportement au contrôle, en fonction de la valeur de la `AttachBehavior` propriété jointe.

Ce qui suit montre l’exemple de code une *explicite* de style pour le `NumericValidationBehavior` qui utilise le `AttachBehavior` jointes propriété, et qui peuvent être appliquées à [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôles :

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

Le [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) peuvent être appliquées à une [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle en définissant sa [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété le `Style` à l’aide de l’instance le `StaticResource` extension de balisage, comme illustré dans l’exemple de code suivant :

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Vous pouvez ajouter des propriétés pouvant être liées à un comportement qui est définie ou interrogée dans XAML, si vous créez des comportements qui présentent l’état qu’ils ne doivent pas être partagées entre les contrôles dans un `Style` dans un `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Suppression d’un comportement d’un contrôle

Le [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthode est déclenchée lorsqu’un comportement est supprimé d’un contrôle et est utilisé pour effectuer le nettoyage nécessaire telles que l’annulation d’abonnement à partir d’un événement pour empêcher une fuite de mémoire. Toutefois, les comportements ne sont pas implicitement supprimés à partir de contrôles, sauf si du contrôle [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) collection est modifiée par une `Remove` ou `Clear` (méthode). L’exemple de code suivant illustre la suppression d’un comportement spécifique à partir d’un contrôle `Behaviors` collection :

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Vous pouvez également du contrôle [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) collection peut être effacée, comme illustré dans l’exemple de code suivant :

```csharp
entry.Behaviors.Clear();
```

En outre, notez que les comportements ne sont pas implicitement supprimés à partir de contrôles lorsque les pages sont dépilés à partir de la pile de navigation. Au lieu de cela, ils doivent être explicitement supprimés avant les pages passe hors de portée.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer et utiliser les comportements de Xamarin.Forms. Xamarin.Forms comportements sont créées en dérivant de la [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) ou [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) classe.


## <a name="related-links"></a>Liens associés

- [Comportement de Xamarin.Forms (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Comportement de Xamarin.Forms appliquée avec un Style (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Comportement](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Comportement<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
