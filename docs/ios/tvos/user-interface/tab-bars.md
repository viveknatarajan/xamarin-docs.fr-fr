---
title: "Fonctionne avec le contrôleur de la barre d’onglet"
description: "Cet article décrit la conception et fonctionne avec le contrôleur de barre d’onglet à l’intérieur d’une application Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 37c0b3f1f826893bef5c1a7ffb5d422c1c8c5c78
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tab-bar-controller"></a>Fonctionne avec le contrôleur de la barre d’onglet

_Cet article décrit la conception et fonctionne avec le contrôleur de barre d’onglet à l’intérieur d’une application Xamarin.tvOS._

Pour de nombreux types d’applications de tvOS, navigation principale est présentée comme une barre d’onglets dans la partie supérieure de l’écran. L’utilisateur fait glisser sa gauche et droite sur la liste des catégories possibles et la zone de contenu ci-dessous les modifications pour refléter la sélection de l’utilisateur.

[ ![](tab-bars-images/tab01.png "Exemple de barre d’onglet")](tab-bars-images/tab01.png)

La barre d’onglet est transparente par défaut et apparaît toujours en haut de l’écran. Dans le focus, une barre d’onglets couvrent 140 pixels supérieur de l’écran, mais sera rapidement diapositive immédiatement lors de la sélection se déplace vers la zone de contenu ci-dessous.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>Onglet Barres tvOS

Le `UITabViewController` fonctionne de manière similaire et a un objectif semblable sur tvOS comme il le fait dans iOS, avec les différences clés suivantes :

- Contrairement à la barre d’onglet sur iOS qui apparaît au bas de l’écran, onglet Barres tvOS occuper 140 pixels supérieur de l’écran et sont translucides par défaut.
- Lorsque le focus quitte la barre d’onglet pour la zone de contenu ci-dessous, la barre d’onglet glisser rapidement la partie supérieure de l’écran et être masquée. L’utilisateur peut appuyez une fois sur le bouton de Menu ou un glissement sur le [Siri distant](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) pour afficher la barre d’onglets.
- Le glissement vers le bas sur l’instance distante Siri pour déplacer le focus vers la zone de contenu sous la barre d’onglet pour la première [élément peut être actif](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) dans le contenu affiché. Là encore, cela permet de masquer la barre d’onglets une fois que le focus se déplace.
- Cliquer pour sélectionner une catégorie affichée dans la barre d’onglet basculent à qui le contenu et le focus de la catégorie va passer au premier élément dans cette vue peut être actif.
- Le nombre de catégories affichées dans la barre d’onglet doit être corrigé et toutes les catégories doivent être accessibles à tout moment, une catégorie donnée ne doit jamais être désactivée.
- Barres de l’onglet ne prennent pas en charge personnalisation sur tvOS. En outre, elles ne présentent pas la **plus** catégorie (par exemple, iOS) s’il existe plusieurs catégories à peut être contenue dans la barre d’onglets.

Apple a les suggestions suivantes pour travailler avec des barres de l’onglet :

- **Utilisez les barres d’onglet au contenu à organiser logiquement** -utiliser la barre d’onglets à organiser logiquement le contenu associé à votre application tvOS. Par exemple, par défaut, les graphiques en haut, acheté et la recherche.
- **Ajouter les Badges à informer les utilisateurs du nouveau contenu** -vous pouvez éventuellement afficher un Badge (une ellipse rouge avec un nombre blanc ou un point d’exclamation) pour informer l’utilisateur de nouveau contenu dans une catégorie.
- **Utilisez Badges avec parcimonie** - ne pas surcharger la barre d’onglet avec des Badges et afficher uniquement les où ils fournissent des informations critiques à l’utilisateur.
- **Limiter le nombre de catégories** - pour réduire la complexité et que vous application reste gérable, ne surcharge votre barre d’onglet avec les catégories et vous assurer que toutes les catégories sont visibles et non encombré. Titres simples, short fonctionnent le mieux.
- **Ne désactivez pas une catégorie** -tous les onglets (catégories) doit toujours être visibles et activés à tout moment. Si un onglet donné n’a aucun contenu, fournissent une explication à l’utilisateur pourquoi. Par exemple, l’onglet achats sera vide si l’utilisateur n’a effectué aucune des achats.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>Éléments de la barre d’onglet

Chaque catégorie (onglet) dans la barre d’onglet est représentée par un élément de barre d’onglet (`UITabBarItem`). Apple a les suggestions suivantes pour travailler avec des éléments de barre d’onglet :

- **Utilisez les onglets en fonction de texte** -tandis que l’onglet barre d’élément ne peut être représentée sous forme d’icône, Apple suggère à l’aide du texte uniquement, car un titre concis est plus facile à interpréter qu’une icône.
- **Utilisez courte, les noms explicites ou les verbes** -un élément de barre d’onglet doit relais clairement le contenu qu’il contienne et fonctionne mieux lorsqu’il est un nom simple (par exemple, les Photos, musique ou des vidéos) ou des verbes (tels que la recherche ou Play).

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>Onglet Barres et les animations

Pour travailler avec des barres de l’onglet dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
1. Démarrez une nouvelle application Xamarin.tvOS et sélectionnez **tvOS** > **application** > **avec onglet l’application**: 

    [ ![](tab-bars-images/tab02.png "Sélectionnez une application avec onglets")](tab-bars-images/tab02.png)
