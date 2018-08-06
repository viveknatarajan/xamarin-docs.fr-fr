---
title: Notifications push dans iOS
description: Ce document décrit comment utiliser des notifications push dans iOS 9 et versions antérieures. Il présente les certificats, l’inscription auprès de l’Apple Push Notifications passerelle de Service (APNS) et bien plus encore.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7bb2a250b9d3cc0c8df02f432330f9fe1dc58f94
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788665"
---
# <a name="push-notifications-in-ios"></a>Notifications push dans iOS

> [!IMPORTANT]
> Les informations contenues dans cette section relative à iOS 9 et antérieure, il est resté ici pour prendre en charge les anciennes versions d’iOS. Pour iOS 10 et versions ultérieures, consultez le [guide utilisateur Notification Framework](~/ios/platform/user-notifications/index.md) pour prendre en charge à la fois Local et distant Notification sur un appareil iOS.

Des notifications push doivent être conservées brèves et ne contient que les données nécessaires pour notifier l’application mobile qu’il doit contacter l’application serveur pour une mise à jour. Par exemple, lors de l’arrivent de nouveaux messages, l’application serveur serait notifier uniquement l’application mobile nouveaux messages sont arrivé. La notification ne contiendrait pas les nouveaux messages lui-même. L’application mobile serait puis récupérer les nouveaux messages à partir du serveur lorsqu’il était approprié

Dans le centre de push notifications dans iOS est la *Service passerelle Apple Push Notification (APNS)*. Il s’agit d’un service fourni par Apple qui est chargée de routage des notifications à partir d’un serveur d’applications sur des appareils iOS.
L’image suivante illustre la topologie de notification push pour iOS : ![](remote-notifications-in-ios-images/image4.png "cette image illustre la topologie de notification push pour iOS")

