---
title: L’inscription d’une empreinte digitale
description: Comment définir jusqu'à un verrouillage d’écran et inscrire une empreinte digitale sur un émulateur ou un appareil Android.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 18903a7d8f6c4033dc3ac7c4c0187a247d023bc1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115911"
---
# <a name="enrolling-a-fingerprint"></a>L’inscription d’une empreinte digitale

## <a name="enrolling-a-fingerprint-overview"></a>L’inscription d’une vue d’ensemble de l’empreinte digitale

Il est uniquement possible pour une application Android tirer parti de l’authentification par empreinte digitale si l’appareil a déjà été configuré avec authentification par empreinte digitale. Ce guide explique comment inscrire une empreinte digitale sur un émulateur ou un appareil Android. Émulateurs n’ont pas le matériel réel pour effectuer une analyse d’empreintes digitales, mais il est possible de simuler une analyse par empreinte digitale à l’aide de Android Debug Bridge (décrits ci-dessous).  Ce guide explique comment activer le verrouillage d’écran sur un appareil Android et inscrire une empreinte digitale pour l’authentification.

## <a name="requirements"></a>Configuration requise

Pour inscrire une empreinte digitale, vous devez disposer d’un appareil Android ou un émulateur en cours d’exécution API de niveau 23 (Android 6.0).

L’utilisation de l’Android Debug Bridge (ADB) nécessite une bonne connaissance avec l’invite de commandes et le **adb** exécutable doit se trouver dans le chemin d’accès de votre interpréteur de commandes, PowerShell, ou environnement d’invite de commandes.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Configuration d’un verrouillage d’écran et l’inscription d’une empreinte digitale 

Pour configurer un verrouillage d’écran, procédez comme suit :

1. Accédez à **Paramètres > sécurité**, puis sélectionnez **écran de verrouillage**:

    ![Emplacement de sélection de verrouillage d’écran sur l’écran de sécurité](enrolling-fingerprint-images/testing-01.png)

2. L’écran suivant apparaît permettra de sélectionner et de configurer une des méthodes de sécurité de verrou écran : 

    ![Sélectionnez le balayage, modèle, code confidentiel ou mot de passe](enrolling-fingerprint-images/testing-02.png)

   Sélectionnez et effectuez une des méthodes de verrouillage d’écran disponible.

3. Une fois que le screenlock est configuré, revenez à la **Paramètres > sécurité** page et sélectionnez **empreinte digitale**:

    ![Emplacement de la sélection de l’empreinte digitale sur l’écran de sécurité](enrolling-fingerprint-images/testing-03.png)

4. À partir de là, suivez la séquence pour ajouter une empreinte digitale à l’appareil :

    [![Séquence de captures d’écran pour l’ajout d’une empreinte digitale à l’appareil](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Dans le dernier écran, vous êtes invité à placer votre doigt sur le lecteur d’empreintes digitales : 

    ![Écran qui vous invite à placer votre doigt sur le capteur](enrolling-fingerprint-images/testing-05.png)

    Si vous utilisez un appareil Android, terminez le processus en modifiant un doigt sur le scanneur. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulation d’une analyse de l’empreinte digitale sur l’émulateur

Sur un émulateur Android, il est possible de simuler une analyse de l’empreinte digitale à l’aide de Android Debug Bridge. Démarrage de l’OS X une session Terminal Server sur Windows Démarrer une invite de commandes ou une session Powershell et exécutez `adb`:

```shell
$ adb -e emu finger touch 1
```

La valeur de **1** est la _doigt\_id_ pour le doigt « analysé ». Il est un entier unique que vous attribuez à chaque empreinte virtuel. À l’avenir lors de l’application s’exécute, vous pouvez exécuter ce même ADB commande chaque fois que l’émulateur vous invite pour une empreinte digitale, vous pouvez exécuter la `adb` commande et passez-lui le _doigt\_id_ pour simuler l’analyse de l’empreinte digitale .

Une fois terminée, l’analyse de l’empreinte digitale Android vous avertir que l’empreinte digitale a été ajouté :  

![Écran affichant l’empreinte digitale ajouté !](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Récapitulatif 

Ce guide a décrit comment configurer un verrouillage d’écran et inscrire une empreinte digitale sur un appareil Android ou dans un émulateur Android. 

