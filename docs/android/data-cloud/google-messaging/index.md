---
title: Messagerie Google
description: Cette section contient des directives qui décrivent comment implémenter des applications de Xamarin.Android à l’aide des services de messagerie de Google.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: cf1eaec3dfee7c3457a4614147c9b5564843b2a7
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044663"
---
# <a name="google-messaging"></a>Messagerie Google

_Cette section contient des directives qui décrivent comment implémenter des applications de Xamarin.Android à l’aide des services de messagerie de Google._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Messagerie cloud Firebase](firebase-cloud-messaging.md)

Messagerie de Cloud firebase (FCM) est un service qui simplifie la messagerie entre les applications mobiles et des applications serveur. FCM est le successeur de Google messagerie Cloud Google. Cet article fournit une vue d’ensemble du fonctionne de FCM, et il fournit une procédure pas à pas pour l’acquisition d’informations d’identification afin que votre application peut utiliser les services FCM.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Notifications à distance avec Firebase de cloud computing de messagerie](remote-notifications-with-fcm.md)

Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser la messagerie de Cloud Firebase pour implémenter des notifications à distance (également appelées des notifications push) dans une application Xamarin.Android. Il montre comment implémenter les classes différentes qui sont nécessaires pour les communications avec la messagerie de Cloud Firebase (FCM) et fournit des exemples montrant comment configurer le manifeste Android pour l’accès à FCM montre en aval de la messagerie à l’aide de la Firebase Console.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Messagerie cloud Google](google-cloud-messaging.md)

Cette section fournit une vue d’ensemble de la façon dont Google Cloud Messaging (GCM) achemine les messages entre votre application et un serveur d’applications, et il fournit une procédure pas à pas pour l’acquisition d’informations d’identification afin que votre application peut utiliser les services GCM. (Notez que GCM a été remplacé par FCM.)

> [!NOTE]
> GCM a été remplacé par [Firebase de messagerie Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serveur et le client API [ont été déconseillées](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) et ne sera plus disponible dès que le 11 avril 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Notifications à distance avec la messagerie Cloud Google](remote-notifications-with-gcm.md)

Cette section fournit une explication pas à pas montrant comment implémenter des notifications à distance dans Xamarin.Android à l’aide de la messagerie Cloud Google.
Il explique les différents composants qui doivent être exploitées pour activer la messagerie Cloud Google dans une application Android.


