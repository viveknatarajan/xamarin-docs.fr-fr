---
title: Fonctionnalités App Group dans Xamarin.iOS
description: L’ajout de fonctionnalités à une application nécessite souvent une configuration supplémentaire du provisionnement. Ce guide explique la configuration requise pour les fonctionnalités App Group.
ms.prod: xamarin
ms.assetid: 0A61220B-BBAC-492B-9D3B-578986E64064
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 4ce04f21a3e520fea9da5d538fb7cc0ac098ad31
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119835"
---
# <a name="app-group-capabilities-in-xamarinios"></a>Fonctionnalités App Group dans Xamarin.iOS

_L’ajout de fonctionnalités à une application nécessite souvent une configuration plus poussée de l’approvisionnement. Ce guide explique la configuration requise pour les fonctionnalités App Group._

Un App Group est un groupe qui permet à différentes applications (ou à une application et ses extensions) d’accéder à un emplacement partagé du stockage de fichiers. Vous pouvez utiliser des App Groups pour les données suivantes, par exemple :

*   [Paramètres Apple Watch](~/ios/watchos/app-fundamentals/settings.md)
*   [Valeurs NSUserDefaults partagées](~/ios/app-fundamentals/user-defaults.md)
*   [Fichiers partagés](~/ios/watchos/app-fundamentals/parent-app.md#files)

## <a name="configure-a-new-app-group"></a>Configurer un nouvel App Group

Pour configurer l’emplacement partagé, utilisez un  [App Group (Groupe d’applications)](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), qui est lui-même configuré dans la section  **Certificates, Identifiers & Profiles (Certificats, identificateurs et profils)**  du  [Centre des développeurs Apple](https://developer.apple.com/account/). Cette valeur doit également être référencée dans le fichier Entitlements.plist de chaque projet.

Le groupe d’applications a un identificateur, qui correspond généralement à l’ID de bundle avec un groupe. « group. ». Par exemple, l’ID de bundle `com.xamarin.WatchSettings` a pour groupe d’applications  `group.com.xamarin.WatchSettings`.

Pour créer un App Group, effectuez les étapes suivantes :

1.  Accédez au  [Centre des développeurs iOS](https://developer.apple.com/account/) d’Apple et connectez-vous à l’aide de votre  **compte** .
2.  Sélectionnez **Certificates, Identifiers & Profiles**.
3.  Sous **Identifiers (Identificateurs)** sélectionnez  **App Groups (Groupes d’applications)** et cliquez sur le bouton **+**  pour créer un groupe.
4.  Entrez un  **Name (Nom)** et un **Identifier (Identificateur)** pour le nouveau groupe, puis cliquez sur le bouton  **Continue (Continuer)** : 
   
    ![Détails de l’ajout d’un App Group](app-groups-capabilities-images/image52.png)

5.  Cliquez sur le bouton **Register (Inscrire)** pour créer le groupe, puis sur  **Done (Terminé)** pour retourner à la liste des groupes d’applications inscrits.

## <a name="configure-an-app-to-use-app-groups"></a>Configurer une application pour utiliser des App Groups

Une fois que vous avez créé l’App Group, configurez les ID d’application pour permettre aux applications de l’utiliser.

Effectuez ce qui suit :

1.  Accédez au  [Centre des développeurs iOS](https://developer.apple.com/account/) d’Apple, puis connectez-vous à l’aide d’un compte de développeur Apple.
2.  Dans le menu **Program Resources (Ressources de programme)** , sélectionnez **Certificates, IDs & Profiles (Certificats, ID et profils)**.
3.  Sous **Identifiers (Identificateurs)** sélectionnez  **App IDs (ID d’applications)** et cliquez sur le bouton **+**  pour créer un ID.
4.  Entrez un nom pour l’ID d’application et attribuez-lui un ID d’application explicite.
5.  Sous **App Services** activez **App Groups (Groupes d’applications)**, puis cliquez sur le bouton Continue (Continuer) :

    ![Ajouter un App Group (App Services)](app-groups-capabilities-images/image53.png)

6.  Vérifiez les paramètres, puis cliquez sur le bouton  **Register (Inscrire)** pour créer l’ID d’application.
7.  Cliquez sur le bouton **Done (Terminé)** pour retourner à la liste des ID d’applications inscrits.
8.  Sélectionnez dans la liste l’ID d’application créé, puis cliquez sur le bouton  **Edit (Modifier)** :

    ![Sélectionner l’ID d’application dans la liste](app-groups-capabilities-images/image54.png)

9.  Sous le service **App Group (Groupe d’applications)**, cliquez sur le bouton **Edit (Modifier)** :

    ![Sélectionner l’ID d’application dans la liste](app-groups-capabilities-images/image55.png)

10. Sélectionnez le groupe d’applications créé plus haut, puis cliquez sur le bouton  **Continue (Continuer)** :

    ![Ajouter un App Group](app-groups-capabilities-images/image56.png)

11. Cliquez sur le bouton  **Assign (Affecter)** , puis sur le bouton  **Done (Terminé)** pour retourner à la liste des ID d’applications inscrits.
12. Répétez ces étapes pour chaque application (ou extension) devant utiliser l’App Group.

## <a name="next-steps"></a>Étapes suivantes
 
S’il y a lieu, effectuez les étapes supplémentaires décrites ci-après :

* Utilisez l’espace de noms de framework dans votre application.
* Ajoutez les droits nécessaires à votre application. Pour obtenir des informations détaillées sur les droits nécessaires et leur ajout, consultez le guide [Utilisation de droits](~/ios/deploy-test/provisioning/entitlements.md).
* Dans la section  **Signature du bundle iOS** de l’application, vérifiez que le paramètre  **Droits personnalisés** a la valeur **Entitlements.plist**. Il ne s’agit  _pas_ du paramètre par défaut pour les builds Debug et iOS Simulator.

Si vous rencontrez des problèmes avec les services d’application, consultez la section [Dépannage](~/ios/deploy-test/provisioning/capabilities/index.md) du guide principal.