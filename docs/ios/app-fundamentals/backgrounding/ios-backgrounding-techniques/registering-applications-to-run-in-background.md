---
title: "L’inscription des Applications à s’exécuter en arrière-plan"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: dddd1ad4ae70b97f17ba71a7e96b553759e35695
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="registering-applications-to-run-in-the-background"></a>L’inscription des Applications à s’exécuter en arrière-plan

L’inscription des tâches individuelles pour les travaux de privilèges en arrière-plan pour certaines applications, mais que se passe-t-il si une application est appelée en permanence pour effectuer des tâches importantes, longs, tels que l’obtention des instructions pour l’utilisateur via le système GPS ? Applications telles que celles-ci doivent plutôt être enregistrées comme des applications nécessaires à l’arrière-plan connues.

L’inscription d’une application signale au iOS que spéciaux de privilèges nécessaires pour effectuer des tâches en arrière-plan doit être émises à l’application.

## <a name="application-registration-categories"></a>Catégories d’inscription de l’application

Applications inscrites peuvent appartenir à plusieurs catégories :

-  **Audio** -lecteurs de musique et d’autres applications qui fonctionnent avec le contenu audio peuvent être inscrit pour poursuivre la lecture de données audio même quand l’application n’est plus au premier plan. Si une application de cette catégorie tente de faire quelque chose que lire des fichiers audio ou de téléchargement en arrière-plan, il se termine par iOS.
-  **VoIP** -aux applications de voix sur IP (VoIP) d’obtenir des privilèges mêmes accordées aux applications audio pour conserver le traitement audio en arrière-plan. Ils sont également autorisés à répondre en cas de besoin, aux services VoIP qu’eux, pour maintenir les connexions de l’alimentation.
-  **Externes accessoires et Bluetooth** -réservé pour les applications qui doivent communiquer avec les appareils Bluetooth et les autres périphériques externes, l’enregistrement sous ces catégories permet à l’application de rester connecté au matériel.
-  **Kiosque** -application d’un kiosque peut continuer à synchroniser le contenu en arrière-plan.
-  **Emplacement** : les Applications qui utilisent des GPS ou les données d’emplacement réseau peuvent envoyer et recevoir des mises à jour de l’emplacement en arrière-plan.
-  **FETCH (iOS 7 +)** : une application d’enregistrement pour des privilèges d’extraction en arrière-plan peuvent vérifier un fournisseur pour le nouveau contenu à intervalles réguliers, présentation de l’utilisateur avec le contenu mis à jour lorsqu’elles sont retournées à l’application.
-  **Notifications à distance (iOS 7 +)** -Applications peuvent s’inscrire pour recevoir des notifications d’un fournisseur et utilisez la notification pour déclencher une mise à jour avant que l’utilisateur ouvre l’application. Les notifications peuvent se présentent sous la forme de notifications push, ou s’abonner afin de sortir de l’application en mode silencieux.


Les applications peuvent être inscrits en définissant le **Modes d’arrière-plan requis** propriété dans l’application *Info.plist*. Une application peut enregistrer dans autant de catégories car il nécessite :

 [ ![](registering-applications-to-run-in-background-images/bgmodes.png "Définir les modes d’arrière-plan")](registering-applications-to-run-in-background-images/bgmodes.png)

Pour un guide pas à pas pour l’inscription d’une application pour les mises à jour en arrière-plan emplacement, consultez le [procédure pas à pas d’arrière-plan emplacement](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Application ne s’exécute pas dans la propriété d’arrière-plan

Une autre propriété qui peut être définie dans *Info.plist* est la *Application n’est pas exécuté en arrière-plan*, ou `UIApplicationExitsOnSuspend` propriété :

 [ ![](registering-applications-to-run-in-background-images/plist.png "La désactivation en arrière-plan en cours d’exécution")](registering-applications-to-run-in-background-images/plist.png)

Cela a exactement le même effet que le paramètre d’actualisation en arrière-plan application OFF dans iOS 7 +, mais il peut uniquement être modifié sur le côté du développeur et est disponible pour les e/s 4 et versions ultérieures. L’application sera suspendue immédiatement après l’entrée de l’arrière-plan et ne sera pas en mesure d’effectuer tout traitement.

Utilisez cette propriété si votre application n’est pas conçue pour gérer le traitement en arrière-plan, car elle permet d’éviter tout comportement inattendu.

## <a name="background-fetch-and-remote-notifications"></a>Extraction en arrière-plan et des Notifications à distance

Extraction en arrière-plan et des notifications à distance sont les catégories d’enregistrement spécial introduites dans iOS 7. Ces catégories permettent aux applications de recevoir le nouveau contenu à partir d’un fournisseur et mettre à jour en arrière-plan. La section suivante présente fetch et des notifications à distance de manière plus détaillée et présente également conscience de l’emplacement comme moyen de mise à jour d’une application en arrière-plan sur iOS 6.
