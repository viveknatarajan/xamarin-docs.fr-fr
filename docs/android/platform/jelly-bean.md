---
title: Fonctionnalités de Bean gelée
description: 'Ce document fournit une vue d’ensemble générale des nouvelles fonctionnalités pour les développeurs qui ont été introduits dans Android 4.1. Ces fonctionnalités sont notamment : améliorée des notifications, les mises à jour Android faisceaux de partager des fichiers volumineux, mises à jour de la découverte du réseau multimédia, égal à égal, les animations, les nouvelles autorisations.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d8068ccfc8d0f159a88704370261ec5f20d8b7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771549"
---
# <a name="jelly-bean-features"></a>Fonctionnalités de Bean gelée

_Ce document fournit une vue d’ensemble générale des nouvelles fonctionnalités pour les développeurs qui ont été introduits dans Android 4.1. Ces fonctionnalités sont notamment : améliorée des notifications, les mises à jour Android faisceaux de partager des fichiers volumineux, mises à jour de la découverte du réseau multimédia, égal à égal, les animations, les nouvelles autorisations._



## <a name="overview"></a>Vue d'ensemble

Android 4.1 (API niveau 16), également appelé « gelée Bean », a été mise en production 9 juillet 2012. Cet article fournit une introduction générale à certaines des nouvelles fonctionnalités dans Android 4.1 pour les développeurs à l’aide de Xamarin.Android. Certaines de ces nouvelles fonctionnalités introduites sont des animations pour le lancement d’une activité, de nouveaux sons pour un appareil photo et une prise en charge améliorée pour la navigation de pile d’application. Il est désormais possible de couper et coller avec d’intentions.

La stabilité des applications Android est d’améliorer la capacité à isoler la dépendance sur les fournisseurs de contenu instables. Services peuvent également être isolés afin qu’ils soient accessibles uniquement par l’activité qui a démarré les.

Prise en charge a été ajoutée pour la découverte de service réseau à l’aide Bonjour, UPnP ou multidiffusion DNS des services basés sur. Il est désormais possible pour les notifications plus riches que vous ont mis en forme de texte, des boutons d’action et des images de grande taille.

Enfin, plusieurs nouvelles autorisations ont été ajoutées dans Android 4.1.



## <a name="requirements"></a>Spécifications

Pour développer des applications de Xamarin.Android à l’aide de gelée Bean nécessite Xamarin.Android 4.2.6 ou supérieur et Android 4.1 (API niveau 16) être installé via le Gestionnaire de SDK Android comme indiqué dans la capture d’écran suivante :

