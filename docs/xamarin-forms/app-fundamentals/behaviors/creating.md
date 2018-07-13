---
title: Comportements de Xamarin.Forms
description: Comportements de Xamarin.Forms sont créées en dérivant le comportement ou le comportement<T> classe. Cet article montre comment créer et consommer des comportements de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998893"
---
# <a name="xamarinforms-behaviors"></a>Comportements de Xamarin.Forms

_Comportements de Xamarin.Forms sont créées en dérivant le comportement ou le comportement<T> classe. Cet article montre comment créer et consommer des comportements de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

Le processus de création d’un comportement Xamarin.Forms est comme suit :

1. Créez une classe qui hérite de la [ `Behavior` ](xref:Xamarin.Forms.Behavior) ou [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) (classe), où `T` est le type du contrôle auquel le comportement doit s’appliquer.
1. Remplacer le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) méthode pour effectuer aucune autre configuration.
1. Remplacer le [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthode pour effectuer tout nettoyage nécessaire.
1. Implémenter la fonctionnalité principale du comportement.

Il en résulte dans la structure illustrée dans l’exemple de code suivant :

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

Le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) méthode est déclenchée immédiatement après le comportement est attaché à un contrôle. Cette méthode reçoit une référence au contrôle auquel il est attaché et peut être utilisé pour inscrire des gestionnaires d’événements ou effectuer d’autres paramètres requis pour prendre en charge la fonctionnalité de comportement. Par exemple, vous pouvez vous abonner à un événement sur un contrôle. La fonctionnalité de comportement serait, être implémentée dans le Gestionnaire d’événements pour l’événement.

