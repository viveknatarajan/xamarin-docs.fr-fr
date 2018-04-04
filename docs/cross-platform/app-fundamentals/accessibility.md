---
title: Accessibilité
description: Assurez-vous que vos applications sont utilisables par l’audience la plus large possible
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 3b7912f7875e9d07de51861e573065b3d1b73de0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility"></a>Accessibilité

_Assurez-vous que vos applications sont utilisables par l’audience la plus large possible_

Accessibilité fait référence au concept de conception des interfaces utilisateur application fonctionnent bien des fonctions d’assistance à un affichage et une entrée de système d’exploitation comme type de grande taille, contraste élevé, faire un zoom, lecture de l’écran (TTS), piles de commentaires visual ou tactiles, et autres méthodes d’entrée.

Poste de travail et mobile, iOS, Android et Windows fournissent générées dans les API qui aident les développeurs à générer des applications accessibles, tel que [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) et [VoiceOver d’Apple](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>API spécifiques à la plateforme

Pour implémenter les recommandations dans ce document, utilisez les API fournies par chaque plateforme :

- [**Accessibilité Android**](~/android/app-fundamentals/accessibility.md)
- [**e/s d’accessibilité**](~/ios/app-fundamentals/accessibility.md)
- [**Système d’exploitation X accessibilité**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Liste de vérification d’accessibilité

Suivez les conseils suivants pour vous assurer que vos applications sont accessibles à l’audience plus large possible. Extraire le [Android aide-mémoire de test d’accessibilité](http://developer.android.com/training/accessibility/testing.html) et [page d’accessibilité d’Apple](http://www.apple.com/accessibility/) pour plus d’informations.

### <a name="support-large-fonts-and-high-contrast"></a>Prend en charge des polices de grande taille et de contraste élevé

Évitez les dimensions de contrôle de coder en dur et, au lieu de cela, préférez les dispositions peuvent redimensionner pour prendre en compte la taille de police supérieure.
Tester les jeux de couleurs en mode de contraste élevé pour s’assurer qu’ils sont lisibles.

### <a name="make-the-user-interface-self-describing"></a>Vérifiez l’utilisateur interface autodescriptifs

Baliser tous les éléments de votre interface utilisateur avec un texte descriptif et les indicateurs qui sont compatibles avec l’écran de la lecture des API sur chaque plateforme.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Assurez-vous que les icônes et les images ont une description texte de remplacement

Images et des icônes qui font partie de l’interface utilisateur (par exemple, des boutons ou des indicateurs d’état, par exemple) doivent être balisés avec une description accessible.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Conception de l’arborescence d’éléments visuels avec une navigation accessible à l’esprit

Utilisez les contrôles de disposition appropriée ou API permettant de naviguer entre les contrôles à l’aide d’autres méthodes d’entrée suit le même flux logique comme à l’aide de l’écran tactile.

Exclure les éléments inutiles de lecteurs d’écran (images décoratifs ou étiquettes pour les champs qui sont déjà accessibles, par exemple).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Ne vous fiez les signaux audio ou de couleur uniquement

Éviter les situations où l’indication unique de progression, de saisie semi-automatique ou un autre état est un son ou un changement de couleur. Soit vous devez concevoir l’interface utilisateur pour inclure des signaux visuels Effacer (avec son et de couleur pour le renforcement uniquement) ou ajouter des indicateurs d’accessibilité spécifiques.

Lors du choix de couleurs, essayez d’éviter une palette est difficile de distinguer des utilisateurs avec des personnes.

### <a name="captioning-for-video-text-for-audio"></a>Sous-titrage pour le texte d’une vidéo, audio

Fournir des légendes pour le contenu vidéo et d’un script accessible en lecture pour le contenu audio. Il s’avère également utile fournir des contrôles qui ajustent la vitesse du contenu audio ou vidéo et vous assurer de ce volume et boutons de lecture/pause sont faciles à trouver et utiliser.

### <a name="localize"></a>Localize

Les descriptions d’accessibilité peuvent (et doivent) être localisées dans lequel l’application prend en charge plusieurs langues.



## <a name="related-links"></a>Liens associés

- [Accessibilité Android](~/android/app-fundamentals/accessibility.md)
- [e/s d’accessibilité](~/ios/app-fundamentals/accessibility.md)
- [Système d’exploitation X accessibilité](~/mac/app-fundamentals/accessibility.md)
- [Accessibilité de Xamarin.Forms](~/xamarin-forms/app-fundamentals/accessibility/index.md)