Les notifications à distance proprement dites sont conformes au format des chaînes au format JSON et protocoles spécifié dans [la charge utile de Notification](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) section de la [Local et Push Notification Guide de programmation](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)dans les [la documentation du développeur iOS](https://developer.apple.com/devcenter/ios/index.action).

Apple gère deux environnements de APNS : un *Sandbox* et un *Production* environnement. L’environnement de bac à sable est conçu pour tester au cours de la phase de développement et peut être à `gateway.sandbox.push.apple.com` sur TCP port 2195. L’environnement de Production est destiné à être utilisé dans les applications qui ont été déployées et peuvent être à `gateway.push.apple.com` sur TCP port 2195.

## <a name="requirements"></a>Configuration requise

Notifications push doivent observer les règles suivantes sont dictés par l’architecture de APNS :

-  **256 octets de Message limite** -la taille de la totalité du message de la notification ne doit pas dépasser 256 octets.
-  **Aucune Confirmation de l’accusé de réception** -APNS ne fournit pas de l’expéditeur avec une notification d’un message rend au destinataire prévu. Si l’appareil n’est pas accessible et que plusieurs notifications séquentielles sont envoyées, toutes les notifications, sauf les plus récentes seront perdues. Seule la notification de la plus récente sera remise à l’appareil.
-  **Chaque application nécessite un certificat de sécurité** -Communication avec APNS doit être effectuée via le protocole SSL.


## <a name="creating-and-using-certificates"></a>Création et l’utilisation de certificats

Chacune des environnements mentionnés dans la section précédente requiert son propre certificat. Cette section décrit comment créer un certificat, associez-le à un profil de configuration, puis obtenir un certificat d’échange d’informations personnelles pour une utilisation avec PushSharp.

1.  Pour créer un certificat atteindre iOS Provisioning Portal sur le site Web d’Apple, comme indiqué dans la capture d’écran suivante (Notez l’élément de menu ID d’application sur la gauche) :

    [![](remote-notifications-in-ios-images/image5new.png "Le portail d’approvisionnement sur le site Web de pommes iOS")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Ensuite, accédez à la section de l’ID d’application et créer un nouvel ID d’application, comme indiqué dans la capture d’écran suivante :

    [![](remote-notifications-in-ios-images/image6new.png "Accédez à la section de l’ID d’application et créer un nouvel ID d’application")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Lorsque vous cliquez sur le **+** bouton, vous serez en mesure d’entrer la description et un identificateur de lot pour l’ID d’application, comme indiqué dans la capture d’écran suivante :

    [![](remote-notifications-in-ios-images/image7new.png "Entrez la description et un identificateur de lot pour l’ID d’application")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Veillez à sélectionner **ID d’application explicite** et que l’identificateur de lot ne se terminent par un `*` . Cette opération crée un identificateur qui est approprié pour plusieurs applications, et les certificats de notification push doivent être d’une seule application.

1. Sous les Services d’application, sélectionnez **des Notifications Push**:

    [![](remote-notifications-in-ios-images/image8new.png "Sélectionnez des Notifications Push")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. Appuyez sur **Submit** pour confirmer l’inscription de l’ID d’application :

    [![](remote-notifications-in-ios-images/image9new.png "Confirmer l’inscription de l’ID d’application")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Ensuite, vous devez créer le certificat pour l’ID d’application. Dans le volet de navigation gauche, accédez à **certificats > tous les** et sélectionnez le `+` bouton, comme illustré dans la capture d’écran suivante :

    [![](remote-notifications-in-ios-images/image10new.png "Créer le certificat pour l’ID d’application")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Indiquez si vous souhaitez utiliser un certificat de développement ou de Production :

    [![](remote-notifications-in-ios-images/image11new.png "Sélectionnez un certificat de développement ou de Production")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. Puis sélectionnez le nouvel ID d’application que nous avons créé uniquement :

    [![](remote-notifications-in-ios-images/image12new.png "Sélectionnez le nouvel ID d’application venez de créer")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Cela affiche des instructions qui vous guidera tout au long du processus de création d’un *demande de signature de certificat* à l’aide de la **trousseau d’accès** application sur votre Mac.

7.  Maintenant que le certificat a été créé, il doit être utilisé pour signer l’application afin qu’il peut s’inscrire avec APNs dans le cadre du processus de génération. Cela requiert la création et l’installation d’un profil de configuration qui utilise le certificat.

8.  Pour créer un profil de configuration de développement, accédez à la **profils de configuration** section et suivez les étapes pour créer, à l’aide de l’Id d’application que nous avons juste créé.

9.  Une fois que vous avez créé le profil de configuration, ouvrez **Xcode organisateur** et l’actualiser. Si le profil de configuration que vous avez créé n’apparaît pas qu'il peut être nécessaire télécharger le profil à partir du portail d’approvisionnement iOS et l’importer manuellement. La capture d’écran suivante montre un exemple de la bibliothèque multimédia avec le profil de configuration ajouté :

    [![](remote-notifications-in-ios-images/image13new.png "Cette capture d’écran montre un exemple de la bibliothèque multimédia avec le profil de configuration ajouté")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  À ce stade, nous devons configurer le projet Xamarin.iOS pour utiliser ce nouveau profil de configuration. Cette opération est effectuée à partir de **Options du projet** boîte de dialogue, sous **signature d’offre groupée iOS** onglet, comme dans la capture d’écran suivante :

    [![](remote-notifications-in-ios-images/image11.png "Configurer le projet Xamarin.iOS pour utiliser ce nouveau profil de configuration")](remote-notifications-in-ios-images/image11.png#lightbox)



À ce stade, l’application est configurée pour fonctionner avec des notifications push. Toutefois, il existe quelques étapes supplémentaires requises avec le certificat. Ce certificat est au format DER qui n’est pas compatible avec PushSharp, ce qui nécessite un certificat d’échange d’informations personnelles (PKCS12). Pour convertir le certificat afin qu’il soit utilisable par PushSharp, effectuez les étapes finales :

1.  **Téléchargez le fichier de certificat** -connexion dans le portail d’approvisionnement iOS choisissez l’onglet certificats, sélectionnez le certificat associé avec le profil de configuration approprié et choisissez **télécharger** .
1.  **Ouvrez le trousseau d’accès** -il s’agit d’application est une interface utilisateur graphique pour le système de gestion de mot de passe dans OS X.
1.  **Importez le certificat** - si le certificat n’est pas déjà installé, **fichier... Importer des éléments de** à partir du menu de trousseau d’accès. Accédez au certificat exporté ci-dessus et sélectionnez-le.
1.  **Exporter le certificat** - développez le certificat de la clé privée associée est visible, avec le bouton droit sur la clé et avez choisi d’exportation. Vous demandera un nom de fichier et un mot de passe pour le fichier exporté.

À ce stade, nous avons terminé avec des certificats. Nous avons créé un certificat qui permet de signer des applications iOS et converti ce certificat dans un format qui peut être utilisé avec PushSharp dans une application serveur. Suivant nous allons voir comment les applications iOS interagissent avec APNS.

## <a name="registering-with-apns"></a>L’inscription auprès de APNS

Avant un iOS application peut recevoir une notification à distance que doit s’inscrire avec APNS. APNS se générer un jeton d’appareil unique et que l’application iOS. L’application iOS sera alors le jeton du périphérique et ensuite de s’inscrire avec le serveur d’applications. Une fois que tous les dans ce cas, l’inscription est terminée et le serveur d’applications peut des notifications push vers le périphérique mobile.

En théorie, le jeton du périphérique peut changer chaque fois qu’une application iOS s’inscrit lui-même avec APNS, toutefois, dans la pratique cela qui souvent. En guise d’optimisation, une application peut mettre en cache le jeton du périphérique plus récent et uniquement mettre à jour le serveur d’applications quand elle change. Le diagramme suivant illustre le processus d’inscription et d’obtenir un jeton du périphérique :

 ![](remote-notifications-in-ios-images/image12.png "Ce diagramme illustre le processus d’inscription et l’obtention d’un jeton de périphérique")

Inscription APNS est gérée dans le `FinishedLaunching` méthode de la classe de délégué d’application en appelant `RegisterForRemoteNotificationTypes` active `UIApplication` objet. Lorsqu’une application iOS s’inscrit avec APNS, il doit également spécifier les types de notifications à distance qu’il souhaite recevoir. Ces types de notification à distance sont déclarées dans l’énumération `UIRemoteNotificationType`. L’extrait de code suivant est un exemple de la façon dont une application iOS peut inscrire pour recevoir des notifications d’alerte et des badges à distance :

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

La demande d’inscription APNS se produit en arrière-plan - lors de la réponse est reçue, iOS appellera la méthode `RegisteredForRemoteNotifications` dans la `AppDelegate` classe et passez le jeton de périphérique enregistré. Le jeton figure dans un `NSData` objet. L’extrait de code suivant montre comment récupérer le jeton de périphérique que APNS fourni :

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

Si l’inscription échoue pour une raison quelconque (tel que l’appareil est non connecté à Internet), iOS appelle `FailedToRegisterForRemoteNotifications` sur l’application de la classe déléguée. L’extrait de code suivant montre comment afficher une alerte à l’utilisateur pour l’informer que l’enregistrement a échoué :

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Nettoyage de jeton de périphérique

Jetons d’appareil seront expirer ou changer au fil du temps. En raison de ce fait, les applications serveur devront faire un nettoyage de la maison et purger ces jetons expirés ou modifiées. Lorsqu’une application envoie en tant que notification push sur un appareil qui a un jeton expiré, APNS enregistre et enregistrer ce jeton a expiré. Serveurs peuvent ensuite interroger APNS pour connaître les jetons ont expiré.

APNS est utilisé pour fournir un *commentaires Service* -point de terminaison HTTPS qui authentifie via le certificat qui a été créé pour renvoyer les push envoie des notifications et des données sur les jetons ont expiré. Cela a été déconseillée par Apple et supprimée.

Au lieu de cela, il est d’un nouveau code d’état HTTP pour le cas a été signalé précédemment par le Service de commentaires :

> 410 - le jeton du périphérique n’est plus actif pour la rubrique.

En outre, un nouveau `timestamp` clé des données JSON sera dans le corps de réponse :

> Si la valeur de le : en-tête de l’état est 410, la valeur de cette clé est la dernière fois à laquelle APNs a confirmé que le jeton du périphérique était n’est plus valide pour la rubrique.
>
> Arrêtez les envoi push notifications jusqu'à ce que l’appareil inscrit un jeton avec un horodatage plus loin avec votre fournisseur.

## <a name="summary"></a>Récapitulatif

Cette section présente les concepts clés liés à des notifications push dans iOS. Il explique le rôle de l’Apple Push Notification passerelle de Service (APNS). Il couvert puis la création et l’utilisation des certificats de sécurité qui sont essentiels pour APNS. Enfin ce document terminé par une discussion comment les serveurs d’applications peuvent utiliser le *commentaires Services* à arrêter le suivi expiré jetons d’appareil.


## <a name="related-links"></a>Liens associés

- [Notifications - montrant les notifications locales et distantes (exemple)](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Local et de Notifications Push pour les développeurs](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
