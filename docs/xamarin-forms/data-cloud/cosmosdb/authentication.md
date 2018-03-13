---
title: "L’authentification des utilisateurs avec une base de données du Document Cosmos Azure DB"
description: "Bases de données de document Cosmos DB Azure prend en charge les collections partitionnées, ce qui peuvent s’étendre sur plusieurs serveurs et des partitions, débit et stockage illimitée : la prise en charge. Cet article explique comment combiner le contrôle d’accès avec les collections partitionnées, afin qu’un utilisateur peut accéder uniquement leurs propres documents dans une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 10c4a1e3355263722d170dff0a5e2707eb794818
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>L’authentification des utilisateurs avec une base de données du Document Cosmos Azure DB

_Bases de données de document Cosmos DB Azure prend en charge les collections partitionnées, ce qui peuvent s’étendre sur plusieurs serveurs et des partitions, débit et stockage illimitée : la prise en charge. Cet article explique comment combiner le contrôle d’accès avec les collections partitionnées, afin qu’un utilisateur peut accéder uniquement leurs propres documents dans une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

Une clé de partition doit être spécifiée lors de la création d’une collection partitionnée, et les documents avec la même clé de partition sont stockés dans la même partition. Par conséquent, spécification de l’identité de l’utilisateur en tant que clé de partition entraîne une collection partitionnée qui stocke uniquement des documents pour cet utilisateur. Cela garantit également que la base de données du document de base de données Azure Cosmos mettra à l’échelle le nombre d’utilisateurs et que les éléments augmentent.

Accès doit être accordé à une collection, et le modèle de contrôle d’accès API DocumentDB définit deux types de constructions d’accès :

- **Les clés principales** activer un accès administratif complet à toutes les ressources au sein d’un compte de base de données Cosmos et sont créés lors de la création d’un compte de base de données Cosmos.
- **Jetons de ressource** capturent la relation entre l’utilisateur de base de données et de l’autorisation de l’utilisateur dispose d’une ressource Cosmos DB spécifique, comme une collection ou un document.

Exposition d’une clé principale ouvre un compte Cosmos DB la possibilité d’utiliser malveillant ou involontaire. Toutefois, les jetons de ressource de base de données Cosmos fournissent un mécanisme sécurisé pour autoriser les clients à lire, écrire et supprimer des ressources spécifiques dans un compte de base de données Cosmos en fonction des autorisations accordées.

Une approche courante à la demande, générer et de livrer des jetons de ressource à une application mobile doit utiliser un courtier de jeton de ressource. Le diagramme suivant montre une vue d’ensemble de comment l’exemple d’application utilise un courtier de jeton de ressource pour gérer l’accès à la base de données de document :

![](authentication-images/documentdb-authentication.png "Processus d’authentification de base de données de document")

Le courtier de jeton de ressource est un service d’API Web de couche intermédiaire, hébergé dans Azure App Service, qui possède la clé principale du compte de base de données Cosmos. L’exemple d’application utilise le courtier de jeton de ressource pour gérer les accès à la base de données de document comme suit :

1. Sur la connexion, l’application Xamarin.Forms contacte pour lancer un flux d’authentification du Service d’applications Azure.
1. Azure App Service exécute un flux d’authentification OAuth avec Facebook. Une fois le flux d’authentification est terminée, l’application de Xamarin.Forms reçoit un jeton d’accès.
1. L’application Xamarin.Forms utilise le jeton d’accès pour demander un jeton de ressource à partir du courtier de jeton de ressource.
1. Le courtier de jeton de ressource utilise le jeton d’accès pour demander l’identité de l’utilisateur à partir de Facebook. Identité de l’utilisateur est ensuite utilisée pour demander un jeton de ressource à partir de la base de données Cosmos, qui est utilisé pour accorder l’accès en lecture/écriture à la collection partitionnée de l’utilisateur authentifié.
1. L’application Xamarin.Forms utilise le jeton de ressource pour accéder directement aux ressources de base de données Cosmos avec les autorisations définies par le jeton de ressource.

> [!NOTE]
> Lorsque le jeton de ressource expire, les demandes de base de données de document suivants recevra une exception non autorisé 401. À ce stade, les applications Xamarin.Forms doivent rétablir l’identité et demander un nouveau jeton de ressource.

