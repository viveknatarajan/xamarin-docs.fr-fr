---
title: Utiliser un Service Web de Windows Communication Foundation (WCF)
description: "WCF est un cadre unifié de Microsoft pour la création d’applications orientées service. Il permet aux développeurs de créer des applications distribuées traitées, fiables, sécurisées et interopérables. Cet article montre comment consommer un service WCF accès protocole SOAP (Simple Object) à partir d’une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 372ea9a20e9f67c41c74fb01afb8110ee741a709
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Utiliser un Service Web de Windows Communication Foundation (WCF)

_WCF est un cadre unifié de Microsoft pour la création d’applications orientées service. Il permet aux développeurs de créer des applications distribuées traitées, fiables, sécurisées et interopérables. Cet article montre comment consommer un service WCF accès protocole SOAP (Simple Object) à partir d’une application de Xamarin.Forms._

WCF décrit un service avec un large éventail de contrats différents qui sont les suivantes :

- **Contrats de données** : définir les structures de données qui constituent la base pour le contenu d’un message.
- **Contrats de message** – composer des messages à partir des contrats de données existants.
- **Contrats d’erreur** – autoriser les erreurs SOAP personnalisées être spécifié.
- **Contrats de service** : spécifier les opérations qui prennent en charge des services et les messages nécessaires pour interagir avec chaque opération. Ils indiquent également tout comportement d’erreur personnalisée qui peut être associé à des opérations sur chaque service.

