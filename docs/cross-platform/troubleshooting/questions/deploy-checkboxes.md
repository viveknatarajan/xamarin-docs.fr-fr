---
title: Déployer des cases à cocher désactivée dans le Gestionnaire de Configuration
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: c0f116565a2741c62a00ed2a255cfde8c57b8569
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Déployer des cases à cocher désactivée dans le Gestionnaire de Configuration

Depuis Xamarin 3.5, Xamarin.iOS projets déployés automatiquement chaque fois que vous appuyez sur la **Démarrer** bouton de barre d’outils ou le choix le **Déboguer > Démarrer le débogage** élément de menu. Vous devez toujours définir le projet d’application Xamarin.iOS souhaité comme le **projet de démarrage** avant avant d’exécuter des deux commandes.

Pour cette raison, le **déployer** cases à cocher sont désactivées intentionnellement dans le Gestionnaire de Configuration Visual Studio pour les projets de Xamarin.iOS :

![](deploy-checkboxes-images/configuration.png "Gestionnaire de Configuration Visual Studio affichant la case à cocher « Déployer » désactivée pour un projet de Xamarin.iOS dans Xamarin 3.5")

Cette modification élimine une erreur peut apparaître dans les versions antérieures de Xamarin (version 3.3 et versions antérieure) lorsque le projet d’application Xamarin.iOS n’a pas été défini à déployer :

![](deploy-checkboxes-images/error.png "Message d’erreur : le projet iPhoneApp1 doit être déployé avant de pouvoir être démarré. Vérifiez que le projet est sélectionné pour être déployé dans le Gestionnaire de Configuration de Solution.")