1. Suivez toutes les invites pour créer une nouvelle Xamarin.tvOS Solution.
1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Pour modifier la **icône** ou **titre** pour une catégorie donnée, sélectionnez le **élément de barre d’onglet** pour le **View Controller** dans le  **Structure du document**:

    [ ![](tab-bars-images/tab03a.png "Élément de la barre d’onglet pour le contrôleur de la vue dans la structure du Document")](tab-bars-images/tab03a.png)
1. Puis définissez les propriétés requises dans le **onglet Widget** de la **l’Explorateur de propriétés**: 

    [ ![](tab-bars-images/tab03.png "L’onglet du Widget")](tab-bars-images/tab03.png)
1. Pour ajouter une nouvelle catégorie (onglet), déposez une **View Controller** votre aire de conception : 

    [ ![](tab-bars-images/tab04.png "Un contrôleur de vue")](tab-bars-images/tab04.png)
1. Cliquez sur le contrôle et faites glisser à partir de la **onglet-View-Controller** au nouveau **View Controller**.
1. Dans le menu contextuel, sélectionnez **afficher les contrôleurs** pour ajouter la nouvelle vue sous forme d’onglet (catégorie) : 

    [ ![](tab-bars-images/tab05.png "Sélectionnez l’onglet")](tab-bars-images/tab05.png)
1. Concevoir la disposition de l’interface utilisateur pour chaque zone de contenu Caterogies comme d’habitude, en ajoutant des éléments d’interface utilisateur dans le concepteur iOS.
1. Exposer tous les événements requis pour travailler avec les contrôles d’IU dans le code c#.
1. Nommez les contrôles d’interface utilisateur que vous souhaitez exposer dans le code c#.
1. Enregistrez les modifications apportées.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Démarrez une nouvelle application Xamarin.tvOS et sélectionnez **tvOS** > **application** > **avec onglet l’application**: 

    [ ![](tab-bars-images/tab02vs.png "Sélectionnez une application avec onglets")](tab-bars-images/tab02vs.png)
1. Suivez toutes les invites pour créer une nouvelle Xamarin.tvOS Solution.
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Pour modifier la **icône** ou **titre** pour une catégorie donnée, sélectionnez le **élément de barre d’onglet** pour le **View Controller** dans le  **Structure du document**:

    [ ![](tab-bars-images/tab03avs.png "Le contrôleur de la vue dans la structure du Document")](tab-bars-images/tab03avs.png)
1. Puis définissez les propriétés requises dans le **onglet Widget** de la **l’Explorateur de propriétés**: 

    [ ![](tab-bars-images/tab03vs.png "L’onglet du Widget")](tab-bars-images/tab03vs.png)
1. Pour ajouter une nouvelle catégorie (onglet), faites glisser un **View Controller** à partir de la **boîte à outils** et déposez-le sur votre surface de conception : 

    [ ![](tab-bars-images/tab04vs.png "Un contrôleur de vue")](tab-bars-images/tab04vs.png)
1. Cliquez sur le contrôle et faites glisser à partir de la **onglet-View-Controller** au nouveau **View Controller**.
1. Dans le menu contextuel, sélectionnez **afficher les contrôleurs** pour ajouter la nouvelle vue sous forme d’onglet (catégorie) : 

    [ ![](tab-bars-images/tab05vs.png "Sélectionnez l’onglet")](tab-bars-images/tab05vs.png)
1. Concevoir la disposition de l’interface utilisateur pour chaque zone de contenu Caterogies comme d’habitude, en ajoutant des éléments d’interface utilisateur dans le Concepteur d’iOS.
1. Exposer tous les événements requis pour travailler avec les contrôles d’IU dans le code c#.
1. Nommez les contrôles d’interface utilisateur que vous souhaitez exposer dans le code c#.
1. Enregistrez les modifications apportées.
    
-----

> [!IMPORTANT]
> **Remarque :** alors qu’il est possible d’attribuer des événements tels que `TouchUpInside` à un élément d’interface utilisateur (comme un `UIButton`) dans le Concepteur d’iOS, il ne sera jamais appelée car Apple TV n’a pas une fonction tactile d’écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser le `Primary Action ` événement lors de la création de gestionnaires d’événements de tvOS éléments d’interface utilisateur.

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>Utilisation des barres de l’onglet

Utilisez le `Items` propriété de la `UITabBar` pour accéder à la collection de `UITabBarItems` qu’il contient comme un tableau indexé zéro (0). Le `SelectedItem` propriété retournera l’onglet actuellement sélectionné (catégorie) comme un `UITabBarItem`.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>Utilisation des éléments de barre d’onglet

Pour afficher un Badge sous un onglet donné (une ellipse rouge avec un texte blanc), utilisez le code suivant :

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

Ce qui produit les résultats suivants lorsque vous exécutez :

[ ![](tab-bars-images/tab06.png "Un élément de barre d’onglet avec badge")](tab-bars-images/tab06.png)

Utilisez le `Title` propriété de la `UITabBarItem` pour modifier le titre et le `Image` propriété à modifier l’icône.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert conception et fonctionne avec le contrôleur de barre d’onglet à l’intérieur d’une application Xamarin.tvOS.




## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
