---
title: "EffectBehavior réutilisable"
description: "Les comportements sont une approche très utile pour l’ajout d’un effet à un contrôle, incidence maquette la gestion du code à partir des fichiers de code-behind. Cet article décrit à l’aide d’un comportement de Xamarin.Forms pour ajouter un effet à un contrôle."
ms.topic: article
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 698e2baef26a985fa40c2943cd25aefae55381f9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="reusable-effectbehavior"></a>EffectBehavior réutilisable

_Les comportements sont une approche très utile pour l’ajout d’un effet à un contrôle, incidence maquette la gestion du code à partir des fichiers de code-behind. Cet article décrit à l’aide d’un comportement de Xamarin.Forms pour ajouter un effet à un contrôle._

## <a name="overview"></a>Vue d'ensemble

Le `EffectBehavior` classe est un comportement personnalisé Xamarin.Forms réutilisable qui ajoute un [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) instance à un contrôle lorsque le comportement est attaché au contrôle et supprime la `Effect` lorsque le comportement est de l’instance détachée du contrôle.

Les propriétés de comportement suivantes doivent être définies pour utiliser le comportement :

- **Groupe** : la valeur de la [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) attribut de la classe de l’effet.
- **Nom** : la valeur de la [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) attribut de la classe de l’effet.

Pour plus d’informations sur les effets, consultez [effets](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Création d’un comportement

Le `EffectBehavior` classe dérive de la [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) (classe), où `T` est un [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/). Cela signifie que la `EffectBehavior` classe pouvant être joints à n’importe quel contrôle Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Implémentation des propriétés pouvant être liées

Le `EffectBehavior` classe définit deux [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instances, qui sont utilisées pour ajouter un [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) à un contrôle lorsque le comportement est attaché au contrôle. Ces propriétés s’affichent dans l’exemple de code suivant :

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

Lorsque le `EffectBehavior` est consommé, le `Group` propriété doit être définie à la valeur de la [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) attribut pour l’effet. En outre, le `Name` propriété doit être définie à la valeur de la [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) attribut de la classe de l’effet.

### <a name="implementing-the-overrides"></a>Mise en œuvre les remplacements

Le `EffectBehavior` substitue le [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) et [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthodes de la [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) de classe, comme indiqué dans le code suivant exemple :

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

Le [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) méthode exécute le programme d’installation en appelant le `AddEffect` méthode, en passant le contrôle attaché en tant que paramètre. Le [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) méthode effectue un nettoyage en appelant le `RemoveEffect` méthode, en passant le contrôle attaché en tant que paramètre.

### <a name="implementing-the-behavior-functionality"></a>Implémentation de la fonctionnalité de comportement

L’objectif du comportement est d’ajouter le [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) défini dans le `Group` et `Name` propriétés à un contrôle lorsque le comportement est attaché au contrôle et supprimer la `Effect` lorsque le comportement est détachée du contrôle. La fonctionnalité de comportement principale est indiquée dans l’exemple de code suivant :

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

Le `AddEffect` méthode est exécutée en réponse à la `EffectBehavior` qui est attaché à un contrôle et il reçoit le contrôle attaché en tant que paramètre. La méthode ajoute ensuite l’effet extrait du contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection. Le `RemoveEffect` méthode est exécutée en réponse à la `EffectBehavior` en cours de détachement à partir d’un contrôle et il reçoit le contrôle attaché en tant que paramètre. La méthode supprime ensuite l’effet à partir du contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection.

Le `GetEffect` utilise le [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) pour récupérer le [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/). L’effet se trouve dans une concaténation de la `Group` et `Name` les valeurs de propriété. Si une plate-forme ne fournit pas l’effet, le `Effect.Resolve` méthode retournera non -`null` valeur.

## <a name="consuming-the-behavior"></a>Utilisation du comportement

Le `EffectBehavior` classe peut être attachée à la [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) collection d’un contrôle, comme illustré dans l’exemple de code XAML suivant :

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

Le `Group` et `Name` propriétés de comportement sont définies sur les valeurs de la [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) et [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) attributs pour la classe d’effet dans chaque spécifique à la plateforme projet.

Lors de l’exécution, lorsque le comportement est attaché à la [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (contrôle), le `Xamarin.LabelShadowEffect` sera ajouté au contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection. Cela entraîne une ombre ajoutée au texte affiché par le `Label` de contrôle, comme indiqué dans les captures d’écran suivants :

![](effect-behavior-images/screenshots.png "Exemple d’Application avec EffectsBehavior")

L’avantage d’utiliser ce comportement pour ajouter et supprimer des effets à partir de contrôles est que le code de gestion d’effet maquette peut être supprimée à partir de fichiers de code-behind.

## <a name="summary"></a>Récapitulatif

Cet article ne présente à l’aide d’un comportement pour ajouter un effet à un contrôle. Le `EffectBehavior` classe est un comportement personnalisé Xamarin.Forms réutilisable qui ajoute un [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) instance à un contrôle lorsque le comportement est attaché au contrôle et supprime la `Effect` lorsque le comportement est de l’instance détachée du contrôle.


## <a name="related-links"></a>Liens associés

- [Effets](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Comportement de l’effet (exemple)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Behavior<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
