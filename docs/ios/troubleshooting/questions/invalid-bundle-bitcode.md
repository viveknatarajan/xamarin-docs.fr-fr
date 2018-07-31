---
title: 'Erreur lors de l’envoi à l’App Store : « Bundle non valide : Options non autorisées à incorporer dans bitcode sont détectées dans l’envoi »'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 393c1ed81c68d21b610781dfe09de97969e031d1
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350922"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Erreur lors de l’envoi à l’App Store : « Bundle non valide : Options non autorisées à incorporer dans bitcode sont détectées dans l’envoi »

watchOS les applications et tvOS _nécessitent_ bitcode lorsqu’ils sont soumis à l’App Store. Créer et soumettre des applications watchOS et tvOS à l’aide de Xcode 8.3 ou une version antérieure, l’erreur suivante peut se produire (via la notification par courrier électronique) lorsque vous tentez de charger sur l’App Store :

>Bundle non valide : l’application ne peut pas être traitée, car les options non autorisées à incorporer dans bitcode sont détectées dans la soumission. Il est probable que vous ne créez pas de l’application avec la chaîne d’outils fourni dans Xcode.

La solution à ce problème consiste à générer les applications avec Xcode 9 et la dernière version de Xamarin.iOS.
