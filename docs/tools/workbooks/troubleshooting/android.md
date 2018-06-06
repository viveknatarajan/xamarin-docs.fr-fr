---
title: Dépannage des classeurs Xamarin sur Android
description: Ce document fournit des conseils de dépannage pour l’utilisation des classeurs Xamarin sur Android. Il décrit la prise en charge de l’émulateur, les classeurs qui ne se chargent pas et d’autres rubriques.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: topgenorth
ms.author: toopge
ms.openlocfilehash: b01fa5b67f8a7f5577c6598f63f8c38e8fa4cf3c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793982"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Dépannage des classeurs Xamarin sur Android

## <a name="emulator-support"></a>Prise en charge de l’émulateur

Pour exécuter un classeur Android, un émulateur Android doit être disponible pour utilisation. Les appareils Android physiques ne sont pas pris en charge.

Nous vous recommandons d’émulateur de Google en même temps que HAXM si votre ordinateur prend en charge.
Si vous devez avoir Hyper-V est activé sur votre système, accédez à la place avec l’émulateur Android de Visual Studio.

Vous devez disposer d’un émulateur qui exécute Android 5.0 ou version ultérieure. Les émulateurs ARM ne sont pas pris en charge. Utilisez `x86` ou `x86_64` uniquement les appareils.

Veuillez lire [notre documentation sur la configuration des émulateurs Android] [ android-emu] si vous n’êtes pas familiarisé avec le processus.

> [!NOTE]
> 1.1 et les versions antérieures de classeurs seront essayez (et échouer !) pour utiliser les émulateurs ARM si elles sont disponibles. Pour contourner cette situation, émulateur le x86 de lancement de votre choix avant d’ouvrir ou de créer un classeur Android. Classeurs préfèreront toujours pour se connecter à un émulateur en cours d’exécution, tant qu’il est compatible.

## <a name="workbooks-wont-load"></a>Ne chargent pas les classeurs

### <a name="workbook-window-spins-forever-never-loads-windows"></a>La fenêtre de classeur tourne indéfiniment, jamais charges (Windows)

Tout d’abord, vérifiez que votre émulateur a un accès réseau complet travail en testant un site Web dans le navigateur de l’émulateur.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>L’émulateur Android de Visual Studio ne peut pas se connecter à internet

Si votre émulateur n’a pas accès au réseau, vous devrez peut-être suivre ces étapes pour résoudre le commutateur de réseau Hyper-V. Si vous basculez entre les réseaux Wi-Fi fréquemment, vous devrez répéter ce régulièrement :

0. **Assurez-vous que les opérations réseau critiques sont terminées, comme cela peut déconnecter temporairement Windows à partir d’internet.**
1. Fermez les émulateurs.
2. Ouvrez `Hyper-V Manager`.
3. Sous `Actions`, ouvrez `Virtual Switch Manager...`.
4. Supprimez tous les commutateurs virtuels.
5. Cliquez sur `OK`.
6. Lancez l’émulateur Android. Vous devrez probablement recréer le commutateur de réseau virtuel.
7. Test que navigateur de l’émulateur VS Android peut accéder à internet.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Liens associés

- [Signalement des bogues](~/tools/workbooks/install.md#reporting-bugs)
