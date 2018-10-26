---
title: Firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM) est un service qui simplifie la messagerie entre les applications mobiles et les applications serveur. Cet article fournit une vue d’ensemble du fonctionne de FCM, et explique comment configurer les Services de Google afin que votre application peut utiliser FCM.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: bf0523a98c8e68691228b6e305265277e2925fa3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116111"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) est un service qui simplifie la messagerie entre les applications mobiles et les applications serveur. Cet article fournit une vue d’ensemble du fonctionne de FCM, et explique comment configurer les Services de Google afin que votre application peut utiliser FCM._

[![Image de héros firebase Cloud Messaging](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Cette rubrique fournit une vue d’ensemble de Firebase Cloud Messaging comment achemine les messages entre votre application Xamarin.Android et un serveur d’applications, et il fournit une procédure pas à pas pour l’acquisition des informations d’identification afin que votre application peut utiliser les services FCM.

## <a name="overview"></a>Vue d'ensemble

Firebase Cloud Messaging (FCM) est un service multiplateforme qui gère l’envoi, le routage et la file d’attente de messages entre les applications serveur et les applications clientes mobiles. FCM succède à Google Cloud Messaging (GCM), et il s’appuie sur Google Play Services.

Comme illustré dans le diagramme suivant, FCM fait Office d’intermédiaire entre les clients et les expéditeurs de messages. Un *application cliente* est une application prenant en charge FCM qui s’exécute sur un appareil. Le *du serveur d’applications* (fourni par vous ou votre société) est le serveur prenant en charge FCM que votre application cliente communique avec via FCM. Contrairement à GCM, FCM rend possible pour vous permettre d’envoyer des messages à des applications clientes directement via l’interface utilisateur graphique de Notifications de la Console Firebase :

[![FCM se situe entre l’application cliente et un serveur d’applications](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

À l’aide de FCM, serveurs d’applications peuvent envoyer des messages à un seul appareil, à un groupe d’appareils ou à un nombre d’appareils qui sont abonnés à une rubrique. Une application cliente peut utiliser FCM pour vous abonner aux messages en aval à partir d’un serveur d’applications (par exemple, pour recevoir des notifications à distance). Pour plus d’informations sur les différents types de messages de Firebase, consultez [propos des Messages FCM](https://firebase.google.com/docs/cloud-messaging/concept-options).

## <a name="fcm-in-action"></a>Firebase Cloud Messaging en action

Lorsqu’un message en aval est envoyé à une application cliente à partir d’un serveur d’applications, le serveur d’application envoie le message à un *connexion du serveur FCM* fournie par Google ; le serveur de connexion FCM, transfère à son tour, le message à un appareil qui exécute le application cliente. Messages peuvent être envoyés via HTTP ou [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging et protocole de présence). Étant donné que les applications clientes ne sont pas toujours connectées ou en cours d’exécution, les FCM connexion serveur empile et banques de messages, les envoyer à des applications clientes, car elles se reconnectent et deviennent disponibles. De même, FCM sera file d’attente des messages en amont à partir de l’application cliente sur le serveur d’application si le serveur d’application n’est pas disponible. Pour plus d’informations sur les serveurs de connexion de FCM, consultez [sur Firebase Cloud Messaging Server](https://firebase.google.com/docs/cloud-messaging/server).

FCM utilise les informations d’identification suivantes pour identifier le serveur d’applications et de l’application cliente et utilise ces informations d’identification pour autoriser les transactions de message via FCM :

-   <a name="fcm-in-action-sender-id"></a>**Sender ID** &ndash; le *Sender ID* est une valeur numérique unique qui est affectée lorsque vous créez votre projet Firebase. L’ID de l’expéditeur est utilisé pour identifier chaque serveur d’applications qui envoie des messages à l’application cliente. L’ID d’expéditeur est également le numéro de projet ; vous obtenir l’ID de l’expéditeur à partir de la Console Firebase lorsque vous enregistrez votre projet. Un exemple d’un ID de l’expéditeur est `496915549731`.

-   <a name="fcm-in-action-api-key"></a>**Clé API** &ndash; le *clé API* fournit l’accès de server application aux services de Firebase ; FCM utilise cette clé pour authentifier le serveur d’application. Ces informations d’identification sont également appelée le *clé serveur* ou *clé d’API Web*. Est un exemple d’une clé API `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   <a name="fcm-in-action-app-id"></a>**ID d’application** &ndash; l’identité de votre application cliente (indépendamment de n’importe quel appareil donné) qui s’inscrit pour recevoir des messages de FCM. Est un exemple d’un ID d’application `1:415712510732:android:0e1eb7a661af2460`.

-   <a name="fcm-in-action-registration-token"></a>**Jeton d’inscription** &ndash; le *le jeton d’inscription* (également appelé le *ID d’Instance*) est l’identité FCM de votre application cliente sur un appareil donné. Le jeton d’inscription est généré au moment de l’exécution &ndash; votre application reçoit un jeton d’inscription lorsqu’il enregistre tout d’abord auprès de FCM lors de l’exécution sur un appareil. Le jeton d’inscription autorise une instance de votre application cliente (en cours d’exécution sur cet appareil particulier) pour recevoir des messages à partir de FCM.
    Est un exemple d’un jeton d’inscription `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (une très longue chaîne).

[Paramètre de Firebase Cloud Messaging](#setup_fcm) (plus loin dans ce guide) fournit des instructions détaillées pour la création d’un projet et la génération de ces informations d’identification. Lorsque vous créez un nouveau projet dans le [Console Firebase](https://console.firebase.google.com/), un fichier d’informations d’identification appelé **google-services.JSON** est créé &ndash; ajouter ce fichier à votre projet Xamarin.Android, comme expliqué dans [ Notifications à distance auprès de FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

Les sections suivantes expliquent comment ces informations d’identification sont utilisées lorsque les applications clientes communiquent avec les serveurs d’applications via FCM.


<a name="registration" />

### <a name="registration-with-fcm"></a>Inscription auprès de FCM

Une application cliente doit tout d’abord s’inscrire auprès de FCM avant de messagerie peut avoir lieu. L’application cliente doit terminer la procédure d’inscription indiquée dans le diagramme suivant :

[![Diagramme d’étapes d’inscription App](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  L’application cliente contacte FCM pour obtenir un jeton d’inscription, en passant le sender ID, une clé d’API et un ID d’application à FCM.

2.  FCM retourne un jeton d’inscription à l’application cliente.

3.  L’application cliente transfère (éventuellement) le jeton d’inscription sur le serveur d’application.

Le serveur de l’application met en cache le jeton d’inscription pour les communications ultérieures avec l’application cliente. Le serveur d’application peut envoyer un accusé de réception à l’application cliente pour indiquer que le jeton d’inscription a été reçu. Une fois que cette liaison a lieu, l’application cliente peut recevoir des messages (ou envoyer des messages à) le serveur d’applications. L’application cliente peut recevoir un nouveau jeton d’inscription si l’ancien jeton est compromis (consultez [des Notifications à distance auprès de FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) pour obtenir un exemple de la façon dont une application reçoit des mises à jour de jeton d’inscription).

Lorsque l’application cliente ne souhaite plus que recevoir des messages à partir du serveur d’application, il peut envoyer une demande au serveur d’application pour supprimer le jeton d’inscription. Si l’application cliente est désinstallée d’un appareil, FCM détecte et notifie automatiquement le serveur d’application pour supprimer le jeton d’inscription.



### <a name="downstream-messaging"></a>Messagerie en aval

Le diagramme suivant illustre comment Firebase Cloud Messaging stocke et transfère les messages en aval :

[![FCM utilise stockage et redirection de messagerie en aval](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Lorsque le serveur d’application envoie un message en aval à l’application cliente, il utilise les étapes suivantes en tant qu’énumérées dans le diagramme ci-dessus :

1.  Le serveur d’application envoie le message à FCM.

2.  Si l’appareil client n’est pas disponible, le serveur FCM stocke le message dans une file d’attente pour une transmission ultérieure. Les messages sont conservés dans le stockage pour un maximum de 4 semaines FCM (pour plus d’informations, consultez [définissant la durée de vie d’un message](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Quand l’appareil client est disponible, FCM transfère le message à l’application cliente sur l’appareil.

4.  L’application cliente reçoit le message de FCM, traite et l’affiche à l’utilisateur. Par exemple, si le message est une notification à distance, il est présenté à l’utilisateur dans la zone de notification.

Dans ce scénario de messagerie (où le serveur d’application envoie un message à une application cliente unique), les messages peuvent être jusqu'à 4 Ko.

Pour plus d’informations sur la réception des messages FCM en aval sur Android, consultez [des Notifications à distance auprès de FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Rubrique de messagerie

*Rubrique Messaging* rend possible pour un serveur d’applications envoyer un message à plusieurs appareils qui ont souscrit à une rubrique particulière. Vous pouvez également composer et envoyer des messages de rubrique par le biais de l’interface utilisateur graphique de Notifications de la Console Firebase. FCM gère le routage et la remise des messages de rubrique aux clients auxquels vous êtes abonnés. Cette fonctionnalité peut être utilisée pour les messages, notamment les alertes de météo, les cotations boursières et les titres d’actualité.

[![Diagramme de messagerie de rubrique](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

Les étapes suivantes sont utilisées dans la rubrique messagerie (une fois que l’application cliente obtient un jeton d’inscription, comme expliqué plus haut) :

1.  L’application cliente s’abonne à une rubrique en envoyant un message de s’abonner à FCM.

2.  Le serveur d’application envoie des messages de rubrique à FCM pour la distribution.

3.  FCM transfère les messages de la rubrique sur les clients qui ont souscrit à cette rubrique.

Pour plus d’informations sur la messagerie de rubrique Firebase, consultez de Google [rubrique messagerie sur Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Configuration de Firebase Cloud Messaging

Avant de pouvoir utiliser les services FCM dans votre application, vous devez créer un nouveau projet (ou importer un projet existant) via le [Console Firebase](https://console.firebase.google.com/). Utilisez les étapes suivantes pour créer un projet Firebase Cloud Messaging pour votre application :

1.  Connectez-vous à la [Console Firebase](https://console.firebase.google.com/) avec votre compte Google (par exemple, votre adresse Gmail) et un clic **créer un nouveau projet**:

    [![Créer le bouton de nouveau projet](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Si vous avez un projet existant, cliquez sur **importer un projet Google**.

2.  Dans le **créer un projet** boîte de dialogue, entrez le nom de votre projet et cliquez sur **créer un projet**. Dans l’exemple suivant, un nouveau projet appelé **XamarinFCM** est créé :

    [![Créer une boîte de dialogue de projet](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Dans la Console Firebase **vue d’ensemble**, cliquez sur **ajouter Firebase à votre application Android**:

    [![Ajouter Firebase à votre application Android](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Dans l’écran suivant, entrez le nom du package de votre application. Dans cet exemple, le nom du package est **com.xamarin.fcmexample**. Cette valeur doit correspondre au nom de package de votre application Android. Un surnom de l’application peut également être entré dans le **surnom de l’application** champ :

    [![Entrant FCM exemple en tant que le surnom de l’application](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Si votre application utilise des liaisons dynamiques, invitations ou Google Auth, vous devez également entrer votre certificat de signature de débogage. Pour plus d’informations sur la localisation de votre certificat de signature, consultez [recherche de votre magasin de clés Signature MD5 ou SHA1](~/android/deploy-test/signing/keystore-signature.md).
    Dans cet exemple, le certificat de signature est vide.

6.  Cliquez sur **ajouter une application**:

    [![En cliquant sur le bouton Ajouter une application](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Une clé d’API de serveur et un ID de Client sont automatiquement générés pour l’application. Ces informations sont regroupées dans un **google-services.JSON** fichier qui est téléchargé automatiquement lorsque vous cliquez sur **ajouter une application**.
    Veillez à enregistrer ce fichier dans un endroit sûr.

Pour un exemple détaillé de l’ajout de **google-services.JSON** à un projet d’application pour recevoir des messages de notification push FCM sur Android, consultez [des Notifications à distance auprès de FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Pour plus d’informations

-   Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) fournit une vue d’ensemble des fonctionnalités de clés du Firebase Cloud Messaging, une explication de son fonctionnement et les instructions d’installation.

-   Google [générer les demandes application serveur envoyer](https://firebase.google.com/docs/cloud-messaging/send-message) explique comment envoyer des messages avec votre serveur d’application.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) et [RFC 6121](https://tools.ietf.org/html/rfc6121) expliquent et définissent les Extensible Messaging et la présence protocole XMPP ().

-   [À propos des Messages de FCM](https://firebase.google.com/docs/cloud-messaging/concept-options) décrit les différents types de messages qui peuvent être envoyés avec Firebase Cloud Messaging.

## <a name="summary"></a>Récapitulatif

Cet article fournit une vue d’ensemble de Firebase Cloud Messaging (FCM). Vous avez appris les informations d’identification différentes qui sont utilisées pour identifier et autoriser la messagerie entre les serveurs d’applications et les applications clientes. Il illustre l’inscription et les scénarios de messagerie en aval, et il les procédures détaillées pour l’inscription de votre application auprès de FCM à utiliser les services FCM.


## <a name="related-links"></a>Liens associés

- [Messagerie cloud Firebase](https://firebase.google.com/docs/cloud-messaging/)
