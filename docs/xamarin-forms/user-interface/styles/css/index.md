---
title: Styles des applications de Xamarin.Forms à l’aide de feuilles de Style en cascade
description: Xamarin.Forms prend en charge les éléments visuels de style à l’aide de feuilles de Style en cascade (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 811abacff330bf7b6e6240691cb6a15ebbd9d242
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets"></a>Styles des applications de Xamarin.Forms à l’aide de feuilles de Style en cascade

_Xamarin.Forms prend en charge les éléments visuels de style à l’aide de feuilles de Style en cascade (CSS)._

Xamarin.Forms 3.0 introduit la possibilité d’appliquer un style d’une application à l’aide de CSS. Une feuille de style se compose d’une liste de règles, chacune comprenant un ou plusieurs sélecteurs et un bloc de déclaration. Un bloc de déclaration se compose d’une liste de déclarations entre accolades, avec chaque déclaration consistant en une propriété, un signe deux-points et une valeur. Lorsqu’il existe plusieurs déclarations dans un bloc, un point-virgule est inséré comme un séparateur. L’exemple de code suivant illustre certaines CSS conforme Xamarin.Forms :

```css
^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

Dans Xamarin.Forms, feuilles de style CSS sont analysées et évaluées au moment de l’exécution, plutôt que de compilation, et les feuilles de style sont réanalysés sur l’utilisation.

> [!NOTE]
> Actuellement, tous des éléments de style qui est possible avec les styles de XAML ne peut pas être effectuée avec CSS. Toutefois, les styles XAML peuvent servir à compléter CSS pour les propriétés qui ne sont actuellement pas pris en charge par Xamarin.Forms. Pour plus d’informations sur les styles XAML, consultez [styles des applications de Xamarin.Forms à l’aide de Styles de XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

Le [MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) exemple illustre l’utilisation de CSS pour une application simple de style et est indiqué dans les captures d’écran suivants :

[![Page principale de MonkeyApp avec des styles CSS](css-images/MonkeyAppMainPage.png "MonkeyApp Main Page avec les styles CSS")](css-images/MonkeyAppMainPage-Large.png#lightbox "MonkeyApp Main Page avec les styles CSS")

[![Page des détails avec les styles CSS MonkeyApp](css-images/MonkeyAppDetailPage.png "MonkeyApp Page des détails avec les styles CSS")](css-images/MonkeyAppDetailPage-Large.png#lightbox "MonkeyApp Page des détails avec les styles CSS")

> [!NOTE]
> Il n’est pas possible actuellement d’appliquer un style de la couleur d’arrière-plan d’un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) à l’aide d’une feuille de style. Par conséquent, dans l’exemple d’application le [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) propriété est définie dans le code.

## <a name="consuming-a-style-sheet"></a>Utilisation d’une feuille de style

Le processus d’ajout d’une feuille de style à une solution est la suivante :

1. Ajouter un fichier CSS vide à votre projet de bibliothèque .NET Standard.
1. Action de génération du fichier CSS à **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Le chargement d’une feuille de style

Il existe plusieurs approches qui peut être utilisé pour charger une feuille de style.

### <a name="xaml"></a>XAML

Une feuille de style peut être chargée et analysée avec le [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) classe avant d’être ajouté à la [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pour la page :

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Le [ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) propriété spécifie la feuille de style en tant qu’URI relatif à l’emplacement du fichier XAML englobant, ou par rapport à la racine du projet si l’URI commence par un `/`.

> [!WARNING]
> Le fichier CSS chargement échouera si elle est action de génération n’est pas définie sur **EmbeddedResource**.

Vous pouvez également la feuille de style peut être chargée et analysée avec le [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) classe par incorporation (inlining) dans un `CDATA` section :

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

### <a name="c"></a>C#

En c#, une feuille de style peut être chargée en tant que ressource incorporée et ajoutée à la [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pour la page :

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

Le premier argument de la `StyleSheet.FromAssemblyResource` méthode est l’assembly qui contient la feuille de style, tandis que le second argument est un `string` représentant l’identificateur de ressource. L’identificateur de ressource peut être obtenu à partir de la **propriétés** fenêtre lorsque le fichier CSS est sélectionné.

Ou bien, la feuille de style peut être chargée à une `StringReader` et ajouté à la [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) pour la page :

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

L’argument de la `StyleSheet.FromReader` méthode est le `TextReader` qui a lu la feuille de style.

## <a name="selecting-elements-and-applying-properties"></a>Sélection des éléments et en appliquant des propriétés

CSS utilise des sélecteurs pour déterminer les éléments à cibler. Styles présentant des sélecteurs sont appliquées de manière consécutive, dans l’ordre de définition. Les styles définis sur un élément spécifique sont toujours appliqués en dernier. Pour plus d’informations sur les sélecteurs de prise en charge, consultez [sélecteur référence](#selector-reference).

CSS utilise les propriétés pour définir le style d’un élément sélectionné. Chaque propriété possède un ensemble de valeurs possibles, et certaines propriétés peuvent affecter n’importe quel type d’élément, tandis que d’autres s’appliquent à des groupes d’éléments. Pour plus d’informations sur les propriétés prises en charge, consultez [référence de propriété](#property-reference).

### <a name="selecting-elements-by-type"></a>Sélection des éléments par type

Les éléments dans l’arborescence d’éléments visuels peuvent être sélectionnés par type avec le non-respect de la casse `element` sélecteur :

```css
stacklayout {
    margin: 20;
}
```

Ce sélecteur identifie les [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) éléments sur les pages qui consomment de la feuille de style et définit les marges pour une épaisseur uniforme de 20.

> [!NOTE]
> Le `element` sélecteur n’identifie pas les sous-classes du type spécifié.

### <a name="selecting-elements-by-base-class"></a>Sélection des éléments par la classe de base

Les éléments dans l’arborescence d’éléments visuels peuvent être sélectionnés par la classe de base avec le non-respect de la casse `^base` sélecteur :

```css
^contentpage {
    background-color: lightgray;
}
```

Ce sélecteur identifie les [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) éléments qui consomment de la feuille de style et définit son arrière-plan couleur `lightgray`.

> [!NOTE]
> Le `^base` sélecteur est spécifique à Xamarin.Forms et ne fait pas partie de la spécification de CSS.

### <a name="selecting-an-element-by-name"></a>Sélection d’un élément par nom

Les éléments individuels dans l’arborescence d’éléments visuels peuvent être sélectionnés avec le respect de la casse `#id` sélecteur :

```css
#listView {
    background-color: lightgray;
}
```

Identifie l’élément de ce sélecteur dont [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) est définie sur `listView`. Toutefois, si le `StyleId` propriété n’est pas définie, le sélecteur fasse appel à l’aide du `x:Name` de l’élément. Par conséquent, dans l’exemple de code XAML suivant, le `#listView` sélecteur identifiera la [ `ListView` ](xref:Xamarin.Forms.ListView) dont `x:Name` attribut a la valeur `listView`et définit sa couleur d’arrière-plan sur `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Sélection d’éléments avec un attribut de classe spécifique

Éléments avec un attribut de classe spécifique peuvent être sélectionnés avec le respect de la casse `.class` sélecteur :

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Une classe CSS permettre être affectée à un élément XAML en définissant le [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) propriété de l’élément au nom de classe CSS. Par conséquent, dans l’exemple de code XAML suivant, les styles définis par le `.detailPageTitle` classe sont affectés à la première [ `Label` ](xref:Xamarin.Forms.Label), tandis que les styles définis par le `.detailPageSubtitle` classe sont affectés à la seconde `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Sélection d’éléments enfants

Éléments enfants dans l’arborescence d’éléments visuels peuvent être sélectionnés avec le non-respect de la casse `element element` sélecteur :

```css
listview image {
    height: 60;
    width: 60;
}
```

Ce sélecteur identifie les [ `Image` ](xref:Xamarin.Forms.Image) les éléments qui sont des enfants [ `ListView` ](xref:Xamarin.Forms.ListView) éléments et définit la hauteur et la largeur à 60. Par conséquent, dans l’exemple de code XAML suivant, le `listview image` sélecteur identifiera la [ `Image` ](xref:Xamarin.Forms.Image) qui est un enfant de la [ `ListView` ](xref:Xamarin.Forms.ListView)et définit sa hauteur et sa largeur à 60.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Le `element element` sélecteur ne nécessite pas l’élément enfant à un _direct_ enfant du parent : l’élément enfant peut avoir un parent différent. La sélection se produit si un ancêtre est le premier élément spécifié.

### <a name="selecting-direct-child-elements"></a>Sélection d’éléments enfants directs

Diriger des éléments enfants dans l’arborescence d’éléments visuels peuvent être sélectionnés avec le non-respect de la casse `element>element` sélecteur :

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Ce sélecteur identifie les [ `Image` ](xref:Xamarin.Forms.Image) les éléments qui sont des enfants directs [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) éléments et définit la hauteur et la largeur à 200. Par conséquent, dans l’exemple de code XAML suivant, le `stacklayout>image` sélecteur identifiera la [ `Image` ](xref:Xamarin.Forms.Image) qui est un enfant direct de le [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)et définit sa hauteur et sa largeur à 200.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> Le `element>element` sélecteur nécessite que l’élément enfant est un _direct_ enfant du parent.

## <a name="selector-reference"></a>Référence du sélecteur

Sélecteurs CSS suivants sont pris en charge par Xamarin.Forms :

|Sélecteur|Exemple|Description|
|---|---|---|
|`.class`|`.header`|Sélectionne tous les éléments dont le `StyleClass` propriété contenant « en-tête ». Notez que ce sélecteur respecte la casse.|
|`#id`|`#email`|Sélectionne tous les éléments avec `StyleId` la valeur `email`. Si `StyleId` n’est pas définie, revenir à `x:Name`. Lors de l’utilisation de XAML, `x:Name` vaut mieux utiliser `StyleId`. Notez que ce sélecteur respecte la casse.|
|`*`|`*`|Sélectionne tous les éléments.|
|`element`|`label`|Sélectionne tous les éléments de type `Label`, mais pas des classes. Notez que ce sélecteur respecte la casse.|
|`^base`|`^contentpage`|Sélectionne tous les éléments avec `ContentPage` comme classe de base, y compris `ContentPage` lui-même. Notez que ce sélecteur respecte la casse et ne fait pas partie de la spécification de CSS.|
|`element,element`|`label,button`|Sélectionne tous les `Button` éléments et tous les `Label` éléments. Notez que ce sélecteur respecte la casse.|
|`element element`|`stacklayout label`|Sélectionne tous les `Label` éléments à l’intérieur d’un `StackLayout`. Notez que ce sélecteur respecte la casse.|
|`element>element`|`stacklayout>label`|Sélectionne tous les `Label` éléments avec `StackLayout` comme un parent direct. Notez que ce sélecteur respecte la casse.|
|`element+element`|`label+entry`|Sélectionne tous les `Entry` éléments directement après une `Label`. Notez que ce sélecteur respecte la casse.|
|`element~element`|`label~entry`|Sélectionne tous les `Entry` éléments précédé par un `Label`. Notez que ce sélecteur respecte la casse.|

Styles présentant des sélecteurs sont appliquées de manière consécutive, dans l’ordre de définition. Les styles définis sur un élément spécifique sont toujours appliqués en dernier.

> [!TIP]
> Sélecteurs peuvent être combinées sans limitation, tel que `StackLayout>ContentView>label.email`.

Les sélecteurs suivantes ne sont actuellement pas pris en charge :

- `[attribute]`
- `@media` et `@supports`
- `:` et `::`

> [!NOTE]
> Spécificité et des remplacements de spécificité sont pris en charge.

## <a name="property-reference"></a>Référence de propriété

Les propriétés CSS suivantes sont prises en charge par Xamarin.Forms (dans le **valeurs** sont des types de colonne, _italique_, tandis que les littéraux de chaîne sont `gray`) :

|Propriété|S'applique à|Valeurs|Exemple|
|---|---|---|---|
|`background-color`|`VisualElement`|_Couleur_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Chaîne_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Couleur_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_Double_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_Couleur_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Chaîne_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Double_ \| _namedsize_  \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Double_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_Épaisseur_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Épaisseur_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Épaisseur_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Épaisseur_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Épaisseur_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_Double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Double_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_Épaisseur_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_Double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _Double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _Double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _Double_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` et `right` doivent être évités dans les environnements de droite à gauche.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` est une valeur valide pour toutes les propriétés. Elle efface la valeur (valeur par défaut réinitialise) qui a été définie à partir d’un autre style.

Les propriétés suivantes sont actuellement pas pris en charge :

- `all: initial`.
- Propriétés de disposition (zone ou grille).
- Propriétés de raccourci, tel que `font`, et `border`.

En outre, il est sans `inherit` l’héritage de valeur et donc n’est pas pris en charge. Par conséquent, vous ne pouvez pas définir, par exemple, le `font-size` propriété sur une mise en page et attendre tous le [ `Label` ](xref:Xamarin.Forms.Label) instances dans la disposition d’hériter de la valeur. La seule exception concerne le `direction` propriété, qui a la valeur par défaut de `inherit`.

### <a name="color"></a>Color

Les éléments suivants `color` valeurs sont prises en charge :

- `X11` [couleurs](https://en.wikipedia.org/wiki/X11_color_names/), qui correspondent aux couleurs CSS, des couleurs prédéfinies UWP et les couleurs de Xamarin.Forms. Notez que ces valeurs de couleur sont insensible à la casse.
- hex couleurs : `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- les couleurs RVB : `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Les valeurs sont dans la plage 0-255, ou 0-100 %.
- les couleurs RGBA : `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. La valeur d’opacité est dans la plage de 0.0 à 1.0.
- les couleurs TSL : `hsl(120, 100%, 50%)`. La valeur h est dans la plage 0-360, tandis que s et l sont dans la plage 0-100 %.
- les couleurs hsla : `hsla(120, 100%, 50%, .8)`. La valeur d’opacité est dans la plage de 0.0 à 1.0.

### <a name="thickness"></a>Thickness

Un, deux, trois ou quatre `thickness` valeurs sont prises en charge, séparés par un espace blanc :

- Une valeur unique indique l’épaisseur uniforme.
- Deux valeurs indiquent épaisseur vertical puis horizontal.
- Trois valeurs indiquent le haut, puis horizontal (gauche et droite), puis épaisseur du bas.
- Quatre valeurs indiquent haut, droite, bas, puis sur épaisseur gauche.

> [!NOTE]
> CSS `thickness` diffèrent des valeurs à partir de XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) valeurs. Par exemple, dans le XAML d’une valeur de deux `Thickness` indique l’épaisseur horizontal puis vertical, tandis qu’une valeur de quatre `Thickness` indique à gauche, puis en haut et à droite, puis bas épaisseur. En outre, XAML `Thickness` les valeurs sont séparés par des virgules.

### <a name="namedsize"></a>NamedSize

Les éléments suivants en majuscules ou minuscules `namedsize` valeurs sont prises en charge :

- `default`
- `micro`
- `small`
- `medium`
- `large`

La signification exacte de chaque `namedsize` valeur est dépendante de la plateforme et dépendantes de la vue.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS dans Xamarin.Forms avec Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS, par [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Liens associés

- [MonkeyAppCSS (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Styles des applications de Xamarin.Forms à l’aide de Styles de XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
