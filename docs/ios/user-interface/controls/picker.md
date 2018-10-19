---
title: Contrôle de sélecteur dans Xamarin.iOS
description: Ce document décrit comment concevoir et utiliser des contrôles de sélecteur dans une application Xamarin.iOS. Il explique comment implémenter un sélecteur dans le code et dans le concepteur iOS.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/14/2018
ms.openlocfilehash: 0ef33c2036b1ff2d5a7e2035ca5fa8af58672867
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "34789910"
---
# <a name="picker-control-in-xamarinios"></a>Contrôle de sélecteur dans Xamarin.iOS

Un [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) rend possible de choisir une valeur dans une liste par des composants individuels d’une interface semblable à la roulette de défilement.

Sélecteurs sont fréquemment utilisés pour sélectionner une date et heure ; Apple fournit le [`UIDatePicker`](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/)
classe à cet effet.

L’article explique comment implémenter et utiliser le `UIPickerView` et `UIDatePicker` contrôles.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>Implémentation d’un sélecteur

Implémenter un sélecteur par instanciation d’un nouvel `UIPickerView`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>Sélecteurs et les tables de montage séquentiel

Pour créer un sélecteur dans la **concepteur iOS**, faites glisser un **sélecteur de vue** à partir de la **boîte à outils** à l’aire de conception.

![Faites glisser un sélecteur de vue à l’aire de conception](picker-images/image1.png "faites glisser un sélecteur de vue à l’aire de conception")

### <a name="working-with-a-picker-control"></a>Utilisation d’un contrôle de sélecteur

Un sélecteur utilise un _modèle_ pour interagir avec les données :

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

