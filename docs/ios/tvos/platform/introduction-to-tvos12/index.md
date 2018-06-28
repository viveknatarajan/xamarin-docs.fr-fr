---
title: Présentation de tvOS 12
description: Ce document fournit un haut niveau de la vue d’ensemble des fonctionnalités nouvelles et mises à jour de tvOS 12 pour la préversion de quels Xamarin fournit actuellement des liaisons de c#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067285"
---
# <a name="introduction-to-tvos-12"></a>Présentation de tvOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Prise en charge de Xamarin tvOS 12 est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, n’est pas complet, et peut changer. Utilisez-le pour seulement l’expérimentation.

> [!NOTE]
> - Examinez le [mise en route](~/ios/platform/introduction-to-ios12/get-started.md) guide pour obtenir des instructions sur la façon de commencer à créer des applications de tvOS 12 avec Xamarin et iOS 12.
> - Pour plus d’informations, consultez la [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/) pour la Xamarin version préliminaire.

Ce document fournit une vue d’ensemble des nouvelles et mises à jour tvOS 12 fonctionnalités pour l’aperçu de quels Xamarin version fournit actuellement des liaisons c#.

## <a name="password-autofill"></a>Remplissage automatique de mot de passe

Avec tvOS 12, les utilisateurs peuvent utiliser leurs appareils iOS pour vous connecter à une application de tvOS d’un seul clic. Cette option est activée par une combinaison des `UITextContentType` l’utilisation de pour spécifier le nom d’utilisateur et mot de passe des champs, des domaines associés pour établir une relation entre une application iOS et une application de tvOS et environnements de focus par défaut pour sélectionner un élément à recevoir le focus après qu’un utilisateur fournit un nom d’utilisateur et un mot de passe.

## <a name="focus-engine-enhancements"></a>Améliorations du moteur de focus

tvOS 12 permet toutes les applications, quel que soit la façon dont ils sont rendus, pour interagir avec le moteur le Focus. Via les interactions d’un utilisateur à la distance Siri, le moteur du Focus peut être utilisé avec n’importe quelle application pour sélectionner un élément, indicateur de modifications de focus possibles et naturellement mettre à jour le focus. Cette option est activée dans les applications personnalisées par le biais de UIKit `IUIFocusItemContainer` interface, le `UIFocusMovementHint` (classe), le `IUIFocusItemScrollableContainer` interface et les autres classes et méthodes.

## <a name="related-links"></a>Liens connexes

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [Nouveautés de tvOS 12 (Apple) (vidéo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Aperçu de Xamarin [notes de publication](https://releases.xamarin.com/preview-release-xcode-10-beta/)
