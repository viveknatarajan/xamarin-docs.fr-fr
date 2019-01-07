---
title: Xamarin.Forms Shell
description: Cet article explique comment utiliser Xamarin.Forms Shell, qui réduit la complexité des applications Xamarin.Forms en fournissant les fonctionnalités d’interface utilisateur clés nécessaires à la plupart des applications.
ms.prod: xamarin
ms.assetid: 1A674212-72DB-4AA4-B626-A4EC135AD1A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 422311c766584cbd27d0ab0c42adee042e9aac3e
ms.sourcegitcommit: 408b78dd6eded4696469e316af7922a5991f2211
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53246293"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

![Preview](~/media/shared/preview.png)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/Microsoft/TailwindTraders-Mobile)

Xamarin.Forms Shell est un conteneur pour applications qui offre des fonctionnalités d’interface utilisateur clés nécessaires à la plupart des applications pour vous permettre de vous concentrer sur la charge de travail principale de l’application. Les applications Shell sont fournies avec les fonctionnalités suivantes :

- Un emplacement unique pour décrire la structure visuelle d’une application.
- Une interface utilisateur de navigation commune.
- Un service de navigation avec lien profond.
- Un gestionnaire de recherche intégré.

Ces fonctionnalités réduisent la complexité des applications, tout en augmentant la productivité des développeurs. En outre, Shell est conçu pour optimiser la vitesse du rendu et la consommation de la mémoire.

> [!IMPORTANT]
> Les applications iOS et Android existantes peuvent adopter Shell, bénéficiant immédiatement d’une navigation, de performances et d’une extensibilité améliorées.

Shell fournit une interface utilisateur de navigation robuste, basée sur des menus volants et des onglets. Le niveau de navigation supérieur dans une application Shell est un menu volant :

![Menu volant](shell-images/flyout.png "Menu volant")

Le niveau de navigation suivant est la barre d’onglets inférieurs :

![Onglets inférieurs](shell-images/bottom-tabs-full.png "Onglets inférieurs")

Quand le menu volant n’est pas ouvert, la barre d’onglets inférieurs fait office de niveau de navigation supérieur.

Dans chaque onglet inférieur, le niveau de navigation suivant est la barre d’onglets supérieurs, où chaque onglet est une `ContentPage` :

![Onglets supérieurs](shell-images/top-tabs-full.png "Onglets supérieurs")

Dans chaque `ContentPage`, des instances `ContentPage` peuvent être ajoutées à la pile de navigation et supprimées de celle-ci.

## <a name="bootstrapping-a-shell-application"></a>Amorçage d’une application Shell

Pour amorcer une application Shell, vous définissez la propriété `MainPage` de la classe `App` sur une instance d’un fichier Shell :

```csharp
namespace TailwindTraders.Mobile
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            Forms.SetFlags("Shell_Experimental");
            MainPage = new TheShell();
        }
    }
}
```

La classe `TheShell` est un fichier XAML qui décrit la structure visuelle de votre application.

> [!IMPORTANT]
> Shell étant en phase expérimentale, vous pouvez uniquement l’utiliser en ajoutant `Forms.SetFlags("Shell_Experimental");` soit à votre classe `App`, avant de créer l’instance `Shell`, soit à votre projet de plateforme, avant d’appeler la méthode `Forms.Init`.

## <a name="shell-file-hierarchy"></a>Hiérarchie d’un fichier Shell

Un fichier Shell se compose de trois éléments hiérarchiques :

- `ShellItem`. Un ou plusieurs éléments dans le menu volant. Chaque `ShellItem` est un enfant d’un `Shell`.
- `ShellSection`. Contenu groupé, explorable par le biais des onglets inférieurs. Chaque `ShellSection` est un enfant d’un `ShellItem`.
- `ShellContent`. Les instances `ContentPage` dans votre application, explorables par le biais des onglets supérieurs. Chaque `ShellContent` est un enfant d’une `ShellSection`.

Aucun de ces éléments ne représente une interface utilisateur, mais plutôt l’organisation de la structure visuelle de l’application. À partir de ces éléments, Shell génère l’interface utilisateur de navigation pour le contenu.