Le [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) classe de base implémente deux interfaces, [`IUIPickerDataSource`](https://developer.xamarin.com/api/type/UIKit.IUIPickerViewDataSource/)
et [ `IUIPickerViewDelegate` ](https://developer.xamarin.com/api/type/UIKit.IUIPickerViewDelegate/), déclarer des diverses méthodes qui spécifient les données d’un sélecteur et comment il gère l’interaction :

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

Un sélecteur peut avoir plusieurs colonnes, ou _composants_. Composants partitionnement un sélecteur de plusieurs sections, ce qui permet la sélection de données plus facile et plus spécifique :

![Sélecteur de deux composants](picker-images/image3.png "sélecteur avec deux composants")

Pour spécifier le nombre de composants dans un sélecteur, utilisez le [`GetComponentCount`](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetComponentCount/p/UIKit.UIPickerView/) 
.

### <a name="customizing-a-pickers-appearance"></a>Personnalisation de l’apparence d’un sélecteur

Pour personnaliser l’apparence d’un sélecteur, utilisez le [`UIPickerView.UIPickerViewAppearance`](https://developer.xamarin.com/api/type/UIKit.UIPickerView+UIPickerViewAppearance/)
classe ou de remplacer le [ `GetView` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetView/p/UIKit.UIPickerView/System.nint/System.nint/UIKit.UIView/) et [ `GetRowHeight` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.GetRowHeight/p/UIKit.UIPickerView/System.nint/) méthodes dans le `UIPickerViewModel`.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>Implémentation d’un sélecteur de dates

Implémenter un sélecteur de dates en instanciant un `UIDatePicker`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>Sélecteurs de dates et des storyboards

Pour créer un sélecteur de dates dans le **concepteur iOS**, faites glisser un **sélecteur de dates** à partir de la **boîte à outils** à l’aire de conception.

![Faites glisser un sélecteur de dates à l’aire de conception](picker-images/image2.png "faites glisser un sélecteur de dates à l’aire de conception")

### <a name="date-picker-properties"></a>Propriétés du sélecteur de date

#### <a name="minimum-and-maximum-date"></a>Au minimum et la date maximale

[`MinimumDate`](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MinimumDate/) et [ `MaximumDate` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MaximumDate/) limiter la plage de dates disponibles dans le sélecteur de dates. Par exemple, le code suivant contraint un sélecteur de dates pour les années soixante mènent au moment présent :

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

> [!TIP]
> Il est possible d’effectuer un cast explicite un `DateTime` à un `NSDate`:
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>Intervalle de minutes

Le [ `MinuteInterval` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.MinuteInterval/) propriété définit l’intervalle auquel le sélecteur affiche les minutes :

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>Mode

Sélecteurs de dates prend en charge quatre [modes](https://developer.xamarin.com/api/type/UIKit.UIDatePickerMode/), décrit ci-dessous :

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

`UIDatePickerMode.Time` Affiche l’heure avec un sélecteur d’heure et minute et une désignation AM ou PM facultative :

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode.Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` Affiche la date avec un mois, jour et sélecteur d’année :

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode.Date](picker-images/image7.png "UIDatePickerMode.Date")

L’ordre des sélecteurs dépend des paramètres régionaux du sélecteur de dates, ce qui utilise les paramètres régionaux système par défaut. L’image ci-dessus montre la disposition des sélecteurs dans le `en_US` paramètres régionaux, mais ce qui suit modifie l’ordre à jour | Mois | Année :

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![Jour | Mois | Année](picker-images/image9.png "jour | Mois | Année")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` affiche une vue abrégée de la date, l’heure en heures et minutes et une désignation AM ou PM facultative (en fonction de l’utilisation d’une horloge de 12 ou 24 heures) :

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode.DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

Comme avec [ `UIDatePickerMode.Date` ](#uidatepickermodedate), l’ordre des sélecteurs et l’utilisation d’une horloge de 12 ou 24 heures dépendent des paramètres régionaux du sélecteur de date.

> [!TIP]
> Utilisez le `Date` propriété pour capturer la valeur d’un sélecteur de dates en mode `UIDatePickerMode.Time`, `UIDatePickerMode.Date`, ou `UIDatePickerMode.DateAndTime`. Cette valeur est stockée comme un `NSDate`.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` Affiche les valeurs d’heure et minute :

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

![« UIDatePickerMode.CountDownTimer »](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

Le `CountDownDuration` propriété capture la valeur d’un sélecteur de dates dans `UIDatePickerMode.CountDownTimer` mode. Par exemple, pour ajouter la valeur de compte à rebours à la date actuelle :

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

Pour mettre en forme un `NSDate`, utilisez un [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/).

Pour utiliser un `NSDateFormatter`, appelez sa [ `ToString` ](https://developer.xamarin.com/api/member/Foundation.NSDateFormatter.ToString/p/Foundation.NSDate/) (méthode). Exemple :

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

Le [ `DateFormat` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.DateFormat/) propriété (une chaîne) d’un `NSDateFormatter` autorise une spécification de format de date personnalisable :

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

Le [ `TimeStyle` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.TimeStyle/) propriété (une [ `NSDateFormatterStyle` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatterStyle/)) d’un `NSDateFormatter` spécifie la mise en forme de temps basé sur les styles prédéterminés :

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Divers `NSDateFormatterStyle` valeurs heures s’affichent comme suit :

- `NSDateFormatterStyle.Full`: 7:46:00 PM est) (heure
- `NSDateFormatterStyle.Long`: 7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 7:47:00 PM
- `NSDateFormatterSytle.Short`: 7:47 PM

##### <a name="datestyle"></a>DateStyle

Le [ `DateStyle` ](https://developer.xamarin.com/api/property/Foundation.NSDateFormatter.DateStyle/) propriété (une `NSDateFormatterStyle`) d’un `NSDateFormatter` spécifie la date mise en forme basée sur les styles prédéterminés :

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Divers `NSDateFormatterStyle` valeurs affichent des dates comme suit :

- `NSDateFormatterStyle.Full`: Mercredi août 2, 2017 à 7:48 PM
- `NSDateFormatterStyle.Long`: 2 août 2017 à 7 h 49
- `NSDateFormatterStyle.Medium`: 2 août 2017, 7 h 49
- `NSDateFormatterStyle.Short`: 8/2/17, 7 H 50

> [!NOTE]
> `DateFormat` et `DateStyle` / `TimeStyle` fournissent différentes façons de spécifier le format de date et heure. Le plus récemment définir les propriétés de déterminent que le formateur de date de sortie.

## <a name="related-links"></a>Liens connexes

- [PickerControl (exemple)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
