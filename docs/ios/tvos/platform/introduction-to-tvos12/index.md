---
title: Introduction à tvOS 12
description: Ce document fournit un haut niveau de la vue d’ensemble des fonctionnalités nouvelles et mises à jour de tvOS 12 pour la préversion de quels Xamarin fournit actuellement des liaisons c#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: e45d9944a2f4fc392b5a78efb4a7751d19641c73
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030663"
---
# <a name="introduction-to-tvos-12"></a>Introduction à tvOS 12

![Preview](~/media/shared/preview.png)

> [!WARNING]
> Support de 12 tvOS de Xamarin est actuellement en version préliminaire, ce qui signifie qu’il peut contenir des bogues, n’est pas complet, et peut changer. Utilisez-le pour l’expérimentation uniquement.

> [!NOTE]
> - Examinez le [mise en route](~/ios/platform/introduction-to-ios12/get-started.md) guide pour obtenir des instructions sur la façon de commencer à créer 12 applications iOS et tvOS 12 avec Xamarin.
> - Pour plus d’informations, consultez la préversion Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Ce document propose une vue d’ensemble de nouveaux et mis à jour tvOS 12 pour la version préliminaire de quels Xamarin version fournit actuellement des liaisons c#.

## <a name="password-autofill"></a>Remplissage automatique de mot de passe

Avec tvOS 12, les utilisateurs peuvent utiliser leurs appareils iOS pour vous connecter à une application tvOS avec un simple clic. Cette option est activée via une combinaison de `UITextContentType` champs de l’utilisation pour spécifier le nom d’utilisateur et mot de passe, les domaines associés pour établir une relation entre une application iOS et une application tvOS et environnements de focus par défaut pour sélectionner un élément devant recevoir le focus après qu’un utilisateur fournit un nom d’utilisateur et le mot de passe.

## <a name="focus-engine-enhancements"></a>Améliorations du moteur de focus

tvOS 12 permet à toutes les applications, quel que soit la façon dont ils sont affichés, pour interagir avec le moteur de Focus. Via les interactions d’un utilisateur avec l’instance distante de Siri, le moteur de Focus peut être utilisé avec n’importe quelle application pour sélectionner un élément, cacher des changements de focus possible et naturellement mettre à jour le focus. Cette option est activée dans des applications personnalisées par le biais de UIKit `IUIFocusItemContainer` interface, le `UIFocusMovementHint` (classe), le `IUIFocusItemScrollableContainer` interface et autres classes et méthodes.

## <a name="related-links"></a>Liens connexes

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – développeur Apple (Apple)](https://developer.apple.com/tvos/)
- [Quelles sont les nouveautés dans tvOS 12 (Apple) (vidéo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Aperçu de Xamarin [mise en production de billet de blog](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
