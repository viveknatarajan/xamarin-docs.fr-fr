---
title: La mise à jour d’une application Xamarin.iOS en arrière-plan
description: Ce document décrit différentes façons de mettre à jour une application Xamarin.iOS qui se trouve dans l’arrière-plan, telles que la surveillance de la région, récupération en arrière-plan et des notifications à distance.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 835dccaea79467582f56fd4b8b6b3b8f42acd632
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392325"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>La mise à jour d’une application Xamarin.iOS en arrière-plan

Actualisation en arrière-plan est le processus de mise en éveil une application est suspendue ou ne pas en cours d’exécution et sa mise à jour avec le nouveau contenu. iOS fournit trois options pour l’actualisation du contenu en arrière-plan :

1.  *Surveillance de la région* et *Service de modifications significatives emplacement* -arrière-plan de déclencheur de géolocalisation APIs met à jour en fonction des changements dans l’emplacement de l’utilisateur. Ces API peuvent être utilisées avec précaution pour actualiser le contenu dans les applications non basée sur un emplacement iOS 6, où les autres options ne sont pas disponibles.
1.  *En arrière-plan Fetch (iOS 7 +)* -une approche temporelle pour l’actualisation *non critiques* contenu met à jour *fréquemment* .
1.  *Notifications à distance (iOS 7 +)* -Applications qui reçoivent des notifications push peuvent utiliser les notifications pour déclencher des actualisations de contenu en arrière-plan. Cette méthode peut être utilisée pour mettre à jour avec *temps important,* contenu met à jour *sporadique* .


Les sections suivantes abordent les principes fondamentaux de ces options.

## <a name="region-monitoring-and-significant-location-changes"></a>Surveillance de la région et les changements d’emplacement importantes

iOS fournit deux API de géolocalisation avec backgrounding fonctionnalités :

