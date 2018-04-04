---
title: Utilisation d’un Service Web ASP.NET (ASMX)
description: ASMX fournit la possibilité de créer des services web qui envoient des messages à l’aide de l’accès protocole SOAP (Simple Object). SOAP est un protocole indépendant de la plateforme et indépendant du langage pour la création et l’accès aux services web. Les consommateurs d’un service ASMX n’avez pas besoin de connaître quoi que ce soit sur la plateforme, modèle d’objet ou langage de programmation utilisé pour implémenter le service. Il leur suffit de comprendre comment envoyer et recevoir des messages SOAP. Cet article explique comment utiliser un service ASMX SOAP à partir d’une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: c45f0de039abc3f98b7c269f183e2883a495910b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Utilisation d’un Service Web ASP.NET (ASMX)

_ASMX fournit la possibilité de créer des services web qui envoient des messages à l’aide de l’accès protocole SOAP (Simple Object). SOAP est un protocole indépendant de la plateforme et indépendant du langage pour la création et l’accès aux services web. Les consommateurs d’un service ASMX n’avez pas besoin de connaître quoi que ce soit sur la plateforme, modèle d’objet ou langage de programmation utilisé pour implémenter le service. Il leur suffit de comprendre comment envoyer et recevoir des messages SOAP. Cet article explique comment utiliser un service ASMX SOAP à partir d’une application de Xamarin.Forms._

Un message SOAP est un document XML contenant les éléments suivants :

- Un élément racine nommé *enveloppe* qui identifie le document XML comme un message SOAP.
- Facultatif *en-tête* élément qui contient des informations spécifiques à l’application, telles que les données d’authentification. Si le *en-tête* élément est présent il doit être le premier élément enfant de le *enveloppe* élément.
- Une valeur obligatoire *corps* élément qui contient le message SOAP destiné au destinataire.
- Facultatif *erreur* élément qui est utilisé pour indiquer les messages d’erreur. Si le *erreur* élément est présent, il doit être un élément enfant de le *corps* élément.

SOAP peut opérer sur plusieurs protocoles de transport, y compris HTTP, SMTP, TCP et UDP. Toutefois, un service ASMX peut uniquement fonctionner sur HTTP. La plateforme Xamarin prend en charge les implémentations standards SOAP 1.1 sur HTTP, et cela inclut la prise en charge pour la plupart des configurations de service ASMX standards.

Vous trouverez des instructions sur la configuration du service ASMX dans le fichier Lisezmoi qui accompagne l’exemple d’application. Toutefois, lorsque l’application d’exemple est exécutée, il se connecte à un service hébergé Xamarin ASMX qui fournit l’accès en lecture seule aux données, comme indiqué dans la capture d’écran suivante :

![](asmx-images/portal.png "Exemple d’Application")

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Utilisation du Service Web

Le service ASMX fournit les opérations suivantes :

|Opération|Description|Paramètres|
|--- |--- |--- |
|GetTodoItems|Obtenir la liste des tâches|
|CreateTodoItem|Créer un nouvel élément d’action|Un code XML sérialisé TodoItem|
|EditTodoItem|Mettre à jour un élément action|Un code XML sérialisé TodoItem|
|DeleteTodoItem|Supprimer un élément d’action|Un code XML sérialisé TodoItem|

