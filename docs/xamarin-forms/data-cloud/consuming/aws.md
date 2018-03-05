---
title: "Utilisation d’un Service de SimpleDB Amazon"
description: "Amazon SimpleDB est un service web qui offre la possibilité de stocker et interroger des données dans un cloud d’Amazon. Cet article explique comment utiliser le AWS SDK pour .NET pour interroger, créer, remplacer et supprimer des données stockées dans un service SimpleDB."
ms.topic: article
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 590e39deb7972df9e45064bb1a96e533a1fc9856
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Utilisation d’un Service de SimpleDB Amazon

_Amazon SimpleDB est un service web qui offre la possibilité de stocker et interroger des données dans un cloud d’Amazon. Cet article explique comment utiliser le AWS SDK pour .NET pour interroger, créer, remplacer et supprimer des données stockées dans un service SimpleDB._

SimpleDB services utilisent un modèle de demande et de réponse familière aux consommateurs de services REST. Les opérations sont appelées sur le service SimpleDB en envoyant une demande, ce qui peut contenir des données. Après avoir traité la demande, le service SimpleDB renvoie une réponse contenant les résultats. Un service SimpleDB doit être créé par programme et ne peut pas être créé via le [AWS Console](https://aws.amazon.com). Toutefois, un compte AWS est requis pour créer et accéder à n’importe quel service web de Amazon.

Dans un service SimpleDB, les données sont organisées dans des domaines, dans laquelle les données peuvent être placées et exécuter des requêtes sur les données. Les domaines sont constituées d’éléments qui sont décrites par les paires nom-valeur d’attribut. Domaines peuvent être considérés comme étant similaire aux tables, avec les attributs sont semblables à des colonnes et des éléments sont semblables aux lignes. Pour plus d’informations sur le modèle de données SimpleDB, consultez [modèle de données](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) sur le site Web d’Amazon.

Vous trouverez des instructions sur la configuration des services Amazon requis dans le fichier Lisezmoi qui accompagne l’exemple d’application. Lorsque l’application d’exemple est exécutée, il se connecte à un pool d’identité Amazon Cognito pour autoriser l’accès au service SimpleDB, comme indiqué dans la capture d’écran suivante :

![](aws-images/portal.png "Exemple d’Application")

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

## <a name="consuming-a-simpledb-service"></a>Utilisation d’un Service SimpleDB

Amazon Cognito identité permet AWS services, tels que SimpleDB, pour être appelée à partir d’une application sans informations d’identification AWS de coder en dur dans l’application. Au lieu de cela, un pool d’identité unique est créé dans le [Amazon Cognito Console](https://console.aws.amazon.com/cognito/home). Le pool d’identité contient des identités qui utilisent des rôles pour spécifier les ressources, telles que SimpleDB, qui peut accéder à l’identité.

Le [AWS SDK pour .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) fournit le `CognitoAWSCredentials` et `AmazonSimpleDBClient` classes, qui sont utilisés par une application de Xamarin.Forms pour accéder au service SimpleDB, comme indiqué dans l’exemple de code suivant :

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

Une nouvelle instance de la `CognitoAWSCredentials` classe est créée en fournissant l’id du pool d’identité unique et la région du compte Cognito identité. Au moment de l’écriture, Cognito Identity est uniquement disponible dans les régions USEast1 et EUWest1. Toutefois, il peut communiquer avec les services de Amazon en dehors de ces régions.

Lorsque le `AmazonSimpleDBClient` instance est créée, le `CognitoAWSCredentials` instance doit être fournie, ainsi que par un `AmazonSimpleDBConfig` instance qui spécifie la région géographique où réside le service SimpleDB. Le `CognitoAWSCredentials` instance garantit que le service SimpleDB accessible est celui associé au compte AWS dans lequel le pool d’identité a été créé, tout en évitant d’avoir à incorporer une clé d’accès AWS et la clé secrète de l’application.

Un domaine de service SimpleDB est créé en appelant le `AmazonSimpleDBClient.CreateDomainAsync` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

Le `CreateDomainAsync` méthode requiert un `CreateDomainRequest` instance en tant que paramètre. Le `CreateDomainRequest` instance initialise le `DomainName` valeur à la propriété à utiliser pour identifier le domaine. Pour créer le domaine, cette valeur doit être unique parmi les domaines associés au compte AWS. Sinon, le domaine ne sera pas créé et aucune erreur de réponse n’est envoyée pour indiquer que. Des opérations sur le nom de domaine aura lieu sur le domaine existant, plutôt que d’un domaine qui vient d’être créé.

### <a name="creating-simpledb-objects"></a>Création d’objets SimpleDB

L’exemple d’application utilise le `TodoItem` classe aux données du modèle. Pour stocker un `TodoItem` instance dans un service SimpleDB il doit tout d’abord être converti en un `List` de `ReplaceableAttribute` objets. Cela est accompli par le `ToSimpleDBReplaceableAttributes` (méthode), comme indiqué dans l’exemple de code suivant :

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

Cette méthode crée un `List` de nouveau `ReplaceableAttribute` instances, avec la `List` représentant un seul `TodoItem` instance. Chaque `ReplaceableAttribute` instance représente une propriété unique depuis le `TodoItem` instance. Pour plus d’informations sur la `ReplaceableAttribute` de classe, consultez [ReplaceableAttribute classe](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) sur le site Web d’Amazon.

De même, lorsque les données sont récupérées à partir du service SimpleDB, elle doit être convertie d’une `List` de `Attribute` instances à un `TodoItem` instance. Pour cela, avec la `FromSimpleDBAttributes` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

Cette méthode récupère simplement chaque `Attribute` instance à partir de la `List` et lui affecte le récemment créé `TodoItem` instance.

Pour plus d’informations sur la `Attribute` de classe, consultez [classe d’attributs](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) sur le site Web d’Amazon.

### <a name="querying-data"></a>Interrogation des données

Le contenu d’un domaine peut être récupéré en appelant le `AmazonSimpleDBClient.SelectAsync` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

Le `SelectAsync` méthode accepte un `SelectRequest` instance comme un paramètre, qui spécifie un `Select` son expression de requête `SelectExpression` propriété. Le format de l’expression de requête est semblable au format de l’instruction SQL standard `SELECT` instruction. Pour plus d’informations sur l’expression de requête, consultez [de choisir de créer des requêtes Amazon SimpleDB](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) sur le site Web d’Amazon.

> [!NOTE]
> **Remarque**: veillez à respecter les règles d’indentation lors de la construction de l’expression de requête. Pour plus d’informations, consultez [sélectionner les règles de devis](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) sur le site Web d’Amazon.

Le `SelectAsync` méthode retourne une réponse contenant une collection d’éléments et attributs qui correspondent à l’expression de requête. Cette collection est ensuite convertie en un `List` de `TodoItem` instances pour l’affichage.

### <a name="creating-and-replacing-data"></a>Création et le remplacement des données

Le `AmazonSimpleDBClient.PutAttributesAsync` méthode est utilisée pour créer et de remplacer des données dans le domaine de service SimpleDB, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

Le `PutAttributesAsync` méthode accepte un `PutAttributesRequest` instance en tant que paramètre. Le `PutAttributesRequest` instance spécifie les paires nom-valeur attribut créé comme un nouvel élément, ou un remplacement dans un élément existant. Le `List` de `ReplaceableAttribute` instances est généré par le `ToSimpleDBReplaceableAttributes` (méthode). Cette méthode définit également la `Replace` propriété de chaque `ReplaceableAttribute` à `true`. Cela entraîne la nouvelle valeur d’attribut remplacer une valeur d’attribut, si les données sont remplacées. Toutefois, la tentative de remplacer les valeurs d’attribut qui n’existent pas n’implique pas d’une réponse d’erreur.

La valeur de la `PutAttributesRequest.ItemName` propriété contrôle si un nouvel élément est ajouté au domaine, ou si un élément existant sera remplacé. Lorsque l’application crée un nouvel élément, il définit le `TodoItem.ID` à une nouvelle propriété `Guid`. Cela garantit que chaque `TodoItem` instance possède un identificateur unique. Par conséquent, si la `PutAttributesRequest.ItemName` est définie à une valeur qui n’existe pas dans le domaine, le service SimpleDB crée un nouvel élément contenant les paires de nom-valeur d’attribut spécifié. Si la `PutAttributesRequest.ItemName` est définie à une valeur qui existe déjà dans le domaine, le service SimpleDB mettre à jour l’élément avec les paires de nom-valeur d’attribut spécifié.

### <a name="deleting-data"></a>Suppression de données

Le `AmazonSimpleDBClient.DeleteAttributesAsync` méthode est utilisée pour supprimer les données du domaine du service SimpleDB, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

Le `DeleteAttributesAsync` méthode accepte un `DeleteAttributesRequest` instance en tant que paramètre.  Le `DeleteAttributesRequest` instance spécifie les attributs qui doivent être supprimées à partir de l’élément, avec la `List` de `Attribute` pour supprimer les instances en cours de génération par le `ToSimpleDBAttributes` (méthode). L’élément est supprimé à condition que tous les attributs de l’élément sont supprimés.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment le AWS SDK pour .NET permet d’interroger, de créer et de remplacer et supprimer des données stockées dans un service SimpleDB. Ce kit fournit le `CognitoAWSCredentials` et `AmazonSimpleDBClient` classes qui sont utilisées par une application de Xamarin.Forms pour accéder au service SimpleDB.


## <a name="related-links"></a>Liens associés

- [TodoAWS (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Amazon Web Services SDK Guide de développement Xamarin](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Identité de Cognito Amazon](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Documentation du développeur de SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Classe de AmazonSimpleDBClient](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [Amazon Web Services SDK pour .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
