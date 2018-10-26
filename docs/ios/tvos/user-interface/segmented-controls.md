---
title: Utilisation de contrôles segmentés de tvOS dans Xamarin
description: Ce document décrit comment utiliser des contrôles segmentés dans une application conçue avec Xamarin tvOS. Il aborde les icônes de segment et le texte, événements, modification de l’apparence du contrôle et bien plus encore.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 98a770d05014e0498b805ed9ffa0c84314efc765
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107113"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Utilisation de contrôles segmentés de tvOS dans Xamarin

Un contrôle segmenté fournit un ensemble d’éléments linéaires, chacun d’eux peut contenir une icône ou du texte et est utilisé pour fournir un ensemble de choix associée à l’utilisateur.

[![](segmented-controls-images/segment01.png "Exemples de contrôles de segment")](segmented-controls-images/segment01.png#lightbox)

Apple a les suggestions suivantes pour l’utilisation des contrôles segmentés :

- **Fournir suffisamment d’espace** -soins doit être prise pour fournir suffisamment d’espace entre les autres [peut recevoir le focus des éléments](~/ios/tvos/app-fundamentals/navigation-focus.md) et un contrôle segmenté. Un Segment individuel devienne sélectionné lorsqu’il est actif (pas lorsque vous cliquez dessus) et l’utilisateur peut modifier accidentellement des segments lorsqu’ils veulent en fait de sélectionner un autre élément pouvant prendre le focus sur le segment actuel.
- **Utiliser des affichages fractionnés pour le filtrage de contenu** -contrôles segmentés ne pas faire de bons choix pour que les affichages fractionnés ont été conçus pour faciliter la navigation entre le contenu et les filtres de filtrage de contenu.
- **Limite au nombre maximal de Segments sept** -vous devez essayer de conserver le nombre maximal de segments ci-dessous huit (8), comme cela est plus facile d’analyse à partir de la salle sur le canapé et plus facile de naviguer.
- **Utiliser la taille du contenu de Segment cohérente** : tous les Segments ont la même largeur et, si possible, il est conseillé de conserver le contenu dans chaque segment de la même taille. Cela n’est pas seulement rend les contrôles de Segment plus agréable, mais rend plus facile à lire en un clin de œil.
- **Éviter le mélange des icônes et du texte** -chaque étape peut contenir une icône ou texte, mais pas les deux. Bien qu’il soit possible de combiner des icônes et du texte dans le même contrôle segmenté, cela doit être évité.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>À propos des icônes de Segment

Apple suggère l’utilisation d’images simples, reconnaissables pour les icônes de Segment, par exemple une loupe pour la recherche. Les icônes trop complexes sont difficiles à reconnaître sur un écran de télévision à travers la pièce, il est donc préférable de limiter vos icônes aux représentations simples.

Vous ne pouvez pas mélanger le texte et les icônes sur un Segment donné et vous devez éviter de mélanger des icônes et du texte dans un seul contrôle segmenté. Il doit être toutes les icônes ou tout le texte.

<a name="Summary" />

## <a name="segment-text"></a>Texte de segment

Apple rend les suggestions suivantes pour travailler avec du texte de Segment :

- **Utilisez faire court, des noms significatifs** -titre du Segment le doit indiquer clairement le type de contenu que l’utilisateur doit attendre lors de la sélection du Segment donné. Par exemple : musique ou des vidéos.
- **Utiliser la mise en majuscules des mots en majuscule** -chaque mot du titre de Segments doit être en majuscule à l’exception des articles, des conjonctions et prépositions de moins de quatre (4) caractères.
- **Utiliser courte, titres concentré** -conserver les titres, à courts et ayant le focus sur le type de contenu attendu lorsque le Segment est sélectionné.

Là encore, vous ne pouvez pas mélanger le texte et les icônes sur un Segment donné, et vous devez éviter de mélanger des icônes et du texte dans un seul contrôle segmenté.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Contrôles de segment et Storyboards

Pour travailler avec des contrôles de Segment dans une application Xamarin.tvOS le plus simple consiste à les ajouter à l’interface utilisateur de l’application à l’aide du concepteur iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Dans le **panneau solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Faites glisser un **Segment contrôle** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](segmented-controls-images/segment02.png "Un contrôle de Segment")](segmented-controls-images/segment02.png#lightbox)
1. Dans le **onglet Widget** de la **remplissage de la propriété**, vous pouvez ajuster plusieurs propriétés du contrôle Segment telles que son **Style** et **état**: 

    [![](segmented-controls-images/segment03.png "L’onglet de Widget")](segmented-controls-images/segment03.png#lightbox)
1. Utilisez le **Segments** champ pour contrôler le nombre de Segments dans le contrôleur.
1. Sélectionnez un Segment donné à partir de la **Segment déroulante** ajuster ses propriétés individuelles telles que **titre** ou **Image** et de contrôler si un Segment donné est  **Activé** ou **sélectionnés** lorsque le contrôle est affiché.
1. Enfin, attribuez **noms** aux contrôles afin que vous pouvez y répondre dans C# code. Exemple : 

    [![](segmented-controls-images/segment04.png "Attribuer un nom")](segmented-controls-images/segment04.png#lightbox)
1. Enregistrez les modifications apportées.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Faites glisser un **Segment contrôle** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](segmented-controls-images/segment02-vs.png "Un contrôle de Segment")](segmented-controls-images/segment02-vs.png#lightbox)
1. Dans le **onglet Widget** de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du contrôle Segment telles que son **Style** et **état**: 

    [![](segmented-controls-images/segment03-vs.png "L’onglet de Widget")](segmented-controls-images/segment03-vs.png#lightbox)
1. Utilisez le **Segments** champ pour contrôler le nombre de Segments dans le contrôleur.
1. Sélectionnez un Segment donné à partir de la **Segment déroulante** ajuster ses propriétés individuelles telles que **titre** ou **Image** et de contrôler si un Segment donné est  **Activé** ou **sélectionnés** lorsque le contrôle est affiché.
1. Enfin, attribuez **noms** aux contrôles afin que vous pouvez y répondre dans C# code. Exemple : 

    [![](segmented-controls-images/segment04-vs.png "Attribuer un nom")](segmented-controls-images/segment04-vs.png#lightbox)
1. Enregistrez les modifications apportées.
    
-----

Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Utilisation de contrôles segmentés

Comme indiqué ci-dessus, s que contrôle segmenté fournit un ensemble d’éléments linéaires, chacun d’eux peut contenir une icône ou du texte et est utilisé pour fournir un ensemble de choix associée à l’utilisateur.

Il existe plusieurs manières différentes, vous pouvez travailler avec les contrôles segmentés dans votre application Xamarin.tvOS.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>Exposées en tant que noms et des événements

Si vous créé votre contrôle Segment dans le Concepteur d’Interface et il exposée comme un contrôle nommé et un événement, vous pouvez utiliser le code suivant pour répondre à la modification de segment :

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

Dans le cas de l’exemple ci-dessus, le contrôle Segment a été exposé comme un `PlayerCount` nom et un `PlayerCountChanged` Action d’événement. Pour plus d’informations sur l’utilisation des Actions et Outlets, veuillez consulter la [écriture du Code avec les outlets et actions](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) section de notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md).

Le `SelectedSegment` propriété obtient ou définit l’index de segment actuellement sélectionné comme un zéro (0) en fonction. Pour que si vous avez des Segments de cinq (5), le premier Segment bénéficient d’un index de zéro (0) et le dernier index de quatre (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Modification des Segments

À tout moment, vous pouvez modifier le nombre et le contenu de vos contrôles segmentés. Utilisez le code suivant pour insérer une nouvelle icône Segment :

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Le deuxième paramètre définit l’emplacement où le Segment seront insérées à l’aide d’un index de base zéro (0). Si le dernier paramètre est `true` l’insertion sera animée.

Pour supprimer un Segment donné, utilisez les éléments suivants :

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Ou les éléments suivants pour supprimer tous les segments :

```csharp
SegmentedControl.RemoveAllSegments();
```

Là encore, si le dernier paramètre est `true`, la suppression sera animée. Utilisez le `NumberOfSegments` propriété pour retourner le nombre de Segments.

Pour obtenir le **titre** ou **icône** pour un segment donné, utilisez la commande suivante :

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

Et pour modifier le **titre** ou **icône**, utilisez la commande suivante :

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Pour voir si un Segment donné est **activé**, utilisez la commande suivante :

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

Et **activer/désactiver** un compte tenu de Segment, utilisez la commande suivante :

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Modification de l’apparence du contrôle segmenté

Vous pouvez utiliser le code suivant pour changer l’arrière-plan d’un Segment donné à une image :

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Où `UIControlState` Spécifie l’état du contrôle que vous définissez l’image pour l’en tant que :

- Normale
- mise en surbrillance
- Désactivé
- Selected
- Avec focus

Et `UIBarMetrics` spécifie les mesures à utiliser en tant que :

- Par défaut
- Compact
- DefaultPrompt
- CompactPrompt

En outre, vous pouvez définir le séparateur entre les Segments à l’aide de :

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

Où le premier `UIControlState` Spécifie l’état du Segment à gauche de la ligne de séparation et le second `UIControlState` Spécifie l’état du Segment à droite.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation de contrôle segmenté à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
