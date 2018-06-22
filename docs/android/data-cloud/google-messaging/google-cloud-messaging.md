---
title: Messagerie Cloud Google
description: Google Cloud Messaging (GCM) est un service qui simplifie la messagerie entre les applications mobiles et des applications serveur. Cet article fournit une vue d’ensemble du fonctionne de GCM, et explique comment configurer les Services Google pour que votre application puisse utiliser GCM.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: 29cccf414759a79a8ba74dfc35b7ba9f6a1cc5d6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044741"
---
# <a name="google-cloud-messaging"></a>Messagerie Cloud Google

_Google Cloud Messaging (GCM) est un service qui simplifie la messagerie entre les applications mobiles et des applications serveur. Cet article fournit une vue d’ensemble du fonctionne de GCM, et explique comment configurer les Services Google pour que votre application puisse utiliser GCM._

[![Logo de messagerie Cloud Google](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Cette rubrique fournit une vue d’ensemble de la messagerie Cloud Google achemine les messages entre votre application et un serveur d’applications, et il fournit une procédure pas à pas pour l’acquisition d’informations d’identification afin que votre application peut utiliser les services GCM.

> [!NOTE]
> GCM a été remplacé par [Firebase de messagerie Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serveur et le client API [ont été déconseillées](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) et ne sera plus disponible dès que le 11 avril 2019.

## <a name="overview"></a>Vue d'ensemble

Google Cloud Messaging (GCM) est un service qui gère l’envoi, le routage et la file d’attente de messages entre les applications serveur et les applications clientes mobiles. A *application cliente* est une application GCM qui s’exécute sur un appareil. Le *serveur d’applications* (fourni par vous ou votre société) est le serveur activé GCM votre application cliente avec lesquels communique via GCM :

[![GCM se trouve entre l’application cliente et le serveur d’applications](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

À l’aide de GCM, les serveurs d’applications peuvent envoyer des messages à une seule unité, un groupe de périphériques ou un nombre de périphériques qui sont abonnés une rubrique. Votre application cliente peut utiliser GCM pour vous abonner aux messages en aval à partir d’un serveur d’applications (par exemple, pour recevoir des notifications à distance). En outre, GCM rend possible pour les applications clientes envoyer des messages en amont sur le serveur d’application.

Pour plus d’informations sur l’implémentation d’un serveur d’applications pour GCM, consultez [sur le serveur de connexion GCM](https://developers.google.com/cloud-messaging/server).



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging en Action

Lorsque des messages en aval sont envoyés à partir d’un serveur d’applications à une application cliente, le serveur d’applications envoie le message à un *de connexion du serveur GCM*; le serveur de connexion GCM, transfère à son tour, le message à un appareil qui exécute votre application cliente. Les messages peuvent être envoyés via HTTP ou [XMPP](https://developers.google.com/cloud-messaging/ccs) (messagerie Extensible et présence Protocol). Étant donné que les applications clientes ne sont pas toujours connectées ou en cours d’exécution, les GCM connexion empile et banques de messages du serveur, les envoyer aux applications clientes qu’ils se reconnectent et deviennent disponibles. De même, GCM sera file d’attente des messages en amont à partir de l’application cliente vers le serveur d’applications si le serveur d’applications n’est pas disponible.

GCM utilise les informations d’identification suivantes pour identifier le serveur d’applications et de votre application cliente, et utilise ces informations d’identification pour autoriser des transactions de message GCM :

-   **Clé API** &ndash; le *clé API* donne accès serveur de votre application pour Google services ; GCM utilise cette clé pour authentifier le serveur de votre application.
    Avant de pouvoir utiliser le service GCM, vous devez d’abord obtenir une clé d’API à partir de la [Console des développeurs Google](https://console.developers.google.com/) en créant un *projet*. La clé d’API doit être conservé en lieu sûr ; Pour plus d’informations sur la protection de votre clé API, consultez [meilleures pratiques de façon sécurisée à l’aide de clés de l’API](https://support.google.com/cloud/answer/6310037?hl=en).

-   **ID de l’expéditeur** &ndash; le *ID de l’expéditeur* autorise le serveur d’applications pour votre application cliente &ndash; il s’agit d’un numéro unique qui identifie le serveur d’applications est autorisé à envoyer des messages à votre application cliente.
    L’ID d’expéditeur est également le numéro de votre projet ; vous obtenez l’ID de l’expéditeur à partir de la Console des développeurs Google lorsque vous enregistrez votre projet.

-   **Jeton d’inscription** &ndash; le *le jeton d’inscription* est l’identité GCM de votre application cliente sur un appareil donné. Le jeton d’inscription est généré au moment de l’exécution &ndash; votre application reçoit un jeton d’inscription lorsqu’il enregistre tout d’abord avec GCM lors de l’exécution sur un appareil. Le jeton d’inscription autorise une instance de votre application cliente (en cours d’exécution sur l’appareil particulier) pour recevoir des messages à partir de GCM.

-   **ID d’application** &ndash; l’identité de votre application cliente (indépendante de tout périphérique donné) qui s’inscrit pour recevoir des messages de GCM. Sur Android, l’ID d’application est enregistré dans le nom du package **AndroidManifest.xml**, tel que `com.xamarin.gcmexample`.

[Paramètre de messagerie Cloud Google](#settingup) (plus loin dans ce guide) fournit des instructions détaillées pour la création d’un projet et la génération de ces informations d’identification.

Les sections suivantes expliquent comment ces informations d’identification sont utilisées lorsque les applications clientes communiquent avec des serveurs d’applications via GCM.



### <a name="registration-with-gcm"></a>Inscription GCM

Une application cliente installée sur un appareil doit d’abord enregistrer avec GCM avant de messagerie peut avoir lieu. L’application cliente doit terminer la procédure d’inscription indiquée dans le diagramme suivant :

[![Étapes de l’inscription d’application](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  L’application cliente contacte GCM pour obtenir un jeton d’inscription, en passant l’ID d’expéditeur pour GCM.

2.  GCM retourne un jeton d’inscription à l’application cliente.

3.  L’application cliente transmet le jeton d’inscription pour le serveur d’applications.

Le serveur d’applications met en cache le jeton d’inscription pour les communications ultérieures avec l’application cliente. Si vous le souhaitez, le serveur d’applications peut renvoyer un accusé de réception à l’application cliente pour indiquer que le jeton d’inscription a été reçu. Une fois cette négociation a lieu, l’application cliente peut recevoir des messages (ou envoyer des messages à) le serveur d’applications.

Lorsque l’application cliente ne souhaite plus que recevoir des messages à partir du serveur d’application, il peut envoyer une demande au serveur d’application pour supprimer le jeton d’inscription. Si l’application cliente reçoit des messages de rubrique (décrites plus loin dans cet article), il peut annuler l’abonnement à la rubrique.
Si l’application cliente est désinstallée d’un appareil, GCM détecte et notifie automatiquement le serveur d’applications pour supprimer le jeton d’inscription.

Google [l’inscription des applications clientes](https://developers.google.com/cloud-messaging/registration) explique le processus d’inscription dans plus de détails ; il explique l’annulation et l’annulation d’abonnement, et décrit le processus d’annulation d’inscription lors de la désinstallation d’une application cliente.



### <a name="downstream-messaging"></a>La messagerie en aval

Lorsque le serveur d’applications envoie un message en aval à l’application cliente, il suit les étapes illustrées dans le diagramme suivant :

[![Banque de messages en aval et le diagramme vers l’avant](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  Le serveur d’applications envoie le message à GCM.

2.  Si le périphérique client n’est pas disponible, le serveur GCM stocke le message dans une file d’attente de transmission ultérieure.

3.  Lorsque le périphérique client est disponible, GCM envoie le message à l’application cliente sur l’appareil.

4.  L’application cliente reçoit le message de GCM et gère en conséquence. Par exemple, si le message est une notification à distance, il est présenté à l’utilisateur.

Dans ce scénario de messagerie (où le serveur d’applications envoie un message à une application cliente unique), les messages peuvent être jusqu'à 4 Ko.

Pour plus d’informations (y compris les exemples de code) sur la réception des messages GCM en aval sur Android, consultez [des Notifications à distance](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


#### <a name="topic-messaging"></a>Rubrique de messagerie

*Messagerie de rubrique* est un type de messagerie en aval sur lequel le serveur d’applications envoie un message unique à plusieurs périphériques d’application client qui s’abonnent à une rubrique (par exemple, les prévisions météorologiques). Messages de la rubrique peuvent être jusqu'à 2 Ko et rubrique messagerie prend en charge les abonnements jusqu'à un million par application. Si GCM est utilisé uniquement pour la rubrique de messagerie, l’application cliente n’est pas requis pour envoyer un jeton d’inscription pour le serveur d’applications. Google [implémentation de la messagerie de rubrique](https://developers.google.com/cloud-messaging/topic-messaging) explique comment envoyer des messages à partir d’un serveur d’applications à plusieurs périphériques s’abonner à une rubrique particulière.



#### <a name="group-messaging"></a>Groupe de messagerie

*Groupe messagerie* est un type de messagerie en aval sur lequel le serveur d’applications envoie un message unique à plusieurs périphériques d’application cliente qui appartiennent à un groupe (par exemple, un groupe de périphériques qui appartiennent à un seul utilisateur). Messages de groupe peuvent être jusqu'à 2 Ko de longueur pour les appareils iOS et jusqu'à 4 Ko de longueur pour les appareils Android. Un groupe est limité à un maximum de 20 membres. Google [Device groupe Messaging](https://developers.google.com/cloud-messaging/notifications) explique comment les serveurs d’applications peuvent envoyer un message unique à plusieurs instances d’application client en cours d’exécution sur les appareils qui appartiennent à un groupe.


### <a name="upstream-messaging"></a>La messagerie en amont

Si votre application cliente se connecte à un serveur qui prend en charge [XMPP](https://developers.google.com/cloud-messaging/ccs), il peut envoyer des messages vers le serveur d’applications, comme illustré dans le diagramme suivant :

[![Diagramme de messagerie en amont](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  L’application cliente envoie un message sur le serveur de connexion GCM XMPP.

2.  Si le serveur d’applications est déconnecté, le serveur GCM stocke le message dans une file d’attente de transmission plus tard.

3.  Lorsque le serveur d’applications est reconnectée, GCM transfère le message vers le serveur d’applications.

4.  Le serveur d’applications analyse le message pour vérifier l’identité de l’application cliente, puis il envoie un accusé de « réception » pour GCM pour accuser réception du message.

5.  Le serveur d’applications traite le message.

Google [Messages en amont](https://developers.google.com/cloud-messaging/ccs#upstream) explique comment structurer les messages encodés en JSON et de les envoyer à des serveurs d’applications qui s’exécutent XMPP Cloud connexion serveur de Google.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Configuration de messagerie Cloud Google

Avant de pouvoir utiliser les services GCM dans votre application, vous devez tout d’abord obtenir des informations d’identification pour accéder aux serveurs GCM de Google. Les sections suivantes décrivent les étapes requises pour exécuter ce processus :



### <a name="enable-google-services-for-your-app"></a>Activer les Services Google pour votre application

1.  Se connecter à la [Console des développeurs Google](https://developers.google.com/mobile/add?platform=android) avec votre Google, compte (autrement dit, votre adresse gmail) et créez un nouveau projet. Si vous avez un projet existant, sélectionnez le projet que vous souhaitez devenir compatibles GCM. Dans l’exemple suivant, un nouveau projet appelé **XamarinGCM** est créé :

    [![Création du projet XamarinGCM](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  Ensuite, entrez le nom du package pour votre application (dans cet exemple, le nom du package est **com.xamarin.gcmexample**) et cliquez sur **continuer pour choisir et configurer les services**:

    [![Entrer le nom du package](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Notez que ce nom de package est également l’ID d’application pour votre application.

3.  Le **choisir et configurer les services** section répertorie les services Google que vous pouvez ajouter à votre application. Cliquez sur **Cloud messagerie**:

    [![Choisissez le Cloud de messagerie](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  Ensuite, cliquez sur **activer la messagerie CLOUD GOOGLE**:

    [![Activer la messagerie Cloud Google](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A **clé API de serveur** et un **ID de l’expéditeur** sont générés pour votre application. Enregistrez ces valeurs, puis cliquez sur **fermer**:

    [![Clé d’API de serveur et ID d’expéditeur affiché](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    Protéger la clé API &ndash; elle n’est pas destinée pour une utilisation publique. Si la clé API est compromise, les serveurs non autorisés peuvent publier des messages pour les applications clientes.
    [Meilleures pratiques pour sécurisée à l’aide de clés API](https://support.google.com/cloud/answer/6310037?hl=en) fournit des conseils utiles pour la protection de votre clé API.



### <a name="view-your-project-settings"></a>Afficher les paramètres du projet

Vous pouvez afficher les paramètres de votre projet à tout moment en vous connectant à la [Google Cloud Console](https://console.cloud.google.com/) et en sélectionnant votre projet. Par exemple, vous pouvez afficher le **ID de l’expéditeur** en sélectionnant votre projet dans le menu déroulant en haut de la page (dans cet exemple, le projet est appelé **XamarinGCM**). L’ID d’expéditeur est le numéro de projet, comme indiqué dans cette capture d’écran (l’ID de l’expéditeur est **9349932736**) :

[![Affichage de l’ID d’expéditeur](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Pour afficher le **clé API**, cliquez sur **API Manager** puis cliquez sur **informations d’identification**:

[![Affichage de la clé d’API](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>Pour obtenir des informations supplémentaires

-   Google [l’inscription des applications clientes](https://developers.google.com/cloud-messaging/registration) décrit le processus d’inscription de client plus en détail, et fournit des informations sur la configuration de nouvelle tentative automatique et de conservation de l’état d’inscription de la synchronisation.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) et [RFC 6121](https://tools.ietf.org/html/rfc6121) expliquent et définir la messagerie Extensible et présence protocole XMPP ().



## <a name="summary"></a>Récapitulatif

Cet article a fourni une vue d’ensemble de Google Cloud Messaging (GCM). Il explique les différentes informations d’identification sont utilisées pour identifier et d’autoriser la messagerie entre les serveurs d’applications et les applications clientes. Il illustre des scénarios de messagerie les plus courants, et il les procédures détaillées pour l’inscription de votre application avec GCM pour utiliser les services GCM.


## <a name="related-links"></a>Liens associés

- [Messagerie de cloud computing](https://developers.google.com/cloud-messaging/)
