---
title: EffectBehavior réutilisable
description: Les comportements sont une approche utile pour l’ajout d’un effet à un contrôle, suppression d’effet de maquette code à partir de fichiers code-behind de gestion. Cet article montre à l’aide d’un comportement de Xamarin.Forms pour ajouter un effet à un contrôle.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 1ce7eda6f556041cbffc3793b00e8e2cba44d3d0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995779"
---
# <a name="reusable-effectbehavior"></a>EffectBehavior réutilisable

_Les comportements sont une approche utile pour l’ajout d’un effet à un contrôle, suppression d’effet de maquette code à partir de fichiers code-behind de gestion. Cet article montre à l’aide d’un comportement de Xamarin.Forms pour ajouter un effet à un contrôle._

## <a name="overview"></a>Vue d'ensemble

Le `EffectBehavior` classe est un comportement personnalisé Xamarin.Forms réutilisable qui ajoute un [ `Effect` ](xref:Xamarin.Forms.Effect) instance à un contrôle lorsque le comportement est attaché au contrôle et supprime le `Effect` lorsque le comportement est l’instance détachée à partir du contrôle.

Les propriétés de comportement suivantes doivent être définies pour utiliser le comportement :

- **Groupe** – la valeur de la [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribut pour la classe de l’effet.
- **Nom** – la valeur de la [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) attribut pour la classe de l’effet.

Pour plus d’informations sur les effets, consultez [effets](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Création d’un comportement

Le `EffectBehavior` classe dérive de la [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) (classe), où `T` est un [ `View` ](xref:Xamarin.Forms.View). Cela signifie que la `EffectBehavior` classe peut être attachée à n’importe quel contrôle Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Implémentation des propriétés pouvant être liées

Le `EffectBehavior` classe définit deux [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instances, qui sont utilisés pour ajouter un [ `Effect` ](xref:Xamarin.Forms.Effect) à un contrôle lorsque le comportement est attaché au contrôle. Ces propriétés s’affichent dans l’exemple de code suivant :

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

Lorsque le `EffectBehavior` est consommée, le `Group` propriété doit être définie à la valeur de la [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) attribut pour l’effet. En outre, le `Name` propriété doit être définie à la valeur de la [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) attribut pour la classe de l’effet.

### <a name="implementing-the-overrides"></a>Implémenter les remplacements

Le `EffectBehavior` substitue le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) et [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthodes de la [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) classe, comme indiqué dans le code suivant exemple :

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

Le [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) méthode exécute le programme d’installation en appelant le `AddEffect` méthode, en passant le contrôle attaché en tant que paramètre. Le [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) méthode exécute un nettoyage en appelant le `RemoveEffect` méthode, en passant le contrôle attaché en tant que paramètre.

### <a name="implementing-the-behavior-functionality"></a>Implémentation de la fonctionnalité de comportement

L’objectif du comportement consiste à ajouter le [ `Effect` ](xref:Xamarin.Forms.Effect) définies dans le `Group` et `Name` propriétés à un contrôle lorsque le comportement est attaché au contrôle et supprimez le `Effect` lorsque le comportement est détachée à partir du contrôle. La fonctionnalité de comportement principale est illustrée dans l’exemple de code suivant :

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

Le `AddEffect` méthode est exécutée en réponse à la `EffectBehavior` qui est attaché à un contrôle et il reçoit le contrôle attaché en tant que paramètre. La méthode ajoute ensuite l’effet récupéré du contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection. Le `RemoveEffect` méthode est exécutée en réponse à la `EffectBehavior` en cours de détachement à partir d’un contrôle et il reçoit le contrôle attaché en tant que paramètre. La méthode puis supprime l’effet du contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection.

Le `GetEffect` méthode utilise le [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) méthode pour récupérer le [ `Effect` ](xref:Xamarin.Forms.Effect). L’effet se trouve dans une concaténation de la `Group` et `Name` les valeurs de propriété. Si une plate-forme ne fournit pas l’effet, le `Effect.Resolve` méthode retournera un non -`null` valeur.

## <a name="consuming-the-behavior"></a>Utilisation du comportement

Le `EffectBehavior` classe peut être attachée à la [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) collection d’un contrôle, comme illustré dans l’exemple de code XAML suivant :

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Le code C# équivalent est affiché dans l’exemple de code suivant :

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

Le `Group` et `Name` propriétés du comportement sont définies sur les valeurs de la [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) et [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) attributs pour la classe d’effet dans chaque spécifiques à la plateforme projet.

Lors de l’exécution, lorsque le comportement est attaché à la [ `Label` ](xref:Xamarin.Forms.Label) contrôle, le `Xamarin.LabelShadowEffect` figurera dans le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection. Il en résulte une ombre ajoutée au texte affiché par le `Label` contrôler, comme indiqué dans les captures d’écran suivante :

![](effect-behavior-images/screenshots.png "Exemple d’Application avec EffectsBehavior")

L’avantage d’utiliser ce comportement pour ajouter et supprimer les effets des contrôles est que le code de gestion d’effet de maquette peut être supprimée à partir de fichiers code-behind.

## <a name="summary"></a>Récapitulatif

Cet article a montré à l’aide d’un comportement pour ajouter un effet à un contrôle. Le `EffectBehavior` classe est un comportement personnalisé Xamarin.Forms réutilisable qui ajoute un [ `Effect` ](xref:Xamarin.Forms.Effect) instance à un contrôle lorsque le comportement est attaché au contrôle et supprime le `Effect` lorsque le comportement est l’instance détachée à partir du contrôle.


## <a name="related-links"></a>Liens associés

- [Effets](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Comportement de l’effet (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Comportement](xref:Xamarin.Forms.Behavior)
- [Comportement<T>](xref:Xamarin.Forms.Behavior`1)
