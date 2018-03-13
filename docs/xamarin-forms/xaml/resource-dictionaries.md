---
title: Dictionnaires de ressources
description: "Ressources XAML sont des définitions d’objets qui peuvent être utilisés plusieurs fois. Un ResourceDictionary permet aux ressources définies dans un emplacement unique et réutilisées dans une application de Xamarin.Forms. Cet article explique comment créer et consommer un ResourceDictionary et comment fusionner les dictionnaires de ressources."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: 9602e4d99e8f5c004fe75ab724bb3746aca46003
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="resource-dictionaries"></a>Dictionnaires de ressources

_Ressources XAML sont des définitions d’objets qui peuvent être utilisés plusieurs fois. Un ResourceDictionary permet aux ressources définies dans un emplacement unique et réutilisées dans une application de Xamarin.Forms. Cet article explique comment créer et consommer un ResourceDictionary et comment fusionner les dictionnaires de ressources._

## <a name="overview"></a>Vue d'ensemble

A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) est un référentiel pour les ressources utilisées par une application de Xamarin.Forms. Les ressources standard qui sont stockés dans un `ResourceDictionary` incluent [styles](~/xamarin-forms/user-interface/styles/index.md), [modèles de contrôle](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [les modèles de données](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), couleurs et les convertisseurs.

En XAML, les ressources sont définies dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) récupérées et appliqués aux éléments à l’aide de la `StaticResource` extension de balisage. En c#, les ressources sont définies dans un `ResourceDictionary` récupérées et appliqués aux éléments à l’aide d’un indexeur basé sur chaîne. Toutefois, il n’est peu avantageuse, à l’aide un `ResourceDictionary` en c#, en tant que ressources peut facilement être directement attribué aux propriétés des éléments visuels sans devoir tout d’abord les récupérer à partir d’un `ResourceDictionary`.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Création et utilisation d’un ResourceDictionary

Les ressources peuvent être définies dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) qui est attaché à la [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) regroupement d’une page ou un contrôle, ou pour le [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) collection de l’application. Choix de l’emplacement définir un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) impacts où il peut être utilisé :

- Ressources dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) défini sur le contrôle niveau peut uniquement être appliqué pour le contrôle et ses enfants.
- Ressources dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) défini dans la page niveau peut uniquement être appliqué à la page et à ses enfants.
- Ressources dans un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) définie par l’application de niveau peut être appliqué dans l’ensemble de l’application.

