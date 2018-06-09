---
title: Mode de liaison de Xamarin.Forms
description: Cet article explique comment contrôler le flux d’informations entre source et cible à l’aide d’un mode de liaison, qui est spécifié avec un membre de l’énumération BindingMode. Chaque propriété pouvant être liée a un mode de liaison par défaut, ce qui indique le mode en vigueur lorsque cette propriété est une cible de liaison de données.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 12e6416eee989b0d36a7b9fe0ca4dcd9b18b0ade
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241814"
---
# <a name="xamarinforms-binding-mode"></a>Mode de liaison de Xamarin.Forms

Dans le [article précédent](basic-bindings.md), le **Alternative la liaison du Code** et **Alternative à une liaison XAML** pages proposées un `Label` avec son `Scale` propriété lié à la `Value` propriété d’un `Slider`. Car le `Slider` valeur initiale est 0, cela a provoqué la `Scale` propriété de la `Label` soit définie sur 0, plutôt que 1 et le `Label` a disparu.

Dans le [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) exemple, le **inverser la liaison** page est similaire pour les programmes dans l’article précédent, mais la liaison de données est définie sur le `Slider` plutôt que sur le `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label"
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

Dans un premier temps, cela peut sembler vers l’arrière : désormais le `Label` est la source de liaison de données et le `Slider` est la cible. Les références de liaison le `Opacity` propriété de la `Label`, qui a une valeur par défaut de 1.

Comme vous pouvez l’imaginer, la `Slider` est initialisé à la valeur 1 à partir de la première `Opacity` valeur `Label`. Ceci est illustré dans la capture d’écran de l’e/s sur la gauche :

[![Inverser la liaison](binding-mode-images/reversebinding-small.png "inverser la liaison")](binding-mode-images/reversebinding-large.png#lightbox "inverser la liaison")

Mais vous serez peut-être surpris que le `Slider` continue à fonctionner, comme les captures d’écran Android et UWP montrent. Cela peut paraître qui suggère que la liaison de données fonctionne mieux pour les `Slider` est la cible de liaison plutôt qu’à le `Label` , car l’initialisation fonctionne comme nous pouvons attendre.

La différence entre la **inverser la liaison** exemple et les exemples précédents implique la *mode de liaison*.

## <a name="the-default-binding-mode"></a>Le Mode de liaison par défaut

Le mode de liaison est spécifié avec un membre de la [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) énumération :

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/)
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; données passent les deux sens entre source et cible
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; données passent à partir de la source à la cible
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; données passent à partir de la cible à la source
- [`OneTime`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; données passent à partir de la source à la cible, mais uniquement lorsque le `BindingContext` modifications (nouvelles avec Xamarin.Forms 3.0)

Chaque propriété pouvant être liée a une liaison de mode qui est défini lors de la création de la propriété pouvant être liée par défaut, et qui est disponible à partir de la [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) propriété de la `BindableProperty` objet. Ce mode de liaison par défaut indique le mode en vigueur lorsque cette propriété est une cible de liaison de données.

Le mode de liaison par défaut pour la plupart des propriétés, telles que `Rotation`, `Scale`, et `Opacity` est `OneWay`. Lorsque ces propriétés sont des cibles de liaison de données, la propriété cible a la valeur de la source.

Toutefois, le mode de liaison par défaut pour le `Value` propriété du `Slider` est `TwoWay`. Cela signifie que lorsque le `Value` propriété est une cible de liaison de données, puis de la cible est définie à partir de la source (comme d’habitude), mais la source est également définie à partir de la cible. C’est ce qui permet la `Slider` à définir à partir de la première `Opacity` valeur.

Cette liaison bidirectionnelle peut sembler pour créer une boucle infinie, mais qui ne se produit pas. Propriétés pouvant être liées ne pas signaler une modification de propriété, sauf si la propriété change. Cela empêche une boucle infinie.

### <a name="two-way-bindings"></a>Liaisons bidirectionnelles

Propriétés pouvant être liées plus ont un mode de liaison par défaut de `OneWay` , mais les propriétés suivantes ont un mode de liaison par défaut de `TwoWay`:

- `Date` propriété de `DatePicker`
- `Text` propriété de `Editor`, `Entry`, `SearchBar`, et `EntryCell`
- `IsRefreshing` propriété de `ListView`
- `SelectedItem` propriété de `MultiPage`
- `SelectedIndex` et `SelectedItem` propriétés de `Picker`
- `Value` propriété de `Slider` et `Stepper`
- `IsToggled` propriété de `Switch`
- `On` propriété de `SwitchCell`
- `Time` propriété de `TimePicker`

Ces propriétés sont définies comme `TwoWay` pour une très bonne raison :

Lorsque des liaisons de données sont utilisés avec l’architecture d’application Model-View-ViewModel (MVVM), la classe ViewModel est la source de liaison de données et la vue, qui se compose des vues telles que `Slider`, sont des cibles de liaison de données. Les liaisons MVVM ressembler à la **inverser la liaison** exemple plus que les liaisons dans les exemples précédents. Il est très probable que vous souhaitez que chaque affichage sur la page pour être initialisé avec la valeur de la propriété correspondante dans le modèle de vues, mais les modifications dans la vue doivent également affecter la propriété ViewModel.

Les propriétés avec les modes de liaison par défaut de `TwoWay` sont les plus susceptibles d’être utilisé dans les scénarios MVVM de propriétés.

### <a name="one-way-to-source-bindings"></a>Liaisons unique bidirectionnelle pour Source

Les propriétés pouvant être liées en lecture seule ont un mode de liaison par défaut de `OneWayToSource`. Il n'existe qu’une seule propriété pouvant être liée en lecture/écriture qui comporte un mode de liaison par défaut de `OneWayToSource`:

- `SelectedItem` propriété de `ListView`

Le raisonnement qui est une liaison sur le `SelectedItem` propriété doit avoir pour résultat lors de la définition de la source de liaison. Un exemple plus loin dans cet article, ce comportement.

### <a name="one-time-bindings"></a>Liaisons à usage unique

Plusieurs propriétés ont un mode de liaison par défaut de `OneTime`. Ces équivalents sont :

- `IsTextPredictionEnabled` propriété de `Entry`
- `Text`, `BackgroundColor`, et `Style` propriétés de `Span`.

Cibler les propriétés ayant le mode de liaison `OneTime` sont mis à jour uniquement lorsque le contexte de liaison change. Pour les liaisons sur ces propriétés cibles, cela simplifie l’infrastructure de liaison, car il n’est pas nécessaire de surveiller les modifications dans les propriétés de la source.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModel et les Notifications de modification de propriété

Le **Simple sélecteur de couleurs** page illustre l’utilisation d’un simple ViewModel. Liaisons de données permettent à l’utilisateur de sélectionner une couleur à l’aide de trois `Slider` éléments pour la teinte, saturation et de luminosité.

Le ViewModel est la source de liaison de données. Le ViewModel est *pas* définir les propriétés pouvant être liées, mais il implémente un mécanisme de notification qui permet à l’infrastructure de liaison être averti lorsque la valeur d’une propriété est modifiée. Ce mécanisme de notification est le [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) interface qui définit une propriété unique nommée [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/). Une classe qui implémente cette interface généralement déclenche l’événement lorsque l’une de ses propriétés publiques change de valeur. L’événement n’a pas besoin être déclenché si la propriété ne change jamais. (Le `INotifyPropertyChanged` interface est également implémentée par `BindableObject` et un `PropertyChanged` événement est déclenché chaque fois qu’une propriété pouvant être liée change de valeur.)

Le `HslColorViewModel` classe définit cinq propriétés : le `Hue`, `Saturation`, `Luminosity`, et `Color` propriétés sont reliés entre eux. Lorsque l’un des trois composants de couleur remplace la valeur, le `Color` propriété est recalculée et `PropertyChanged` les événements sont déclenchés pour tous les quatre propriétés :

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

Lorsque le `Color` de propriété est modifiée, la méthode statique `GetNearestColorName` méthode dans le `NamedColor` classe (également inclus dans le **DataBindingDemos** solution) Obtient la couleur nommée le plus proche et définit le `Name` propriété. Cela `Name` propriété est privée `set` accesseur, donc il ne peut pas être défini en dehors de la classe de.

Quand un ViewModel est défini en tant que source de liaison, l’infrastructure de liaison attache un gestionnaire à la `PropertyChanged` événement. De cette façon, la liaison peut être avertie des modifications apportées aux propriétés et peut alors définir les propriétés cibles à partir de valeurs modifiées.

Toutefois, lorsqu’une propriété cible (ou le `Binding` définition sur une propriété cible) a une `BindingMode` de `OneTime`, il n’est pas nécessaire pour l’infrastructure de liaison joindre un gestionnaire dans le `PropertyChanged` événement. La propriété cible est mis à jour uniquement lorsque la `BindingContext` modifications et pas lorsque la propriété de la source est modifié.

Le **Simple sélecteur de couleurs** fichier XAML instancie le `HslColorViewModel` dans le dictionnaire de ressources de la page et l’initialise le `Color` propriété. Le `BindingContext` propriété de la `Grid` est définie sur une `StaticResource` extension fassent référence à cette ressource de liaison :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel"
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />

            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

Le `BoxView`, `Label`et trois `Slider` vues héritent du contexte de liaison à partir de la `Grid`. Ces vues sont toutes les cibles de liaison qui font référence les propriétés de la source dans le modèle de vues. Pour le `Color` propriété de la `BoxView`et le `Text` propriété de la `Label`, les liaisons de données sont `OneWay`: les propriétés dans la vue sont définies à partir des propriétés dans le modèle de vues.

Le `Value` propriété de la `Slider`, toutefois, est `TwoWay`. Cela permet à chaque `Slider` à définir le ViewModel, ainsi que pour le ViewModel à définir à partir de chaque `Slider`.

Lors de l’exécution du programme tout d’abord, le `BoxView`, `Label`et trois `Slider` éléments sont définis à partir de ViewModel en fonction de la première `Color` propriété est définie lorsque ce dernier a été instancié. Ceci est illustré dans la capture d’écran d’iOS situé à gauche :

[![Sélecteur de couleurs simple](binding-mode-images/simplecolorselector-small.png "sélecteur de couleurs Simple")](binding-mode-images/simplecolorselector-large.png#lightbox "sélecteur de couleurs Simple")

Lorsque vous manipulez les curseurs, les `BoxView` et `Label` sont mis à jour en conséquence, comme illustré dans les captures d’écran Android et UWP.

L’instanciation ViewModel dans le dictionnaire de ressources est une approche commune. Il est également possible d’instancier le ViewModel au sein des balises d’élément de propriété pour le `BindingContext` propriété. Dans le **Simple sélecteur de couleurs** XAML file, essayez de supprimer le `HslColorViewModel` à partir du dictionnaire de ressources et affectez-lui la valeur la `BindingContext` propriété de la `Grid` comme suit :

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Le contexte de liaison peut être défini dans plusieurs façons. Parfois, le fichier code-behind instancie le ViewModel et lui affecte le `BindingContext` propriété de la page. Voici les approches possibles.

## <a name="overriding-the-binding-mode"></a>Remplacement du Mode de liaison

Si le mode de liaison par défaut sur la propriété cible n’est pas approprié pour une liaison de données particulier, il est possible de remplacer en définissant le [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) propriété du `Binding` (ou [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) propriété de la `Binding` extension de balisage) à un des membres de la `BindingMode` énumération.

Toutefois, la définition du `Mode` propriété `TwoWay` ne fonctionne toujours pas comme prévu. Par exemple, essayez de modifier le **Alternative XAML liaison** fichier XAML pour inclure `TwoWay` dans la définition de la liaison :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Il se peut que le `Slider` serait initialisée à la valeur initiale de la `Scale` propriété, qui est 1, mais qui ne se produit pas. Lorsqu’un `TwoWay` liaison est initialisée, la cible est définie à partir de la source de tout d’abord, ce qui signifie que le `Scale` est définie sur le `Slider` par défaut la valeur 0. Lorsque le `TwoWay` liaison est définie sur le `Slider`, le `Slider` a initialement la valeur de la source.

Vous pouvez définir le mode de liaison `OneWayToSource` dans les **Alternative XAML liaison** exemple :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Maintenant le `Slider` est initialisé à 1 (la valeur par défaut `Scale`) mais manipulation la `Slider` n’affecte pas le `Scale` propriété, donc il n’est pas très utile.

Une application très utile de remplacer le mode de liaison par défaut avec `TwoWay` implique la `SelectedItem` propriété du `ListView`. Le mode de liaison par défaut est `OneWayToSource`. Lorsqu’une liaison de données est définie sur le `SelectedItem` propriété pour faire référence à une propriété de la source dans un ViewModel, alors cette propriété source est définie à partir de la `ListView` sélection. Toutefois, dans certains cas, vous pourriez également le `ListView` à être initialisées à partir de ce dernier.

Le **paramètres exemple** page illustre cette technique. Cette page représente une implémentation simple de paramètres d’application, qui sont très souvent définis dans un ViewModel, tel que `SampleSettingsViewModel` fichier :

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Chaque paramètre d’application est une propriété qui est enregistrée dans le dictionnaire de propriétés Xamarin.Forms dans une méthode nommée `SaveState` et chargées à partir de ce dictionnaire dans le constructeur. Au bas de la classe sont deux méthodes qui permettent de rationaliser ViewModel et de les rendre moins sujet aux erreurs. Le `OnPropertyChanged` méthode en bas possède un paramètre optionnel qui est défini à la propriété d’appel. Cela évite les fautes d’orthographe lorsque vous spécifiez le nom de la propriété sous forme de chaîne.

Le `SetProperty` méthode dans la classe est encore plus : il compare la valeur est définie avec la valeur stockée en tant que champ à la propriété et appelle uniquement `OnPropertyChanged` lorsque les deux valeurs ne sont pas égales.

Le `SampleSettingsViewModel` classe définit deux propriétés pour la couleur d’arrière-plan : le `BackgroundNamedColor` propriété est de type `NamedColor`, qui est une classe également incluse dans le **DataBindingDemos** solution. Le `BackgroundColor` propriété est de type `Color`et sont obtenues à partir de la `Color` propriété de la `NamedColor` objet.

Le `NamedColor` classe utilise la réflexion .NET pour énumérer tous les champs publics statiques dans le Xamarin.Forms `Color` structure et les stocker avec leurs noms dans une collection accessible à partir de la méthode statique `All` propriété :

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

Le `App` classe dans le **DataBindingDemos** projet définit une propriété nommée `Settings` de type `SampleSettingsViewModel`. Cette propriété est initialisée lorsque la `App` classe est instanciée et le `SaveState` méthode est appelée lorsque le `OnSleep` méthode est appelée :

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Pour plus d’informations sur les méthodes de cycle de vie des applications, consultez l’article [ **cycle de vie**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Presque tout le reste est gérée dans le **SampleSettingsPage.xaml** fichier. Le `BindingContext` de la page est définie à l’aide un `Binding` extension de balisage : la source de liaison est statique `Application.Current` propriété, qui est l’instance de la `App` classe dans le projet et le `Path` est définie sur le `Settings` propriété, qui est le `SampleSettingsViewModel` objet :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Tous les enfants de la page héritent du contexte de liaison. La plupart des autres liaisons sur cette page sont aux propriétés dans `SampleSettingsViewModel`. Le `BackgroundColor` propriété est utilisée pour définir le `BackgroundColor` propriété de la `StackLayout`et le `Entry`, `DatePicker`, `Switch`, et `Stepper` toutes les propriétés sont liées à d’autres propriétés dans le modèle de vues.

Le `ItemsSource` propriété de la `ListView` a la valeur statique `NamedColor.All` propriété. Il remplit la `ListView` avec tous les `NamedColor` instances. Pour chaque élément de la `ListView`, le contexte de l’élément de liaison est défini un `NamedColor` objet. Le `BoxView` et `Label` dans les `ViewCell` liées aux propriétés dans `NamedColor`.

Le `SelectedItem` propriété de la `ListView` est de type `NamedColor`et est lié à la `BackgroundNamedColor` propriété du `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Le mode de liaison par défaut pour `SelectedItem` est `OneWayToSource`, qui définit la propriété ViewModel à partir de l’élément sélectionné. Le `TwoWay` mode permet le `SelectedItem` pour être initialisées à partir de ce dernier.

Toutefois, lorsque le `SelectedItem` a la valeur de cette façon, le `ListView` ne défile pas automatiquement pour afficher l’élément sélectionné. Un peu de code dans le fichier code-behind est nécessaire :

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem,
                                   ScrollToPosition.MakeVisible,
                                   false);
        }
    }
}
```

La capture d’écran d’iOS situé à gauche montre le programme lors de son exécution. Le constructeur de `SampleSettingsViewModel` initialise la couleur d’arrière-plan blanc, et c’est ce qui est sélectionné dans le `ListView`:

[![Paramètres](binding-mode-images/samplesettings-small.png "paramètres")](binding-mode-images/samplesettings-large.png#lightbox "paramètres")

Deux autres captures d’écran affichent les paramètres modifiés. Lorsque vous testez cette page, n’oubliez pas que le programme de mise en veille ou à le résilier sur le périphérique ou l’émulateur qu’elle s’exécute. Arrêt du programme à partir du débogueur Visual Studio n’entraîne pas la `OnSleep` remplacer dans la `App` classe d’être appelée.

Dans l’article suivant, vous verrez comment spécifier [ **mise en forme de chaîne** ](string-formatting.md) des liaisons de données qui sont définies sur le `Text` propriété du `Label`.


## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du carnet de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
