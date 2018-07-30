---
title: Aperçu de la distribution OpenJDK de Microsoft
description: Guide pas à pas pour la configuration de la distribution OpenJDK de Microsoft.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 6c1346918ca6881e551f6c5b89ab16ad13d3f804
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242512"
---
# <a name="microsofts-openjdk-distribution-preview"></a>Aperçu de la distribution OpenJDK de Microsoft

_Ce guide décrit les étapes permettant de passer à la préversion de la distribution OpenJDK de Microsoft._

![Fonctionnalité en préversion](~/media/shared/preview.png)

## <a name="overview"></a>Vue d'ensemble

À partir de Visual Studio 15.9 et Visual Studio pour Mac 7.7, Visual Studio Tools pour Xamarin passera du kit JDK d’Oracle à une version légère d’OpenJDK, destinée uniquement au développement Android :

![Nouveau flux de travail offrant un aperçu web d’OpenJDK dans VS 15.8 préversion 5](openjdk-images/openjdk.png)

Cette migration présente les avantages suivants :

- Vous aurez toujours une version d’OpenJDK qui fonctionne pour le développement Android.

- Le téléchargement du kit JDK 9 ou 10 est sans incidence sur l’expérience de développement.

- Cela réduit considérablement l’encombrement et la taille du téléchargement.

- Il n’y a plus de problème avec les programmes d’installation et les serveurs tiers.

Si vous souhaitez passer plus tôt à l’expérience améliorée, vous pouvez tester les builds de la distribution OpenJDK de Microsoft sous Windows et Mac. Le processus d’installation est décrit ci-dessous. Vous pourrez revenir au kit JDK Oracle à tout moment.

## <a name="download"></a>Téléchargement

Pour commencer, téléchargez la build correspondant à votre système :

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>Configurer

Décompressez à l’emplacement approprié :

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> Cet exemple utilise la build 1.8.0.9 ; toutefois, il est possible que vous téléchargiez une version plus récente.

Faites pointer l’environnement IDE sur le nouveau kit JDK :

- **Mac** &ndash; Cliquez sur **Outils > Gestionnaire des kits SDK > Emplacements** et, dans **Emplacement du kit SDK Java (JDK)**, indiquez le chemin d’accès complet de l’installation d’OpenJDK. Dans l’exemple suivant, ce chemin est défini sur **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**.

![Définir le chemin du JDK pour la distribution OpenJDK de Microsoft sur Mac](openjdk-images/vsm.png)

- **Windows** &ndash; Cliquez sur **Outils > Options > Xamarin > Paramètres Android** et, dans **Emplacement du kit de développement Java**, indiquez le chemin d’accès complet de l’installation d’OpenJDK. Dans l’exemple suivant, ce chemin est défini sur **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9** :

![Définir le chemin du JDK pour la distribution OpenJDK de Microsoft sous Windows](openjdk-images/vs.png)

## <a name="revert"></a>Revenir à la dernière version

Pour rétablir le JDK Oracle, remplacez l’emplacement du kit SDK Java par le chemin d’accès du kit JDK Oracle utilisé précédemment et régénérez la solution. Sur Mac, vous pouvez revenir au chemin d’accès du kit JDK Oracle en cliquant sur **Rétablir les valeurs par défaut**.

Si vous rencontrez des problèmes avec la distribution OpenJDK de Microsoft, signalez les problèmes à l’aide de l’Outil Commentaires de votre environnement IDE pour qu’ils fassent l’objet d’un suivi et d’une correction rapide.

## <a name="known-issues--planned-fix-dates"></a>Problèmes connus et dates de correction prévues

Il peut arriver que la variable d’environnement `JAVA_HOME` ne s’exporte pas correctement vers le kit SDK et le Gestionnaire de périphériques. Pour contourner le problème, vous pouvez la définir à l’emplacement du kit OpenJDK sur votre ordinateur. Ce problème est résolu dans les préversions 15.9.

## <a name="summary"></a>Récapitulatif

Dans cet article, vous avez appris à configurer votre environnement IDE de façon à utiliser la préversion de la distribution OpenJDK de Microsoft, dont une version plus stable est prévue dans le courant de l’année 2018.
