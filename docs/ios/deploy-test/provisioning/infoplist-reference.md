---
title: "Informations de référence sur Info.plist"
ms.topic: article
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
ms.openlocfilehash: 7732419af22ab8bd37cbfbf828dc59e48841217f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="infoplist-reference"></a>Informations de référence sur Info.plist

Pour plus d’informations sur l’utilisation des clés Info.plist, consultez le guide [Sécurité et confidentialité](~/ios/app-fundamentals/security-privacy.md). 

## <a name="location"></a>Emplacement 

L’accès à l’emplacement de l’utilisateur nécessite également des modifications dans le fichier Info.plist. Vous devez définir les clés suivantes relatives aux données d’emplacement : 

* **NSLocationWhenInUseUsageDescription** : cette clé est utilisée quand vous accédez à l’emplacement d’un utilisateur en train d’interagir avec votre application. 
* **NSLocationAlwaysUsageDescription** : cette clé est utilisée quand votre application accède à l’emplacement d’un utilisateur en arrière-plan.

## <a name="photos"></a>Photos 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>Contacts 

NSContactsUsageDescription 

## <a name="calendar-data"></a>Données de calendrier 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>Données de rappel 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Périphériques Bluetooth 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>Microphone 

NSMicrophoneUsageDescription 

## <a name="camera"></a>Appareil photo 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>Lecture HealthKit  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>Modes d’arrière-plan 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>Liens associés

- [Guide de référence Apple.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
