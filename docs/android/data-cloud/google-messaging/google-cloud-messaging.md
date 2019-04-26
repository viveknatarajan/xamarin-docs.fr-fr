---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM) est un service qui simplifie la messagerie entre les applications mobiles et les applications serveur. Cet article fournit une vue d’ensemble du fonctionne de GCM, et explique comment configurer les Services de Google afin que votre application peut utiliser GCM.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: c2ca567ffcb247622d1b3e8f3e0136c453723b96
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61017684"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

_Google Cloud Messaging (GCM) est un service qui simplifie la messagerie entre les applications mobiles et les applications serveur. Cet article fournit une vue d’ensemble du fonctionne de GCM, et explique comment configurer les Services de Google afin que votre application peut utiliser GCM._

[![Logo Google Cloud Messaging](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Cette rubrique fournit une vue d’ensemble de la façon dont Google Cloud Messaging achemine les messages entre votre application et un serveur d’applications, et il fournit une procédure pas à pas pour l’acquisition des informations d’identification afin que votre application peut utiliser les services GCM.

> [!NOTE]
> GCM a été remplacé par [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serveur et client API [ont été déconseillées](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) et ne sera plus disponible dès que le 11 avril 2019.

## <a name="overview"></a>Vue d'ensemble

Google Cloud Messaging (GCM) est un service qui gère l’envoi, le routage et la file d’attente de messages entre les applications serveur et les applications clientes mobiles. Un *application cliente* est une application prenant en charge GCM qui s’exécute sur un appareil. Le *du serveur d’applications* (fourni par vous ou votre société) est le serveur prenant en charge GCM que votre application cliente communique avec via GCM :

[![GCM se trouve entre l’application cliente et le serveur d’applications](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

À l’aide de GCM, les serveurs d’applications peuvent envoyer des messages à un seul appareil, un groupe d’appareils ou un nombre d’appareils qui sont abonnés à une rubrique. Votre application cliente peut utiliser GCM pour vous abonner aux messages en aval à partir d’un serveur d’applications (par exemple, pour recevoir des notifications à distance). En outre, GCM rend possible pour les applications clientes envoyer des messages en amont sur le serveur d’application.

Pour plus d’informations sur l’implémentation d’un serveur d’applications pour GCM, consultez [sur le serveur de connexion GCM](https://developers.google.com/cloud-messaging/server).



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging en Action

Lorsque les messages en aval sont envoyés à partir d’un serveur d’applications à une application cliente, le serveur d’application envoie le message à un *du serveur GCM connexion*; le GCM connexion, à son tour, transfère le message à un appareil qui exécute votre application cliente. Messages peuvent être envoyés via HTTP ou [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging et protocole de présence). Étant donné que les applications clientes ne sont pas toujours connectées ou en cours d’exécution, les GCM connexion empile et banques de messages du serveur, les envoyer à des applications clientes, car elles se reconnectent et deviennent disponibles. De même, GCM sera file d’attente des messages en amont à partir de l’application cliente sur le serveur d’application si le serveur d’application n’est pas disponible.

GCM utilise les informations d’identification suivantes pour identifier le serveur d’applications et de votre application cliente et utilise ces informations d’identification pour autoriser les transactions de message via GCM :

-   **Clé API** &ndash; le *clé API* donne accès serveur de votre application pour Google services ; GCM utilise cette clé pour authentifier votre serveur d’application.
    Avant de pouvoir utiliser le service GCM, vous devez d’abord obtenir une clé API à partir de la [Google Developer Console](https://console.developers.google.com/) en créant un *projet*. La clé d’API doivent être conservées de manière sécurisée ; Pour plus d’informations sur la protection de votre clé API, consultez [meilleures pratiques d’utilisation en toute sécurité les clés API](https://support.google.com/cloud/answer/6310037?hl=en).

-   **Sender ID** &ndash; le *Sender ID* autorise le serveur d’applications pour votre application cliente &ndash; c’est un nombre unique qui identifie le serveur d’application qui est autorisé à envoyer des messages à votre application cliente.
    L’ID d’expéditeur est également le numéro de projet ; vous obtenir l’ID de l’expéditeur à partir de la Console des développeurs Google lorsque vous enregistrez votre projet.

-   **Jeton d’inscription** &ndash; le *le jeton d’inscription* est l’identité GCM de votre application cliente sur un appareil donné. Le jeton d’inscription est généré au moment de l’exécution &ndash; votre application reçoit un jeton d’inscription lorsqu’il enregistre tout d’abord auprès de GCM lors de l’exécution sur un appareil. Le jeton d’inscription autorise une instance de votre application cliente (en cours d’exécution sur cet appareil particulier) pour recevoir des messages de GCM.

-   **ID d’application** &ndash; l’identité de votre application cliente (indépendamment de n’importe quel appareil donné) qui s’inscrit pour recevoir des messages de GCM. Sur Android, l’ID d’application est enregistré dans le nom du package **AndroidManifest.xml**, tel que `com.xamarin.gcmexample`.

[Paramètre de Google Cloud Messaging](#settingup) (plus loin dans ce guide) fournit des instructions détaillées pour la création d’un projet et la génération de ces informations d’identification.

Les sections suivantes expliquent comment ces informations d’identification sont utilisées lorsque les applications clientes communiquent avec les serveurs d’applications via GCM.



### <a name="registration-with-gcm"></a>Inscription auprès de GCM

Une application cliente installée sur un appareil doit tout d’abord s’inscrire auprès de GCM avant de messagerie peut avoir lieu. L’application cliente doit terminer la procédure d’inscription indiquée dans le diagramme suivant :

[![Étapes de l’inscription d’application](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  L’application cliente contacte GCM pour obtenir un jeton d’inscription, en passant l’ID d’expéditeur à GCM.

2.  GCM retourne un jeton d’inscription à l’application cliente.

3.  L’application cliente transmet le jeton d’inscription sur le serveur d’application.

Le serveur de l’application met en cache le jeton d’inscription pour les communications ultérieures avec l’application cliente. Si vous le souhaitez, le serveur d’application peut envoyer un accusé de réception à l’application cliente pour indiquer que le jeton d’inscription a été reçu. Une fois que cette liaison a lieu, l’application cliente peut recevoir des messages (ou envoyer des messages à) le serveur d’applications.

Lorsque l’application cliente ne souhaite plus que recevoir des messages à partir du serveur d’application, il peut envoyer une demande au serveur d’application pour supprimer le jeton d’inscription. Si l’application cliente reçoit des messages de rubrique (décrite plus loin dans cet article), il peut se désabonner de la rubrique.
Si l’application cliente est désinstallée d’un appareil, GCM détecte et notifie automatiquement le serveur d’application pour supprimer le jeton d’inscription.

Google [l’inscription des applications clientes](https://developers.google.com/cloud-messaging/registration) explique le processus d’inscription plus en détail ; il explique l’annulation d’inscription et annulation d’abonnement, et il décrit le processus d’annulation d’inscription lors de la désinstallation d’une application cliente.



### <a name="downstream-messaging"></a>Messagerie en aval

Lorsque le serveur d’application envoie un message en aval à l’application cliente, il suit les étapes illustrées dans le diagramme suivant :

[![Banque de messagerie en aval et diagramme vers l’avant](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  Le serveur d’application envoie le message à GCM.

2.  Si l’appareil client n’est pas disponible, le serveur GCM stocke le message dans une file d’attente pour une transmission ultérieure.

3.  Quand l’appareil client est disponible, GCM envoie le message à l’application cliente sur l’appareil.

4.  L’application cliente reçoit le message de GCM et gère en conséquence. Par exemple, si le message est une notification à distance, il est présenté à l’utilisateur.

Dans ce scénario de messagerie (où le serveur d’application envoie un message à une application cliente unique), les messages peuvent être jusqu'à 4 Ko.

Pour plus d’informations (y compris les exemples de code) sur la réception des messages GCM en aval sur Android, consultez [Notifications à distance](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


#### <a name="topic-messaging"></a>Rubrique de messagerie

*Rubrique Messaging* est un type de messagerie en aval dans lequel le serveur d’application envoie un message unique à plusieurs périphériques d’application client qui s’abonnent à une rubrique (par exemple, des prévisions météorologiques). Messages de rubrique peuvent être jusqu'à 2 Ko et rubrique messagerie prend en charge les abonnements jusqu'à un million par application. Si GCM est utilisé uniquement pour la rubrique de messagerie, l’application cliente n’est pas obligée d’envoyer un jeton d’inscription sur le serveur d’application. Google [mise en œuvre la messagerie de rubrique](https://developers.google.com/cloud-messaging/topic-messaging) explique comment envoyer des messages à partir d’un serveur d’applications à plusieurs périphériques s’abonner à une rubrique particulière.



#### <a name="group-messaging"></a>Groupe de messagerie

*Groupe messagerie* est un type de messagerie en aval dans lequel le serveur d’application envoie un message unique à plusieurs périphériques d’application client qui appartiennent à un groupe (par exemple, un groupe d’appareils qui appartiennent à un seul utilisateur). Groupe de messages peut être jusqu'à 2 Ko de longueur pour les appareils iOS et jusqu'à 4 Ko de longueur pour les appareils Android. Un groupe est limité à un maximum de 20 membres. Google [Device groupe Messaging](https://developers.google.com/cloud-messaging/notifications) explique comment les serveurs d’applications peuvent envoyer un message unique à plusieurs instances d’application de client en cours d’exécution sur les appareils qui appartiennent à un groupe.


### <a name="upstream-messaging"></a>Messagerie en amont

Si votre application cliente se connecte à un serveur qui prend en charge [XMPP](https://developers.google.com/cloud-messaging/ccs), il peut envoyer des messages sur le serveur d’application, comme illustré dans le diagramme suivant :

[![Diagramme de messagerie en amont](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  L’application cliente envoie un message sur le serveur de connexion GCM XMPP.

2.  Si le serveur d’application est déconnecté, le serveur GCM stocke le message dans une file d’attente de transmission plus tard.

3.  Lorsque le serveur d’application est reconnectée, GCM transfère le message au serveur d’application.

4.  Le serveur d’application analyse le message pour vérifier l’identité de l’application cliente, puis il envoie un accusé de « réception » vers GCM afin d’accuser réception du message.

5.  Le serveur d’application traite le message.

Google [Messages en amont](https://developers.google.com/cloud-messaging/ccs#upstream) explique comment structurer les messages codés au format JSON et les envoient aux serveurs d’application qui s’exécutent XMPP Cloud connexion serveur de Google.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Configuration de Google Cloud Messaging

Avant de pouvoir utiliser les services GCM dans votre application, vous devez tout d’abord obtenir des informations d’identification pour accéder aux serveurs GCM de Google. Les sections suivantes décrivent les étapes requises pour effectuer ce processus :



### <a name="enable-google-services-for-your-app"></a>Activer les Services de Google pour votre application

1.  Connectez-vous à la [Google Developers Console](https://developers.google.com/mobile/add?platform=android) avec votre Google compte (autrement dit, votre adresse gmail) et créez un nouveau projet. Si vous avez un projet existant, choisissez le projet que vous souhaitez devenir compatibles avec GCM. Dans l’exemple suivant, un nouveau projet appelé **XamarinGCM** est créé :

    [![Création du projet XamarinGCM](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  Ensuite, entrez le nom du package pour votre application (dans cet exemple, le nom du package est **com.xamarin.gcmexample**) et cliquez sur **continuer pour choisir et configurer les services**:

    [![Entrer le nom du package](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Notez que ce nom de package est également l’ID d’application pour votre application.

3.  Le **choisir et configurer les services** section répertorie les services Google que vous pouvez ajouter à votre application. Cliquez sur **Cloud Messaging**:

    [![Choisissez le Cloud de messagerie](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  Ensuite, cliquez sur **activer GOOGLE CLOUD MESSAGING**:

    [![Activer Google Cloud Messaging](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  Un **clé API de serveur** et un **Sender ID** sont générés pour votre application. Enregistrez ces valeurs, puis cliquez sur **fermer**:

    [![Clé d’API serveur et Sender ID affiché](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    Protéger la clé API &ndash; il n’est pas destinée pour une utilisation publique. Si la clé API est compromise, les serveurs non autorisés peuvent publier des messages aux applications clientes.
    [Meilleures pratiques d’utilisation en toute sécurité les clés API](https://support.google.com/cloud/answer/6310037?hl=en) fournit des instructions utiles pour la protection de votre clé API.



### <a name="view-your-project-settings"></a>Afficher les paramètres de votre projet

Vous pouvez afficher les paramètres de votre projet à tout moment en vous connectant à la [Google Cloud Console](https://console.cloud.google.com/) et en sélectionnant votre projet. Par exemple, vous pouvez afficher le **Sender ID** en sélectionnant votre projet dans le menu déroulant en haut de la page (dans cet exemple, le projet est appelé **XamarinGCM**). L’ID d’expéditeur est le numéro de projet, comme indiqué dans cette capture d’écran (l’ID d’expéditeur est **9349932736**) :

[![Affichage de l’ID d’expéditeur](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Pour afficher le **clé API**, cliquez sur **API Manager** puis cliquez sur **informations d’identification**:

[![Affichage de la clé d’API](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>Pour obtenir des informations supplémentaires

-   Google [l’inscription des applications clientes](https://developers.google.com/cloud-messaging/registration) décrit le processus d’inscription de client plus en détail, et il fournit des informations sur la configuration de nouvelle tentative automatique et de conservation de l’état d’inscription de la synchronisation.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) et [RFC 6121](https://tools.ietf.org/html/rfc6121) expliquent et définissent les Extensible Messaging et la présence protocole XMPP ().



## <a name="summary"></a>Récapitulatif

Cet article fournit une vue d’ensemble de Google Cloud Messaging (GCM). Vous avez appris les informations d’identification différentes qui sont utilisées pour identifier et autoriser la messagerie entre les serveurs d’applications et les applications clientes. Il illustre les scénarios de messagerie les plus courants, et il les procédures détaillées pour l’inscription de votre application auprès de GCM à utiliser les services GCM.


## <a name="related-links"></a>Liens associés

- [Messagerie cloud](https://developers.google.com/cloud-messaging/)
