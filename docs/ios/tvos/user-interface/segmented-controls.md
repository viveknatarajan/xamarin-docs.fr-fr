---
title: Utilisation des contrôles segmentés
description: Cet article décrit la conception et l’utilisation de contrôles segmentés à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d4eac932c7fad628a0a65127bceb641f34ea5d79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-segmented-controls"></a>Utilisation des contrôles segmentés

_Cet article décrit la conception et l’utilisation de contrôles segmentés à l’intérieur d’une application Xamarin.tvOS._


Un contrôle segmenté fournit un ensemble d’éléments linéaires, chacun d’eux peut contenir un texte ou une icône et est utilisée pour fournir un ensemble de choix associée à l’utilisateur.

[![](segmented-controls-images/segment01.png "Exemples de contrôles de segment")](segmented-controls-images/segment01.png#lightbox)

Apple a les suggestions suivantes pour travailler avec des contrôles segmentés :

- **Fournir suffisamment d’espace** -soins doit être prise pour fournir suffisamment d’espace entre les autres [éléments peut être actif](~/ios/tvos/app-fundamentals/navigation-focus.md) et un contrôle segmenté. Un Segment individuel est sélectionné quand il est actif (non pas lorsque vous cliquez dessus) et l’utilisateur peut modifier accidentellement des segments lorsqu’ils souhaitent réellement sélectionner un autre élément peut être actif sur le segment actuel.
- **Utilisez les affichages fractionnés de filtrage du contenu** -contrôles segmenté ne faites pas bons choix pour le filtrage comme affichages fractionnés ont été conçus pour faciliter la navigation entre le contenu et les filtres de contenu.
- **Limite au nombre maximal de Segments sept** -vous devez tenter de conserver le nombre maximal de segments ci-dessous huit (8), comme cela est plus facile d’analyse à partir de l’espace sur le canapé et plus facile de naviguer.
- **Utiliser la taille de contenu cohérent Segment** - tous les Segments ont la même largeur et, si possible, vous devriez conserver le contenu dans chaque segment de la même taille. Cela n’est pas seulement rend les contrôles de Segment plus agréable, mais rend plus facile à lire un coup de œil.
- **Éviter le mélange des icônes et du texte** -chaque étape peut contenir une icône ou texte, mais pas les deux. Bien qu’il soit possible de combiner des icônes et du texte dans le même contrôle segmenté, cela doit être évité.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>À propos des icônes de Segment

Apple suggère à l’aide des images reconnaissables simples pour les icônes de Segment, telles que Loupe pour la recherche. Icônes trop complexes sont difficiles à reconnaître sur un écran dans l’espace, il est donc préférable de limiter les icônes des représentations simples.

Vous ne pouvez pas mélanger les formats texte et les icônes sur un Segment donné, et vous devez éviter de mélanger des icônes et du texte dans un seul contrôle segmenté. Il convient de toutes les icônes ou tout le texte.

<a name="Summary" />

## <a name="segment-text"></a>Texte de segment

Apple rend les suggestions suivantes pour travailler avec du texte de Segment :

- **Utilisez court, des noms explicites** -titre du Segment doit définir clairement le type de contenu que l’utilisateur doit attendre lors de la sélection du Segment donné. Par exemple : musique ou des vidéos.
- **Utiliser la mise en majuscules de la casse du titre** -chaque mot du titre de Segments doit être en majuscule à l’exception des articles, conjonctions et prépositions de moins de quatre (4) caractères.
- **Utilisez courte, axée sur les titres** -conserver les titres, courts et se concentrent sur le type de contenu attendu lorsque le Segment est sélectionné.

Là encore, vous ne pouvez pas mélanger les formats texte et les icônes sur un Segment donné, et vous devez éviter de mélanger des icônes et du texte dans un seul contrôle segmenté.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Contrôles de segment et les animations

Utilisation des contrôles de Segment dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **Segment contrôle** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](segmented-controls-images/segment02.png "Un contrôle de Segment")](segmented-controls-images/segment02.png#lightbox)
1. Dans le **onglet Widget** de la **remplissage de la propriété**, vous pouvez ajuster plusieurs propriétés du contrôle Segment, telles que son **Style** et **état**: 

    [![](segmented-controls-images/segment03.png "L’onglet du Widget")](segmented-controls-images/segment03.png#lightbox)
1. Utilisez le **Segments** champ pour contrôler le nombre de Segments dans le contrôleur.
1. Sélectionner un Segment donné à partir de la **Segment déroulante** ajuster ses propriétés individuelles telles que **titre** ou **Image** et de contrôler si un Segment donné est  **Activé** ou **sélectionnés** lorsque le contrôle est affiché.
1. Enfin, affectez **noms** aux contrôles afin que vous pouvez répondre à leur en code c#. Par exemple : 

    [![](segmented-controls-images/segment04.png "Attribuez un nom")](segmented-controls-images/segment04.png#lightbox)
1. Enregistrez les modifications apportées.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **Segment contrôle** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](segmented-controls-images/segment02-vs.png "Un contrôle de Segment")](segmented-controls-images/segment02-vs.png#lightbox)
1. Dans le **onglet Widget** de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du contrôle Segment, telles que son **Style** et **état**: 

    [![](segmented-controls-images/segment03-vs.png "L’onglet du Widget")](segmented-controls-images/segment03-vs.png#lightbox)
1. Utilisez le **Segments** champ pour contrôler le nombre de Segments dans le contrôleur.
1. Sélectionner un Segment donné à partir de la **Segment déroulante** ajuster ses propriétés individuelles telles que **titre** ou **Image** et de contrôler si un Segment donné est  **Activé** ou **sélectionnés** lorsque le contrôle est affiché.
1. Enfin, affectez **noms** aux contrôles afin que vous pouvez répondre à leur en code c#. Par exemple : 

    [![](segmented-controls-images/segment04-vs.png "Attribuez un nom")](segmented-controls-images/segment04-vs.png#lightbox)
1. Enregistrez les modifications apportées.
    
-----

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Utilisation des contrôles segmentés

Comme indiqué ci-dessus, s que contrôle segmenté fournit un ensemble d’éléments linéaires, chacun d’eux peut contenir un texte ou une icône et est utilisée pour fournir un ensemble de choix associée à l’utilisateur.

Il existe plusieurs manières différentes, vous pouvez travailler avec des contrôles segmentés dans votre application Xamarin.tvOS.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>Exposées en tant que noms et des événements

Si vous avez créé votre contrôle Segment dans le Concepteur de l’Interface il exposée comme un contrôle nommé et d’un événement, vous pouvez utiliser le code suivant pour répondre à la modification de segment :

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

Dans le cas de l’exemple ci-dessus, le contrôle Segment a été exposé comme un `PlayerCount` nom et un `PlayerCountChanged` Action de l’événement. Pour plus d’informations sur l’utilisation des Actions et les points de vente, consultez la [l’écriture du Code avec les sorties et les actions](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) section de notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md).

Le `SelectedSegment` propriété obtient ou définit l’index de segment sélectionné en tant qu’un zéro (0) en fonction. Donc si vous avez des Segments de cinq (5), le premier Segment ont un index de zéro (0) et le dernier index de quatre (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Modification des Segments

À tout moment, vous pouvez modifier le nombre et le contenu de vos contrôles segmenté. Utilisez le code suivant pour insérer une nouvelle icône de Segment :

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Le deuxième paramètre définit où le Segment sera inséré à l’aide d’un index de base zéro (0). Si le dernier paramètre est `true` l’insertion est animée.

Pour supprimer un Segment donné, utilisez les éléments suivants :

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Ou le pour supprimer tous les segments suivants :

```csharp
SegmentedControl.RemoveAllSegments();
```

Là encore, si le dernier paramètre est `true`, la suppression est animée. Utilisez le `NumberOfSegments` propriété pour retourner le nombre de Segments.

Pour obtenir le **titre** ou **icône** pour un segment donné, utilisez les éléments suivants :

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

Et pour modifier la **titre** ou **icône**, utilisez les éléments suivants :

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Pour voir si un Segment donné est **activé**, utilisez les éléments suivants :

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

Et **activer/désactiver** un Segment, utiliser les éléments suivants :

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Modification de l’apparence du contrôle segmenté

Vous pouvez utiliser le code suivant pour modifier l’arrière-plan d’un Segment donné à une image :

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Où `UIControlState` Spécifie l’état du contrôle que vous définissez l’image en tant que :

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

Où la première `UIControlState` Spécifie l’état du Segment à gauche de la ligne de séparation et le second `UIControlState` Spécifie l’état du Segment à droite.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation de contrôle segmentés à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
