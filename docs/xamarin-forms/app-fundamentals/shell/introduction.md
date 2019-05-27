---
title: Présentation de Xamarin.Forms Shell
description: Xamarin.Forms Shell fournit les fonctionnalités fondamentales nécessaires à la plupart des applications, y compris une expérience utilisateur de navigation commune, un schéma de navigation basée sur des URI et un gestionnaire de recherche intégré.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: f6662f20485c6671edcb5a1654569cdd8498607e
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005208"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

Xamarin.Forms Shell réduit la complexité du développement d’applications mobiles en fournissant les fonctionnalités fondamentales nécessaires à la plupart des applications mobiles, y compris :

- Un emplacement unique pour décrire la hiérarchie visuelle d’une application.
- Une expérience utilisateur de navigation commune.
- Un schéma de navigation basée sur des URI permettant la navigation vers n’importe quelle page dans l’application.
- Un gestionnaire de recherche intégré.

En outre, les applications Shell bénéficient d’une vitesse de rendu accrue et d’une consommation de mémoire réduite.

> [!IMPORTANT]
> Xamarin.Forms Shell est uniquement disponible sur iOS et Android. Les applications iOS et Android existantes peuvent adopter Shell, bénéficiant immédiatement d’une navigation, de performances et d’une extensibilité améliorées.

## <a name="shell-navigation-experience"></a>Expérience de navigation Shell

Shell fournit une expérience de navigation robuste, basée sur des menus volants et des onglets. Le niveau de navigation supérieur dans une application Shell est un menu volant :

