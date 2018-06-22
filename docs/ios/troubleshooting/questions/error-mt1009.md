---
title: 'Erreur MT1009 : N’a pas pu copier l’assembly'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776077"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Erreur MT1009 : N’a pas pu copier l’assembly

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin.iOS. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.

Comme décrit dans notre [Notes de publication](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), il s’agit d’un problème connu dans Xamarin.iOS 7.2.6. Ce problème est en raison d’autorisations de fichier nécessitant des privilèges plus élevés lorsque Xamarin.iOS est installé avec un autre compte d’utilisateur puis le compte de principal.

Pour contourner le problème, veuillez ouvrir le Terminal.app sur la station de travail Mac et exécutez la commande suivante :

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

