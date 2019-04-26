---
title: Introduction à tvOS 12
description: Ce document fournit un haut niveau de la vue d’ensemble des fonctionnalités nouvelles et mises à jour de tvOS 12 pour la préversion de quels Xamarin fournit actuellement des liaisons c#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7fb8cc379a070b848c5154c9c1d4fbfc8186266
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60932538"
---
# <a name="introduction-to-tvos-12"></a>Introduction à tvOS 12

Ce document fournit une vue d’ensemble de nouveaux et mis à jour tvOS 12.

Pour commencer la création d’applications tvOS 12 avec Xamarin, examinons le [guide Mise en route](~/ios/platform/introduction-to-ios12/get-started.md).

## <a name="tvuikit"></a>TVUIKit

tvOS 12 inclut TVUIKit, un ensemble d’API qui permettent aux développeurs de tvOS d’utiliser des contrôles de tvOS courants tels que des boutons de légende, des affichages de poster, des vues de la carte et affichages de Monogramme. tvOS 12 introduit également une propriété qui permet des étiquettes faire défiler le texte est trop long pour être complètement visibles.

## <a name="password-autofill"></a>Remplissage automatique de mot de passe

Avec tvOS 12, les utilisateurs peuvent utiliser leurs appareils iOS pour vous connecter à une application tvOS avec un simple clic. Cette option est activée via une combinaison de `UITextContentType` champs de l’utilisation pour spécifier le nom d’utilisateur et mot de passe, les domaines associés pour établir une relation entre une application iOS et une application tvOS et environnements de focus par défaut pour sélectionner un élément devant recevoir le focus après qu’un utilisateur fournit un nom d’utilisateur et le mot de passe.

## <a name="focus-engine-enhancements"></a>Améliorations du moteur de focus

tvOS 12 permet à toutes les applications, quel que soit la façon dont ils sont affichés, pour interagir avec le moteur de Focus. Via les interactions d’un utilisateur avec l’instance distante de Siri, le moteur de Focus peut être utilisé avec n’importe quelle application pour sélectionner un élément, cacher des changements de focus possible et naturellement mettre à jour le focus. Cette option est activée dans des applications personnalisées par le biais de UIKit `IUIFocusItemContainer` interface, le `UIFocusMovementHint` (classe), le `IUIFocusItemScrollableContainer` interface et autres classes et méthodes.

## <a name="vision-framework"></a>Framework de vision

L’infrastructure de Vision inclut un détecteur de visage améliorée qui peut détecter les visages dans différentes orientations. En outre, demander une révision est désormais utilisable pour sélectionner une révision d’algorithme de framework Vision spécifique.

## <a name="natural-language-framework"></a>Framework de langage naturel

L’infrastructure de langage naturel permet aux applications d’effectuer différents types d’analyse linguistique. Par exemple, il peut être utilisé pour identifier les parties du discours et de déterminer la langue représentée par un bloc de texte.

## <a name="deprecations"></a>Dépréciations

Avec tvOS 12, Apple a déconseillé OpenGL ES, [encouragent les développeurs](https://developer.apple.com/tvos/whats-new/) à adopter complète.

## <a name="related-links"></a>Liens connexes

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – développeur Apple (Apple)](https://developer.apple.com/tvos/)
- [Quelles sont les nouveautés dans tvOS 12 (Apple) (vidéo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)