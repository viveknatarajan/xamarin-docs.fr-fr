---
title: Mode de liaison Xamarin.Forms
description: Cet article explique comment contrôler le flux d’informations entre la source et cible à l’aide d’un mode de liaison, qui est spécifié avec un membre de l’énumération BindingMode. Chaque propriété pouvant être liée a un mode de liaison par défaut, ce qui indique le mode en vigueur lorsque cette propriété est une cible de liaison de données.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: a6eaf08d17f70c43f451361e27555a09c39f26a9
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935665"
---
# <a name="xamarinforms-binding-mode"></a>Mode de liaison Xamarin.Forms

Dans le [article précédent](basic-bindings.md), le **Alternative Code liaison** et **Alternative de liaison XAML** pages proposées un `Label` avec son `Scale` propriété lié à la `Value` propriété d’un `Slider`. Étant donné que le `Slider` valeur initiale est 0, cela a provoqué la `Scale` propriété de la `Label` être définie sur 0, plutôt que 1 et le `Label` a disparu.

Dans le [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) exemple, le **liaison inverse** page est similaire pour les programmes dans l’article précédent, sauf que la liaison de données est définie sur le `Slider` plutôt que sur le `Label`:

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

Dans un premier temps, cela peut sembler vers l’arrière : maintenant le `Label` est la source de liaison de données et le `Slider` est la cible. Les références de liaison la `Opacity` propriété de la `Label`, qui a la valeur par défaut de 1.

Comme vous pouvez l’imaginer, la `Slider` est initialisé à la valeur 1 à partir de la première `Opacity` valeur `Label`. Ceci est illustré dans la capture d’écran d’iOS sur la gauche :

