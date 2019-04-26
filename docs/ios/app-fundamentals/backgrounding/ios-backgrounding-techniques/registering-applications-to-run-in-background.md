---
title: L’inscription d’applications Xamarin.iOS à exécuter en arrière-plan
description: Ce document décrit comment inscrire une application Xamarin.iOS s’exécuter en arrière-plan. Il aborde Audio applications VoIP applications, Accessoires externes et bluetooth et bien plus encore.
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a0a66571d0249ef6fd65ff382f14c38f48a8af37
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61393700"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>L’inscription d’applications Xamarin.iOS à exécuter en arrière-plan

L’inscription des tâches individuelles pour les travaux de privilèges en arrière-plan pour certaines applications, mais que se passe-t-il si une application est appelée en permanence à effectuer des tâches importantes et de longue durée, telles que l’obtention des instructions relatives à l’utilisateur par le biais de GPS ? Applications telles que celles-ci doivent plutôt être enregistrées comme applications nécessaire à l’arrière-plan connues.

Inscription d’une application signale à iOS, que l’application doit disposer des privilèges spéciaux nécessaires pour effectuer des tâches en arrière-plan.

## <a name="application-registration-categories"></a>Catégories d’inscription de l’application

Applications inscrites peuvent appartenir à plusieurs catégories :

-  **Audio** -lecteurs de musique et d’autres applications qui fonctionnent avec contenu audio peuvent être inscrit pour poursuivre la lecture audio même quand l’application n’est plus au premier plan. Si une application dans cette catégorie tente de faire quelque chose autre que de lire des fichiers audio ou de téléchargement en arrière-plan, il se termine par iOS.
-  **VoIP** -obtiennent des applications de voix sur IP (VoIP) des mêmes privilèges accordées aux applications audio à continuer de traiter l’audio en arrière-plan. Ils sont également autorisés à répondre si nécessaire aux services VoIP qui alimentent les, pour maintenir leurs connexions.
-  **Accessoires et Bluetooth externes** -réservé pour les applications qui doivent communiquer avec les périphériques Bluetooth et autres accessoires de matériel externe, l’inscription sous ces catégories autorise l’application à rester connecté à du matériel.
-  **Newsstand** -application d’un kiosque peut continuer à synchroniser le contenu en arrière-plan.
-  **Emplacement** : les Applications qui utilisent de GPS ou les données d’emplacement réseau peuvent envoyer et recevoir des mises à jour de l’emplacement en arrière-plan.
-  **FETCH (iOS 7 +)** : une application inscrite pour des privilèges d’extraction en arrière-plan peuvent vérifier un fournisseur pour nouveau contenu à intervalles réguliers, présentant l’utilisateur avec le contenu mis à jour leur retour à l’application.
-  **Notifications à distance (iOS 7 +)** -Applications peuvent s’inscrire pour recevoir des notifications à partir d’un fournisseur et utilisez la notification pour lancer une mise à jour avant que l’utilisateur ouvre l’application. Notifications peuvent se présentent sous la forme de notifications push, ou choisir de mettre en éveil de l’application en mode silencieux.


Les applications peuvent être inscrits en définissant le **Required Background Modes** propriété dans l’application *Info.plist*. Une application peut enregistrer dans autant de catégories car il nécessite :

 [![](registering-applications-to-run-in-background-images/bgmodes.png "Définir les modes d’arrière-plan")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

Pour obtenir un guide pas à pas pour l’inscription d’une application pour les mises à jour en arrière-plan, consultez le [procédure pas à pas de d’arrière-plan emplacement](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Application ne s’exécute pas dans la propriété d’arrière-plan

Une autre propriété qui peut être définie dans *Info.plist* est la *Application ne s’exécute pas en arrière-plan*, ou `UIApplicationExitsOnSuspend` propriété :

 [![](registering-applications-to-run-in-background-images/plist.png "La désactivation en cours d’exécution en arrière-plan")](registering-applications-to-run-in-background-images/plist.png#lightbox)

Cela a exactement le même effet que de définir le paramètre d’application d’actualisation en arrière-plan à désactivée dans iOS 7 +, mais il peut uniquement être modifié sur le côté développeur et est disponible pour iOS 4 et versions ultérieures. L’application sera suspendue immédiatement après l’entrée de l’arrière-plan et ne sera pas en mesure d’effectuer tout traitement.

Utilisez cette propriété si votre application n’est pas conçue pour gérer le traitement en arrière-plan, car elle permet d’éviter tout comportement inattendu.

## <a name="background-fetch-and-remote-notifications"></a>Récupération en arrière-plan et des Notifications à distance

Récupération en arrière-plan et des notifications à distance sont les catégories d’enregistrement spécial introduits dans iOS 7. Ces catégories permettent aux applications de recevoir le nouveau contenu à partir d’un fournisseur et mettre à jour en arrière-plan. La section suivante explore l’extraction et des notifications à distance plus en détail et présente également connaissance des emplacements comme moyen de mise à jour d’une application en arrière-plan sur iOS 6.
