---
title: Services intentionnels dans Xamarin.Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763340"
---
# <a name="intent-services-in-xamarinandroid"></a>Services intentionnels dans Xamarin.Android

## <a name="intent-services-overview"></a>Vue d’ensemble des Services Intent

Les deux démarré et lié les services exécutés sur le thread principal, ce qui signifie que pour maintenir les performances smooth, un service a besoin effectuer le travail de façon asynchrone. Une des méthodes plus simples pour résoudre ce problème est avec un _modèle de processeur de file d’attente de travail_, où le travail est placé dans une file d’attente est traitée par un thread unique. 

Le [ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) est une sous-classe de la `Service` classe qui fournit une implémentation spécifique Android de ce modèle. Qu’il va gérer le travail de file d’attente, à partir d’un thread de travail à la file d’attente, de service et demandes d’extraction désactiver la file d’attente pour être exécuté sur le thread de travail. Un `IntentService` en mode silencieux arrête lui-même et supprimer le thread de travail lorsqu’il n’existe plus aucun travail dans la file d’attente.
 
Travail est envoyé à la file d’attente en créant un `Intent` , puis en passant qui `Intent` à la `StartService` (méthode).

Il n’est pas possible d’arrêter ou interrompre le `OnHandleIntent` méthode `IntentService` pendant le travail. En raison de cette conception, un `IntentService` doit être conservée sans état &ndash; qu’il ne doit pas dépendre la communication du reste de l’application ou une connexion active. Un `IntentService` est destiné à statelessly traiter les demandes de travail.

Il existe deux conditions sous-classement `IntentService`:

1. Le nouveau type (créée par le sous-classement `IntentService`) remplace uniquement la `OnHandleIntent` (méthode).
2. Le constructeur pour le nouveau type nécessite une chaîne qui est utilisée pour nommer le thread de travail qui gère les demandes. Le nom de ce thread de travail est principalement utilisé lors du débogage de l’application.

Le code suivant illustre un `IntentService` implémentation avec le substituée `OnHandleIntent` méthode :

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }
    
    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

Travail est envoyé à un `IntentService` en instanciant une `Intent` , puis en appelant le [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) méthode avec cette intention en tant que paramètre. L’objectif est passée au service en tant que paramètre dans le `OnHandleIntent` (méthode). Cet extrait de code est un exemple d’envoi d’une demande de travail à une intention : 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

Le `IntentService` peut extraire les valeurs de l’objectif, comme illustré dans cet extrait de code :  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>Liens associés

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