L’exemple de code XAML suivant montre les ressources définies dans le niveau de l’application [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Cela [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) définit trois [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) ressources et un [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ressource. Pour plus d’informations sur la création d’un code XAML `App` de classe, consultez [classe App](~/xamarin-forms/app-fundamentals/application-class.md).

Chaque ressource a une clé qui est spécifiée à l’aide de la `x:Key` attribut, ce qui lui donne une clé Description dans la `ResourceDictionary`. La clé est utilisée pour récupérer une ressource à partir de la [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) par le `StaticResource` extension de balisage, comme illustré dans l’exemple de code XAML suivant qui indique des ressources supplémentaires définies dans un contrôle au niveau `ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

La première [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance récupère et utilise le `LabelPageHeadingStyle` définie dans le niveau de l’application de ressource [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), et de la seconde `Label` instance la récupération et la consommation du `LabelNormalStyle` définies dans le niveau de contrôle des ressources `ResourceDictionary`. De même, la [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) instance récupère et utilise le `NormalTextColor` définie dans le niveau de l’application de ressource `ResourceDictionary`et le `MediumBoldText` définies dans le niveau de contrôle des ressources `ResourceDictionary`. Cela provoque l’affichage indiqué dans les captures d’écran suivants :

[![](resource-dictionaries-images/screenshots-sml.png "Consommation des ressources de ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "consomment des ressources de ResourceDictionary")

> [!NOTE]
> Ressources qui sont spécifiques à une page unique ne doit pas être inclus dans un application au niveau dictionnaire de ressources, en tant que tel ressources seront ensuite analysés au démarrage de l’application au lieu de lorsque requis par une page. Pour plus d’informations, consultez [réduire la taille de dictionnaire de ressources Application](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Substitution de ressources

Lorsque [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) partagent des ressources `x:Key` des valeurs d’attribut définis inférieur dans la hiérarchie des ressources ont priorité sur ceux définis plus haut. Par exemple, si le `PageBackgroundColor` ressource `Blue` au niveau de l’application au niveau sera remplacé par le niveau de la page `PageBackgroundColor` ressource définie pour `Yellow`. De même, le niveau de la page `PageBackgroundColor` ressource sera remplacée par un niveau de contrôle `PageBackgroundColor` ressource. Cette priorité est illustrée par l’exemple de code XAML suivant :

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

La version d’origine `PageBackgroundColor` et `NormalTextColor` instances, définies au niveau de l’application, sont remplacées par les `PageBackgroundColor` et `NormalTextColor` instances définies au niveau de la page. Par conséquent, la couleur d’arrière-plan de page devient bleue et le texte dans la page devient jaune, comme indiqué dans les captures d’écran suivantes :

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Substitution de ressources de ResourceDictionary")](resource-dictionaries-images/overridding-screenshots.png#lightbox "substitution ResourceDictionary ressources")

Toutefois, notez que la barre de l’arrière-plan de la [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) est toujours jaune, car le [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) est définie sur la valeur de la `PageBackgroundColor` défini dans l’application de ressource niveau [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).

## <a name="merged-resource-dictionaries"></a>Dictionnaires de ressources fusionnés

Les dictionnaires de ressources combinent un ou plusieurs [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) instances dans un autre. Cela est accompli en définissant le `ResourceDictionary.MergedDictionaries` propriété à un ou plusieurs dictionnaires de ressources qui seront fusionnées dans l’application, une page ou un niveau de contrôle `ResourceDictionary`.

> [!IMPORTANT]
> Le [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) type a également un [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) propriété qui peut être utilisée pour fusionner un seul `ResourceDictionary` dans un autre, avec le `ResourceDictionary` spécifié en tant que la valeur de la `MergedWith`propriété en cours de fusion dans la zone actuelle `ResourceDictionary` instance. Lors de la fusion la `MergedWith` propriété, toutes les ressources en cours `ResourceDictionary` ce partage `x:Key` des ressources dans les valeurs d’attribut du `ResourceDictionary` devant être fusionnées, seront remplacée. Toutefois, le `MergedWith` propriété sera supprimée dans une future version de Xamarin.Forms. Par conséquent, il est recommandé d’utiliser le `MergedDictionaries` propriété fusionner `ResourceDictionary` instances.

L’exemple de code XAML suivant montre un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) nommé `MyResourceDictionary` qui contient une seule ressource :

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` peuvent être fusionnés dans n’importe quel application, une page ou un niveau de contrôle `ResourceDictionary`. L’exemple de code XAML suivant montre qu’il est en cours de fusion dans le niveau de la page `ResourceDictionary` à l’aide de la `MergedDictionaries` propriété :

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Lors de la fusion [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ressources de partage identiques `x:Key` valeurs d’attribut, Xamarin.Forms utilise la priorité de ressource suivant :

1. Les ressources locales pour le dictionnaire de ressources.
1. Les ressources contenues dans le dictionnaire de ressources qui a été fusionné via la [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) propriété.
1. Les ressources contenues dans les dictionnaires de ressources qui ont été fusionnées via la `MergedDictionaries` collection, dans l’ordre d’apparition dans le `MergedDictionaries` propriété.

> [!NOTE]
> Recherche les dictionnaires de ressources peut être une tâche nécessitant si une application contient plusieurs dictionnaires de ressources volumineux. Par conséquent, assurez-vous que chaque page dans une application utilise uniquement les dictionnaires de ressources qui sont appropriées à la page, pour éviter des recherches superflues.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment créer et utiliser un [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)et comment fusionner les dictionnaires de ressources. A `ResourceDictionary` permet aux ressources d’être définis dans un emplacement unique et réutilisées dans une application de Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [Dictionnaires de ressources (exemple)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Styles](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
