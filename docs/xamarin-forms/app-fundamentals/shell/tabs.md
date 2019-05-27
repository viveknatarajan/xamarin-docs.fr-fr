---
title: Disposition Shell Xamarin.Forms
description: Le niveau de navigation suivant le menu volant dans une application Shell est la barre d’onglets du bas. Lorsqu’un onglet contient plusieurs pages, il est possible de passer de l’une à l’autre grâce aux onglets du haut.
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a8da1e96bbdf51899b1780265933402da791a03e
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005161"
---
# <a name="xamarinforms-shell-tabs"></a>Onglets Shell Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Le niveau de navigation suivant le menu volant dans une application Shell est la barre d’onglets du bas. Lorsque le menu volant est fermé, la barre d’onglets du bas est considérée comme le plus haut niveau de navigation.

Chaque objet `FlyoutItem` peut contenir un ou plusieurs objets `Tab`, chaque objet `Tab` représentant un onglet dans la barre d'onglets inférieure. Chaque objet `Tab` peut contenir un ou plusieurs objets `ShellContent`, et chaque objet `ShellContent` affichera un seul [`ContentPage`](xref:Xamarin.Forms.ContentPage). Lorsque plusieurs objets `ShellContent` sont présents dans un objet `Tab`, il est possible de naviguer entre les objets `ContentPage` grâce aux onglets du haut.

Dans chaque objet `ContentPage`, des objets `ContentPage` supplémentaires sont accessibles. Pour plus d’informations sur la navigation, voir [Navigation Shell Xamarin.Forms](navigation.md).

## <a name="single-page-application"></a>Application monopage

L’application Shell la plus simple est une application monopage, créée en ajoutant un seul objet `Tab` à un objet `FlyoutItem`. Dans l’objet `Tab`, un objet `ShellContent` doit être défini sur un objet [`ContentPage`](xref:Xamarin.Forms.ContentPage) :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

Cet exemple de code génère l’application monopage suivante :

