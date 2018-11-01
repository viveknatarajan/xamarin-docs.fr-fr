---
title: Services en cours d’exécution Android dans des processus à distance
description: En règle générale, tous les composants dans une application Android seront exécutés dans le même processus. Services Android sont une exception notable, car ils peuvent être configurés pour s’exécuter dans leur propre processus et partagés avec d’autres applications, y compris ceux d’autres développeurs Android. Ce guide explique comment créer et utiliser un service distant Android à l’aide de Xamarin.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 8514d3b2c423e524d03a800f5f56359f3aee4b75
ms.sourcegitcommit: 650fd5813e243d67eea13c4bc76683c0f8134123
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50737191"
---
# <a name="running-android-services-in-remote-processes"></a>Services en cours d’exécution Android dans des processus à distance

_En règle générale, tous les composants dans une application Android seront exécutés dans le même processus. Services Android sont une exception notable, car ils peuvent être configurés pour s’exécuter dans leur propre processus et partagés avec d’autres applications, y compris ceux d’autres développeurs Android. Ce guide explique comment créer et utiliser un service distant Android à l’aide de Xamarin._

## <a name="out-of-process-services-overview"></a>Vue d’ensemble des Services de processus à l’emploi

Lorsqu’une application démarre, Android crée un processus dans lequel exécuter l’application. En règle générale, tous les composants de l’application s’exécutera dans ce processus une seule. Services Android sont une exception notable, car ils peuvent être configurés pour s’exécuter dans leur propre processus et partagés avec d’autres applications, y compris ceux d’autres développeurs Android. Ces types de services sont appelés _services à distance_ ou _out-of-process services_. Le code pour ces services se trouvera dans le fichier APK même en tant que l’application principale ; Toutefois, lorsque le service est démarré Android créera un nouveau processus pour un seul service. En revanche, un service qui s’exécute dans le même processus que le reste de l’application est parfois appelé un _service local_.

En règle générale, il n’est pas nécessaire pour une application implémenter un service distant. Un service local est suffisant (et souhaitable) pour la configuration requise d’une application dans de nombreux cas. Un out-of-process a son propre espace mémoire qui doit être géré par Android. Bien que cela introduit davantage de surcharge dans l’application globale, il existe certains scénarios où il peut être avantageux d’exécuter un service dans son propre processus :

1. **Fonctionnalité de partage** &ndash; certains développeurs d’applications peuvent avoir plusieurs applications et les fonctionnalités partagées entre toutes les applications. Empaquetage de cette fonctionnalité dans un service Android qui s’exécute dans son propre processus peut-être simplifier la maintenance de l’application. Il est également possible d’empaqueter le service dans son propre fichier APK autonome et le déployer séparément du reste de l’application.
2. **Amélioration de l’expérience utilisateur** &ndash; il existe deux façons possibles qu’un service out-of-process peut améliorer l’expérience utilisateur de l’application. La première méthode traite avec gestion de la mémoire. Quand un garbage collection (GC) cycle se produit, Android va suspendre toutes les activités dans le processus jusqu'à ce que le garbage collector est terminé. L’utilisateur peut noter cette pause en tant que « interruption » ou « jank ». Lorsqu’un service est en cours d’exécution des processus propres, c’est le processus de service est suspendu, pas le processus d’application. Cette interruption sera beaucoup moins visible à l’utilisateur comme le processus d’application (et par conséquent, l’interface utilisateur) ne sont pas interrompues.

    Deuxièmement, si les besoins en mémoire d’un processus devient trop volumineux, Android risque d’arrêter ce processus pour libérer des ressources pour l’appareil. Si un service n’a pas une grande quantité de mémoire et il est en cours d’exécution dans le même processus que l’interface utilisateur, puis quand Android récupère force ces ressources de l’interface utilisateur va être arrêté, forçant ainsi l’utilisateur pour démarrer l’application. Toutefois, si un service en cours d’exécution dans son propre processus est arrêté par Android, le processus de l’interface utilisateur reste inchangé. L’interface utilisateur peut lier (et redémarrer) le service, transparent à l’utilisateur et reprendre un fonctionnement normal.

