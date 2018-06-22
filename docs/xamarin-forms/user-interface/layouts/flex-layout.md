---
title: Le Xamarin.Forms FlexLayout
description: Utilisez FlexLayout pour l’empilage ou une collection de vues enfants d’habillage.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 7585138cd6c33c2a5dc537ba28101a84e1c4b7ae
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921834"
---
# <a name="the-xamarinforms-flexlayout"></a>Le Xamarin.Forms FlexLayout

_Utilisez FlexLayout pour l’empilage ou une collection de vues enfants d’habillage._

Le Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) est une nouveauté de la version 3.0 de Xamarin.Forms. Il est basé sur le code CSS [Flexible Module de mise en page de boîte de](http://www.w3.org/TR/css-flexbox-1/), communément appelé _flex disposition_ ou _flex-box_, appelée ainsi car il comprend de nombreuses options flexibles pour réorganiser des enfants la mise en page.

`FlexLayout` est similaire à la Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) dans la mesure où il peut réorganiser ses enfants horizontalement et verticalement dans une pile. Toutefois, le `FlexLayout` est également capable d’encapsuler ses enfants si elles sont trop nombreuses pour tenir dans une seule ligne ou une colonne, et a également de nombreuses options pour l’orientation, l’alignement et s’adapter aux différentes tailles d’écran.

`FlexLayout` dérive de [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) et hérite d’un [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) propriété de type `IList<View>`.

