---
title: Accès aux données distantes
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0f29377c52dcf65247c11079178efe0745c654eb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="accessing-remote-data"></a>Accès aux données distantes

Assurez-vous de nombreuses solutions modernes basée sur le web utilisation des services web, hébergées par les serveurs web, pour fournir des fonctionnalités de client à distance des applications. Les opérations exposées par un service web constituent une API web.

Les applications clientes doivent être en mesure d’utiliser l’API web sans savoir comment les données ou les opérations qui expose de l’API sont implémentées. Cela nécessite que l’API respecte les normes communes qui permettent un service d’application et web client d’accord sur les formats de données à utiliser et la structure des données qui sont échangées entre les applications client et le service web.

## <a name="introduction-to-representational-state-transfer"></a>Introduction à Representational State Transfer

Transfert de REST (Representational State) est un style d’architecture pour la création de systèmes distribués selon hypermédia. Le principal avantage du modèle reste est qu’il est basé sur des normes ouvertes et ne lie pas l’implémentation du modèle ou les applications clientes qui y accéder à l’implémentation spécifique. Par conséquent, un service web REST peut être implémenté à l’aide de Microsoft ASP.NET MVC de base et les applications clientes pourraient être développement à l’aide de n’importe quel langage et un ensemble d’outils qui peut générer des requêtes HTTP et analyser des réponses HTTP.

Le modèle REST utilise un schéma de navigation pour représenter les objets et les services sur un réseau, appelé ressources. Les systèmes qui implémentent reste généralement utilisent le protocole HTTP pour transmettre les demandes d’accès à ces ressources. Dans ces systèmes, une application cliente soumet une demande sous la forme d’un URI qui identifie une ressource et une méthode HTTP (par exemple, GET, POST, PUT ou DELETE) qui indique l’opération à effectuer sur cette ressource. Le corps de la requête HTTP contient toutes les données requises pour effectuer l’opération.

> [!NOTE]
> REST définit un modèle de requête sans état. Par conséquent, les requêtes HTTP doivent être indépendantes et peuvent se produire dans n’importe quel ordre.

La réponse à partir d’un reste demande rend utilisent des codes d’état HTTP standards. Par exemple, une requête qui retourne des données valides doit inclure le code de réponse HTTP 200 (OK), lors d’une demande qui ne parvient pas à trouver ou de supprimer une ressource spécifiée doit renvoyer une réponse qui inclut le code d’état HTTP 404 (introuvable).

Une API de web RESTful expose un ensemble de ressources connectées et fournit les opérations principales qui permettent une application à manipuler ces ressources et de naviguer facilement entre elles. Pour cette raison, les URI qui constituent une API web RESTful classique sont orientées vers les données qu’il expose et utiliser les fonctionnalités fournies par le protocole HTTP pour intervenir sur ces données.

Les données incluses par une application cliente dans une requête HTTP et les messages de réponse correspondant à partir du serveur web, peuvent être présentées dans divers formats, appelés types de médias. Lorsqu’une application cliente envoie une requête qui retourne des données dans le corps d’un message, il peut spécifier les types de médias qu’il peut gérer dans le `Accept` en-tête de la demande. Si le serveur web prend en charge ce type de média, il peut répondre à une réponse qui inclut le `Content-Type` en-tête qui spécifie le format des données dans le corps du message. Il est ensuite la responsabilité de l’application cliente d’analyser le message de réponse et d’interpréter les résultats dans le corps du message de manière appropriée.