Il existe des différences entre les Services de Web ASP.NET (ASMX) et WCF, mais il est important de comprendre que WCF prend en charge les mêmes fonctionnalités offrant ASMX – les messages SOAP sur HTTP. Pour plus d’informations sur l’utilisation d’un service ASMX, consultez [consommation ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

En règle générale, la plateforme Xamarin prend en charge le même sous-ensemble de côté client de WCF est fourni avec le runtime Silverlight. Cela inclut les implémentations d’encodage et au protocole courantes de WCF, codée au format texte des messages SOAP sur HTTP à l’aide du protocole de transport la `BasicHttpBinding` classe. En outre, la prise en charge de WCF requiert l’utilisation d’outils disponibles uniquement dans un environnement Windows pour générer le proxy.

Vous trouverez des instructions sur la configuration du service WCF dans le fichier Lisezmoi qui accompagne l’exemple d’application. Toutefois, lors de l’exécution de l’exemple d’application qu’il se connectera à un service WCF hébergé par Xamarin qui fournit l’accès en lecture seule aux données, comme indiqué dans la capture d’écran suivante :

![](wcf-images/portal.png "Exemple d’Application")

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Utilisation du Service Web

Le service WCF fournit les opérations suivantes :

<table>
  <thead>
    <tr>
      <th>Opération</th>
      <th>Description</th>
      <th>Paramètres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GetTodoItems</td>
      <td>Obtenir la liste des tâches</td>
      <td></td>
    </tr>
    <tr>
      <td>CreateTodoItem</td>
      <td>Créer un nouvel élément d’action</td>
      <td>Un code XML sérialisé <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>EditTodoItem</td>
      <td>Mettre à jour un élément action</td>
      <td>Un code XML sérialisé <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>DeleteTodoItem</td>
      <td>Supprimer un élément d’action</td>
      <td>Un code XML sérialisé <code>TodoItem</code></td>
    </tr>
  </tbody>
</table>

Pour plus d’informations sur le modèle de données utilisé dans l’application, consultez [modélisation de données](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> **Remarque**: l’exemple d’application utilise le service WCF hébergé par Xamarin qui fournit l’accès en lecture seule au service web. Par conséquent, les opérations créent, mettre à jour et supprimer des données ne modifiera pas les données utilisées dans l’application. Toutefois, une version principale du service ASMX est disponible dans le **TodoWCFService** dossier dans l’exemple d’application qui l’accompagne. Cette version principale d’autorisés par le service WCF complètes créer, mettre à jour, lire et supprimer l’accès aux données.

A *proxy* doit être généré pour utiliser un service WCF, ce qui permet de se connecter au service. Le proxy est construit par la consommation des métadonnées de service qui définissent les méthodes et la configuration de service associé. Ces métadonnées sont exposées sous la forme d’un document Web Services Description Language (WSDL) généré par le service web. Le proxy peut être généré à l’aide du fournisseur Microsoft WCF Web Service référence dans Visual Studio 2017 pour ajouter une référence de service pour le service web dans une bibliothèque Standard de .NET. Une alternative à la création du proxy à l’aide du fournisseur Microsoft WCF Web Service référence dans Visual Studio 2017 est d’utiliser le service Model Metadata Utility Tool (svcutil.exe). Pour plus d’informations, consultez [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Les classes proxy généré fournissent des méthodes pour utiliser les services web qui utilisent le modèle de conception de modèle de programmation asynchrone (APM). Dans ce modèle, une opération asynchrone est implémentée comme deux méthodes nommées *BeginOperationName* et *EndOperationName*, qui commencent et terminent l’opération asynchrone.

Le *BeginOperationName* méthode commence l’opération asynchrone et retourne un objet qui implémente le `IAsyncResult` interface. Après avoir appelé *BeginOperationName*, une application peut continuer à exécuter des instructions sur le thread appelant, tandis que l’opération asynchrone s’exécute sur un thread de pool de threads.

Pour chaque appel à *BeginOperationName*, l’application doit également appeler *EndOperationName* pour obtenir les résultats de l’opération. La valeur de retour de *EndOperationName* est du même type que celui retourné par la méthode de service web synchrone. Par exemple, le `EndGetTodoItems` méthode retourne une collection de `TodoItem` instances. Le *EndOperationName* inclut également un `IAsyncResult` paramètre qui doit être définie sur l’instance retournée par l’appel correspondant à la *BeginOperationName* (méthode).

La bibliothèque parallèle de tâches (TPL) peut simplifier le processus d’utilisation d’une paire de méthodes begin/end APM en encapsulant les opérations asynchrones dans le même `Task` objet. Cette encapsulation est fournie par plusieurs surcharges de la `TaskFactory.FromAsync` (méthode).

Pour plus d’informations sur APM consultez [modèle de programmation asynchrone](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) et [bibliothèque parallèle de tâches et le Framework programmation asynchrone .NET traditionnelle](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) sur MSDN.

### <a name="creating-the-todoserviceclient-object"></a>Création de l’objet TodoServiceClient

La classe proxy générée fournit le `TodoServiceClient` (classe), qui est utilisé pour communiquer avec le service WCF via HTTP. Il fournit des fonctionnalités permettant d’appeler les méthodes de service web comme opérations asynchrones à partir d’un URI identifié à l’instance de service. Pour plus d’informations sur les opérations asynchrones, consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

Le `TodoServiceClient` instance est déclarée au niveau de la classe afin que l’objet réside pour tant que l’application a besoin utiliser le service WCF, comme indiqué dans l’exemple de code suivant :

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

Le `TodoServiceClient` instance est configurée avec une adresse de point de terminaison et les informations de liaison. Une liaison est utilisée pour spécifier le transport, le codage et les détails de protocole requis pour les applications et services communiquer entre eux. Le `BasicHttpBinding` Spécifie que les messages SOAP codée au format texte seront envoyés via le protocole de transport HTTP. Spécification d’une adresse de point de terminaison permet à l’application pour se connecter à différentes instances du service WCF, condition qu’il existe plusieurs instances publiées.

Pour plus d’informations sur la configuration de la référence de service, consultez [configuration de la référence de Service](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="creating-data-transfer-objects"></a>Création d’objets transfert de données

L’exemple d’application utilise le `TodoItem` classe aux données du modèle. Pour stocker un `TodoItem` élément dans le service web, il doit d’abord être converti vers le proxy généré `TodoItem` type. Cela est accompli par le `ToWCFServiceTodoItem` (méthode), comme indiqué dans l’exemple de code suivant :

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Cette méthode crée simplement un `TodoWCFService.TodoItem` d’instance et définit chaque propriété à la propriété identique à partir de la `TodoItem` instance.

De même, lorsque les données sont récupérées à partir du service web, elle doit être convertie à partir du proxy généré `TodoItem` type à un `TodoItem` instance. Pour cela, avec la `FromWCFServiceTodoItem` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Cette méthode récupère simplement les données à partir du proxy généré `TodoItem` de type et le définit dans nouvellement créé `TodoItem` instance.

### <a name="retrieving-data"></a>Récupération de données

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

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoServiceClient.EndGetTodoItems` méthode une fois la `TodoServiceClient.BeginGetTodoItems` méthode se termine, avec la `null` paramètre indiquant qu’aucune donnée n’est passée dans le `BeginGetTodoItems` déléguer. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

Le `TodoServiceClient.EndGetTodoItems` méthode retourne un `ObservableCollection` de `TodoWCFService.TodoItem` instances, qui est ensuite convertie en un `List` de `TodoItem` instances pour l’affichage.

### <a name="creating-data"></a>Création de données

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoServiceClient.EndCreateTodoItem` méthode une fois la `TodoServiceClient.BeginCreateTodoItem` méthode se termine, avec la `todoItem` paramètre en cours de données qui a été passées dans le `BeginCreateTodoItem` délégué pour spécifier le `TodoItem` doit être créé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception un `FaultException` si elle ne peut pas créer le `TodoItem`, qui est géré par l’application.

### <a name="updating-data"></a>Mise à jour des données

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoServiceClient.EndEditTodoItem` méthode une fois la `TodoServiceClient.BeginCreateTodoItem` méthode se termine, avec la `todoItem` paramètre en cours de données qui a été passées dans le `BeginEditTodoItem` délégué pour spécifier le `TodoItem` être mis à jour par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La méthode web service lève une `FaultException` en cas d’échec rechercher ou de mettre à jour le `TodoItem`, qui est géré par l’application.

### <a name="deleting-data"></a>Suppression de données

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoServiceClient.EndDeleteTodoItem` méthode une fois la `TodoServiceClient.BeginDeleteTodoItem` méthode se termine, avec la `id` paramètre en cours de données qui a été passées dans le `BeginDeleteTodoItem` délégué pour spécifier le `TodoItem` doit être supprimé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception un `FaultException` en cas d’échec localiser ou de supprimer le `TodoItem`, qui est géré par l’application.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment consommer un service WCF SOAP à partir d’une application de Xamarin.Forms. En règle générale, la plateforme Xamarin prend en charge le même sous-ensemble de côté client de WCF est fourni avec le runtime Silverlight. Cela inclut les implémentations d’encodage et au protocole courantes de WCF, codée au format texte des messages SOAP sur HTTP à l’aide du protocole de transport la `BasicHttpBinding` classe. En outre, la prise en charge de WCF requiert l’utilisation d’outils disponibles uniquement dans un environnement Windows pour générer le proxy.


## <a name="related-links"></a>Liens associés

- [TodoWCF (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
