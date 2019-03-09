---
title: Utiliser un Service Web de Windows Communication Foundation (WCF)
description: Cet article illustre l’utilisation d’un service WCF SOAP Simple Object Access Protocol () à partir d’une application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2019
ms.openlocfilehash: 600120b6ed8484399cf5fc48638ef4b129e9c406
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671960"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Utiliser un Service Web de Windows Communication Foundation (WCF)

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF est un cadre unifié de Microsoft pour la création d’applications orientées service. Il permet aux développeurs de créer des applications distribuées traitées, fiables, sécurisées et interopérables. Cet article illustre l’utilisation d’un service WCF SOAP Simple Object Access Protocol () à partir d’une application Xamarin.Forms._

WCF décrit un service avec un large éventail de marchés différents qui sont les suivantes :

- **Contrats de données** : définir les structures de données qui constituent la base pour le contenu d’un message.
- **Contrats de message** – composer des messages à partir des contrats de données existants.
- **Contrats de l’erreur** – autoriser les erreurs SOAP personnalisées à être spécifié.
- **Contrats de service** : spécifier les opérations qui prennent en charge des services et les messages requis pour l’interaction avec chaque opération. Ils spécifient également tout comportement d’erreur personnalisé qui peut être associé à des opérations sur chaque service.

Il existe des différences entre ASP.NET Web Services (ASMX) et WCF, mais il est important de comprendre que WCF prend en charge les mêmes fonctionnalités que ASMX fournit – les messages SOAP sur HTTP. Pour plus d’informations sur l’utilisation d’un service ASMX, consultez [consommation ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

> [!IMPORTANT]
> La prise en charge de la plateforme Xamarin pour WCF est limité à texte encodé des messages SOAP via HTTP/HTTPS en utilisant la `BasicHttpBinding` classe. En outre, la prise en charge WCF nécessite l’utilisation des outils disponibles uniquement dans un environnement Windows pour générer le proxy.

L’exemple d’application consomme un service WCF hébergeable qui est disponible dans le **TodoWCFService** dossier de l’exemple et est affiché dans la capture d’écran suivante :

![](wcf-images/portal.png "Exemple d’Application")

> [!NOTE]
> Dans iOS 9 et supérieur, App Transport Security (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et l’application, ce qui empêche la divulgation accidentelle d’informations sensibles. Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9, toutes les connexions seront soumis à des exigences de sécurité ATS. Si les connexions ne répondent pas à ces exigences, ils échoueront avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser la communication de ressources internet. Cela est possible en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consume-the-web-service"></a>Consommer le service web

Le service WCF fournit les opérations suivantes :

|Opération|Description|Paramètres|
|--- |--- |--- |
|GetTodoItems|Obtenir une liste de tâches|
|CreateTodoItem|Créer un élément de tâche|Un document XML sérialisé TodoItem|
|EditTodoItem|Mettre à jour une tâche|Un document XML sérialisé TodoItem|
|DeleteTodoItem|Supprimer une tâche|Un document XML sérialisé TodoItem|

Pour plus d’informations sur le modèle de données utilisé dans l’application, consultez [les données de modélisation](~/xamarin-forms/data-cloud/walkthrough.md).

Un *proxy* doit être généré pour utiliser un service WCF, ce qui permet à l’application pour se connecter au service. Le proxy est construit par la consommation des métadonnées de service qui définissent les méthodes et la configuration de service associé. Ces métadonnées sont présentées sous la forme d’un document Web Services Description Language (WSDL) généré par le service web. Le proxy peut être généré à l’aide de la Microsoft WCF Web Service Reference Provider dans Visual Studio 2017 pour ajouter une référence de service pour le service web à une bibliothèque .NET Standard. Une alternative à la création du proxy à l’aide de la Microsoft WCF Web Service Reference Provider dans Visual Studio 2017 consiste à utiliser ServiceModel Metadata Utility Tool (svcutil.exe). Pour plus d’informations, consultez [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Les classes proxy générées fournissent des méthodes de consommer les services web qui utilisent le modèle de conception de modèle de programmation asynchrone (APM). Dans ce modèle, une opération asynchrone est implémentée en tant que deux méthodes nommées *Beginnomopération* et *Endnomopération*, qui commencent et terminent l’opération asynchrone.

Le *Beginnomopération* méthode commence l’opération asynchrone et retourne un objet qui implémente le `IAsyncResult` interface. Après avoir appelé *Beginnomopération*, une application peut continuer à exécuter des instructions sur le thread appelant, tandis que l’opération asynchrone se déroule sur un thread de pool de threads.

Pour chaque appel à *Beginnomopération*, l’application doit également appeler *Endnomopération* pour obtenir les résultats de l’opération. La valeur de retour de *Endnomopération* est du même type que celui retourné par la méthode de service web synchrone. Par exemple, le `EndGetTodoItems` méthode retourne une collection de `TodoItem` instances. Le *Endnomopération* inclut également un `IAsyncResult` paramètre qui doit être définie sur l’instance retournée par l’appel correspondant à la *Beginnomopération* (méthode).

La bibliothèque parallèle de tâches (TPL) peut simplifier le processus d’utilisation d’une paire de méthodes begin/end APM en encapsulant les opérations asynchrones dans le même `Task` objet. Cette encapsulation est fournie par plusieurs surcharges de la `TaskFactory.FromAsync` (méthode).

Pour plus d’informations sur APM consultez [modèle de programmation asynchrone](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) et [TPL et Framework programmation asynchrone .NET traditionnelle](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) sur MSDN.

### <a name="create-the-todoserviceclient-object"></a>Créer l’objet TodoServiceClient

Fournit la classe proxy générée la `TodoServiceClient` (classe), qui est utilisé pour communiquer avec le service WCF via HTTP. Il offre des fonctionnalités permettant d’appeler des méthodes de service web comme des opérations asynchrones à partir d’un URI identifié l’instance de service. Pour plus d’informations sur les opérations asynchrones, consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

Le `TodoServiceClient` instance est déclarée au niveau de la classe afin que l’objet est utilisable que pendant tant que l’application doit utiliser le service WCF, comme indiqué dans l’exemple de code suivant :

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

Le `TodoServiceClient` instance est configurée avec une adresse de point de terminaison et les informations de liaison. Une liaison est utilisée pour spécifier le transport, le codage et les détails de protocole requis pour les applications et services communiquer entre eux. Le `BasicHttpBinding` Spécifie que les messages SOAP encodés en texte seront envoyés via le protocole de transport HTTP. Spécification d’une adresse de point de terminaison permet à l’application pour se connecter à différentes instances du service WCF, si plusieurs instances publiées.

Pour plus d’informations sur la configuration de la référence de service, consultez [configuration de la référence de Service](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="create-data-transfer-objects"></a>Créer des objets de transfert de données

L’exemple d’application utilise le `TodoItem` classe pour modéliser les données. Pour stocker un `TodoItem` élément dans le service web, il doit d’abord être converti vers le proxy généré `TodoItem` type. Pour cela le `ToWCFServiceTodoItem` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Cette méthode crée simplement un `TodoWCFService.TodoItem` de l’instance et définit chaque propriété à la propriété identique à partir de la `TodoItem` instance.

De même, lorsque les données sont récupérées à partir du service web, il doit être converti à partir du proxy généré `TodoItem` type à un `TodoItem` instance. Pour cela, avec la `FromWCFServiceTodoItem` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Cette méthode récupère simplement les données à partir du proxy généré `TodoItem` tapez, puis il définit dans nouvellement créé `TodoItem` instance.

### <a name="retrieve-data"></a>Récupérer des données

Le `TodoServiceClient.BeginGetTodoItems` et `TodoServiceClient.EndGetTodoItems` méthodes sont utilisées pour appeler le `GetTodoItems` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoServiceClient.EndGetTodoItems` méthode une fois le `TodoServiceClient.BeginGetTodoItems` méthode se termine, avec le `null` paramètre indiquant qu’aucune donnée n’est passée dans le `BeginGetTodoItems` déléguer. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

Le `TodoServiceClient.EndGetTodoItems` méthode retourne un `ObservableCollection` de `TodoWCFService.TodoItem` instances, qui est ensuite convertie en un `List` de `TodoItem` instances pour l’affichage.

### <a name="create-data"></a>Créer des données

Le `TodoServiceClient.BeginCreateTodoItem` et `TodoServiceClient.EndCreateTodoItem` méthodes sont utilisées pour appeler le `CreateTodoItem` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoServiceClient.EndCreateTodoItem` méthode une fois le `TodoServiceClient.BeginCreateTodoItem` méthode se termine, avec le `todoItem` paramètre en cours de données qui a été passées dans le `BeginCreateTodoItem` délégué pour spécifier le `TodoItem` doit être créé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception une `FaultException` en cas d’échec créer le `TodoItem`, qui est géré par l’application.

### <a name="update-data"></a>mettre à jour des données

Le `TodoServiceClient.BeginEditTodoItem` et `TodoServiceClient.EndEditTodoItem` méthodes sont utilisées pour appeler le `EditTodoItem` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoServiceClient.EndEditTodoItem` méthode une fois le `TodoServiceClient.BeginCreateTodoItem` méthode se termine, avec le `todoItem` paramètre en cours de données qui a été passées dans le `BeginEditTodoItem` délégué pour spécifier le `TodoItem` à mettre à jour par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception une `FaultException` si elle ne parvient pas à localiser ou de mettre à jour le `TodoItem`, qui est géré par l’application.

### <a name="delete-data"></a>Supprimer des données

Le `TodoServiceClient.BeginDeleteTodoItem` et `TodoServiceClient.EndDeleteTodoItem` méthodes sont utilisées pour appeler le `DeleteTodoItem` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoServiceClient.EndDeleteTodoItem` méthode une fois le `TodoServiceClient.BeginDeleteTodoItem` méthode se termine, avec le `id` paramètre en cours de données qui a été passées dans le `BeginDeleteTodoItem` délégué pour spécifier le `TodoItem` doit être supprimé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception une `FaultException` si elle ne parvient pas à localiser ou de supprimer le `TodoItem`, qui est géré par l’application.

## <a name="related-links"></a>Liens connexes

- [TodoWCF (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [Guide pratique pour Créer un Client Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Service Model Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