Pour plus d’informations sur REST, consultez [conception de l’API](/azure/architecture/best-practices/api-design/) et [implémentation de l’API](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Utilisation des API RESTful

L’application mobile eShopOnContainers utilise le modèle Model-View-ViewModel (MVVM) et les éléments de modèle des représentent le modèle utilisées par les entités de domaine dans l’application. Les classes de contrôleur et l’espace de stockage dans l’application de référence eShopOnContainers acceptent et retournent la plupart de ces objets de modèle. Par conséquent, ils sont utilisés en tant qu’objets de transfert de données (DTO) qui contiennent toutes les données qui sont transmises entre l’application mobile et le microservices en conteneur. Le principal avantage de l’utilisation de données pour passer des données et de recevoir des données à partir d’un service web est que, en transmettant plus de données dans un seul appel à distance, l’application peut réduire le nombre d’appels distants qui doivent être apportées.

### <a name="making-web-requests"></a>Effectuer des requêtes web

Les utilisations d’application mobile eShopOnContainers la `HttpClient` classe pour effectuer des demandes via HTTP, avec JSON utilisé comme type de support. Cette classe fournit une fonctionnalité pour envoyer des requêtes HTTP de façon asynchrone et recevoir des réponses HTTP à partir d’un URI identifié de ressource. La `HttpResponseMessage` classe représente un message de réponse HTTP reçu à partir d’une API REST, après la réalisation d’une requête HTTP. Il contient des informations sur la réponse, y compris le code d’état, les en-têtes et les corps. Le `HttpContent` classe représente le corps HTTP et des en-têtes de contenu, tel que `Content-Type` et `Content-Encoding`. Le contenu peut être lu à l’aide de la `ReadAs` méthodes, telles que `ReadAsStringAsync` et `ReadAsByteArrayAsync`, selon le format des données.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Une demande GET

La `CatalogService` classe est utilisée pour gérer le processus de récupération de données à partir du catalogue microservice. Dans le `RegisterDependencies` méthode dans le `ViewModelLocator` (classe), le `CatalogService` classe est enregistrée comme un mappage de type par rapport à la `ICatalogService` type avec le conteneur d’injection de dépendance Autofac. Ensuite, lorsqu’une instance de la `CatalogViewModel` son constructeur de classe est créée, accepte un `ICatalogService` de type, qui résout les Autofac, retourner une instance de la `CatalogService` classe. Pour plus d’informations sur l’injection de dépendance, consultez [Introduction à l’Injection de dépendance](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Figure 10-1 illustre l’interaction des classes qui lisent des données de catalogue à partir du catalogue microservice pour l’affichage par le `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "La récupération des données à partir du catalogue microservice")](accessing-remote-data-images/catalogdata-large.png#lightbox "la récupération des données à partir du catalogue microservice")

**Figure 10-1**: la récupération des données à partir du catalogue microservice

Lorsque le `CatalogView` est accédé, le `OnInitialize` méthode dans la `CatalogViewModel` classe est appelée. Cette méthode récupère les données du catalogue à partir du catalogue microservice, comme illustré dans l’exemple de code suivant :

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Cette méthode appelle la `GetCatalogAsync` méthode de la `CatalogService` instance a été injecté dans le `CatalogViewModel` par Autofac. Le code suivant exemple illustre la `GetCatalogAsync` méthode :

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

Cette méthode crée l’URI qui identifie la ressource de la demande va être envoyée à et utilise le `RequestProvider` classe pour appeler la méthode HTTP GET sur la ressource, avant de retourner les résultats à le `CatalogViewModel`. La `RequestProvider` classe contient des fonctionnalités qui soumet une demande sous la forme d’un URI qui identifie une ressource, une méthode HTTP qui indique l’opération à effectuer sur cette ressource, et un corps qui contient toutes les données nécessaires pour effectuer l’opération. Pour plus d’informations sur la façon dont `RequestProvider` classe est injecté dans le `CatalogService class`, consultez [Introduction à l’Injection de dépendance](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Montre l’exemple de code suit le `GetAsync` méthode dans la `RequestProvider` classe :

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Cette méthode appelle la `CreateHttpClient` méthode qui retourne une instance de la `HttpClient` classe avec le jeu d’en-têtes appropriés. Il envoie ensuite une demande GET asynchrone à la ressource identifiée par l’URI, avec la réponse qui est stockée dans le `HttpResponseMessage` instance. Le `HandleResponse` méthode est ensuite appelée, ce qui lève une exception si la réponse n’inclut pas un code d’état HTTP de réussite. Ensuite, la réponse est lu comme une chaîne convertie à partir de JSON à un `CatalogRoot` de l’objet et renvoyé à la `CatalogService`.

Le `CreateHttpClient` méthode est affichée dans l’exemple de code suivant :

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

Cette méthode crée une nouvelle instance de la `HttpClient` classe et affecte le `Accept` en-tête de toutes les demandes effectuées par le `HttpClient` instance `application/json`, ce qui signifie qu’elle attend le contenu de toute réponse à mettre en forme à l’aide de JSON. Ensuite, si un jeton d’accès a été passé comme argument à la `CreateHttpClient` (méthode), il est ajouté à la `Authorization` en-tête de toutes les demandes effectuées par le `HttpClient` instance, précédé de la chaîne `Bearer`. Pour plus d’informations sur l’autorisation, consultez [autorisation](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Lorsque le `GetAsync` méthode dans le `RequestProvider` classe appelle `HttpClient.GetAsync`, le `Items` méthode dans le `CatalogController` de classe dans le projet Catalog.API est appelée, ce qui est indiqué dans l’exemple de code suivant :

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

Cette méthode récupère les données du catalogue à partir de la base de données SQL à l’aide de EntityFramework et le retourne en tant qu’un message de réponse qui inclut un code d’état HTTP de réussite, et une collection de JSON au format `CatalogItem` instances.

#### <a name="making-a-post-request"></a>Une demande POST

La `BasketService` classe est utilisée pour gérer la récupération de données et des mises à jour avec le microservice du panier d’achat. Dans le `RegisterDependencies` méthode dans le `ViewModelLocator` (classe), le `BasketService` classe est enregistrée comme un mappage de type par rapport à la `IBasketService` type avec le conteneur d’injection de dépendance Autofac. Ensuite, lorsqu’une instance de la `BasketViewModel` son constructeur de classe est créée, accepte un `IBasketService` de type, qui résout les Autofac, retourner une instance de la `BasketService `classe. Pour plus d’informations sur l’injection de dépendance, consultez [Introduction à l’Injection de dépendance](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Figure 10-2 illustre l’interaction des classes qui envoient les données du panier d’achat affichées par le `BasketView`, à la microservice du panier d’achat.

[![](accessing-remote-data-images/basketdata.png "Envoi de données vers le panier d’achat microservice")](accessing-remote-data-images/basketdata-large.png#lightbox "envoi de données à le microservice du panier d’achat")

**Figure 10-2**: envoi de données à le microservice du panier d’achat

Lorsqu’un élément est ajouté au panier d’achat, la `ReCalculateTotalAsync` méthode dans la `BasketViewModel` classe est appelée. Cette méthode met à jour la valeur totale des éléments dans le panier et envoie les données de panier d’achat à le microservice du panier d’achat, comme illustré dans l’exemple de code suivant :

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

Cette méthode appelle la `UpdateBasketAsync` méthode de la `BasketService` instance a été injecté dans le `BasketViewModel` par Autofac. L’exemple de méthode suivant le `UpdateBasketAsync` méthode :

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Cette méthode crée l’URI qui identifie la ressource de la demande va être envoyée à et utilise le `RequestProvider` classe pour appeler la méthode HTTP POST sur la ressource, avant de retourner les résultats à le `BasketViewModel`. Notez qu’un jeton d’accès obtenu à partir de IdentityServer pendant le processus d’authentification est nécessaire pour autoriser les demandes pour le microservice du panier d’achat. Pour plus d’informations sur l’autorisation, consultez [autorisation](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

L’exemple de code suivant montre un de le `PostAsync` méthodes dans la `RequestProvider` classe :

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Cette méthode appelle la `CreateHttpClient` méthode qui retourne une instance de la `HttpClient` classe avec le jeu d’en-têtes appropriés. Il envoie ensuite une requête POST asynchrone à la ressource identifiée par l’URI, avec les données sérialisées du panier d’achat envoyées dans le format JSON et la réponse qui est stocké dans le `HttpResponseMessage` instance. Le `HandleResponse` méthode est ensuite appelée, ce qui lève une exception si la réponse n’inclut pas un code d’état HTTP de réussite. Lisez ensuite la réponse sous forme de chaîne convertie à partir de JSON pour un `CustomerBasket` de l’objet et renvoyé à le `BasketService`. Pour plus d’informations sur la `CreateHttpClient` méthode, consultez [qui effectue une demande GET](#making_a_get_request).

Lorsque le `PostAsync` méthode dans le `RequestProvider` classe appelle `HttpClient.PostAsync`, le `Post` méthode dans le `BasketController` de classe dans le projet Basket.API est appelée, ce qui est indiqué dans l’exemple de code suivant :

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Cette méthode utilise une instance de la `RedisBasketRepository` classe pour conserver les données du panier d’achat pour le cache Redis et la retourne comme un message de réponse qui inclut un code d’état HTTP de réussite et JSON au format `CustomerBasket` instance.

#### <a name="making-a-delete-request"></a>Une demande de suppression

Figure 10-3 illustre les interactions de classes qui suppriment les données du panier d’achat à partir de la microservice du panier d’achat, pour le `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Données de suppression à partir de la microservice du panier d’achat")

**Figure 10-3**: suppression de données dans le microservice du panier d’achat

Lorsque le processus de validation est appelé, le `CheckoutAsync` méthode dans la `CheckoutViewModel` classe est appelée. Cette méthode crée une nouvelle commande, avant d’en supprimer le panier d’achat, comme illustré dans l’exemple de code suivant :

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Cette méthode appelle la `ClearBasketAsync` méthode de la `BasketService` instance a été injecté dans le `CheckoutViewModel` par Autofac. L’exemple de méthode suivant le `ClearBasketAsync` méthode :

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Cette méthode crée l’URI qui identifie la ressource à laquelle la demande va être envoyée à et utilise la `RequestProvider` classe pour appeler la méthode HTTP DELETE sur la ressource. Notez qu’un jeton d’accès obtenu à partir de IdentityServer pendant le processus d’authentification est nécessaire pour autoriser les demandes pour le microservice du panier d’achat. Pour plus d’informations sur l’autorisation, consultez [autorisation](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Montre l’exemple de code suit le `DeleteAsync` méthode dans la `RequestProvider` classe :

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Cette méthode appelle la `CreateHttpClient` méthode qui retourne une instance de la `HttpClient` classe avec le jeu d’en-têtes appropriés. Puis, il soumet une demande de suppression asynchrone à la ressource identifiée par l’URI. Pour plus d’informations sur la `CreateHttpClient` méthode, consultez [qui effectue une demande GET](#making_a_get_request).

Lorsque le `DeleteAsync` méthode dans le `RequestProvider` classe appelle `HttpClient.DeleteAsync`, le `Delete` méthode dans le `BasketController` de classe dans le projet Basket.API est appelée, ce qui est indiqué dans l’exemple de code suivant :

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Cette méthode utilise une instance de la `RedisBasketRepository` classe à supprimer les données du panier d’achat à partir du cache Redis.

## <a name="caching-data"></a>Mise en cache des données

Les performances d’une application peuvent être améliorées en mettant en cache les données fréquemment sollicitées sur un stockage rapide qui se trouve fermer l’application. Si le stockage rapide se trouve proche à l’application à la source d’origine, la mise en cache peut améliorer considérablement la réponse fois lors de la récupération des données.

La forme la plus courante de mise en cache est différée mise en cache, où une application récupère des données en faisant référence à la mémoire cache. Si les données n’est pas dans le cache, il a récupéré dans le magasin de données et ajoutés au cache. Les applications peuvent implémenter différée mise en cache avec le modèle de type cache-aside. Ce modèle détermine si l’élément est actuellement dans le cache. Si l’élément n’est pas dans le cache, il a lu à partir du magasin de données et ajoutés au cache. Pour plus d’informations, consultez la [Cache-Aside](/azure/architecture/patterns/cache-aside/) modèle.

> [!TIP]
> Mettre en cache les données lues fréquemment et qui changent rarement. Ces données peuvent être ajoutées au cache à la demande la première fois, qu'il est récupéré par une application. Cela signifie que l’application a besoin pour extraire les données qu’une seule fois à partir du magasin de données, et que l’accès suivant peuvent être satisfait en utilisant le cache.

Applications distribuées, telles que les eShopOnContainers font référence à l’application, doivent fournir une ou les deux des caches suivants :

-   Un cache partagé, qui est accessible par plusieurs processus ou ordinateurs.
-   Un cache privé, où les données sont stockées localement sur l’appareil qui exécute l’application.

L’application mobile eShopOnContainers utilise un cache privé, où les données sont stockées localement sur l’appareil qui exécute une instance de l’application. Pour plus d’informations sur le cache utilisé par l’application de référence eShopOnContainers, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

> [!TIP]
> Considérez le cache comme un magasin de données temporaires qui pourrait disparaître à tout moment. Assurez-vous que les données sont conservées dans le magasin de données d’origine, ainsi que le cache. Puis de réduire les risques de perte de données si le cache n’est plus disponible.

### <a name="managing-data-expiration"></a>Expiration de la gestion des données

Il est peu pratique de vous attendre à ce que les données mises en cache seront toujours cohérentes avec les données d’origine. Données dans le magasin de données d’origine peuvent changer lorsqu’il est été mis en cache, rendant les données mises en cache expirent. Par conséquent, les applications doivent implémenter une stratégie qui permet de se pour assurer que les données dans le cache sont aussi à jour que possible, mais peut également détecter et gérer les situations qui surviennent lorsque les données dans le cache sont devenue obsolètes. Mécanismes de mise en cache plus activer le cache à être configuré pour faire expirer les données et par conséquent, réduisent la période pendant laquelle les données peuvent être obsolètes.

> [!TIP]
> Définir une date d’expiration par défaut lors de la configuration d’un cache de temps. Nombre de caches implémentent d’expiration, ce qui invalide les données et supprime du cache si elle n’est pas accessible pour une période spécifiée. Toutefois, être vigilant lors du choix de la période d’expiration. S’il s’agit trop court, données expire trop rapidement, et les avantages de la mise en cache seront réduits. S’il s’agit trop long, les risques de données devient obsolète. Par conséquent, le délai d’expiration doit mettre en correspondance de l’accès pour les applications qui utilisent les données.

Lorsque les données mises en cache expirent, il doit être supprimé du cache, et l’application doit récupérer les magasin de données à partir des données d’origine et placez-le dans le cache.

Il est également possible qu’un cache peut remplir si les données sont autorisées à rester pour une période trop longue. Par conséquent, les demandes pour ajouter de nouveaux éléments au cache peuvent être nécessaires pour supprimer des éléments dans un processus appelé *éviction*. Services de mise en cache supprimez généralement les données selon le moins récemment utilisé. Toutefois, il existe des autres stratégies d’éviction, y compris les plus récemment utilisés et in-first-out. Pour plus d’informations, consultez [Guide de mise en cache](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Mise en cache d’Images

L’application mobile eShopOnContainers consomme les images de produit à distance qui bénéficient de la mise en cache. Ces images sont affichés par le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) (contrôle) et le `CachedImage` contrôle fourni par le [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) bibliothèque.

Le Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) contrôle prend en charge la mise en cache des images téléchargées. La mise en cache est activée par défaut et stocke l’image localement pendant 24 heures. En outre, le délai d’expiration peut être configuré avec le [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) propriété. Pour plus d’informations, consultez [téléchargé la mise en cache d’Image](~/xamarin-forms/user-interface/images.md#Image_Caching).

De FFImageLoading `CachedImage` contrôle est un remplacement pour le Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) contrôle, en fournissant des propriétés supplémentaires qui activent des fonctionnalités supplémentaires. Dans la liste de cette fonctionnalité, le contrôle fournit une mise en cache configurable, lors de la prise en charge d’erreur et le chargement des espaces réservés de l’image. L’exemple de code suivant montre comment l’application mobile eShopOnContainers utilise le `CachedImage` contrôler dans le `ProductTemplate`, qui est le modèle de données utilisé par le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) contrôler dans le `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP, WinRT, WinPhone" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP, WinRT, WinPhone" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

Le `CachedImage` contrôle définit le `LoadingPlaceholder` et `ErrorPlaceholder` propriétés pour les images propres à la plateforme. Le `LoadingPlaceholder` propriété spécifie l’image à afficher lors de l’image spécifiée par la `Source` propriété est récupérée et le `ErrorPlaceholder` propriété spécifie l’image à afficher si une erreur se produit lorsque vous tentez de récupérer l’image spécifié par le `Source` propriété.

Comme son nom l’indique, le `CachedImage` contrôle met en cache des images à distance sur l’appareil pendant la durée spécifiée par la valeur de la `CacheDuration` propriété. Lorsque cette valeur de propriété n’est pas définie explicitement, la valeur par défaut de 30 jours est appliquée.

## <a name="increasing-resilience"></a>Augmentation de résilience

Toutes les applications qui communiquent avec les ressources et les services à distance doivent être sensibles aux erreurs temporaires. Erreurs transitoires incluent la perte momentanée de la connectivité réseau pour les services, l’indisponibilité temporaire d’un service ou les délais d’attente qui surviennent lorsqu’un service est occupé. Ces erreurs sont souvent correction automatique, et si l’action est répétée après un délai approprié, il est susceptible de réussir.

Erreurs temporaires peuvent avoir un impact important sur la qualité d’une application, même s’il a été soigneusement testé dans toutes les circonstances prévisibles. Pour vous assurer qu’une application qui communique avec les services à distance fonctionne de manière fiable, il doit être en mesure d’effectuer toutes les opérations suivantes :

-   Détecter des erreurs lorsqu’ils se produisent et déterminent si les erreurs sont susceptibles d’être temporaire.
-   Recommencez l’opération s’il détermine que l’erreur est susceptible d’être temporaire et de suivre le nombre de fois où que l’opération a été tentée.
-   Utiliser une stratégie de nouvelle tentative appropriée, qui spécifie le nombre de nouvelles tentatives, le délai entre chaque tentative et les actions à entreprendre après l’échec d’une tentative.

Cette gestion d’erreurs transitoires peut être obtenue en encapsulant toutes les tentatives d’accéder à un service distant dans le code qui implémente le modèle de nouvelle tentative.

### <a name="retry-pattern"></a>Réessayez de modèle

Si une application détecte un échec lorsqu’il tente d’envoyer une demande à un service distant, il peut gérer l’échec d’une des manières suivantes :

-   Recommencer l’opération. L’application peut réessayer la demande échouer immédiatement.
-   Recommencer l’opération après un certain délai. L’application doit attendre un délai approprié avant de réessayer la demande.
-   L’annulation de l’opération. L’application doit annuler l’opération et une exception de rapports.

La stratégie de nouvelle tentative doit être réglée pour faire correspondre les besoins de l’application. Par exemple, il est important optimiser le nombre de tentatives et l’intervalle de l’opération en cours a tenté de nouvelle tentative. Si l’opération fait partie d’une intervention de l’utilisateur, l’intervalle avant nouvelle tentative doit être court et seuls quelques tentatives pour éviter d’effectuer les utilisateurs à attendre une réponse. Si l’opération est la partie d’un workflow en cours d’exécution longue, où l’annulation ou redémarrer le workflow est coûteuse ou qui prennent du temps, il convient pour attendre plus longtemps entre les tentatives et plusieurs tentatives.

> [!NOTE]
> Une stratégie agressive de nouvelle tentative avec un délai minimal entre les tentatives et un grand nombre de nouvelles tentatives, pourrait nuire à un service distant qui exécute fermer ou atteint la capacité maximale. En outre, une telle stratégie de nouvelle tentative peut également affecter la réactivité de l’application si elle essaie en permanence effectuer une opération de basculement.

Si une demande échoue encore après un certain nombre de nouvelles tentatives, il est préférable de l’application pour empêcher les autres demandes accédant à la même ressource et signaler un échec. Ensuite, après une période définie, l’application peut effectuer une ou plusieurs demandes à la ressource pour voir si elles sont réussies. Pour plus d’informations, consultez [modèle de disjoncteur](#circuit_breaker_pattern).

> [!TIP]
> Jamais implémenter un mécanisme de nouvelle tentative infinie. Utiliser un nombre fini de nouvelles tentatives, ou implémenter la [disjoncteur](/azure/architecture/patterns/circuit-breaker/) modèle pour autoriser un service à récupérer.

L’application mobile eShopOnContainers n’implémente pas actuellement le modèle de nouvelle tentative lors de la création de demandes web RESTful. Toutefois, le `CachedImage` contrôle, fourni par le [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) bibliothèque prend en charge la gestion d’erreurs transitoires par une nouvelle tentative de chargement de l’image. En cas de chargement de l’image, autre tentative sera effectuée. Le nombre de tentatives spécifié par le `RetryCount` de propriété et de nouvelles tentatives se produit après un délai spécifié par le `RetryDelay` propriété. Si ces valeurs de propriété ne sont pas définies explicitement, par défaut les valeurs sont appliquées – 3 pour le `RetryCount` propriété et 250ms pour le `RetryDelay` propriété. Pour plus d’informations sur la `CachedImage` du contrôle, consultez [mise en cache des Images](#caching_images).

L’application de référence eShopOnContainers implémente le modèle de nouvelle tentative. Pour plus d’informations, y compris une présentation de la combinaison du modèle de nouvelle tentative avec le `HttpClient` de classe, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

Pour plus d’informations sur le modèle de nouvelle tentative, consultez la [réessayer](/azure/architecture/patterns/retry/) modèle.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Modèle de disjoncteur

Dans certains cas, les erreurs peuvent se produire en raison d’événements anticipés qui prennent plus de temps à corriger. Ces erreurs peuvent varier d’une perte partielle de connectivité à la défaillance complète d’un service. Dans ces situations, il est inutile d’une application retenter une opération qui est peu probable que réussisse et à la place d’accepter que l’opération a échoué et de gérer en conséquence de cette défaillance.

Modèle de disjoncteur peut empêcher une application à plusieurs reprises exécuter une opération qui risque d’échouer, tout en permettant également l’application pour détecter si l’erreur a été résolu.

> [!NOTE]
> L’objectif du modèle de disjoncteur est différent du modèle de nouvelle tentative. Le modèle de nouvelle tentative permet à une application retenter une opération dans l’attente elle réussira. Modèle de disjoncteur empêche l’application d’une opération qui risque d’échouer.

Un disjoncteur agit comme un proxy pour les opérations qui risque d’échouer. Le serveur proxy doit surveiller le nombre d’échecs récents qui se sont produites et utiliser ces informations pour décider s’il faut autoriser l’opération pour continuer, ou pour retourner une exception immédiatement.

Actuellement, l’application mobile eShopOnContainers n’implémente pas le modèle de disjoncteur. Toutefois, l’eShopOnContainers est effectuée. Pour plus d’informations, consultez [.NET Microservices : Architecture pour les Applications .NET en conteneur](https://aka.ms/microservicesebook).

> [!TIP]
> Combiner les modèles de disjoncteur et réessayez. Une application peut combiner les modèles de nouvelle tentative et disjoncteur à l’aide du modèle de nouvelle tentative pour appeler une opération via un disjoncteur. Toutefois, la logique de nouvelle tentative doit être sensibles à toutes les exceptions retournées par le disjoncteur et abandonner les nouvelles tentatives si le disjoncteur indique qu’une erreur n’est pas temporaire.

Pour plus d’informations sur le modèle de disjoncteur, consultez le [disjoncteur](/azure/architecture/patterns/circuit-breaker/) modèle.

## <a name="summary"></a>Récapitulatif

Assurez-vous de nombreuses solutions modernes basée sur le web utilisation des services web, hébergées par les serveurs web, pour fournir des fonctionnalités de client à distance des applications. Les opérations exposées par un service web constituent une API web et les applications clientes doivent être en mesure d’utiliser l’API web sans savoir comment les données ou les opérations qui expose de l’API sont implémentées.

Les performances d’une application peuvent être améliorées en mettant en cache les données fréquemment sollicitées sur un stockage rapide qui se trouve fermer l’application. Les applications peuvent implémenter différée mise en cache avec le modèle de type cache-aside. Ce modèle détermine si l’élément est actuellement dans le cache. Si l’élément n’est pas dans le cache, il a lu à partir du magasin de données et ajoutés au cache.

Lors de la communication avec l’API web, les applications doivent être sensibles aux erreurs temporaires. Erreurs transitoires incluent la perte momentanée de la connectivité réseau pour les services, l’indisponibilité temporaire d’un service ou les délais d’attente qui surviennent lorsqu’un service est occupé. Ces erreurs sont souvent correction automatique, et si l’action est répétée après un délai approprié, puis il est susceptible de réussir. Par conséquent, les applications doivent encapsuler toutes les tentatives d’accéder à une API web dans le code qui implémente un mécanisme de gestion d’erreurs transitoires.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