1.  *Surveillance de la région* est le processus de paramétrage de régions avec des limites et la mise en éveil de l’appareil lorsque l’utilisateur entre ou quitte une région. Régions sont circulaires et peuvent être de taille variable. Quand l’utilisateur franchit une limite de région, l’appareil sera sortir de veille pour gérer l’événement, généralement par le déclenchement d’une notification ou lancer une tâche. Surveillance de la région nécessite le GPS et augmente la batterie et l’utilisation des données.
1.  Le *Service de modifications significatives emplacement* est une option plus simple et la conservation d’alimentation disponible pour les appareils avec des signaux radio de téléphonie mobile. Une application à écouter les changements d’emplacement significatif est notifiée lorsque l’appareil bascule tours de cellule. Ce service peut être utilisé pour l’éveil par une application suspendue ou arrêtée et offre la possibilité de vérifier de nouveau contenu en arrière-plan. Activité d’arrière-plan est limitée à environ 10 secondes, sauf si associé à un [tâche en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Une application n’a pas besoin de l’emplacement `UIBackgroundMode` à utiliser ces API de géolocalisation. Étant donné qu’iOS ne suit pas les types de tâches pouvant s’exécuter quand l’appareil est sorti par des modifications dans l’emplacement de l’utilisateur, ces API offrent une solution de contournement pour la mise à jour de contenu en arrière-plan sur iOS 6. *N’oubliez pas que déclencher des mises à jour en arrière-plan avec les API de localisation dessine sur les ressources de l’appareil et risque de perturber les utilisateurs qui ne comprennent pas pourquoi une application doit accéder à leur emplacement*. Faites preuve de discernement lors de l’implémentation de surveillance de la région ou l’emplacement des modifications importantes pour les applications qui ne sont pas déjà à l’aide de l’API de l’emplacement de traitement en arrière-plan.

Applications à l’aide de la surveillance de l’emplacement pour le traitement en arrière-plan exposent un défaut dans iOS 6 : si les besoins d’une application ne tiennent pas dans une catégorie nécessaire à l’arrière-plan, il a backgrounding dispose d’options limitées. Avec l’introduction de deux nouvelles API, *récupération en arrière-plan* et *Notifications à distance*, iOS 7 (et supérieur) fournit des opportunités de backgrounding à davantage d’applications. Les deux sections suivantes présentent ces nouvelles API.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Récupération en arrière-plan (iOS 7 et ultérieures)

Dans iOS 6, une application de saisie de premier plan avaient besoin de temps pour charger le nouveau contenu, présenter brièvement les utilisateurs avec du contenu, qu'ils ont déjà vu. Récupération en arrière-plan permet aux applications de charger de nouvelles données *avant* un utilisateur lance l’application et fournir à l’utilisateur avec le contenu le plus récent.

Pour implémenter la récupération en arrière-plan, modifier *Info.plist* et vérifiez le **Enable Background Modes** et **récupération en arrière-plan** cases à cocher :

 [![](updating-an-application-in-the-background-images/fetch.png "Modifier le fichier Info.plist et cochez les cases à cocher Activer les Modes d’arrière-plan et de récupération en arrière-plan")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Ensuite, dans le `AppDelegate`, remplacer le `FinishedLaunching` méthode pour définir l’intervalle d’extraction minimale. Dans cet exemple, nous laissons le système d’exploitation décider comment souvent extraire de nouveau contenu :

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Pour finir, effectuez l’extraction en substituant le `PerformFetch` méthode dans le `AppDelegate`et en passant un *Gestionnaire d’achèvement*. Le Gestionnaire d’achèvement est un délégué qui prend un `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Lorsque nous avons terminé la mise à jour de contenu, nous informer le système d’exploitation en appelant le Gestionnaire d’achèvement avec le statut approprié. iOS offre trois options pour l’état de gestionnaire d’achèvement :

1.  `UIBackgroundFetchResult.NewData` -Appelée lorsque le nouveau contenu a été extrait, et l’application a été mis à jour.
1.  `UIBackgroundFetchResult.NoData` -Appelée lorsque l’extraction du nouveau contenu a abouti, mais aucun contenu n’est disponible.
1.  `UIBackgroundFetchResult.Failed` -Utile pour la gestion des erreurs, on parle lors de l’opération de récupération n’a pas pu suivre.


Applications à l’aide de la récupération en arrière-plan peuvent effectuer des appels pour mettre à jour de l’interface utilisateur à partir de l’arrière-plan. Lorsque l’utilisateur ouvre l’application, l’interface utilisateur sera jusqu'à la date et l’affichage du nouveau contenu. Cela met également à jour instantané du sélecteur d’application de l’application, afin que l’utilisateur peut voir lorsque l’application a le nouveau contenu.

> [!IMPORTANT]
> Une fois `PerformFetch` est appelée, l’application a environ 30 secondes pour lancer le téléchargement du contenu nouveau et appeler le bloc de gestionnaire d’achèvement. Si cela prend trop de temps, l’application va être interrompue. Envisagez d’utiliser la récupération en arrière-plan avec le _Service de transfert en arrière-plan_ lors du téléchargement de média ou autres fichiers volumineux.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

Dans l’exemple de code ci-dessus, nous laisser le système d’exploitation décider de la fréquence à laquelle extraire de nouveau contenu en définissant l’intervalle de récupération minimale `BackgroundFetchIntervalMinimum`. iOS offre trois options pour l’intervalle d’extraction :

1.  `BackgroundFetchIntervalNever` -Informer le système pour extraire jamais de nouveau contenu. Cela permet de désactiver l’extraction dans certaines situations, par exemple lorsque l’utilisateur n’est pas signée. Ceci est la valeur par défaut pour l’intervalle d’extraction. 
1.  `BackgroundFetchIntervalMinimum` -Laisser le système décider comment souvent extraire selon des modèles utilisateur, autonomie, l’utilisation des données et les besoins des autres applications.
1.  `BackgroundFetchIntervalCustom` -Si vous connaissez la fréquence à laquelle le contenu de l’application est mise à jour, vous pouvez spécifier un intervalle de « veille » après chaque opération d’extraction, au cours de laquelle l’application ne peuvent pas l’extraction du nouveau contenu. Une fois cet intervalle est opérationnel, le système détermine quand rechercher du contenu.


Les deux `BackgroundFetchIntervalMinimum` et `BackgroundFetchIntervalCustom` s’appuient sur le système pour planifier les extractions. Cet intervalle est dynamique, adaptation aux besoins de l’appareil, ainsi que les habitudes de l’utilisateur. Par exemple, si un utilisateur vérifie une application chaque matin, et un autre vérifie toutes les heures, iOS garantira le contenu est mis à jour pour les deux utilisateurs chaque fois qu’ils ouvrent l’application.

Récupération en arrière-plan doit être utilisée pour les applications qui mettent à jour fréquemment avec du contenu non critiques. Pour les applications avec les mises à jour critiques, les Notifications à distance doit être utilisées. Notifications à distance sont basées sur la récupération en arrière-plan et partagent le même gestionnaire d’achèvement. Nous allons approfondir ensuite Notifications à distance.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Notifications à distance (iOS 7 et ultérieures)

Notifications Push sont des messages JSON envoyées à partir d’un fournisseur à un appareil par le biais de la *Apple Push Notification service (APNs)*.

Dans iOS 6, des notifications push entrantes indique au système pour avertir l’utilisateur que quelque chose d’intéressant s’est produit dans une application. En cliquant sur la notification extrait l’application hors de l’état suspendu ou se termine et l’application serait commencer la mise à jour de contenu. iOS 7 (et supérieur) étend les notifications push ordinaire en permettant aux applications une occasion de mettre à jour le contenu en arrière-plan *avant* avertir l’utilisateur, afin que l’utilisateur peut ouvrir l’application et être présentée avec le nouveau contenu immédiatement.

Pour implémenter des notifications à distance, modifiez *Info.plist* et vérifiez le **Enable Background Modes** et **notifications à distance** cases à cocher :

 [![](updating-an-application-in-the-background-images/remote.png "Mode d’arrière-plan de la valeur Enable Background Modes et des notifications à distance")](updating-an-application-in-the-background-images/remote.png#lightbox)

Ensuite, définissez le `content-available` indicateur sur la notification push elle-même à 1. Cela permet à l’application de savoir à extraire de nouveau contenu avant d’afficher l’alerte :

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

Dans le *AppDelegate*, remplacer le `DidReceiveRemoteNotification` méthode pour vérifier la charge utile de notification pour le contenu disponible et appelez du bloc de gestionnaire d’achèvement approprié :

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Notifications à distance doivent être utilisées pour les mises à jour peu fréquents avec du contenu qui est essentiel pour la fonctionnalité d’application. Pour plus d’informations sur les notifications à distance, consultez le Xamarin [Notifications Push dans iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) guide.

> [!IMPORTANT]
> Étant donné que le mécanisme de mise à jour dans les Notifications à distance repose sur la récupération en arrière-plan, l’application doit lancer le téléchargement du contenu nouveau et appeler le bloc de gestionnaire de saisie semi-automatique dans les 30 secondes de la réception de notifications ou iOS pour fermer l’application. Prendre en compte le jumelage des Notifications à distance avec _Service de transfert en arrière-plan_ lors du téléchargement de média ou autres fichiers volumineux en arrière-plan.


### <a name="silent-remote-notifications"></a>Notifications à distance en mode silencieux

Notifications à distance sont un moyen simple pour avertir les applications des mises à jour et de lancer l’extraction de nouveau contenu, mais il existe des cas où nous ne devons avertir l’utilisateur que quelque chose a changé. Par exemple, si un utilisateur signale un fichier permettant la synchronisation, nous n’avez pas besoin de les avertir chaque fois que le fichier met à jour. Synchronisation de fichier n’est pas un événement surprenant, et aucune attention immédiate de l’utilisateur. Les utilisateurs attendent simplement le fichier soit à jour lorsqu’il est ouvert.

Pour les cas comme celui ci-dessus, iOS permet de notifications push à envoyer en mode silencieux - autrement dit, sans une alerte. Pour activer une notification régulière dans une autre en mode silencieux, supprimez simplement l’alerte à partir de la charge utile de notification :

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Limites du débit

La différence majeure entre normales et en mode silencieux les notifications à partir d’un point de vue des développeurs est qu’en mode silencieux de notifications push est soumis à restriction. APNs retardera la remise de notifications push en mode silencieux à l’appareil si le taux de push est trop élevé. Il s’agit pour vous assurer que les applications ne d’épuiser les ressources de périphérique avec trop de notifications en mode silencieux.

Toutefois, APNs permettra de notifications en mode silencieux une « superposition » en même temps que d’une Notification à distance normal ou d’une réponse keep-alive. Étant donné que des notifications régulières ne sont pas des taux limité, elles peuvent servir à stockée des silencieux des notifications push à partir de l’APNs à l’appareil, comme l’illustre le diagramme suivant :

 [![](updating-an-application-in-the-background-images/silent.png "Des notifications régulières peuvent être utilisées pour stockées en mode silencieux des notifications push à partir de l’APNs à l’appareil, comme illustré dans ce diagramme")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Encourage les développeurs à envoyer des notifications push silent chaque fois que l’application requiert d’Apple et permettre à l’APNs de planifier leur remise.


Dans cette section, nous avons couvert les différentes options de l’actualisation du contenu en arrière-plan pour exécuter des tâches qui ne tiennent pas dans une catégorie nécessaire à l’arrière-plan. Maintenant, nous allons voir certains de ces API en action.

 [Suivant : 4ème partie - iOS Backgrounding procédures pas à pas](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
