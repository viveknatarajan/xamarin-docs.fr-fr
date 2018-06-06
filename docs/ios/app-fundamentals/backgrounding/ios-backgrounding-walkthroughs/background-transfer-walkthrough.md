---
title: NSURLSession dans Xamarin.iOS et transfert en arrière-plan
description: Ce document fournit une procédure pas à pas qui montre comment utiliser le transfert en arrière-plan et NSUrlSession pour lancer le téléchargement d’une image de grande taille et continuer à ce téléchargement lorsque l’application est placée en arrière-plan.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 08a0ba1337c0d28d1f0d60d04394ccaf4a9ccfc7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783737"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>NSURLSession dans Xamarin.iOS et transfert en arrière-plan

Un transfert en arrière-plan est lancé en configurant un arrière-plan `NSURLSession` et file d’attente charger ou télécharger des tâches. Si les tâches se termine lors de l’application est backgrounded, suspendue ou arrêtée, iOS informe l’application en appelant le Gestionnaire d’achèvement de l’application *AppDelegate*. Le diagramme suivant illustre cela en action :

 [![](background-transfer-walkthrough-images/transfer.png "Un transfert en arrière-plan est initialisé en configurant un arrière-plan NSURLSession et file d’attente charger ou télécharger des tâches")](background-transfer-walkthrough-images/transfer.png#lightbox)

Voyons à quoi cela ressemble dans le code.

## <a name="configuring-a-background-session"></a>Configuration d’une Session en arrière-plan

Pour rendre une session en arrière-plan, créez un `NSUrlSession` et configurez-le en utilisant un `NSUrlSessionConfiguration` objet.

L’objet de configuration détermine ce que peut faire la session et les types de tâches qu’il peut s’exécuter.
Sessions configurées à l’aide de la `CreateBackgroundSessionConfiguration` méthode s’exécutera dans un processus distinct et effectuent des transferts (WiFi) discrétionnaires pour conserver les données et l’autonomie.
L’exemple de code suivant illustre une configuration correcte d’une session de transfert en arrière-plan à l’aide du `CreateBackgroundSessionConfiguration` (méthode) et un identificateur de chaîne unique :

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

En dehors d’un objet de configuration, une session nécessite également un délégué de session et une file d’attente.
La file d’attente détermine l’ordre dans lequel les tâches seront termine. Le délégué de la session chaperones le processus de transfert et gère l’authentification, la mise en cache et les autres problèmes liés à la session.

## <a name="working-with-tasks-and-delegates"></a>Utilisation des tâches et des délégués

Maintenant que nous avons configuré une session en arrière-plan, nous allons déclencher des tâches pour gérer le transfert. Nous pouvons effectuer le suivi de ces tâches à l’aide de l’un `NSUrlSessionDelegate` instance appelée un délégué de la session. Le délégué de la session est responsable de la mise en éveil d’une application arrêtée ou suspendue en arrière-plan pour l’authentification de handle, erreurs ou à l’achèvement de transfert.

Un `NSUrlSessionDelegate` fournit les méthodes de base suivantes pour vérifier l’état du transfert :

-  *DidFinishEventsForBackgroundSession* -cette méthode est appelée lorsque toutes les tâches terminées, et le transfert est terminé.
-  *DidReceiveChallenge* - appelé pour des informations d’identification de demande lors de l’autorisation est requise.
-  *DidBecomeInvalidWithError* -appelé si le `NSURLSession` est invalidée.


Les sessions d’arrière-plan requièrent plus spécialisées délégués selon les types de tâches qui sont en cours d’exécution. Les sessions en arrière-plan sont limitées à deux types de tâches :

-  *Télécharger des tâches* -tâches de type `NSUrlSessionUploadTask` utiliser le `NSUrlSessionTaskDelegate` , qui hérite de `NSUrlSessionDelegate` . Ce délégué fournit des méthodes supplémentaires pour effectuer le suivi téléchargent la progression et la redirection HTTP de handle.
-  *Télécharger les tâches* -tâches de type `NSUrlSessionDownloadTask` utiliser le `NSUrlSessionDownloadDelegate` , qui hérite de `NSUrlSessionTaskDelegate` . Ce délégué fournit que toutes les méthodes pour charger des tâches, ainsi que les méthodes de téléchargement spécifique pour suivre la progression du téléchargement et de déterminer lorsqu’une tâche de téléchargement a repris ou s’est terminée.


Le code suivant définit une tâche qui peut être utilisée pour télécharger une image à partir d’une URL. Nous commencerons par la tâche en appelant `CreateDownloadTask` sur notre session d’arrière-plan et en passant la demande d’URL :

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Ensuite, nous allons créer un délégué de téléchargement de nouvelle session afin de suivre toutes les tâches de téléchargement de cette session :

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Si vous souhaitez connaître la progression d’une tâche de téléchargement, nous pouvons remplacer le `DidWriteData` pour suivre la progression et la même méthode de mise à jour de l’interface utilisateur. Mises à jour de l’interface utilisateur apparaît immédiatement si l’application est au premier plan, ou sera être en attente de l’utilisateur la prochaine fois qu’ils ouvrent l’application.

L’API de délégué de session fournit un large ensemble d’outils permettant d’interagir avec les tâches. Pour obtenir la liste complète de la session de déléguer des méthodes, reportez-vous à la `NSUrlSessionDelegate` documentation de l’API.

> [!IMPORTANT]
> Les sessions en arrière-plan sont démarrées sur un thread d’arrière-plan, afin que tous les appels à mettre à jour l’interface utilisateur doivent être explicitement exécutés sur le thread d’interface utilisateur en appelant `InvokeOnMainThread` afin d’éviter les e/s de l’arrêt de l’application. 


## <a name="handling-transfer-completion"></a>Fin du transfert de la gestion

L’étape finale consiste à permettre à l’application lorsque toutes les tâches associées à la session terminées et de gérer le nouveau contenu.

Dans le *AppDelegate*, s’abonner à la `HandleEventsForBackgroundUrl` événement. Lorsque l’application passe à l’arrière-plan et une session de transfert est en cours d’exécution, cette méthode est appelée et le système nous passe un gestionnaire d’achèvement :

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Nous allons utiliser le Gestionnaire d’achèvement pour informer iOS lorsque votre application a terminé le traitement.

Rappelez-vous qu’une session peut générer plusieurs tâches pour traiter un transfert. Lorsque la dernière tâche est terminée, une application suspendue ou arrêtée est ré-exécutée en arrière-plan. Ensuite, l’application se connecte à nouveau à la `NSURLSession` à l’aide de l’identificateur de session unique et appelle `DidFinishEventsForBackgroundSession` sur le délégué de la session. Cette méthode est la possibilité de l’application pour gérer le nouveau contenu, y compris la mise à jour de l’interface utilisateur pour refléter les résultats du transfert :

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Une fois que nous avons terminé le traitement du nouveau contenu, nous appelons au gestionnaire d’achèvement pour que le système sache qu’il est sécurisé à prendre un instantané de l’application et revenir à la mise en veille :

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

Dans cette procédure pas à pas, nous avons abordé les étapes de base pour implémenter le Service de transfert en arrière-plan dans iOS 7.



## <a name="related-links"></a>Liens associés

- [Simple transfert en arrière-plan (exemple)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
