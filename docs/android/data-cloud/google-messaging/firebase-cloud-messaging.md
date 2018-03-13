---
title: Firebase de cloud computing de messagerie
description: "Messagerie de Cloud firebase (FCM) est un service qui simplifie la messagerie entre les applications mobiles et des applications serveur. Cet article fournit une vue d’ensemble du fonctionne de FCM, et elle explique comment configurer les Services Google afin que votre application peut utiliser FCM."
ms.topic: article
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: ef073999e10e184806089bed3ef9ab93e2f28dd6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="firebase-cloud-messaging"></a>Firebase de cloud computing de messagerie

_Messagerie de Cloud firebase (FCM) est un service qui simplifie la messagerie entre les applications mobiles et des applications serveur. Cet article fournit une vue d’ensemble du fonctionne de FCM, et elle explique comment configurer les Services Google afin que votre application peut utiliser FCM._

[![Image de héros firebase messagerie Cloud](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Cette rubrique fournit une vue d’ensemble de la messagerie de Cloud Firebase achemine les messages entre votre application Xamarin.Android et un serveur d’applications, et il fournit une procédure pas à pas pour l’acquisition d’informations d’identification afin que votre application peut utiliser les services FCM.



## <a name="overview"></a>Vue d'ensemble

Messagerie de Cloud firebase (FCM) est un service de cross-platform qui gère l’envoi, le routage et la file d’attente de messages entre les applications serveur et les applications clientes mobiles. FCM est le successeur de Google Cloud Messaging (GCM), et est basé sur les Services Google Play.

Comme illustré dans le diagramme suivant, FCM joue le rôle d’intermédiaire entre les clients et les expéditeurs de messages. A *application cliente* est une application compatible FCM qui s’exécute sur un appareil. Le *serveur d’applications* (fourni par vous ou votre société) est le serveur FCM activé votre application cliente avec lesquels communique via FCM. Contrairement aux GCM, FCM rend possible pour vous permet d’envoyer des messages aux applications clientes directement via l’interface utilisateur graphique de Notifications de la Console Firebase :

[![FCM se situe entre l’application cliente et un serveur d’applications](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

À l’aide de FCM, serveurs d’applications peuvent envoyer des messages à une seule unité, à un groupe d’appareils ou à un nombre de périphériques qui sont abonnés une rubrique. Une application cliente peut utiliser FCM pour vous abonner aux messages en aval à partir d’un serveur d’applications (par exemple, pour recevoir des notifications à distance). Pour plus d’informations sur les différents types de messages de Firebase, consultez [sur les Messages FCM](https://firebase.google.com/docs/cloud-messaging/concept-options).



## <a name="firebase-cloud-messaging-in-action"></a>Cloud firebase de messagerie en Action

Lorsqu’un message en aval est envoyé à une application cliente à partir d’un serveur d’applications, le serveur d’applications envoie le message à un *de connexion du serveur FCM* fournie par Google ; le serveur de connexion FCM, transfère à son tour, le message à un appareil qui exécute le application cliente. Les messages peuvent être envoyés via HTTP ou [XMPP](https://developers.google.com/cloud-messaging/ccs) (messagerie Extensible et présence Protocol). Étant donné que les applications clientes ne sont pas toujours connectées ou en cours d’exécution, FCM connexion serveur empile et banques de messages, les envoyer aux applications clientes qu’ils se reconnectent et deviennent disponibles. De même, FCM sera file d’attente des messages en amont à partir de l’application cliente vers le serveur d’applications si le serveur d’applications n’est pas disponible. Pour plus d’informations sur les serveurs de connexion FCM, consultez [sur Firebase Cloud serveur de messagerie](https://firebase.google.com/docs/cloud-messaging/server).

FCM utilise les informations d’identification suivantes pour identifier le serveur d’applications et de l’application cliente, et utilise ces informations d’identification pour autoriser des transactions de message FCM :

-   **ID de l’expéditeur** &ndash; le *ID de l’expéditeur* est une valeur numérique unique qui est affectée lorsque vous créez votre projet Firebase. L’ID d’expéditeur est utilisé pour identifier chaque serveur d’applications qui envoie des messages à l’application cliente. L’ID d’expéditeur est également le numéro de votre projet ; vous obtenez l’ID de l’expéditeur à partir de la Console Firebase lorsque vous enregistrez votre projet. Un exemple d’un ID de l’expéditeur est `496915549731`.

-   **Clé API** &ndash; le *clé API* fournit l’accès de serveur d’application aux services de Firebase ; FCM utilise cette clé pour authentifier le serveur d’applications. Ces informations d’identification sont également appelée le *clé serveur* ou *clé d’API Web*. Est un exemple d’une clé d’API `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   **ID d’application** &ndash; l’identité de votre application cliente (indépendante de tout périphérique donné) qui s’inscrit pour recevoir des messages de FCM. Est un exemple d’un ID d’application `1:415712510732:android:0e1eb7a661af2460`.

-   **Jeton d’inscription** &ndash; le *le jeton d’inscription* (également appelé le *ID d’Instance*) est l’identité FCM de votre application cliente sur un appareil donné. Le jeton d’inscription est généré au moment de l’exécution &ndash; votre application reçoit un jeton d’inscription lorsqu’il enregistre tout d’abord avec FCM lors de l’exécution sur un appareil. Le jeton d’inscription autorise une instance de votre application cliente (en cours d’exécution sur l’appareil particulier) pour recevoir les messages FCM.
    Est un exemple d’un jeton d’inscription `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (une très longue chaîne).

[Paramètre des Firebase Cloud de messagerie](#setup_fcm) (plus loin dans ce guide) fournit des instructions détaillées pour la création d’un projet et la génération de ces informations d’identification. Lorsque vous créez un nouveau projet dans le [Firebase Console](https://console.firebase.google.com/), un fichier d’informations d’identification appelé **google-services.json** est créé &ndash; ajouter ce fichier à votre projet Xamarin.Android, comme expliqué dans [ Notifications à distance avec FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

Les sections suivantes expliquent comment ces informations d’identification sont utilisées lorsque les applications clientes communiquent avec des serveurs d’applications via FCM.


<a name="registration" />

### <a name="registration-with-fcm"></a>Inscription auprès de FCM

Une application cliente doit d’abord enregistrer avec FCM avant de messagerie peut avoir lieu. L’application cliente doit terminer la procédure d’inscription indiquée dans le diagramme suivant :

[![Diagramme d’étapes d’inscription application](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  L’application cliente contacte FCM pour obtenir un jeton d’inscription, en passant l’ID de l’expéditeur, une clé d’API et un ID d’application à FCM.

2.  FCM retourne un jeton d’inscription à l’application cliente.

3.  (Facultatif), l’application cliente transmet le jeton d’inscription pour le serveur d’applications.

Le serveur d’applications met en cache le jeton d’inscription pour les communications ultérieures avec l’application cliente. Le serveur d’applications peut renvoyer un accusé de réception à l’application cliente pour indiquer que le jeton d’inscription a été reçu. Une fois cette négociation a lieu, l’application cliente peut recevoir des messages (ou envoyer des messages à) le serveur d’applications. L’application cliente peut recevoir un nouveau jeton d’inscription si l’ancien jeton est compromis (consultez [des Notifications à distance avec FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) pour obtenir un exemple de comment une application reçoit des mises à jour de jeton d’inscription).

Lorsque l’application cliente ne souhaite plus que recevoir des messages à partir du serveur d’application, il peut envoyer une demande au serveur d’application pour supprimer le jeton d’inscription. Si l’application cliente est désinstallée d’un appareil, FCM détecte et notifie automatiquement le serveur d’applications pour supprimer le jeton d’inscription.



### <a name="downstream-messaging"></a>La messagerie en aval

Le diagramme suivant illustre comment Firebase de messagerie Cloud stocke et transfère des messages en aval :

[![FCM utilise stocker et transférer de la messagerie en aval](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Lorsque le serveur d’applications envoie un message en aval à l’application cliente, il utilise les étapes suivantes en tant qu’énumérées dans le diagramme ci-dessus :

1.  Le serveur d’applications envoie le message à FCM.

2.  Si le périphérique client n’est pas disponible, le serveur FCM stocke le message dans une file d’attente de transmission ultérieure. Les messages sont conservés dans le stockage pour un maximum de 4 semaines FCM (pour plus d’informations, consultez [définissant la durée de vie d’un message](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Lorsque le périphérique client est disponible, FCM transmet le message à l’application cliente sur l’appareil.

4.  L’application cliente reçoit le message de FCM, traite et l’affiche à l’utilisateur. Par exemple, si le message est une notification à distance, il est présenté à l’utilisateur dans la zone de notification.

Dans ce scénario de messagerie (où le serveur d’applications envoie un message à une application cliente unique), les messages peuvent être jusqu'à 4 Ko.

Pour plus d’informations sur la réception des messages FCM en aval sur Android, consultez [des Notifications à distance avec FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Rubrique de messagerie

*Rubrique messagerie* rend possible pour un serveur d’application pour envoyer un message à plusieurs appareils qui ont souscrit à une rubrique particulière. Vous pouvez également composer et envoyer des messages de rubrique via l’interface utilisateur graphique de Notifications de la Console Firebase. FCM gère le routage et la remise des messages de la rubrique sur les clients auxquels vous êtes abonnés. Cette fonctionnalité peut être utilisée pour les messages, tels que des alertes météo et actualités cotations boursières.

[![Diagramme de messagerie de rubrique](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

Les étapes suivantes sont utilisées dans la rubrique de messagerie (une fois que l’application cliente obtient un jeton d’inscription, comme expliqué plus haut) :

1.  L’application cliente s’abonne à une rubrique en envoyant un message de s’abonner à FCM.

2.  Le serveur d’applications envoie des messages de la rubrique à FCM pour la distribution.

3.  FCM transfère les messages de la rubrique sur les clients qui sont abonnés à cette rubrique.

Pour plus d’informations sur la messagerie de rubrique Firebase, consultez Google [rubrique messagerie sur Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Configuration de Firebase de cloud computing de messagerie

Avant de pouvoir utiliser les services FCM dans votre application, vous devez créer un projet (ou importer un projet existant) via le [Firebase Console](https://console.firebase.google.com/). Utilisez les étapes suivantes pour créer un projet de messagerie Cloud Firebase pour votre application :

1.  Se connecter à la [Firebase Console](https://console.firebase.google.com/) avec votre compte de Google (autrement dit, votre adresse Gmail) et d’un clic **créer un nouveau projet**:

    [![Créer le bouton Nouveau projet](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Si vous avez un projet existant, cliquez sur **importer un projet Google**.

2.  Dans le **créer un projet** boîte de dialogue, entrez le nom de votre projet et cliquez sur **créer un projet de**. Dans l’exemple suivant, un nouveau projet appelé **XamarinFCM** est créé :

    [![Créer une boîte de dialogue de projet](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Dans la Console Firebase **vue d’ensemble**, cliquez sur **ajouter Firebase à votre application Android**:

    [![Ajoutez Firebase à votre application Android](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Dans l’écran suivant, entrez le nom du package de votre application. Dans cet exemple, le nom du package est **com.xamarin.fcmexample**. Cette valeur doit correspondre au nom de package de votre application Android. Vous pouvez également entrer un surnom de l’application dans le **surnom de l’application** champ :

    [![Entrant FCM exemple du surnom de l’application](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Si votre application utilise des liaisons dynamiques, invitations ou Google Authentication, vous devez également entrer votre certificat de signature de débogage. Pour plus d’informations sur la localisation de votre certificat de signature, consultez [recherche de votre magasin de clés MD5 ou SHA1 Signature](~/android/deploy-test/signing/keystore-signature.md).
    Dans cet exemple, le certificat de signature est vide.

6.  Cliquez sur **ajouter application**:

    [![Cliquez sur le bouton Ajouter une application](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Une clé d’API de serveur et un ID de Client sont générés automatiquement pour l’application. Ces informations sont regroupées dans un **google-services.json** fichier qui est automatiquement téléchargé lorsque vous cliquez sur **ajouter une application**.
    Veillez à enregistrer ce fichier dans un endroit sûr.

Pour obtenir un exemple détaillé de l’ajout de **google-services.json** à un projet d’application pour recevoir des messages de notification push FCM sur Android, consultez [des Notifications à distance avec FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Pour obtenir des informations supplémentaires

-   Google [Firebase de messagerie Cloud](https://firebase.google.com/docs/cloud-messaging/) fournit une vue d’ensemble de fonctionnalités de clé de messagerie de Cloud Firebase, une explication de son fonctionnement et les instructions d’installation.

-   Google [générer les demandes d’envoi d’application serveur](https://firebase.google.com/docs/cloud-messaging/send-message) explique comment envoyer des messages avec votre serveur d’applications.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) et [RFC 6121](https://tools.ietf.org/html/rfc6121) expliquent et définir la messagerie Extensible et présence protocole XMPP ().



## <a name="summary"></a>Récapitulatif

Cet article a fourni une vue d’ensemble de la messagerie de Cloud Firebase (FCM). Il explique les différentes informations d’identification sont utilisées pour identifier et d’autoriser la messagerie entre les serveurs d’applications et les applications clientes. Il illustre l’inscription et les scénarios de messagerie en aval, et il les procédures détaillées pour l’inscription de votre application avec FCM à utiliser les services FCM.


## <a name="related-links"></a>Liens associés

- [Messagerie cloud Firebase](https://firebase.google.com/docs/cloud-messaging/)
