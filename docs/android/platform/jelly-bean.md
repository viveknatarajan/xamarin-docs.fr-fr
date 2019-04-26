---
title: Jelly Bean fonctionnalités
description: 'Ce document vous fournit une vue d’ensemble détaillée des nouvelles fonctionnalités pour les développeurs qui ont été introduites dans Android 4.1. Ces fonctionnalités incluent : améliorée des notifications, les mises à jour Android faisceaux de partager des fichiers volumineux, les mises à jour de la découverte de réseau peer-to-peer multimédia, les animations, les nouvelles autorisations.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 24fc14b0342591c56f5bf91862b0d94759a42834
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61078393"
---
# <a name="jelly-bean-features"></a>Jelly Bean fonctionnalités

_Ce document vous fournit une vue d’ensemble détaillée des nouvelles fonctionnalités pour les développeurs qui ont été introduites dans Android 4.1. Ces fonctionnalités incluent : améliorée des notifications, les mises à jour Android faisceaux de partager des fichiers volumineux, les mises à jour de la découverte de réseau peer-to-peer multimédia, les animations, les nouvelles autorisations._



## <a name="overview"></a>Vue d'ensemble

Android 4.1 (niveau d’API 16), également appelé « Jelly Bean », a été mise en production le 9 juillet 2012. Cet article fournit une introduction générale à certaines des nouvelles fonctionnalités dans Android 4.1 pour les développeurs à l’aide de Xamarin.Android. Certaines de ces nouvelles fonctionnalités introduites sont des animations pour le lancement d’une activité, de nouveaux sons pour un appareil photo et prise en charge améliorée pour la navigation de pile d’application. Il est désormais possible de couper et coller avec des intentions.

La stabilité des applications Android est améliorée avec la possibilité d’isoler la dépendance sur les fournisseurs de contenu instables. Services peuvent également être isolés afin qu’ils soient accessibles uniquement par l’activité qui a démarré les.

Prise en charge a été ajoutée pour la découverte de service de réseau à l’aide de Bonjour, UPnP ou multidiffusion DNS des services basés sur. Il est désormais possible pour les notifications plus riches qui ont mises en forme de texte, des boutons d’action et des images de grande taille.

Enfin, plusieurs nouvelles autorisations ont été ajoutées dans Android 4.1.



## <a name="requirements"></a>Configuration requise

Pour développer des applications Xamarin.Android à l’aide de Jelly Bean nécessite Xamarin.Android 4.2.6 ou supérieur et Android 4.1 (niveau API 16) être installé via le gestionnaire Android SDK comme indiqué dans la capture d’écran suivante :

