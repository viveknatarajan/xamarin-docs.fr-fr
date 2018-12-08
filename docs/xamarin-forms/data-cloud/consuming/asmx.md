---
title: Utilisation d’un Service Web ASP.NET (ASMX)
description: Cet article illustre l’utilisation d’un service ASMX SOAP à partir d’une application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1fa2fee36619a2a443d84b861b2473a1f616ed0e
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055139"
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Utilisation d’un Service Web ASP.NET (ASMX)

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)

_ASMX offre la possibilité de créer des services web qui envoient des messages à l’aide de l’accès protocole SOAP (Simple Object). SOAP est un protocole indépendant de la plateforme et indépendant du langage pour la création et l’accès aux services web. Consommateurs d’un service ASMX n’avez pas besoin de savoir quoi que ce soit sur la plateforme, le modèle d’objet ou le langage de programmation utilisé pour implémenter le service. Ils doivent uniquement comprendre comment envoyer et recevoir des messages SOAP. Cet article illustre l’utilisation d’un service ASMX SOAP à partir d’une application Xamarin.Forms._

Un message SOAP est un document XML contenant les éléments suivants :

- Un élément racine nommé *enveloppe* qui identifie le document XML sous forme de message SOAP.
- Facultatif *en-tête* élément qui contient des informations spécifiques aux applications telles que les données de l’authentification. Si le *en-tête* élément est présent il doit être le premier élément enfant de le *enveloppe* élément.
- Obligatoire *corps* élément qui contient le message SOAP destiné au destinataire.
- Facultatif *pannes* élément qui est utilisé pour indiquer les messages d’erreur. Si le *erreur* élément est présent, il doit être un élément enfant de le *corps* élément.

SOAP peut opérer sur plusieurs protocoles de transport, y compris HTTP, SMTP, TCP et UDP. Toutefois, un service ASMX ne peut agir que sur HTTP. La plateforme Xamarin prend en charge les implémentations SOAP 1.1 standard sur HTTP, et cela inclut la prise en charge pour la plupart des configurations de service ASMX standards.

Vous trouverez des instructions sur la configuration du service ASMX dans le fichier Lisez-moi qui accompagne l’exemple d’application. Toutefois, lorsque l’exemple d’application est exécuté, il se connecte à un service ASMX hébergé de Xamarin qui fournit l’accès en lecture seule aux données, comme illustré dans la capture d’écran suivante :

![](asmx-images/portal.png "Exemple d’Application")

