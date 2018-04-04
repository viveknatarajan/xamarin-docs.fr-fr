---
title: 'Erreur lors de l’envoi à l’App Store : « Regroupement non valide - Options ne pas autorisés à incorporer dans bitcode sont détectés dans l’envoi »'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: cacb9040ddc8582490c68bcfd24e80c4c4679eb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Erreur lors de l’envoi à l’App Store : « Regroupement non valide - Options ne pas autorisés à incorporer dans bitcode sont détectés dans l’envoi »

watchOS applications et les applications de tvOS _nécessitent_ bitcode lorsqu’ils sont soumis à l’App Store. Pendant la génération et envoi watchOS et tvOS des applications à l’aide de Xcode 8.3 ou une version antérieure, l’erreur suivante peut se produire (via la notification par courrier électronique) lorsque vous tentez de télécharger vers le magasin d’applications :

>Regroupement non valide - l’application ne peut pas être traitée, car les options non autorisées à incorporer dans bitcode sont détectées dans la soumission. Il est probable que vous ne créez pas de l’application avec la chaîne d’outils fourni dans Xcode.

La solution à ce problème consiste à générer les applications avec Xcode 9 et la dernière version de Xamarin.iOS.