`FlexLayout` définit six propriétés pouvant être liées publiques et cinq propriétés pouvant être liées jointes qui affectent la taille, l’orientation et l’alignement de ses éléments enfants. (Si vous n’êtes pas familiarisé avec les propriétés pouvant être liées jointes, consultez l’article  **[propriétés jointes](~/xamarin-forms/xaml/attached-properties.md)**.) Ces propriétés sont décrites en détail dans les sections ci-dessous sur **[les propriétés pouvant être liées en détail](#bindable-properties)** et  **[les propriétés attachées pouvant être liées en détail](#attached-properties)**. Toutefois, cet article commence par une section sur certains **[scénarios d’utilisation courants](#common-scenarios)** de `FlexLayout` qui décrit un grand nombre de ces propriétés de manière plus informelle. Vers la fin de l’article, vous allez apprendre à combiner `FlexLayout` avec [feuilles de style CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Scénarios d’utilisation courants

Le **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** exemple de programme contient plusieurs pages qu’illustrer utilisations courantes de `FlexLayout` et vous permet de faire des essais avec ses propriétés.

### <a name="using-flexlayout-for-a-simple-stack"></a>À l’aide de FlexLayout forment une pile simple

Le **pile Simple** page montre comment `FlexLayout` pouvez remplacer par un `StackLayout` mais avec balisage plus simple. Tous les éléments de cet exemple sont défini dans la page XAML. Le `FlexLayout` contient quatre enfants :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

Voici la page en cours d’exécution sur iOS, Android et la plateforme Windows universelle :

[![Page de la pile Simple](flex-layout-images/SimpleStack.png "Simple de Page de la pile")](flex-layout-images/SimpleStack-Large.png#lightbox)

Trois propriétés de `FlexLayout` figurent dans le **SimpleStackPage.xaml** fichier :

- Le [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) est définie sur une valeur de la [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) énumération. La valeur par défaut est `Row`. Attribuer à la propriété `Column` entraîne les enfants de le `FlexLayout` à être réorganisées dans une seule colonne d’éléments.

    Lorsque les éléments dans un `FlexLayout` sont réorganisées dans une colonne, la `FlexLayout` est dit d’un vertical _axe principal_ et horizontal _coupe axe_.

- Le [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) propriété est de type [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) et spécifie la façon dont les éléments sont alignés sur l’axe transversal. Le `Center` option entraîne par chaque élément pour être centrée horizontalement.

    Si vous utilisiez un `StackLayout` plutôt qu’un `FlexLayout` pour cette tâche, vous serez centrer tous les éléments en assignant le `HorizontalOptions` propriété de chaque élément pour `Center`. Le `HorizontalOptions` propriété ne fonctionne pas pour les enfants d’un `FlexLayout`, mais le seul `AlignItems` propriété accomplit le même objectif. Si vous le souhaitez, vous pouvez utiliser la `AlignSelf` attaché une propriété pouvant être liée pour remplacer le `AlignItems` propriété pour des éléments individuels :

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Avec cette modification, celle-ci `Label` est positionné sur le bord gauche de la `FlexLayout` lorsque l’ordre de lecture est de gauche à droite.

- Le [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) propriété est de type [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)et spécifie la façon dont les éléments sont organisés sur l’axe principal. Le `SpaceEvenly` option alloue tout espace vertical restant entre tous les éléments et au-dessus du premier élément et en dessous du dernier élément.

    Si vous utilisiez un `StackLayout`, vous devez affecter la `VerticalOptions` propriété de chaque élément à `CenterAndExpand` pour obtenir un résultat similaire. Mais la `CenterAndExpand` option affecte deux fois plus d’espace entre chaque élément qu’avant le premier élément et après le dernier élément. Vous pouvez imiter le `CenterAndExpand` option de `VerticalOptions` en définissant le `JustifyContent` propriété du `FlexLayout` à `SpaceAround`.

Ces `FlexLayout` propriétés sont décrites plus en détail dans la section **[les propriétés pouvant être liées en détail](#bindable-properties)** ci-dessous.

### <a name="using-flexlayout-for-wrapping-items"></a>À l’aide de FlexLayout pour l’habillage des éléments

Le **Photo Wrapping** page de la **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** exemple illustre comment `FlexLayout` peut encapsuler ses enfants à d’autres lignes ou colonnes. Le fichier XAML instancie le `FlexLayout` et assigne les deux propriétés :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

Le `Direction` propriété de ce `FlexLayout` n’est pas définie, elle a la valeur par défaut `Row`, ce qui signifie que les enfants sont organisées en lignes et l’axe principal est horizontale.

Le [ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) propriété est d’un type énumération [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). S’il y a trop d’éléments pour tenir sur une ligne, ce paramètre de propriété entraîne les éléments à inclure dans un wrapper à la ligne suivante.

Notez que la `FlexLayout` est un enfant d’un `ScrollView`. S’il existe trop de lignes pour s’ajuster à la page, puis le `ScrollView` a une valeur par défaut `Orientation` propriété du `Vertical` et permet le défilement vertical.

Le `JustifyContent` propriété alloue l’espace sur l’axe principal (l’axe horizontal) afin que chaque élément est entourée par la même quantité d’espace vide.

Le fichier code-behind accède à une collection d’exemples de photos et les ajoute à la `Children` collection de la `FlexLayout`:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Voici le programme en cours d’exécution sur les trois plateformes, progressivement défilés de haut en bas :

[![La Page de renvoi à la ligne Photo](flex-layout-images/PhotoWrapping.png "la Page de renvoi à la ligne de Photo")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Mise en page avec FlexLayout

Il existe une disposition standard dans la conception web appelée le [ _Salut_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) , car il s’agit d’un format de disposition est très souhaitable, mais il est souvent difficile à réaliser avec la perfection. La disposition se compose d’un en-tête en haut de la page et un pied de page en bas, les deux étendant sur toute la largeur de la page. Occupée par le centre de la page est le contenu principal, mais souvent avec un menu en colonnes à gauche les contenu et les informations supplémentaires (parfois appelé un _mettre de côté_ zone) à droite. [Section 5.4.1 de la spécification de mise en forme de zone Flexible CSS](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) décrit la façon dont la disposition Salut peut être réalisée avec une zone de flex.

Le **Salut disposition** page de la **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** montre une implémentation simple de cette disposition à l’aide d’un `FlexLayout` imbriqué dans un autre. Cette page est conçu pour un téléphone en mode portrait, les zones à gauche et à droite de la zone de contenu sont uniquement 50 pixels de large :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

Ici, il s’exécute sur les plateformes de trois :

[![La Page de disposition Salut](flex-layout-images/HolyGrailLayout.png "la Page de disposition Salut")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Les zones de navigation et aside sont rendus avec une `BoxView` à gauche et droite.

Le premier `FlexLayout` dans le code XAML fichier a un axe principal et contient les trois enfants sont réorganisés dans une colonne. Il s’agit de l’en-tête, le corps de la page et le pied de page. Imbriqué `FlexLayout` a un axe principal horizontal avec trois enfants disposées dans une ligne.

Trois propriétés pouvant être liées attachées sont illustrées dans ce programme :

- Le `Order` attachée propriété pouvant être liée est définie sur le premier `BoxView`. Cette propriété est un entier avec la valeur par défaut 0. Vous pouvez utiliser cette propriété pour modifier l’ordre de mise en page. En général les développeurs préfèrent le contenu de la page s’affiche dans le balisage avant les éléments de navigation et mettre de côté les éléments. Définition de la `Order` propriété sur le premier `BoxView` à une valeur inférieure à ses autres frères fait apparaître comme le premier élément dans la ligne. De même, vous pouvez vous assurer qu’un élément apparaît en dernier en définissant le `Order` propriété à une valeur supérieure à ses frères.

- Le `Basis` attachée propriété pouvant être liée est définie sur les deux `BoxView` éléments à leur attribuer une largeur de 50 pixels. Cette propriété est de type `FlexBasis`, une structure qui définit une propriété statique de type `FlexBasis` nommé `Auto`, qui est la valeur par défaut. Vous pouvez utiliser `Basis` pour spécifier une taille en pixels ou un pourcentage qui indique la quantité d’espace, l’élément occupe sur l’axe principal. Il est appelé un _base_ car elle spécifie une taille de l’élément qui est la base de la disposition de toutes les.

- Le `Grow` est définie sur l’imbriquée `Layout` et sur la `Label` enfants représentant le contenu. Cette propriété est de type `float` et a la valeur par défaut 0. Lorsque la valeur est une valeur positive, tout l’espace restant sur l’axe principal est alloué à cet élément et frères à l’aide des valeurs positives de `Grow`. L’espace est alloué proportionnellement aux valeurs de données, similaire à la spécification en étoile dans un `Grid`.

    La première `Grow` propriété jointe est définie sur l’imbriquée `FlexLayout`, qui indique que cette `FlexLayout` pour occuper tout l’espace vertical inutilisé dans la liste externe est `FlexLayout`. La seconde `Grow` propriété jointe est définie sur le `Label` représentant le contenu, indiquant que ce contenu est pour occuper tout l’espace horizontal inutilisé dans interne `FlexLayout`.

    Il existe également une procédure similaire `Shrink` attaché une propriété pouvant être liée que vous pouvez utiliser lorsque la taille d’enfants dépasse la taille de la `FlexLayout` mais habillage n’est pas souhaité.

### <a name="catalog-items-with-flexlayout"></a>Éléments du catalogue avec FlexLayout

Le **éléments de catalogue** page dans le **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** exemple est semblable à [exemple 1 dans la version 1.1 de Section de la spécification de la zone de disposition CSS Flex](http://www.w3.org/TR/css-flexbox-1/#overview), sauf qu’elle affiche une série de défilement horizontale d’images et les descriptions de trois singes :

[![Les éléments du catalogue](flex-layout-images/CatalogItems.png "Page des éléments du catalogue")](flex-layout-images/CatalogItems-Large.png#lightbox)

Chacune des trois singes est un `FlexLayout` contenus dans un `Frame` donné une hauteur explicite et la largeur, et qui est également un enfant d’une plus grande `FlexLayout`. Dans ce fichier XAML, la plupart des propriétés de la `FlexLayout` enfants sont spécifiés dans les styles, tous sauf un d’eux est un style implicite :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Le style implicit pour la `Image` comprend des paramètres de deux propriétés pouvant être liées attachées de `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

Le `Order` paramètre &ndash;1 provoque le `Image` élément à afficher en premier dans chacune des imbriqué `FlexLayout` vues, quelle que soit sa position dans la collection d’enfants. Le `AlignSelf` propriété du `Center` provoque la `Image` sera centré dans la `FlexLayout`. Cela remplace le paramètre de la `AlignItems` propriété, qui a la valeur par défaut de `Stretch`, ce qui signifie qui le `Label` et `Button` enfants sont étirées sur toute la largeur de la `FlexLayout`.

Dans chacun des trois `FlexLayout` consulte, une valeur vide `Label` précède le `Button`, mais il comporte un `Grow` paramètre 1. Cela signifie que tout l’espace vertical est affectée à ce vide `Label`, qui transmet en réalité le `Button` vers le bas.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Les propriétés pouvant être liées en détail

Maintenant que vous avez vu certaines applications courantes de `FlexLayout`, les propriétés de `FlexLayout` permet d’Explorer plus en détail. 
`FlexLayout` définit les propriétés pouvant être liées six que vous définissez sur le `FlexLayout` lui-même, soit dans le code ou XAML, l’alignement et de contrôle orientatin. (Une de ces propriétés, [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), n’est pas couverte dans cet article.)

Vous pouvez expérimenter cinq restant propriétés pouvant être liées à l’aide de la **expérimenter** page de la **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** exemple. Cette page permet d’ajouter ou supprimer des enfants d’un `FlexLayout` et pour définir des combinaisons de cinq propriétés pouvant être liées. Tous les enfants de la `FlexLayout` sont `Label` vues de différentes couleurs et les tailles, avec la `Text` propriété définie sur un nombre correspondant à sa position dans le `Children` collection.

Lorsque le programme démarre, cinq `Picker` vues affichent les valeurs par défaut de ces cinq `FlexLayout` propriétés. Le `FlexLayout` vers le bas de l’écran contient trois enfants :

[![La Page expérience : la valeur par défaut](flex-layout-images/ExperimentDefault.png "la Page expérience - par défaut")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Chacun de la `Label` vues a un fond gris qui affiche l’espace alloué à cette `Label` dans le `FlexLayout`. L’arrière-plan de la `FlexLayout` lui-même est Alice bleu. Il occupe la zone entière en bas de la page à l’exception d’une petite marge à gauche et à droite.

<a name="direction" />

### <a name="the-direction-property"></a>La propriété Direction

Le [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) propriété est de type [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), une énumération avec quatre membres :

- `Column`
- `ColumnReverse` (ou « colonne inverse » en XAML)
- `Row`, la valeur par défaut
- `RowReverse` (ou « ligne-inverse » en XAML)

En XAML, vous pouvez spécifier la valeur de cette propriété en utilisant les noms de membre d’énumération en minuscules, majuscules, ou casse mixte, ou vous pouvez utiliser deux chaînes supplémentaires indiqués entre parenthèses sont les mêmes que les indicateurs CSS. (Les chaînes « colonne inversion » et « ligne inversion » sont définies dans le [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) classe utilisée par l’analyseur XAML.)

Voici le **expérience** page indiquant (de gauche à droite), le `Row` direction, `Column` direction, et `ColumnReverse` direction :

[![La Page expérience : Direction](flex-layout-images/ExperimentDirection.png "la Page expérience - Direction")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Notez que pour la `Reverse` options, les éléments de démarrent à droite ou au-dessous.

<a name="wrap" />

### <a name="the-wrap-property"></a>La propriété de type Wrap

Le [ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) propriété est de type [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), une énumération avec trois membres :

- `NoWrap`, la valeur par défaut
- `Wrap`
- `Reverse` (ou « wrap inversion » en XAML)

De gauche à droite, ces écrans montrent le `NoWrap`, `Wrap` et `Reverse` options pour les enfants de 12 :

[![La Page expérience : Encapsuler](flex-layout-images/ExperimentWrap.png "la Page expérience - Wrap")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Lorsque le `Wrap` est définie sur `NoWrap` l’axe principal est limitée (par exemple, ce programme) et l’axe principal n’est pas large ou suffisamment grands pour répondre à tous les enfants, le `FlexLayout` tente de réduire les éléments, comme la capture d’écran iOS montre. Vous pouvez contrôler le shrinkness des éléments avec le [ `Shrink` ](#shrink) jointe propriété pouvant être liée.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>La propriété JustifyContent

Le [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) propriété est de type [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), une énumération avec six membres :

- `Start` (ou « flex-start » en XAML), la valeur par défaut
- `Center`
- `End` (ou « flex-fin » en XAML)
- `SpaceBetween` (ou « espace entre « en XAML)
- `SpaceAround` (ou « espace around » en XAML)
- `SpaceEvenly`

Cette propriété spécifie la manière dont les éléments sont régulièrement espacées sur l’axe principal, qui est l’axe horizontal dans cet exemple :

[![La Page expérience : Justifier contenu](flex-layout-images/ExperimentJustifyContent.png "la Page expérience - justifier le contenu")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Dans toutes les captures de trois écran, le `Wrap` est définie sur `Wrap`. Le `Start` par défaut est indiqué dans la capture d’écran Android précédente. La capture d’écran iOS ici montre la `Center` option : tous les éléments sont déplacés vers le centre. Les trois autres options commence par le mot `Space` allouer l’espace supplémentaire non occupée par les éléments. `SpaceBetween` alloue de l’espace entre les éléments ; `SpaceAround` met égal à l’espace autour de chaque élément, tandis que `SpaceEvenly` place égal à l’espace entre chaque élément et avant le premier élément et après le dernier élément sur la ligne.

<a name="align-items" />

### <a name="the-alignitems-property"></a>La propriété AlignItems

Le [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) propriété est de type [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), une énumération avec quatre membres :

- `Stretch`, la valeur par défaut
- `Center`
- `Start` (ou « flex-start » en XAML)
- `End` (ou « flex-fin » en XAML)

Il s’agit d’une des deux propriétés (l’autre étant [ `AlignContent` ](#align-content)) qui indique la façon dont les enfants sont alignés sur l’axe transversal. Dans chaque ligne, les enfants sont étendues (comme indiqué dans la capture d’écran précédente), ou alignés sur le début, centre ou fin de chaque élément, comme indiqué dans les captures d’écran de trois suivantes :

[![La Page expérience : Aligner les éléments](flex-layout-images/ExperimentAlignItems.png "aligner les éléments de la Page expérience -")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Dans la capture d’écran iOS, les parties supérieures de tous les enfants sont alignés. Dans les captures d’écran Android, les éléments sont centrés verticalement en fonction de l’enfant plus haut. Dans la capture d’écran de la plateforme Windows universelle, le bas de tous les éléments sont aligné.

Pour n’importe quel élément individuel, le `AlignItems` paramètre peut être remplacé par le [ `AlignSelf` ](#align-self) jointe propriété pouvant être liée.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>La propriété AlignContent

Le [ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) propriété est de type [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), une énumération avec sept membres :

- `Stretch`, la valeur par défaut
- `Center`
- `Start` (ou « flex-start » en XAML)
- `End` (ou « flex-fin » en XAML)
- `SpaceBetween` (ou « espace entre « en XAML)
- `SpaceAround` (ou « espace around » en XAML)
- `SpaceEvenly`

Comme `AlignItems`, le `AlignContent` propriété également aligne enfants sur l’axe transversal, mais affecte des lignes et colonnes entières :

[![La Page expérience : Aligner le contenu](flex-layout-images/ExperimentAlignContent.png "la Page expérience - aligner le contenu")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

Dans la capture d’écran iOS, les deux lignes sont en haut ; dans la capture d’écran Android, ils contiennent dans le centre de ; et la capture d’écran de la plateforme Windows universelle, elles sont en bas. Les lignes peuvent également être espacés de différentes manières :

[![La Page expérience : Aligner 2 contenu](flex-layout-images/ExperimentAlignContent2.png "la Page expérience - aligner contenu 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

Le `AlignContent` n’a aucun effet lorsqu’il n'existe qu’une seule ligne ou colonne.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Les propriétés attachées pouvant être liées en détail

`FlexLayout` définit les cinq propriétés pouvant être liées attachées. Ces propriétés sont définies sur les enfants de le `FlexLayout` et se rapportent uniquement à cet enfant particulier.

<a name="align-self" />

### <a name="the-alignself-property"></a>La propriété AlignSelf

Le [ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) propriété pouvant être liée jointe est de type [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), une énumération avec cinq membres :

- `Auto`, la valeur par défaut
- `Stretch`
- `Center`
- `Start` (ou « flex-start » en XAML)
- `End` (ou « flex-fin » en XAML)

Pour les enfants individuels de la `FlexLayout`, cette propriété remplacements de paramètres la [ `AlignItems` ](#align-items) propriété définie sur la `FlexLayout` lui-même. Le paramètre par défaut de `Auto` consiste à utiliser le `AlignItems` paramètre.

Pour un `Label` élément nommé `label` (ou exemple), vous pouvez définir le `AlignSelf` propriété dans le code comme suit :

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Notez qu’il n’existe aucune référence à la `FlexLayout` parent de la `Label`. En XAML, vous définissez la propriété comme suit :

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>La propriété Order

Le [ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) propriété est de type `int`. La valeur par défaut est 0.

Le `Order` propriété vous permet de modifier l’ordre que les enfants de la `FlexLayout` sont réorganisées. En règle générale, les enfants d’un `FlexLayout` sont réorganisées est le même ordre d’apparition dans le `Children` collection. Vous pouvez remplacer cet ordre en définissant le `Order` jointe propriété pouvant être liée à une valeur entière non nulle sur un ou plusieurs enfants. Le `FlexLayout` réorganise ses enfants en fonction du paramètre de la `Order` propriété sur chaque enfant, mais les enfants avec le même `Order` paramètre sont disposés dans l’ordre où ils apparaissent dans le `Children` collection.

### <a name="the-basis-property"></a>La propriété de base

Le [ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) propriété pouvant être liée jointe indique la quantité d’espace alloué à un enfant de la `FlexLayout` sur l’axe principal. La taille spécifiée par la `Basis` propriété est la taille de l’axe principal du parent `FlexLayout`. Par conséquent, `Basis` indique la largeur d’un enfant lorsque les enfants sont organisés dans des lignes ou la hauteur lorsque les enfants sont organisées en colonnes.

Le `Basis` propriété est de type [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), une structure. La taille peut être spécifiée en unités indépendantes du périphérique, ou sous forme de pourcentage de la taille de la `FlexLayout`. La valeur par défaut de la `Basis` propriété est la propriété statique `FlexBasis.Auto`, ce qui signifie que l’enfant demandé largeur ou hauteur est utilisée.

Dans le code, vous pouvez définir le `Basis` propriété pour un `Label` nommé `label` à 40 unités indépendantes du périphérique comme suit :

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Le deuxième argument de la `FlexBasis` constructeur est appelé `isRelative` et indique si la taille est relative (`true`) ou absolu (`false`). L’argument a la valeur par défaut `false`, de sorte que vous pouvez également utiliser le code suivant :

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Une conversion implicite de `float` à `FlexBasis` est défini, vous pouvez ainsi simplifier il encore :

```csharp
FlexLayout.SetBasis(label, 40);
```

Vous pouvez définir la taille de 25 % de la `FlexLayout` parent comme suit :

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Cette valeur de fraction doit être dans la plage de 0 à 1.

En XAML, vous pouvez utiliser un nombre pour une taille en unités indépendantes du périphérique :

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Ou vous pouvez spécifier un pourcentage de la plage de 0 à 100 % :

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

Le **base expérimenter** page de la **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** exemple vous permet de faire des essais avec le `Basis` propriété. La page affiche une colonne encapsulée de cinq `Label` éléments avec les couleurs d’arrière-plan et de remplacement. Deux `Slider` éléments vous permettent de spécifier `Basis` valeurs pour les deuxième et quatrième `Label`:

[![La base de faire des essais Page](flex-layout-images/BasisExperiment.png "faire des essais la base de la Page")](flex-layout-images/BasisExperiment-Large.png#lightbox)

La capture d’écran d’iOS situé à gauche montre les deux `Label` éléments recevant des hauteurs en unités indépendantes du périphérique. L’écran Android montre les donnée hauteurs sont une fraction de la hauteur totale de la `FlexLayout`. Si le `Basis` est défini à 100 %, l’enfant est la hauteur de la `FlexLayout`et est renvoyé à la colonne suivante et occuper toute la hauteur de cette colonne, comme illustré par la capture d’écran de la plateforme Windows universelle dans : il s’affiche comme si les cinq enfants sont organisés dans une ligne , mais elles apparaissent effectivement dans cinq colonnes.

### <a name="the-grow-property"></a>La propriété de la croissance

Le [ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) propriété pouvant être liée jointe est de type `int`. La valeur par défaut est 0, et la valeur doit être supérieure ou égale à 0.

Le `Grow` propriété joue un rôle lorsque le `Wrap` est définie sur `NoWrap` et la ligne d’enfants a une largeur totale inférieure à la largeur de la `FlexLayout`, ou la colonne d’enfants a une hauteur plus courte que la `FlexLayout`. Le `Grow` propriété indique comment répartir l’espace restante parmi les enfants.

Dans le **augmenter l’expérience** page cinq `Label` éléments des couleurs alternées sont organisés dans une colonne et deux `Slider` éléments vous permettent d’ajuster la `Grow` propriété du deuxième et quatrième `Label`. La capture d’écran d’iOS à l’extrême gauche affiche la valeur par défaut `Grow` propriétés 0 :

[![La Page expérience Grow](flex-layout-images/GrowExperiment.png "la Page expérience Grow")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Si tout un seul enfant reçoit un nombre positif `Grow` valeur, alors que les enfants occupe tout l’espace restant, comme illustré par la capture d’écran Android dans. Cet espace peut également être alloué entre deux ou plusieurs enfants. Dans la capture d’écran de la plateforme Windows universelle, le `Grow` propriété du deuxième `Label` a la valeur 0,5, tandis que la `Grow` propriété du quatrième `Label` est de 1,5, ce qui donne le quatrième `Label` trois fois plus de l’espace restant comme second `Label`.

Comment la vue enfant utilise cet espace dépend du type particulier d’enfant. Pour un `Label`, le texte peut être placé dans l’espace total de la `Label` en utilisant les propriétés `HorizontalTextAlignment` et `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>La propriété de réduction

Le [ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) propriété pouvant être liée jointe est de type `int`. La valeur par défaut est 1, et la valeur doit être supérieure ou égale à 0.

Le `Shrink` propriété joue un rôle lorsque le `Wrap` est définie sur `NoWrap` et la largeur d’agrégation d’une ligne d’enfants est supérieure à la largeur de la `FlexLayout`, ou la hauteur d’agrégation d’une seule colonne d’enfants est supérieure à la hauteur de la `FlexLayout`. Normalement le `FlexLayout` affiche ces enfants en limitant les membres de leur taille. Le `Shrink` propriété peut indiquer les enfants sont prioritaires dans l’affichage à leur taille complète.

Le **réduire l’expérience** page crée un `FlexLayout` avec une seule ligne de cinq `Label` enfants qui nécessitent plus d’espace que le `FlexLayout` largeur. La capture d’écran d’iOS situé à gauche montre tous les `Label` éléments avec les valeurs par défaut de 1 :

[![La réduction expérimenter Page](flex-layout-images/ShrinkExperiment.png "la réduction expérimenter la Page")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Dans la capture d’écran Android, le `Shrink` valeur pour la deuxième `Label` a la valeur 0 et que `Label` s’affiche dans sa pleine largeur. En outre, le quatrième `Label` reçoit un `Shrink` valeur supérieure à 1, et il a réduit. La capture d’écran UWP montre à la fois `Label` les éléments d’un `Shrink` la valeur 0 pour pouvoir être affichés dans leur taille, si cela est possible.

Vous pouvez définir à la fois le `Grow` et `Shrink` valeurs pour prendre en charge des situations où les tailles d’agrégation enfant peuvent parfois être inférieur ou parfois supérieure à la taille de la `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>Styles CSS avec FlexLayout

Vous pouvez utiliser la [styles CSS](~/xamarin-forms/user-interface/styles/css/index.md) fonctionnalité introduite avec 3.0 Xamarin.Forms par `FlexLayout`. Le **CSS les éléments de catalogue** page de la **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** exemple duplique la disposition de la **éléments de catalogue** page, mais avec une CSS feuille de style pour la plupart des styles :

[![Page des éléments de catalogue CSS](flex-layout-images/CssCatalogItems.png "le CSS Page des éléments de catalogue")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

La version d’origine **CatalogItemsPage.xaml** fichier possède cinq `Style` définitions dans son `Resources` section avec 15 `Setter` objets. Dans le **CssCatalogItemsPage.xaml** fichier, ce qui a été réduite à deux `Style` définitions en seulement quatre `Setter` objets. Ces styles de complément la feuille de style CSS pour les propriétés qui la fonctionnalité de style CSS de Xamarin.Forms actuellement ne prennent en charge :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

La feuille de style CSS est référencée dans la première ligne de la `Resources` section :

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Notez également que les deux éléments dans chacun des trois éléments incluent `StyleClass` paramètres :

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Ils désignent les sélecteurs dans le **CatalogItemsStyles.css** feuille de style :

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

Plusieurs `FlexLayout` joint des propriétés pouvant être liées sont référencées ici. Dans le `label.empty` sélecteur, vous verrez la `flex-grow` attribut, les styles vide `Label` pour fournir un espace vide ci-dessus le `Button`. Le `image` sélecteur contient un `order` attribut et un `align-self` attribut, qui correspondent aux `FlexLayout` jointe propriétés pouvant être liées.

Vous avez vu que vous pouvez définir des propriétés directement sur le `FlexLayout` et vous pouvez définir les propriétés pouvant être liées jointes sur les enfants d’un `FlexLayout`. Ou bien, vous pouvez définir ces propriétés indirectement à l’aide de styles de basée sur XAML traditionnels ou les styles CSS. Est de ce qui est important de connaître et comprendre ces propriétés. Ces propriétés sont en quoi le `FlexLayout` réellement flexible. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout avec Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 Flex mise en page, par [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Liens connexes

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