Le [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthode est déclenchée lorsque le comportement est supprimé à partir du contrôle. Cette méthode reçoit une référence au contrôle auquel il est attaché et est utilisé pour effectuer tout nettoyage nécessaire. Par exemple, vous pouvez désabonner d’un événement sur un contrôle pour empêcher les fuites de mémoire.

Le comportement peut ensuite être consommé en le joignant à la [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) collection du contrôle approprié.

## <a name="creating-a-xamarinforms-behavior"></a>Création d’un comportement de Xamarin.Forms

L’exemple d’application montre un `NumericValidationBehavior`, qui met en évidence la valeur entrée par l’utilisateur dans un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôler en rouge, si elle n’est pas un `double`. Le comportement est indiqué dans l’exemple de code suivant :

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

Le `NumericValidationBehavior` dérive le [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) (classe), où `T` est un [ `Entry` ](xref:Xamarin.Forms.Entry). Le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) méthode inscrit un gestionnaire d’événements pour le [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) événement, avec la [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthode désinscrivant le `TextChanged`fuites d’événement pour empêcher la mémoire. La fonctionnalité principale du comportement est fournie par le `OnEntryTextChanged` (méthode), qui analyse la valeur entrée par l’utilisateur dans le `Entry`et définit le [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) propriété rouge si la valeur n’est pas un `double`.

> [!NOTE]
> Xamarin.Forms ne définit pas le `BindingContext` d’un comportement, étant donné que les comportements peuvent être partagés et appliquées à plusieurs contrôles avec des styles.

## <a name="consuming-a-xamarinforms-behavior"></a>Utilisation d’un comportement de Xamarin.Forms

Chaque contrôle Xamarin.Forms a une [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) collection, à laquelle un ou plusieurs comportements peuvent être ajoutés, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

L’équivalent [ `Entry` ](xref:Xamarin.Forms.Entry) en c# est illustré dans l’exemple de code suivant :

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Lors de l’exécution du comportement répondra à l’interaction avec le contrôle, en fonction de l’implémentation du comportement. Les captures d’écran suivantes montrent le comportement de répondre à l’entrée non valide :

[![](creating-images/screenshots-sml.png "Exemple d’Application avec le comportement de Xamarin.Forms")](creating-images/screenshots.png#lightbox "exemple d’Application avec le comportement de Xamarin.Forms")

> [!NOTE]
> Comportements sont écrites pour un type de contrôle spécifique (ou une superclasse qui permettre s’appliquent à de nombreux contrôles), et elles doivent uniquement être ajoutées à un contrôle compatible. Tentative d’attachement d’un comportement à un contrôle incompatible entraîne la levée d’une exception.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Utilisation d’un comportement de Xamarin.Forms avec un Style

Comportements peuvent également être utilisées par un style explicit ou implicite. Toutefois, création d’un style qui définit le [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) propriété d’un contrôle n’est pas possible car la propriété est en lecture seule. La solution consiste à ajouter une propriété jointe à la classe de comportement qui contrôle l’ajout et le comportement de suppression. Le processus est comme suit :

1. Ajoutez une propriété jointe à la classe de comportement qui permet de contrôler l’ajout ou la suppression du comportement pour le contrôle auquel le comportement est attaché. Assurez-vous que la propriété jointe enregistre un `propertyChanged` délégué qui sera exécutée lorsque la valeur de la propriété change.
1. Créer un `static` méthodes getter et setter pour la propriété jointe.
1. Implémenter une logique dans le `propertyChanged` délégué à ajouter et supprimer le comportement.

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

Le `NumericValidationBehavior` classe contient une propriété jointe nommée `AttachBehavior` avec un `static` méthodes getter et setter, qui contrôle l’ajout ou la suppression du comportement au contrôle auquel il est attaché. Registres de la propriété jointe la `OnAttachBehaviorChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. Cette méthode ajoute ou supprime le comportement au contrôle, selon la valeur de la `AttachBehavior` propriété jointe.

Le code suivant montre l’exemple un *explicite* de style pour le `NumericValidationBehavior` qui utilise le `AttachBehavior` attaché de propriété, et qui peut être appliqué à [ `Entry` ](xref:Xamarin.Forms.Entry) contrôles :

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

Le [ `Style` ](xref:Xamarin.Forms.Style) peut être appliqué à un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle en définissant son [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) propriété le `Style` à l’aide de l’instance la `StaticResource` extension de balisage, comme illustré dans l’exemple de code suivant :

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Vous pouvez ajouter des propriétés pouvant être liées à un comportement qui est définie ou interrogées dans XAML, si vous créez des comportements qui présentent l’état qu’ils ne doivent pas être partagées entre les contrôles dans un `Style` dans un `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Suppression d’un comportement d’un contrôle

Le [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthode est déclenchée lorsqu’un comportement est supprimé à partir d’un contrôle et est utilisé pour effectuer tout nettoyage nécessaire telles que l’annulation de l’abonnement à partir d’un événement pour empêcher une fuite de mémoire. Toutefois, les comportements ne sont pas supprimés de manière implicite des contrôles, sauf si le contrôle [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) collection est modifiée par un `Remove` ou `Clear` (méthode). L’exemple de code suivant illustre la suppression d’un comportement spécifique à partir d’un contrôle `Behaviors` collection :

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Vous pouvez également du contrôle [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) collection peut être effacée, comme illustré dans l’exemple de code suivant :

```csharp
entry.Behaviors.Clear();
```

En outre, notez que les comportements ne sont pas supprimés de manière implicite des contrôles lorsque les pages sont dépilés à partir de la pile de navigation. Au lieu de cela, ils doivent être explicitement supprimés avant les pages vont hors de portée.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer et consommer des comportements de Xamarin.Forms. Comportements de Xamarin.Forms sont créées en dérivant le [ `Behavior` ](xref:Xamarin.Forms.Behavior) ou [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) classe.


## <a name="related-links"></a>Liens associés

- [Comportement de Xamarin.Forms (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Comportement de Xamarin.Forms appliqué avec un Style (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Comportement](xref:Xamarin.Forms.Behavior)
- [Comportement<T>](xref:Xamarin.Forms.Behavior`1)
