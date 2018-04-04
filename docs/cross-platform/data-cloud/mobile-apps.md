---
title: Applications mobiles Microsoft Azure
description: Exemples et code téléchargements pour la documentation du portail Azure.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: 14ae6bdb3419bac13aebe6b5a9d8346ed5163de5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Applications mobiles Microsoft Azure

_Exemples et code téléchargements pour la documentation du portail Azure._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Ces liens sont pour la documentation de Xamarin disponible sur le [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) site Web.
Ajout d’une fonctionnalité Azure à une application Xamarin en téléchargeant le [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Utilisation du composant Xamarin Azure

Documentation générale [utilisation de la bibliothèque cliente de Xamarin (composant)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) pour effectuer diverses tâches liées aux applications mobiles Azure. Cette page contient un grand nombre d’exemples d’extraits de code, sans les explications détaillées et les exemples disponibles dans chacun des articles de procédure pas à pas répertoriées ci-dessous.

## <a name="getting-started"></a>Prise en main

Cet article fournit des instructions pas à pas pour obtenir votre première application Azure de Xamarin haut et en cours d’exécution.
Elle couvre la création d’une application Mobile Azure dans le portail de téléchargement et exécution de l’application préconfigurée.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Prise en main les utilisateurs

Fournit des instructions complètes pour la configuration et le codage d’un écran de connexion à l’aide d’Azure Mobile Services. Fournisseurs d’authentification pris en charge incluent Microsoft, Google, Facebook et Twitter.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autoriser les utilisateurs dans les Scripts

Un exemple de code pour les serveurs principaux de Javascript

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Prise en main par émission de données

Suivez les instructions pour configurer des notifications push sur les sites Web Apple et Google, puis envoyer une notification push à partir d’Azure Mobile Services à un appareil.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Prise en main des concentrateurs de Notification

Suivez les instructions pour configurer des notifications push sur les sites Web Apple et Google, configurez le Hub de Notification Azure puis générer des notifications push aux appareils.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>Liens associés

- [Mise en route (exemple)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (exemple)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Clients mobiles Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Chemin d’accès de l’apprentissage d’applications mobiles Azure](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->