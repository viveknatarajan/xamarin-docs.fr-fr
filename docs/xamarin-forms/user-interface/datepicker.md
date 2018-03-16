---
title: "À l’aide du sélecteur de dates"
description: "Une vue Xamarin.Forms qui permet à l’utilisateur de sélectionner une date"
ms.topic: article
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/12/2018
ms.openlocfilehash: d47499c1e309fbc67c85b55cacbbba3942188f54
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2018
---
# <a name="using-datepicker"></a>À l’aide du sélecteur de dates

_Une vue Xamarin.Forms qui permet à l’utilisateur de sélectionner une date_

Le Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) appelle le contrôle de sélecteur de dates de la plateforme et permet à l’utilisateur de sélectionner une date. `DatePicker` définit les cinq propriétés :

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) de type [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), qui utilise par défaut le premier jour de l’année 1900.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) de type `DateTime`, les valeurs par défaut et le dernier jour de l’année 2100.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) de type `DateTime`, la date sélectionnée, qui utilise la valeur par défaut [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/).
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) de type `string`, un [standard](/dotnet/standard/base-types/standard-date-and-time-format-strings/) ou [personnalisé](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET mise en forme de chaîne, qui par défaut est « D », modèle de date long.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) de type [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), la couleur utilisée pour afficher la date sélectionnée, qui utilise par défaut [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).

Le `DatePicker` se déclenche un [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) événement lorsque l’utilisateur sélectionne une date.

> [!WARNING]
> Lors de la définition `MinimumDate` et `MaximumDate`, assurez-vous que `MinimumDate` est toujours inférieure ou égale à `MaximumDate`. Dans le cas contraire, `DatePicker` lève une exception.

En interne, le `DatePicker` garantit que `Date` entre `MinimumDate` et `MaximumDate`(inclus). Si `MinimumDate` ou `MaximumDate` est défini afin que `Date` n’est pas entre eux, `DatePicker` s’ajuste à la valeur de `Date`.

Toutes les cinq propriétés sont soutenues par [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) objets, ce qui signifie qu’ils peuvent avoir le format et les propriétés peuvent être des cibles de liaisons de données. Le `Date` propriété a un mode de liaison par défaut de [ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/), ce qui signifie qu’il peut être une cible d’une liaison de données dans une application qui utilise le [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architecture.

## <a name="initializing-the-datetime-properties"></a>Initialiser les propriétés de date/heure

Dans le code, vous pouvez initialiser la `MinimumDate`, `MaximumDate`, et `Date` propriétés aux valeurs de type `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Lorsqu’un `DateTime` valeur spécifiée en XAML, l’analyseur XAML utilise le `DateTime.Parse` méthode avec un `CultureInfo.InvariantCulture` argument à convertir la chaîne en un `DateTime` valeur. Les dates doivent être spécifiées dans un format précis : les mois à deux chiffres, les jours à deux chiffres et les années à quatre chiffres séparées par des barres obliques :

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Si le `BindingContext` propriété du `DatePicker` est définie sur une instance d’un ViewModel contenant les propriétés de type `DateTime` nommé `MinDate`, `MaxDate`, et `SelectedDate` (par exemple), vous pouvez instancier la `DatePicker` comme suit :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

Dans cet exemple, les trois propriétés sont initialisées dans les propriétés correspondantes dans le modèle de vues. Étant donné que la `Date` propriété possède un mode de liaison de `TwoWay`, toute nouvelle date que l’utilisateur sélectionne sont automatiquement reflétées dans le ViewModel.

Si le `DatePicker` ne contient pas de liaison sur son `Date` propriété, une application doit attacher un gestionnaire pour le `DateSelected` événement doit être informé lorsque l’utilisateur sélectionne une nouvelle date.

## <a name="datepicker-and-layout"></a>Sélecteur de dates et la disposition

Il est possible d’utiliser une option de disposition horizontale sans contrainte comme `Center`, `Start`, ou `End` avec `DatePicker`:

```xaml
<DatePicker ··· 
            HorizontalOptions="Center" 
            ··· />
```

Toutefois, cela n’est pas recommandée. Selon le paramètre de la `Format` propriété sélectionnée dates peuvent nécessiter la largeur d’affichage différente. Par exemple, la chaîne de format « D », `DateTime` pour afficher des dates dans un format long et « Mercredi 12 septembre 2018 » nécessite une largeur d’écran supérieure à « Vendredi 4 mai 2018 ». Selon la plateforme, cette différence peut entraîner la `DateTime` vue pour modifier la largeur de disposition, ou pour l’affichage à tronquer.

> [!TIP]
> Il est préférable d’utiliser la valeur par défaut `HorizontalOptions` paramètre `Fill` avec `DatePicker`et non ne pas d’utiliser une largeur de `Auto` lors du placement de `DatePicker` dans un `Grid` cellule.

## <a name="datepicker-in-an-application"></a>Sélecteur de dates dans une application

Le [ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) exemple inclut deux `DatePicker` vues sur la page. Ils peuvent être utilisés pour sélectionner les deux dates, et que le programme calcule le nombre de jours entre ces dates. Le programme ne change pas les paramètres de la `MinimumDate` et `MaximumDate` propriétés, de sorte que les deux dates doivent être comprise entre 1900 et 2100.

Voici le fichier XAML :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Chaque `DatePicker` est attribué un `Format` propriété de « D » pour un format de date longue. Notez également que la `endDatePicker` objet a une liaison qui cible son `MinimumDate` propriété. La source de liaison est sélectionné `Date` propriété de la `startDatePicker` objet. Cela garantit que la date de fin est toujours plus tard ou égale à la date de début. En plus de deux `DatePicker` objets, un `Switch` est étiqueté « Inclure les deux jours au total ». 

Les deux `DatePicker` vues ont des gestionnaires d’associés à la `DateSelected` événement et le `Switch` a un gestionnaire attaché à son `Toggled` événement. Ces gestionnaires d’événements dans le fichier code-behind et déclenchent un nouveau calcul des jours entre les deux dates :

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

Lorsque l’exemple est tout d’abord exécuté, les deux `DatePicker` vues sont initialisés à la date du jour. La capture d’écran suivante montre le programme en cours d’exécution sur iOS, Android et la plateforme Windows universelle :

[![Jours entre les Dates de début](datepicker-images/DaysBetweenDatesStart.png "jours entre les Dates de début")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "jours entre les Dates de début")

En cliquant sur un de le `DatePicker` affiche appelle le sélecteur de dates de plateforme. Les trois plateformes implémentent ce sélecteur de dates différentes, mais chaque approche est familière aux utilisateurs de ce type de plateforme :

[![Sélectionnez des jours entre les Dates](datepicker-images/DaysBetweenDatesSelect.png "jours entre les Dates sélectionnez")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "sélectionner de jours entre les Dates")

Une fois que les deux dates sont sélectionnés, l’application affiche le nombre de jours entre ces dates :

[![Jours entre les Dates résultat](datepicker-images/DaysBetweenDatesResult.png "jours entre les Dates résultat")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "jours entre les Dates de résultats")

## <a name="related-links"></a>Liens associés

- [Exemple de DaysBetweenDates](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