[![Capture d’écran d’une application Shell monopage sur iOS et Android](tabs-images/single-page-app.png "Application Shell monopage")](tabs-images/single-page-app-large.png#lightbox "Application Shell monopage")

Un menu volant n’est pas nécessaire dans une application monopage ; par conséquent, la propriété `Shell.FlyoutBehavior` est définie sur `Disabled`.

> [!NOTE]
> Pour masquer la barre de navigation, si nécessaire, définissez la propriété jointe `Shell.NavBarIsVisible` sur `false` sur l’objet [`ContentPage`](xref:Xamarin.Forms.ContentPage).

Shell comporte des opérateurs de conversion implicite qui permettent de simplifier la hiérarchie visuelle de Shell sans introduire de vues supplémentaires dans l’arborescence d’éléments visuels. En effet, un objet `Shell` sous-classé ne peut contenir que des objets `FlyoutItem`, qui ne peuvent contenir que des objets `Tab`, qui ne peuvent contenir que des objets `ShellContent`. Ces opérateurs de conversion implicite peuvent permettre de supprimer les objets `FlyoutItem`, `Tab` et `ShellContent` de l’exemple précédent :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

Cette conversion implicite encapsule automatiquement l’objet [`ContentPage`](xref:Xamarin.Forms.ContentPage) dans un objet `ShellContent`, qui est encapsulé dans un objet `Tab`, lui-même encapsulé dans un objet `FlyoutItem`.

> [!IMPORTANT]
> Dans une application Shell, tous les [`ContentPage`](xref:Xamarin.Forms.ContentPage) enfants d’un objet `ShellContent` sont créés au démarrage de l’application. L’ajout d’autres objets `ShellContent` avec cette approche crée des pages supplémentaires au démarrage de l’application, ce qui peut nuire à l’expérience de démarrage. Toutefois, Shell est également capable de créer des pages à la demande en réponse à la navigation. Pour plus d’informations, voir [Chargement efficace des pages](tabs.md#efficient-page-loading).

## <a name="bottom-tabs"></a>Onglets du bas

Les objets `Tab` sont rendus sous forme d'onglets inférieurs, à condition qu'il y ait plusieurs objets `Tab` dans un seul objet `FlyoutItem`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

Le titre et l’icône des onglets, définis sur chaque objet `Tab`, s’affichent dans les onglets du bas :

[![Capture d’écran d’une application Shell comportant deux pages avec onglets en bas sur iOS et Android](tabs-images/two-page-app-bottom-tabs.png "Application Shell comportant deux pages avec onglets en bas")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "Application Shell comportant deux pages avec onglets en bas")

Les opérateurs de conversion implicite de Shell peuvent sinon permettre de supprimer les objets `ShellContent` et `Tab` de l’exemple précédent :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </FlyoutItem>
</Shell>
```

Cette conversion implicite encapsule automatiquement chaque objet [`ContentPage`](xref:Xamarin.Forms.ContentPage) dans un objet `ShellContent`, les deux étant ensuite encapsulés dans un objet `Tab`.

> [!IMPORTANT]
> Dans une application Shell, tous les [`ContentPage`](xref:Xamarin.Forms.ContentPage) enfants d’un objet `ShellContent` sont créés au démarrage de l’application. L’ajout d’autres objets `ShellContent` avec cette approche crée des pages supplémentaires au démarrage de l’application, ce qui peut nuire à l’expérience de démarrage. Toutefois, Shell est également capable de créer des pages à la demande en réponse à la navigation. Pour plus d’informations, voir [Chargement efficace des pages](tabs.md#efficient-page-loading).

### <a name="tab-class"></a>Classe Tab

La classe `Tab` comporte différentes propriétés qui contrôlent l’apparence et le comportement des onglets :

- `CurrentItem`, de type `ShellContent`, représente l’élément sélectionné.
- `FlyoutDisplayOptions`, de type `FlyoutDisplayOptions`, définit la façon dont l’élément et ses enfants s’affichent dans le menu volant. La valeur par défaut est `AsSingleItem`.
- `FlyoutIcon`, de type `ImageSource`, définit l’icône à afficher dans le menu volant.
- `Icon`, de type `ImageSource`, définit l’icône à afficher dans les parties du chrome autres que le menu volant.
- `IsChecked`, de type `boolean`, détermine si l’élément est actuellement en surbrillance dans le menu volant.
- `IsEnabled`, de type `boolean`, détermine si l’élément est sélectionnable dans le chrome.
- `Items`, de type `ShellContentCollection`, définit tout le contenu d’un `Tab`.
- `Title`, de type `string`, représente le titre à afficher sur l’onglet dans l’interface utilisateur.

## <a name="shell-content"></a>Contenu de Shell

L’enfant de chaque objet `Tab` est un objet `ShellContent`, dont la propriété `Content` est définie sur une [`ContentPage`](xref:Xamarin.Forms.ContentPage) :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

Dans chaque objet `ContentPage`, des objets `ContentPage` supplémentaires sont accessibles. Pour plus d’informations sur la navigation, voir [Navigation Shell Xamarin.Forms](navigation.md).

### <a name="shellcontent-class"></a>Classe ShellContent

La classe `ShellContent` comporte différentes propriétés qui contrôlent l’apparence et le comportement du contenu des onglets :

- `Content`, de type `object`, représente le contenu de `ShellContent`.
- `ContentTemplate`, de type `DataTemplate`, représente le modèle servant à grossir dynamiquement le contenu de `ShellContent`.
- `FlyoutIcon`, de type `ImageSource`, définit l’icône à afficher dans le menu volant.
- `Icon`, de type `ImageSource`, définit l’icône à afficher dans les parties du chrome autres que le menu volant.
- `IsChecked`, de type `boolean`, détermine si l’élément est actuellement en surbrillance dans le menu volant.
- `IsEnabled`, de type `boolean`, détermine si l’élément est sélectionnable dans le chrome.
- `MenuItems`, de type `MenuItemCollection`, représente les éléments de menu à afficher dans le menu volant lorsque ce `ShellContent` est la page présentée.
- `Title`, de type `string`, représente le titre à afficher dans l’interface utilisateur.

Toutes ces propriétés s’appuient sur des objets [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), ce qui signifie qu’elles peuvent être des cibles de liaisons de données.

## <a name="bottom-and-top-tabs"></a>Onglets du haut et du bas

Lorsque plusieurs objets `ShellContent` sont présents dans un objet `Tab`, une barre d’onglets supérieure est ajoutée à l’onglet du bas, qui permet de naviguer entre les objets `ContentPage` :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

En résulte la disposition des captures d’écran suivantes :

[![Capture d’écran d’une application Shell comportant deux pages avec onglets en haut et en bas sur iOS et Android](tabs-images/two-page-app-top-tabs.png "Application Shell comportant deux pages avec onglets en haut et en bas")](tabs-images/two-page-app-top-tabs-large.png#lightbox "Application Shell comportant deux pages avec onglets en haut et en bas")

Les opérateurs de conversion implicite de Shell peuvent sinon permettre de supprimer les objets `ShellContent` et le deuxième objet `Tab` de l’exemple précédent :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </FlyoutItem>
</Shell>
```

Cette conversion implicite encapsule automatiquement `MonkeysPage` dans un objet `ShellContent`, qui est ensuite encapsulé dans un objet `Tab`. En outre, `CatsPage` et `DogsPage` sont implicitement encapsulées dans des objets `ShellContent`.

## <a name="efficient-page-loading"></a>Chargement efficace des pages

Dans une application Shell, chaque objet [`ContentPage`](xref:Xamarin.Forms.ContentPage) d’un objet `ShellContent` est créé au lancement de l’application, ce qui risque de donner une mauvaise expérience de démarrage. Cependant, Shell permet également de créer des pages à la demande, en réponse à la navigation. Pour cela, utilisez l’extension de balisage `DataTemplate` pour convertir chaque `ContentPage` en [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), puis définissez le résultat sous forme de valeur de propriété `ShellContent.ContentTemplate` :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">    
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
    </FlyoutItem>    
</Shell>
```

Ce XAML crée et affiche `CatsPage`, car il s’agit du premier élément de contenu déclaré dans l’objet `Shell` sous-classé. Les pages `CatsPage` et `MonkeysPage`, accessibles par les onglets du bas, ne sont créées que lorsque l’utilisateur y accède. Cette approche présente l’avantage d’éviter une mauvaise expérience de démarrage, car les pages sont créées à la demande en réponse à la navigation, plutôt qu’au lancement de l’application.

## <a name="tab-appearance"></a>Apparence des onglets

La classe `Shell` comporte différentes propriétés qui contrôlent l’apparence des onglets :

- `TabBarBackgroundColor`, de type `Color`, représente une propriété jointe qui définit la couleur d’arrière-plan de la barre d’onglets. Si la propriété n’est pas définie, la valeur de propriété `BackgroundColor` est utilisée.
- `TabBarDisabledColor`, de type `Color`, représente une propriété jointe qui définit la couleur de la barre d’onglets lorsqu’elle est désactivée. Si la propriété n’est pas définie, la valeur de propriété `DisabledColor` est utilisée.
- `TabBarForegroundColor`, de type `Color`, représente une propriété jointe qui définit la couleur de premier plan de la barre d’onglets. Si la propriété n’est pas définie, la valeur de propriété `ForegroundColor` est utilisée.
- `TabBarTitleColor`, de type `Color`, représente une propriété jointe qui définit la couleur de titre de la barre d’onglets. Si la propriété n’est pas définie, la valeur de propriété `TitleColor` est utilisée.
- `TabBarUnselectedColor`, de type `Color`, représente une propriété jointe qui définit la couleur de la barre d’onglets lorsqu’elle n’est pas sélectionnée. Si la propriété n’est pas définie, la valeur de propriété `UnselectedColor` est utilisée.

Toutes ces propriétés s’appuient sur des objets [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), ce qui signifie qu’elles peuvent être des cibles de liaisons de données.

Par conséquent, les styles XAML peuvent servir à définir l’apparence des onglets. L’exemple suivant en montre un qui définit différentes propriétés de couleur des onglets :

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

Il est également possible de styliser les onglets avec des feuilles de style en cascade (CSS). Pour plus d’informations, voir [Propriétés spécifiques Shell Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="related-links"></a>Liens connexes

- [Xaminals (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Navigation Shell Xamarin.Forms](navigation.md)
- [Propriétés spécifiques Shell Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