3. **Amélioration des performances de l’Application** &ndash; processus de l’interface utilisateur peut être arrêtée ou arrêter indépendant du processus de service. En déplaçant les tâches de longue durée de démarrage à un service out-of-process, il est possible que le temps de démarrage de l’interface utilisateur améliorée peut-être (en supposant que le processus de service reste actif entre les temps de lancée de l’interface utilisateur).

Dans bien des égards, la liaison à un service en cours d’exécution dans un autre processus est identique à [liaison à un service local](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Le client appellera `BindService` pour lier (et démarrer, si nécessaire) le service. Un `Android.OS.IServiceConnection` objet sera créé pour gérer la connexion entre le client et le service. Si le client est correctement lié au service, Android retournera un objet via le `IServiceConnection` qui peut être utilisé pour appeler des méthodes sur le service. Le client interagit ensuite avec le service à l’aide de cet objet. Pour passer en revue, voici les étapes pour lier un service :

* **Créer une intention** &ndash; une intention explicite doit être utilisé pour la liaison au service.
* **Mettre en œuvre et instancier une `IServiceConnection` objet** &ndash; le `IServiceConnection` objet agit comme intermédiaire entre le client et le service.  Il est responsable de la surveillance de la connexion entre client et serveur.
* **Appeler le `BindService` méthode** &ndash; appelant `BindService` dépêchera l’objectif et la connexion de service créé dans les étapes précédentes pour Android, qui se chargera de démarrage du service et l’établissement de la communication entre le client et le service.

La nécessité de franchir les limites de processus introduit néanmoins une complexité supplémentaire : la communication est unidirectionnels (client et serveur) et le client ne peut pas appeler directement des méthodes sur la classe de service. Rappelez-vous que, quand un service s’exécute le même processus que le client, Android fournit une `IBinder` objet qui peut permettre une communication bilatérale. Cela n’est pas le cas avec le service en cours d’exécution dans son propre processus. Un client communique avec un service distant à l’aide de la `Android.OS.Messenger` classe.

Lorsqu’un client demande à lier avec le service distant, Android appellera le `Service.OnBind` méthode du cycle de vie, qui retournera interne `IBinder` objet encapsulé par le `Messenger`. Le `Messenger` est un wrapper mince sur une spéciale `IBinder` implémentation est fournie par le Kit de développement logiciel Android. Le `Messenger` s’occupe de la communication entre les deux processus différents. Le développeur est indifférente avec les détails de la sérialisation de message, le message de marshaling au-delà de la limite de processus et puis de le désérialiser sur le client. Ce travail est géré par le `Messenger` objet. Ce diagramme illustre les composants Android côté client qui sont impliqués lorsqu’un client établit la liaison à un service out-of-process :

![Diagramme illustrant les étapes et les composants pour une liaison à un service de client](out-of-process-services-images/ipc-01.png "diagramme illustrant les étapes et les composants pour une liaison à un service de client.")

Le `Service` classe dans le processus distant passera par les rappels même cycle de vie qui passe par un service lié dans un processus local et de nombreuses API impliqués sont identiques. `Service.OnCreate` est utilisé pour initialiser un `Handler` et injecter dans `Messenger` objet. De même, `OnBind` est substituée, mais au lieu de retourner un `IBinder` objet, le service renvoie le `Messenger`.  Ce diagramme illustre ce qui se passe dans le service lorsqu’un client est lié à celui-ci :

![Diagramme qui montre les étapes et les composants à un service s’effectue en cours de liaison par un client distant](out-of-process-services-images/ipc-02.png "diagramme qui montre les étapes et les composants à un service s’effectue en cours de liaison par un client distant.")

Quand un `Message` est reçu par un service, il est traité par dans l’instance de `Android.OS.Handler`. Le service sera implémenter son propre `Handler` sous-classe doit remplacer le `HandleMessage` (méthode). Cette méthode est appelée par le `Messenger` et reçoit le `Message` en tant que paramètre. Le `Handler` inspecte le `Message` métadonnées et utiliser ces informations pour appeler des méthodes sur le service.

Communication unidirectionnelle se produit lorsqu’un client crée un `Message` de l’objet et l’envoie au service en utilisant le `Messenger.Send` (méthode). `Messenger.Send` Sérialise le `Message` et remettez cet des données sérialisées pour Android, qui achemine le message sur la limite de processus et le service.  Le `Messenger` qui est hébergé par le service créera un `Message` objet à partir des données entrantes. Cela `Message` est placé dans une file d’attente, où les messages sont celui soumis à la fois pour le `Handler`. Le `Handler` inspecte les métadonnées contenues dans le `Message` et appeler les méthodes appropriées sur le `Service`. Le diagramme suivant illustre ces différents concepts en action :

![Diagramme montrant comment les messages sont transmis entre les processus](out-of-process-services-images/ipc-03.png "diagramme montrant comment les messages sont transmis entre les processus.")

Ce guide décrira les détails d’implémentation d’un service out-of-process. Il explique comment implémenter un service qui est destiné à être exécuté dans son propre processus et comment un client peut communiquer avec ce service à l’aide de la `Messenger` framework. Il étudie également brièvement la communication bidirectionnelle : le client qui envoie un message à un service et le service envoie un message au client. Étant donné que les services peuvent être partagés entre différentes applications, ce guide traite également une technique permettant de limiter l’accès client au service à l’aide d’autorisations Android.

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950 - Services avec un processus isolé et de la classe d’Application personnalisée ne pas résoudre correctement les surcharges](https://github.com/xamarin/xamarin-android/issues/1950) rapports un service de Xamarin.Android ne démarre pas correctement lorsque le `IsolatedProcess` est défini sur `true`. Ce guide est fourni pour une référence. Une application Xamarin.Android doit toujours être en mesure de communiquer avec un service out-of-process qui est écrit en Java.

## <a name="requirements"></a>Configuration requise

Ce guide suppose que vous êtes familiarisé avec la création de services.

Bien qu’il soit possible d’utiliser les intentions implicites avec les applications qui ciblent plus anciennes API Android, ce guide se concentrera exclusivement sur l’utilisation d’intentions explicites. Applications qui ciblent Android 5.0 (niveau 21 d’API) ou une version ultérieure doivent utiliser d’une intention explicite à lier à un service ; Il s’agit de la technique qui est présentée dans ce guide...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Créer un Service qui s’exécute dans un processus distinct

Comme décrit ci-dessus, le fait qu’un service s’exécute dans son propre processus signifie que certaines API différents est impliqués. Une vue d’ensemble rapide, voici les étapes pour lier avec et consommer un service distant :  

* **Créer le `Service` sous-classe** &ndash; sous-classe la `Service` tapez et implémenter les méthodes de cycle de vie d’un service lié. Il est également nécessaire de définir des métadonnées qui contribueront à Android utilisée par le service doit s’exécuter dans son propre processus.
* **Implémentez un `Handler`**  &ndash; le `Handler` est responsable de l’analyse les demandes du client, extraction de tous les paramètres qui ont été passés à partir du client et appeler les méthodes appropriées sur le service.
* **Instancier un `Messenger`**  &ndash; comme décrit ci-dessus, chaque `Service` doit conserver une instance de la `Messenger` classe qui achemine les demandes des clients vers le `Handler` qui a été créé à l’étape précédente.

Un service qui est destiné à être exécuté dans son propre processus est, fondamentalement, toujours un service lié. La classe de service étendra la base de `Service` classe et qui est agrémentée avec le `ServiceAttribute` contenant les métadonnées devant Android offre groupée dans le manifeste Android. Pour commencer avec les propriétés suivantes de la `ServiceAttribute` qui sont importantes pour un service out-of-process :

1. `Exported` &ndash; Cette propriété doit être définie sur `true` pour permettre aux autres applications d’interagir avec le service. La valeur par défaut de cette propriété est `false`.
2. `Process` &ndash; Cette propriété doit être définie. Il est utilisé pour spécifier le nom du processus que le service s’exécutera dans.
3. `IsolatedProcess` &ndash; Cette propriété permet une sécurité supplémentaire, indiquant Android pour exécuter le service dans un bac à sable isolé avec l’autorisation minimale d’iteract avec le reste du système. Consultez [Bugzilla 51940 - Services avec un processus isolé et de la classe d’Application personnalisée ne parviennent pas à résoudre les surcharges correctement](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; Il est possible de contrôler l’accès client au service en spécifiant une autorisation que les clients doivent demander (et accordées).

Pour exécuter un service de son propre processus, le `Process` propriété sur le `ServiceAttribute` doit être définie sur le nom du service. Pour interagir avec les applications externes, la `Exported` propriété doit être définie sur `true`. Si `Exported` est `false`, uniquement aux clients dans le même APK (par exemple, la même application) et en cours d’exécution dans le même processus seront alors en mesure d’interagir avec le service.

Quel type de processus que le service s’exécutera dans dépend de la valeur de la `Process` propriété. Android identifie les trois différents types de processus :

-   **Processus privé** &ndash; un processus privé est un qui est uniquement disponible pour l’application qui l’a démarrée. Pour identifier un processus comme privés, son nom doit commencer par un **:** (point-virgule). Le service illustré dans la capture d’écran et un extrait de code précédent est un processus privé. L’extrait de code suivant est un exemple de la `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Processus global** &ndash; un service qui est exécuté dans un processus global est accessible à toutes les applications en cours d’exécution sur l’appareil. Un processus global doit être un nom de classe qualifié complet qui commence par une lettre minuscule.
    (À moins que les mesures sont prises pour sécuriser le service, autres applications peuvent lier et interagir avec lui. Sécurisation du service contre toute utilisation non autorisée est abordée plus loin dans ce guide.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Isolé processus** &ndash; un processus isolé est un processus qui s’exécute dans son propre bac à sable, isolé du reste du système et aucune autorisation spéciale qui lui sont propres. Pour exécuter un service dans un processus isolé, le `IsolatedProcess` propriété de la `ServiceAttribute` est défini sur `true` comme indiqué dans cet extrait de code :
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Consultez [Bugzilla 51940 - Services avec un processus isolé et de la classe d’Application personnalisée ne parviennent pas à résoudre correctement les surcharges](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Un service isolé est un moyen simple de sécuriser une application et l’appareil contre le code non approuvé. Par exemple, une application peut télécharger et exécuter un script à partir d’un site Web. Dans ce cas, cette exécution dans un processus isolé fournit une couche supplémentaire de sécurité par rapport au code non fiable, compromettre l’appareil Android.

> [!IMPORTANT]
> Une fois qu’un service a été exporté, le nom du service ne doit pas modifier. Modification du nom du service peut-être gêner des autres applications qui utilisent le service.

Pour voir l’effet que la `Process` a de propriété, la capture d’écran suivante montre un service s’exécutant dans son propre processus privé :

![Capture d’écran montrant un service s’exécutant dans un processus privé](out-of-process-services-images/ipc-04.png "capture d’écran montrant un service s’exécutant dans un processus privé.")

Cette capture d’écran suivante montre `Process="com.xamarin.xample.messengerservice.timestampservice_process"` et le service en cours d’exécution dans un processus global :

![Capture d’écran d’un service en cours d’exécution dans un processus global](out-of-process-services-images/ipc-05.png "capture d’écran d’un service en cours d’exécution dans un processus global.")

Une fois le `ServiceAttribute` a été défini, le service doit implémenter un `Handler`.

### <a name="implementing-a-handler"></a>Implémentation d’un gestionnaire

Pour traiter les demandes des clients, le service doit implémenter un `Handler` et remplacer le `HandleMessage` methodThis est la méthode prend un `Message` quels qui encapsule l’appel de méthode à partir du client et traduit qui appellent des actions de l’instance ou tâche qui effectue le service. Le `Message` objet expose une propriété appelée `What` qui est une valeur entière, dont la signification est partagée entre le client et le service et est lié à une tâche le service consiste à effectuer pour le client.

L’extrait de code suivant à partir de l’exemple d’application illustre un exemple de `HandleMessage`. Dans cet exemple, il existe deux actions qu’un client peut demander du service :

* La première action est un _Hello, World_ message, le client a envoyé un message simple au service.
* La deuxième action appeler une méthode sur le service et la récupération une chaîne, dans ce cas la chaîne est un message qui renvoie à quelle heure le service de prise en main et la durée pendant laquelle qu'il a été en cours d’exécution :

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client as sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

Il est également possible de paramètres de package pour le service dans le `Message`. Ce sujet sera abordé plus loin dans ce guide. La création de la rubrique suivante à prendre en compte la `Messenger` objet à traiter entrant `Message`s.

### <a name="instantiating-the-messenger"></a>L’instanciation du Messenger

Comme indiqué précédemment, la désérialisation du `Message` objet et l’appel `Handler.HandleMessage` est la gestion de la `Messenger` objet. Le `Messenger` classe fournit également un `IBinder` que le client utilisera pour envoyer des messages au service de l’objet.  

Lorsque le service démarre, il instancie le `Messenger` et injecter le `Handler`. Un bon point pour effectuer cette initialisation est sur le `OnCreate` méthode du service. Cet extrait de code est un exemple d’un service qui initialise sa propre `Handler` et `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

À ce stade, l’étape finale consiste pour le `Service` substituer `OnBind`.

### <a name="implementing-serviceonbind"></a>Implémentation Service.OnBind

Tous les services liés, qu’elles soient exécutées dans leur propre processus ou non, doivent implémenter le `OnBind` (méthode). La valeur de retour de cette méthode est un objet que le client peut utiliser pour interagir avec le service. Exactement ce que cet objet est dépend si le service est un service local ou un service distant. Pendant un service local retournera un personnalisé `IBinder` implémentation, un service distant retourne le `IBinder` qui est encapsulé mais le `Messenger` qui a été créé dans la section précédente :

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Une fois ces trois étapes sont accomplies, le service distant peut être considérée comme terminé.

## <a name="consuming-the-service"></a>Consommation du Service

Tous les clients doivent implémenter du code pour être en mesure de lier et consommer le service distant. Sur le plan conceptuel, à partir du point de vue du client, il existe très peu de différences entre la liaison au service local ou un service à distance. Le client appelle le `BindService` méthode, en passant une intention explicite pour identifier le service et un `IServiceConnection` permettant de gérer la connexion entre le client et le service.

Cet extrait de code est un exemple montrant comment créer un **intention explicite** pour la liaison à un service distant. L’objectif doit identifier le package qui contient le service et le nom du service. Pour définir ces informations consiste à utiliser un `Android.Content.ComponentName` objet et à fournir à l’intention. Cet extrait de code est un exemple :  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

Lorsque le service est lié, le `IServiceConnection.OnServiceConnected` méthode est appelée et fournit un `IBinder` à un client. Toutefois, le client s’utilisent pas directement la `IBinder`. Au lieu de cela, il instancie un `Messenger` objet à partir de qui `IBinder`. Il s’agit du `Messenger` que le client utilisera pour interagir avec le service distant.

Voici un exemple d’une très basique `IServiceConnection` implémentation qui montre comment un client traiterait connexion et déconnexion à partir d’un service. Notez que le `OnServiceConnected` méthode reçoit et `IBinder`, et le client crée un `Messenger` de celui `IBinder`:

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

Une fois la connexion de service et l’intention sont créés, il est possible pour le client appelle `BindService` et lancer le processus de liaison :

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Une fois que le client a correctement lié au service et la `Messenger` est disponible, il est possible pour le client envoie `Messages` au service.

## <a name="sending-messages-to-the-service"></a>Envoi de Messages au Service

Une fois que le client est connecté et a un `Messenger` de l’objet, il est possible de communiquer avec le service via une distribution `Message` objets le `Messenger`. Cette communication est unidirectionnelle, le client envoie le message, mais il n’existe aucun message de retour à partir du service au client. À cet égard, le `Message` est un mécanisme de déclenchement et oubli.

La meilleure façon de créer un `Message` objet consiste à utiliser le [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) méthode de fabrique. Cette méthode extrait un `Message` objet à partir d’un pool global qui est géré par Android. `Message.Obtain` également a certaines des méthodes surchargées qui permettent la `Message` objet soit initialisé avec les valeurs et les paramètres requis par le service.  Une fois le `Message` est instancié, il distribué au service en appelant `Messenger.Send`. Cet extrait de code est un exemple de création et la distribution un `Message` au processus du service :

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

Il existe plusieurs formes différentes de la `Message.Obtain` (méthode). L’exemple précédent utilise le [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Comme il s’agit d’une requête asynchrone à un service out-of-process ; Il n’y aura aucune réponse du service, de sorte que la `Handler` est défini sur `null`. Le deuxième paramètre, `Int32 what`, seront stockées dans le `.What` propriété de la `Message` objet. Le `.What` propriété est utilisée par le code dans le processus du service à appeler des méthodes sur le service.

Le `Message` classe expose également deux propriétés supplémentaires qui peuvent être utiles pour le nom : `Arg1` et `Arg2`. Ces deux propriétés sont des valeurs entières qui peuvent avoir certaines spécial convenus des valeurs qui ont un sens entre le client et le service. Par exemple, `Arg1` peut contenir un ID de client et `Arg2` peut contenir un numéro de bon de commande pour ce client. Le [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) peut être utilisée pour définir les deux propriétés lorsque le `Message` est créé. Une autre façon de remplir ces deux valeurs consiste à définir le `.Arg` et `.Arg2` propriétés directement suite le `Message` de l’objet après qu’il a été créé.

### <a name="passing-additional-values-to-the-service"></a>Transmission de valeurs supplémentaires au Service

Il est possible de transmettre des données plus complexes au service en utilisant un `Bundle`. Dans ce cas, les valeurs supplémentaires peuvent être placés dans un `Bundle` et envoyé avec la `Message` en définissant le [ `.Data` propriété](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) avant d’envoyer.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> En règle générale, un `Message` ne doit pas avoir une charge utile supérieure à 1 Mo. La limite de taille peut varier selon la version d’Android, et toute modification de propriétaire, le fournisseur peut apportées à leur implémentation de l’Android Open Source Project (AOSP) qui est fourni avec l’appareil.

## <a name="returning-values-from-the-service"></a>Retourner des valeurs à partir du Service

L’architecture de messagerie qui a été évoquée à ce point est à sens unique, le client envoie un message au service. S’il est nécessaire pour le service retourner une valeur à un client tout ce qui a été évoquée à ce point est inversé. Le service doit créer un `Message`, au sein des valeurs de retour et la distribution du `Message` via un `Messenger` au client. Toutefois, le service ne crée pas sa propre `Messenger`; au lieu de cela, elle s’appuie sur le package et le client de l’instanciation d’un `Messenger` dans le cadre de la demande initiale. Le service sera `Send` le message à l’aide de ce fournies par le client `Messenger`.  

La séquence d’événements pour la communication bidirectionnelle est la suivante :

1. Le client est lié au service. Lorsque le service et le client se connectent, le `IServiceConnection` qui est géré par le client aura une référence à un `Messenger` objet qui est utilisé pour transmettre `Message`s pour le service. Pour éviter toute confusion, il est appelé le _Service Messenger_.
2. Client instancie un `Handler` (appelé le _Gestionnaire Client_) et l’utilise pour initialiser son propre `Messenger` (le _Client Messenger_). Notez que le Service Messenger et le Client Messenger sont deux objets différents qui gèrent le trafic dans les deux directions différentes. Le Service Messenger gère les messages à partir du client au service, tandis que le Client Messenger gérera les messages à partir du service au client.
3. Le client crée un `Message` objet et affecte le `ReplyTo` propriété avec le Client Messenger. Le message est ensuite envoyé au service à l’aide du Service Messenger.
4. Le service reçoit le message à partir du client et effectue le travail demandé.
5. Lorsqu’il est temps pour le service envoyer la réponse au client, il utilisera `Message.Obtain` pour créer un nouveau `Message` objet.
6. Pour envoyer ce message au client, le service va extraire le Messenger Client à partir de la `.ReplyTo` propriété du client du message et l’utiliser pour `.Send` le `Message` au client.
7. Lorsque la réponse est reçue par le client, il possède son propre `Handler` qui traitera le `Message` en inspectant le `.What` propriété (et si nécessaire, extraction de tous les paramètres contenus dans le `Message`).

Cet exemple de code montre comment le client instancie le `Message` et empaqueter un `Messenger` que le service doit utiliser pour sa réponse :

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

Le service doit apporter des modifications à son propre `Handler` pour extraire le `Messenger` et l’utiliser pour envoyer des réponses au client. Cet extrait de code est un exemple illustrant le service `Handler` créerait un `Message` et l’envoyer au client :  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

Notez que dans les exemples de code ci-dessus, le `Messenger` instance qui est créé par le client est *pas* le même objet qui est reçu par le service. Il s’agit de deux différentes `Messenger` objets en cours d’exécution dans deux processus distincts qui représentent le canal de communication.

## <a name="securing-the-service-with-android-permissions"></a>Sécurisation du Service avec des autorisations Android

Un service qui s’exécute dans un processus global est accessible par toutes les applications en cours d’exécution sur l’appareil Android. Dans certaines situations, cette transparence et la disponibilité n’est pas souhaitable, et il est nécessaire sécuriser le service contre tout accès à partir de clients non autorisés. Permet de limiter l’accès au service distant consiste à utiliser les autorisations Android.

Autorisations peuvent être identifiées par le `Permission` propriété de la `ServiceAttribute` qui décore le `Service` sous-classe. Cela nommera une autorisation qui le client doit être accordé lors de la liaison au service. Si le client ne dispose pas des autorisations appropriées, puis Android lèvera une `Java.Lang.SecurityException` lorsque le client essaie de lier au service.

Il existe quatre différents niveaux d’autorisation qui Android fournit :

* **normal** &ndash; c’est le niveau d’autorisation par défaut. Il est utilisé pour identifier les autorisations de faible risque qui peuvent être accordées automatiquement par Android pour les clients qui le demandent. L’utilisateur ne dispose pas explicitement accorder ces autorisations, mais les autorisations peuvent être affichées dans les paramètres d’application.
* **signature** &ndash; il s’agit d’une catégorie spéciale d’autorisation qui est automatiquement accordée par Android pour les applications qui sont signées avec le même certificat. Cette autorisation est conçue pour le rendre facilement pour un développeur d’applications de partager des composants ou des données entre leurs applications sans déranger l’utilisateur pour les approbations de constante.
* **signatureOrSystem** &ndash; cela est très similaire à la **signature** autorisations décrites ci-dessus. En plus d’être automatiquement accordée aux applications qui sont signées par le même certificat, cette autorisation sera également accordée aux applications qui sont signées le même certificat qui a été utilisé pour signer les applications installées avec l’image du système Android. Cette autorisation est généralement uniquement utilisée par les développeurs Android ROM pour permettre à leurs applications fonctionnent avec des applications tierces. Il n’est pas couramment utilisé par les applications qui sont destinées à la distribution générale pour le grand public.
* **dangereux** &ndash; autorisations dangereuses sont ceux qui peut entraîner des problèmes pour l’utilisateur. Pour cette raison, **dangereux** autorisations doivent être explicitement approuvées par l’utilisateur.

Étant donné que `signature` et `normal` autorisations sont automatiquement accordées au moment de l’installation par Android, il est essentiel que l’APK qui héberge le service soit installé **avant** l’APK contenant le client. Si le client est installé en premier, Android ne sera pas accorder les autorisations. Dans ce cas, il sera nécessaire désinstaller le client APK, installez le service APK, puis réinstaller le client APK.

Il existe deux méthodes courantes pour sécuriser un service avec des autorisations Android :

1.  **Implémenter la sécurité au niveau de signature** &ndash; sécurité au niveau de Signature signifie qu’autorisation est automatiquement accordée à ces applications sont signées avec la même clé a été utilisée pour signer l’APK contenant le service. Il s’agit d’un moyen simple pour les développeurs à sécuriser leur service tout en préservant les accessible à partir de leurs propres applications. Autorisations de niveau de signature sont déclarées en définissant le `Permission` propriété de la `ServiceAttribute` à `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Créer une autorisation personnalisée** &ndash; il est possible pour le développeur du service créer une autorisation personnalisée pour le service. Ceci est préférable lorsqu’un développeur veut partager leur service avec les applications à partir d’autres développeurs. Une autorisation personnalisée nécessite un peu plus d’efforts pour implémenter et sera abordée ci-dessous.

Un exemple simplifié de création personnalisé `normal` autorisation sera décrite dans la section suivante. Pour plus d’informations sur les autorisations Android, consultez la documentation de Google pour [: bonnes pratiques et sécurité](https://developer.android.com/training/articles/security-tips.html). Pour plus d’informations sur les autorisations Android, consultez le [section autorisations](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) de la documentation Android pour le manifeste d’application pour plus d’informations sur les autorisations Android.

> [!NOTE]
> En règle générale, [Google déconseille l’utilisation des autorisations personnalisées](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) comme ils peuvent s’avérer confus pour les utilisateurs.

### <a name="creating-a-custom-permission"></a>Création d’une autorisation personnalisée

Pour utiliser une autorisation personnalisée, il est déclaré par le service pendant que le client demande explicitement cette autorisation.

Pour créer une autorisation dans le service APK, un `permission` élément est ajouté à la `manifest` élément **AndroidManifest.xml**. Cette autorisation doit avoir le `name`, `protectionLevel`, et `label` jeu d’attributs. Le `name` attribut doit être défini sur une chaîne qui identifie de façon unique l’autorisation. Le nom s’affichera dans le **App Info** afficher de la **paramètres Android** (comme indiqué dans la section suivante).

Le `protectionLevel` attribut doit être défini à une des valeurs de chaîne de quatre qui ont été décrites ci-dessus.  Le `label` et `description` doit faire référence à des ressources de type chaîne et sont utilisés pour fournir un nom convivial et une description pour l’utilisateur.

Cet extrait de code est un exemple de déclaration personnalisée `permission` attribut **AndroidManifest.xml** de l’APK qui contient le service :

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

Ensuite, le **AndroidManifest.xml** du client APK doit demander explicitement cette nouvelle autorisation. Cela s’effectue en ajoutant le `users-permission` attribut le **AndroidManifest.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>Afficher les autorisations accordées à une application

Pour afficher les autorisations octroyées à une application, ouvrez l’application paramètres Android, puis sélectionnez **applications**. Recherchez et sélectionnez l’application dans la liste. À partir de la **App Info** écran, appuyez sur **autorisations** qui fera apparaître une vue qui affiche toutes les autorisations accordées à l’application :

[![Captures d’écran à partir d’un appareil Android montrant comment trouver les autorisations accordées à une application](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Récapitulatif

Ce guide a été une avancée discussion sur la façon d’exécuter un service Android dans un processus distant. Les différences entre une variable locale et un service distant a été expliqué, ainsi que certaines raisons pourquoi un service distant peut être utile de stabilité et de performances d’une application Android. Après avoir expliqué comment implémenter un service distant et la façon dont un client peut communiquer avec le service, le guide s’est passé afin de fournir un moyen pour limiter l’accès au service à partir des seuls les clients autorisés.


## <a name="related-links"></a>Liens associés

- [Gestionnaire](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Message](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [L’attribut exporté](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Les services avec un processus isolé et de la classe d’Application personnalisée pas résoudre correctement les surcharges](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Processus et Threads](https://developer.android.com/guide/components/processes-and-threads.html)
- [Manifeste Android - autorisations](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Conseils de sécurité](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
