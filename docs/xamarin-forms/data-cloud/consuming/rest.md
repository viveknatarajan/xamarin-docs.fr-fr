---
title: Utilisation d’un Service Web RESTful
description: Intégration d’un service web dans une application est un scénario courant. Cet article montre comment utiliser un service web RESTful à partir d’une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 48b81c5beb1643501c69e5de1ea4f4197d587001
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-a-restful-web-service"></a>Utilisation d’un Service Web RESTful

_Intégration d’un service web dans une application est un scénario courant. Cet article montre comment utiliser un service web RESTful à partir d’une application de Xamarin.Forms._

Transfert de REST (Representational State) est un style d’architecture pour la création de services web. Demandes REST sont effectuées via HTTP utilisent les mêmes verbes HTTP qui utilisent de navigateurs web pour récupérer des pages web et envoient des données à des serveurs. Les verbes sont :

- **OBTENIR** : cette opération est utilisée pour récupérer des données à partir du service web.
- **POST** – cette opération permet de créer un nouvel élément de données sur le service web.
- **PUT** : cette opération est utilisée pour mettre à jour un élément de données sur le service web.
- **CORRECTIF** : cette opération est utilisée pour mettre à jour un élément de données sur le service web en décrivant un ensemble d’instructions sur la façon dont l’élément doit être modifié. Ce verbe n’est pas utilisé dans l’exemple d’application.
- **SUPPRIMER** : cette opération est utilisée pour supprimer un élément de données sur le service web.

API qui adhèrent à REST de service Web sont appelées API RESTful et sont définies à l’aide de :

- Un URI de base.
- Méthodes HTTP, telles que GET, POST, PUT, PATCH ou DELETE.
- Un type de média pour les données, tels que JavaScript Objet Notation (JSON).

