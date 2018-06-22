---
title: Comment puis-je réactiver des options pour développeurs après la mise à jour iOS ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F38BD21E-0C21-43FF-80A6-BB4A88DB88A5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9eb25676cc4a3a0679d6a02fc64fef5632f76e33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775482"
---
# <a name="how-can-i-reenable-developer-options-after-updating-ios"></a>Comment puis-je réactiver des options pour développeurs après la mise à jour iOS ?

Un bogue iOS peut provoquer les options de développeur à disparaître après la mise à jour des versions d’iOS, il a été observée lors du passage à iOS 8.x. Ces options peuvent être réactivées en procédant comme suit :

1. Se connecter à l’appareil iOS à votre ordinateur de développement Mac.
2. Démarrez Xcode.
3. Ouvrez **fenêtre > appareils** & Sélectionnez votre appareil.
4. Attendez Xcode terminer la copie des symboles, puis vérifiez Settings.app du périphérique pour les paramètres de développeur.
5. Redémarrez l’appareil.