Le code XAML suivant montre un exemple simple d’un fichier Shell :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>
</Shell>
```

> [!NOTE]
> Chaque `ShellItem` peut également définir une propriété `FlyoutIcon` sur une `ImageSource`, qui s’affiche à gauche du titre.

Ce code XAML affiche la `HomePage`, car c’est le premier élément de contenu déclaré dans le fichier Shell :

![Page d’accueil](shell-images/homepage.png "Page d’accueil")

Quand l’utilisateur appuie sur le bouton de menu général, le menu volant s’affiche :

![Menu volant ouvert](shell-images/flyout-initial.png "Menu volant ouvert")

Vous pouvez augmenter le nombre d’éléments du menu volant en ajoutant des instances `ShellItem` au fichier Shell. Toutefois, notez que la `HomePage` est créée au cours du démarrage de l’application et que si vous ajoutez des instances `ShellItem` au moyen de cette approche, les pages correspondantes sont également créées pendant le démarrage de l’application. Vous pouvez l’éviter en définissant la propriété `ShellContent.ContentTemplate` sur un `DataTemplate` :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    <ShellItem>
    <ShellContent Title="Profile"
                  ContentTemplate="{DataTemplate local:ProfilePage}" />
</Shell>
```

Avec cette approche, la `ProfilePage` est créée uniquement quand l’utilisateur y accède, plutôt qu’au démarrage de l’application. De plus, notez que pour la `ProfilePage`, les objets `ShellItem` et `ShellSection` sont omis, avec la propriété `Title` définie sur l’instance `ShellContent`. Cette approche concise nécessite moins de balisage, Shell fournissant les wrappers logiques nécessaires (sans ajouter de vues à l’arborescence visuelle).

Vous pouvez aussi personnaliser l’apparence de chaque `ShellItem` en définissant la propriété `Shell.ItemTemplate` sur un `DataTemplate` :

```xaml
<Shell.ItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Title}" />
        </ContentView>
    </DataTemplate>
</Shell.ItemTemplate>          
```

Ce code repositionne simplement le texte de chaque `ShellItem` dans le menu volant. Notez que Shell fournit les propriétés `Title` (et `Icon`) pour le `BindingContext` du `DataTemplate`.

## <a name="flyout"></a>Menu volant

Le menu volant est le menu racine de l’application ; il se compose d’un en-tête de menu volant et d’éléments de menu :

![Menu volant annoté](shell-images/flyout-annotated.png "Menu volant annoté")

### <a name="flyout-behavior"></a>Comportement du menu volant

L’utilisateur peut accéder au menu volant via le bouton de menu général ou en effectuant un balayage à partir du côté de l’écran. Toutefois, vous pouvez changer ce comportement en définissant la propriété `Shell.FlyoutBehavior` sur un des membres d’énumération `FlyoutBehavior` :

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

La définition de la propriété `FlyoutBehavior` sur `Disabled` masque le menu volant, ce qui est utile quand vous avez un seul `ShellItem`. Les autres valeurs `FlyoutBehavior` valides sont `Flyout` (valeur par défaut) et `Locked`.

### <a name="flyout-header"></a>En-tête de menu volant

L’en-tête de menu volant correspond au contenu qui apparaît facultativement en haut du menu volant, et son apparence est définie par une `View` configurable par le biais de la valeur de propriété `Shell.FlyoutHeader` :

```xaml
<Shell.FlyoutHeader>
    <local:FlyoutHeader />
</Shell.FlyoutHeader>
```

Vous pouvez également définir l’apparence de l’en-tête de menu volant en définissant la propriété `Shell.FlyoutHeaderTemplate` sur un `DataTemplate` :

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <StackLayout HorizontalOptions="Fill"
                     VerticalOptions="Fill"
                     BackgroundColor="White"
                     Padding="16">
            <Label FontSize="Medium"
                   Text="Smart Shopping">
                <Label.Margin>
                    <Thickness Left="8" />
                </Label.Margin>
            </Label>
            <Button Image="photo"
                    Text="By taking a photo">
                <Button.Margin>
                    <Thickness Top="16" />
                </Button.Margin>
            </Button>
            <Button Image="ia"
                    Text="By using AR">
                <Button.Margin>
                    <Thickness Top="8" />
                </Button.Margin>
            </Button>
        </StackLayout>
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

Ce code XAML génère l’en-tête de menu volant suivant :

![En-tête de menu volant](shell-images/flyout-header.png "En-tête de menu volant")

