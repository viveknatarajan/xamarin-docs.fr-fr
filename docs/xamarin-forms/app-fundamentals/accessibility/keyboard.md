---
title: Navigation au clavier
description: Au lieu d’utiliser la séquence d’onglet par défaut, il est parfois nécessaire paramétrer votre interface utilisateur en spécifiant la séquence de l’onglet avec une combinaison des propriétés TabIndex et IsTapStop.
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: f703dff56d2947c35a9bc76e0eb909bfe9023bac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131165"
---
# <a name="keyboard-navigation-in-xamarinforms"></a>Navigation au clavier dans Xamarin.Forms

Certains utilisateurs peuvent avoir des difficultés à utiliser les applications qui ne fournissent pas les accès clavier appropriés. Vous spécifiez un ordre de tabulation des contrôles permet la navigation au clavier et prépare les pages d’application pour recevoir l’entrée dans un ordre particulier.

Par défaut, l’ordre de tabulation des contrôles est le même ordre dans lequel ils sont répertoriés dans le XAML ou ajoutés par programme à une collection d’enfants. Cet ordre est l’ordre dans lequel les contrôles naviguera via avec un clavier et l’ordre par défaut est souvent le meilleur ordre possible. Toutefois, l’ordre par défaut n’est pas toujours le même que l’ordre attendu, comme indiqué dans l’exemple de code XAML suivant :

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname" />
</Grid>
```

La capture d’écran suivante montre l’ordre de tabulation par défaut pour cet exemple de code :

![](keyboard-images/default-tab-order.png "Ordre par défaut basée sur la ligne")

L’ordre de tabulation est basé sur une ligne et est l’ordre que les contrôles sont répertoriés dans le XAML. Par conséquent, en appuyant sur la touche Tab parcourt prénom [ `Entry` ](xref:Xamarin.Forms.Entry) instances, suivis par nom de famille `Entry` instances. Toutefois, une expérience plus intuitive consisterait à utiliser une navigation par onglets de la colonne en premier, afin que l’utilisation de la touche Tab parcourt les paires nom-prénom. Cela est possible en spécifiant l’ordre de tabulation des contrôles d’entrée.

> [!NOTE]
> Sur la plateforme Windows universelle, les raccourcis clavier qui fournissent une manière intuitive permettant aux utilisateurs de naviguer rapidement et d’interagir avec l’interface utilisateur visible de l’application via un clavier au lieu de via tactile ou d’une souris peuvent être définies. Pour plus d’informations, consultez [clés d’accès de paramètre VisualElement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys).

## <a name="setting-the-tab-order"></a>Définition de l’ordre de tabulation

Le `VisualElement.TabIndex` propriété est utilisée pour indiquer l’ordre dans lequel [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) instances recevoir le focus lorsque l’utilisateur parcourt les contrôles en appuyant sur la touche Tab. La valeur par défaut de la propriété est 0, et il peut être définie sur any `int` valeur.

Les règles suivantes s’appliquent quand à l’aide de l’ordre de tabulation par défaut ou en définissant le `TabIndex` propriété :

 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances avec un `TabIndex` égale à 0 sont ajoutés à l’ordre de tabulation selon leur ordre de déclaration dans les collections XAML ou enfant.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances avec un `TabIndex` supérieure à 0 sont ajoutés à l’ordre de tabulation selon leur `TabIndex` valeur.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances avec un `TabIndex` inférieures à 0 sont ajoutés à l’ordre de tabulation et apparaître avant tout la valeur zéro.
 - Est en conflit sur une `TabIndex` sont résolus par ordre de déclaration.

Après avoir défini un ordre de tabulation, en appuyant sur la touche Tab se répète le focus via des contrôles dans l’ordre croissant `TabIndex` ordre, encapsuleur au début, une fois que le contrôle final est atteinte.

L’exemple XAML suivant montre le `TabIndex` propriété sur les contrôles d’entrée pour activer la navigation par onglets de la colonne en premier :

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="1" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="3" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="2" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="4" />
</Grid>
```

La capture d’écran suivante montre l’ordre de tabulation pour cet exemple de code :

![](keyboard-images/correct-tab-order.png "Ordre de tabulation en colonnes")

L’ordre de tabulation est basée sur une colonne. Par conséquent, en appuyant sur la touche Tab parcourt prénom-surname [ `Entry` ](xref:Xamarin.Forms.Entry) paires.

## <a name="excluding-controls-from-the-tab-order"></a>À l’exclusion des contrôles à partir de l’ordre de tabulation

Outre la définition de l’ordre de tabulation des contrôles, il peut être nécessaire d’exclure les contrôles de l’ordre de tabulation. Ceci est une façon de réaliser en définissant le [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement) propriété des contrôles à `false`, car les contrôles désactivés sont exclus de l’ordre de tabulation.

Toutefois, il peut être nécessaire d’exclure les contrôles de l’ordre de tabulation même lorsqu’ils ne sont pas désactivés. Cela peut être obtenue avec la `VisualElement.IsTapStop` propriété, qui indique si un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) est inclus dans la navigation par onglets. Sa valeur par défaut est `true`, et lorsque sa valeur est `false` le contrôle est ignoré par l’infrastructure de navigation de l’onglet, indépendamment de si un `TabIndex` est définie.

## <a name="supported-controls"></a>Contrôles pris en charge

Le `TabIndex` et `IsTapStop` propriétés sont prises en charge sur les contrôles suivants, qui acceptent les entrées au clavier sur une ou plusieurs plateformes :

- [`Button`](xref:Xamarin.Forms.Button)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`Switch`](xref:Xamarin.Forms.Switch)
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

> [!NOTE]
> Chacun de ces contrôles n’est pas actif sur toutes les plateformes.

## <a name="related-links"></a>Liens associés

- [Accessibilité (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