[![Sélection d’Android 4.1 dans le gestionnaire Android SDK](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Nouveautés



### <a name="animations"></a>Animations

Les activités peuvent être lancées à l’aide des animations de zoom ou des animations personnalisées à l’aide de la `ActivityOptions` classe. Les nouvelles méthodes suivantes sont fournies pour prendre en charge ces animations :

-   `MakeScaleUpAnimation` – Cette opération crée une animation qui monte une fenêtre d’activité à partir d’une position de départ et la taille à l’écran.
-   `MakeThumbnailScaleUpAnimation` – Cette opération crée une animation qui monte à partir d’une image miniature à partir de la position spécifiée dans l’écran.
-   `MakeCustomAnimation` : Il crée une animation à partir des ressources dans l’application. Il existe une animation pour lorsque l’activité s’ouvre et l’autre à l’arrêt de l’activité.


La nouvelle `TimeAnimator` classe fournit une interface `TimeAnimator.ITimeListener` qui peut avertir une application chaque fois qu’un frame change dans une animation. Par exemple, considérez l’implémentation suivante de `TimeAnimator.ITimeListener`:

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

Comme le `TimeAnimator` instance est en cours d’exécution, il appelle `ITimeAnimator.ITimeListener`, qui enregistre alors la façon dont l’animation a depuis longtemps en cours d’exécution et la durée pendant laquelle il comme depuis la dernière fois que la méthode a été appelée.



### <a name="application-stack-navigation"></a>Navigation de pile d’application

Android 4.1 améliore la navigation de pile d’application qui a été introduite dans 3.0 Android. En spécifiant le `ParentName` propriété de la `ActivityAttribute`, Android peut ouvrir l’activité parente appropriée lorsque l’utilisateur appuie sur le [bouton](https://developer.android.com/design/patterns/navigation.html#up-vs-back) sur la barre d’action - Android instancie l’activité spécifiée par le `ParentName`propriété. Cela permet aux applications conserver la hiérarchie d’activités qui rendent une tâche donnée.

Pour la plupart des applications définissant le `ParentName` sur l’activité est suffisamment d’informations pour Android fournir le comportement correct pour naviguer dans la pile d’applications ; Android sera synthétiser la pile de retour nécessaires en créant une série d’intentions pour chaque activité parent. Toutefois, comme il s’agit d’une pile d’applications artificiel, chaque activité synthétique n’aura pas l’état enregistré ayant une activité naturelle. Pour fournir l’état enregistré pour une activité parente synthétique, une activité peut remplacer le `OnPrepareNavigationUpTaskStack` (méthode). Cette méthode reçoit un `TaskStackBuilder` objets d’instance qui auront un ensemble d’intention que Android utilisera pour créer la pile de retour. L’activité peut modifier ces intentions afin que, comme la création de l’activité synthétique, il recevra les informations d’état approprié.

Pour des scénarios plus complexes, il existe nouvelles méthodes sur la classe d’activité qui peut-être être utilisée pour gérer le comportement de la navigation et de construire la pile de retour :

-   `OnNavigateUp` – En substituant cette méthode, il est possible d’effectuer une action personnalisée lorsque le <span class="ui">des</span> bouton est enfoncé.
-   `NavigateUpTo` – Cette méthode entraîne l’application à accéder à partir de l’activité actuelle à l’activité spécifiée par une intention donnée.
-   `ParentActivityIntent` : Il est utilisé pour obtenir une intention qui lancera l’activité parente de l’activité actuelle.
-   `ShouldUpRecreateTask` – Cette méthode est utilisée pour interroger si la pile de retour synthétique doit être créée pour naviguer jusqu'à une activité parente. Retourne `true` si la pile synthétique doit être créée. 
-   `FinishAffinity` – Cette méthode se termine l’activité actuelle et toutes les activités en dessous de la tâche en cours qui ont une affinité même de la tâche.
-   `OnCreateNavigateUpTaskStack` – Cette méthode est substituée lorsqu’il est nécessaire de disposer d’un contrôle complet sur la façon dont la pile synthétique est créée.




### <a name="camera"></a>Appareil photo

Il existe une nouvelle interface, `Camera.IAutoFocusMoveCallback`, qui peut être utilisé pour détecter lorsque le focus automatique a démarré ou arrêté le déplacement. Un exemple de cette nouvelle interface peut être consultée dans l’extrait suivant :

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

-   `MediaActionSoundType.FocusComplete` – Cet son est lu en mettant l’accent est terminée.
-   `MediaActionSoundType.ShutterClick` – Ce son est lu lors de la prise d’une image d’image fixe.
-   `MediaActionSoundType.StartVideoRecording` – Ce son sert indiquer le début d’un enregistrement vidéo.
-   `MediaActionSoundType.StopVideoRecording` – Ce son est lu pour indiquer la fin de l’enregistrement vidéo.


Un exemple montrant comment utiliser le `MediaActionSound` classe peut être consultée dans l’extrait de code suivant :

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



#### <a name="android-beam"></a>Android Beam

FAISCEAU Android est une technologie de NFC en autorisant les appareils Android deux communiquer entre eux. Android 4.1 fournit une meilleure prise en charge pour le transfert de fichiers volumineux. Lors de l’utilisation de la nouvelle méthode `NfcAdapter.SetBeamPushUris()` Android bascule entre les mécanismes de transport autre (par exemple, Bluetooth) pour atteindre une vitesse de transfert rapide.



#### <a name="network-services-discovery"></a>Découverte de Services réseau

Android 4.1 contient de nouvelles API pour la découverte de service de multidiffusion basée sur DNS.
Cela permet à une application détecter et se connecter via le Wi-Fi avec d’autres appareils tels que des imprimantes, des caméras et des appareils multimédias. Ces nouvelles API est dans le `Android.Net.Nsd` package.

Pour créer un service qui peut être consommé par d’autres services, le `NsdServiceInfo` classe est utilisée pour créer un objet qui définit les propriétés d’un service. Cet objet est alors fourni `NsdManager.RegisterService()` ainsi que d’une implémentation de `NsdManager.ResolveListener`. Les implémentations de `NsdManager.ResolveListener` servent à avertir d’une inscription réussie et annuler l’inscription du service.

Pour découvrir les services sur le réseau et l’implémentation de `Nsd.DiscoveryListener` passé à `NsdManager.discoverServices()`.



#### <a name="network-usage"></a>Utilisation du réseau

Une nouvelle méthode, `ConnectivityManager.IsActiveNetworkMetered` permet à un appareil vérifier si elle est connecté à un réseau limitées. Cette méthode peut être utilisée pour aider à gérer l’utilisation des données en informant précisément des utilisateurs qu’il existe peut-être des frais coûteux pour les opérations de données.



#### <a name="wifi-direct-service-discovery"></a>Détection du Service Wi-Fi Direct

Le `WifiP2pManager` classe a été introduite dans Android 4.0 pour prendre en charge *zeroconf*. Zeroconf (aucune gestion de réseau de configuration) est un ensemble de techniques qui permette aux appareils (ordinateurs, imprimantes, téléphones) pour se connecter aux réseaux automatiquement, avec l’intervention des opérateurs humains réseau ou des serveurs de configuration spéciale.

Dans Jelly Bean, `WifiP2pManager` peut découvrir les périphériques à l’aide proches *Bonjour* ou *Upnp*. Bonjour est mise en œuvre d’Apple de zeroconf. UPnP est un ensemble de protocoles réseau qui prend également en charge zeroconf. Les méthodes suivantes, ajoutées à la `WiFiP2pManager` pour prendre en charge la détection du service Wi-Fi :

-   `AddLocalService()` – Cette méthode est utilisée annoncer une application en tant que service via le Wi-Fi pour la découverte par les homologues.
-   `AddServiceRequest(` ) : Cette méthode consiste à envoyer une demande de découverte de service à l’infrastructure. Elle est utilisée pour initialiser la découverte de service Wi-Fi.
-   `SetDnsSdResponseListeners()` – Cette méthode est utilisée pour enregistrer des rappels à appeler à la réception d’une réponse aux demandes de découverte de Bonjour.
-   `SetUpnpServiceResponseListener()` – Cette méthode est utilisée pour enregistrer des rappels à appeler pour recevoir une réponse aux demandes de découverte Upnp.




### <a name="content-providers"></a>Fournisseurs de contenu

Le `ContentResolver` classe a reçu une nouvelle méthode, `AcquireUnstableContentProvider`. Cette méthode permet à une application acquérir un fournisseur de contenu « instable ». Normalement, quand une application acquiert un fournisseur de contenu et ce fournisseur de contenu se bloque, il en sera de l’application. Avec cet appel de méthode, une application se bloquera pas si le fournisseur de contenu tombe en panne. Au lieu de cela, `Android.OS.DeadObjectionException` sera levée à partir des appels sur le fournisseur de contenu pour informer une application du fournisseur de contenu a disparu. Un fournisseur de contenu « instable » est utile lors de l’interaction avec les fournisseurs de contenu à partir d’autres applications : il est moins probable que le code bogué à partir d’une autre application affectera une autre application.



### <a name="copy-and-paste-with-intents"></a>Copier et coller avec des intentions

Le `Intent` classe peut désormais avoir un `ClipData` objet associé à celui-ci via le `Intent.ClipData` propriété. Cette méthode permet des données supplémentaires à partir du Presse-papiers doit être transmis avec l’intention. Une instance de `ClipData` peut contenir un ou plusieurs `ClipData.Item`. `ClipData.Item`de sont des éléments des types suivants :

-   **Texte** : il s’agit d’une chaîne de texte, soit HTML ou étendues de n’importe quelle chaîne dont le format est pris en charge par le style Android intégré.
-  **Intention** – n’importe laquelle `Intent` objet.
-   **URI** : cela peut être n’importe quel URI, par exemple un signet HTTP ou l’URI vers un fournisseur de contenu.




### <a name="isolated-services"></a>Services isolés

Un service isolé est un service qui s’exécute sous son propre processus spécial et dispose d’aucune autorisation de son propre. La seule communication avec le service est lorsque démarre le service et la liaison à ce dernier via l’API de Service. Il est possible de déclarer un service en tant qu’isolé en définissant la propriété `IsolatedProcess="true"` dans le `ServiceAttribute` qui orne une classe de service.


### <a name="media"></a>Médias

La nouvelle `Android.Media.MediaCodec` classe fournit une API permettant de codecs multimédias de bas niveau. Les applications peuvent interroger le système pour savoir quels codecs de bas niveau sont disponibles sur l’appareil.

La nouvelle `Android.Media.Audiofx.AudioEffect` sous-classes ont été ajoutées pour prendre en charge audio supplémentaire prétraitement sur audio capturé :

-   `Android.Media.Audiofx.AcousticEchoCanceler` – Cette classe est utilisée pour le prétraitement audio pour supprimer le signal d’un tiers distant à partir d’un signal audio capturé. Par exemple, la suppression l’écho à partir d’une application de communication vocale.
-   `Android.Media.Audiofx.AutomaticGainControl` – Cette classe est utilisée pour normaliser le signal capturé par boosting ou réduction d’un signal d’entrée afin que le signal de sortie est constant.
-   `Android.Media.Audiofx.NoiseSuppressor` -Cette classe supprime le bruit de fond le signal capturé.


Tous les périphériques prendra en charge ces effets. La méthode `AudioEffect.IsAvailable` doit être appelée par une application pour voir si l’effet audio en question est pris en charge sur l’appareil qui exécute l’application.

Le `MediaPlayer` classe prend désormais en charge la lecture du ininterrompue avec le `SetNextMediaPlayer()` (méthode). Cette nouvelle méthode spécifie MediaPlayer suivant pour démarrer le lecteur multimédia actuel issue de sa lecture.

Les nouvelles classes suivantes fournissent des mécanismes standard et l’interface utilisateur permettant de sélectionner où le média est lu :

-   `MediaRouter` – Cette classe permet aux applications de contrôler le routage des canaux de support à partir d’un appareil à haut-parleurs externes ou d’autres périphériques.
-   `MediaRouterActionProvider` et `MediaRouteButton` – ces classes permettent de fournir une interface utilisateur cohérente pour sélectionner et de lecture de médias.




### <a name="notifications"></a>Notifications

Android 4.1 permet aux applications de plus de souplesse et de contrôle d’affichage des notifications. Applications peuvent désormais afficher les notifications plues aux utilisateurs. Une nouvelle méthode, `NotificationBuilder.SetStyle()` permet d’être définie sur les notifications pour l’une des trois nouvelles nouveau style :

-   `Notification.BigPictureStyle` – Il s’agit d’une classe d’assistance qui génère des notifications qui contiennent une image dans les. L’illustration suivante montre un exemple d’une notification avec une grande image :


 [![Capture d’écran de l’exemple d’une notification BigPictureStyle](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – Il s’agit d’une classe d’assistance qui génère des notifications qui ont plusieurs lignes de texte, comme le courrier électronique. Un exemple de ce nouveau style de notification peut être consultée dans la capture d’écran suivante :


 [![Capture d’écran de l’exemple d’une notification BigTextStyle](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – Il s’agit d’une classe d’assistance qui génère des notifications qui contiennent une liste de chaînes, telles que des extraits de code à partir d’un message électronique, comme indiqué dans cette capture d’écran :


 [![Capture d’écran de l’exemple d’une notification Notification.InboxStyle](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Il est possible d’ajouter deux boutons d’action en bas d’un message de notification lorsque la notification utilise le style normal ou supérieur.
Un exemple de ce peut être consultée dans la capture d’écran suivante, où les boutons d’action sont visibles en bas de la notification :

 [![Capture d’écran de l’exemple de boutons d’action affichés un message de notification ci-dessous](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

Le `Notification` classe a reçu de nouvelles constantes qui permettent au développeur de spécifier l’une des cinq niveaux de priorité pour une notification. Ils peuvent être définis sur une notification à l’aide de la `Priority` propriété.



### <a name="permissions"></a>Autorisations

Les nouvelles autorisations suivantes ont été ajoutées :

-   `READ_EXTERNAL_STORAGE` -L’application requiert un accès en lecture seule à un stockage externe. Actuellement toutes les applications ont un accès en lecture par défaut, mais les versions futures d’Android nécessiteront applications demandent explicitement l’accès en lecture.
-   `READ_USER_DICTIONARY` -Permet un accès en lecture au dictionnaire de word de l’utilisateur.
-   `READ_CALL_LOG` -Permet d’obtenir des informations sur les appels entrants et sortants en lisant le journal des appels.
-   `WRITE_CALL_LOG` -Permet à une application à écrire dans le journal des appels sur le téléphone.
-   `WRITE_USER_DICTIONARY` -Permet à une application à écrire dans le dictionnaire de word de l’utilisateur.


Un changement important de noter `READ_EXTERNAL_STORAGE` – actuellement cette autorisation est accordée automatiquement par Android. Les versions futures d’Android nécessiteront une application de demander cette autorisation avant de l’autorisation.



## <a name="summary"></a>Récapitulatif

Cet article a présenté certaines de la nouvelle API qui sont disponible dans Android 4.1 (niveau API 16). Mis en surbrillance certaines modifications pour les animations et animer le lancement d’une activité, elle a introduit la nouvelle API Service découverte du réseau d’autres appareils à l’aide de protocoles tels que Bonjour ou UPnP. Autres modifications apportées à l’API ont été mis en surbrillance, comme la possibilité de couper et coller des données via les intentions, la possibilité d’utiliser des services isolés ou des fournisseurs de contenu « instables ».

Cet article s’est passé pour présenter les mises à jour les notifications, puis abordé certaines des nouvelles autorisations qui ont été introduites avec Android 4.1


## <a name="related-links"></a>Liens associés

- [Exemple d’Animation de temps (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 API](https://developer.android.com/about/versions/android-4.1.html)
- [Tâches et les piles Back](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Navigation précédent et à distance](https://developer.android.com/design/patterns/navigation.html)
