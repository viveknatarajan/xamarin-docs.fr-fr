---
title: Comportements attachés
description: Les comportements attachés sont des classes static avec un ou plusieurs des propriétés jointes. Cet article montre comment créer et consommer des comportements attachés.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: af891ad1ff1389d5a48c6c47ba1914b8d4dfc20f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="attached-behaviors"></a>Comportements attachés

_Les comportements attachés sont des classes static avec un ou plusieurs des propriétés jointes. Cet article montre comment créer et consommer des comportements attachés._

## <a name="overview"></a>Vue d'ensemble

Une propriété jointe est un type spécial de propriété pouvant être liée. Ils sont définis dans une classe mais jointe à d’autres objets, et sont identifiables dans XAML en tant qu’attributs qui contiennent une classe et un nom de propriété séparés par un point.

Une propriété jointe peut définir un `propertyChanged` délégué qui sera exécutée lorsque la valeur de la propriété change, par exemple lorsque la propriété est définie sur un contrôle. Lorsque le `propertyChanged` délégué s’exécute, il a passé une référence au contrôle sur lequel il est en cours d’attachement et de paramètres qui contiennent les valeurs anciennes et nouvelles pour la propriété. Ce délégué peut être utilisé pour ajouter de nouvelles fonctionnalités pour le contrôle associé à la propriété en manipulant la référence est passée, comme suit :

1. Le `propertyChanged` délégué effectue un cast de la référence de contrôle, qui est reçue en tant qu’un [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), vers le type de contrôle le comportement est conçu pour améliorer.
1. Le `propertyChanged` délégué modifie les propriétés du contrôle, et appelle des méthodes du contrôle, ou les gestionnaires d’événements de registres pour les événements exposés par le contrôle, pour implémenter la fonctionnalité de comportement principale.

Un problème avec les comportements attachés est qu’elles sont définies dans un `static` (classe), avec `static` propriétés et méthodes. Cela rend difficile créer des comportements attachés qui présentent l’état. En outre, les comportements de Xamarin.Forms ont remplacé comportements attachés en tant que la meilleure approche à la construction du comportement. Pour plus d’informations sur les comportements de Xamarin.Forms, consultez [Xamarin.Forms comportements](~/xamarin-forms/app-fundamentals/behaviors/creating.md) et [réutilisable comportements](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Création d’un comportement attaché

L’exemple d’application montre un `NumericValidationBehavior`, qui met en surbrillance la valeur entrée par l’utilisateur dans un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôler en rouge, si elle n’est pas un `double`. Le comportement est indiqué dans l’exemple de code suivant :

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

Le `NumericValidationBehavior` classe contient une propriété jointe nommée `AttachBehavior` avec un `static` getter et setter, qui contrôle l’ajout ou la suppression du comportement du contrôle auquel il est attaché. Registres de la propriété jointe la `OnAttachBehaviorChanged` méthode qui sera exécutée lorsque la valeur de la propriété change. Cette méthode inscrit ou annuler inscrit un gestionnaire d’événements pour le [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) événement, en fonction de la valeur de la `AttachBehavior` propriété jointe. Les fonctionnalités principales du comportement sont fournie par le `OnEntryTextChanged` (méthode), qui analyse la valeur entrée dans le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) par l’utilisateur et définit le `TextColor` propriété rouge si la valeur n’est pas un `double`.

## <a name="consuming-an-attached-behavior"></a>Utilisation d’un comportement attaché

Le `NumericValidationBehavior` classe peut être consommée en ajoutant le `AttachBehavior` propriété attachée une [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôler, comme illustré dans l’exemple de code XAML suivant :

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

L’équivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) en c# est indiqué dans l’exemple de code suivant :

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Lors de l’exécution, le comportement doit répondre à l’interaction avec le contrôle, en fonction de l’implémentation de comportement. Les captures d’écran suivantes illustrent le comportement d’attaché répondre à l’entrée non valide :

[![](attached-images/screenshots-sml.png "Exemple d’Application avec un comportement attaché")](attached-images/screenshots.png#lightbox "exemple d’Application avec un comportement attaché")

> [!NOTE]
> Comportements attachés sont écrites pour un type de contrôle spécifique (ou d’une superclasse qui permettre s’appliquent à de nombreux contrôles), et elles doivent uniquement être ajoutés à un contrôle compatible. Toute tentative d’attachement d’un comportement à un contrôle incompatible entraîne un comportement inconnu et dépend de l’implémentation de comportement.

### <a name="removing-an-attached-behavior-from-a-control"></a>Suppression d’un comportement attaché à partir d’un contrôle

Le `NumericValidationBehavior` classe peut être supprimée à partir d’un contrôle en définissant le `AttachBehavior` propriété attachée `false`, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

L’équivalent [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) en c# est indiqué dans l’exemple de code suivant :

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Lors de l’exécution, le `OnAttachBehaviorChanged` méthode sera exécutée lorsque la valeur de la `AttachBehavior` a la valeur de propriété jointe `false`. Le `OnAttachBehaviorChanged` méthode sera ensuite annuler l’inscription du Gestionnaire d’événements pour le [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) événement, garantissant que le comportement n’est pas exécuté lorsque l’utilisateur interagit avec le contrôle.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment créer et consommer des comportements attachés. Joint les comportements sont `static` classes avec une ou plusieurs propriétés jointes.


## <a name="related-links"></a>Liens associés

- [Comportements attachés (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
