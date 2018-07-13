---
title: Comportements joints
description: Comportements joints sont les classes static avec un ou plusieurs des propriétés jointes. Cet article montre comment créer et consommer des comportements joints.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995344"
---
# <a name="attached-behaviors"></a>Comportements joints

_Comportements joints sont les classes static avec un ou plusieurs des propriétés jointes. Cet article montre comment créer et consommer des comportements joints._

## <a name="overview"></a>Vue d'ensemble

Une propriété jointe est un type spécial de propriété pouvant être liée. Ils sont définis dans une classe mais jointe à d’autres objets, et ils sont reconnaissables dans XAML en tant qu’attributs qui contiennent une classe et un nom de propriété séparés par un point.

Une propriété jointe peut définir un `propertyChanged` délégué qui sera exécutée lorsque la valeur de la propriété change, par exemple lorsque la propriété est définie sur un contrôle. Lorsque le `propertyChanged` délégué s’exécute, il a passé une référence au contrôle sur lequel elle est attachée et les paramètres qui contiennent les valeurs anciennes et nouvelles pour la propriété. Ce délégué peut être utilisé pour ajouter de nouvelles fonctionnalités au contrôle associé à la propriété en manipulant la référence est passée, comme suit :

1. Le `propertyChanged` délégué effectue un cast de la référence de contrôle, qui est reçue en tant qu’un [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), vers le type de contrôle le comportement est conçu pour améliorer.
1. Le `propertyChanged` délégué modifie les propriétés du contrôle, et appelle des méthodes du contrôle ou des gestionnaires d’événements inscrit pour les événements exposés par le contrôle, pour implémenter la fonctionnalité de comportement principale.

Un problème avec les comportements joints est qu’elles sont définies dans un `static` (classe), avec `static` propriétés et méthodes. Cela rend difficile de créer des comportements joints qui présentent l’état. En outre, les comportements de Xamarin.Forms ont remplacé comportements joints en tant que la meilleure approche pour la construction de comportement. Pour plus d’informations sur les comportements de Xamarin.Forms, consultez [les comportements de Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) et [réutilisable comportements](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Création d’un comportement attaché

L’exemple d’application montre un `NumericValidationBehavior`, qui met en évidence la valeur entrée par l’utilisateur dans un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôler en rouge, si elle n’est pas un `double`. Le comportement est indiqué dans l’exemple de code suivant :

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

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
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Le `NumericValidationBehavior` classe contient une propriété jointe nommée `AttachBehavior` avec un `static` méthodes getter et setter, qui contrôle l’ajout ou la suppression du comportement au contrôle auquel il est attaché. Registres de la propriété jointe la `OnAttachBehaviorChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. Cette méthode inscrit ou annulation inscrit un gestionnaire d’événements pour le [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) événement, selon la valeur de la `AttachBehavior` propriété jointe. La fonctionnalité principale du comportement est fournie par le `OnEntryTextChanged` (méthode), qui analyse la valeur entrée dans le [ `Entry` ](xref:Xamarin.Forms.Entry) par l’utilisateur et définit le `TextColor` propriété rouge si la valeur n’est pas un `double`.

## <a name="consuming-an-attached-behavior"></a>Utilisation d’un comportement attaché

Le `NumericValidationBehavior` classe peut être consommée en ajoutant le `AttachBehavior` propriété jointe un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôler, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

L’équivalent [ `Entry` ](xref:Xamarin.Forms.Entry) en c# est illustré dans l’exemple de code suivant :

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Lors de l’exécution, le comportement répondra à l’interaction avec le contrôle, en fonction de l’implémentation de comportement. Les captures d’écran suivantes illustrent le comportement d’attaché répondre à l’entrée non valide :

[![](attached-images/screenshots-sml.png "Exemple d’Application avec un comportement attaché")](attached-images/screenshots.png#lightbox "exemple d’Application avec un comportement attaché")

> [!NOTE]
> Comportements joints sont écrites pour un type de contrôle spécifique (ou une superclasse qui permettre s’appliquent à de nombreux contrôles), et elles doivent uniquement être ajoutées à un contrôle compatible. Tentative d’attachement d’un comportement à un contrôle incompatible entraîne un comportement inconnu et dépend de l’implémentation de comportement.

### <a name="removing-an-attached-behavior-from-a-control"></a>Suppression d’un comportement attaché à partir d’un contrôle

Le `NumericValidationBehavior` classe peut être supprimée à partir d’un contrôle en définissant le `AttachBehavior` propriété jointe `false`, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

L’équivalent [ `Entry` ](xref:Xamarin.Forms.Entry) en c# est illustré dans l’exemple de code suivant :

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Lors de l’exécution, le `OnAttachBehaviorChanged` méthode sera exécutée lorsque la valeur de la `AttachBehavior` propriété jointe est définie `false`. Le `OnAttachBehaviorChanged` méthode inscrira puis retirer le Gestionnaire d’événements pour le [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) événement, en garantissant que le comportement n’est pas exécuté lorsque l’utilisateur interagit avec le contrôle.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer et consommer des comportements joints. Comportements joints sont `static` classes avec un ou plusieurs des propriétés jointes.


## <a name="related-links"></a>Liens associés

- [Comportements joints (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
