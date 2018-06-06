---
title: Microsoft Azure et Xamarin
description: Ce document est lié à la documentation sur les Services connectés dans Visual Studio pour Mac, les applications mobiles Azure, l’authentification Active Directory et WebAPI.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: 25eec247a0cb664897541a0e6e818a77018fda43
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781076"
---
# <a name="microsoft-azure-and-xamarin"></a>Microsoft Azure et Xamarin

[ ![](images/evolve-mikej-azure-sml.png "Fonctionnalités des Services d’application Azure sont faciles à ajouter pour les applications Xamarin, y compris le stockage des données cloud et des notifications push d’inter-plateformes")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Faire évoluer 2016 : Développement d’applications connectées à l’aide d’Azure et Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Services connectés dans Visual Studio pour Mac

La nouvelle [Services connectés](connected-services.md) fonctionnalité de Visual Studio pour Mac permet aux développeurs de rapidement et facilement ajouter la fonctionnalité d’Azure à des applications mobiles à partir de l’IDE. Actuellement disponible pour les tests dans le canal Alpha.

## <a name="azure-app-services"></a>Services d’application Azure

Il existe une collection de [documentation des applications mobiles Azure](~/cross-platform/data-cloud/mobile-apps.md) qui vous guide tout au long du processus de mise en œuvre le [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin propose également des packages NuGet de messagerie un Azure pour [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) et [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) pour aider à implémenter des notifications push sur plusieurs plateformes.

Configurer vos applications sur le [portail des Services d’application Azure](https://portal.azure.com/) pour accéder aux applications mobiles, API Web, de stockage et bien plus encore. En savoir plus sur [comment les services d’application sont différents](http://azure.microsoft.com/updates/whats-new-with-azure-app-service/) et Regarder dans [ces vidéos de Microsoft](http://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Authentification Active Directory

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) peut être utilisé pour les utilisateurs de connexion dans des applications Xamarin via la [Xamarin.Auth composant](https://www.nuget.org/packages/Xamarin.Auth/).
Les applications peuvent ensuite accéder des services supplémentaires tels qu’Office 365.

## <a name="webapi"></a>WebAPI

Microsoft Web API expose une interface REST qui peut être facilement utilisée par les applications Xamarin.
Vous pouvez facilement accès un [site Web Azure](https://trywebsites.azurewebsites.net/) et générer une application basée sur les WebAPI pour se connecter à des applications Xamarin.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Introduction aux Services Web](~/cross-platform/data-cloud/web-services/index.md)

Ce didacticiel explique comment intégrer REST, WCF et SOAP web des technologies de service avec des applications mobiles Xamarin. Il examine les différentes implémentations de service, évalue les outils disponibles et les bibliothèques de les intégrer et fournit des exemples de modèles d’utilisation des données de service. Enfin, il fournit une vue d’ensemble de la création d’un service web RESTful pour la consommation d’une application mobile Xamarin.

## <a name="samples"></a>Exemples

Outre la [exemples de la documentation](https://github.com/xamarin/mobile-samples/tree/master/Azure), les applications complètes suivantes illustrent diverses fonctionnalités de Azure incorporées dans les applications Xamarin :

- [Sport](https://github.com/xamarin/Sport) – application de suivi des sports-et la ligue convivial qui utilise des notifications d’envoi et de stockage de données.
- [Instants](https://github.com/pierceboggan/Moments) – photo instantanée partage qui utilise le stockage Azure pour les images.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – utilise des API Web pour le serveur principal.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -des applications mobiles Azure.

- [Shopping](https://github.com/dotnet-architecture/eShopOnContainers) – exemple pour le [Architecture série](https://www.microsoft.com/net/learn/architecture) de livres électroniques.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IoT un échantillonnage à partir de 2016 de Build.


## <a name="related-links"></a>Liens associés

- [Exemple de bibliothèque de classes portables Azure (par @paulbatum) (exemple)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Portail Azure](http://azure.microsoft.com/)
- [Client mobile pour Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
