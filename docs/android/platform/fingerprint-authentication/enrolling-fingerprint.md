---
title: "L’inscription d’une empreinte digitale"
description: "Comment définir un verrouillage d’écran et de s’inscrire une empreinte digitale sur un émulateur ou un appareil Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20e6d693f2a3eba54afaf1d3c7054ad75d7a7610
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="enrolling-a-fingerprint"></a>L’inscription d’une empreinte digitale

## <a name="enrolling-a-fingerprint-overview"></a>L’inscription d’une vue d’ensemble de l’empreinte digitale

Il n’est possible pour une application Android tirer parti de l’authentification par empreinte digitale si l’appareil a déjà été configuré avec une authentification par empreinte digitale. Ce guide explique comment inscrire une empreinte digitale sur un émulateur ou un appareil Android. Émulateurs n’ont pas le matériel réel pour effectuer une analyse d’empreintes digitales, mais il est possible de simuler une analyse d’empreintes digitales, à l’aide de la Android Debug Bridge (décrites ci-dessous).  Ce guide explique comment activer le verrouillage d’écran sur un appareil Android et inscrire une empreinte digitale pour l’authentification.

## <a name="requirements"></a>Configuration requise

Pour inscrire une empreinte digitale, vous devez disposer d’un appareil Android ou un émulateur en cours d’exécution API de niveau 23 (Android 6.0).

L’utilisation de la Android Debug Bridge (ADB) nécessite une connaissance de l’invite de commandes et le **adb** exécutable doit se trouver dans le chemin d’accès de votre interpréteur de commandes PowerShell, ou environnement d’invite de commandes.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Configuration d’un verrouillage d’écran et l’inscription d’une empreinte digitale 

Pour configurer un verrouillage d’écran, procédez comme suit :

1. Accédez à **Paramètres > sécurité**, puis sélectionnez **écran de verrouillage**:

    ![Emplacement de sélection de verrouillage d’écran sur l’écran de sécurité](enrolling-fingerprint-images/testing-01.png)

2. L’écran suivant apparaît permettra sélectionnez et configurez une des méthodes de sécurité de verrou écran : 

    ![Sélection balayez, le modèle, code confidentiel ou mot de passe](enrolling-fingerprint-images/testing-02.png)

   Sélectionnez et effectuez une des méthodes de verrouillage d’écran disponible.

3. Une fois que le screenlock est configuré, revenez à la **Paramètres > sécurité** page et sélectionnez **par empreinte digitale**:

    ![Emplacement de la sélection de l’empreinte digitale sur l’écran de sécurité](enrolling-fingerprint-images/testing-03.png)

4. À partir de là, suivez la séquence pour ajouter une empreinte digitale à l’appareil :

    [![Séquence de captures d’écran pour l’ajout d’une empreinte digitale à l’appareil](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Dans le dernier écran, vous êtes invité à placer votre doigt sur le lecteur d’empreintes digitales : 

    ![Écran qui vous invite à placer votre doigt sur le capteur.](enrolling-fingerprint-images/testing-05.png)

    Si vous utilisez un appareil Android, terminez le processus en touchant un doigt sur l’analyseur. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulation d’une analyse d’empreinte digitale sur l’émulateur

Sur un émulateur Android, il est possible de simuler une analyse par empreinte digitale à l’aide de la passerelle du débogage Android. Au démarrage du système d’exploitation X une session Terminal Server sur Windows Démarrer une invite de commandes ou une session Powershell et exécutez `adb`:

```shell
$ adb -e emu finger touch 1
```

La valeur de **1** est la _doigt\_id_ pour le doigt « analysé ». Il est un entier unique que vous attribuez à chaque empreinte virtuel. Lorsque l’application est en cours d’exécution vous pouvez exécuter ce même ADB commande chaque fois que l’émulateur vous invite pour une empreinte digitale, vous pouvez exécuter la `adb` commande et lui passer le _doigt\_id_ pour simuler l’analyse de l’empreinte digitale .

Une fois terminée, l’analyse de l’empreinte digitale Android vous informera que l’empreinte digitale a été ajouté :  

![Écran affichant l’empreinte digitale ajouté !](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Récapitulatif 

Ce guide expliqué comment le programme d’installation d’un verrouillage d’écran et inscrire une empreinte digitale sur un appareil Android ou dans un émulateur Android. 

