---
title: Connexion des Services dans Visual Studio pour Mac
description: "Ajouter un stockage de données Azure, l’authentification et des notifications push aux applications mobiles à partir de Visual Studio pour Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 5c9ae8d65a2bfdd035cccb6b911448ea047f8c40
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="connected-services-walkthrough"></a>Procédure pas à pas de Services connectés

Le flux de travail des Services connectés met le flux de travail portail Azure dans Visual Studio pour Mac, afin que vous n’êtes pas obligé de laisser votre projet pour ajouter des services.

Cette procédure pas à pas montre comment ajouter un service Azure principal, qui offre le stockage cloud des données, l’authentification et des notifications push à une application de bibliothèque de classe Portable (PCL) Xamarin.Forms inter-plateformes.


1.  Commencez par double-clic sur le **Services connectés** nœud dans la solution, qui s’affiche la **galerie Services**.
  Il s’agit d’une liste de tous les services disponibles pour ce type d’application. Sélectionnez un service (tel que **backend Mobile avec Azure App Service**) en cliquant dessus.

  [![](connected-services-images/image001-sml.png "Connecté nœud Services dans Visual Studio pour Mac")](connected-services-images/image001.png#lightbox)

2. La Page Détails du Service comporte une description du service et les dépendances à installer.
  Cliquez sur le **ajouter** pour ajouter les dépendances à l’application :

  [![](connected-services-images/image002-sml.png "Service principal mobile avec Azure")](connected-services-images/image002.png#lightbox)

3. Les dépendances doivent être ajoutés à la bibliothèque de classes portables et les projets spécifiques à une plateforme pour fonctionner.
  Sélectionnez les cases à cocher pour ajouter le service à chaque projet qui référence (directement ou indirectement) :

  [![](connected-services-images/image003-sml.png "Vérifiez tous les projets qui doivent référencer le service")](connected-services-images/image003.png#lightbox)

4. Choisissez **accepter** sur la **acceptation de licence** boîtes de dialogue pour les packages NuGet.
  Il peut y avoir deux boîtes de dialogue pour accepter un pour le Clientmobile et les dépendances et l’autre pour SQLiteStore, ce qui est nécessaire pour la synchronisation de données hors connexion :

  [![](connected-services-images/image004-sml.png "Acceptez les contrats de licence")](connected-services-images/image004.png#lightbox)

  ![](connected-services-images/image005.png "Fenêtre d’acceptation de licence")

5. Une fois que les dépendances ont été ajoutés, vous devez ouvrir une session avec le compte que vous souhaitez utiliser pour communiquer avec Azure.
  Si vous êtes déjà connecté avec un ID Microsoft, Visual Studio pour Mac va tenter de récupérer vos abonnements Azure et les services d’application qui s’y rapportent. Si vous n’avez pas d’abonnements, vous pouvez ajouter un en vous inscrivant pour une évaluation gratuite ou acheter un abonnement dans le portail Azure.

6. Sélectionnez un service d’application dans la liste. Cela remplira le code du modèle pour le `MobileServiceClient` objet avec l’URL correspondante du service application sur Azure :

  [![](connected-services-images/image006-sml.png "Sélectionnez le service d’applications à partir de la liste")](connected-services-images/image006.png#lightbox)

  S’il n’y a aucun service répertorié, cliquez sur le **nouveau** bouton (voir l’étape 9.)

7. Copiez le code de modèle pour le `MobileServiceClient` dans la bibliothèque PCL. L’application d’emplacement de fichier dans pas important, tant n'est qu’une seule instance de celui-ci.
  L’approche recommandée consiste à créer un `AzureService` classe qui gère toutes les interactions de Azure et utilise le `MobileServiceClient`:

  ![](connected-services-images/image007.png "Copier le code de configuration de l’application")

8. Suivre la documentation **étapes** pour ajouter des données, la synchronisation hors connexion, l’authentification et des notifications push à votre application :

  [![](connected-services-images/image008-sml.png "Passez en revue les instructions de procédure suivantes")](connected-services-images/image008.png#lightbox)

10. Si vous n’avez pas tous les services d’application existant, vous pouvez créer de nouveaux services à partir de Visual Studio pour Mac.
  Cliquez sur le **nouveau** bouton dans la partie inférieure gauche de la liste des services pour ouvrir la **nouveau Service App** boîte de dialogue :

  [![](connected-services-images/image009-sml.png "Créer un nouveau service d’application dans Visual Studio pour Mac")](connected-services-images/image009.png#lightbox)

Un nouveau service requiert les paramètres suivants :

-   **Nom du service application** – nom/id unique pour le plan
-   **Abonnement** – l’abonnement que vous souhaitez utiliser pour le service
-   **Groupe de ressources** – une manière ou organisation de toutes vos ressources Azure pour un projet. Option pour utiliser l’existant ou créez-en un. S’il s’agit de votre premier service Azure, créez-en un.
-   **Plan de service** – détermine l’emplacement et le coût des ressources qui l’utilisent. Option pour utiliser l’existant ou créez-en un. S’il s’agit de votre premier service Azure, utilisez celle par défaut ou créer un nouveau dans le niveau gratuit (F1).

Visitez le [documentation d’Azure App Service](https://docs.microsoft.com/azure/app-service/) pour plus d’informations


## <a name="related-links"></a>Liens associés

- [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/)
- [Notes de publication](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
