---
title: Déployer des cases à cocher désactivées dans le Gestionnaire de Configuration
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 35efb00a721062ad3217300f7e3a5430b1bd1560
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528141"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Déployer des cases à cocher désactivées dans le Gestionnaire de Configuration

Depuis Xamarin 3.5, les projets Xamarin.iOS déploiement automatiquement chaque fois que vous appuyez sur la **Démarrer** bouton de barre d’outils ou choisissez la **Déboguer > Démarrer le débogage** élément de menu. Vous devez toujours définir le projet d’application Xamarin.iOS souhaité en tant que le **projet de démarrage** avant d’exécuter une de ces commandes.

Pour cette raison, le **déployer** cases à cocher sont désactivées intentionnellement dans le Gestionnaire de Configuration Visual Studio pour les projets Xamarin.iOS :

![](deploy-checkboxes-images/configuration.png "Gestionnaire de Configuration Visual Studio montrant la case à cocher « Déployer » désactivée pour un projet Xamarin.iOS dans Xamarin 3.5")

Cette modification élimine une erreur qui peut apparaître dans les versions antérieures de Xamarin (version 3.3 et versions antérieure) lorsque le projet d’application Xamarin.iOS n’a pas été défini à déployer :

![](deploy-checkboxes-images/error.png "Message d’erreur : le projet iPhoneApp1 doit être déployé avant de pouvoir être démarré. Vérifiez que le projet est sélectionné pour être déployé dans le Gestionnaire de Configuration de Solution.")
