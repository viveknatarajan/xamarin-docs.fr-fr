---
title: Utilisation des Extensions de balisage XAML
description: Utiliser les Extensions de balisage XAML disponibles dans Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 0c68d2cfd43eb535fe2e02b5662250cb6e453ddd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="consuming-xaml-markup-extensions"></a>Utilisation des Extensions de balisage XAML

Extensions de balisage XAML améliorer la puissance et la flexibilité du code XAML en permettant aux attributs de l’élément à définir à partir de diverses sources. Plusieurs extensions de balisage XAML font partie de la spécification de XAML 2009. Ceux-ci s’affichent dans les fichiers XAML avec l’usage `x` préfixe d’espace de noms et sont communément avec ce préfixe. Ceux-ci sont décrits dans les sections ci-dessous :

- [`x:Static`](#static) &ndash; référencer des membres de l’énumération, des champs ou propriétés statiques.
- [`x:Reference`](#reference) &ndash; référence nommée d’éléments sur la page.
- [`x:Type`](#type) &ndash; définir un attribut un `System.Type` objet.
- [`x:Array`](#array) &ndash; créer un tableau d’objets d’un type particulier.
- [`x:Null`](#null) &ndash; définir un attribut un `null` valeur.

Trois autres extensions de balisage XAML ont toujours été pris en charge par d’autres implémentations XAML et sont également prises en charge par Xamarin.Forms. Ceux-ci sont décrits plus en détail dans d’autres articles :

- `StaticResource` &ndash; référencer des objets à partir d’un dictionnaire de ressources, comme décrit dans l’article [ **des dictionnaires de ressources**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; répondre aux modifications apportées aux objets dans un dictionnaire de ressources, comme décrit dans l’article [ **Styles dynamiques**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; établir un lien entre les propriétés de deux objets, comme décrit dans l’article [ **une liaison de données**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; effectue la liaison de données à partir d’un modèle de contrôle, comme indiqué dans l’article [**liaison à partir d’un modèle de contrôle**] / guides/xamarin-forms/application-notions de base/modèles/contrôle-modèles/liaison de modèle /)

Le [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) disposition utilise l’extension de balisage personnalisée [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/). Cette extension de balisage est décrit dans l’article [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static, extension de balisage

Le `x:Static` extension de balisage est prise en charge par le [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) classe. La classe a une propriété unique nommée [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) de type `string` que vous définissez le nom de constante publique, une propriété statique, champ statique ou membre de l’énumération.

Une façon courante d’utiliser `x:Static` doit d’abord définir une classe avec des constantes ou des variables statiques, tels que ce petit `AppConstants` classe dans le [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) programme :

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

Le **x : Static démonstration** page montre plusieurs façons d’utiliser le `x:Static` extension de balisage. L’approche la plus abondante instancie le `StaticExtension` classe entre `Label.FontSize` balises d’élément de propriété :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

L’analyseur XAML permet également la `StaticExtension` abrégé en tant que classe `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Cela peut être simplifié davantage, mais la modification introduit une nouvelle syntaxe : il se compose de placer la `StaticExtension` classe et le nouveau membre entre accolades. L’expression résultante est définie directement sur le `FontSize` attribut :

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Notez qu’il y *aucune* guillemets entre les accolades. Le `Member` propriété du `StaticExtension` n’est plus un attribut XML. Il appartient à la place de l’expression pour l’extension de balisage.

Tout comme vous pouvez abréger `x:StaticExtension` à `x:Static` lorsque vous l’utilisez en tant qu’élément objet, vous pouvez l’abréger également dans l’expression entre accolades :

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Le `StaticExtension` classe a un `ContentProperty` attribut faisant référence à la propriété `Member`, qui indique que cette propriété en tant que propriété de contenu par défaut de la classe. Pour les extensions de balisage XAML d’exprimer des accolades, vous pouvez éliminer le `Member=` dans le cadre de l’expression :

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Il s’agit de la forme la plus commune le `x:Static` extension de balisage.

Le **démonstration statique** page contient deux autres exemples. La balise racine du fichier XAML contient une déclaration d’espace de noms XML pour le .NET `System` espace de noms :

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Cela permet la `Label` taille de police à définir pour le champ statique `Math.PI`. Qui résulte dans le texte de petite au lieu de cela, donc la `Scale` est définie sur `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Le dernier exemple affiche la `Device.RuntimePlatform` valeur. Le `Environment.NewLine` propriété statique est utilisée pour insérer un caractère de saut de ligne entre les deux `Span` objets :

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Voici l’exemple en cours d’exécution sur les trois plateformes :

[![x : Static démonstration](consuming-images/staticdemo-small.png "x : Static démonstration")](consuming-images/staticdemo-large.png#lightbox "x : Static démonstration")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Référence, extension de balisage

Le `x:Reference` extension de balisage est prise en charge par le [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) classe. La classe a une propriété unique nommée [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) de type `string` que vous définissez le nom d’un élément sur la page qui a reçu un nom avec `x:Name`. Cela `Name` propriété est la propriété de contenu de `ReferenceExtension`, de sorte que `Name=` n’est pas requise lorsque `x:Reference` apparaît entre accolades.

Le `x:Reference` extension de balisage est utilisée exclusivement avec les liaisons de données, qui sont décrits plus en détail dans l’article [ **une liaison de données**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Le **x : Reference démonstration** page montre deux utilisations de `x:Reference` avec des liaisons de données, la première où il est utilisé pour définir le `Source` propriété de la `Binding` objet et le second où il est utilisé pour définir le `BindingContext` propriété pour les liaisons de deux données :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Les deux `x:Reference` expressions utilisent la version abrégée de le `ReferenceExtension` nom de classe et d’éliminer la `Name=` dans le cadre de l’expression. Dans le premier exemple, le `x:Reference` extension de balisage est incorporée dans le `Binding` extension de balisage. Notez que la `Source` et `StringFormat` paramètres sont séparés par des virgules. Voici le programme en cours d’exécution sur les trois plateformes :

[![Démonstration de x : Reference](consuming-images/referencedemo-small.png "x : Reference démonstration")](consuming-images/referencedemo-large.png#lightbox "x : Reference démonstration")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type, extension de balisage

Le `x:Type` extension de balisage est l’équivalent XAML du langage c# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) (mot clé). Il est pris en charge par le [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) (classe), qui définit une propriété nommée [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) de type `string` qui est défini sur un nom de classe ou structure. Le `x:Type` retourne d’extension de balisage la [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) objet de cette classe ou structure. `TypeName` est la propriété de contenu de `TypeExtension`, de sorte que `TypeName=` n’est pas requise lorsque `x:Type` s’affiche avec des accolades.

Dans Xamarin.Forms, il existe plusieurs propriétés qui possèdent des arguments de type `Type`. Exemples incluent la [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) propriété de `Style`et le [x : TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) attribut utilisé pour spécifier des arguments dans les classes génériques. Toutefois, l’analyseur XAML effectue le `typeof` opération automatiquement et le `x:Type` extension de balisage n’est pas utilisée dans ces cas.

Un seul emplacement où `x:Type` *est* est requis par le `x:Array` extension de balisage, ce qui est décrit dans la [section suivante](#array).

Le `x:Type` extension de balisage est également utile lors de la construction d’un menu dans lequel chaque élément de menu correspondant à un objet d’un type particulier. Vous pouvez associer un `Type` avec chaque élément de menu de l’objet et ensuite instancier l’objet lorsque l’élément de menu est sélectionné.

Il s’agit de comment le menu de navigation dans `MainPage` dans les **les Extensions de balisage** programme fonctionne. Le **MainPage.xaml** fichier contient un `TableView` avec chaque `TextCell` correspondant à une page spécifique dans le programme :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Voici la page d’accueil principale **les Extensions de balisage**:

[![Principale Page](consuming-images/mainpage-small.png "Main Page")](consuming-images/mainpage-large.png#lightbox "Main Page")

Chaque `CommandParameter` est définie sur une `x:Type` extension de balisage qui fait référence à une des autres pages. Le `Command` propriété est liée à une propriété nommée `NavigateCommand`. Cette propriété est définie dans le `MainPage` fichier code-behind :

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Le `NavigateCommand` propriété est un `Command` objet qui implémente une exécution de commande avec un argument de type `Type` &mdash; la valeur de `CommandParameter`. La méthode utilise `Activator.CreateInstance` pour instancier la page puis accède à ce dernier. Le constructeur se termine en définissant le `BindingContext` de la page à elle-même, ce qui permet la `Binding` sur `Command` pour fonctionner. Consultez le [ **une liaison de données** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) l’article et en particulier le [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) article pour plus d’informations sur ce type de code.

Le **x : Type démonstration** page utilise une technique similaire pour instancier Xamarin.Forms éléments et les ajouter à un `StackLayout`. XAML initialement se compose de trois `Button` éléments avec leurs `Command` propriétés définies un `Binding` et le `CommandParameter` propriétés définies sur les types de trois vues Xamarin.Forms :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Le fichier code-behind définit et initialise le `CreateCommand` propriété :

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

La méthode qui est exécutée lorsque un `Button` est enfoncé crée une nouvelle instance de l’argument, définit son `VerticalOptions` propriété et l’ajoute à la `StackLayout`. Les trois `Button` éléments puis partagent la page avec les vues créées dynamiquement :

[![x : Type démonstration](consuming-images/typedemo-small.png "x : Type démonstration")](consuming-images/typedemo-large.png#lightbox "démonstration de x : Type")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array, extension de balisage

Le `x:Array` extension de balisage vous permet de définir un tableau dans le balisage. Il est pris en charge par le [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) (classe), qui définit les deux propriétés :

- `Type` de type `Type`, ce qui indique le type des éléments du tableau.
- `Items` de type `IList`, qui est une collection des éléments eux-mêmes. Il s’agit de la propriété de contenu `ArrayExtension`.

Le `x:Array` extension de balisage lui-même n’apparaît jamais entre accolades. Au lieu de cela, `x:Array` la liste des éléments de délimitent les balises de début et de fin. Définir le `Type` propriété un `x:Type` extension de balisage.

Le **x : Array démonstration** page montre comment utiliser `x:Array` pour ajouter des éléments à un `ListView` en définissant le `ItemsSource` propriété dans un tableau :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

Le `ViewCell` crée un simple `BoxView` pour chaque entrée de couleur :

[![x : Array démonstration](consuming-images/arraydemo-small.png "x : Array démonstration")](consuming-images/arraydemo-large.png#lightbox "x : Array, démonstration")

Il existe plusieurs façons de spécifier la personne `Color` éléments de ce tableau. Vous pouvez utiliser un `x:Static` extension de balisage :

```xaml
<x:Static Member="Color.Blue" />
```

Vous pouvez également utiliser `StaticResource` pour récupérer une couleur à partir d’un dictionnaire de ressources :

```xaml
<StaticResource Key="myColor" />
```

Vers la fin de cet article, vous verrez une extension de balisage XAML personnalisée qui crée également une nouvelle valeur de couleur :

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Lorsque vous définissez des tableaux de types courants tels que des chaînes ou des nombres, utilisez les balises répertoriées dans le [ **en passant les Arguments de constructeur** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) article pour délimiter les valeurs.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null, extension de balisage

Le `x:Null` extension de balisage est prise en charge par le [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) classe. Il ne possède aucune propriété et est simplement l’équivalent XAML du langage c# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) (mot clé).

Le `x:Null` extension de balisage est rarement nécessaire et rarement utilisée, mais si vous ne trouvez pas besoin, vous serez féliciterez de l’avoir il existe.

Le **x : Null démonstration** page illustre un scénario lorsque `x:Null` peut s’avérer utile. Supposons que vous définissez implicite `Style` pour `Label` qui inclut un `Setter` qui définit le `FontFamily` propriété à un nom de famille dépend de la plateforme :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Puis vous découvrez que pour l’une de la `Label` éléments, vous voulez tous les paramètres de propriété dans implicite `Style` à l’exception de la `FontFamily`, auquel vous souhaitez être la valeur par défaut. Vous pouvez définir un autre `Style` pour cet effet, mais une approche plus simple consiste simplement à définir le `FontFamily` propriété de particulier `Label` à `x:Null`, comme illustré dans le centre de `Label`.

Voici le programme en cours d’exécution sur les plateformes de trois :

[![Démonstration de x : Null](consuming-images/nulldemo-small.png "x : Null démonstration")](consuming-images/nulldemo-large.png#lightbox "démonstration de x : Null")

Remarquez que quatre de la `Label` éléments ont une police sans serif, mais le centre de `Label` a la police sans serif par défaut.

## <a name="define-your-own-markup-extensions"></a>Définir vos propres Extensions de balisage

Si vous avez rencontré un besoin pour une extension de balisage XAML qui n’est pas disponible dans Xamarin.Forms, vous pouvez [créer vos propres](creating.md).


## <a name="related-links"></a>Liens associés

- [Extensions de balisage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Chapitre d’extensions de balisage XAML à partir du carnet de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Dictionnaires de ressources](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Styles dynamiques](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Liaison de données](~/xamarin-forms/app-fundamentals/data-binding/index.md)
