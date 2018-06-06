---
title: Les packages du SDK Android dois-je installer ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: df359fae545079c7ac1d7106ec86e9297f183f90
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732773"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Les packages du SDK Android dois-je installer ?

Installation du SDK Android n’inclut pas automatiquement tous les packages requis minimales pour le développement. Bien que les développeurs individuels doivent varier, les packages suivants généralement seront nécessaires pour le développement avec Xamarin.Android :

## <a name="tools"></a>Outils

Installez les derniers outils à partir du dossier Outils dans le Gestionnaire du SDK :

- Outils SDK Android
- Outils de plateforme du kit SDK Android
- Outils de génération SDK Android

## <a name="android-platforms"></a>Plateformes Android

Installez la plate-forme de kit de développement logiciel « » pour les versions Android que vous avez définie comme minimum & cible. 

Exemples :

- API 23 cible
- API minimale 23

Vous devez uniquement installer le Kit de développement logiciel Platform pour API 23

- API 23 cible
- API minimale 15

Devez installer le SDK plateformes pour API 15 et 23. Notez que vous n’avez pas besoin installer les niveaux d’API entre les valeurs minimale et la cible (même si vous êtes backporting à ces niveaux d’API).

## <a name="system-images"></a>Images du système

Il s’agit uniquement requis si vous souhaitez utiliser les émulateurs Android out of box à partir de Google. Pour plus d’informations, consultez [le programme d’installation d’émulateur Android](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Suppléments
Les fonctionnalités supplémentaires de kit de développement logiciel Android n’est généralement pas obligatoire ; mais il est utile d’être informé, car elles peuvent être requises en fonction de votre cas d’usage.

## <a name="further-reading"></a>informations supplémentaires
Le guide suivant traite de ces options et approfondit sur les différents packages le Kit de développement logiciel gestionnaire est disponible : [Guide d’installation Android SDK Manager](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

