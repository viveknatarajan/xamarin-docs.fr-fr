---
title: À l’aide du curseur
description: Utilisez un curseur pour la sélection d’une plage de valeurs continues.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: 99109f6377037ffb9f622b7ddb237b42d241e505
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="using-slider"></a>À l’aide du curseur

_Utilisez un curseur pour la sélection d’une plage de valeurs continues._

Le Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) est une barre horizontale qui peut être manipulée par l’utilisateur de sélectionner un `double` valeur d’une plage continue. 

Le `Slider` définit trois propriétés de type `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) est la valeur minimale de la plage, avec 0 comme valeur par défaut.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) est la valeur maximale de la plage, avec une valeur par défaut de 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) est la valeur du curseur, qui peut être comprise entre `Minimum` et `Maximum` et a la valeur par défaut 0.

Les trois propriétés sont soutenues par `BindableProperty` objets. Le `Value` propriété a un mode de liaison par défaut de `BindingMode.TwoWay`, ce qui signifie qu’il est adapté en tant que source de liaison dans une application qui utilise le [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architecture. 

> [!WARNING]
> En interne, le `Slider` garantit que `Minimum` est inférieure à `Maximum`. Si `Minimum` ou `Maximum` sont déjà définies afin que `Minimum` est au moins `Maximum`, une exception est levée. Consultez le [ **précautions** ](#precautions) section ci-dessous pour plus d’informations sur la configuration de la `Minimum` et `Maximum` propriétés.

Le `Slider` force le `Value` propriété afin qu’elle est comprise entre `Minimum` et `Maximum`(inclus). Si le `Minimum` propriété est définie sur une valeur supérieure à la `Value` propriété, le `Slider` définit le `Value` propriété `Minimum`. De même, si `Maximum` est défini sur une valeur inférieure à `Value`, puis `Slider` définit le `Value` propriété `Maximum`. 

`Slider` définit un [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) événement est déclenché lorsque la `Value` change, que ce soit via la manipulation d’utilisateur de la `Slider` ou lorsque le programme définit le `Value` propriété directement. A `ValueChanged` événement se déclenche également lorsque la `Value` propriété est convertie comme décrit dans le paragraphe précédent. 

Le [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) objet qui accompagne le `ValueChanged` événement a deux propriétés, les deux de type `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) et [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). Au moment où l’événement est déclenché, la valeur de `NewValue` est le même que le `Value` propriété de la `Slider` objet.

> [!WARNING]
> N’utilisez pas les options de disposition horizontale sans contrainte de `Center`, `Start`, ou `End` avec `Slider`. Sur Android et la plateforme Windows universelle, le `Slider` est réduite à une barre de longueur nulle et sur iOS, la barre est courte. Conservez la valeur par défaut `HorizontalOptions` paramètre `Fill`et n’utilisez pas une largeur de `Auto` lors du placement de `Slider` dans un `Grid` mise en page.

## <a name="basic-slider-code-and-markup"></a>Balisage et code de curseur de base

Le [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) exemple commence par trois pages qui sont fonctionnellement identique, mais sont implémentées de différentes façons. La première page utilise uniquement du code c#, la seconde utilise XAML avec un gestionnaire d’événements dans le code et le troisième est possible d’éviter le Gestionnaire d’événements à l’aide de la liaison de données dans le fichier XAML.

### <a name="creating-a-slider-in-code"></a>Création d’un curseur dans le code

Le **Code de curseur base** page dans le [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) montre de manière à créer un `Slider` et deux `Label` objets dans le code :

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

Le `Slider` est initialisé pour avoir un `Maximum` propriété 360. Le `ValueChanged` Gestionnaire de la `Slider` utilise le `Value` propriété de la `slider` objet pour définir la `Rotation` propriété du premier `Label` et utilise le `String.Format` méthode avec le `NewValue` propriété de la arguments d’événement pour définir le `Text` propriété du deuxième `Label`. Ces deux approches pour obtenir la valeur actuelle de la `Slider` sont interchangeables. 

Voici le programme en cours d’exécution sur iOS, Android et plateforme Windows universelle (UWP) des appareils :

