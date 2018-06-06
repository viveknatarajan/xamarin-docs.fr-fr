---
title: Lecture du son dans tvOS avec AVAudioPlayer dans Xamarin
description: Cet article explique comment utiliser une classe d’assistance pour contrôler la lecture de son un AVAudioPlayer à l’aide d’une application Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d95a8ea6c22c0d897d8ccfe0c2ca401f6523783
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788628"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Lecture du son dans tvOS avec AVAudioPlayer dans Xamarin

## <a name="about-the-avaudioplayer"></a>Sur le AVAudioPlayer

Le `AVAudioPlayer` est utilisé pour les données audio de lecture à partir de la mémoire ou un fichier. Apple recommande l’utilisation de cette classe pour lire des fichiers audio dans votre application, sauf si vous êtes en train réseau de diffusion en continu ou nécessitent une latence faible des e/s audio.

Vous pouvez utiliser la `AVAudioPlayer` pour effectuer les opérations suivantes :

- Lire les sons d’une durée quelconque avec bouclage facultatif.
- Lire les sons multiples en même temps avec la synchronisation facultatif.
- Contrôler le volume, la vitesse de lecture et le positionnement stéréo pour chaque lecture de sons.
- Prend en charge des fonctionnalités telles qu’avance rapide ou rembobiner.
- Obtenir des données de contrôle au niveau de la lecture.

`AVAudioPlayer` prend en charge des sons dans n’importe quel format audio fourni par iOS, tvOS et OS X, tel que `.aif`, `.wav` ou `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Lire des sons dans tvOS

TvOS prenant en charge les mêmes classes de boîte à outils Audio comme iOS, consultez notre iOS [son jouer avec AVAudioPlayer](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) documentation pour plus d’informations de lecture audio dans une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Référence de AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
