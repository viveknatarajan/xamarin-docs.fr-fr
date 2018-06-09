---
title: Déclencheurs de Xamarin.Forms
description: Cet article explique comment utiliser des déclencheurs de Xamarin.Forms pour répondre aux modifications de l’interface utilisateur avec XAML. Déclencheurs permettent de vous permettent d’exprimer de manière déclarative en XAML, les actions qui modifient l’apparence des contrôles basés sur des événements ou des modifications de propriété.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: b28ebb8845b7eae0d818e1279b4d6eaef4ad5b8b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241433"
---
# <a name="xamarinforms-triggers"></a>Déclencheurs de Xamarin.Forms

Déclencheurs permettent de vous permettent d’exprimer de manière déclarative en XAML, les actions qui modifient l’apparence des contrôles basés sur des événements ou des modifications de propriété.

Vous pouvez affecter un déclencheur directement à un contrôle ou l’ajouter à un dictionnaire de ressources de niveau de la page ou au niveau de l’application à appliquer à plusieurs contrôles.

Il existe quatre types de déclencheur :

* [Déclencheur de propriété](#property) -se produit lorsqu’une propriété sur un contrôle est définie à une valeur particulière.

* [Déclencheur de données](#data) - utilise des données de liaison à un déclencheur basé sur les propriétés d’un autre contrôle.

* [Déclencheur d’événement](#event) -se produit lorsqu’un événement se produit sur le contrôle.

* [Déclencheur de multiples](#multi) -permet à plusieurs conditions du déclencheur à être définie avant une action se produit.

<a name="property" />

## <a name="property-triggers"></a>Déclencheurs de propriété

Un déclencheur simple peut être exprimé en XAML, ajout d’un `Trigger` élément d’un contrôle déclenche la collection.
Cet exemple montre un déclencheur qui modifie une `Entry` lorsqu’il reçoit le focus la couleur d’arrière-plan :

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Les parties importantes de la déclaration du déclencheur sont :

* **TargetType** -le type de contrôle auquel le déclencheur s’applique.

* **Propriété** -la propriété sur le contrôle qui est analysé.

* **Valeur** -la valeur, lorsque cela se produit pour la propriété surveillée, qui provoque l’activation du déclencheur.

* **Méthode Setter** -une collection de `Setter` éléments peuvent être ajoutés et lorsque la condition de déclenchement est remplie. Vous devez spécifier le `Property` et `Value` à définir.

* **EnterActions et ExitActions** (non affiché) - sont écrites dans le code et peut servir à (ou instead of) `Setter` éléments. Ils sont [ci-dessous](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Application d’un déclencheur à l’aide d’un Style

Les déclencheurs peuvent également être ajoutés à un `Style` déclaration d’un contrôle, une page ou d’une application `ResourceDictionary`. Cet exemple déclare un style implicite (ie. aucun `Key` est défini) qui signifie qu’il s’applique à tous les `Entry` contrôles sur la page.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Déclencheurs de données

Déclencheurs de données utilisent la liaison de données pour surveiller un autre contrôle pour provoquer le `Setter`s appel. Au lieu du `Property` dans un déclencheur de propriété, affectez le `Binding` attribut à analyser la valeur spécifiée.

L’exemple ci-dessous utilise la syntaxe de liaison de données `{Binding Source={x:Reference entry}, Path=Text.Length}` qui est la façon dont nous nous référons aux propriétés d’un autre contrôle. Lorsque la longueur de la `entry` est égal à zéro, le déclencheur est activé. Dans cet exemple le déclencheur désactive le bouton lorsque l’entrée est vide.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

Conseil : lors de l’évaluation `Path=Text.Length` toujours fournir une valeur par défaut pour la propriété cible (par exemple). `Text=""`), car sinon elle sera `null` et le déclencheur ne fonctionne pas comme prévu.

Outre la spécification `Setter`s, vous pouvez également fournir [ `EnterActions` et `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Déclencheurs d’événements

Le `EventTrigger` élément nécessite uniquement une `Event` propriété, tel que `"Clicked"` dans l’exemple ci-dessous.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Notez qu’il n’y aucun `Setter` les éléments, mais plutôt une référence à une classe définie par `local:NumericValidationTriggerAction` qui requiert le `xmlns:local` pour être déclarée dans la page du XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

La classe elle-même implémente `TriggerAction` qui signifie qu’il doit fournir une substitution pour la `Invoke` méthode est appelée chaque fois que l’événement déclencheur se produit.

Une implémentation d’action de déclencheur doit :

* Implémenter le modèle générique `TriggerAction<T>` (classe), avec le paramètre générique correspondant avec le type de contrôle est appliqué pour le déclencheur. Vous pouvez utiliser comme superclasses `VisualElement` pour écrire les actions de déclencheur qui fonctionnent avec un large éventail de contrôles, ou spécifient un type de contrôle `Entry`.

* Remplacer la `Invoke` méthode - cela est appelée chaque fois que les critères de déclenchement sont satisfaits.

* Si vous le souhaitez exposer des propriétés qui peuvent être définies dans le code XAML lorsque le déclencheur est déclaré (tel que `Anchor`, `Scale`, et `Length` dans cet exemple).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Les propriétés exposées par l’action du déclencheur peuvent être définies dans la déclaration XAML, comme suit :

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Soyez prudent lorsque vous partagez des déclencheurs dans un `ResourceDictionary`, une seule instance sera partagée parmi les contrôles afin de n’importe quel état configuré qu’une seule fois s’applique à toutes les.

Notez que les déclencheurs d’événements ne prennent pas en charge `EnterActions` et `ExitActions` [ci-dessous](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Plusieurs déclencheurs

A `MultiTrigger` ressemble à un `Trigger` ou `DataTrigger` , sauf qu’il peut y avoir plus d’une condition. Toutes les conditions doivent être remplies avant du `Setter`s sont déclenchées.

Voici un exemple d’un déclencheur pour un bouton qui est lié à deux entrées différentes (`email` et `phone`) :

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

Le `Conditions` collection peut également contenir `PropertyCondition` éléments comme suit :

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Création d’un déclencheur de multiples « exiger toutes »

Le déclencheur de plusieurs mises à jour uniquement son contrôle lorsque toutes les conditions sont remplies. Test « toutes les longueurs de champ sont zéro » (par exemple, une page de connexion dans laquelle toutes les entrées doivent être terminées) est difficile, car vous souhaitez une condition « où Text.Length > 0 », mais il ne peut pas être exprimée en XAML.

Cela peut être fait avec un `IValueConverter`. Le code de convertisseur ci-dessous transformations le `Text.Length` de liaison dans un `bool` qui indique si un champ est vide ou non :


```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Pour utiliser ce convertisseur dans un déclencheur multiples, d’abord l’ajouter au dictionnaire de ressources de la page (avec une personnalisée `xmlns:local` définition de l’espace de noms) :

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Le code XAML est présenté ci-dessous. Notez les différences suivantes dans le premier exemple de déclencheur multiples :

* Le bouton a `IsEnabled="false"` la valeur par défaut.
* Les conditions du déclencheur multi utilisent le convertisseur pour activer la `Text.Length` valeur en une valeur booléenne.
* Lorsque toutes les conditions sont `true`, la méthode setter rend du bouton `IsEnabled` propriété `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

Ces captures d’écran affichent la différence entre les deux exemples de déclencheur multi ci-dessus. Dans la partie supérieure de l’écran, le texte d’entrée dans un seul `Entry` est suffisant pour permettre la **enregistrer** bouton.
Dans la partie inférieure de l’écran, la **connexion** bouton reste inactif jusqu'à ce que les deux champs contiennent des données.


![](triggers-images/multi-requireall.png "Exemples multiTrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions et ExitActions

Une autre consiste à implémenter les modifications lorsqu’un déclencheur se produit en ajoutant `EnterActions` et `ExitActions` collections et en spécifiant `TriggerAction<T>` implémentations.

Vous pouvez fournir *les deux* `EnterActions` et `ExitActions` ainsi que `Setter`s dans un déclencheur, mais gardez à l’esprit qui le `Setter`s sont appelées immédiatement (qu’ils n’attendent pas la `EnterAction` ou `ExitAction` à Terminer). Ou bien vous pouvez effectuer tous les éléments dans le code et utilisez pas `Setter`s du tout.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Comme toujours, lorsqu’une classe est référencée dans XAML vous devez déclarer un espace de noms tels que `xmlns:local` comme indiqué ici :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Le `FadeTriggerAction` code est indiqué ci-dessous :

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Remarque : `EnterActions` et `ExitActions` sont ignorés sur **les déclencheurs d’événements**.



## <a name="related-links"></a>Liens associés

- [Exemple de déclencheurs](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
