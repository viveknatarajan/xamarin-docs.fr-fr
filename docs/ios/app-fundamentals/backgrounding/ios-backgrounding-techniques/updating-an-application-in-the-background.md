---
title: Mise à jour d’une Application en arrière-plan
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 370d1cba71fa695e4e01dfb93241536a8df01b11
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="updating-an-application-in-the-background"></a>Mise à jour d’une Application en arrière-plan

L’actualisation en arrière-plan est le processus de mise en éveil d’une application est suspendue ou ne pas en cours d’exécution et mettre à jour avec le nouveau contenu. iOS offre trois options d’actualisation du contenu en arrière-plan :

1.  *Surveillance de la région* et *Service de modifications importantes emplacement* -arrière-plan de déclencheur APIs sensibles à l’emplacement des mises à jour en fonction des changements dans l’emplacement de l’utilisateur. Ces API peuvent être utilisées avec précaution pour actualiser le contenu dans les applications non basée sur un emplacement iOS 6, où les autres options ne sont pas disponibles.
1.  *Fetch (iOS 7 +) d’arrière-plan* -une approche temporelle pour l’actualisation *non critiques* qui met à jour le contenu *fréquemment* .
1.  *Notifications à distance (iOS 7 +)* -Applications qui reçoivent des notifications push peuvent utiliser les notifications pour déclencher des actualisations de contenu en arrière-plan. Cette méthode peut être utilisée pour mettre à jour avec *temps important,* qui met à jour le contenu *sporadiquement* .


Les sections suivantes couvrent les principes fondamentaux de ces options.

## <a name="region-monitoring-and-significant-location-changes"></a>Analyse de la région et les modifications importantes emplacement

iOS fournit deux API sensibles à l’emplacement avec backgrounding fonctionnalités :

