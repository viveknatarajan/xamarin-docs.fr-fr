---
title: Déclencheurs de Xamarin.Forms
description: Cet article explique comment utiliser des déclencheurs de Xamarin.Forms pour répondre aux modifications de l’interface utilisateur avec XAML. Autoriser les déclencheurs vous permettent d’exprimer de manière déclarative dans XAML, les actions qui modifient l’apparence des contrôles basés sur des événements ou des modifications de propriété.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995535"
---
# <a name="xamarinforms-triggers"></a>Déclencheurs de Xamarin.Forms

Autoriser les déclencheurs vous permettent d’exprimer de manière déclarative dans XAML, les actions qui modifient l’apparence des contrôles basés sur des événements ou des modifications de propriété.

Vous pouvez affecter un déclencheur directement à un contrôle, ou ajoutez-le à un dictionnaire de ressources de niveau page ou au niveau de l’application à appliquer à plusieurs contrôles.

Il existe quatre types de déclencheur :

* [Déclencheur de propriété](#property) -se produit lorsqu’une propriété sur un contrôle est définie à une valeur particulière.

* [Déclencheur de données](#data) - utilise des données de liaison de déclencheur basée sur les propriétés d’un autre contrôle.

* [Déclencheur d’événement](#event) -se produit lorsqu’un événement se produit sur le contrôle.

* [Déclencheur de multiples](#multi) -permet à plusieurs conditions du déclencheur à définir avant une action se produit.

<a name="property" />

## <a name="property-triggers"></a>Déclencheurs de propriété

Un déclencheur simple peut être exprimé entièrement dans XAML, ajout d’un `Trigger` élément d’un contrôle déclenche la collection.
Cet exemple montre un déclencheur qui modifie un `Entry` couleur d’arrière-plan lorsqu’il reçoit le focus :

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

* **Propriété** -la propriété sur le contrôle qui est surveillé.

* **Valeur** -la valeur, lorsqu’il se produit pour la propriété surveillée, qui entraîne l’activation du déclencheur.

* **Méthode Setter** -une collection de `Setter` éléments peuvent être ajoutés et lorsque la condition de déclenchement est remplie. Vous devez spécifier le `Property` et `Value` à définir.

* **EnterActions et ExitActions** (non affiché) : sont écrits en code et peut être utilisé en plus (ou à la place de) `Setter` éléments. Ils sont [ci-dessous](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Application d’un déclencheur à l’aide d’un Style

Les déclencheurs peuvent également être ajoutés à un `Style` déclaration sur un contrôle, dans une page ou d’une application `ResourceDictionary`. Cet exemple déclare un style implicite (ie. aucun `Key` est défini) ce qui signifie qu’elle s’applique à tous les `Entry` contrôles sur la page.

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

Déclencheurs de données utilisent la liaison de données pour surveiller un autre contrôle pour provoquer le `Setter`s appelée. Au lieu du `Property` dans un déclencheur de propriété, affectez la `Binding` attribut à surveiller pour la valeur spécifiée.

L’exemple ci-dessous utilise la syntaxe de liaison de données `{Binding Source={x:Reference entry}, Path=Text.Length}` qui est la façon dont nous faisons référence aux propriétés de contrôle à un autre. Lorsque la longueur de la `entry` est égal à zéro, le déclencheur est activé. Dans cet exemple le déclencheur désactive le bouton lorsque l’entrée est vide.

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

Conseil : lorsque vous évaluez `Path=Text.Length` toujours fournir une valeur par défaut pour la propriété cible (par exemple). `Text=""`), car sinon elle sera `null` et le déclencheur ne fonctionne pas comme prévu.

Outre la spécification `Setter`s, vous pouvez également fournir [ `EnterActions` et `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Déclencheurs d’événements

Le `EventTrigger` élément nécessite uniquement un `Event` propriété, tel que `"Clicked"` dans l’exemple ci-dessous.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Notez qu’il existe aucune `Setter` les éléments, mais plutôt une référence à une classe définie par `local:NumericValidationTriggerAction` qui nécessite le `xmlns:local` pour être déclarée dans la page du XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Implémente la classe elle-même `TriggerAction` qui signifie qu’il doit fournir un remplacement pour le `Invoke` méthode est appelée chaque fois que l’événement déclencheur se produit.

Une implémentation d’action de déclencheur doit :

* Implémenter le modèle générique `TriggerAction<T>` classe, avec le paramètre générique correspondant avec le type de contrôle pour le déclencheur sera appliqué. Vous pouvez utiliser comme superclasse `VisualElement` pour écrire les actions du déclencheur qui fonctionnent avec un large éventail de contrôles, ou spécifient un type de contrôle comme `Entry`.

* Remplacer le `Invoke` méthode - Ceci est appelé chaque fois que les critères de déclenchement sont satisfaits.

* Si vous le souhaitez exposer des propriétés qui peuvent être définies dans le XAML lorsque le déclencheur est déclaré (tel que `Anchor`, `Scale`, et `Length` dans cet exemple).

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

Les propriétés exposées par l’action du déclencheur peuvent être définies dans la déclaration XAML comme suit :

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Soyez prudent lorsque vous partagez des déclencheurs dans un `ResourceDictionary`, une instance sera partagée parmi les contrôles afin de n’importe quel état une fois configuré s’applique à toutes les.

Notez que les déclencheurs d’événements ne prennent pas en charge `EnterActions` et `ExitActions` [ci-dessous](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Plusieurs déclencheurs

Un `MultiTrigger` ressemble à un `Trigger` ou `DataTrigger` , à ceci près qu’il peut y avoir plus d’une condition. Toutes les conditions doivent être remplies avant le `Setter`s sont déclenchées.

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

Le `Conditions` collection peut contenir également `PropertyCondition` éléments comme suit :

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Création d’un déclencheur de plusieurs « nécessitent tous »

Le déclencheur de plusieurs mises à jour uniquement son contrôle lorsque toutes les conditions sont remplies. Tests pour une « toutes les longueurs de champ sont zéro » (par exemple, une page de connexion où toutes les entrées doivent être terminées) est difficile, car vous voulez une condition « où Text.Length > 0 », mais il ne peut pas être exprimé en XAML.

Cela est possible avec un `IValueConverter`. Le code de convertisseur ci-dessous transformations le `Text.Length` de liaison dans un `bool` qui indique si un champ est vide ou non :


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

Pour utiliser ce convertisseur dans un déclencheur multiple, d’abord l’ajouter au dictionnaire de ressources de la page (avec un personnalisé `xmlns:local` définition de l’espace de noms) :

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Le XAML est indiqué ci-dessous. Notez les différences suivantes du premier exemple de déclencheur de multiples :

* Le bouton a `IsEnabled="false"` la valeur par défaut.
* Les conditions du déclencheur multi utilisent le convertisseur pour activer la `Text.Length` valeur dans une valeur booléenne.
* Lorsque toutes les conditions sont `true`, la méthode setter rend le bouton `IsEnabled` propriété `true`.

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

Ces captures d’écran montrent la différence entre les deux exemples de déclencheur multi ci-dessus. Dans la partie supérieure des écrans, de saisie de texte seul `Entry` est suffisant pour activer la **enregistrer** bouton.
Dans la partie inférieure de l’écran, le **connexion** bouton reste inactif jusqu'à ce que les deux champs contiennent des données.


![](triggers-images/multi-requireall.png "Exemples multiTrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions et ExitActions

Une autre consiste à implémenter les modifications lorsqu’un déclencheur se produit en ajoutant `EnterActions` et `ExitActions` collections et en spécifiant `TriggerAction<T>` implémentations.

Vous pouvez fournir *à la fois* `EnterActions` et `ExitActions` ainsi que `Setter`s dans un déclencheur, mais sachez qui le `Setter`s sont appelées immédiatement (elles n’attendent pas que le `EnterAction` ou `ExitAction` à Terminer). Vous pouvez également vous pouvez effectuer tous les éléments dans le code et utiliser pas `Setter`s du tout.

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

Comme toujours, lorsqu’une classe est référencée dans XAML doit déclarer un espace de noms tels que `xmlns:local` comme indiqué ici :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Le `FadeTriggerAction` code est illustré ci-dessous :

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

Remarque : `EnterActions` et `ExitActions` sont ignorés sur **déclencheurs d’événements**.



## <a name="related-links"></a>Liens associés

- [Exemples de déclencheurs](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Documentation de l’API Xamarin.Forms](xref:Xamarin.Forms.TriggerAction`1)
