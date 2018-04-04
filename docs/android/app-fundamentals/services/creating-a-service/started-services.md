---
title: Services démarrés avec Xamarin.Android
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: c0aeeaad3798dd840e69c6da6d7857298f4da3c1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="started-services-with-xamarinandroid"></a>Services démarrés avec Xamarin.Android

## <a name="started-services-overview"></a>Vue d’ensemble des Services démarrés

Services démarrés effectuent généralement une unité de travail sans fournir des commentaires ou les résultats au client. Un exemple d’une unité de travail est un service qui télécharge un fichier vers un serveur. Le client fait une demande à un service pour charger un fichier à partir de l’appareil à un site Web. Le service en mode silencieux télécharger le fichier (même si l’application n’a aucune activité au premier plan) et se terminer lorsque le téléchargement est terminé. Il est important de savoir qu’un service démarré s’exécutera sur le thread d’interface utilisateur d’une application. Cela signifie que si un service est d’exécuter le travail qui bloque le thread d’interface utilisateur, il doit créer et dispose de threads que nécessaire.

Contrairement à un service lié, il n’existe aucun canal de communication entre un service démarré « pur » et ses clients. Cela signifie qu’un service démarré implémentera certaines méthodes de cycle de vie différent à un service lié. La liste suivante présente les méthodes de cycle de vie courants dans un service démarré :

* `OnCreate` &ndash; Appelée une fois lorsque le service est démarré pour la première. Il s’agit où le code d’initialisation doit être implémenté.
* `OnBind` &ndash; Cette méthode doit être implémentée par toutes les classes de service, cependant, un service démarré généralement client n’est pas un qui lui est associée. Pour cette raison, un service démarré retourne simplement `null`. En revanche, un service hybride (qui est la combinaison d’un service lié et un service démarré) a implémenter et renvoyer un `Binder` pour le client.
* `OnStartCommand` &ndash; Appelé pour chaque demande de démarrage du service, soit en réponse à un appel à `StartService` ou un redémarrage par le système. Il s’agit où le service peut commencer à n’importe quelle tâche d’exécution longue. La méthode retourne un `StartCommandResult` valeur qui indique comment ou si le système doit gérer le redémarrage du service après un arrêt en raison d’une insuffisance de mémoire. Cet appel a lieu sur le thread principal. Cette méthode est décrite plus en détail ci-dessous.
* `OnDestroy` &ndash; Cette méthode est appelée lorsque le Service est détruit. Il est utilisé pour exécuter tout dernier nettoyage requis.

La méthode importante pour un service démarré est le `OnStartCommand` (méthode). Elle sera appelée chaque fois que le service reçoit une demande d’effectuer un travail. L’extrait de code suivant est un exemple de `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Le premier paramètre est un `Intent` objet contenant les métadonnées sur le travail à effectuer. Le deuxième paramètre contient un `StartCommandFlags` valeur qui fournit des informations sur l’appel de méthode. Ce paramètre a l’une des deux valeurs possibles :

* `StartCommandFlag.Redelivery` &ndash; Cela signifie que la `Intent` est une nouvelle date de livraison d’une précédente `Intent`. Cette valeur est fournie lorsque le service a retourné `StartCommandResult.RedeliverIntent` mais a été arrêtée avant qu’il peut être correctement arrêté.
* `StartCommandFlag.Retry` &dash; Cette valeur est reçue lorsqu’une précédente `OnStartCommand` Échec de l’appel et Android essaie de redémarrer le service avec l’intention de même que l’échec de la tentative précédente.
 
Enfin, le troisième paramètre est une valeur entière qui est unique à l’application qui identifie la demande. Il est possible que plusieurs appelants peuvent appeler le même objet de service. Cette valeur est utilisée pour associer une demande d’arrêt d’un service avec une demande donnée pour démarrer un service. Nous l’aborderons plus en détail dans la section [l’arrêt du Service](#Stopping_the_Service). 

La valeur `StartCommandResult` est retourné par le service comme une suggestion pour Android sur que faire si le service est arrêté en raison de contraintes de ressources. Il existe trois valeurs possibles pour `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; cette valeur indique Android qui n’est pas nécessaire de redémarrer le service qu’il a terminé. Comme exemple, imaginons un service qui génère des miniatures pour une galerie dans une application. Si le service est arrêté, il n’est pas essentiel pour recréer la miniature immédiatement &ndash; la miniature peut être recréée lors de la prochaine exécution de l’application.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; en déduire Android pour redémarrer le Service, mais ne pas pour remettre l’intention de dernier qui a démarré le Service. S’il n’y aucun intentions en attente à gérer, puis un `null` doivent être fournies pour le paramètre d’intention. Un exemple de ce peut être une application de lecteur de musique ; le service redémarrera prêt pour la musique, mais il sera lu la dernière chanson. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)**  &ndash; cette valeur est va indiquer Android pour redémarrer le service et de livrer à nouveau la dernière `Intent`. Ceci est un service qui télécharge un fichier de données pour une application. Si le service est arrêté, le fichier de données doit toujours être téléchargée. En retournant `StartCommandResult.RedeliverIntent`, lorsque Android redémarre le service fournit également l’intention (qui conserve l’URL du fichier à télécharger) au service. Cette opération activera le téléchargement redémarrer ou reprendre (en fonction de l’implémentation exacte du code).

Il existe une quatrième valeur `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Cette valeur est retournée par `OnStartCommand` et décrit comment Android continue le service qu’il a terminé. Cette valeur n’est pas généralement utilisée pour démarrer un service.

Les événements de son cycle de vie d’un service démarré sont répertoriées dans ce diagramme : 

![Un diagramme indiquant l’ordre dans lequel les méthodes de cycle de vie sont appelées](started-services-images/started-service-01.png "un diagramme indiquant l’ordre dans lequel les méthodes de cycle de vie sont appelées.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>L’arrêt du Service

Un service démarré continuent de s’exécuter indéfiniment ; Android conservera le service exécuté en tant qu’il existe suffisamment de ressources système. Le client doit arrêter le service, ou le service peut s’arrêter lorsqu’il a terminé son travail. Il existe deux façons d’arrêter un service : 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)**  &ndash; un client (par exemple, une activité) peut demander un service s’arrêter appelant le `StopService` méthode : 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; un service peut s’arrêter lui-même en appelant le `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>Pour arrêter un Service à l’aide de startId

Plusieurs appelants peuvent demander qu’un service est démarré. S’il existe une demande de démarrage en attente, le service peut utiliser le `startId` qui a été passée dans `OnStartCommand` pour empêcher le service s’est arrêté prématurément. Le `startId` correspond à l’appel de dernière `StartService`et est incrémentée chaque fois qu’elle est appelée. Par conséquent, si une demande suivante pour `StartService` n’a encore eu un appel à `OnStartCommand`, le service peut appeler `StopSelfResult`, en lui passant la valeur la plus récente de `startId` qu’il a reçu (au lieu d’appeler simplement `StopSelf`). Si un appel à `StartService` n’a encore eu un appel correspondant à `OnStartCommand`, le système ne sera pas arrêter le service, car le `startId` utilisé dans le `StopSelf` appel ne correspondront pas vers la dernière `StartService` appeler.


## <a name="related-links"></a>Liens associés

- [StartedServicesDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [Icônes de barre d’état](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