Pour plus d’informations sur le partitionnement de base de données Cosmos, consultez [la partition et l’échelle dans la base de données Azure Cosmos](/azure/cosmos-db/partition-data/). Pour plus d’informations sur le contrôle d’accès de base de données Cosmos, consultez [sécurisation de l’accès aux données de la base de données Cosmos](/azure/cosmos-db/secure-access-to-data/) et [contrôle d’accès dans l’API DocumentDB](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Installation

Le processus d’intégration du courtier de jeton de ressource dans une application de Xamarin.Forms est la suivante :

1. Créer un compte de base de données Cosmos qui utilise le contrôle d’accès. Pour plus d’informations, consultez [Cosmos DB Configuration](#cosmosdb_configuration).
1. Créer un Service d’application Azure pour héberger le courtier de jeton de ressource. Pour plus d’informations, consultez [Azure App Service de Configuration](#app_service_configuration).
1. Créer une application Facebook pour effectuer une authentification. Pour plus d’informations, consultez [Configuration de l’application Facebook](#facebook_configuration).
1. Configurer le Service d’application Azure pour effectuer une authentification simple avec Facebook. Pour plus d’informations, consultez [Configuration de l’authentification du Service Azure App](#app_service_authentication_configuration).
1. Configurer l’exemple d’application Xamarin.Forms pour communiquer avec le Service d’applications Azure et Cosmos DB. Pour plus d’informations, consultez [Xamarin.Forms Configuration de l’Application](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="cosmos-db-configuration"></a>Configuration de la base de données de COSMOS

Le processus de création d’un compte de base de données Cosmos qui utilise le contrôle d’accès est la suivante :

1. Créez un compte de base de données Cosmos. Pour plus d’informations, consultez [créer un compte de base de données Cosmos](/azure/cosmos-db/documentdb-dotnetcore-get-started#step-1-create-a-documentdb-account).
1. Dans le compte de base de données Cosmos, créez un regroupement nommé `UserItems`, en spécifiant une clé de partition de `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Configuration du Service d’application Azure

Le processus d’hébergement de service broker jeton de ressource dans Azure App Service est la suivante :

1. Dans le portail Azure, créer une application web du Service d’applications. Pour plus d’informations, consultez [créer une application web dans un environnement App Service](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Dans le portail Azure, ouvrez le panneau des paramètres de l’application pour l’application web et ajoutez les paramètres suivants :
    - `accountUrl` – la valeur doit être l’URL Cosmos DB du compte à partir du Panneau de clés du compte de base de données Cosmos.
    - `accountKey` – la valeur doit être la clé principale Cosmos DB (principale ou secondaire) à partir du Panneau de clés du compte de base de données Cosmos.
    - `databaseId` – la valeur doit être le nom de la base de données de la base de données Cosmos.
    - `collectionId` – la valeur doit être le nom de la collection de la base de données Cosmos (dans ce cas, `UserItems`).
    - `hostUrl` – la valeur doit être l’URL de l’application web à partir du Panneau de vue d’ensemble du compte de Service de l’application.

    La capture d’écran suivante illustre cette configuration :

    [![](authentication-images/azure-web-app-settings.png "Paramètres de l’application du Service d’applications Web")](authentication-images/azure-web-app-settings-large.png#lightbox "paramètres de l’application du Service d’applications Web")

1. Publiez la solution de service Broker pour les jetons de ressource à l’application web de Service d’applications Azure.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Configuration de l’application Facebook

Le processus de création d’une application Facebook pour effectuer une authentification est comme suit :

1. Créer une application Facebook. Pour plus d’informations, consultez [inscrire et configurer une application](https://developers.facebook.com/docs/apps/register) sur le centre de développement Facebook.
1. Ajoutez le produit de la connexion de Facebook à l’application. Pour plus d’informations, consultez [ajouter un compte de connexion Facebook pour votre application ou le site Web](https://developers.facebook.com/docs/facebook-login) sur le centre de développement Facebook.
1. Configurez le compte de connexion Facebook comme suit :
  - Activer la connexion Client OAuth.
  - Activer la connexion Web OAuth.
  - Définir l’URI à l’URI de l’application web du Service d’applications, de la redirection OAuth valide avec `/.auth/login/facebook/callback` ajouté.

  La capture d’écran suivante illustre cette configuration :

  ![](authentication-images/facebook-oauth-settings.png "Paramètres de connexion Facebook OAuth")

Pour plus d’informations, consultez [inscrire votre application avec Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Configuration de l’authentification du Service d’applications Azure

Le processus de configuration de l’authentification simple du Service de l’application est la suivante :

1. Dans le portail Azure, accédez à l’application web du Service d’applications.
1. Dans le portail Azure, ouvrez l’authentification / panneau d’autorisation et d’effectuer la configuration suivante :
  - L’authentification du Service de l’application doit être activée.
  - L’action à entreprendre lorsqu’une demande n’est pas authentifiée doit être définie sur **connexion avec Facebook**.

  La capture d’écran suivante illustre cette configuration :

  [![](authentication-images/app-service-authentication-settings.png "Paramètres d’authentification du Service d’applications Web application")](authentication-images/app-service-authentication-settings-large.png#lightbox "paramètres d’authentification du Service d’applications Web App")

L’application web du Service d’applications doit également être configurée pour communiquer avec l’application Facebook pour activer le flux d’authentification. Cela peut être accompli en sélectionnant le fournisseur d’identité Facebook et en entrant le **ID d’application** et **Secret d’application** valeurs à partir des paramètres d’application Facebook sur le centre de développement Facebook. Pour plus d’informations, consultez [Facebook d’ajouter des informations à votre application](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Configuration de l’Application Xamarin.Forms

Le processus de configuration de l’exemple d’application Xamarin.Forms est la suivante :

1. Ouvrez la solution Xamarin.Forms.
1. Ouvrez `Constants.cs` et mettre à jour les valeurs des constantes suivantes :
  - `EndpointUri` – la valeur doit être l’URL Cosmos DB du compte à partir du Panneau de clés du compte de base de données Cosmos.
  - `DatabaseName` – la valeur doit être le nom de la base de données du document.
  - `CollectionName` – la valeur doit être le nom de la collection de base de données de document (dans ce cas, `UserItems`).
  - `ResourceTokenBrokerUrl` – la valeur doit être l’URL de l’application de web de broker jeton de ressource à partir du Panneau de vue d’ensemble du compte de Service de l’application.

## <a name="initiating-login"></a>Connexion d’origine

L’exemple d’application lance le processus de connexion à l’aide de Xamarin.Auth pour rediriger un navigateur vers une URL de fournisseur d’identité, comme illustré dans l’exemple de code suivant :

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Cela provoque un flux d’authentification OAuth être initiée entre Azure App Service et Facebook, qui affiche la page de connexion Facebook :

![](authentication-images/login.png "Facebook Login")

La connexion peut être annulée en appuyant sur la **Annuler** bouton sur iOS, ou en appuyant sur la **précédent** bouton sur Android, auquel cas l’utilisateur reste non authentifiée et l’interface utilisateur du fournisseur identité est supprimé de l’écran.

Pour plus d’informations sur Xamarin.Auth, consultez [authentification des utilisateurs avec un fournisseur d’identité](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Obtention d’un jeton de ressource

Après une authentification réussie, le `WebRedirectAuthenticator.Completed` se déclenche des événements. L’exemple de code suivant illustre la gestion de cet événement :

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Le résultat d’une authentification réussie est un jeton d’accès, qui est disponible `AuthenticatorCompletedEventArgs.Account` propriété. Le jeton d’accès est extraites et utilisé dans une demande GET pour la ressource jeton du service broker `resourcetoken` API.

Le `resourcetoken` API utilise le jeton d’accès pour demander l’identité de l’utilisateur à partir de Facebook, qui à son tour, est utilisé pour demander un jeton de ressource à partir de la base de données Cosmos. Si un document d’autorisation valide existe déjà pour l’utilisateur dans la base de données du document, elle est récupérée et un document JSON qui contient le jeton de ressource est renvoyé à l’application de Xamarin.Forms. Si un document d’autorisation valide n’existe pas pour l’utilisateur, un utilisateur et l’autorisation est créé dans la base de données du document et le jeton de ressource est extraite à partir du document d’autorisation et renvoyé à l’application Xamarin.Forms dans un document JSON.

> [!NOTE]
> Un utilisateur de base de données de document est une ressource associée à une base de données du document, et chaque base de données peut contenir zéro ou plusieurs utilisateurs. Une autorisation de base de données de document est une ressource associée à un utilisateur de base de données de document, et chaque utilisateur peut contenir zéro ou plusieurs autorisations. Une ressource autorisation donne accès à un jeton de sécurité nécessitant l’utilisateur lorsque vous tentez d’accéder à une ressource, tel qu’un document.

Si le `resourcetoken` API terminée avec succès, il enverra un code d’état HTTP 200 (OK) dans la réponse, ainsi que d’un document JSON qui contient le jeton de ressource. Les données JSON suivantes affiche un message de réponse correcte classique :

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

Le `WebRedirectAuthenticator.Completed` Gestionnaire d’événements lit la réponse de le `resourcetoken` API et extrait le jeton de ressource et l’id d’utilisateur. Le jeton de ressource est ensuite passé comme argument à la `DocumentClient` constructeur, qui encapsule le point de terminaison, informations d’identification et la stratégie de connexion utilisé pour accéder à la base de données Cosmos et est utilisé pour configurer et exécuter des demandes sur la base de données Cosmos. Le jeton de ressource est envoyé avec chaque requête pour accéder directement à une ressource et indique que l’accès en lecture/écriture à la collection partitionnée de l’utilisateur authentifié est accordé.

## <a name="retrieving-documents"></a>Récupération de Documents

Récupération de documents qui n’appartiennent qu’à l’utilisateur authentifié peut être obtenue en créant une requête de document qui inclut l’id de l’utilisateur en tant que clé de partition, est illustrée dans l’exemple de code suivant :

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

La requête de façon asynchrone récupère tous les documents appartenant à l’utilisateur authentifié, à partir de la collection spécifiée et les place dans un `List<TodoItem>` collection pour l’affichage.

Le `CreateDocumentQuery<T>` méthode spécifie un `Uri` argument qui représente la collection doit être interrogée pour les documents, et un `FeedOptions` objet. Le `FeedOptions` objet spécifie qu’un nombre illimité d’éléments peut être retourné par la requête et l’id d’utilisateur en tant que clé de partition. Cela garantit que seuls les documents dans la collection partitionnée de l’utilisateur sont retournés dans le résultat.

> [!NOTE]
> Notez que les documents d’autorisation, qui sont créés par l’intermédiaire de jeton de ressource, sont stockés dans la même collection de document que les documents créés par l’application de Xamarin.Forms. Par conséquent, la requête de document contient un `Where` clause qui applique un prédicat de filtrage à la requête par rapport à la collection de documents. Cette clause permet de s’assurer que les documents d’autorisation ne sont pas retournées à partir de la collection de documents.

Pour plus d’informations sur la récupération de documents à partir d’une collection de documents, consultez [Documents de la Collection de la récupération de Document](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query).

## <a name="inserting-documents"></a>Insertion de Documents

Avant l’insertion d’un document dans une collection de documents, le `TodoItem.UserId` propriété doit être mis à jour avec la valeur utilisée comme clé de partition, comme illustré dans l’exemple de code suivant :

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Cela garantit que le document est inséré dans la collection partitionnée de l’utilisateur.

Pour plus d’informations sur l’insertion d’un document dans une collection de documents, consultez [insertion d’un Document dans une Collection de documents](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document).

## <a name="deleting-documents"></a>Suppression de Documents

La valeur de clé de partition doit être spécifiée lors de la suppression d’un document à partir d’une collection partitionnée, comme illustré dans l’exemple de code suivant :

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Cela garantit que Cosmos DB sache qui partitionnée collection à supprimer le document à partir de.

Pour plus d’informations sur la suppression d’un document à partir d’une collection de documents, consultez [suppression d’un Document à partir d’une Collection de documents](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document).

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment combiner le contrôle d’accès avec les collections partitionnées, afin qu’un utilisateur peut accéder uniquement leurs propres documents de base de données de document dans une application de Xamarin.Forms. Spécification de l’identité de l’utilisateur en tant que clé de partition garantit qu’une collection partitionnée ne peut stocker des documents pour cet utilisateur.


## <a name="related-links"></a>Liens associés

- [TodoDocumentDBAuth (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Consommation d’une base de données de documents Azure Cosmos DB](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Sécurisation de l’accès aux données de la base de données Azure Cosmos](/azure/cosmos-db/secure-access-to-data/)
- [Contrôle d’accès dans l’API DocumentDB](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [La partition et l’échelle dans la base de données Azure Cosmos](/azure/cosmos-db/partition-data/)
- [Bibliothèque cliente de DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API de base de données de Cosmos Azure](https://msdn.microsoft.com/library/azure/dn948556.aspx)
