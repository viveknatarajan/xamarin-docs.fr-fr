---
title: Contrôle de sélecteur de Xamarin.iOS
description: Ce document décrit comment concevoir et manipuler les contrôles de sélecteur dans une application Xamarin.iOS. Elle explique comment implémenter un sélecteur dans du code et dans le concepteur iOS.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: 7f46d354af86027d1e2656171c6595562d3555a6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789910"
---
# <a name="picker-control-in-xamarinios"></a>Contrôle de sélecteur de Xamarin.iOS

Le contrôle de sélecteur affiche 'roue-like' contrôle qui contient une liste déroulante de valeurs avec la valeur sélectionnée est en cours de mise en surbrillance. Les utilisateurs tournez la molette pour sélectionner l’option de que leur choix.

Un utilisateur spécifique de cas pour les sélecteurs pour définir la date et / ou l’heure. Prévoir cette Apple a créé une sous-classe personnalisée de la classe UIPickerView appelée UIDatePicker.

L’article couvre la mise en œuvre et à l’aide de la [sélecteur](#picker) et [sélecteur de dates](#datepicker) contrôles.

<a name="picker" />

## <a name="picker"></a>Sélecteur

### <a name="implementing-a-picker"></a>Implémentation d’un sélecteur

Un sélecteur est implémenté en instanciant une nouvelle [`UIPickerView`](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>Les sélecteurs et les animations

Si vous utilisez le concepteur iOS pour créer votre interface utilisateur, le sélecteur peut être ajouté à votre disposition à partir de la boîte à outils :

![](picker-images/image1.png)


### <a name="working-with-picker"></a>Utilisation de sélecteur

Une fois que vous avez créé un sélecteur, que ce soit dans le code ou via des plans conceptuels, vous devrez attribuer une _modèle_ lui afin que vous pouvez passer et interagir avec les données ;

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

Le code suivant montre un exemple de modèle :

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

Tout d’abord, vous devez passer des données pour fournir des options différentes pour un utilisateur à sélectionner. Lorsque possible d’utiliser cette liste autant que possible, ou si vous essayez d’utiliser plusieurs 'appeler' nécessaire (appelé *composants*) :

![Sélecteur de deux composants](picker-images/image3.png)

Pour définir le nombre de composants, vous devez substituer la `GetComponentCount` méthode : 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

La valeur de retour indique le nombre de compose que votre sélecteur aura.

### <a name="customizing-appearance"></a>Personnaliser l’apparence
 
L’apparence de la `UIPickerView` peuvent être personnalisés à l’aide de la `UIPickerView.UIPickerViewAppearance` classe ou en remplaçant le `UIPickerViewModel.GetView` et `UIPickerViewModel.GetRowHeight` méthodes dans le `UIPickerViewModel`.


<a name="datepicker" />

## <a name="date-picker"></a>Sélecteur de dates

### <a name="implementing-a-date-picker"></a>Implémentation d’un sélecteur de dates

Un sélecteur de dates est implémenté en instanciant une nouvelle [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>Sélecteurs de date et d’animations

Si vous utilisez le concepteur iOS pour créer votre interface utilisateur, le **sélecteur de dates** peuvent être ajoutés à votre disposition à partir de la boîte à outils. Les propriétés suivantes peuvent être ajustées à partir de la zone de propriétés :

![](picker-images/image2.png)

* **Mode** : mode de la Date et l’heure. Cela peut être date, heure, date et heure ou un compte à rebours. 
* **Paramètres régionaux** : les paramètres régionaux du sélecteur de date. Choisissez **par défaut** pour définir la valeur par défaut du système ou à des paramètres régionaux spécifique.
* **Intervalle** – affiche l’incrément dans lequel les options de l’horloge seront affichera.
* **Date, Date de Minimum, Date maximale** – définit la première date affiche le sélecteur et les contraintes pour les dates sélectionnables.

### <a name="configuring-the-datepicker"></a>Configuration du sélecteur

Vous pouvez limiter la plage de dates que l’utilisateur peut sélectionner à partir d’à l’aide de la `MinimumDate` et `MaximumDate` propriétés. L’extrait de code suivant montre un exemple de définition de la plage comprise entre 60 ans et aujourd'hui :

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

Vous pouvez également utiliser les contrôles .NET pour définir la plage de dates minimales et maximales. Exemple :

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

Vous pouvez également définir le `MinuteInterval` propriété pour définir l’intervalle auquel le sélecteur affiche les minutes. L’extrait de code suivant peut être utilisé pour définir le sélecteur de minutes à définir à des intervalles de 10.

```csharp
datePickerView.MinuteInterval = 10;
```

Il existe quatre modes le sélecteur de dates peut être défini l’aide de la [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) propriété. La liste ci-dessous présente un exemple de chacune d’elles et comment l’implémenter :

#### <a name="time"></a>réflexion

Le mode de temps affiche l’heure avec un sélecteur d’heure et une désignation AM ou PM facultatif. Il est défini avec la `UIDatePickerMode.Time` propriété. Exemple :

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

L’image suivante illustre un exemple de ce mode de sélecteur de dates :

![](picker-images/image8.png)



#### <a name="date"></a>Date

Le mode de date affiche la date avec un mois, le jour et la sélection de l’année. Il est défini avec la `UIDatePickerMode.Date` propriété. Exemple :

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

L’image suivante illustre un exemple de ce sélecteur de dates :

![](picker-images/image7.png)

L’ordre des sélecteurs varie selon les paramètres régionaux de la la `UIDatePicker`. Par défaut, il est défini par défaut du système. L’image ci-dessus illustre la disposition des sélecteurs dans le `en_US` paramètres régionaux, mais pour modifier un jour | Mois | Mise en page de l’année, vous pouvez utiliser le code suivant pour définir les paramètres régionaux :

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>Date et heure

Le mode de date et d’heure affiche une vue de shortend de la date, l’heure en heures et minutes et un dependings de désignation AM ou PM facultatif sur si une horloge de 12 ou 24 heures est utilisé. Il est défini avec la `UIDatePickerMode.DateAndTime` propriété. Exemple :

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

L’image suivante illustre un exemple de ce sélecteur de dates :

![](picker-images/image6.png)

Comme avec [Date](#Date), l’ordre des sélecteurs et l’utilisation de l’horloge au format 12 ou 24 heures varient selon les paramètres régionaux de la la `UIDatePicker`.

#### <a name="countdown-timer"></a>Compte à rebours

Le mode de minuteur du compte à rebours affiche l’heure et la valeur des minutes. Il est défini avec la `UIDatePickerMode.CountDownTimer` propriété. Exemple :

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

L’image suivante illustre un exemple de ce sélecteur de dates :

![](picker-images/image5.png)

Vous pouvez utiliser le `CountDownDuration` propriété pour capturer la valeur dispayed par le sélecteur de dates du compte à rebours. Par exemple, pour ajouter la valeur du compte à rebours à la date actuelle, vous pouvez utiliser le code suivant :

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>Mise en forme 

Les valeurs des modes DateAndTime, Date et l’heure peuvent être capturés à l’aide de la `Date` propriété sur le UIDatePicker (par exemple : `datePickerView.Date`), qui est de type NSDate. Pour mettre en forme cette date en quelque chose plus lisible, utilisez [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/). Les exemples ci-dessous montrent comment utiliser certaines des propriétés disponibles sur cette classe.

Le `DateFormat` a la valeur sous forme de chaîne pour représenter le mode d’affichage de la date :

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

Le `TimeStyle` jeux de propriétés un `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Les champs de `NSDateFormatterStyle` afficher comme suit :

![](picker-images/timestyle.png)

Le `DateStyle` jeux de propriétés un `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Les champs de `NSDateFormatterStyle` afficher comme suit :

![](picker-images/datestyle.png)

Puis de sortie le NSDate mis en forme en une chaîne en utilisant le code suivant :

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>Liens associés

- [PickerControl (exemple)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