> [!NOTE]
> Dans iOS 9 et supérieur, App Transport Security (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et l’application, ce qui empêche la divulgation accidentelle d’informations sensibles. Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9, toutes les connexions seront soumis à des exigences de sécurité ATS. Si les connexions ne répondent pas à ces exigences, ils échoueront avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser la communication de ressources internet. Cela est possible en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Consommation du Service Web

Le service ASMX fournit les opérations suivantes :

|Opération|Description|Paramètres|
|--- |--- |--- |
|GetTodoItems|Obtenir une liste de tâches|
|CreateTodoItem|Créer un élément de tâche|Un document XML sérialisé TodoItem|
|EditTodoItem|Mettre à jour une tâche|Un document XML sérialisé TodoItem|
|DeleteTodoItem|Supprimer une tâche|Un document XML sérialisé TodoItem|

Pour plus d’informations sur le modèle de données utilisé dans l’application, consultez [les données de modélisation](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> L’exemple d’application consomme le service ASMX hébergé de Xamarin qui fournit l’accès en lecture seule au service web. Par conséquent, les opérations créent, mettre à jour et supprimer des données ne modifiera pas les données utilisées dans l’application. Toutefois, une version principale du service ASMX est disponible dans le **TodoASMXService** dossier dans l’exemple d’application qui accompagne cet article. Cette version principale d’autorisés par le service ASMX complètes créer, mettre à jour, lire et supprimer l’accès aux données.

Un *proxy* doit être générée pour consommer le service ASMX, ce qui permet à l’application pour se connecter au service. Le proxy est construit par la consommation des métadonnées de service qui définissant les méthodes et la configuration de service associé. Ces métadonnées sont présentées sous la forme d’un document Web Services Description Language (WSDL) généré par le service web. Le proxy est créé en ajoutant une référence web pour le service web pour les projets spécifiques à la plateforme.

Les classes proxy générées fournissent des méthodes pour la consommation du service web qui utilisent le modèle de conception de modèle de programmation asynchrone (APM). Dans ce modèle, une opération asynchrone est implémentée en tant que deux méthodes nommées *Beginnomopération* et *Endnomopération*, qui commencent et terminent l’opération asynchrone.

Le *Beginnomopération* méthode commence l’opération asynchrone et retourne un objet qui implémente le `IAsyncResult` interface. Après avoir appelé *Beginnomopération*, une application peut continuer à exécuter des instructions sur le thread appelant, tandis que l’opération asynchrone se déroule sur un thread de pool de threads.

Pour chaque appel à *Beginnomopération*, l’application doit également appeler *Endnomopération* pour obtenir les résultats de l’opération. La valeur de retour de *Endnomopération* est du même type que celui retourné par la méthode de service web synchrone. Par exemple, le `EndGetTodoItems` méthode retourne une collection de `TodoItem` instances. Le *Endnomopération* inclut également un `IAsyncResult` paramètre qui doit être définie sur l’instance retournée par l’appel correspondant à la *Beginnomopération* (méthode).

La bibliothèque parallèle de tâches (TPL) peut simplifier le processus d’utilisation d’une paire de méthodes begin/end APM en encapsulant les opérations asynchrones dans le même `Task` objet. Cette encapsulation est fournie par plusieurs surcharges de la `TaskFactory.FromAsync` (méthode).

Pour plus d’informations sur APM consultez [modèle de programmation asynchrone](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) et [TPL et Framework programmation asynchrone .NET traditionnelle](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) sur MSDN.

### <a name="creating-the-todoservice-object"></a>Création de l’objet TodoService

Fournit la classe proxy générée la `TodoService` (classe), qui est utilisé pour communiquer avec le service ASMX via HTTP. Il offre des fonctionnalités permettant d’appeler des méthodes de service web comme des opérations asynchrones à partir d’un URI identifié l’instance de service. Pour plus d’informations sur les opérations asynchrones, consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

Le `TodoService` instance est déclarée au niveau de la classe afin que l’objet est utilisable que pendant tant que l’application doit consommer le service ASMX, comme illustré dans l’exemple de code suivant :

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

Le `TodoService` constructeur accepte un paramètre de chaîne facultatif qui spécifie l’URL de l’instance de service ASMX. Cela permet à l’application pour se connecter à différentes instances du service ASMX, autant que plusieurs instances publiées.

### <a name="creating-data-transfer-objects"></a>Création d’objets transfert de données

L’exemple d’application utilise le `TodoItem` classe pour modéliser les données. Pour stocker un `TodoItem` élément dans le service web, il doit d’abord être converti vers le proxy généré `TodoItem` type. Pour cela le `ToASMXServiceTodoItem` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Cette méthode crée simplement un `ASMService.TodoItem` de l’instance et définit chaque propriété à la propriété identique à partir de la `TodoItem` instance.

De même, lorsque les données sont récupérées à partir du service web, il doit être converti à partir du proxy généré `TodoItem` type à un `TodoItem` instance. Pour cela, avec la `FromASMXServiceTodoItem` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Cette méthode récupère simplement les données à partir du proxy généré `TodoItem` tapez, puis il définit dans nouvellement créé `TodoItem` instance.

### <a name="retrieving-data"></a>Récupération de données

Le `TodoService.BeginGetTodoItems` et `TodoService.EndGetTodoItems` méthodes sont utilisées pour appeler le `GetTodoItems` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoService.EndGetTodoItems` méthode une fois le `TodoService.BeginGetTodoItems` méthode se termine, avec le `null` paramètre indiquant qu’aucune donnée n’est passée dans le `BeginGetTodoItems` déléguer. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

Le `TodoService.EndGetTodoItems` méthode retourne un tableau de `ASMXService.TodoItem` instances, qui est ensuite convertie en un `List` de `TodoItem` instances pour l’affichage.

### <a name="creating-data"></a>Création de données

Le `TodoService.BeginCreateTodoItem` et `TodoService.EndCreateTodoItem` méthodes sont utilisées pour appeler le `CreateTodoItem` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoService.EndCreateTodoItem` méthode une fois le `TodoService.BeginCreateTodoItem` méthode se termine, avec le `todoItem` paramètre en cours de données qui a été passées dans le `BeginCreateTodoItem` délégué pour spécifier le `TodoItem` doit être créé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception une `SoapException` en cas d’échec créer le `TodoItem`, qui est géré par l’application.

### <a name="updating-data"></a>Mise à jour des données

Le `TodoService.BeginEditTodoItem` et `TodoService.EndEditTodoItem` méthodes sont utilisées pour appeler le `EditTodoItem` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoService.EndEditTodoItem` méthode une fois le `TodoService.BeginCreateTodoItem` méthode se termine, avec le `todoItem` paramètre en cours de données qui a été passées dans le `BeginEditTodoItem` délégué pour spécifier le `TodoItem` à mettre à jour par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception une `SoapException` si elle ne parvient pas à localiser ou de mettre à jour le `TodoItem`, qui est géré par l’application.

### <a name="deleting-data"></a>Suppression de données

Le `TodoService.BeginDeleteTodoItem` et `TodoService.EndDeleteTodoItem` méthodes sont utilisées pour appeler le `DeleteTodoItem` opération fournie par le service web. Ces méthodes asynchrones sont encapsulées dans un `Task` de l’objet, comme indiqué dans l’exemple de code suivant :

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui exécute le `TodoService.EndDeleteTodoItem` méthode une fois le `TodoService.BeginDeleteTodoItem` méthode se termine, avec le `id` paramètre en cours de données qui a été passées dans le `BeginDeleteTodoItem` délégué pour spécifier le `TodoItem` doit être supprimé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception une `SoapException` si elle ne parvient pas à localiser ou de supprimer le `TodoItem`, qui est géré par l’application.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment consommer un service web ASMX à partir d’une application Xamarin.Forms. ASMX offre la possibilité de créer des services web qui envoient des messages via HTTP à l’aide de SOAP. Consommateurs d’un service ASMX n’avez pas besoin de savoir quoi que ce soit sur la plateforme, le modèle d’objet ou le langage de programmation utilisé pour implémenter le service. Ils doivent uniquement comprendre comment envoyer et recevoir des messages SOAP.


## <a name="related-links"></a>Liens associés

- [TodoASMX (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
