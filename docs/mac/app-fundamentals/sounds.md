---
title: Lecture du son avec AVAudioPlayer dans Xamarin.Mac
description: Ce document décrit comment lire un son avec AVAudioPlayer dans une application Xamarin.Mac. Elle explique AVAudioPlayer à un niveau élevé et des liens vers d’autres documentations qui explore de plus en détail.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791853"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Lecture du son avec AVAudioPlayer dans Xamarin.Mac

## <a name="about-the-avaudioplayer"></a>Sur le AVAudioPlayer

La `AVAudioPlayer` classe est utilisée pour les données audio de lecture à partir de la mémoire ou un fichier. Apple recommande l’utilisation de cette classe pour lire des fichiers audio dans votre application, sauf si vous êtes en train réseau de diffusion en continu ou nécessitent une latence faible des e/s audio.

Vous pouvez utiliser la `AVAudioPlayer` classe pour effectuer les opérations suivantes :

- Lire les sons d’une durée quelconque avec bouclage facultatif.
- Lire les sons multiples en même temps avec la synchronisation facultatif.
- Contrôler le volume, la vitesse de lecture et le positionnement stéréo pour chaque lecture de sons.
- Prend en charge des fonctionnalités telles qu’avance rapide ou rembobiner.
- Obtenir des données de contrôle au niveau de la lecture.

`AVAudioPlayer` prend en charge des sons dans n’importe quel format audio fournie par iOS, tvOS et macOS comme .aif, .wav ou .mp3.

## <a name="playing-sounds-in-macos"></a>Lire des sons dans macOS

MacOS prenant en charge les mêmes classes de boîte à outils Audio comme iOS, consultez notre iOS [son jouer avec AVAudioPlayer](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) documentation pour plus d’informations de lecture audio dans une application Xamarin.Mac.

## <a name="related-links"></a>Liens associés

- [Référence de AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
