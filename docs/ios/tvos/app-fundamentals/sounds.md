---
title: Lecture du son dans tvOS avec AVAudioPlayer dans Xamarin
description: Cet article explique comment utiliser une classe d’assistance pour contrôler la lecture du son à l’aide d’un AVAudioPlayer dans une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: bc1e9febea9529c3fadbacd689404562952276fe
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61414098"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Lecture du son dans tvOS avec AVAudioPlayer dans Xamarin

## <a name="about-the-avaudioplayer"></a>Sur le AVAudioPlayer

Le `AVAudioPlayer` est utilisé pour les données audio de la lecture à partir de la mémoire ou un fichier. Apple recommande l’utilisation de cette classe pour la lecture audio dans votre application, sauf si vous êtes en train réseau de diffusion en continu ou nécessitent une faible latence des e/s audio.

Vous pouvez utiliser le `AVAudioPlayer` pour effectuer les opérations suivantes :

- Lire les sons d’une durée quelconque avec bouclage facultatif.
- Lire les sons multiples en même temps avec la synchronisation facultative.
- Contrôler le volume, la vitesse de lecture et le positionnement stéréo pour chaque lecture des sons.
- Prend en charge des fonctionnalités telles qu’avance rapide ou rembobiner.
- Obtenir les données de contrôle de niveau de la lecture.

`AVAudioPlayer` prend en charge des sons dans n’importe quel format audio fourni par iOS, tvOS et OS X, tel que `.aif`, `.wav` ou `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Lecture de sons en tvOS

Étant donné que tvOS prend en charge les mêmes classes de boîte à outils Audio comme iOS, consultez notre iOS [audio de jouer avec AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) documentation pour tous les détails de la lecture audio dans une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Référence de AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
