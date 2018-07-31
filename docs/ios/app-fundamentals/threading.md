---
title: Threading dans Xamarin.iOS
description: Ce document décrit comment utiliser les APIs System.Threading dans une application Xamarin.iOS. Il aborde la bibliothèque parallèle de tâches création d’applications réactives et le garbage collection.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 8e4ee10fdabdcbb4c6cefe02b15dc93459708364
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350418"
---
# <a name="threading-in-xamarinios"></a>Threading dans Xamarin.iOS

Le runtime Xamarin.iOS offre aux développeurs l’accès à .NET API, à la fois explicitement lorsque vous utilisez des threads de threading (`System.Threading.Thread, System.Threading.ThreadPool`) et implicitement lorsque vous utilisez les modèles de délégué asynchrone ou les méthodes BeginXXX, ainsi que la gamme complète d’API qui prennent en charge le Bibliothèque parallèle de tâches.



Xamarin recommande fortement d’utiliser le [bibliothèque parallèle de tâches](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) pour créer des applications pour plusieurs raisons :
-  Le Planificateur de la bibliothèque parallèle de tâches par défaut délègue l’exécution des tâches au pool de threads, qui à son tour, se développera dynamiquement le nombre de threads nécessaires dans le processus a lieu, tout en évitant un scénario où trop de threads se retrouvent en concurrence pour le temps processeur. 
-  Il est plus facile à réfléchir sur les opérations en termes de tâches TPL. Vous pouvez facilement les manipuler, les planifier, sérialiser leur exécution ou lancer plusieurs en parallèle avec un ensemble rich d’API. 
-  Il constitue la base pour la programmation avec les extensions de langage d’async c#. 


Le pool de threads lentement augmente le nombre de threads selon vos besoins en fonction du nombre de cœurs de processeur disponibles sur le système, la charge système et exigée par votre application. Vous pouvez utiliser ce pool de threads en appelant des méthodes dans `System.Threading.ThreadPool` ou à l’aide de la valeur par défaut `System.Threading.Tasks.TaskScheduler` (dans le cadre de la *infrastructures parallèles*).

Les développeurs utilisent généralement des threads lorsqu’ils ont besoin créer des applications réactives et ils ne souhaitent pas bloquer l’interface utilisateur principale, exécutez la boucle.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Développement d’Applications réactives

Accès aux éléments d’interface utilisateur doit être limitée pour le même thread qui exécute la boucle principale de votre application. Si vous souhaitez apporter des modifications à l’interface utilisateur principale à partir d’un thread, vous devez file d’attente le code à l’aide de [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), comme suit :

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

La méthode ci-dessus appelle le code dans le délégué dans le contexte du thread principal, sans provoquer des conditions de concurrence qui pouvait bloquer potentiellement de votre application.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Threading et Garbage Collection

Au cours de l’exécution, le runtime Objective-C est créé et libérer des objets. Si les objets sont marqués pour « auto-version » du runtime Objective-C publiera ces objets vers le thread actuel du `NSAutoReleasePool`. Xamarin.iOS crée une `NSAutoRelease` pool pour chaque thread à partir de la `System.Threading.ThreadPool` et pour le thread principal. Cela par extension couvre tous les threads créés à l’aide de la valeur par défaut TaskScheduler dans System.Threading.Tasks.

Si vous créez vos propres threads à l’aide de `System.Threading` vous devez fournir que vous possédez `NSAutoRelease` pool pour éviter la fuite de données. Pour ce faire, encapsulez simplement votre thread dans le fragment de code suivant :

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Remarque : Depuis Xamarin.iOS 5.2 il est inutile de fournir votre propre `NSAutoReleasePool` plus comme est fourni automatiquement pour vous.


## <a name="related-links"></a>Liens associés

- [Utilisation du thread d’interface utilisateur](~/ios/user-interface/ios-ui/ui-thread.md)
