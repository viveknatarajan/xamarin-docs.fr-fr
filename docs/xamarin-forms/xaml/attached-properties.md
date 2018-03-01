---
title: "Propriétés jointes"
description: "Une propriété jointe est un type spécial de propriété pouvant être liée, définie dans une classe mais est associé à d’autres objets et reconnaissable dans XAML en tant qu’attribut qui contient une classe et un nom de propriété séparés par un point. Cet article fournit une introduction aux propriétés jointes et montre comment créer et les utiliser."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7112812c843ccbcd6c24ea028deae3c09851b03d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="attached-properties"></a>Propriétés jointes

_Une propriété jointe est un type spécial de propriété pouvant être liée, définie dans une classe mais est associé à d’autres objets et reconnaissable dans XAML en tant qu’attribut qui contient une classe et un nom de propriété séparés par un point. Cet article fournit une introduction aux propriétés jointes et montre comment créer et les utiliser._

## <a name="overview"></a>Vue d'ensemble

Joint des propriétés, activer un objet pour affecter une valeur pour une propriété qui ne définit pas sa propre classe. Par exemple, les enfants d’éléments peuvent utiliser attachée propriétés pour informer leur élément parent de la façon dont ils doivent être présentés dans l’interface utilisateur. Le [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) contrôle permet de ligne et colonne d’un enfant d’être spécifié en définissant le `Grid.Row` et `Grid.Column` propriétés jointes. `Grid.Row` et `Grid.Column` sont des propriétés jointes car ils sont définis sur des éléments qui sont des enfants d’un `Grid`, plutôt que sur le `Grid` lui-même.

Propriétés pouvant être liées doivent être implémentées en tant que propriétés attachées dans les scénarios suivants :

- Lorsqu’il est nécessaire d’avoir un mécanisme de paramètre de propriété disponible pour les classes autres que la définition de classe.
- Lorsque la classe représente un service qui doit être facilement intégré à d’autres classes.

Pour plus d’informations sur les propriétés pouvant être liées, consultez [propriétés pouvant être liées](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Création et utilisation d’une propriété jointe

Le processus de création d’une propriété jointe est comme suit :

1. Créer un [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance avec l’un de le [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) surcharges de méthode.
1. Fournir `static` `Get` *PropertyName* et `Set` *PropertyName* méthodes que les accesseurs de la propriété jointe.

### <a name="creating-a-property"></a>Création d’une propriété

Lorsque vous créez une propriété jointe pour une utilisation sur d’autres types, la classe dans laquelle la propriété est créée pas nécessairement dérivent [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/). Toutefois, le *cible* accesseurs de propriété doit être d’ou dériver [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Une propriété jointe qui peut être créée en déclarant un `public static readonly` propriété de type [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). La propriété peut être liée doit être définie à la valeur retournée de l’un de le [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) surcharges de méthode. La déclaration doit être dans le corps de la classe propriétaire, mais en dehors de toutes les définitions de membre.

Le code suivant montre un exemple d’une propriété jointe :

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Cette opération crée une propriété jointe nommée `HasShadow`, de type `bool`. La propriété est possédée par le `ShadowEffect` de classe, et a la valeur par défaut `false`. La convention d’affectation de noms pour les propriétés jointes est que l’identificateur de la propriété jointe doit correspondre au nom de propriété spécifié dans le `CreateAttached` méthode, avec « Property » ajouté à la fin. Par conséquent, dans l’exemple ci-dessus, l’identificateur de la propriété jointe est `HasShadowProperty`.

Pour plus d’informations sur la création de propriétés pouvant être liées, y compris les paramètres qui peuvent être spécifiées lors de la création, consultez [création et utilisation d’une propriété pouvant être liée](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Création d’accesseurs

Statique `Get` *PropertyName* et `Set` *PropertyName* méthodes sont requises, comme les accesseurs de la propriété attachée, sinon le système de propriétés ne peut pas utiliser la propriété jointe. Le `Get` *PropertyName* accesseur doit être conforme à la signature suivante :

```csharp
public static valueType GetPropertyName(BindableObject target)
```

Le `Get` *PropertyName* accesseur doit retourner la valeur qui est contenue dans le `BindableProperty` pour la propriété jointe. Cela peut être obtenue en appelant le [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) (méthode), en passant l’identificateur de propriété pouvant être liée sur laquelle obtenir la valeur, puis effectuer un cast de la valeur obtenue dans le type requis.

Le `Set` *PropertyName* accesseur doit être conforme à la signature suivante :

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

Le `Set` *PropertyName* accesseur doit définir la valeur correspondantes `BindableProperty` champ pour la propriété jointe. Cela peut être obtenue en appelant le [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) méthode, en passant l’identificateur de propriété pouvant être liée sur lequel définir la valeur et la valeur à définir.

Pour les accesseurs, le *cible* objet doit être d’ou dérivent, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

L’exemple de code suivant montre des accesseurs pour les `HasShadow` propriété attachée :

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>Utilisation d’une propriété jointe

Une fois qu’une propriété jointe a été créée, il peut être consommé à partir de XAML ou du code. En XAML, cela est possible en déclarant un espace de noms avec un préfixe, avec la déclaration d’espace de noms indiquant le nom d’espace de noms Common Language Runtime (CLR) et éventuellement un nom d’assembly. Pour plus d’informations, consultez [espaces de noms XAML](~/xamarin-forms/xaml/namespaces.md).

L’exemple de code suivant illustre un espace de noms XAML pour un type personnalisé qui contient une propriété jointe, qui est définie dans le même assembly que le code d’application qui référence le type personnalisé :

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

La déclaration d’espace de noms est ensuite utilisée lors de la définition de la propriété jointe sur un contrôle spécifique, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Utilisation d’une propriété jointe avec un Style

Propriétés jointes peuvent également être ajoutées à un contrôle à un style. L’exemple de code XAML suivant montre une *explicite* style qui utilise le `HasShadow` propriété attachée, qui peut être appliquée aux [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) contrôles :

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

Le [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) peuvent être appliquées à un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) en définissant son [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété le `Style` instance à l’aide de la `StaticResource`extension de balisage, comme illustré dans l’exemple de code suivant :

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Scénarios avancés

Lorsque vous créez une propriété attachée, il existe un nombre de paramètres facultatifs qui peuvent être définies pour permettre les scénarios avancés de propriété jointe. Cela inclut détecter des modifications de propriété, de valider les valeurs de propriété et de contraindre les valeurs de propriété. Pour plus d’informations, consultez [scénarios avancés](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Récapitulatif

Cet article fourni une introduction aux propriétés jointes et vous a montré comment créer et de les utiliser. Une propriété jointe est un type spécial de propriété pouvant être liée, définie dans une classe mais jointe à d’autres objets et reconnaissable dans XAML en tant qu’attributs qui contiennent une classe et un nom de propriété séparés par un point.


## <a name="related-links"></a>Liens associés

- [Propriétés pouvant être liées](~/xamarin-forms/xaml/bindable-properties.md)
- [Espaces de noms XAML](~/xamarin-forms/xaml/namespaces.md)
- [Effet d’ombre portée (exemple)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