[![Sélection d’Android 4.1 dans le Gestionnaire du SDK Android](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Nouveautés



### <a name="animations"></a>Animations

Les activités peuvent être lancées à l’aide des animations de zoom ou d’animations personnalisées à l’aide de la `ActivityOptions` classe. Pour prendre en charge ces animations, les nouvelles méthodes suivantes sont fournies :

-   `MakeScaleUpAnimation` – Cette opération crée une animation qui met à l’échelle d’une fenêtre d’activité à partir d’une position de début et de la taille à l’écran.
-   `MakeThumbnailScaleUpAnimation` – Cette action crée une animation qui passe d’une image miniature à la position spécifiée dans l’écran.
-   `MakeCustomAnimation` – Cette opération crée une animation à partir des ressources dans l’application. Il existe une animation pour lorsque l’activité s’ouvre et l’autre à l’arrêt de l’activité.


La nouvelle `TimeAnimator` classe fournit une interface `TimeAnimator.ITimeListener` qui peut avertir une application chaque fois qu’un frame change d’une animation. Par exemple, considérez l’implémentation suivante de `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

Et maintenant, pour utiliser la classe, une instance de `TimeAnimator` est créé, et l’écouteur est défini :

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

En tant que le `TimeAnimator` instance est en cours d’exécution, il appelle `ITimeAnimator.ITimeListener`, qui enregistre alors la durée pendant laquelle l’animation est en cours d’exécution et la durée pendant laquelle il comme depuis la dernière fois que la méthode a été appelée.



### <a name="application-stack-navigation"></a>Navigation de pile d’application

Android 4.1 améliore la navigation de pile d’application qui a été introduite dans 3.0 Android. En spécifiant le `ParentName` propriété de la `ActivityAttribute`, Android peut ouvrir le parent correct activité lorsque l’utilisateur appuie sur le [bouton](http://developer.android.com/design/patterns/navigation.html#up-vs-back) sur la barre d’action - Android instancie l’activité spécifiée par la `ParentName`propriété. Cela permet aux applications conserver la hiérarchie d’activités qui rendent une tâche donnée.

La plupart des applications définissant le `ParentName` sur l’activité est suffisamment d’informations pour Android fournir le comportement correct pour naviguer dans la pile de l’application ; Android est synthétiser la pile back nécessaire en créant une série de modes pour chaque activité parent. Toutefois, comme il s’agit d’une pile artificielle, chaque activité synthétique n’auront l’état enregistré ayant une activité naturelle. Pour fournir l’état enregistré pour une activité parent synthétique, une activité peut se substituer à la `OnPrepareNavigationUpTaskStack` (méthode). Cette méthode reçoit un `TaskStackBuilder` objets d’instance qui aura un ensemble de l’intention de que Android utilisera pour créer la pile back. L’activité peut modifier ces modes, afin que, lorsque l’activité synthétique est créée, il reçoit les informations d’état approprié.

Pour des scénarios plus complexes, il existe un nouvelles méthodes de la classe d’activité qui peut être utilisée pour gérer le comportement de la navigation et de construire la pile de retour :

-   `OnNavigateUp` – En substituant cette méthode, il est possible d’effectuer une action personnalisée lorsque le <span class="ui">des</span> bouton est enfoncé.
-   `NavigateUpTo` – Cette méthode entraîne l’application à accéder à partir de l’activité actuelle à l’activité spécifiée par un mode donné.
-   `ParentActivityIntent` : Il est utilisé pour obtenir une intention qui sera lancée à l’activité parente de l’activité actuelle.
-   `ShouldUpRecreateTask` – Cette méthode est utilisée pour interroger si la pile back synthétique doit être créée pour naviguer jusqu'à une activité parent. Retourne `true` si la pile synthétique doit être créée. 
-   `FinishAffinity` – Cette méthode termine l’activité actuelle et toutes les activités en dessous de la tâche en cours qui ont le même d’affinité de tâche.
-   `OnCreateNavigateUpTaskStack` – Cette méthode est substituée lorsqu’il est nécessaire de disposer d’un contrôle complet sur la façon dont la pile synthétique est créée.




### <a name="camera"></a>Appareil photo

Il existe une nouvelle interface `Camera.IAutoFocusMoveCallback`, qui peut être utilisé pour détecter les lorsque le focus automatique a démarré ou arrêté le déplacement. Un exemple de cette nouvelle interface peut être consulté dans l’extrait de code suivant :

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

La nouvelle classe `MediaActionSound` fournit un ensemble de l’API de production des sons pour les différentes actions de support. Il existe plusieurs actions qui peuvent se produire avec un appareil photo, elles sont définies par l’énumération `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – Ce son est lu lorsque le focus est terminée.
-   `MediaActionSoundType.ShutterClick` – Ce son est lu lors de la prise d’une image d’image fixe.
-   `MediaActionSoundType.StartVideoRecording` – Ce son sert indiquent le début de l’enregistrement vidéo.
-   `MediaActionSoundType.StopVideoRecording` – Ce son est lu pour indiquer la fin d’un enregistrement vidéo.


Un exemple montrant comment utiliser la `MediaActionSound` classe peut être consulté dans l’extrait de code suivant :

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>Connectivité



#### <a name="android-beam"></a>Faisceaux Android

Faisceaux Android est une technologie de NFC en fonction qui permet aux deux appareils Android de communiquer entre eux. Android 4.1 fournit une meilleure prise en charge pour le transfert de fichiers volumineux. Lors de l’utilisation de la nouvelle méthode `NfcAdapter.SetBeamPushUris()` Android basculent entre les mécanismes de transport autre (par exemple, Bluetooth) pour obtenir une vitesse de transfert rapide.



#### <a name="network-services-discovery"></a>Découverte de Services réseau

Android 4.1 contient de nouvelles API pour la découverte de service de multidiffusion basée sur DNS.
Cela permet à une application détecter et se connecter via le Wi-Fi avec d’autres périphériques tels que des imprimantes, des appareils et appareils multimédias. Ces API est dans le `Android.Net.Nsd` package.

Pour créer un service qui peut être consommé par d’autres services, le `NsdServiceInfo` classe est utilisée pour créer un objet qui définit les propriétés d’un service. Cet objet est alors fourni à `NsdManager.RegisterService()` ainsi que d’une implémentation de `NsdManager.ResolveListener`. Les implémentations de `NsdManager.ResolveListener` sont utilisées pour informer d’une inscription réussie et pour annuler l’inscription du service.

Pour découvrir les services sur le réseau et l’implémentation de `Nsd.DiscoveryListener` passé à `NsdManager.discoverServices()`.



#### <a name="network-usage"></a>Utilisation du réseau

Une nouvelle méthode `ConnectivityManager.IsActiveNetworkMetered` permet à un périphérique vérifier si elle est connectée à un réseau limitée. Cette méthode peut être utilisée pour aider à gérer l’utilisation des données avec précision pour informer les utilisateurs qu’il existe peut-être des frais coûteux pour les opérations de données.



#### <a name="wifi-direct-service-discovery"></a>Détection du Service Wi-Fi Direct

Le `WifiP2pManager` classe a été introduite dans Android 4.0 pour prendre en charge *zeroconf*. Zeroconf (aucun réseau de configuration) est un ensemble de techniques qui permet aux appareils (ordinateurs, imprimantes, les téléphones) pour se connecter automatiquement aux réseaux avec l’intervention d’opérateurs réseau humaine ou les serveurs de configuration particulière.

Dans le composant Bean gelée, `WifiP2pManager` peut découvrir à l’aide de périphériques à proximité *Bonjour* ou *Upnp*. Bonjour est implémentation d’Apple zeroconf. UPnP est un ensemble de protocoles réseau qui prend également en charge zeroconf. Les méthodes suivantes, ajoutées à la `WiFiP2pManager` pour prendre en charge la détection du service Wi-Fi :

-   `AddLocalService()` – Cette méthode est utilisée annoncer une application en tant que service via le Wi-Fi pour la découverte par les homologues.
-   `AddServiceRequest(` ) : Cette méthode est d’envoyer une demande de découverte de service à l’infrastructure. Elle est utilisée pour initialiser la découverte de service Wi-Fi.
-   `SetDnsSdResponseListeners()` – Cette méthode est utilisée pour enregistrer des rappels à appeler lors de la réception d’une réponse aux demandes de découverte de Bonjour.
-   `SetUpnpServiceResponseListener()` – Cette méthode est utilisée pour enregistrer des rappels à appeler lors de la réception d’une réponse aux demandes de découverte Upnp.




### <a name="content-providers"></a>Fournisseurs de contenu

Le `ContentResolver` classe a reçu une nouvelle méthode, `AcquireUnstableContentProvider`. Cette méthode permet à une application acquérir un fournisseur de contenu « instable ». Normalement, quand une application acquiert un fournisseur de contenu et que celui-ci tombe en panne, les applications. Avec cet appel de méthode, une application ne bloquera pas si le fournisseur de contenu tombe en panne. Au lieu de cela, `Android.OS.DeadObjectionException` est levée à partir des appels sur le fournisseur de contenu pour informer une application du fournisseur de contenu a disparu. Un fournisseur de contenu « instable » est utile lors de l’interaction avec les fournisseurs de contenu à partir d’autres applications, il est moins probable que le code incorrect à partir d’une autre application affectera une autre application.



### <a name="copy-and-paste-with-intents"></a>Copier et coller avec les modes

Le `Intent` classe peut désormais avoir un `ClipData` objet associé à celui-ci via la `Intent.ClipData` propriété. Cette méthode permet de données supplémentaires à partir du Presse-papiers doit être transmis avec l’intention. Une instance de `ClipData` peut contenir un ou plusieurs `ClipData.Item`. `ClipData.Item`d’éléments des types suivants :

-   **Texte** : il s’agit d’une chaîne de texte, soit HTML ou n’importe quelle chaîne dont le format est pris en charge par le style Android intégré s’étend sur.
-  **Intention de** – n’importe laquelle `Intent` objet.
-   **URI** : cela peut être n’importe quel URI, par exemple un signet HTTP ou l’URI vers un fournisseur de contenu.




### <a name="isolated-services"></a>Services isolés

Un service isolé est un service qui s’exécute sous son propre processus spécial et n’est pas autorisé de son propre. La seule communication avec le service est lorsque démarrage du service et la liaison à ce dernier via l’API de Service. Il est possible de déclarer un service isolés en définissant la propriété `IsolatedProcess="true"` dans les `ServiceAttribute` qui orne une classe de service.


### <a name="media"></a>Médias

La nouvelle `Android.Media.MediaCodec` classe fournit une API pour les codecs de support de bas niveau. Les applications peuvent interroger le système pour savoir quels codecs de bas niveau sont disponibles sur l’appareil.

La nouvelle `Android.Media.Audiofx.AudioEffect` sous-classes ont été ajoutées pour prendre en charge supplémentaire audio de pré-traitement sur audio capturé :

-   `Android.Media.Audiofx.AcousticEchoCanceler` : Cette classe est utilisée pour le traitement de l’audio pour supprimer le signal d’un tiers à distance à partir d’un signal audio capturé. Par exemple, suppression de l’écho à partir d’une application de communication de voix.
-   `Android.Media.Audiofx.AutomaticGainControl` – Cette classe est utilisée pour normaliser le signal capturé par la valorisation ou réduire un signal d’entrée afin que le signal de sortie est constant.
-   `Android.Media.Audiofx.NoiseSuppressor` -Cette classe supprime le bruit de fond de signal capturé.


Tous les périphériques prendront en charge ces effets. La méthode `AudioEffect.IsAvailable` doit être appelée par une application pour voir si l’effet audio en question est pris en charge sur l’appareil qui exécute l’application.

Le `MediaPlayer` classe prend désormais en charge la lecture ininterrompue avec la `SetNextMediaPlayer()` (méthode). Cette nouvelle méthode spécifie MediaPlayer suivant pour démarrer le lecteur multimédia actuel après sa lecture.

Nouvelles classes suivantes fournissent des mécanismes standard et l’interface utilisateur pour la sélection de support sur lequel est lu :

-   `MediaRouter` Cette classe permet aux applications de contrôler le routage de canaux de support à partir d’un appareil pour haut-parleurs externes ou d’autres périphériques.
-   `MediaRouterActionProvider` et `MediaRouteButton` – ces classes permettent de fournir une interface utilisateur cohérente permettant de sélectionner et de lecture du contenu multimédia.




### <a name="notifications"></a>Notifications

Android 4.1 permet aux applications de plus de souplesse et de contrôle d’affichage des notifications. Applications peuvent désormais afficher plues notifications aux utilisateurs. Une nouvelle méthode, `NotificationBuilder.SetStyle()` permet d’être définie sur les notifications pour l’une des trois nouveaux nouveau style :

-   `Notification.BigPictureStyle` – Il s’agit d’une classe d’assistance qui génère des notifications ayant une image dans. L’illustration suivante montre un exemple d’une notification avec une grande image :


 [![Capture d’écran de l’exemple d’une notification BigPictureStyle](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – Il s’agit d’une classe d’assistance qui génère des notifications qui ont plusieurs lignes de texte, comme le courrier électronique. Un exemple de ce nouveau style de notification sont consultables dans la capture d’écran suivante :


 [![Capture d’écran de l’exemple d’une notification BigTextStyle](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – Il s’agit d’une classe d’assistance qui génère des notifications qui contiennent une liste de chaînes, telles que des extraits de code à partir d’un message électronique, comme indiqué dans cette capture d’écran :


 [![Capture d’écran de l’exemple d’une notification Notification.InboxStyle](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Il est possible d’ajouter deux boutons d’action au bas d’un message de notification lors de la notification utilise le style normal ou supérieur.
Ceci peut s’affichent dans la capture d’écran suivante, où les boutons d’action sont visibles au bas de la notification :

 [![Capture d’écran de boutons d’action affichés ci-dessous un message de notification](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

La `Notification` classe a reçu de nouvelles constantes qui permettent au développeur de spécifier l’une des cinq niveaux de priorité d’une notification. Ils peuvent être définis sur une notification à l’aide de la `Priority` propriété.



### <a name="permissions"></a>Autorisations

Les nouvelles autorisations suivantes ont été ajoutées :

-   `READ_EXTERNAL_STORAGE` -L’application requiert un accès en lecture seule à un stockage externe. Actuellement toutes les applications ont accès en lecture par défaut, mais les versions futures d’Android nécessitera applications demandent explicitement l’accès en lecture.
-   `READ_USER_DICTIONARY` -Permet un accès en lecture au dictionnaire word de l’utilisateur.
-   `READ_CALL_LOG` -Permet d’obtenir plus d’informations sur les appels entrants et sortants en lisant le journal d’appel.
-   `WRITE_CALL_LOG` -Permet à une application à écrire dans le journal d’appel sur le téléphone.
-   `WRITE_USER_DICTIONARY` -Permet à une application à écrire dans le dictionnaire de l’utilisateur.


Un changement important de noter `READ_EXTERNAL_STORAGE` – actuellement cette autorisation est accordée automatiquement par Android. Les versions futures de Android nécessitera une application de demander cette autorisation avant l’autorisation.



## <a name="summary"></a>Récapitulatif

Cet article a introduit des de la nouvelle API qui sont disponible dans Android 4.1 (API niveau 16). Il mis en évidence certaines des modifications pour les animations et animer le lancement d’une activité et a introduit la nouvelle API Service découverte du réseau d’autres appareils à l’aide des protocoles tels que Bonjour ou UPnP. D’autres modifications de l’API ont été mis en surbrillance, telles que la capacité à couper et coller des données, par conséquent, la possibilité d’utiliser les services isolés ou des fournisseurs de contenu « instables ».

Cet article avant de présenter les mises à jour des notifications, puis décrit quelques-unes des nouvelles autorisations qui ont été introduites avec Android 4.1


## <a name="related-links"></a>Liens associés

- [Exemple d’Animation heure (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 API](http://developer.android.com/about/versions/android-4.1.html)
- [Tâches et les piles arrière](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Navigation précédent et à distance](http://developer.android.com/design/patterns/navigation.html)
