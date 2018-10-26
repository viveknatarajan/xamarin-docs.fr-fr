---
title: Quels packages Android SDK dois-je installer ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: 04a07d5b7f37222515136e5592f31a4583b02fe3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107521"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Quels packages Android SDK dois-je installer ?

L’installation du Kit Android SDK n’inclut pas automatiquement tous les packages requis minimales pour le développement. Bien que les développeurs individuels doivent varient, les packages suivants généralement seront nécessaires pour le développement avec Xamarin.Android :

## <a name="tools"></a>Outils

Installez les derniers outils à partir du dossier Outils dans le Gestionnaire du Kit de développement logiciel :

- Android SDK Tools
- Outils de plateforme du Kit Android SDK
- Outils de génération du Kit Android SDK

## <a name="android-platforms"></a>Plateformes Android

Installez la « plate-forme SDK » pour les versions Android que vous avez défini en tant que cible & minimum. 

Exemples :

- API 23 cible
- API minimal 23

Devez uniquement installer le Kit de développement logiciel Platform pour API 23

- API 23 cible
- API minimal 15

Devez installer des plateformes SDK pour API 15 et 23. Notez que vous n’avez pas besoin d’installer les niveaux d’API entre les valeurs minimale et la cible (même si vous êtes backporting à ces niveaux d’API).

## <a name="system-images"></a>Images du système

Elles sont uniquement requis si vous souhaitez utiliser les émulateurs Android out-of-the-box à partir de Google. Pour plus d’informations, consultez [configuration de l’émulateur Android](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Suppléments
Android SDK Extras ne sont généralement pas requis ; mais il est utile de connaître les dans la mesure où elles peuvent être requises en fonction de votre cas d’utilisation.

## <a name="further-reading"></a>informations supplémentaires
Le guide suivant décrit ces options et approfondit sur les différents packages du SDK manager est disponible : [Guide d’installation Android SDK Manager](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