[![Code de base de curseur](slider-images/BasicSliderCode.png "Code de base de curseur")](slider-images/BasicSliderCode-Large.png#lightbox)

La seconde `Label` affiche le texte « (non initialisé) » jusqu'à ce que le `Slider` est manipulé, quelles circonstances le premier `ValueChanged` événement à déclencher. Notez que le nombre de décimales affichées est différent pour les trois plateformes. Ces différences sont liées aux implémentations de plateforme de la `Slider` et sont décrits plus loin dans cet article dans la section [les différences de plateformes implémentation](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Création d’un curseur en XAML

Le **base curseur XAML** page est fonctionnellement identique **Code de curseur base** mais implémentée principalement dans XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel" 
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Le fichier code-behind contient le gestionnaire pour le `ValueChanged` événement :

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Il est également possible pour le Gestionnaire d’événements obtenir le `Slider` qui déclenche l’événement via le `sender` argument. Le `Value` propriété contient la valeur actuelle :

```csharp
double value = ((Slider)sender).Value;
```

Si le `Slider` objet ont été attribuées à un nom dans le fichier XAML avec une `x:Name` attribut (par exemple, « curseur »), puis le Gestionnaire d’événements peut faire référence à cet objet directement :

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Le curseur de liaison de données

Le **base curseur liaisons** montre comment écrire un programme presque équivalent qui élimine le `Value` Gestionnaire d’événements à l’aide de [une liaison de données](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider}, 
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider}, 
                              Path=Value, 
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Le `Rotation` propriété du premier `Label` est lié à la `Value` propriété de la `Slider`, que le `Text` propriété du deuxième `Label` avec un `StringFormat` spécification. Le **base curseur liaisons** page fonctions un peu différemment dans les deux pages précédentes : lorsque la page s’affiche tout d’abord, le second `Label` affiche la chaîne de texte avec la valeur. Il s’agit d’un avantage de l’utilisation de la liaison de données. Pour afficher du texte sans liaison de données, vous devez initialiser spécifiquement le `Text` propriété de la `Label` ou simuler le déclenchement de le `ValueChanged` événement en appelant le Gestionnaire d’événements à partir du constructeur de classe.

<a name="precautions" />

## <a name="precautions"></a>Précautions

La valeur de la `Minimum` propriété doit toujours être inférieure à la valeur de la `Maximum` propriété. Le code suivant extrait les causes du `Slider` pour lever une exception :

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Le compilateur c# génère du code qui définit ces deux propriétés dans l’ordre, et à quel moment le `Minimum` est définie sur 10, il est supérieur à la valeur par défaut `Maximum` la valeur 1. Vous pouvez éviter l’exception dans ce cas en définissant le `Maximum` propriété premier :

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Paramètre `Maximum` 20 n’est pas un problème, car elle est supérieure à la valeur par défaut `Minimum` paramètre 0. Lorsque `Minimum` est défini, la valeur est inférieure à la `Maximum` valeur de 20.

Le même problème se produit en XAML. Définissez les propriétés dans un ordre qui garantit que `Maximum` est toujours supérieure à `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Vous pouvez définir le `Minimum` et `Maximum` des valeurs pour les nombres négatifs, mais uniquement dans l’ordre où `Minimum` est toujours inférieure à `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

Le `Value` propriété est toujours supérieur ou égal à la `Minimum` valeur et inférieure ou égale à `Maximum`. Si `Value` est définie à une valeur en dehors de cette plage, la valeur est convertie pour être comprises dans la plage, mais aucune exception n’est levée. Par exemple, ce code vous sera *pas* lever une exception :

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Au lieu de cela, le `Value` propriété est convertie en la `Maximum` la valeur 1.

Voici un extrait de code ci-dessus : 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Lorsque `Minimum` est définie sur 10, puis `Value` est également défini sur 10. 

Si un `ValueChanged` Gestionnaire d’événements a été attaché au moment qui le `Value` propriété est convertie en un élément autre que sa valeur par défaut de 0, puis un `ValueChanged` événement est déclenché. Voici un extrait de code XAML : 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Lorsque `Minimum` est défini sur 10, `Value` est également défini sur 10 et le `ValueChanged` événement est déclenché. Cela peut se produire avant le reste de la page a été construit, le gestionnaire peut tenter de référencer d’autres éléments sur la page qui n’ont pas encore été créés. Vous souhaiterez peut-être ajouter du code pour le `ValueChanged` gestionnaire qui recherche les `null` les valeurs des autres éléments sur la page. Ou bien, vous pouvez définir le `ValueChanged` Gestionnaire d’événements après le `Slider` valeurs aient été initialisés.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Différences d’implémentation de plate-forme

Les captures d’écran ci-dessus affichent la valeur de la `Slider` avec un nombre différent de décimales. Cela est lié à la manière dont le `Slider` est implémentée sur les plateformes Android et UWP.

### <a name="the-android-implementation"></a>L’implémentation Android 

L’implémentation Android de `Slider` est basée sur le Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) et définit toujours la [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) propriété à 1000. Cela signifie que le `Slider` sur Android a des valeurs discrètes uniquement 1 001. Si vous définissez la `Slider` d’avoir un `Minimum` de 0 et un `Maximum` de 5 000 lignes, puis en tant que le `Slider` est manipulé, le `Value` propriété a les valeurs 0, 5, 10, 15 et ainsi de suite. 

