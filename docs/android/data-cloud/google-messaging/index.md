---
title: Messagerie Google
description: Cette section contient des guides qui expliquent comment implémenter des applications Xamarin.Android à l’aide des services de messagerie de Google.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: 8c2e3705f0f867b6993d0bdcb22f6672c853498e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61016737"
---
# <a name="google-messaging"></a>Messagerie Google

_Cette section contient des guides qui expliquent comment implémenter des applications Xamarin.Android à l’aide des services de messagerie de Google._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Messagerie cloud Firebase](firebase-cloud-messaging.md)

Firebase Cloud Messaging (FCM) est un service qui simplifie la messagerie entre les applications mobiles et les applications serveur. FCM est le successeur de Google à Google Cloud Messaging. Cet article fournit une vue d’ensemble du fonctionne de FCM, et il fournit une procédure pas à pas pour l’acquisition des informations d’identification afin que votre application peut utiliser les services FCM.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Notifications à distance avec Firebase Cloud Messaging](remote-notifications-with-fcm.md)

Cette procédure pas à pas fournit une explication pas à pas montrant comment utiliser Firebase Cloud Messaging pour implémenter des notifications à distance (également appelées notifications push) dans une application Xamarin.Android. Il illustre comment implémenter les différentes classes qui sont nécessaires pour les communications avec Firebase Cloud Messaging (FCM), fournit des exemples montrant comment configurer le manifeste Android pour accéder à FCM et montre en aval de la messagerie à l’aide de la Firebase Console.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Messagerie cloud Google](google-cloud-messaging.md)

Cette section fournit une vue d’ensemble de la façon dont Google Cloud Messaging (GCM) achemine les messages entre votre application et un serveur d’applications, et il fournit une procédure pas à pas pour l’acquisition des informations d’identification afin que votre application peut utiliser les services GCM. (Notez que GCM a été remplacé par FCM).

> [!NOTE]
> GCM a été remplacé par [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serveur et client API [ont été déconseillées](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) et ne sera plus disponible dès que le 11 avril 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Notifications à distance avec Google Cloud Messaging](remote-notifications-with-gcm.md)

Cette section fournit une explication pas à pas montrant comment implémenter des notifications à distance dans Xamarin.Android à l’aide de Google Cloud Messaging.
Elle explique les différents composants qui doivent être exploitées pour activer Google Cloud Messaging dans une application Android.