Services web rESTful utilisent généralement des messages JSON pour retourner des données au client. JSON est un format d’échange de données textuelles que produit compact charges utiles, ce qui entraîne réduit les besoins en bande passante lors de l’envoi de données. L’exemple d’application utilise open source [NewtonSoft JSON.NET bibliothèque](http://www.newtonsoft.com/json) pour sérialiser et désérialiser les messages.

La simplicité de REST a permis de rendre la méthode principale pour accéder aux services web dans les applications mobiles.

Vous trouverez des instructions sur la configuration du service REST dans le fichier Lisezmoi qui accompagne l’exemple d’application. Toutefois, lorsque l’application d’exemple est exécutée, il se connecte à un service hébergé par Xamarin REST qui fournit l’accès en lecture seule aux données, comme indiqué dans la capture d’écran suivante :

![](rest-images/portal.png "Exemple d’Application")

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
>
>ATS peuvent être ignorées si elle n’est pas possible d’utiliser le **HTTPS** de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Utilisation du Service Web

Le service REST est écrit à l’aide d’ASP.NET Core et effectue les opérations suivantes :

|Opération|Méthode HTTP|URI relatif|Paramètres|
|--- |--- |--- |--- |
|Obtenir la liste des tâches|GET|/api/todoitems/|
|Créer un nouvel élément d’action|POST|/api/todoitems/|TodoItem au format JSON|
|Mettre à jour un élément action|PUT|/api/todoitems/|TodoItem au format JSON|
|Supprimer un élément d’action|SUPPR|/api/todoitems/{id}|

La majorité des URI incluent le `TodoItem` ID dans le chemin d’accès. Par exemple, pour supprimer la `TodoItem` dont l’ID est `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, le client envoie une demande de suppression pour `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Pour plus d’informations sur le modèle de données utilisé dans l’exemple d’application, consultez [modélisation de données](~/xamarin-forms/data-cloud/walkthrough.md).

Lorsque l’infrastructure API Web reçoit une demande, il achemine la demande à une action. Ces actions sont les méthodes publiques simplement la `TodoItemsController` classe. Le framework utilise une table de routage pour déterminer l’action à appeler en réponse à une demande, qui est indiquée dans l’exemple de code suivant :

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

La table de routage contient un modèle d’itinéraire, et lorsque l’infrastructure API Web reçoit une requête HTTP, il essaie de faire correspondre l’URI par rapport au modèle d’itinéraire dans la table de routage. Si une correspondance itinéraire Impossible de trouver que le client reçoit l’erreur 404 (introuvable). Si un itinéraire correspondant est trouvé, API Web sélectionne le contrôleur et l’action comme suit :

- Pour rechercher le contrôleur, API Web ajoute « controller » à la valeur de la *{controller}* variable.
- Pour trouver l’action, API Web ressemble à la méthode HTTP et examine les actions de contrôleur sont décorées avec la même méthode HTTP en tant qu’attribut.
- Le *{id}* variable d’espace réservé est mappé à un paramètre d’action.

Le service REST utilise l’authentification de base. Pour plus d’informations, consultez [l’authentification d’un service web RESTful](~/xamarin-forms/data-cloud/authentication/rest.md). Pour plus d’informations sur le routage de l’API Web ASP.NET, consultez [le routage ASP.NET Web API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) sur le site Web ASP.NET. Pour plus d’informations sur la création du service REST à l’aide d’ASP.NET Core, consultez [création de Services principaux pour les Applications mobiles natives](/aspnet/core/mobile/native-mobile-backend/).

> [!NOTE]
> L’exemple d’application consomme le service hébergé Xamarin REST qui fournit l’accès en lecture seule au service web. Par conséquent, les opérations créent, mettre à jour et supprimer des données ne modifiera pas les données utilisées dans l’application. Toutefois, une version principale du service REST est disponible dans le **TodoRESTService** dossier dans l’accompagnant [exemple de code](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/).
> Si vous hébergez le service REST vous-même, il permet de créer complète créer, mettre à jour, lire et supprimer l’accès aux données.

La `HttpClient` classe est utilisée pour envoyer et recevoir des demandes via HTTP. Elle fournit des fonctionnalités pour envoyer des requêtes HTTP et recevoir des réponses HTTP à partir d’un URI identifié de ressource. Chaque demande est envoyée en tant qu’une opération asynchrone. Pour plus d’informations sur les opérations asynchrones, consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

La `HttpResponseMessage` classe représente un message de réponse HTTP reçu par le service web après la réalisation d’une requête HTTP. Il contient des informations sur la réponse, y compris le code d’état, les en-têtes et les corps. Le `HttpContent` classe représente le corps HTTP et des en-têtes de contenu, tel que `Content-Type` et `Content-Encoding`. Le contenu peut être lu à l’aide de la `ReadAs` méthodes, telles que `ReadAsStringAsync` et `ReadAsByteArrayAsync`, selon le format des données.

### <a name="creating-the-httpclient-object"></a>Création de l’objet HTTPClient

Le `HttpClient` instance est déclarée au niveau de la classe afin que l’objet réside pour tant que l’application a besoin effectuer des requêtes HTTP, comme indiqué dans l’exemple de code suivant :

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

Le `HttpClient.MaxResponseContentBufferSize` propriété est utilisée pour spécifier le nombre maximal d’octets à mettre en mémoire tampon lors de la lecture du contenu dans le message de réponse HTTP. La taille par défaut de cette propriété est la taille maximale d’un entier. Par conséquent, la propriété est définie sur une valeur inférieure, par précaution, pour limiter la quantité de données que l’application accepte en tant que réponse à partir du service web.

### <a name="retrieving-data"></a>Récupération de données

Le `HttpClient.GetAsync` méthode est utilisée pour envoyer la demande d’obtention du service web spécifié par l’URI et la réponse du service web, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Le service REST envoie un code d’état HTTP le `HttpResponseMessage.IsSuccessStatusCode` propriété, pour indiquer si la requête HTTP a réussi ou a échoué. Pour cette opération reste service envoie le code d’état HTTP 200 (OK) dans la réponse, ce qui indique que la demande a réussi et que les informations demandées dans la réponse.

Si l’opération HTTP a réussi, le contenu de la réponse est en lecture pour l’affichage. Le `HttpResponseMessage.Content` propriété représente le contenu de la réponse HTTP et le `HttpContent.ReadAsStringAsync` méthode écrit de façon asynchrone le contenu HTTP dans une chaîne. Ce contenu est ensuite converti à partir de JSON pour un `List` de `TodoItem` instances.

### <a name="creating-data"></a>Création de données

Le `HttpClient.PostAsync` méthode est utilisée pour envoyer la demande POST pour le service web spécifié par l’URI, puis de recevoir la réponse du service web, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

Le `TodoItem` instance est convertie en une charge JSON pour l’envoi au service web. Cette charge utile est incorporée dans le corps du contenu HTTP qui sera envoyé au service web avant de la demande est effectuée avec la `PostAsync` (méthode).

Le service REST envoie un code d’état HTTP le `HttpResponseMessage.IsSuccessStatusCode` propriété, pour indiquer si la requête HTTP a réussi ou a échoué. Les réponses courantes pour cette opération sont :

- **201 (créé)** – la demande a entraîné une ressource en cours de création avant que la réponse a été envoyée.
- **400 (demande incorrecte)** – la demande n’est pas comprise par le serveur.
- **409 (conflit)** – la demande n’a pas pu être exécutée en raison d’un conflit sur le serveur.

### <a name="updating-data"></a>Mise à jour des données

Le `HttpClient.PutAsync` méthode est utilisée pour envoyer la demande PUT pour le service web spécifié par l’URI et la réponse du service web, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
L’opération de le `PutAsync` méthode est identique à la `PostAsync` méthode qui est utilisée pour créer des données dans le service web. Toutefois, les réponses possibles envoyés par le service web diffèrent.

Le service REST envoie un code d’état HTTP le `HttpResponseMessage.IsSuccessStatusCode` propriété, pour indiquer si la requête HTTP a réussi ou a échoué. Les réponses courantes pour cette opération sont :

- **204 (aucun contenu)** : la demande a été traitée avec succès et que la réponse vide est intentionnelle.
- **400 (demande incorrecte)** – la demande n’est pas comprise par le serveur.
- **404 (introuvable)** : la ressource demandée n’existe pas sur le serveur.

### <a name="deleting-data"></a>Suppression de données

Le `HttpClient.DeleteAsync` méthode est utilisée pour envoyer la demande de suppression pour le service web spécifié par l’URI et la réponse du service web, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

Le service REST envoie un code d’état HTTP le `HttpResponseMessage.IsSuccessStatusCode` propriété, pour indiquer si la requête HTTP a réussi ou a échoué. Les réponses courantes pour cette opération sont :

- **204 (aucun contenu)** : la demande a été traitée avec succès et que la réponse vide est intentionnelle.
- **400 (demande incorrecte)** – la demande n’est pas comprise par le serveur.
- **404 (introuvable)** : la ressource demandée n’existe pas sur le serveur.

## <a name="summary"></a>Récapitulatif

Cet article examine comment consommer un service web RESTful à partir d’une application de Xamarin.Forms, à l’aide de la `HttpClient` classe. La simplicité de REST a permis de rendre la méthode principale pour accéder aux services web dans les applications mobiles.


## <a name="related-links"></a>Liens associés

- [Création de services backend pour les applications mobiles natives](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