1.  *Surveillance de la région* est le processus de définition des régions avec des limites et le périphérique de sortie de veille lorsque l’utilisateur entre ou quitte une région. Les régions sont circulaires et peuvent être de taille variable. Quand l’utilisateur franchit une limite de région, le périphérique réveille pour gérer l’événement, généralement par déclencher une notification ou lancer une tâche. Surveillance de la région nécessite GPS et augmente l’utilisation des données et la batterie.
1.  Le *Service de modifications importantes emplacement* est une option plus simple, la conservation d’alimentation disponible pour les appareils avec radios cellulaires. Une application qui écoute les modifications d’emplacement importantes soient prévenue lorsque l’appareil bascule tours de cellule. Ce service peut être utilisé pour réactiver une application suspendue ou arrêtée et fournit une opportunité de vérifier de nouveau contenu en arrière-plan. Activité d’arrière-plan est limitée à environ 10 secondes, sauf si associé à un [tâche en arrière-plan](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Une application ne doit pas l’emplacement `UIBackgroundMode` d’utiliser ces API de géolocalisation. Étant donné que les types de tâches peuvent s’exécuter quand l’appareil est mis en modifications dans l’emplacement de l’utilisateur ne suit pas les e/s, ces API fournissent une solution de contournement pour mettre à jour le contenu de l’arrière-plan sur iOS 6. *N’oubliez pas que déclencher des mises à jour en arrière-plan avec les API de basée sur l’emplacement doit dessiner sur les ressources de l’appareil et risque de perturber les utilisateurs qui ne comprennent pas pourquoi une application doit accéder à leur emplacement*. Procédez avec caution lors de la mise en œuvre de la surveillance de la région ou d’importantes modifications d’emplacement pour dans les applications qui ne sont pas déjà à l’aide de l’API de l’emplacement de traitement en arrière-plan.

Applications à l’aide de la surveillance d’emplacement pour le traitement en arrière-plan exposent une faille dans iOS 6 : si les besoins d’une application ne tiennent pas dans une catégorie nécessaire à l’arrière-plan, il a backgrounding des options limitées. Avec l’introduction de deux nouvelles API, *arrière-plan Fetch* et *des Notifications à distance*, iOS 7 (et supérieur) offre des possibilités backgrounding d’applications. Les deux sections suivantes présentent ces nouvelles API.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Extraction en arrière-plan (iOS 7 et version ultérieure)

Dans iOS 6, une application de saisie de premier plan nécessaire des temps de chargement de nouveau contenu, en présentant brièvement les utilisateurs avec le contenu, qu'ils ont déjà vu. Extraction en arrière-plan permet aux applications de charger de nouvelles données *avant* un utilisateur lance l’application et fournir à l’utilisateur avec le contenu le plus récent.

Pour implémenter l’extraction en arrière-plan, modifier *Info.plist* et vérifiez la **activer les Modes d’arrière-plan** et **extraction en arrière-plan** cases à cocher :

 [![](updating-an-application-in-the-background-images/fetch.png "Modifier le fichier Info.plist et cochez les cases à cocher Activer les Modes d’arrière-plan et l’extraction de l’arrière-plan")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Ensuite, dans le `AppDelegate`, remplacer le `FinishedLaunching` méthode pour définir l’intervalle de récupération minimale. Dans cet exemple, nous laissons le système d’exploitation en déterminer la fréquence à laquelle récupérer le nouveau contenu :

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Pour finir, effectuez l’extraction en substituant le `PerformFetch` méthode dans le `AppDelegate`et en passant un *Gestionnaire d’achèvement*. Le Gestionnaire d’achèvement est un délégué qui accepte une `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Lorsque nous avons terminé la mise à jour de contenu, nous indiquer le système d’exploitation en appelant le Gestionnaire d’achèvement avec l’état approprié. iOS offre trois options pour l’état d’achèvement gestionnaire :

1.  `UIBackgroundFetchResult.NewData` -Appelée lorsqu’un nouveau contenu a été extrait et l’application a été mis à jour.
1.  `UIBackgroundFetchResult.NoData` -Appelée lors de l’extraction pour le nouveau contenu a abouti, mais aucun contenu n’est disponible.
1.  `UIBackgroundFetchResult.Failed` -Utile pour la gestion des erreurs, cela est appelé lors de l’extraction n’a pas pu passer par.


Applications à l’aide de Fetch d’arrière-plan peuvent adresser des appels pour mettre à jour l’interface utilisateur de l’arrière-plan. Lorsque l’utilisateur ouvre l’application, l’interface utilisateur sera jusqu'à la date et l’affichage du nouveau contenu. Cela met également à jour instantané du sélecteur de l’application de l’application, afin que l’utilisateur peut voir lorsque l’application a du contenu nouveau.

> [!IMPORTANT]
> Une fois `PerformFetch` est appelée, l’application a environ 30 secondes pour déclencher le téléchargement du contenu nouveau et appeler le bloc de gestionnaire d’achèvement. Si cela est trop longue, l’application va être interrompue. Envisagez d’utiliser l’extraction d’arrière-plan avec le _Service de transfert en arrière-plan_ lors du téléchargement du support ou autres fichiers volumineux.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

Dans l’exemple de code ci-dessus, nous laisser le système d’exploitation décider de la fréquence à laquelle récupérer le contenu de nouveau en affectant à l’intervalle d’extraction minimale `BackgroundFetchIntervalMinimum`. iOS offre trois options pour l’intervalle d’extraction :

1.  `BackgroundFetchIntervalNever` -Indiquez au système pour extraire jamais de nouveau contenu. Cela permet de désactiver l’extraction dans certaines situations, par exemple lorsque l’utilisateur n’est pas connecté. Ceci est la valeur par défaut pour l’intervalle d’extraction. 
1.  `BackgroundFetchIntervalMinimum` -Laisser le système décider de la fréquence d’extraction basé sur des modèles de l’utilisateur, autonomie, l’utilisation des données et les besoins d’autres applications.
1.  `BackgroundFetchIntervalCustom` -Si vous connaissez la fréquence à laquelle le contenu d’une application est mise à jour, vous pouvez spécifier un intervalle de « veille » après chaque opération d’extraction, au cours de laquelle l’application ne peuvent pas extraire de nouveau contenu. Une fois cet intervalle, le système va déterminer quand récupérer le contenu.


Les deux `BackgroundFetchIntervalMinimum` et `BackgroundFetchIntervalCustom` s’appuient sur le système pour planifier les extractions. Cet intervalle est dynamique, pour s’adapter aux besoins de l’appareil, ainsi que des habitudes de l’utilisateur. Par exemple, si un utilisateur recherche une application, tous les matins, et un autre vérifie toutes les heures, iOS garantit le contenu est mis à jour pour les deux utilisateurs chaque fois qu’ils ouvrent l’application.

Extraction en arrière-plan doit être utilisée pour les applications qui mettent à jour fréquemment avec un contenu non critiques. Pour les applications mises à jour critiques, des Notifications à distance doit être utilisées. Notifications à distance sont basées sur l’extraction de l’arrière-plan et partagent le même gestionnaire d’achèvement. Nous allons approfondir ensuite des Notifications à distance.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Notifications à distance (iOS 7 et version ultérieure)

Notifications Push sont des messages JSON envoyés à partir d’un fournisseur à un périphérique par le biais de la *les services de notifications Push Apple (APN)*.

Dans iOS 6, les notifications push un entrant indique au système pour avertir l’utilisateur que quelque chose d’intéressant s’est produit dans une application. En cliquant sur la notification d’extrait de l’application hors de l’état suspendu ou arrêté, et commencez l’application mise à jour de contenu. iOS 7 (et supérieur) étend les notifications push ordinaire en permettant aux applications de possibilité de mettre à jour le contenu en arrière-plan *avant* avertir l’utilisateur, afin que l’utilisateur peut ouvrir l’application et affiche un nouveau contenu immédiatement.

Pour implémenter des notifications à distance, vous devez modifier *Info.plist* et vérifiez la **activer les Modes d’arrière-plan** et **des notifications à distance** cases à cocher :

 [![](updating-an-application-in-the-background-images/remote.png "Mode d’arrière-plan définie pour activer les Modes d’arrière-plan et des notifications à distance")](updating-an-application-in-the-background-images/remote.png#lightbox)

Ensuite, définissez le `content-available` indicateur sur les notifications push lui-même à 1. Cela permet à l’application de savoir à extraire de nouveau contenu avant l’affichage de l’alerte :

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

Dans le *AppDelegate*, remplacer le `DidReceiveRemoteNotification` méthode pour vérifier la charge utile de notification pour le contenu disponible, appelez le bloc de gestionnaire d’achèvement approprié :

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Notifications à distance doivent être utilisées pour peu fréquentes mises à jour avec du contenu qui est essentiel pour les fonctionnalités de l’application. Pour plus d’informations sur les notifications à distance, consultez le Xamarin [des Notifications Push dans iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) guide.

> [!IMPORTANT]
> Le mécanisme de mise à jour dans les Notifications à distance étant basé sur l’extraction de l’arrière-plan, l’application doit déclencher le téléchargement du contenu nouveau et appeler le bloc de gestionnaire d’achèvement dans les 30 secondes de réception de la notification, ou iOS mettra fin à l’application. Envisagez d’associer des Notifications à distance avec _Service de transfert en arrière-plan_ lors du téléchargement du support ou autres fichiers volumineux en arrière-plan.


### <a name="silent-remote-notifications"></a>Notifications à distance en mode silencieux

Notifications à distance sont un moyen simple pour informer les applications des mises à jour et de déclencher l’extraction du nouveau contenu, mais il existe des cas où nous n’avez pas besoin pour informer l’utilisateur que quelque chose a changé. Par exemple, si un utilisateur de marquer un fichier pour la synchronisation, nous n’avez pas besoin de les avertir chaque fois que des mises à jour le fichier. Synchronisation de fichier n’est pas un événement étonnant, ni nécessite une attention immédiate de l’utilisateur. Les utilisateurs attendent simplement le fichier sont à jour à son ouverture.

Dans des cas comme celle ci-dessus, iOS permet de notifications push à envoyer en mode silencieux - c'est-à-dire, sans une alerte. Pour activer une notification régulière en un en mode silencieux, il vous suffit de supprimer l’alerte à partir de la charge utile de notification :

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Limites du débit

La différence majeure entre normales et en mode silencieux les notifications à partir d’un point de vue du développeur est en mode silencieux push taux limitée. APNs retardera la remise de notifications push en mode silencieux à l’appareil si le taux de push est trop intensive. Il s’agit pour vous assurer que les applications ne d’épuiser les ressources de périphérique avec trop de notifications en mode silencieux.

Toutefois, APNs permettra de notifications en mode silencieux « phagocytent » à côté d’une Notification à distance normal ou d’une réponse keep-alive. Des notifications régulières n’étant pas limité de taux, elles peuvent servir à pousser stockée des notifications en mode silencieux à partir de l’APNs sur le périphérique, comme illustré dans le diagramme suivant :

 [![](updating-an-application-in-the-background-images/silent.png "Des notifications régulières peuvent être utilisées pour envoyer des notifications silencieuses stockées à partir de l’APNs sur l’appareil, comme illustré dans ce schéma.")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple encourage les développeurs à chaque fois que l’application requiert d’envoi des notifications push en mode silencieux et laissez le certificat APNs planifier leur remise.


Dans cette section, nous avons couvert les différentes options d’actualisation du contenu en arrière-plan pour exécuter les tâches qui ne tiennent pas dans une catégorie nécessaire à l’arrière-plan. Maintenant, nous allons voir certaines de ces API en action.

 [Ensuite : Partie 4 - iOS Backgrounding procédures pas à pas](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