Par défaut, l’en-tête de menu volant occupe une position fixe dans le menu volant, tandis que le contenu situé en dessous défile s’il comprend suffisamment d’éléments. Toutefois, vous pouvez changer ce comportement en définissant la propriété `Shell.FlyoutHeaderBehavior` sur un des membres d’énumération `FlyoutHeaderBehavior` :

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

La définition de `FlyoutHeaderBehavior` sur `CollapseOnScroll` réduit le menu volant si un défilement se produit. Les autres valeurs `FlyoutHeaderBehavior` valides sont `Default`, `Fixed` et `Scroll` (défilement avec les éléments de menu).

## <a name="menu-items"></a>Éléments de menu

Vous pouvez augmenter le nombre d’éléments du menu volant en ajoutant des instances `ShellItem`. Toutefois, il est également possible d’ajouter des instances `MenuItem` au menu volant. Cela permet des scénarios comme la navigation vers une page identique tout en passant les données par le biais de la propriété `MenuItem.CommandParameter`.

Les instances `MenuItem` doivent être ajoutées à la collection `Shell.MenuItems` :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Views"
       x:Class="TailwindTraders.Shell"
       FlyoutHeaderBehavior="Fixed"
       Title="Tailwind Traders"
       ...>
    <Shell.MenuItems>
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="1"
                  Text="Holiday decorations" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="2"
                  Text="Appliances" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="3"
                  Text="Bathrooms" />
        ...
    </Shell.MenuItems>
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>    
</Shell>
```

> [!NOTE]
> Chaque `MenuItem` peut également définir une propriété `Icon` sur une `FileImageSource`, qui s’affiche à gauche du texte.

Ce code XAML génère le menu volant suivant :

![Menu volant complet](shell-images/flyout-full.png "Menu volant complet")

De plus, vous pouvez personnaliser l’apparence de `MenuItem` en définissant la propriété `Shell.MenuItemTemplate` sur un `DataTemplate` :

```xaml
<Shell.MenuItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Text}"
                   FontAttributes="Bold" />
        </ContentView>
    </DataTemplate>
</Shell.MenuItemTemplate>
```

Ainsi, le texte des instances `MenuItem` apparaît en gras :

![Éléments de menu en gras](shell-images/menuitems-bold.png "Éléments de menu en gras")

## <a name="tabs"></a>Tabulations

Les instances `ShellSection` sont affichées sous forme d’onglets inférieurs, à condition qu’il y ait plusieurs instances `ShellSection` dans un `ShellItem` unique :

```xaml
<ShellItem Title="Bottom Tab Sample"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="AR" Icon="ia.png">
        <ShellContent ContentTemplate="{DataTemplate local:ARPage}"/>
    </ShellSection>
    <ShellSection Title="Photo" Icon="photo.png">
        <ShellContent ContentTemplate="{DataTemplate local:PhotoPage}"/>
    </ShellSection>
</ShellItem>
```

Dans cet exemple, les instances `ShellSection` sont affichées sous forme d’onglets inférieurs :

![Onglets inférieurs](shell-images/tabs-bottom.png "Onglets inférieurs")

Les éléments `ShellContent` sont affichés sous forme d’onglets supérieurs, à condition qu’il y ait plusieurs instances `ShellContent` dans une `ShellSection` unique :

```xaml
<ShellItem Title="Store Home"
           Shell.TitleView="Store Home"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="Browse Product">
        <ShellContent Title="Featured"
                      ContentTemplate="{DataTemplate local:FeaturedPage}" />
        <ShellContent Title="On Sale"
                      ContentTemplate="{DataTemplate local:SalePage}" />
    </ShellSection>
</ShellItem>
```

Dans cet exemple, les instances `ShellContent` sont affichées sous forme d’onglets supérieurs :

![Onglets supérieurs](shell-images/tabs-top.png "Onglets supérieurs")

Vous pouvez définir le style des onglets à l’aide de styles XAML ou en fournissant un renderer personnalisé. L’exemple suivant montre un style XAML qui définit la couleur des onglets :

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.ShellTabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.ShellTabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.ShellTabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

## <a name="navigation"></a>Navigation

Shell inclut une expérience de navigation basée sur les URI. Les URI fournissent une expérience de navigation améliorée qui permet d’accéder à n’importe quelle page de l’application sans avoir à suivre une hiérarchie de navigation définie. De plus, l’utilisateur peut revenir en arrière sans avoir à visiter toutes les pages de la pile de navigation.

Cette navigation basée sur les URI fait appel à des routes, segments d’URI utilisés pour naviguer dans l’application. Le fichier Shell doit déclarer un schéma de route, un hôte de route et une route :

```xaml
<Shell ...
       Route="tailwindtraders"
       RouteHost="www.microsoft.com"
       RouteScheme="app">
    ...
