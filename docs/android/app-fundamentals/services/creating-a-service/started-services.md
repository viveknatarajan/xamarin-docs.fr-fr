---
title: Services démarrés avec Xamarin.Android
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: df3d1bba57c1caf23c615410a184bc5458fc848b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013298"
---
# <a name="started-services-with-xamarinandroid"></a>Services démarrés avec Xamarin.Android

## <a name="started-services-overview"></a>Vue d’ensemble des Services de prise en main

Prise en main des services effectuent généralement une unité de travail sans fournir vos commentaires directs ou résultats au client. Un exemple d’une unité de travail est un service qui télécharge un fichier sur un serveur. Le client fait une demande à un service pour charger un fichier à partir de l’appareil à un site Web. Le service va charger discrètement le fichier (même si l’application n’a aucune activité au premier plan) et se terminer si le chargement est terminé. Il est important de savoir qu’un service démarré s’exécutera sur le thread d’interface utilisateur d’une application. Cela signifie que si un service consiste à effectuer un travail qui ne se bloque le thread d’interface utilisateur, il doit créer et supprimer des threads en fonction des besoins.

Contrairement à un service lié, il n’existe aucun canal de communication entre un service démarré « pur » et ses clients. Cela signifie qu’un service démarré implémente certaines méthodes de cycle de vie autre qu’un service dépendant. La liste suivante met en évidence les méthodes de cycle de vie commun dans un service démarré :

* `OnCreate` &ndash; Appelée une seule fois lorsque le service est démarré pour la première. Il s’agit où le code d’initialisation doit être implémenté.
* `OnBind` &ndash; Cette méthode doit être implémentée par toutes les classes de service, mais un service démarré ne dispose généralement pas un client lié à celui-ci. Pour cette raison, un service démarré retourne simplement `null`. En revanche, un service hybride (qui est la combinaison d’un service lié et un service démarré) doit implémenter et retourner un `Binder` pour le client.
* `OnStartCommand` &ndash; Appelée pour chaque demande de démarrage du service, soit en réponse à un appel à `StartService` ou un redémarrage par le système. Il s’agit où le service peut commencer à n’importe quelle tâche longue. La méthode retourne un `StartCommandResult` valeur qui indique comment ou si le système doit gérer le redémarrage du service après un arrêt en raison d’une insuffisance de mémoire. Cet appel a lieu sur le thread principal. Cette méthode est décrite plus en détail ci-dessous.
* `OnDestroy` &ndash; Cette méthode est appelée lorsque le Service est en cours de destruction. Il est utilisé pour effectuer le tout dernier nettoyage requis.

La méthode importante pour un service de prise en main est la `OnStartCommand` (méthode). Elle sera appelée chaque fois que le service reçoit une demande d’effectuer une tâche. L’extrait de code suivant est un exemple de `OnStartCommand`: 

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

* `StartCommandFlag.Redelivery` &ndash; Cela signifie que le `Intent` est une nouvelle date de livraison d’une précédente `Intent`. Cette valeur est fournie lorsque le service a retourné `StartCommandResult.RedeliverIntent` mais a été interrompue avant qu’il a pu être correctement arrêté.
* `StartCommandFlag.Retry` &dash; Cette valeur est reçue lorsqu’une précédente `OnStartCommand` appel a échoué et Android tente de redémarrer le service dans le même but en tant que l’échec de la tentative précédent.
 
Enfin, le troisième paramètre est une valeur entière qui est propre à l’application qui identifie la demande. Il est possible que plusieurs appelants peuvent appeler le même objet de service. Cette valeur est utilisée pour associer une demande d’arrêt d’un service avec une requête donnée pour démarrer un service. Il nous reviendrons plus en détail dans la section [l’arrêt du Service](#Stopping_the_Service). 

La valeur `StartCommandResult` est retourné par le service en tant qu’une suggestion pour Android sur que faire si le service est arrêtée en raison de contraintes de ressources. Il existe trois valeurs possibles pour `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; cette valeur indique à Android qui n’est pas nécessaire de redémarrer le service qu’il a terminé. Comme exemple, imaginez un service qui génère des miniatures pour une galerie dans une application. Si le service est arrêté, il n’est pas essentiel recréer la miniature immédiatement &ndash; la miniature peut être recréée la prochaine fois que l’application est exécutée.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; ce code indique à Android pour redémarrer le Service, mais ne pas à fournir l’intention de dernier qui a démarré le Service. S’il en existe aucun intentions en attente pour gérer, puis un `null` seront fournies pour le paramètre intention. Un exemple de ce peut être une application de lecteur de musique ; le service redémarrera prêt à jouer de la musique, mais elle jouera le dernier morceau. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)**  &ndash; cette valeur est va indiquer Android pour redémarrer le service et de livrer à nouveau le dernier `Intent`. Un exemple de ceci est un service qui télécharge un fichier de données pour une application. Si le service est arrêté, le fichier de données doit toujours être téléchargé. En retournant `StartCommandResult.RedeliverIntent`, lorsque Android redémarre le service il fournira également l’intention (qui conserve l’URL du fichier à télécharger) au service. Cela permet le téléchargement redémarrer ou reprendre (selon l’implémentation exacte du code).

Il existe une quatrième valeur pour `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Cette valeur est retournée par `OnStartCommand` et décrit comment Android continue du service qu’il a terminé. Cette valeur n’est pas généralement utilisée pour démarrer un service.

Les événements de cycle de vie d’un service de prise en main sont indiquées dans ce diagramme : 

![Un diagramme indiquant l’ordre dans lequel les méthodes de cycle de vie sont appelées](started-services-images/started-service-01.png "un diagramme indiquant l’ordre dans lequel les méthodes de cycle de vie sont appelées.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>L’arrêt du Service

Un service démarré continueront de s’exécuter indéfiniment ; Android conservera le service exécuté en tant qu’il existe suffisamment de ressources système. Le client doit arrêter le service, ou le service peut s’arrêter lui-même lorsqu’il a terminé son travail. Il existe deux façons de suspendre un service : 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)**  &ndash; un client (par exemple, une activité) peut demander un service s’arrêter appelant le `StopService` méthode : 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; un service peut s’arrêter lui-même en appelant le `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>À l’aide de startId pour arrêter un Service

Les appelants plusieurs peuvent demander qu’un service de démarrer. S’il existe une demande de démarrage en attente, le service peut utiliser le `startId` qui est passé dans `OnStartCommand` pour empêcher le service s’est arrêté prématurément. Le `startId` correspond à la dernière appel à `StartService`et est incrémentée chaque fois qu’elle est appelée. Par conséquent, si une demande ultérieure `StartService` n’a pas encore entraîné dans un appel à `OnStartCommand`, le service peut appeler `StopSelfResult`, en lui passant la valeur la plus récente de `startId` qu’il a reçu (au lieu d’appeler simplement `StopSelf`). Si un appel à `StartService` n’a pas encore conduit à un appel correspondant à `OnStartCommand`, le système ne sera pas arrêter le service, car le `startId` utilisé dans le `StopSelf` appel ne correspondra pas à la dernière version `StartService` appeler.


## <a name="related-links"></a>Liens associés

- [StartedServicesDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [Icônes de barre d’état](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
