---
title: Services en cours d’exécution Android dans les processus à distance
description: 'En règle générale, tous les composants dans une application Android seront exécute dans le même processus. Android services sont une exception notable : peut être configurés pour s’exécuter dans leur propre processus et partagés avec d’autres applications, y compris ceux d’autres développeurs Android. Ce guide explique comment créer et utiliser un service distant Android à l’aide de Xamarin.'
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 57be8187509ad7607c38ea17233e9ab5d25b41f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773658"
---
# <a name="running-android-services-in-remote-processes"></a>Services en cours d’exécution Android dans les processus à distance

_En règle générale, tous les composants dans une application Android seront exécute dans le même processus. Android services sont une exception notable : peut être configurés pour s’exécuter dans leur propre processus et partagés avec d’autres applications, y compris ceux d’autres développeurs Android. Ce guide explique comment créer et utiliser un service distant Android à l’aide de Xamarin._

## <a name="out-of-process-services-overview"></a>Hors de vue d’ensemble des Services de processus

Lorsqu’une application démarre, Android crée un processus dans lequel exécuter l’application. En règle générale, tous les composants de l’application s’exécutera dans ce processus une seule. Android services sont une exception notable : peut être configurés pour s’exécuter dans leur propre processus et partagés avec d’autres applications, y compris ceux d’autres développeurs Android. Ces types de services sont appelés _services à distance_ ou _out-of-process services_. Le code pour ces services se trouvera dans le même APK en tant que l’application principale ; Toutefois, lorsque le service est démarré Android créera un nouveau processus pour un seul service. En revanche, un service qui s’exécute dans le même processus que le reste de l’application est parfois appelée une _service local_.

En règle générale, il n’est pas nécessaire pour une application implémenter un service distant. Un service local est suffisant (et souhaitable) pour la configuration requise d’une application dans de nombreux cas. Un out-of-process a son propre espace mémoire qui doit être géré par Android. Bien que cela introduit une charge plus importante dans l’application globale, il existe certains scénarios où il peut être avantageux d’exécuter un service dans son propre processus :

1. **Fonctionnalité de partage** &ndash; certains développeurs d’applications peuvent avoir plusieurs applications et fonctionnalités qui sont partagée entre toutes les applications. Empaquetage de ces fonctionnalités dans un service Android qui s’exécute dans son propre processus peut simplifier la maintenance de l’application. Il est également possible pour le service dans son propre APK autonome du package et le déployer séparément du reste de l’application.
2. **Amélioration de l’expérience utilisateur** &ndash; deux méthodes sont possibles qu’un service d’out-of-process peut améliorer l’expérience utilisateur de l’application. La première méthode porte sur la gestion de la mémoire. Lorsqu’un garbage collection (GC) cycle se produit, Android suspend toute l’activité dans le processus jusqu'à ce que le garbage collector est terminé. L’utilisateur peut noter cette pause en tant que « ou » ou « jank ». Lorsqu’un service est en cours d’exécution des processus propres, c’est le processus de service est en pause, pas le processus d’application. Cette pause sera bien moins visible à l’utilisateur que le processus d’application (et par conséquent, l’interface utilisateur) ne sont pas suspendues.

    Ensuite, si les besoins en mémoire d’un processus devient trop volumineux, Android risque d’arrêter ce processus pour libérer des ressources pour le périphérique. Si un service n’a pas une grande quantité de mémoire et il est en cours d’exécution dans le même processus que l’interface utilisateur, puis quand Android récupère force les ressources de l’interface utilisateur va être arrêté, forçant l’utilisateur pour démarrer l’application. Toutefois, si un service en cours d’exécution dans son propre processus est arrêté par Android, le processus de l’interface utilisateur reste pas affecté. L’interface utilisateur peut lier (et redémarrer) le service, transparent pour l’utilisateur et reprendre un fonctionnement normal.