### <a name="the-uwp-implementation"></a>L’implémentation de la plateforme Windows universelle

L’implémentation de la plateforme Windows universelle de `Slider` est basée sur la plateforme Windows universelle [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) contrôle. Le `StepFrequency` propriété de la plateforme Windows universelle `Slider` est définie sur la différence entre la `Maximum` et `Minimum` propriétés divisé par 10, mais pas supérieure à 1. 

Par exemple, pour la plage par défaut de 0 à 1, le `StepFrequency` est définie sur 0,1. Comme le `Slider` est manipulé, le `Value` propriété est limitée à 0, 0.1, 0,2, 0,3, 0,4, 0,5, 0,6, 0,7, 0,8, 0,9 et 1.0. (Ceci est évident dans la dernière page de la [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) exemple.) Lorsque la différence entre la `Maximum` et `Minimum` propriétés est 10 ou version ultérieure, puis `StepFrequency` est définie sur 1 et le `Value` propriété a des valeurs intégrales. 

### <a name="the-stepslider-solution"></a>La solution StepSlider

Plus polyvalente `StepSlider` est décrite dans [chapitre 27. Les convertisseurs personnalisés](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) du livre *créer des applications mobiles avec Xamarin.Forms*. Le `StepSlider` est similaire à `Slider` mais ajoute un `Steps` propriété pour spécifier le nombre de valeurs comprises entre `Minimum` et `Maximum`.

## <a name="sliders-for-color-selection"></a>Curseurs de sélection de couleur

La dernière deux pages dans le [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) exemple utilisent trois `Slider` instances de sélection de couleur. La première page gère toutes les interactions dans le fichier code-behind, tandis que la deuxième page montre comment utiliser la liaison de données avec un ViewModel. 

### <a name="handling-sliders-in-the-code-behind-file"></a>La gestion des curseurs dans le fichier code-behind 

Le **curseurs de couleur RVB** page instancie un `BoxView` pour afficher une couleur, trois `Slider` pour sélectionner les composants rouges, verts et bleus de la couleur et les trois instances `Label` éléments pour afficher les couleurs valeurs :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>
            
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

A `Style` donne les trois `Slider` éléments une plage de 0 à 255. Le `Slider` éléments partagent le même `ValueChanged` gestionnaire, qui est implémenté dans le fichier code-behind :

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

Les premiers jeux de section le `Text` propriété de l’un de la `Label` instances à une chaîne de texte courte qui indique la valeur de la `Slider` en hexadécimal. Ensuite, les trois `Slider` instances sont accessibles pour créer un `Color` valeur à partir des composants RVB :

[![Les curseurs de couleur RVB](slider-images/RgbColorSliders.png "curseurs de couleur RVB")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Le curseur de liaison à un ViewModel

Le **curseurs de couleurs TSL** page montre comment utiliser un ViewModel pour effectuer les calculs utilisés pour créer un `Color` valeur à partir des valeurs de teinte, saturation et de luminosité. Comme tous les ViewModel, le `HSLColorViewModel` la classe implémente le `INotifyPropertyChanged` interface et se déclenche un `PropertyChanged` événement chaque fois que l’une des propriétés change :

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

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
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModel et `INotifyPropertyChanged` interface sont décrites dans l’article [une liaison de données](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Le **HslColorSlidersPage.xaml** fichier instancie le `HslColorViewModel` et lui affecte la valeur de la page `BindingContext` propriété. Cela permet à tous les éléments dans le fichier XAML à lier aux propriétés dans le modèle de vues :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage> 
```

Comme le `Slider` éléments sont manipulés, le `BoxView` et `Label` éléments sont mis à jour à partir de ce dernier :

[![Les curseurs de couleur TSL](slider-images/HslColorSliders.png "curseurs de couleur TSL")](slider-images/HslColorSliders-Large.png#lightbox)

Le `StringFormat` composant de la `Binding` extension de balisage est définie pour un format de « F2 » pour afficher deux décimales. (Chaîne de mise en forme dans les liaisons de données est présentée dans l’article [mise en forme de chaîne](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Toutefois, la version de la plateforme Windows universelle du programme est limitée aux valeurs de 0, 0.1, 0,2... 0,9 et 1,0. Il s’agit d’un résultat direct de l’implémentation de la plateforme Windows universelle `Slider` comme décrit ci-dessus dans la section [les différences de plateformes implémentation](#implementations).

## <a name="related-links"></a>Liens associés

- [Exemple de démonstrations de curseur](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Curseur API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
