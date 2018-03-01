---
title: "Référence des contrôles de pages de données"
ms.topic: article
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: fbbc7e8c93f8ed562381d9203060c862960f44b9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="datapages-controls-reference"></a>Référence des contrôles de pages de données

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!IMPORTANT]
> Pages de données requiert un [Xamarin.Forms thème](~/xamarin-forms/user-interface/themes/index.md) référence à restituer.


Nuget Xamarin.Forms pages de données inclut un nombre de contrôles qui peuvent tirer parti de la liaison de source de données.

Pour utiliser ces contrôles en XAML, vérifiez l’espace de noms a été inclus, par exemple le `xmlns:pages` déclaration ci-dessous :

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Les exemples ci-dessous incluent `DynamicResource` références qui doivent exister dans le dictionnaire de ressources du projet à utiliser. Il existe également un exemple de la création d’un [contrôle personnalisé](#custom)

## <a name="built-in-controls"></a>Contrôles intégrés

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

Le `HeroImage` contrôle possède quatre propriétés :

* Texte
* Détail
* ImageSource
* Aspect

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Contrôle HeroImage sur Android") ![ ] (controls-images/heroimage-dark-android.png "contrôle HeroImage sur Android")

**iOS**

![](controls-images/heroimage-light-ios.png "Contrôle HeroImage sur iOS") ![ ] (controls-images/heroimage-dark-ios.png "contrôle HeroImage sur iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

Le `ListItem` mise en page du contrôle est semblable à natives iOS et Android liste ou un tableau lignes, toutefois il peut également être utilisé comme une vue standard. Dans l’exemple de code en dessous figure hébergé à l’intérieur d’un `StackLayout`, mais il peut également être utilisé dans les contrôles de liste lié aux données scolling.

Il existe cinq propriétés :

* Titre
* Détail
* ImageSource
* PlaceholdImageSource
* Aspect

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Ces captures d’écran affichent les `ListItem` sur iOS et Android plateformes à l’aide à la fois les thèmes clairs et foncés :

**Android**

![](controls-images/listitem-light-android.png "Contrôle ListItem sur Android") ![ ] (controls-images/listitem-dark-android.png "contrôle ListItem sur Android")

**iOS**

![](controls-images/listitem-light-ios.png "Contrôle ListItem sur iOS") ![ ] (controls-images/listitem-dark-ios.png "contrôle ListItem sur iOS")


## <a name="custom-control-example"></a>Exemple de contrôle personnalisé

L’objectif de cette personnalisation `CardView` contrôle doit ressembler à la CardView Android natif.

Il contient trois propriétés :

* Texte
* Détail
* ImageSource

L’objectif est d’un contrôle personnalisé qui se présenter comme le code ci-dessous (Notez que personnalisé `xmlns:local` est obligatoire qui fait référence à l’assembly actuel) :

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Il doit ressembler aux captures d’écran ci-dessous à l’aide de couleurs correspondant aux thèmes clairs et foncés intégrés :

**Android**

![](controls-images/cardview-light-android.png "Un contrôle personnalisé de CardView sur Android") ![ ] (controls-images/cardview-dark-android.png "un contrôle personnalisé de CardView sur Android")

**iOS**

![](controls-images/cardview-light-ios.png "Un contrôle personnalisé CardView sur iOS") ![ ] (controls-images/cardview-dark-ios.png "CardView un contrôle personnalisé sur iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Création de la CardView personnalisé

1. [Sous-classe de DataView](#1)
2. [Définir la police, la disposition et les marges](#2)
3. [Créer des Styles pour les enfants du contrôle](#3)
4. [Créer le modèle de contrôle de disposition](#4)
5. [Ajoutez les ressources spécifiques au thème](#5)
6. [Définir le modèle ControlTemplate pour la classe CardView](#6)
7. [Ajouter le contrôle à une Page](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. Sous-classe de DataView

La sous-classe de C# `DataView` définit les propriétés pouvant être liées pour le contrôle.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Définir la police, la disposition et les marges

Le Concepteur de contrôles est identifier ces valeurs en tant que partie de la conception de l’interface utilisateur pour le contrôle personnalisé. Lorsque les spécifications de plateforme spécifique sont requises, le `OnPlatform` élément est utilisé.

Notez que certaines valeurs font référence aux `StaticResource`s : elles sont définies dans [étape 5](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness" x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness" x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Créer des Styles pour les enfants du contrôle

Faire référence à tous les éléments définis sur le point de créer les enfants qui seront utilisés dans le contrôle personnalisé :

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Créer le modèle de contrôle de disposition

La conception visuelle du contrôle personnalisé est déclarée explicitement dans le modèle de contrôle, à l’aide de la ressource définie ci-dessus :

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Ajoutez les ressources spécifiques au thème

Comme il s’agit d’un contrôle personnalisé, ajoutez les ressources qui correspondent au thème que vous utilisez le dictionnaire de ressources :

##### <a name="light-theme-colors"></a>Couleurs de thème clair

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Couleurs du thème sombre

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Définir le modèle ControlTemplate pour la classe CardView

Enfin, assurez-vous de la classe c# créée dans [étape 1](#1) utilise le modèle de contrôle défini dans [étape 4](#4) à l’aide un `Style` `Setter` élément

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Ajouter le contrôle à une Page

Le `CardView` contrôle peut maintenant être ajouté à une page. L’exemple ci-dessous montre qu’il est hébergé dans un `StackLayout`:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
