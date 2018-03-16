---
title: Thread
ms.topic: article
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 693ada611dc24d3bb22de7c51efe378939a732ad
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2018
---
# <a name="threading"></a>Thread

Le runtime Xamarin.iOS permet aux développeurs .NET API, les deux explicitement lors de l’utilisation de threads de threading (`System.Threading.Thread, System.Threading.ThreadPool`) et implicitement lorsque vous utilisez les modèles de délégué asynchrone ou les méthodes BeginXXX ainsi que de la gamme complète d’API qui prennent en charge le Bibliothèque parallèle de tâches.



Xamarin recommande vivement d’utiliser le [bibliothèque parallèle de tâches](http://msdn.microsoft.com/en-us/library/dd460717.aspx) (TPL) pour générer des applications pour les raisons suivantes :
-  Le Planificateur de la bibliothèque parallèle de tâches par défaut délègue l’exécution des tâches au pool de threads, qui à son tour augmentera dynamiquement le nombre de threads nécessaires comme processus a lieu, tout en évitant un scénario où trop de threads finissent en concurrence pour le temps processeur. 
-  Il est plus facile de réfléchir aux opérations en termes de tâches. Vous pouvez facilement les manipuler, les planifier, sérialiser leur exécution ou lancer plusieurs en parallèle avec un ensemble rich d’API. 
-  C’est la base pour la programmation avec les extensions de langage d’async c#. 


Le pool de threads à variation lente augmente le nombre de threads selon vos besoins en fonction du nombre de cœurs de processeur disponibles sur le système, de la charge du système et de vos demandes d’applications. Vous pouvez utiliser ce pool de threads en appelant des méthodes dans `System.Threading.ThreadPool` ou à l’aide de la valeur par défaut `System.Threading.Tasks.TaskScheduler` (dans le cadre de la *des infrastructures parallèles*).

Les développeurs utilisent généralement des threads lorsqu’ils ont besoin créer des applications réactives et ils ne souhaitent pas bloquer l’interface utilisateur principale, exécutez la boucle.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Développement d’Applications réactives

Accès aux éléments d’interface doit être limité à la même thread que celui qui exécute la boucle principale pour votre application. Si vous souhaitez apporter des modifications à l’interface utilisateur principale à partir d’un thread, vous devez file d’attente le code à l’aide de [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), comme suit :

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

Ci-dessus appelle le code dans le délégué dans le contexte du thread principal, sans entraîner des conditions de concurrence qui pouvaient bloquer potentiellement de votre application.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Thread et le Garbage Collection

Au cours de l’exécution, le runtime de Objective-C créer et libérer des objets. Si les objets marqués pour « auto-version « Objective-C runtime libère ces objets vers le thread actuel de le `NSAutoReleasePool`. Xamarin.iOS crée un `NSAutoRelease` pool pour chaque thread à partir de la `System.Threading.ThreadPool` et pour le thread principal. Cela par extension couvre tous les threads créés à l’aide de la valeur par défaut TaskScheduler dans System.Threading.Tasks.

Si vous créez vos propres threads à l’aide de `System.Threading` vous devez fournir vous-même `NSAutoRelease` pool pour éviter la fuite de données. Pour ce faire, encapsulez simplement votre thread dans le fragment de code suivant :

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Remarque : Depuis Xamarin.iOS 5.2 inutile fournir votre propre `NSAutoReleasePool` puisque les est fourni automatiquement pour vous.


## <a name="related-links"></a>Liens associés

- [Utilisation du thread d’interface utilisateur](~/ios/user-interface/ios-ui/ui-thread.md)
