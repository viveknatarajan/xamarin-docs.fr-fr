---
title: Transfert en arrière-plan et NSURLSession dans Xamarin.iOS
description: Ce document fournit une procédure pas à pas qui montre comment utiliser le transfert en arrière-plan et NSUrlSession pour lancer le téléchargement d’une grande image, puis continuez ce téléchargement lorsque l’application est placée en arrière-plan.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e525388290d92901e68e61f1ffa81866f5aac4d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114234"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Transfert en arrière-plan et NSURLSession dans Xamarin.iOS

Un transfert en arrière-plan est lancé en configurant un arrière-plan `NSURLSession` et file d’attente charger ou télécharger des tâches. Si les tâches sont terminées pendant que l’application est lancé en arrière-plan, suspendue ou arrêtée, iOS notifie l’application en appelant le Gestionnaire d’achèvement de l’application *AppDelegate*. Le diagramme suivant illustre cela en action :

 [![](background-transfer-walkthrough-images/transfer.png "Un transfert en arrière-plan est lancé en configurant un arrière-plan NSURLSession et file d’attente charger ou télécharger des tâches")](background-transfer-walkthrough-images/transfer.png#lightbox)

Nous allons voir à quoi cela ressemble dans le code.

## <a name="configuring-a-background-session"></a>Configuration d’une Session d’arrière-plan

Pour effectuer une session en arrière-plan, créez un nouveau `NSUrlSession` et configurez-le en utilisant un `NSUrlSessionConfiguration` objet.

L’objet de configuration détermine ce que peut faire la session et les types de tâches peut s’intégrer.
Sessions configurées à l’aide de la `CreateBackgroundSessionConfiguration` méthode s’exécutent dans un processus distinct et effectuent des transferts (Wi-Fi) discrétionnaires pour conserver les données et l’autonomie.
L’exemple de code suivant illustre la configuration correcte d’une session de transfert en arrière-plan à l’aide du `CreateBackgroundSessionConfiguration` (méthode) et un identificateur de chaîne unique :

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

Mis à part un objet de configuration, une session requiert également un délégué de session et une file d’attente.
La file d’attente détermine l’ordre dans lequel les tâches seront termine. Le délégué de la session chaperones le processus de transfert et gère l’authentification, la mise en cache et les autres problèmes liés à la session.

## <a name="working-with-tasks-and-delegates"></a>Utilisation des tâches et des délégués

Maintenant que nous avons configuré une session en arrière-plan, nous allons lancer des tâches pour gérer le transfert. Nous pouvons effectuer le suivi de ces tâches à l’aide de l’un `NSUrlSessionDelegate` instance appelée un délégué de la session. Le délégué de la session est responsable de la réactivation d’une application, arrêtée ou suspendue, en arrière-plan pour l’authentification de handle, erreurs ou à l’achèvement de transfert.

Un `NSUrlSessionDelegate` fournit les méthodes de base suivantes pour vérifier l’état de transfert :

-  *DidFinishEventsForBackgroundSession* -cette méthode est appelée lorsque toutes les tâches terminées, et le transfert est terminé.
-  *DidReceiveChallenge* : appelée demande des informations d’identification lors de l’autorisation est requise.
-  *DidBecomeInvalidWithError* -appelé si le `NSURLSession` est invalidée.


Sessions de l’arrière-plan nécessitent des délégués plus spécialisées selon les types de tâches qui sont en cours d’exécution. Les sessions en arrière-plan sont limitées à deux types de tâches :

-  *Charger des tâches* -tâches de type `NSUrlSessionUploadTask` utiliser le `NSUrlSessionTaskDelegate` , qui hérite de `NSUrlSessionDelegate` . Ce délégué fournit des méthodes supplémentaires pour effectuer le suivi charger la progression, la redirection HTTP de handle et bien plus encore.
-  *Télécharger les tâches* -tâches de type `NSUrlSessionDownloadTask` utiliser le `NSUrlSessionDownloadDelegate` , qui hérite de `NSUrlSessionTaskDelegate` . Ce délégué fournit que toutes les méthodes pour charger des tâches, ainsi que les méthodes de téléchargement spécifique pour suivre la progression du téléchargement et de déterminer lorsqu’une tâche de téléchargement a repris ou terminé.


Le code suivant définit une tâche qui peut être utilisée pour télécharger une image à partir d’une URL. Nous débuterons la tâche en appelant `CreateDownloadTask` sur notre session d’arrière-plan et en passant dans la demande d’URL :

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

Si vous souhaitez connaître la progression d’une tâche de téléchargement, nous pouvons remplacer le `DidWriteData` méthode à suivre la progression et même mettre à jour l’interface utilisateur. Mises à jour de l’interface utilisateur seront affiche immédiatement si l’application est au premier plan, ou vous attend l’utilisateur la prochaine fois qu’ils ouvrent l’application.

L’API de délégué de session fournit un kit de ressources vaste pour interagir avec les tâches. Pour obtenir la liste complète de la session de déléguer des méthodes, reportez-vous à la `NSUrlSessionDelegate` documentation de l’API.

> [!IMPORTANT]
> Les sessions en arrière-plan sont démarrées sur un thread d’arrière-plan, afin que tous les appels à mettre à jour de l’interface utilisateur doivent explicitement être exécutés sur le thread d’interface utilisateur en appelant `InvokeOnMainThread` afin d’éviter les e/s de l’arrêt de l’application. 


## <a name="handling-transfer-completion"></a>Exécution du transfert gestion

L’étape finale consiste à permettre à l’application quand toutes les tâches associées à la session ont terminé et gérer le nouveau contenu.

Dans le *AppDelegate*, s’abonner à la `HandleEventsForBackgroundUrl` événement. Lorsque l’application entre l’arrière-plan et une session de transfert est en cours d’exécution, cette méthode est appelée et le système nous transmet un gestionnaire d’achèvement :

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Nous allons utiliser le Gestionnaire d’achèvement pour informer iOS lorsque notre application est terminée traitement.

Rappelez-vous qu’une session peut générer plusieurs tâches pour traiter un transfert. Lorsque la dernière tâche est terminée, une application suspendue ou arrêtée est ré-lancée en arrière-plan. Ensuite, l’application se connecte à nouveau à la `NSURLSession` à l’aide de l’identificateur de session unique et appelle `DidFinishEventsForBackgroundSession` sur le délégué de la session. Cette méthode est l’occasion de l’application pour gérer le nouveau contenu, y compris la mise à jour de l’interface utilisateur pour refléter les résultats du transfert :

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Une fois que nous avons terminé le traitement du contenu nouveau, nous appelons le Gestionnaire d’achèvement pour indiquer au système qu'il est possible de prendre un instantané de l’application et revenir en arrière en veille :

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

Dans cette procédure pas à pas, nous avons couvert les étapes de base pour implémenter le Service de transfert en arrière-plan dans iOS 7.



## <a name="related-links"></a>Liens associés

- [Simple transfert en arrière-plan (exemple)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