[![Inverser la liaison](binding-mode-images/reversebinding-small.png "inverse liaison")](binding-mode-images/reversebinding-large.png#lightbox "inverse de liaison")

Mais vous serez peut-être surpris que le `Slider` continue de fonctionner, comme le montrent les captures d’écran Android et UWP. Cela semble suggérer que la liaison de données fonctionne mieux pour le `Slider` est la cible de liaison plutôt que `Label` , car l’initialisation fonctionne comme on pouvait s’attendre.

La différence entre la **liaison inverse** exemple et les exemples précédents implique la *mode de liaison*.

## <a name="the-default-binding-mode"></a>Le Mode de liaison par défaut

Le mode de liaison est spécifié avec un membre de la [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) énumération :

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; données vont les deux sens entre source et cible
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; données passant à partir de la source à la cible
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; données accède à partir de la cible à la source
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; données accède à partir de la source à la cible, mais uniquement lorsque le `BindingContext` modifications (nouveau avec Xamarin.Forms 3.0)

Chaque propriété pouvant être liée a une valeur par défaut de mode de liaison qui est défini lorsque la propriété peut être liée est créée, et qui est disponible à partir de la [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) propriété de la `BindableProperty` objet. Ce mode de liaison par défaut indique le mode en vigueur lorsque cette propriété est une cible de liaison de données.

Le mode de liaison par défaut pour la plupart des propriétés, telles que `Rotation`, `Scale`, et `Opacity` est `OneWay`. Lorsque ces propriétés sont des cibles de liaison de données, la propriété cible est définie à partir de la source.

Toutefois, le mode de liaison par défaut pour le `Value` propriété du `Slider` est `TwoWay`. Cela signifie que lorsque le `Value` propriété est une cible de liaison de données, puis la cible est définie à partir de la source (comme d’habitude), mais la source est également définie à partir de la cible. C’est ce qui permet la `Slider` soit définie à partir de la première `Opacity` valeur.

Cette liaison bidirectionnelle peut sembler pour créer une boucle infinie, mais qui ne se produit. Propriétés pouvant être liées ne pas signaler une modification de propriété, sauf si la propriété change. Cela empêche une boucle infinie.

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

Ces propriétés particulières sont définies comme `TwoWay` pour une très bonne raison :

Lorsque des liaisons de données sont utilisés avec l’architecture d’application Model-View-ViewModel (MVVM), la classe ViewModel est la source de liaison de données et la vue, qui se compose des vues telles que `Slider`, sont des cibles de liaison de données. Liaisons de MVVM ressemblent à la **inverse liaison** exemple plus que les liaisons dans les exemples précédents. Il est très probable que vous souhaitez que chaque affichage de la page pour être initialisé avec la valeur de la propriété correspondante dans le ViewModel, mais les modifications dans la vue doivent aussi affecter la propriété ViewModel.

Les propriétés avec les modes de liaison par défaut de `TwoWay` sont celles susceptibles d’être utilisée dans les scénarios MVVM.

### <a name="one-way-to-source-bindings"></a>Liaisons une-Way-to-Source

Les propriétés pouvant être liées en lecture seule ont un mode de liaison par défaut de `OneWayToSource`. Il n'existe qu’une seule propriété peut être liée en lecture/écriture qui dispose d’un mode de liaison par défaut de `OneWayToSource`:

- `SelectedItem` propriété de `ListView`

Le raisonnement qui est une liaison sur le `SelectedItem` propriété doit aboutir à la définition de la source de liaison. Un exemple plus loin dans cet article remplace ce comportement.

### <a name="one-time-bindings"></a>Liaisons à usage unique

Plusieurs propriétés ont un mode de liaison par défaut de `OneTime`. Ces équivalents sont :

- `IsTextPredictionEnabled` propriété de `Entry`
- `Text`, `BackgroundColor`, et `Style` propriétés de `Span`.

Cibler des propriétés avec un mode de liaison de `OneTime` sont mis à jour uniquement lorsque le contexte de liaison change. Pour les liaisons sur ces propriétés cibles, cela simplifie l’infrastructure de liaison, car il n’est pas nécessaire de surveiller les modifications de propriétés de la source.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels et les Notifications de modification de propriété

Le **sélecteur de couleurs Simple** page illustre l’utilisation d’un ViewModel simple. Liaisons de données permettent à l’utilisateur de sélectionner une couleur à l’aide de trois `Slider` éléments pour la teinte, de saturation et de luminosité.

Le ViewModel est la source de liaison de données. Le ViewModel est *pas* définir les propriétés pouvant être liées, mais il implémente un mécanisme de notification qui permet à l’infrastructure de liaison d’être averti lorsque la valeur d’une propriété change. Ce mécanisme de notification est le [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) interface qui définit une propriété unique nommée [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged). Une classe qui implémente cette interface généralement déclenche l’événement lorsque l’une de ses propriétés publiques change de valeur. L’événement n’a pas besoin être déclenché si la propriété ne change jamais. (Le `INotifyPropertyChanged` interface est également implémentée par `BindableObject` et un `PropertyChanged` événement est déclenché chaque fois qu’une propriété peut être liée change de valeur.)

Le `HslColorViewModel` classe définit cinq propriétés : le `Hue`, `Saturation`, `Luminosity`, et `Color` propriétés sont liées entre elles. Lorsque l’un des trois composants de couleur remplace la valeur, le `Color` propriété est recalculée, et `PropertyChanged` les événements sont déclenchés pour toutes les quatre propriétés :

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

Lorsque le `Color` les modifications de propriété, la méthode statique `GetNearestColorName` méthode dans le `NamedColor` classe (également inclus dans le **DataBindingDemos** solution) Obtient la couleur nommée le plus proche et définit le `Name` propriété. Cela `Name` propriété a une privée `set` accesseur, donc elle ne peut pas être définie en dehors de la classe à partir de.

En cas d’un ViewModel est défini comme une source de liaison, l’infrastructure de liaison attache un gestionnaire à la `PropertyChanged` événement. De cette façon, la liaison peut être avertie des modifications apportées aux propriétés et pouvez puis définissez les propriétés de la cible à partir des valeurs modifiées.

Toutefois, lorsqu’une propriété cible (ou le `Binding` définition sur une propriété cible) a un `BindingMode` de `OneTime`, il n’est pas nécessaire pour l’infrastructure de liaison attacher un gestionnaire sur le `PropertyChanged` événement. La propriété cible est mis à jour uniquement lorsque la `BindingContext` modifications et pas lorsque la propriété source proprement dite est modifié.

Le **sélecteur de couleurs Simple** XAML fichier instancie le `HslColorViewModel` dans le dictionnaire de ressources de la page et l’initialise le `Color` propriété. Le `BindingContext` propriété de la `Grid` est définie sur une `StaticResource` binding, extension de faire référence à cette ressource :

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

Le `BoxView`, `Label`et trois `Slider` vues héritent du contexte de liaison à partir de la `Grid`. Ces vues sont toutes les cibles de liaison qui font référence à des propriétés de source dans le ViewModel. Pour le `Color` propriété de la `BoxView`et le `Text` propriété de la `Label`, les liaisons de données sont `OneWay`: les propriétés dans la vue sont définies à partir des propriétés dans le ViewModel.

Le `Value` propriété de la `Slider`, cependant, est `TwoWay`. Cela permet à chaque `Slider` à définir à partir du ViewModel, ainsi que pour le ViewModel à définir à partir de chaque `Slider`.

Lorsque le programme est exécuté tout d’abord, le `BoxView`, `Label`et trois `Slider` éléments ont tous la valeur à partir du ViewModel en fonction de la première `Color` propriété est définie lorsque le ViewModel a été instancié. Ceci est illustré dans la capture d’écran d’iOS à gauche :

[![Sélecteur de couleurs simple](binding-mode-images/simplecolorselector-small.png "sélecteur de couleurs Simple")](binding-mode-images/simplecolorselector-large.png#lightbox "sélecteur de couleurs Simple")

Lorsque vous manipulez les curseurs, les `BoxView` et `Label` sont mis à jour en conséquence, comme illustré par les captures d’écran Android et UWP.

Instanciation du ViewModel dans le dictionnaire de ressources est une approche courante. Il est également possible d’instancier le ViewModel au sein des balises d’élément de propriété pour le `BindingContext` propriété. Dans le **sélecteur de couleurs Simple** XAML de fichier, essayez de supprimer le `HslColorViewModel` à partir du dictionnaire de ressources et affectez-lui la valeur la `BindingContext` propriété de la `Grid` comme suit :

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Le contexte de liaison peut être défini dans un de différentes façons. Parfois, le fichier code-behind instancie le ViewModel et lui affecte le `BindingContext` propriété de la page. Il s’agit des approches tous valides.

## <a name="overriding-the-binding-mode"></a>Remplacement du Mode de liaison

Si le mode de liaison par défaut sur la propriété cible n’est pas approprié pour une liaison de données particulier, il est possible de remplacer en définissant le [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) propriété du `Binding` (ou le [ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) propriété de la `Binding` extension de balisage) à un des membres de la `BindingMode` énumération.

Cependant, la définition de la `Mode` propriété `TwoWay` ne fonctionne toujours pas comme prévu. Par exemple, essayez de modifier le **Alternative XAML liaison** fichier XAML pour inclure `TwoWay` dans la définition de liaison :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Il peut s’attendre à qui le `Slider` serait initialisée à la valeur initiale de la `Scale` propriété, qui est 1, mais qui ne se produit. Quand un `TwoWay` liaison est initialisée, la cible est définie à partir de la source de tout d’abord, ce qui signifie que le `Scale` propriété est définie sur la `Slider` valeur 0 par défaut. Lorsque le `TwoWay` liaison est définie sur le `Slider`, le `Slider` est initialement défini à partir de la source.

Vous pouvez définir le mode de liaison sur `OneWayToSource` dans le **Alternative XAML liaison** exemple :

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Maintenant le `Slider` est initialisé à 1 (la valeur par défaut `Scale`) mais manipuler le `Slider` n’affecte pas le `Scale` propriété, donc il n’est pas très utile.

Une application très utile de substituer le mode de liaison par défaut avec `TwoWay` implique la `SelectedItem` propriété du `ListView`. Le mode de liaison par défaut est `OneWayToSource`. Quand une liaison de données est définie sur le `SelectedItem` propriété à référencer une propriété de source dans un ViewModel, cette propriété source est définie à partir de la `ListView` sélection. Toutefois, dans certains cas, vous pourriez également le `ListView` à initialiser à partir du ViewModel.

Le **exemples de paramètres** page illustre cette technique. Cette page représente une implémentation simple de paramètres d’application, qui sont très souvent définies dans un ViewModel, telle que celle-ci `SampleSettingsViewModel` fichier :

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

Chaque paramètre d’application est une propriété qui est enregistrée dans le dictionnaire de propriétés de Xamarin.Forms dans une méthode nommée `SaveState` et chargé à partir de ce dictionnaire dans le constructeur. Au bas de la classe sont deux méthodes qui permettent de rationaliser les ViewModels et de les rendre moins sujet aux erreurs. Le `OnPropertyChanged` méthode en bas possède un paramètre facultatif qui est défini sur la propriété appelante. Cela permet d’éviter les fautes d’orthographe lorsque vous spécifiez le nom de la propriété sous forme de chaîne.

Le `SetProperty` fait de méthode dans la classe encore plus importantes : elle compare la valeur est définie avec la valeur stockée en tant que champ à la propriété et appelle uniquement `OnPropertyChanged` lorsque les deux valeurs ne sont pas identiques.

Le `SampleSettingsViewModel` classe définit deux propriétés pour la couleur d’arrière-plan : le `BackgroundNamedColor` propriété est de type `NamedColor`, qui est une classe également incluse dans le **DataBindingDemos** solution. Le `BackgroundColor` propriété est de type `Color`et est obtenu à partir de la `Color` propriété de la `NamedColor` objet.

Le `NamedColor` classe utilise la réflexion .NET pour énumérer tous les champs publics statiques dans Xamarin.Forms `Color` structure et les stocker avec leurs noms dans une collection accessible à partir de la méthode statique `All` propriété :

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

Pour plus d’informations sur les méthodes de cycle de vie d’application, consultez l’article [ **cycle de vie**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Presque tout le reste est géré dans le **SampleSettingsPage.xaml** fichier. Le `BindingContext` de la page est définie à l’aide un `Binding` extension de balisage : la source de liaison est statique `Application.Current` propriété, qui est l’instance de la `App` classe dans le projet et le `Path` est défini sur le `Settings` propriété, qui est le `SampleSettingsViewModel` objet :

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

Tous les enfants de la page héritent du contexte de liaison. La plupart des autres liaisons sur cette page sont aux propriétés dans `SampleSettingsViewModel`. Le `BackgroundColor` propriété est utilisée pour définir le `BackgroundColor` propriété de la `StackLayout`et le `Entry`, `DatePicker`, `Switch`, et `Stepper` toutes les propriétés sont liées à d’autres propriétés dans le ViewModel.

Le `ItemsSource` propriété de la `ListView` est défini sur la méthode statique `NamedColor.All` propriété. Cela remplit la `ListView` avec tous les `NamedColor` instances. Pour chaque élément dans le `ListView`, le contexte de l’élément de liaison est défini un `NamedColor` objet. Le `BoxView` et `Label` dans le `ViewCell` sont liés aux propriétés dans `NamedColor`.

Le `SelectedItem` propriété de la `ListView` est de type `NamedColor`et est lié à la `BackgroundNamedColor` propriété du `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Le mode de liaison par défaut pour `SelectedItem` est `OneWayToSource`, qui définit la propriété ViewModel à partir de l’élément sélectionné. Le `TwoWay` mode permet la `SelectedItem` à initialiser à partir du ViewModel.

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

La capture d’écran d’iOS à gauche montre le programme lors de sa première exécution. Le constructeur dans `SampleSettingsViewModel` initialise la couleur d’arrière-plan sur blanc, et c’est ce qui est sélectionné dans le `ListView`:

[![Exemples de paramètres](binding-mode-images/samplesettings-small.png "paramètres")](binding-mode-images/samplesettings-large.png#lightbox "exemples de paramètres")

Deux autres captures d’écran affichent les paramètres modifiés. Lorsque vous testez cette page, n’oubliez pas que le programme de mise en veille ou pour mettre fin sur le périphérique ou un émulateur qu’elle s’exécute. Arrêt du programme à partir du débogueur Visual Studio n’entraîne pas la `OnSleep` remplacer dans la `App` classe à appeler.

Dans le prochain article, vous verrez comment spécifier [ **mise en forme de chaîne** ](string-formatting.md) des liaisons de données qui sont définies sur le `Text` propriété du `Label`.


## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du livre de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
