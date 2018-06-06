---
title: Technologies de Notification déconseillées dans Xamarin.iOS
description: Ce document décrit les technologies de notification iOS qui ont été déconseillées en faveur de l’infrastructure de Notifications à l’utilisateur, introduit dans iOS 10.
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2016
ms.openlocfilehash: 4becc5e296fb6e2496d9ffd863f7137419480262
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788551"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Technologies de Notification déconseillées dans Xamarin.iOS

Cette section montre comment implémenter local et des notifications push dans Xamarin.iOS. Elle explique les différents éléments d’interface utilisateur d’une notification d’e/s et traitent de l’API d’impliquée dans la création et affichage d’une notification.

> [!IMPORTANT]
> Les informations contenues dans cette section relative à iOS 9 et antérieure, il est resté ici pour prendre en charge les anciennes versions d’iOS. Pour iOS 10 et versions ultérieures, consultez le [guide utilisateur Notification Framework](~/ios/platform/user-notifications/index.md) pour prendre en charge à la fois Local et distant Notification sur un appareil iOS.

## <a name="sections"></a>Sections

<a name="Local Notifications In iOS" />

##  <a name="local-notifications-in-ioslocal-notifications-in-iosmd"></a>[Notifications locales dans iOS](local-notifications-in-ios.md)

Cette section explique comment implémenter des notifications locales dans Xamarin.iOS. Elle explique les différents éléments d’interface utilisateur d’une notification d’e/s et traitent de l’API d’impliquée dans la création et affichage d’une notification.

<a name="Local Notifications Walkthrough" />

##  <a name="walkthrough---using-local-notifications-in-xamarinioslocal-notifications-in-ios-walkthroughmd"></a>[Procédure pas à pas : utilisation des notifications locales dans Xamarin.iOS](local-notifications-in-ios-walkthrough.md)

Dans cette section, nous examinerons l’utilisation de notifications locales dans une application Xamarin.iOS. Il va vous montrer les principes fondamentaux de la création et la publication d’une notification s’affiche une alerte lors de la réception par l’application.

<a name="Remote Notifications In iOS" />

##  <a name="remote-notifications-in-iosremote-notifications-in-iosmd"></a>[Notifications à distance dans iOS](remote-notifications-in-ios.md)

Cette section couvre des notifications push dans iOS. Il présente les Apple Push Notifications passerelle de Service (APNS) et son rôle dans les notifications de publication pour les applications iOS. Il explique comment créer les certificats de sécurité nécessaires pour activer les notifications push et de discuter. Pour finir cette section décrit certaines des tâches de maintenance que les serveurs d’applications doivent effectuer pour effectuer le suivi des périphériques mobiles clients.

## <a name="related-links"></a>Liens associés

- [Notifications (exemple)](https://developer.xamarin.com/samples/monotouch/Notifications/)
