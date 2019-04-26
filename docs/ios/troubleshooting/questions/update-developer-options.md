---
title: Comment puis-je réactiver les options pour développeurs après la mise à jour iOS ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F38BD21E-0C21-43FF-80A6-BB4A88DB88A5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 42f7f63c1da54bc7596d52c78cbcc1ed76931424
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417680"
---
# <a name="how-can-i-reenable-developer-options-after-updating-ios"></a>Comment puis-je réactiver les options pour développeurs après la mise à jour iOS ?

Un bogue iOS peut provoquer les options de développeur à disparaître après la mise à jour des versions d’iOS, cela a été observé lors du passage à iOS 8.x. La configuration de ces options peuvent être réactivées en procédant comme suit :

1. Connecter l’appareil iOS à votre machine de développement Mac.
2. Démarrez Xcode.
3. Ouvrez **fenêtre > appareils** & Sélectionnez votre appareil.
4. Attendez que Xcode pour terminer la copie des symboles, puis vérifiez Settings.app de l’appareil pour les paramètres de développeur.
5. Redémarrer l’appareil.
