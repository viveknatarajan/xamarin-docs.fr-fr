---
title: Synchronisation des données hors connexion avec Azure Mobile Apps
description: Cet article explique comment ajouter la fonctionnalité de synchronisation hors connexion à une application Xamarin.Forms qui consomme un back-end Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: 6080b4dc152558d6f532399cee7424670c588c28
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058176"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Synchronisation des données hors connexion avec Azure Mobile Apps

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)

_Synchronisation hors connexion permet aux utilisateurs d’interagir avec une application mobile, affichage, ajout ou modification des données, même quand il n’est pas une connexion réseau. Les modifications sont stockées dans une base de données locale et une fois que l’appareil est en ligne, les modifications peuvent être synchronisées avec l’instance d’Azure Mobile Apps. Cet article explique comment ajouter la fonctionnalité de synchronisation hors connexion à une application Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

Le [kit SDK Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) fournit le `IMobileServiceTable` interface, qui représente les opérations qui peuvent être effectuées sur les tables stockées dans l’instance d’Azure Mobile Apps. Ces opérations se connecter directement à l’instance d’Azure Mobile Apps et échouent si l’appareil mobile n’a pas une connexion réseau.

Pour prendre en charge la synchronisation hors connexion, le kit SDK Azure Mobile Client prend en charge *synchroniser des tables*, qui sont fournis par le `IMobileServiceSyncTable` interface. Cette interface fournit la création de même, la lecture, la mise à jour, les opérations de suppression (CRUD) en tant que le `IMobileServiceTable` interface, mais les opérations de lecture ou d’écriture dans un magasin local. Le magasin local n’est pas rempli avec de nouvelles données à partir de l’instance d’Azure Mobile Apps jusqu'à ce qu’un appel à *extraction* données. De même, les données ne sont pas envoyées à l’instance d’Azure Mobile Apps jusqu'à ce qu’un appel à *push* modifications locales.

Synchronisation hors connexion inclut également la prise en charge pour la détection des conflits lorsque le même enregistrement a changé dans les deux le magasin local et dans l’instance d’Azure Mobile Apps et la résolution des conflits personnalisé. Conflits peuvent être pris en charge dans le magasin local ou dans l’instance d’Azure Mobile Apps.

