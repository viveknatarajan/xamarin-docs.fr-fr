---
title: Limite des Services dans Xamarin.Android
description: Services liés sont Android qui fournissent une interface client-serveur interagissant avec un client (par exemple, une activité Android). Ce guide explique les principaux composants impliqués dans la création d’un service lié et comment l’utiliser dans une application Xamarin.Android.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 1cb151cc5c741a020fcbb398441ed4958ec5980b
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="bound-services-in-xamarinandroid"></a>Limite des Services dans Xamarin.Android

_Services liés sont Android qui fournissent une interface client-serveur interagissant avec un client (par exemple, une activité Android). Ce guide explique les principaux composants impliqués dans la création d’un service lié et comment l’utiliser dans une application Xamarin.Android._

## <a name="bound-services-overview"></a>Vue d’ensemble des Services liés

Les services qui fournissent une interface client-serveur pour les clients peuvent interagir directement avec le service sont désignés en tant que _lié services_.  Il peut y avoir plusieurs clients connectés à une seule instance d’un service en même temps. Le service lié et le client sont isolés les uns des autres. Au lieu de cela, Android fournit une série d’objets intermédiaires qui gèrent l’état de la connexion entre les deux. Cet état est géré par un objet qui implémente le [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) interface.  Cet objet est créé par le client et passé en tant que paramètre à la [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) (méthode). Le `BindService` est disponible sur n’importe quel [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) objet (tel qu’une activité). Il s’agit d’une demande au système d’exploitation Android pour démarrer le service et le lier un client. Il existe trois façons d’un client peut lier à un service à l’aide de la `BindService` méthode :

* **Un classeur de service** &ndash; un classeur de service est une classe qui implémente le [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) interface. La plupart des applications n’est pas implémenter cette interface directement, au lieu de cela, elles s’étendent la [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) classe. Ceci est l’approche la plus courante et convient lorsque le service et le client existent dans le même processus.
* **À l’aide d’un Messenger** &ndash; cette technique est adaptée aux lorsque le service peut exister dans un processus séparé. Au lieu de cela, les demandes de service sont marshalés entre le client et le service via un [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger). Un [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) est créé dans le service qui gère la `Messenger` demandes. Ce point sera abordé dans un autre guide.
* **À l’aide de AIDL** &ndash; réponde à la fois au cœur de la plupart des développeurs Android %terreur lors de le, il ne sera pas couvert dans ce guide.

Une fois qu’un client a été lié à un service, la communication entre les deux est se produit `Android.OS.IBinder` objet.  Cet objet est responsable de l’interface qui permet au client d’interagir avec le service. Il n’est pas nécessaire pour chaque application Xamarin.Android implémenter cette interface à partir de zéro, le SDK Android fournit le [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) classe qui prend en charge la plupart du code requis avec le marshaling de l’objet entre le client et le service.

Lorsqu’un client est effectué avec le service, il doit supprimer la liaison à partir de celle-ci en appelant le `UnbindService` (méthode). Une fois que le dernier client a indépendante à partir d’un service, Android arrête et dispose d’un service lié.

Ce diagramme illustre comment l’activité, connexion de service, liant et service liés entre eux :

![Un diagramme montrant comment les composants de service sont liés entre eux](bound-services-images/bound-services-02.png "un diagramme montrant comment les composants de service sont liés entre eux.")

Ce guide explique comment étendre le `Service` classe pour implémenter un service dépendant. Il explique également implémenter `IServiceConnection` et l’extension `Binder` pour permettre à un client communiquer avec le service. Un exemple d’application de ce guide, qui contiennent une solution avec un seul projet Xamarin.Android appelé **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Il s’agit d’une application très simple qui montre comment implémenter un service et comment lier une activité. Le service lié a une API très simple avec une seule méthode, `GetFormattedTimestamp`, qui retourne une chaîne qui indique à l’utilisateur lorsque le service a démarré et la durée pendant laquelle il a été exécuté. L’application permet également à l’utilisateur d’annuler la liaison et la liaison au service manuellement.