Pour plus d’informations sur le modèle de données utilisé dans l’application, consultez [modélisation de données](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> L’exemple d’application consomme le service ASMX de hébergé de Xamarin qui fournit l’accès en lecture seule au service web. Par conséquent, les opérations créent, mettre à jour et supprimer des données ne modifiera pas les données utilisées dans l’application. Toutefois, une version principale du service ASMX est disponible dans le **TodoASMXService** dossier dans l’exemple d’application qui l’accompagne. Cette version principale d’autorisés par le service ASMX complètes créer, mettre à jour, lire et supprimer l’accès aux données.

A *proxy* doivent être générées pour consommer le service ASMX, ce qui permet de se connecter au service. Le proxy est construit par la consommation des métadonnées de service qui définit les méthodes et la configuration de service associé. Ces métadonnées sont exposées sous la forme d’un document Web Services Description Language (WSDL) généré par le service web. Le proxy est créé en ajoutant une référence web pour le service web pour les projets spécifiques à la plateforme.

Les classes de proxy généré fournissent des méthodes pour utiliser le service web qui utilisent le modèle de conception de modèle de programmation asynchrone (APM). Dans ce modèle, une opération asynchrone est implémentée comme deux méthodes nommées *BeginOperationName* et *EndOperationName*, qui commencent et terminent l’opération asynchrone.

Le *BeginOperationName* méthode commence l’opération asynchrone et retourne un objet qui implémente le `IAsyncResult` interface. Après avoir appelé *BeginOperationName*, une application peut continuer à exécuter des instructions sur le thread appelant, tandis que l’opération asynchrone s’exécute sur un thread de pool de threads.

Pour chaque appel à *BeginOperationName*, l’application doit également appeler *EndOperationName* pour obtenir les résultats de l’opération. La valeur de retour de *EndOperationName* est du même type que celui retourné par la méthode de service web synchrone. Par exemple, le `EndGetTodoItems` méthode retourne une collection de `TodoItem` instances. Le *EndOperationName* inclut également un `IAsyncResult` paramètre qui doit être définie sur l’instance retournée par l’appel correspondant à la *BeginOperationName* (méthode).

La bibliothèque parallèle de tâches (TPL) peut simplifier le processus d’utilisation d’une paire de méthodes begin/end APM en encapsulant les opérations asynchrones dans le même `Task` objet. Cette encapsulation est fournie par plusieurs surcharges de la `TaskFactory.FromAsync` (méthode).

Pour plus d’informations sur APM consultez [modèle de programmation asynchrone](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) et [bibliothèque parallèle de tâches et le Framework programmation asynchrone .NET traditionnelle](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) sur MSDN.

### <a name="creating-the-todoservice-object"></a>Création de l’objet TodoService

La classe proxy générée fournit le `TodoService` (classe), qui est utilisé pour communiquer avec le service ASMX sur HTTP. Il fournit des fonctionnalités permettant d’appeler les méthodes de service web comme opérations asynchrones à partir d’un URI identifié à l’instance de service. Pour plus d’informations sur les opérations asynchrones, consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

Le `TodoService` instance est déclarée au niveau de la classe afin que l’objet réside pour tant que l’application a besoin consommer le service ASMX, comme indiqué dans l’exemple de code suivant :

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

Le `TodoService` constructeur accepte un paramètre de chaîne facultatif qui spécifie l’URL de l’instance de service ASMX. Cela permet à l’application pour se connecter à différentes instances du service ASMX, condition qu’il existe plusieurs instances publiées.

### <a name="creating-data-transfer-objects"></a>Création d’objets transfert de données

L’exemple d’application utilise le `TodoItem` classe aux données du modèle. Pour stocker un `TodoItem` élément dans le service web, il doit d’abord être converti vers le proxy généré `TodoItem` type. Cela est accompli par le `ToASMXServiceTodoItem` (méthode), comme indiqué dans l’exemple de code suivant :

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

Cette méthode crée simplement un `ASMService.TodoItem` d’instance et définit chaque propriété à la propriété identique à partir de la `TodoItem` instance.

De même, lorsque les données sont récupérées à partir du service web, elle doit être convertie à partir du proxy généré `TodoItem` type à un `TodoItem` instance. Pour cela, avec la `FromASMXServiceTodoItem` méthode, comme indiqué dans l’exemple de code suivant :

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

Cette méthode récupère simplement les données à partir du proxy généré `TodoItem` de type et le définit dans nouvellement créé `TodoItem` instance.

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoService.EndGetTodoItems` méthode une fois la `TodoService.BeginGetTodoItems` méthode se termine, avec la `null` paramètre indiquant qu’aucune donnée n’est passée dans le `BeginGetTodoItems` déléguer. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoService.EndCreateTodoItem` méthode une fois la `TodoService.BeginCreateTodoItem` méthode se termine, avec la `todoItem` paramètre en cours de données qui a été passées dans le `BeginCreateTodoItem` délégué pour spécifier le `TodoItem` doit être créé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception un `SoapException` si elle ne peut pas créer le `TodoItem`, qui est géré par l’application.

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoService.EndEditTodoItem` méthode une fois la `TodoService.BeginCreateTodoItem` méthode se termine, avec la `todoItem` paramètre en cours de données qui a été passées dans le `BeginEditTodoItem` délégué pour spécifier le `TodoItem` être mis à jour par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La méthode web service lève une `SoapException` en cas d’échec rechercher ou de mettre à jour le `TodoItem`, qui est géré par l’application.

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

Le `Task.Factory.FromAsync` méthode crée un `Task` qui s’exécute le `TodoService.EndDeleteTodoItem` méthode une fois la `TodoService.BeginDeleteTodoItem` méthode se termine, avec la `id` paramètre en cours de données qui a été passées dans le `BeginDeleteTodoItem` délégué pour spécifier le `TodoItem` doit être supprimé par le service web. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

La service web lève une exception un `SoapException` en cas d’échec localiser ou de supprimer le `TodoItem`, qui est géré par l’application.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment consommer un service web ASMX à partir d’une application de Xamarin.Forms. ASMX fournit la possibilité de créer des services web qui envoient des messages sur HTTP à l’aide de SOAP. Les consommateurs d’un service ASMX n’avez pas besoin de connaître quoi que ce soit sur la plateforme, modèle d’objet ou langage de programmation utilisé pour implémenter le service. Il leur suffit de comprendre comment envoyer et recevoir des messages SOAP.


## <a name="related-links"></a>Liens associés

- [TodoASMX (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
