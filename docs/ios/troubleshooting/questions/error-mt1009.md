---
title: 'Erreur MT1009 : impossible de copier l’assembly'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4a67537cc53aeecf1b86d11dbf041cea79587dd2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61422007"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Erreur MT1009 : impossible de copier l’assembly

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin.iOS. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.

Comme décrit dans notre [Notes de publication](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), il s’agit d’un problème connu dans Xamarin.iOS 7.2.6. Ce problème est en raison d’autorisations de fichier nécessitant des privilèges plus élevés lorsque Xamarin.iOS est installé avec un autre compte d’utilisateur puis compte principal du développeur.

Pour contourner ce problème, veuillez ouvrir le Terminal.app sur la station de travail Mac et exécutez la commande suivante :

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