Pour plus d’informations sur la synchronisation hors connexion, consultez [synchronisation des données hors connexion dans Azure Mobile Apps](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) et [activer la synchronisation hors connexion pour votre application mobile Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Installation

Le processus d’intégration de la synchronisation hors connexion entre une application Xamarin.Forms et une instance d’Azure Mobile Apps est la suivante :

1. Créez une instance d’Azure Mobile Apps. Pour plus d’informations, consultez [consommer une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Ajouter le [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) package NuGet à tous les projets dans la solution Xamarin.Forms.
1. (Facultatif) Activer l’authentification dans l’instance d’Azure Mobile Apps et de l’application Xamarin.Forms. Pour plus d’informations, consultez [authentification des utilisateurs avec Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

La section suivante fournit des instructions d’installation supplémentaires pour configurer des projets de plateforme universelle Windows (UWP) à utiliser le package NuGet de Microsoft.Azure.Mobile.Client.SQLiteStore. Aucune configuration supplémentaire n’est nécessaire pour utiliser le package NuGet de Microsoft.Azure.Mobile.Client.SQLiteStore sur iOS et Android.

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Pour utiliser SQLite sur la plateforme Windows universelle (UWP), procédez comme suit :

1. Installer le [SQLite pour la plateforme Windows universelle](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) Extension de Visual Studio dans votre environnement de développement.
1. Dans le projet UWP dans Visual Studio, avec le bouton droit cliquez sur **références > Ajouter une référence**, accédez à **Extensions** et ajoutez le **SQLite pour plateforme Windows universelle** et  **Le Runtime Visual C++ 2015 pour applications de plateforme Windows universelle** extensions du projet UWP.

## <a name="initializing-the-local-store"></a>L’initialisation du Store Local

Le magasin local doit être initialisé avant de pouvoir effectuer des opérations de table de synchronisation. Cela s’effectue dans le projet de bibliothèque de classes Portable (PCL) de la solution Xamarin.Forms :

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

Une nouvelle base de données SQLite locale est créé par la `MobileServiceSQLiteStore` classe, la condition qu’il n’existe pas déjà. Ensuite, le `DefineTable<T>` méthode crée une table dans le magasin local qui correspond aux champs dans le `TodoItem` tapez, la condition qu’il n’existe pas déjà.

Un *contexte de synchronisation* est associé un `MobileServiceClient` instance et suivi des modifications sont effectuées avec des tables de synchronisation. Le contexte de synchronisation gère une file d’attente qui conserve une liste ordonnée des opérations Create, Update et Delete (CUD) qui sera envoyée à l’instance d’Azure Mobile Apps ultérieurement. Le `IMobileServiceSyncContext.InitializeAsync()` méthode est utilisée pour associer le magasin local avec le contexte de synchronisation.

Le `todoTable` champ est un `IMobileServiceSyncTable`, et par conséquent, toutes les opérations CRUD utilisent le magasin local.

## <a name="performing-synchronization"></a>Exécution de la synchronisation

Le magasin local est synchronisé avec l’application Azure Mobile Apps instance lorsque la `SyncAsync` méthode est appelée :

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

Le `IMobileServiceSyncTable.PushAsync` méthode opère sur le contexte de synchronisation, plutôt que d’une table spécifique et envoie toutes les modifications CUD depuis le dernier envoi.

Extraction est effectuée par le `IMobileServiceSyncTable.PullAsync` méthode sur une seule table. Le premier paramètre de la `PullAsync` méthode est un nom de requête qui est utilisé uniquement sur le périphérique mobile. En fournissant une requête non null des résultats de nom dans le kit SDK Azure Mobile Client qui exécute un *synchronisation incrémentielle*, où chaque fois qu’une opération d’extraction retourne des résultats, la dernière version `updatedAt` timestamp à partir des résultats est stocké dans local tables système. Opérations d’extraction ultérieures puis uniquement récupérer les enregistrements postérieurs à cet horodatage. Vous pouvez également *synchronisation complète* peut être obtenue en passant `null` en tant que le nom de la requête, ce qui se traduit dans tous les enregistrements en cours de récupération de chaque opération d’extraction. Après toute opération de synchronisation, les données reçues sont insérées dans le magasin local.

> [!NOTE]
> Si une extraction est exécutée sur une table en attente de mises à jour locales, l’extraction commence par exécuter un push sur le contexte de synchronisation. Cela réduit les conflits entre les modifications qui sont déjà en attente et de nouvelles données à partir de l’instance d’Azure Mobile Apps.

Le `SyncAsync` inclut également une implémentation de base pour la gestion des conflits lorsque le même enregistrement a changé dans les deux le magasin local et dans l’instance d’Azure Mobile Apps. Lorsque le conflit est que les données a été mis à jour dans le magasin local et dans l’instance Azure Mobile Apps, le `SyncAsync` méthode met à jour les données dans le magasin local à partir des données stockées dans l’instance d’Azure Mobile Apps. Lorsque n’importe quel autre conflit se produit, le `SyncAsync` méthode ignore la modification locale. Cela gère le scénario où une modification locale existe pour les données qui a été supprimées de l’instance d’Azure Mobile Apps.

Dans une application de production, les développeurs doivent rédiger un personnalisé `IMobileServiceSyncHandler` mise en œuvre de la gestion des conflits qui convient à leur cas d’usage. Pour plus d’informations, consultez [utiliser l’accès concurrentiel optimiste pour la résolution des conflits](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) sur le portail Azure, et [découverte approfondie de la prise en charge hors connexion dans le Kit de développement logiciel du client géré](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) sur les blogs MSDN.

## <a name="purging-data"></a>Purge des données

Tables dans le magasin local peuvent être effacées de données avec le `IMobileServiceSyncTable.PurgeAsync` (méthode). Cette méthode prend en charge des scénarios tels que la suppression des données périmées une application n’a plus besoin. Par exemple, l’exemple d’application affiche uniquement les `TodoItem` instances qui ne sont pas terminées. Par conséquent, éléments terminés n’avez plus besoin soient stockées localement. Purge les éléments terminés à partir du magasin local peut être obtenue comme suit :

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Un appel à `PurgeAsync` déclenche également une opération push. Par conséquent, tous les éléments qui sont marquées comme terminées localement recevront à l’instance d’Azure Mobile Apps avant d’être supprimés du magasin local. Toutefois, s’il existe des opérations en attente de synchronisation avec l’instance d’Azure Mobile Apps, la purge lève une `InvalidOperationException` , sauf si le `force` paramètre est défini sur `true`. Une autre stratégie consiste à examiner le `IMobileServiceSyncContext.PendingOperations` propriété, qui retourne le nombre d’opérations qui n’ont pas été envoyées à l’instance d’Azure Mobile Apps et effectuer uniquement la purge si la propriété est égale à zéro en attente.

> [!NOTE]
> Appel `PurgeAsync` avec la `force` paramètre défini sur `true` perdrez les modifications en attente.

## <a name="initiating-synchronization"></a>Lancement de la synchronisation

Dans l’exemple d’application, le `SyncAsync` méthode est appelée via le `TodoList.OnAppearing` méthode :

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

Cela signifie que l’application tentera de se synchroniser avec l’instance d’Azure Mobile Apps lors de son démarrage.

En outre, synchronisation peut être lancée dans iOS et Android à l’aide de tirage pour actualiser sur la liste des données et sur les plateformes Windows à l’aide de la **synchronisation** bouton sur l’interface utilisateur. Pour plus d’informations, consultez [extraire pour actualiser](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment ajouter la fonctionnalité de synchronisation hors connexion à une application Xamarin.Forms. Synchronisation hors connexion permet aux utilisateurs d’interagir avec une application mobile, affichage, ajout ou modification des données, même quand il n’est pas une connexion réseau. Les modifications sont stockées dans une base de données locale et une fois que l’appareil est en ligne, les modifications peuvent être synchronisées avec l’instance d’Azure Mobile Apps.


## <a name="related-links"></a>Liens associés

- [TodoAzureAuthOfflineSync (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentification des utilisateurs avec Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Client Mobile Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
