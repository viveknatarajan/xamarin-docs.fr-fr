---
title: XAML Standard (version préliminaire)
description: Cet article explique comment commencer à Explorer la préversion Standard de XAML dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
ms.openlocfilehash: d9fb19fb233c25e5dd525c7c157013ef66f4a4f3
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51562755"
---
# <a name="xaml-standard-preview"></a>XAML Standard (version préliminaire)

![Preview](~/media/shared/preview.png)

Suivez ces étapes pour faire des essais avec XAML Standard dans Xamarin.Forms :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Téléchargez le [afficher un aperçu du package NuGet ici](https://aka.ms/xf-xamlstandard-nuget).
2. Ajouter le **Xamarin.Forms.Alias** package NuGet à vos projets Xamarin.Forms .NET Standard et de plateforme.
3. Initialiser le package avec `Alias.Init()`
4. Ajouter un `xmlns:a` référence `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Utiliser les types dans XAML, consultez la [contrôle référence](controls.md) pour plus d’informations.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Téléchargez le [afficher un aperçu du package NuGet ici](https://aka.ms/xf-xamlstandard-nuget).
2. Ajouter le **Xamarin.Forms.Alias** package NuGet à vos projets Xamarin.Forms .NET Standard et de plateforme.
3. Initialiser le package avec `Alias.Init()`
4. Ajouter un `xmlns:a` référence `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Utiliser les types dans XAML, consultez la [contrôle référence](controls.md) pour plus d’informations.

-----

Le XAML suivant illustre certains contrôles XAML Standard qui sont utilisés dans un Xamarin.Forms `ContentPage`:

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
