---
title: Accessibilité dans les applications Xamarin
description: Ce document fournit des conseils différentes pour la création d’applications accessibles. Par exemple, il inclut des recommandations sur les polices de grande taille, contraste élevé, les interfaces autodescriptifs et bien plus encore.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 0ec264e0f3d381fdac46c79dd479da2bc768954f
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668333"
---
# <a name="accessibility-in-xamarin-apps"></a>Accessibilité dans les applications Xamarin

_Assurez-vous que vos applications sont utilisables par l’audience la plus large possible_

Accessibilité fait référence au concept de conception des interfaces utilisateur application qui fonctionnent bien des fonctions d’assistance d’Affichage et d’entrée de système d’exploitation comme type volumineux, contraste élevé, un zoom avant, lecture d’écran (synthèse vocale), des piles de commentaires visual ou HAPTIQUE, et autres méthodes d’entrée.

Plateformes de bureau et mobiles comme iOS, Android et Windows fournissent intégrées dans les API qui aident les développeurs à créer des applications accessibles, tel que [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) et [VoiceOver d’Apple](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>API propres aux plateformes

Pour implémenter les recommandations dans ce document, utilisez les API fournies par chaque plateforme :

- [**Accessibilité Android**](~/android/app-fundamentals/accessibility.md)
- [**iOS d’accessibilité**](~/ios/app-fundamentals/accessibility.md)
- [**Système d’exploitation X accessibilité**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Liste de contrôle d’accessibilité

Suivez ces conseils pour vous assurer que vos applications sont accessibles à l’audience plus large possible. Découvrez le [Android aide-mémoire de test d’accessibilité](https://developer.android.com/training/accessibility/testing.html) et [page d’accessibilité d’Apple](http://www.apple.com/accessibility/) pour plus d’informations.

### <a name="support-large-fonts-and-high-contrast"></a>Prend en charge des polices de grande taille et le contraste élevé

Évitez les dimensions de contrôle de coder en dur et, au lieu de cela, préférez les dispositions qui peuvent être redimensionné pour prendre en charge des tailles de police supérieures.
Tester sa palette de couleurs en mode de contraste élevé pour vous assurer qu’ils sont accessibles en lecture.

### <a name="make-the-user-interface-self-describing"></a>Rendre l’utilisateur interface autodescriptifs

Baliser tous les éléments de votre interface utilisateur avec un texte descriptif et les indicateurs qui sont compatibles avec l’écran de la lecture d’API sur chaque plateforme.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Assurez-vous que les images et icônes ont une description de texte de remplacement

Images et icônes qui font partie de l’interface utilisateur (par exemple, des boutons ou des indicateurs d’état, par exemple) doivent être marqués avec une description accessible.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Conception de l’arborescence visuelle avec navigation accessible à l’esprit

Utilisez les contrôles de disposition appropriée ou API afin que la navigation entre les contrôles à l’aide d’autres méthodes d’entrée suivant le même flux logique comme à l’aide de l’écran tactile.

Exclure les éléments inutiles de lecteurs d’écran (images décoratives ou étiquettes pour les champs qui sont déjà accessibles, par exemple).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Ne vous fiez les signaux audio ou la couleur uniquement

Éviter les situations où l’indication exclusive de progression, de saisie semi-automatique ou d’un autre état est un son ou un changement de couleur. Soit concevoir l’interface utilisateur pour inclure des signaux visuels clair (avec son et la couleur pour le renforcement uniquement), ou ajouter des indicateurs d’accessibilité spécifiques.

Lors du choix des couleurs, essayez d’éviter une palette est difficile de faire la distinction pour les utilisateurs avec daltonisme.

### <a name="captioning-for-video-text-for-audio"></a>Sous-titrage codé pour le texte d’une vidéo, audio

Fournir des légendes pour le contenu vidéo et un script lisible pour le contenu audio. Il est également utile de fournir des contrôles qui s’ajustent la vitesse du contenu audio ou vidéo et vous assurer de ce volume et boutons de lecture/pause sont faciles à trouver et à utiliser.

### <a name="localize"></a>Localize

Les descriptions d’accessibilité peuvent (et doivent) être localisées où l’application prend en charge plusieurs langues.



## <a name="related-links"></a>Liens associés

- [Accessibilité Android](~/android/app-fundamentals/accessibility.md)
- [iOS d’accessibilité](~/ios/app-fundamentals/accessibility.md)
- [Système d’exploitation X accessibilité](~/mac/app-fundamentals/accessibility.md)
- [Accessibilité de Xamarin.Forms](~/xamarin-forms/app-fundamentals/accessibility/index.md)