3. **Amélioration des performances de l’Application** &ndash; les processus de l’interface utilisateur peuvent être arrêté ou arrêt indépendant du processus de service. En déplaçant les tâches de longue durée de démarrage à un service d’out-of-process, il est possible que le temps de démarrage de l’interface utilisateur améliorée peut-être (en supposant que le processus du service est persistante entre les temps de lancée de l’interface utilisateur).

Dans bien des égards, la liaison à un service en cours d’exécution dans un autre processus est identique à [de liaison à un service local](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Le client appelle `BindService` pour lier (et démarrer, si nécessaire) le service. Un `Android.OS.IServiceConnection` objet sera créé pour gérer les connexions entre le client et le service. Si le client est correctement lié au service, puis Android retournera un objet via le `IServiceConnection` qui peut être utilisé pour appeler des méthodes sur le service. Ensuite, le client interagit avec le service à l’aide de cet objet. Pour passer en revue, voici les étapes pour lier à un service :

* **Créer une intention** &ndash; une intention explicite doit être utilisée pour la liaison au service.
* **Implémentez et instancier une `IServiceConnection` objet** &ndash; le `IServiceConnection` objet joue le rôle d’intermédiaire entre le client et le service.  Il est responsable de l’analyse de la connexion entre le client et le serveur.
* **Appeler le `BindService` méthode** &ndash; appel `BindService` distribue l’objectif et la connexion au service créé dans les étapes précédentes pour Android, qui s’occupe de démarrage du service et l’établissement de la communication entre le client et le service.

Le besoin de franchir les limites de processus présente une complexité supplémentaire : la communication est unidirectionnel (client et serveur) et le client ne peut pas appeler directement les méthodes sur la classe de service. Rappelez-vous que, lorsqu’un service s’exécute le même processus que le client, Android fournit un `IBinder` objet qui peut permettre de communication bidirectionnelle. Cela n’est pas le cas avec le service qui s’exécute dans son propre processus. Un client communique avec un service distant avec le l’aide de la `Android.OS.Messenger` classe.

Lorsqu’un client demande à lier avec le service distant, Android appellera la `Service.OnBind` méthode du cycle de vie, ce qui permet de renvoyer le texte interne `IBinder` objet encapsulé par le `Messenger`. Le `Messenger` est un wrapper mince sur une spéciale `IBinder` implémentation fournie par le Kit de développement logiciel Android. Le `Messenger` prend en charge la communication entre les deux processus différents. Le développeur est indifférente pour avec les détails de la sérialisation d’un message, le marshaling du message sur la limite de processus, puis de le désérialiser sur le client. Ce travail est géré par le `Messenger` objet. Ce diagramme illustre les composants côté client Android concernés lorsqu’un client établit la liaison à un service d’out-of-process :

![Diagramme illustrant les étapes et les composants pour une liaison à un service de client](out-of-process-services-images/ipc-01.png "diagramme illustrant les étapes et les composants pour une liaison à un service de client.")

La `Service` classe dans le processus distant parcourt les rappels du cycle de vie même parcourt un service lié dans un processus local, et la plupart des API impliqués sont identiques. `Service.OnCreate` est utilisé pour initialiser un `Handler` et injecter dans `Messenger` objet. De même, `OnBind` est substituée, mais au lieu de retourner un `IBinder` objet, le service renvoie le `Messenger`.  Ce diagramme illustre ce qui se passe dans le service lorsqu’un client est lié à celui-ci :

![Diagramme illustrant les étapes et les composants de service s’effectue liée par un client distant](out-of-process-services-images/ipc-02.png "diagramme qui illustre les étapes et les composants à un service s’effectue liée par un client distant.")

Lorsqu’un `Message` est reçu par un service, il est traité par dans l’instance de `Android.OS.Handler`. Le met en œuvre son propre `Handler` sous-classe doit remplacer le `HandleMessage` (méthode). Cette méthode est appelée par le `Messenger` et reçoit le `Message` en tant que paramètre. Le `Handler` inspecte le `Message` métadonnées et utiliser ces informations pour appeler des méthodes sur le service.

Communication unidirectionnelle se produit lorsqu’un client crée un `Message` de l’objet et l’envoie au service en utilisant le `Messenger.Send` (méthode). `Messenger.Send` Sérialise le `Message` et que les données sérialisées pour Android, qui achemine le message sur la limite de processus et le service.  Le `Messenger` qui est hébergé par le service créera un `Message` objet des données entrantes. Cela `Message` est placé dans une file d’attente, où les messages sont soumis à la fois pour le `Handler`. Le `Handler` inspecte les métadonnées contenues dans le `Message` et appeler les méthodes appropriées sur le `Service`. Le diagramme suivant illustre ces différents concepts en action :

![Diagramme illustrant la manière dont les messages sont transmis entre les processus](out-of-process-services-images/ipc-03.png "diagramme illustrant la manière dont les messages sont transmis entre les processus.")

Ce guide traite des détails d’implémentation d’un service d’out-of-process. Il explique comment implémenter un service qui est destiné à être exécuté dans son propre processus et comment un client peut communiquer avec ce service à l’aide de le `Messenger` framework. Il sera également Décrivez la communication bidirectionnelle : le client envoie un message à un service et le service envoie un message au client. Étant donné que les services peuvent être partagés entre différentes applications, ce guide explique également une technique permettant de limiter l’accès client au service à l’aide d’autorisations Android.

> [!IMPORTANT]
> [Bugzilla 51940 - échouent de Services avec un processus isolé et de la classe d’Application personnalisée résoudre correctement des surcharges de](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) rapports Xamarin.Android service ne démarre pas correctement lorsque le `IsolatedProcess` a la valeur `true`. Ce guide est fourni pour une référence. Une application de Xamarin.Android doit toujours être en mesure de communiquer avec un service d’out-of-process qui est écrit en langage Java.

## <a name="requirements"></a>Spécifications

Ce guide suppose que vous êtes familiarisé avec la création de services.

Bien qu’il soit possible d’utiliser les intentions implicites avec les applications qui ciblent ancienne API Android, ce guide se concentrera exclusivement sur l’utilisation de modes explicites. Applications ciblant Android 5.0 (niveau 21 d’API) ou une version ultérieure doivent utiliser d’une intention explicite à lier à un service ; Il s’agit de la méthode qui est présentée dans ce guide...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Créer un Service qui s’exécute dans un processus séparé

Comme décrit ci-dessus, le fait qu’un service s’exécute dans son propre processus signifie que certaines API différents est impliqués. Comme une vue d’ensemble rapide, voici les étapes pour lier avec et utiliser un service à distance :  

* **Créer le `Service` sous-classe** &ndash; sous-classe le `Service` de type et d’implémenter les méthodes de cycle de vie d’un service dépendant. Il est également nécessaire de définir des métadonnées qui contribueront Android le service doit s’exécuter dans son propre processus.
* **Implémentez un `Handler`**  &ndash; le `Handler` est responsable de l’analyse des demandes du client, l’extraction de tous les paramètres qui ont été passés à partir du client et appeler les méthodes appropriées sur le service.
* **Instancier une `Messenger`**  &ndash; comme décrit ci-dessus, chaque `Service` doit conserver une instance de la `Messenger` classe qui route les demandes des clients vers le `Handler` qui a été créé à l’étape précédente.

Un service qui est destiné à être exécuté dans son propre processus est fondamentalement toujours un service lié. La classe de service étend la base de `Service` classe et est décorée avec le `ServiceAttribute` contenant les métadonnées qui Android doit regrouper dans le manifeste Android. Pour commencer avec les propriétés suivantes de la `ServiceAttribute` qui sont importants pour un service d’out-of-process :

1. `Exported` &ndash; Cette propriété doit être définie `true` pour autoriser d’autres applications d’interagir avec le service. La valeur par défaut de cette propriété est `false`.
2. `Process` &ndash; Cette propriété doit être définie. Il est utilisé pour spécifier le nom du processus que le service s’exécutera dans.
3. `IsolatedProcess` &ndash; Cette propriété permet à une sécurité supplémentaire, indiquant Android pour exécuter le service dans un bac à sable isolé avec l’autorisation minimale iteract avec le reste du système. Consultez [Bugzilla 51940 - Services avec un processus isolé et de la classe d’Application personnalisée parviennent pas à résoudre correctement des surcharges](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; Il est possible de contrôler l’accès client au service en spécifiant une autorisation que les clients doivent demander (et accordées).

Pour exécuter un service de son propre processus, le `Process` propriété sur le `ServiceAttribute` doit être définie sur le nom du service. Pour interagir avec des applications externes, la `Exported` propriété doit être définie sur `true`. Si `Exported` est `false`, seuls les clients dans le même APK (autrement dit, la même application) et en cours d’exécution dans le même processus seront alors en mesure d’interagir avec le service.

Le type de processus que le service s’exécutera dans dépend de la valeur de la `Process` propriété. Android identifie trois différents types de processus :

-   **Les processus privés** &ndash; un processus privé est une tâche qui est uniquement disponible pour l’application qui l’a démarré. Pour identifier un processus comme privé, son nom doit commencer par une **:** (point-virgule). Le service illustré dans l’extrait de code précédent et la capture d’écran est un processus privé. L’extrait de code suivant est un exemple de la `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Processus global** &ndash; un service qui est exécuté dans un processus global est accessible à toutes les applications en cours d’exécution sur l’appareil. Un processus global doit être un nom de classe complet qui commence par une minuscule.
    (À moins que les mesures sont prises pour sécuriser le service, autres applications peuvent lier et interagir avec lui. Sécurisation du service contre toute utilisation non autorisée aborderons plus loin dans ce guide.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Isolé du processus** &ndash; un processus isolé est un processus qui s’exécute dans son propre bac à sable, isolé du reste du système et aucune autorisation spéciale qui lui sont propres. Pour exécuter un service dans un processus isolé, le `IsolatedProcess` propriété de la `ServiceAttribute` a la valeur `true` comme indiqué dans cet extrait de code :
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Consultez [Bugzilla 51940 - Services avec un processus isolé et de la classe d’Application personnalisée parviennent pas à résoudre correctement des surcharges](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Un service isolé est un moyen simple pour sécuriser une application et l’appareil contre le code non fiable. Par exemple, une application peut télécharger et exécuter un script à partir d’un site Web. Dans ce cas, effectuer cela dans un processus isolé fournit une couche supplémentaire de sécurité contre le code non fiable, compromettre l’appareil Android.

> [!IMPORTANT]
> Une fois qu’un service a été exporté, le nom du service ne devez pas modifier. La modification du nom du service peut-être interrompre les autres applications qui utilisent le service.

Pour voir l’effet que la `Process` a de propriété, la capture d’écran suivante montre un service qui s’exécute dans son propre processus privé :

![Capture d’écran qui illustre un service en cours d’exécution dans un processus privé](out-of-process-services-images/ipc-04.png "capture d’écran qui illustre un service en cours d’exécution dans un processus privé.")

Cette capture d’écran suivante montre `Process="com.xamarin.xample.messengerservice.timestampservice_process"` et le service en cours d’exécution dans un processus global :

![Capture d’écran d’un service en cours d’exécution dans un processus global](out-of-process-services-images/ipc-05.png "capture d’écran d’un service en cours d’exécution dans un processus global.")

Une fois la `ServiceAttribute` a été défini, le service doit implémenter un `Handler`.

### <a name="implementing-a-handler"></a>L’implémentation d’un gestionnaire

Pour traiter les demandes des clients, le service doit implémenter un `Handler` et remplacez le `HandleMessage` methodThis est la méthode prend un `Message` les qui encapsule l’appel de méthode à partir du client et traduit qui appellent des actions de l’instance ou tâche qui effectue le service. Le `Message` objet expose une propriété appelée `What` qui est une valeur entière, dont la signification est partagée entre le client et le service et est lié à une tâche que le service est à effectuer pour le client.

L’extrait de code suivant à partir de l’exemple d’application illustre un exemple de `HandleMessage`. Dans cet exemple, il existe deux actions qu’un client peut demander du service :

* La première action est un _Hello, World_ message, le client a envoyé un message simple pour le service.
* La seconde action s’appeler une méthode sur le service et extraire une chaîne, dans ce cas la chaîne est un message qui retourne heure à laquelle le service démarré et la durée pendant laquelle qu'il a été exécuté :

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

Il est également possible de paramètres de package pour le service dans le `Message`. Ce sujet est abordé plus loin dans ce guide. La création de la rubrique suivante à prendre en compte la `Messenger` objet entrant `Message`s.

### <a name="instantiating-the-messenger"></a>L’instanciation Messenger

Comme indiqué précédemment, la désérialisation du `Message` objet et en appelant `Handler.HandleMessage` est la gestion de la `Messenger` objet. Le `Messenger` classe fournit également une `IBinder` que le client utilisera pour envoyer des messages au service de l’objet.  

Lorsque le service démarre, il instancie le `Messenger` et injecter la `Handler`. Un bon emplacement pour effectuer cette initialisation est sur le `OnCreate` méthode du service. Cet extrait de code est un exemple d’un service qui initialise ses propres `Handler` et `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

À ce stade, l’étape finale consiste pour le `Service` à substituer `OnBind`.

### <a name="implementing-serviceonbind"></a>Implémentation de Service.OnBind

Tous les services liés, si elles s’exécuter dans leur propre processus ou non, doivent implémenter le `OnBind` (méthode). La valeur de retour de cette méthode est un objet que le client peut utiliser pour interagir avec le service. Exactement ce que cet objet varie en fonction si le service est un service local ou distant. Un service local renvoient des personnalisé `IBinder` implémentation, un service distant retourne le `IBinder` qui est encapsulé mais la `Messenger` qui a été créé dans la section précédente :

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Une fois ces trois étapes sont accomplies, le service distant peut être considéré comme terminé.

## <a name="consuming-the-service"></a>Utilisation du Service

Tous les clients doivent implémenter du code pour être en mesure de créer une liaison et de consommer le service distant. Point de vue conceptuel, à partir du point de vue du client, il existe très peu de différences entre la liaison de service local ou distant. Le client appelle le `BindService` méthode, en passant une intention explicite pour identifier le service et un `IServiceConnection` permettant de gérer la connexion entre le client et le service.

Cet extrait de code est un exemple montrant comment créer un **intention explicite** pour la liaison à un service distant. L’objectif doit identifier le package qui contient le service et le nom du service. Manière de configurer ces informations consiste à utiliser un `Android.Content.ComponentName` objet et qui fournissent à l’intention. Cet extrait de code est un exemple :  

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

Lorsque le service est lié, le `IServiceConnection.OnServiceConnected` méthode est appelée et fournit un `IBinder` à un client. Toutefois, le client n’est pas utiliser directement le `IBinder`. Au lieu de cela, il instancie un `Messenger` objet à partir de ce `IBinder`. Il s’agit de la `Messenger` que le client utilisera pour interagir avec le service distant.

Voici un exemple de très basique `IServiceConnection` implémentation qui montre comment un client serait gérer la connexion et déconnexion d’un service. Notez que la `OnServiceConnected` méthode reçoit et `IBinder`, et le client crée un `Messenger` à partir de ce `IBinder`:

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

Une fois la connexion au service et l’intention sont créés, il est possible pour le client appeler `BindService` et lancer le processus de liaison :

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Une fois que le client a correctement lié au service et la `Messenger` est disponible, il est possible pour le client envoie `Messages` au service.

## <a name="sending-messages-to-the-service"></a>Envoi de Messages au Service

Une fois que le client est connecté et a un `Messenger` de l’objet, il est possible de communiquer avec le service à la distribution `Message` objets la `Messenger`. Cette communication est à sens unique, le client envoie le message, mais aucun message de retour à partir du service au client. À cet égard, le `Message` est un mécanisme d’incendie initiale.

La meilleure façon de créer un `Message` objet consiste à utiliser le [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) méthode de fabrique. Cette méthode extrait un `Message` objet à partir d’un pool global qui est géré par Android. `Message.Obtain` également a certaines des méthodes surchargées qui permettent la `Message` objet à être initialisée avec les valeurs et les paramètres requis par le service.  Une fois la `Message` est instancié, il distribués au service en appelant `Messenger.Send`. Cet extrait de code est un exemple de création et la distribution un `Message` au processus de service :

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

Il existe plusieurs formes différentes de la `Message.Obtain` (méthode). L’exemple précédent utilise la [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Comme il s’agit d’une demande asynchrone pour un service d’out-of-process. Il n’y aura aucune réponse du service, donc la `Handler` a la valeur `null`. Le deuxième paramètre, `Int32 what`, sont stockés dans le `.What` propriété de la `Message` objet. Le `.What` propriété est utilisée par le code dans le processus du service à appeler des méthodes sur le service.

Le `Message` classe expose également deux propriétés supplémentaires qui peuvent être utiles au destinataire comportera un : `Arg1` et `Arg2`. Ces deux propriétés sont des valeurs entières qui peuvent avoir certaines spéciaux convenu de valeurs qui ont un sens entre le client et le service. Par exemple, `Arg1` peut contenir un ID de client et `Arg2` peut contenir un numéro de bon de commande pour ce client. Le [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) peut être utilisé pour définir les deux propriétés lorsque le `Message` est créé. Une autre méthode pour remplir ces deux valeurs consiste à définir le `.Arg` et `.Arg2` propriétés directement sur le `Message` de l’objet une fois qu’elle a été créée.

### <a name="passing-additional-values-to-the-service"></a>Transmission de valeurs supplémentaires au Service

Il est possible de passer des données plus complexes pour le service en utilisant un `Bundle`. Dans ce cas, les valeurs supplémentaires peuvent être placés dans un `Bundle` et envoyé avec les `Message` en définissant le [ `.Data` propriété](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) avant d’envoyer.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> En règle générale, un `Message` ne doit pas avoir une charge supérieure à 1 Mo. La limite de taille peut varier selon la version d’Android et sur toutes les modifications de propriétaires, le fournisseur peut apportées à leur implémentation de l’Android Open Source Project (AOSP) qui est fourni avec l’appareil.

## <a name="returning-values-from-the-service"></a>Retour de valeurs à partir du Service

L’architecture de messagerie qui a été traitée à ce point est à sens unique, le client envoie un message au service. S’il est nécessaire pour le service retourner une valeur à un client tout ce qui a été traitée à ce point est inversé. Le service doit créer un `Message`, au sein des valeurs de retour et la distribution du `Message` via un `Messenger` au client. Toutefois, le service ne crée pas sa propre `Messenger`; au lieu de cela, elle s’appuie sur le package et le client de l’instanciation d’un `Messenger` dans le cadre de la demande initiale. Le service sera `Send` le message à l’aide de ce fournies par le client `Messenger`.  

La séquence d’événements pour la communication bidirectionnelle est la suivante :

1. Le client est lié au service. Quand le service et le client se connectent, le `IServiceConnection` qui est géré par le client dispose d’une référence à un `Messenger` objet qui est utilisé pour transmettre `Message`s pour le service. Pour éviter toute confusion, il est appelé le _Service Messenger_.
2. Client instancie un `Handler` (appelé le _Client gestionnaire_) et l’utilise pour initialiser ses propres `Messenger` (le _Client Messenger_). Notez que le Service Messenger et le Client Messenger sont deux objets différents qui gèrent le trafic dans deux directions opposées. Le Service Messenger gère des messages à partir du client au service, tandis que le Client Messenger gérera les messages à partir du service au client.
3. Le client crée un `Message` objet et définit le `ReplyTo` propriété avec le Client Messenger. Le message est ensuite envoyé au service à l’aide du Service Messenger.
4. Le service reçoit le message à partir du client et effectue le travail demandé.
5. Lorsqu’il est temps pour le service envoie la réponse au client, il utilisera `Message.Obtain` pour créer un nouveau `Message` objet.
6. Pour envoyer ce message au client, le service extrait le Messenger Client à partir de la `.ReplyTo` propriété du client de message et l’utiliser pour `.Send` le `Message` au client.
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

Le service doit apporter des modifications à son propre `Handler` pour extraire le `Messenger` et l’utiliser pour envoyer des réponses au client. Cet extrait de code est un exemple de procédure du service `Handler` créerait un `Message` et l’envoyer au client :  

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

Notez que dans les exemples de code ci-dessus, le `Messenger` instance qui est créé par le client est *pas* le même objet qui est reçu par le service. Voici deux `Messenger` objets en cours d’exécution dans deux processus distincts qui représentent le canal de communication.

## <a name="securing-the-service-with-android-permissions"></a>Sécurisation du Service avec des autorisations Android

Un service qui s’exécute dans un processus global est accessible par toutes les applications en cours d’exécution sur l’appareil Android. Dans certains cas, cette ouverture et la disponibilité n’est pas souhaitable, et il est nécessaire sécuriser le service contre les accès à partir de clients non autorisés. Une façon de limiter l’accès au service distant consiste à utiliser des autorisations Android.

Les autorisations peuvent être identifiées par le `Permission` propriété de la `ServiceAttribute` qui décore le `Service` sous-classe. Cela sera nommer une autorisation qui doit être accordé le client lors de la liaison au service. Si le client ne dispose pas des autorisations appropriées, puis Android lèvera une `Java.Lang.SecurityException` lorsque le client essaie de lier au service.

Il existe quatre différents niveaux d’autorisation qui Android fournit :

* **normal** &ndash; c’est le niveau d’autorisation par défaut. Il est utilisé pour identifier les autorisations de faible risque qui peuvent être accordées automatiquement par Android aux clients qui demandent. L’utilisateur ne dispose pas explicitement accorder ces autorisations, mais les autorisations peuvent être affichées dans les paramètres d’application.
* **signature** &ndash; il s’agit d’une catégorie spéciale d’autorisations seront automatiquement accordées par Android pour les applications qui sont signées avec le même certificat. Cette autorisation est conçue pour le rendre facilement pour les développeurs d’applications partager des données entre leurs applications sans déranger l’utilisateur pour les approbations de constantes ou composants.
* **signatureOrSystem** &ndash; cela est très similaire à la **signature** autorisations décrites ci-dessus. En plus d’être automatiquement accordée aux applications qui sont signées par le même certificat, cette autorisation sera également accordée aux applications qui sont signées le même certificat qui a été utilisé pour signer les applications installées avec l’image du système Android. Cette autorisation est généralement uniquement utilisée par les développeurs Android ROM pour permettre à leurs applications fonctionnent avec des applications tierces. Il n’est pas couramment utilisé par les applications qui sont destinées à la distribution générale pour le grand public.
* **dangereuses** &ndash; autorisations dangereuses sont celles qui peuvent provoquer des problèmes pour l’utilisateur. Pour cette raison, **dangereuses** autorisations doivent être approuvées par l’utilisateur.

Étant donné que `signature` et `normal` autorisations sont automatiquement accordées au moment de l’installation par Android, il est essentiel que APK qui héberge le service installé **avant** APK contenant le client. Si le client est installé en premier, Android ne sera pas accorder les autorisations. Dans ce cas, il sera nécessaire désinstaller le client APK, installez le service APK, puis réinstallez le client APK.

Il existe deux méthodes courantes pour sécuriser un service avec des autorisations Android :

1.  **Implémenter la sécurité au niveau de signature** &ndash; sécurité au niveau de Signature signifie qu’autorisation est automatiquement accordée aux applications qui sont signées avec la même clé que celle utilisée pour signer l’APK contenant le service. Il s’agit d’un moyen simple pour les développeurs à sécuriser leur service tout en préservant les accessible à partir de leurs propres applications. Autorisations de niveau de signature sont déclarées en définissant le `Permission` propriété de la `ServiceAttribute` à `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Créer une autorisation personnalisée** &ndash; qu’il est possible que le développeur du service créer une autorisation personnalisée pour le service. Il est préférable lorsqu’un développeur veut partager leur service avec des applications à partir d’autres développeurs. Une autorisation personnalisée requiert un peu plus d’effort pour implémenter et seront abordés ci-dessous.

Un exemple simplifié de création personnalisé `normal` autorisation sera décrite dans la section suivante. Pour plus d’informations sur les autorisations Android, consultez la documentation de Google pour [meilleures pratiques et sécurité](https://developer.android.com/training/articles/security-tips.html). Pour plus d’informations sur les autorisations Android, consultez le [section autorisations](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) de la documentation d’Android pour le manifeste d’application pour plus d’informations sur les autorisations Android.

> [!NOTE]
> En général, [Google déconseille l’utilisation d’autorisations personnalisées](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) comme ils peuvent s’avérer déconcertant pour les utilisateurs.

### <a name="creating-a-custom-permission"></a>Création d’une autorisation personnalisée

Pour utiliser une autorisation personnalisée, il est déclaré par le service pendant que le client demande explicitement cette autorisation.

Pour créer une autorisation dans le service APK, un `permission` élément est ajouté à la `manifest` élément **AndroidManifest.xml**. Cette autorisation doit avoir le `name`, `protectionLevel`, et `label` jeu d’attributs. Le `name` attribut doit être défini sur une chaîne qui identifie de façon unique l’autorisation. Le nom s’affichera dans le **App Info** afficher de la **paramètres Android** (comme indiqué dans la section suivante).

Le `protectionLevel` attribut doit être défini à une des valeurs de chaîne de quatre qui ont été décrites ci-dessus.  Le `label` et `description` doit faire référence à des ressources de type chaîne et sont utilisés pour fournir un nom convivial et une description pour l’utilisateur.

Cet extrait de code est un exemple de déclaration personnalisé `permission` attribut **AndroidManifest.xml** de APK qui contient le service :

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

Ensuite, la **AndroidManifest.xml** du client APK doit demander explicitement cette nouvelle autorisation. Cela est fait en ajoutant le `users-permission` d’attribut pour le **AndroidManifest.xml**:

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

Pour afficher les autorisations octroyées à une application, ouvrez l’application paramètres Android, puis sélectionnez **applications**. Recherchez et sélectionnez l’application dans la liste. À partir de la **App Info** de l’écran, appuyez sur **autorisations** qui s’affiche une vue qui affiche toutes les autorisations accordées à l’application :

[![Captures d’écran à partir d’un appareil Android montrant comment trouver les autorisations accordées à une application](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Récapitulatif

Ce guide a été une discussion avancée sur l’exécution d’un service Android dans un processus distant. Les différences entre une variable locale et un service distant a été expliqué, ainsi que certaines raisons pour lesquelles un service distant peut être utile de stabilité et de performances d’une application Android. Après expliquant comment implémenter un service distant et comment un client peut communiquer avec le service, le guide avant de fournir un moyen pour limiter l’accès au service à partir de clients autorisés uniquement.


## <a name="related-links"></a>Liens associés

- [Gestionnaire](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Message](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [L’attribut exporté](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Services avec un processus isolé et de la classe d’Application personnalisée parviennent pas à résoudre correctement des surcharges](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Processus et Threads](https://developer.android.com/guide/components/processes-and-threads.html)
- [Manifeste Android - autorisations](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Conseils de sécurité](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
