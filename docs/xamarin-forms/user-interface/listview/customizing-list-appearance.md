---
title: Apparence de la liste
description: "Personnaliser les ListViews à l’aide d’en-têtes, les pieds de page, les groupes et les cellules de hauteur variable."
ms.topic: article
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 4e849cc034b8c77b84d1be8cb31cc1206f203ef6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="list-appearance"></a>Apparence de la liste

`ListView` a des options pour contrôler la présentation de la liste globale, en plus de sous-jacent `ViewCell`s. Les options sont les suivantes :

- [**Regroupement** ](#Grouping) &ndash; grouper des éléments dans le ListView pour faciliter la navigation et l’amélioration de l’organisation.
- [**En-têtes et pieds de page** ](#Headers_and_Footers) &ndash; afficher des informations au début et à la fin de la vue défile avec les autres éléments.
- [**Séparateurs de ligne** ](#Row_Separators) &ndash; afficher ou masquer les lignes de séparation entre les éléments.
- [**Les lignes de hauteur variable** ](#Row_Heights) &ndash; par défaut, toutes les lignes ont la même hauteur, mais cela peut être modifié pour autoriser des lignes avec différentes hauteurs à afficher.

<a name="Grouping" />

## <a name="grouping"></a>Regroupement
Souvent, les grands ensembles de données peuvent devenir complexe lorsque présentée dans une liste déroulante en permanence. L’activation de regroupement peut améliorer l’expérience utilisateur dans ces cas par mieux organiser le contenu et l’activation des contrôles spécifiques à une plateforme qui facilitent la navigation des données.

Lorsque le regroupement est activé pour un `ListView`, une ligne d’en-tête est ajoutée pour chaque groupe.

Pour activer le regroupement :

- Créer une liste de listes (une liste de groupes, chaque groupe en cours d’une liste d’éléments).
- Définir le `ListView`de `ItemsSource` à cette liste.
- Définissez `IsGroupingEnabled` sur true.
- Définissez [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) à lier à des groupes de la propriété qui est utilisée comme titre du groupe.
- [Facultatif] Définissez [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) à lier à des groupes de la propriété qui est utilisée en tant que le nom court pour le groupe. Le nom court est utilisé pour les listes de saut (colonne rigt côté sur iOS, la grille sur Windows Phone).

Commencez par créer une classe pour les groupes :

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

Dans le code ci-dessus, `All` est la liste qui sera allouée à notre ListView comme source de liaison. `Title` et `ShortName` sont les propriétés qui seront utilisées pour les en-têtes de groupe.

À ce stade, `All` est une liste vide. Ajoutez un constructeur statique afin que la liste contiendra au lancement du programme :

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

Dans le code ci-dessus nous pouvons également appeler `Add` sur les éléments de `groups`, qui sont des instances de type `PageTypeGroup`. Cela est possible, car `PageTypeGroup` hérite `List<PageModel>`. Il s’agit d’un exemple de la liste du modèle de liste indiqué ci-dessus.

Voici le code XAML pour l’affichage de la liste groupée :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Voici le résultat :

![](customizing-list-appearance-images/grouping-depth.png "Exemple de regroupement de ListView")

Notez que nous avons :

- Définissez `GroupShortNameBinding` à la `ShortName` propriété définie dans la classe de groupe
- Définissez `GroupDisplayBinding` à la `Title` propriété définie dans la classe de groupe
- Définissez `IsGroupingEnabled` sur true
- Modifié le `ListView`de `ItemsSource` à la liste groupée

### <a name="customizing-grouping"></a>Personnalisation du regroupement
Maintenant que nous avons vu comment mettre en œuvre de la base de regroupement dans ListView, voyons comment personnaliser l’affichage des en-têtes de groupe.

Semblable à la façon dont le `ListView` a un `ItemTemplate` pour définir le mode d’affichage des lignes `ListView` a un `GroupHeaderTemplate`. Il s’agit d’un exemple de ListView ci-dessus, avec un modèle d’en-tête de groupe personnalisé :

![](customizing-list-appearance-images/grouping-depth.png "ListView avec GroupHeaderTemplate personnalisé")


Voici comment accomplir cette conception en XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>En-têtes et pieds de page
Il est possible qu’un ListView permet de présenter un en-tête et pied de page que vous faites défiler les éléments de la liste. L’en-tête et le pied de page peuvent être des chaînes de texte ou une mise en page plus complexe. Notez que c’est différent [section groupes](#Grouping).

Vous pouvez définir le `Header` et/ou `Footer` en une chaîne simple valeur, ou vous pouvez les définir à une disposition plus complexe.
Il existe également `HeaderTemplate` et `FooterTemplate` les propriétés qui vous permettent de créent des dispositions plus complexes pour l’en-tête et le pied de page de cette prise en charge la liaison de données.

Pour créer un en-tête/pied de page simple, définissez simplement au texte que vous souhaitez afficher les propriétés d’en-tête ou pied de page. En code :

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

En XAML :

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView avec l’en-tête et pied de page")

Pour créer un en-tête personnalisé et le pied de page, définissez les vues de l’en-tête et pied de page :

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView avec l’en-tête personnalisé et un pied de page")

<a name="Row_Separators" />

## <a name="row-separators"></a>Séparateurs de lignes
Les lignes de séparation sont affichées entre `ListView` éléments par défaut sur iOS et Android. Windows Phone ne prend pas en charge les lignes du séparateur, par ce plateformes instructions d’expérience utilisateur. Si vous préférez masquer les lignes de séparation sur iOS et Android, définissez le `SeparatorVisibility` propriété sur votre contrôle ListView. Les options de `SeparatorVisibility` sont :

* **Par défaut** -affiche une ligne de séparation sur iOS et Android.
* **Aucun** -masque le séparateur sur toutes les plateformes.

Visibilité de la valeur par défaut :

C# :

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML :

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView avec séparateurs de ligne par défaut")

None :

C# :

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML :

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView sans séparateurs de lignes")

Vous pouvez également définir la couleur de la ligne de séparation via le `SeparatorColor` propriété :

C# :

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML :

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView avec séparateurs de lignes de vert")

> [!NOTE]
> Définition d’une de ces propriétés sur Android après le chargement du `ListView` entraîne une baisse des performances.

<a name="Row_Heights" />

## <a name="row-heights"></a>Hauteurs de lignes
Par défaut, toutes les lignes dans un ListView ont la même hauteur. ListView possède deux propriétés qui peuvent être utilisées pour modifier ce comportement :

- `HasUnevenRows` &ndash; `true`/`false` valeur, les lignes ont des hauteurs variables si la valeur `true`. La valeur par défaut est `false`.
- `RowHeight` &ndash; définit la hauteur de chaque ligne lorsque `HasUnevenRows` est `false`.

Vous pouvez définir la hauteur de toutes les lignes en définissant le `RowHeight` propriété sur le `ListView`.

### <a name="custom-fixed-row-height"></a>Hauteur de ligne fixe personnalisé

C# :

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML :

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView avec la hauteur de ligne fixe")


### <a name="uneven-rows"></a>Lignes inégale

Si vous souhaitez que des lignes individuelles dans ont des hauteurs différentes, vous pouvez définir le `HasUnevenRows` propriété `true`.
Notez que les hauteurs de lignes ne doivent être définies manuellement une fois `HasUnevenRows` a été défini sur `true`, car la hauteur est automatiquement calculée par Xamarin.Forms.


C# :

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML :

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView avec des lignes inégale")

### <a name="runtime-resizing-of-rows"></a>Runtime redimensionnement des lignes

Individuels `ListView` lignes peuvent être redimensionnées par programmation à l’exécution, à condition que la `HasUnevenRows` est définie sur `true`. Le [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) méthode met à jour les taille d’une cellule, même si elle n’est pas actuellement visible, comme illustré dans l’exemple de code suivant :

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

Le `OnImageTapped` Gestionnaire d’événements est exécuté en réponse à une [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) dans une cellule qui est activé par un clic et augmente la taille de la `Image` affiché dans la cellule afin qu’il est affiché facilement.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView avec le redimensionnement de la ligne de l’exécution")

Notez qu’il existe un fort risque de dégradation des performances si cette fonctionnalité est utilisée de manière excessive.



## <a name="related-links"></a>Liens associés

- [Regroupement (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Affichage de convertisseur personnalisé (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Redimensionnement des lignes dynamiques (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [notes de version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [notes de version 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
