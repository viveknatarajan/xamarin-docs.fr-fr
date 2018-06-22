---
title: Introduction aux Services Web
description: Ce guide montre comment utiliser des technologies de service web différents. Rubriques couvertes incluent la communication avec les services REST, les services SOAP et les services Windows Communication Foundation.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 5c07443519391192f7bdb62cc19ff8b1f87b2558
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919083"
---
# <a name="introduction-to-web-services"></a>Introduction aux Services Web

_Ce guide montre comment utiliser des technologies de service web différents. Rubriques couvertes incluent la communication avec les services REST, les services SOAP et les services Windows Communication Foundation._

Pour fonctionner correctement, de nombreuses applications mobiles sont dépendantes sur le cloud, et donc l’intégration des services web dans les applications mobiles est un scénario courant. La plateforme Xamarin prend en charge l’utilisation des technologies de service web différents et inclut la prise en charge intégrée et tiers pour consommer des services Windows Communication Foundation (WCF) RESTful et ASMX.

Cet article traite des sujets suivants :

- [Services REST](#rest)
- [Services Web ASP.Net (ASMX)](#asmx)
- [Services WCF](#wcf)

Pour les clients à l’aide de Xamarin.Forms, il existe des exemples complets à l’aide de chacune de ces technologies dans les [Xamarin.Forms Web Services](~/xamarin-forms/data-cloud/index.md) documentation.

> [!IMPORTANT]
> Dans iOS 9, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles.
> Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.

Vous pouvez annuler des couches d’application si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).



<a name="rest" />

## <a name="rest"></a>REST

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

La simplicité de REST a permis de rendre la méthode principale pour accéder aux services web dans les applications mobiles.

## <a name="consuming-rest-services"></a>Consommation de Services REST

Il existe un certain nombre de bibliothèques de classes peuvent être utilisés pour consommer des services REST, et les sous-sections suivantes présentent les. Pour plus d’informations sur l’utilisation d’un service REST, consultez [utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

Le [Microsoft HTTP Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http) fournit le `HttpClient` (classe), qui est utilisé pour envoyer et recevoir des demandes via HTTP. Il fournit des fonctionnalités de l’envoi des demandes HTTP et la réception de réponses HTTP à partir d’une ressource identifiée d’URI. Chaque demande est envoyée en tant qu’une opération asynchrone. Pour plus d’informations sur les opérations asynchrones, consultez [vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md).

La `HttpResponseMessage` classe représente un message de réponse HTTP reçu par le service web après la réalisation d’une requête HTTP. Il contient des informations sur la réponse, y compris le code d’état, en-têtes et corps. Le `HttpContent` classe représente le corps HTTP et des en-têtes de contenu, tel que `Content-Type` et `Content-Encoding`. Le contenu peut être lu à l’aide de la `ReadAs` méthodes, telles que `ReadAsStringAsync` et `ReadAsByteArrayAsync`, selon le format des données.

Pour plus d’informations sur la `HttpClient` de classe, consultez [création de l’objet HTTPClient](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Appel de services web avec `HTTPWebRequest` implique :

-  Création de l’instance de la demande d’un URI particulier.
-  Définition des diverses propriétés HTTP sur l’instance de requête.
-  Récupérer un `HttpWebResponse` à partir de la demande.
-  La lecture des données de la réponse.

Par exemple, le code suivant récupère des données à partir des États-Unis Service de web national les mesures de bibliothèque :

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

L’exemple ci-dessus crée un `HttpWebRequest` qui retourne des données au format JSON. Les données sont retournées dans un `HttpWebResponse`, à partir de laquelle un `StreamReader` peut être obtenu pour lire les données.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Une autre approche pour la consommation de services REST est à l’aide de la [RestSharp](http://restsharp.org/) bibliothèque. RestSharp encapsule les demandes HTTP, y compris la prise en charge de la récupération des résultats en tant que contenu de chaîne brute ou en tant qu’objet c# désérialisé. Par exemple, le code suivant effectue une demande pour les États-Unis. Service web de bibliothèque de médicament national et récupère les résultats en tant que JSON au format chaîne :

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` est une méthode qui prend la chaîne JSON brute à partir de la `RestSharp.RestResponse.Content` propriété et le convertir en un objet c#. La désérialisation des données retournées à partir de services web est décrite plus loin dans cet article.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

En plus des classes disponibles dans la base Mono bibliothèque de classes (BCL), tel que `HttpWebRequest`et des bibliothèques tierces c#, telles que RestSharp, classes spécifiques à la plateforme sont également disponibles pour la consommation de services web. Par exemple, dans iOS, le `NSUrlConnection` et `NSMutableUrlRequest` classes peuvent être utilisées.

L’exemple de code suivant montre comment appeler des États-Unis Service web de bibliothèque de médicament national à l’aide des classes d’e/s :

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

En règle générale, les classes spécifiques à la plateforme pour l’utilisation de services web doivent être limités aux scénarios où le code natif est porté à c#. Si possible, code d’accès aux services web doivent être portable afin qu’il puisse être partagé inter-plateformes.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Une autre option pour appeler des services web est la [pile Service](http://www.servicestack.net/) bibliothèque. Par exemple, le code suivant montre comment utiliser la pile de Service `IServiceClient.GetAsync` méthode émettre une demande de service :

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> Alors que les outils comme ServiceStack et RestSharp facilitent appeler et consommer REST des services, il est parfois non trivial pour consommer XML ou JSON qui n’est pas conforme à la norme _DataContract_ conventions de sérialisation. Si nécessaire, la demande de code non managé et gérer la sérialisation appropriée explicitement à l’aide de la bibliothèque ServiceStack.Text décrite ci-dessous.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Utilisation des données RESTful

Services web rESTful utilisent généralement des messages JSON pour retourner des données au client. JSON est un textuel, échange de données format qui produit compact charges utiles, ce qui entraîne la bande passante réduite lors de l’envoi de données. Dans cette section, nous étudierons mécanismes pour la consommation des réponses RESTful dans JSON et brut-Old-XML (POX).

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

La plateforme de Xamarin est fourni avec la prise en charge de JSON prêtes à l’emploi. En utilisant un `JsonObject`, résultats peuvent être récupérés comme indiqué dans l’exemple de code suivant :

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Toutefois, il est important de savoir que le `System.Json` outils chargent l’intégralité des données en mémoire.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

Le [NewtonSoft JSON.NET bibliothèque](http://www.newtonsoft.com/json) est une bibliothèque largement utilisée pour sérialiser et désérialiser les messages JSON. L’exemple de code suivant montre comment utiliser JSON.NET pour désérialiser un message JSON en un objet c# :

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack.Text est une bibliothèque de sérialisation JSON conçue pour fonctionner avec la bibliothèque ServiceStack. L’exemple de code suivant montre comment analyser le JSON en utilisant un `ServiceStack.Text.JsonObject`:

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

En cas d’utilisation d’un service de web REST basé sur XML, LINQ to XML peut être utilisé pour analyser le fichier XML et de remplir une c# inline de l’objet, comme illustré dans l’exemple de code suivant :

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>Service Web ASP.NET (ASMX)

ASMX fournit la possibilité de créer des services web qui envoient des messages à l’aide de l’accès protocole SOAP (Simple Object). SOAP est un protocole indépendant de la plateforme et indépendant du langage pour la création et l’accès aux services web. Les consommateurs d’un service ASMX n’avez pas besoin de connaître quoi que ce soit sur la plateforme, modèle d’objet ou langage de programmation utilisé pour implémenter le service. Il leur suffit de comprendre comment envoyer et recevoir des messages SOAP.

Un message SOAP est un document XML contenant les éléments suivants :

- Un élément racine nommé *enveloppe* qui identifie le document XML comme un message SOAP.
- Facultatif *en-tête* élément qui contient des informations spécifiques à l’application, telles que les données d’authentification. Si le *en-tête* élément est présent il doit être le premier élément enfant de le *enveloppe* élément.
- Une valeur obligatoire *corps* élément qui contient le message SOAP destiné au destinataire.
- Facultatif *erreur* élément qui est utilisé pour indiquer les messages d’erreur. Si le *erreur* élément est présent, il doit être un élément enfant de le *corps* élément.

SOAP peut opérer sur plusieurs protocoles de transport, y compris HTTP, SMTP, TCP et UDP. Toutefois, un service ASMX peut uniquement fonctionner sur HTTP. La plateforme Xamarin prend en charge les implémentations standards SOAP 1.1 sur HTTP, et cela inclut la prise en charge pour la plupart des configurations de service ASMX standards.

### <a name="generating-a-proxy"></a>Génération d’un Proxy

A *proxy* doivent être générées pour consommer un service ASMX, ce qui permet de se connecter au service. Le proxy est construit par la consommation des métadonnées de service qui définit les méthodes et la configuration de service associé. Ces métadonnées sont exposées sous forme de document Web Services Description Language (WSDL) généré par le service web. Le proxy est construit à l’aide de Visual Studio pour Mac ou Visual Studio pour ajouter une référence web pour le service web aux projets spécifiques à la plateforme.

L’URL du service web peut être une source distante hébergée ou la ressource du système de fichiers local accessible via la `file:///` préfixe de chemin d’accès, par exemple :

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "L’URL du service web peut être une source distante hébergée ou une ressource du système de fichiers local accessible via le préfixe de chemin d’accès de fichier")](images/add-webreference-dialog.png#lightbox)

Cette opération génère le proxy dans le dossier Web ou des références de Service du projet. Depuis la génération d’un proxy de code, il ne doit pas être modifié.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Ajout manuel d’un Proxy pour un projet

Si vous avez un proxy existant qui a été généré à l’aide des outils compatibles, cette sortie peut être consommée lorsque inclus dans le cadre de votre projet. Dans Visual Studio pour Mac, utilisez le **ajouter des fichiers...** option de menu pour ajouter le proxy. En outre, cela nécessite *System.Web.Services.dll* à référencer explicitement à l’aide de la **ajouter des références...** boîte de dialogue.

### <a name="consuming-the-proxy"></a>Utilisation du Proxy

Les classes de proxy généré fournissent des méthodes pour utiliser le service web qui utilisent le modèle de conception de modèle de programmation asynchrone (APM). Dans ce modèle, une opération asynchrone est implémentée comme deux méthodes nommées *BeginOperationName* et *EndOperationName*, qui commencent et terminent l’opération asynchrone.

Le *BeginOperationName* méthode commence l’opération asynchrone et retourne un objet qui implémente le `IAsyncResult` interface. Après avoir appelé *BeginOperationName*, une application peut continuer à exécuter des instructions sur le thread appelant, tandis que l’opération asynchrone s’exécute sur un thread de pool de threads.

Pour chaque appel à *BeginOperationName*, l’application doit également appeler *EndOperationName* pour obtenir les résultats de l’opération. La valeur de retour de *EndOperationName* est du même type que celui retourné par la méthode de service web synchrone. L’exemple de code suivant montre un exemple :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

La bibliothèque parallèle de tâches (TPL) peut simplifier le processus d’utilisation d’une paire de méthodes begin/end APM en encapsulant les opérations asynchrones dans le même `Task` objet. Cette encapsulation est fournie par plusieurs surcharges de la `Task.Factory.FromAsync` (méthode). Cette méthode crée un `Task` qui s’exécute le `TodoService.EndGetTodoItems` méthode une fois la `TodoService.BeginGetTodoItems` méthode se termine, avec la `null` paramètre indiquant qu’aucune donnée n’est passée dans le `BeginGetTodoItems` déléguer. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

Pour plus d’informations sur l’APM, consultez [modèle de programmation asynchrone](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) et [bibliothèque parallèle de tâches et le Framework programmation asynchrone .NET traditionnelle](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) sur MSDN.

Pour plus d’informations sur l’utilisation d’un service ASMX, consultez [consommer un Service Web de ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF est un cadre unifié de Microsoft pour la création d’applications orientées service. Il permet aux développeurs de créer des applications distribuées traitées, fiables, sécurisées et interopérables.

WCF décrit un service avec un large éventail de contrats différents qui sont les suivantes :

- **Contrats de données** : définir les structures de données qui constituent la base pour le contenu d’un message.
- **Contrats de message** – composer des messages à partir des contrats de données existants.
- **Contrats d’erreur** – autoriser les erreurs SOAP personnalisées être spécifié.
- **Contrats de service** : spécifier les opérations qui prennent en charge des services et les messages nécessaires pour interagir avec chaque opération. Ils indiquent également tout comportement d’erreur personnalisée qui peut être associé à des opérations sur chaque service.

Il existe des différences entre les Services de Web ASP.NET (ASMX) et WCF, mais il est important de comprendre que WCF prend en charge les mêmes fonctionnalités offrant ASMX – les messages SOAP sur HTTP.

En règle générale, la plateforme Xamarin prend en charge le même sous-ensemble de côté client de WCF est fourni avec le runtime Silverlight. Cela inclut les implémentations d’encodage et au protocole courantes de WCF, codée au format texte des messages SOAP sur HTTP à l’aide du protocole de transport la `BasicHttpBinding` classe. En outre, la prise en charge de WCF requiert l’utilisation d’outils disponibles uniquement dans un environnement Windows pour générer le proxy.

Pour plus d’informations sur l’utilisation de la plateforme de Xamarin pour consommer un service WCF web service avec le `BasicHttpBinding` de classe, consultez [procédure pas à pas : utilisation de WCF](walkthrough-working-with-wcf.md).

### <a name="generating-a-proxy"></a>Génération d’un Proxy

A *proxy* doit être généré pour utiliser un service WCF, ce qui permet de se connecter au service. Le proxy est construit par la consommation des métadonnées de service qui définissent les méthodes et la configuration de service associé. Ces métadonnées sont exposées sous la forme d’un document Web Services Description Language (WSDL) généré par le service web. Le proxy peut être généré à l’aide du fournisseur Microsoft WCF Web Service référence dans Visual Studio 2017 pour ajouter une référence de service pour le service web dans une bibliothèque Standard de .NET.

Une alternative à la création du proxy à l’aide du fournisseur Microsoft WCF Web Service référence dans Visual Studio 2017 est d’utiliser le service Model Metadata Utility Tool (svcutil.exe). Pour plus d’informations, consultez [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Configuration du serveur Proxy

Configuration du proxy généré prennent généralement deux arguments de configuration (en fonction de SOAP 1.1/ASMX ou WCF) lors de l’initialisation : le `EndpointAddress` et/ou les informations de liaison associés, comme indiqué dans l’exemple ci-dessous :

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Une liaison est utilisée pour spécifier le transport, le codage et les détails de protocole requis pour les applications et services communiquer entre eux. Le `BasicHttpBinding` Spécifie que les messages SOAP codée au format texte seront envoyés via le protocole de transport HTTP. Spécification d’une adresse de point de terminaison permet à l’application pour se connecter à différentes instances du service WCF, condition qu’il existe plusieurs instances publiées.

### <a name="consuming-the-proxy"></a>Utilisation du Proxy

Les classes proxy généré fournissent des méthodes pour utiliser les services web qui utilisent le modèle de conception de modèle de programmation asynchrone (APM). Dans ce modèle, une opération asynchrone est implémentée comme deux méthodes nommées *BeginOperationName* et *EndOperationName*, qui commencent et terminent l’opération asynchrone.

Le *BeginOperationName* méthode commence l’opération asynchrone et retourne un objet qui implémente le `IAsyncResult` interface. Après avoir appelé *BeginOperationName*, une application peut continuer à exécuter des instructions sur le thread appelant, tandis que l’opération asynchrone s’exécute sur un thread de pool de threads.

Pour chaque appel à *BeginOperationName*, l’application doit également appeler *EndOperationName* pour obtenir les résultats de l’opération. La valeur de retour de *EndOperationName* est du même type que celui retourné par la méthode de service web synchrone. L’exemple de code suivant montre un exemple :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

La bibliothèque parallèle de tâches (TPL) peut simplifier le processus d’utilisation d’une paire de méthodes begin/end APM en encapsulant les opérations asynchrones dans le même `Task` objet. Cette encapsulation est fournie par plusieurs surcharges de la `Task.Factory.FromAsync` (méthode). Cette méthode crée un `Task` qui s’exécute le `TodoServiceClient.EndGetTodoItems` méthode une fois la `TodoServiceClient.BeginGetTodoItems` méthode se termine, avec la `null` paramètre indiquant qu’aucune donnée n’est passée dans le `BeginGetTodoItems` déléguer. Enfin, la valeur de la `TaskCreationOptions` énumération spécifie que le comportement par défaut pour la création et l’exécution de tâches doit être utilisé.

Pour plus d’informations sur l’APM, consultez [modèle de programmation asynchrone](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) et [bibliothèque parallèle de tâches et le Framework programmation asynchrone .NET traditionnelle](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) sur MSDN.

Pour plus d’informations sur l’utilisation d’un service WCF, consultez [utilisation d’un Service Web de Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>À l’aide de la sécurité de Transport

Les Services WCF peuvent utiliser la sécurité de niveau transport pour se protéger contre l’interception de messages. La plateforme Xamarin prend en charge les liaisons qui utilisent la sécurité de niveau transport à l’aide de SSL. Toutefois, il peut y avoir des cas dans lequel la pile devra peut-être valider le certificat, ce qui entraîne un comportement inattendu. La validation peut être remplacée en inscrivant un `ServerCertificateValidationCallback` délégué avant d’appeler le service, comme illustré dans l’exemple de code suivant :

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Cela maintient le chiffrement de transport tout en ignorant la validation de certificat côté serveur. Toutefois, cette approche ne tient pas compte les problèmes de confiance associées au certificat efficacement et peut ne pas convenir. Pour plus d’informations, consultez [à l’aide des racines de confiance salutations](http://www.mono-project.com/UsingTrustedRootsRespectfully) sur [mono-Project](http://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>À l’aide de la sécurité des informations d’identification de Client

Les services WCF peuvent également nécessiter les clients de service pour s’authentifier à l’aide des informations d’identification. La plateforme Xamarin ne prend pas en charge le protocole WS-Security, ce qui permet aux clients d’envoyer des informations d’identification à l’intérieur de l’enveloppe du message SOAP. Toutefois, la plateforme Xamarin prend en charge la capacité d’envoyer des informations d’identification de l’authentification de base HTTP sur le serveur en spécifiant la `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Ensuite, les informations d’identification de l’authentification de base peuvent être spécifiées :

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

Dans l’exemple ci-dessus, si vous recevez le message « Ran hors trampolines de type 0 » vous pouvez augmenter le nombre de trampolines de type 0 en ajoutant le `–aot “trampolines={number of trampolines}”` l’argument de la build. Pour plus d’informations, consultez [dépannage](~/ios/troubleshooting/troubleshooting.md#trampolines).

Pour plus d’informations sur l’authentification de base HTTP, bien que dans le contexte d’un service web REST, consultez [l’authentification d’un Service Web RESTful](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="summary"></a>Récapitulatif

Ce guide vous a montré comment consommer des technologies de service web différents. Rubriques couvertes incluent la communication avec les services REST, les services SOAP et les services Windows Communication Foundation.

## <a name="related-links"></a>Liens associés

- [Exemple de services Web](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Services Web dans Xamarin.Forms](~/xamarin-forms/data-cloud/index.md)
- [Service Model Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
