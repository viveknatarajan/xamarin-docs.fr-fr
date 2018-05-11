---
title: XAML Standard (version préliminaire)
description: Comment commencer à Explorer l’aperçu Standard XAML dans Xamarin.Forms
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: b16d146c5ad1097f38c41763a3ae111e7439256f
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="xaml-standard-preview"></a>XAML Standard (version préliminaire)

![Preview](~/media/shared/preview.png)

Suivez ces étapes pour faire des essais avec XAML Standard dans Xamarin.Forms :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Téléchargez le [afficher un aperçu du package NuGet ici](https://aka.ms/xf-xamlstandard-nuget).
2. Ajouter le **Xamarin.Forms.Alias** package NuGet pour vos projets de plateforme et de Xamarin.Forms .NET Standard.
3. Initialiser le package avec `Alias.Init()`
4. Ajouter un `xmlns:a` référence `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Utiliser les types dans XAML, voir la [contrôle référence](controls.md) pour plus d’informations.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Téléchargez le [afficher un aperçu du package NuGet ici](https://aka.ms/xf-xamlstandard-nuget).
2. Ajouter le **Xamarin.Forms.Alias** package NuGet pour vos projets de plateforme et de Xamarin.Forms .NET Standard.
3. Initialiser le package avec `Alias.Init()`
4. Ajouter un `xmlns:a` référence `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Utiliser les types dans XAML, voir la [contrôle référence](controls.md) pour plus d’informations.

-----

Le code XAML suivant illustre certains contrôles XAML Standard qui sont utilisés dans un Xamarin.Forms `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Exiger le xmlns `a:` préfixe sur les contrôles XAML Standard est une limitation de la version préliminaire actuelle.


## <a name="related-links"></a>Liens associés

- [Aperçu NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Informations de référence sur les contrôles](controls.md)
