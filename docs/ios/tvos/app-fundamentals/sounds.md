---
title: Lecture son avec AVAudioPlayer
description: "Cet article explique comment utiliser une classe d’assistance pour contrôler la lecture de son à l’aide d’un AVAudioPlayer."
ms.topic: article
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 59b91c22060fbbc0d1855db59f234048d4673110
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Lecture son avec AVAudioPlayer

_Cet article explique comment utiliser une classe d’assistance pour contrôler la lecture de son à l’aide d’un AVAudioPlayer._

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