[![Capture d’écran d’un menu volant Shell sur iOS et Android](introduction-images/flyout.png "Menu volant Shell")](introduction-images/flyout-large.png#lightbox "Menu volant Shell")

La sélection d’un élément de menu volant permet de sélectionner et d’afficher cet élément dans un onglet en bas de l’écran :

[![Capture d’écran des onglets Shell inférieurs sur iOS et Android](introduction-images/monkeys.png "Onglets Shell inférieurs")](introduction-images/monkeys-large.png#lightbox "Onglets Shell inférieurs")

> [!NOTE]
> Lorsque le menu volant n’est pas ouvert, la barre d’onglets inférieure représente le niveau supérieur de la navigation dans l’application.

Chaque onglet affiche une [`ContentPage`](xref:Xamarin.Forms.ContentPage). Toutefois, si un onglet inférieur contient plusieurs pages, les pages sont accessibles via la barre d’onglets supérieure :

[![Capture d’écran des onglets Shell supérieurs sur iOS et Android](introduction-images/cats.png "Onglets Shell supérieurs")](introduction-images/cats-large.png#lightbox "Onglets Shell supérieurs")

Chaque onglet permet d’accéder à des objets [`ContentPage`](xref:Xamarin.Forms.ContentPage) supplémentaires :

[![Capture d’écran de la navigation dans une page Shell sur iOS et Android](introduction-images/cat-details.png "Navigation dans une application Shell")](introduction-images/cat-details-large.png#lightbox "Navigation dans une application Shell")

## <a name="subclassing-the-shell-class"></a>Sous-classement de la classe Shell

Un objet `Shell` sous-classé décrit la hiérarchie visuelle d’une application Shell et se compose de trois principaux objets hiérarchiques :

- `FlyoutItem`, qui représente un ou plusieurs éléments du menu volant. Chaque objet `FlyoutItem` est un enfant de l’objet `Shell`.
- `Tab`, qui représente le contenu regroupé, accessible via les onglets inférieurs. Chaque objet `Tab` est un enfant d’un objet `FlyoutItem`.
- `ShellContent`, qui représente les objets `ContentPage` dans votre application. Chaque objet `ShellContent` est un enfant d’un objet `Tab`. Lorsque plusieurs objets `ShellContent` sont présents dans un `Tab`, les objets sont accessibles via les onglets supérieurs.

Aucun de ces objets ne représente une interface utilisateur, mais plutôt l’organisation de la hiérarchie visuelle de l’application. Shell sélectionnera ces objets et générera l’interface utilisateur de navigation pour le contenu.

> [!NOTE]
> La classe `FlyoutItem` est un alias pour la classe `ShellItem`, et la classe `Tab` est un alias pour la classe `ShellSection`. Ces alias sont conçus pour rendre l’API plus conviviale à utiliser.

Le fichier XAML suivant montre un exemple d’objet `Shell` sous-classé :

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

Lorsqu’il est exécuté, ce fichier XAML affiche la `CatsPage` car il s’agit du premier élément de contenu déclaré dans l’objet `Shell` sous-classé :

[![Capture d’écran d’une application Shell sur iOS et Android](introduction-images/cats.png "Application Shell")](introduction-images/cats-large.png#lightbox "Application Shell")

Appuyez sur l’icône du menu latéral, ou balayez depuis la gauche, pour afficher le menu volant :

[![Capture d’écran d’un menu volant Shell sur iOS et Android](introduction-images/flyout-reduced.png "Menu volant Shell")](introduction-images/flyout-reduced-large.png#lightbox "Menu volant Shell")

> [!IMPORTANT]
> Dans une application Shell, chaque [`ContentPage`](xref:Xamarin.Forms.ContentPage) qui est un enfant d’un objet `ShellContent` est créé au démarrage de l’application. L’ajout d’autres objets `ShellContent` avec cette approche crée des pages supplémentaires au démarrage de l’application, ce qui peut nuire à l’expérience de démarrage. Mais Shell est également capable de créer des pages à la demande, en réponse à la navigation. Pour plus d’informations, consultez [Chargement efficace d’une page](tabs.md#efficient-page-loading) dans le guide [Onglets Shell Xamarin.Forms](tabs.md).

## <a name="bootstrapping-a-shell-application"></a>Amorçage d’une application Shell

Une application Shell est amorcée en définissant la propriété [`MainPage`](xref:Xamarin.Forms.Application.MainPage) de la classe `App` pour un objet `Shell` sous-classé :

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

Dans cet exemple, la classe `AppShell` est un fichier XAML dérivé de la classe `Shell`, qui décrit la hiérarchie visuelle de l’application.

## <a name="shell-appearance"></a>Apparence Shell

La classe `Shell` définit les propriétés suivantes, qui contrôlent l’apparence d’une application Shell :

- `BackgroundColor`, de type `Color` : propriété jointe qui définit la couleur d’arrière-plan du chrome Shell. La couleur n’apparaît pas derrière le contenu Shell.
- `DisabledColor`, de type `Color` : propriété jointe qui définit la couleur permettant d’ombrer le texte et les icônes désactivées.
- `ForegroundColor`, de type `Color` : propriété jointe qui définit la couleur permettant d’ombrer le texte et les icônes.
- `TitleColor`, de type `Color` : propriété jointe qui définit la couleur utilisée pour le titre de la page actuelle.
- `UnselectedColor`, de type `Color` : propriété jointe qui définit la couleur appliquée au texte et aux icônes désactivés dans le chrome Shell.

Toutes ces propriétés sont soutenues par des objets [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), ce qui signifie qu’elles peuvent être des cibles de liaisons de données.

En outre, ces propriétés peuvent être définies à l’aide de feuilles de style en cascade (CSS). Pour plus d’informations, consultez [Propriétés spécifiques de Xamarin.Forms Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="shell-content-layout"></a>Disposition du contenu Shell

La classe `Shell` définit les propriétés suivantes qui affectent la disposition du contenu de l’application Shell :

- `NavBarIsVisible`, de type `boolean` : propriété jointe qui définit si la barre de navigation doit être visible lorsqu’une page s’affiche. Cette propriété doit être définie sur une page, et sa valeur par défaut est `true`.
- `TabBarIsVisible`, de type `bool` : propriété jointe qui définit si la barre d’onglets doit être visible lorsqu’une page s’affiche. Cette propriété doit être définie sur une page, et sa valeur par défaut est `true`.
- `TitleView`, de type `View` : propriété jointe qui définit l’élément `TitleView` d’une page. Cette propriété doit être définie sur une page.

Toutes ces propriétés sont soutenues par des objets [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), ce qui signifie qu’elles peuvent être des cibles de liaisons de données.

## <a name="related-links"></a>Liens connexes

- [Xaminals (exemple)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Propriétés spécifiques de Xamarin.Forms Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