</Shell>
```

Combinées, les valeurs de propriété `RouteScheme`, `RouteHost` et `Route` forment l’URI racine `app://www.microsoft.com/tailwindtraders`.

Chaque élément du fichier Shell peut également définir une propriété de routage utilisable dans une navigation programmatique.

Dans le constructeur de fichier Shell, ou à tout autre emplacement qui s’exécute avant qu’une route ne soit appelée, vous pouvez explicitement inscrire des routes supplémentaires pour toutes les pages qui ne sont pas représentées par un élément Shell (telles que les instances `MenuItem`) :

```csharp
Routing.RegisterRoute("productcategory", typeof(ProductCategoryPage));
```

### <a name="implementing-navigation"></a>Implémentation de la navigation

Les éléments de menu exposent une propriété `Command` servant à implémenter la navigation :

```csharp
public ICommand ProductTypeCommand { get; } = new Command<string>(NavigateToProductType);

static void NavigateToProductType(string typeId)
{
  (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"app:///tailwindtraders/productcategory?id={typeId}", true);
}
```

Pour appeler la navigation, vous devez obtenir une référence à l’application `Shell`, par le biais de la propriété `MainPage` de la classe `Application`. Ensuite, appelez la méthode `GoToAsync` en passant un URI valide comme argument. La méthode `GoToAsync` navigue à l’aide d’un objet `ShellNavigationState`, qui est construit à partir d’une `string` ou d’un `Uri`.

### <a name="passing-data"></a>Passage de données

Les données peuvent être passées entre les pages par le biais de paramètres de chaîne de requête. Shell définit les valeurs des paramètres de chaîne de requête sur la `ContentPage` ou le ViewModel quand vous ajoutez des attributs de propriété de requête à la classe :

```csharp
[QueryProperty("TypeID", "id")]
public partial class ProductCategoryPage : ContentPage
{
    private string _typeId;

    public ProductCategoryPage()
    {
        InitializeComponent();

        BindingContext = new ProductCategoryViewModel();
    }

    public string TypeID
    {
        get => _typeId;
        set => MyLabel.Text = value;
    }
}
```

L’attribut `QueryProperty` spécifie que le `TypeID` reçoit les données passées dans le paramètre de chaîne de requête `id` à partir de l’URI dans l’appel de la méthode `GoToAsync`.

### <a name="intercepting-navigation"></a>Interception de la navigation

Shell permet d’intervenir dans le routage de la navigation avant et après son terme. Pour ce faire, vous inscrivez des gestionnaires d’événements pour les événements `Navigating` et `Navigated`, respectivement :

```xaml
<Shell ...
       Navigating="OnShellNavigating">
    ...
</Shell>
```

```csharp
void OnShellNavigating(object sender, ShellNavigatingEventArgs e)
{
  if (...)
  {
    e.Cancel(); // Cancel the navigation
  }
}
```

La classe `ShellNavigatingEventArgs` fournit les propriétés suivantes :

| Property | Type | Description |
|---|---|---|
| Actuel | `ShellNavigationState` | URI de la page actuelle. |
| Source | `ShellNavigationState` | URI représentant l’origine de la navigation. |
| une cible | `ShellNavigationSource`  | URI représentant la destination de la navigation. |
| CanCancel  | `bool` | Valeur indiquant s’il est possible d’annuler la navigation. |
| Annulé  | `bool` | Valeur indiquant si la navigation a été annulée. |

De plus, la classe `ShellNavigatingEventArgs` fournit une méthode `Cancel`.

La classe `ShellNavigatedEventArgs` fournit les propriétés suivantes :

| Property | Type | Description |
|---|---|---|
| Actuel | `ShellNavigationState` | URI de la page actuelle. |
| Précédent| `ShellNavigationState` | URI de la page précédente. |
| Source  | `ShellNavigationSource` | URI représentant l’origine de la navigation. |

De plus, Shell fournit une méthode `OnBackButtonPressed` substituable permettant d’intercepter l’appui sur un bouton précédent.

## <a name="related-links"></a>Liens connexes

- [Tailwind Traders (exemple)](https://github.com/Microsoft/TailwindTraders-Mobile)