[![Capture d’écran de l’application en cours d’exécution sur un téléphone Android](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Mise en œuvre et l’utilisation d’un Service dépendant

Il existe trois composants qui doivent être implémentées dans les applications Android consommer un service lié :

1. **Étendre la `Service` classe et implémenter les méthodes de rappel du cycle de vie** &ndash; cette classe contiendra le code qui effectue le travail qui vous est demandé du service. Ce point sera abordé plus en détail ci-dessous.
2. **Créez une classe qui implémente `IServiceConnection`**  &ndash; cet objet contient des méthodes de rappel qu’avertir le client est connecté à (ou a perdu la connexion) à partir du service. La connexion au service fournit également une référence à un objet que le client peut utiliser pour interagir directement avec le service. Cette référence est appelée le _binder_.
3. **Créez une classe qui implémente `IBinder`**  &ndash; A _Binder_ implémentation fournit l’API qu’un client utilise pour communiquer avec le service. Le classeur peut fournir une référence à un service lié, autorisant des méthodes à appeler directement ou le classeur peut fournir une API qui encapsule et masque le service lié à partir de l’application cliente. Un `IBinder` doit fournir le code nécessaire pour les appels de procédure distante. Il n’est pas nécessaire (ou recommandé) pour implémenter le `IBinder` interface directement. Un `IBinder` Instead applications doivent étendre la `Binder` qui fournit la plupart des fonctionnalités de base requises par une `IBinder`.
4. **Démarrage et la lier à un Service** &ndash; une fois la connexion au service, le classeur et le service créés l’application Android est chargée de démarrage du service et de lui.

Chacune de ces étapes sera abordée dans les sections suivantes plus en détail.

### <a name="extend-the-service-class"></a>Étendre la `Service` classe

Pour créer un service à l’aide de Xamarin.Android, il est nécessaire de sous-classe `Service` et orner le la classe avec le [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute). L’attribut est utilisé par les outils de génération de Xamarin.Android pour enregistrer correctement le service dans l’application **AndroidManifest.xml** de fichiers de façon très similaire à une activité, un service lié possède son propre cycle de vie et le rappel de méthodes associées à la événements importants dans son cycle de vie. La liste suivante est un exemple de certaines des méthodes de rappel plus courantes qui implémente un service :

* `OnCreate` &ndash; Cette méthode est appelée par Android comme il est également l’instanciation du service. Il est utilisé pour initialiser des variables ou des objets qui sont requis par le service pendant sa durée de vie. Cette méthode est facultative.
* `OnBind` &ndash; Cette méthode doit être implémentée par tous les services liés. Elle est appelée lorsque le premier client tente de se connecter au service. Il retourne une instance de `IBinder` afin que le client peut interagir avec le service. Tant que le service est en cours d’exécution, le `IBinder` objet sera utilisé pour répondre à toutes les futures demandes du client à lier au service.
* `OnUnbind` &ndash; Cette méthode est appelée lorsque tous les clients liés ont indépendant. En retournant `true` à partir de cette méthode, le service appelle ultérieurement `OnRebind` avec l’intention de passé à `OnUnbind` lorsque les nouveaux clients s’y lier. Vous faites cela quand un service continue l’exécution une fois qu’elle a été détachée. Cela se produit si le service récemment indépendant ont également un service démarré, et `StopService` ou `StopSelf` n’avait pas été appelée. Dans ce scénario, `OnRebind` permet à l’intention de le récupérer. La valeur par défaut retourne `false` , qui ne fait rien. Facultatif.
* `OnDestroy` &ndash; Cette méthode est appelée lorsque Android est détruire le service. Tout nettoyage nécessaire, telles que la libération des ressources, doit être effectuée dans cette méthode. Facultatif.

Les événements de son cycle de vie d’un service lié sont affichés dans ce diagramme :

![Un diagramme indiquant l’ordre dans lequel les méthodes de cycle de vie sont appelées](bound-services-images/bound-services-01.png "un diagramme indiquant l’ordre dans lequel les méthodes de cycle de vie sont appelées.")

L’extrait de code suivant à partir de l’application d’accompagnement qui accompagne ce guide, montre comment implémenter un service dépendant dans Xamarin.Android :

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

Dans l’exemple, le `OnCreate` méthode initialise un objet qui contient la logique de la récupération et la mise en forme d’un horodatage qui est demandé par un client. Quand le premier client essaie de lier au service, Android appellent le `OnBind` (méthode). Ce service instancie un `TimestampServiceBinder` objet qui permet aux clients d’accéder à cette instance du service en cours d’exécution. La `TimestampServiceBinder` classe est décrite dans la section suivante.

### <a name="implementing-ibinder"></a>Implémentation de IBinder

Comme mentionné, un `IBinder` objet fournit le canal de communication entre un client et un service. Les applications Android ne doivent pas implémenter le `IBinder` interface, le [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) doit être étendu. La `Binder` classe fournit la majeure partie de l’infrastructure nécessaire qui est nécessaire marshaler l’objet de classeur à partir du service (qui peut être exécuté dans un processus séparé) au client. Dans la plupart des cas, le `Binder` sous-classe uniquement quelques lignes de code et encapsule une référence au service. Dans cet exemple, `TimestampBinder` a une propriété qui expose le `TimestampService` au client :

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

Cela `Binder` rend possible d’appeler les méthodes publiques sur le service ; par exemple :

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Une fois `Binder` a été étendu, il est nécessaire d’implémenter `IServiceConnection` pour interconnecter tous les éléments.

### <a name="creating-the-service-connection"></a>Création de la connexion de Service

Le `IServiceConnection` présentera | introduire | exposer | connecter le `Binder` objet au client. En plus d’implémenter la `IServiceConnection` interface, la classe doit étendre `Java.Lang.Object`. La connexion du service doit également fournir une façon qui le client peut accéder à la `Binder` (et donc communiquer avec le service lié).

Ce code provient de l’accompagnant exemple de projet est une méthode d’implémenter `IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

Appelle dans le cadre du processus de liaison, Android le `OnServiceConnected` (méthode), en fournissant le `name` du service qui est lié et le `binder` qui contient une référence au service lui-même. Dans cet exemple, la connexion du service a deux propriétés, qui contient une référence pour le classeur et un indicateur booléen pour si le client est connecté au service ou non.

Le `OnServiceDisconnected` méthode est appelée uniquement lorsque la connexion entre un client et un service est perdue ou interrompue de façon inattendue. Cette méthode permet au client une chance de répondre à l’interruption de service.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Démarrage et la lier à un Service avec l’intention explicite

Pour utiliser un service lié, un client (par exemple, une activité) doit instancier un objet qui implémente `Android.Content.IServiceConnection` et appeler le `BindService` (méthode).` BindService` retournera `true` si le service est lié, `false` si elle n’est pas. La méthode `BindService` accepte trois paramètres :

* **Un `Intent`**  &ndash; l’intention doit identifier de manière explicite le service pour se connecter à.
* **Un `IServiceConnection` objet** &ndash; cet objet est un intermédiaire qui fournit des méthodes de rappel pour notifier le client lorsque le service lié est démarré et arrêté.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) enum** &ndash; ce paramètre est un ensemble d’indicateurs sont utilisées par le système pour la liaison lorsque de l’objet. La valeur la plus couramment utilisée est [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), ce qui démarre automatiquement le service s’il n’est pas déjà en cours d’exécution.

L’extrait de Code suivant est un exemple montrant comment démarrer un service lié dans une activité à l’aide d’une intention explicite :

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> En commençant avec Android 5.0 (niveau 21 d’API), il n’est possible de lier à un service avec l’intention explicite.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Note d’architecture sur la connexion au Service et le classeur.

Certains puristes OOP peuvent désapprouver l’implémentation précédente de la `TimestampBinder` classe, comme il s’agit d’une violation de la [loi de Déméter](https://en.wikipedia.org/wiki/Law_of_Demeter) qui, dans sa forme la plus simple indique « ne parlez pas aux inconnus ; communiquer uniquement avec vos amis ». Cette implémentation particulière expose le concrète `TimestampService` classe à tous les clients.

Parler, il n’est pas nécessaire pour le client connaître le `TimestampService` et l’exposition de cette classe concrète pour les clients peut rendre une application plus fragile et plus difficile à gérer pendant sa durée de vie. Une autre approche consiste à utiliser une interface qui expose le `GetFormattedTimestamp()` (méthode) et les appels de proxy au service via la `Binder` (ou possible de la classe de connexion de service) :  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

Cet exemple particulier autorisent une activité appeler les méthodes du service :

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>Liens associés

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
