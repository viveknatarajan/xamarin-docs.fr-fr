---
title: Authentification et autorisation
ms.topic: article
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 16cd5571a8c3b0e179046351e9996e55b040a9de
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/23/2018
---
# <a name="authentication-and-authorization"></a>Authentification et autorisation

L’authentification est le processus d’obtention des informations d’identification telles que le nom et le mot de passe d’un utilisateur et de validation de ces informations d’identification par rapport à une autorité. Si les informations d’identification sont valides, l’entité qui a envoyé les informations d’identification est considérée comme une identité authentifiée. Une fois qu’une identité a été authentifiée, un processus d’autorisation détermine si cette identité a accès à une ressource donnée.

Il existe de nombreuses méthodes d’intégration d’authentification et autorisation dans une application Xamarin.Forms qui communique avec une application web ASP.NET MVC, y compris à l’aide d’ASP.NET Core Identity, fournisseurs d’authentification externes telles que Microsoft, Google, Intergiciel (middleware) Twitter ou Facebook et d’authentification. L’application mobile eShopOnContainers effectue l’authentification et autorisation avec un microservice d’identité en conteneur qui utilise IdentityServer 4. L’application mobile demande des jetons de sécurité IdentityServer, pour authentifier un utilisateur ou pour accéder à une ressource. Pour IdentityServer pour émettre des jetons pour le compte d’un utilisateur, l’utilisateur doit connectez-vous au IdentityServer. Toutefois, IdentityServer ne fournit pas une interface utilisateur ou la base de données pour l’authentification. Par conséquent, dans l’application de référence eShopOnContainers, ASP.NET Core Identity est utilisé à cet effet.

## <a name="authentication"></a>Authentification

L’authentification est requise lorsqu’une application doit connaître l’identité de l’utilisateur actuel. Mécanisme principal d’ASP.NET Core identification des utilisateurs est le système d’appartenance ASP.NET Core Identity, qui stocke les informations utilisateur dans un magasin de données configuré par le développeur. En règle générale, cette banque de données sera un magasin EntityFramework, bien que les magasins personnalisés ou des packages tiers peuvent être utilisés pour stocker les informations d’identité dans le stockage Azure, base de données Azure Cosmos ou d’autres emplacements.

Pour les scénarios d’authentification qui utilisent un magasin de données utilisateur local, et qui rendre persistantes les informations d’identité entre les demandes via les cookies (comme souvent dans les applications web ASP.NET MVC), ASP.NET Core Identity est une solution adaptée. Toutefois, les cookies ne sont pas toujours des moyens naturels de persistance et de transmission de données. Par exemple, une application web ASP.NET Core qui expose des points de terminaison RESTful qui sont accessibles à partir d’une application mobile devrez généralement utiliser l’authentification de jeton de porteur, étant donné que les cookies ne peuvent pas être utilisés dans ce scénario. Toutefois, les jetons de porteur peuvent facilement être récupérées et inclus dans l’en-tête d’autorisation de requêtes web à partir de l’application mobile.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Émission de jetons de support à l’aide de IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) est une infrastructure open source qui OpenID Connect et OAuth 2.0 pour ASP.NET Core, qui peut être utilisé pour de nombreux scénarios d’authentification et d’autorisation, y compris l’émission de jetons de sécurité pour les utilisateurs ASP.NET Core Identity locaux.

> [!NOTE]
> OpenID Connect et OAuth 2.0 sont très semblables, tout en ayant des responsabilités différentes.

OpenID Connect est une couche de l’authentification sur le protocole OAuth 2.0. OAuth 2 est un protocole qui permet aux applications de demander des jetons d’accès à partir d’un service de jeton de sécurité et les utiliser pour communiquer avec les API. Cette délégation réduit la complexité dans les API et les applications clientes étant donné que l’authentification et l’autorisation peuvent être centralisées.

La combinaison de OpenID Connect et OAuth 2.0 combiner les deux problèmes fondamentaux de la sécurité d’authentification et l’accès aux API et IdentityServer 4 est une implémentation de ces protocoles.

Dans les applications qui utilisent la communication client à microservice directe, telles que l’application de référence eShopOnContainers, un microservice authentification dédiée qui agit comme un Service STS (Security Token) peut servir à authentifier les utilisateurs, comme indiqué dans l’illustration 9-1. Pour plus d’informations sur la communication client à microservice directe, consultez [Communication entre le Client et Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Authentification en un microservice dédié de l’authentification")

**La figure 9-1 :** l’authentification en un microservice dédié de l’authentification

L’application mobile eShopOnContainers communique avec l’identité microservice, qui utilise des IdentityServer 4 pour effectuer l’authentification et contrôle d’accès pour les API. Par conséquent, l’application mobile demande des jetons à partir de IdentityServer, pour authentifier un utilisateur ou pour accéder à une ressource :

-   L’authentification des utilisateurs avec IdentityServer est obtenue par l’application mobile demande un *identité* jeton qui représente le résultat d’un processus d’authentification. Au minimum, il contient un identificateur pour l’utilisateur et des informations sur comment et quand l’utilisateur authentifié. Il peut également contenir des données d’identité supplémentaires.
-   L’accès à une ressource avec IdentityServer est obtenue par l’application mobile demande un *accès* jeton, ce qui permet d’accéder à une ressource de l’API. Clients de demander des jetons d’accès et les transfèrent à l’API. Les jetons d’accès contiennent des informations sur le client et l’utilisateur (le cas échéant). Ensuite, les API utiliser ces informations pour autoriser l’accès à leurs données.

> [!NOTE]
> Il peut demander des jetons, un client doit être inscrit avec IdentityServer.

### <a name="adding-identityserver-to-a-web-application"></a>Ajout de IdentityServer à une Application Web

Dans l’ordre pour une application web ASP.NET Core utilisent IdentityServer 4, il doit être ajouté à la solution Visual Studio de l’application web. Pour plus d’informations, consultez [vue d’ensemble et le programme d’installation](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) dans la documentation IdentityServer.

Une fois que IdentityServer est inclus dans la solution Visual Studio de l’application web, il doit être ajouté à la requête HTTP de l’application web du traitement de pipeline, afin qu’il peut traiter les demandes pour les points de terminaison OpenID Connect et OAuth 2.0. Cela est possible dans le `Configure` méthode dans l’application web `Startup` classe, comme illustré dans l’exemple de code suivant :

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Ordre est important dans la requête HTTP de l’application web pipeline de traitement. Par conséquent, IdentityServer doit être ajouté au pipeline avant l’infrastructure d’interface utilisateur qui implémente l’écran de connexion.

### <a name="configuring-identityserver"></a>Configuration IdentityServer

IdentityServer doit être configuré dans le `ConfigureServices` méthode dans l’application web `Startup` classe en appelant le `services.AddIdentityServer` méthode, comme illustré dans l’exemple de code suivant à partir de l’application de référence eShopOnContainers :

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

Après avoir appelé la `services.AddIdentityServer` (méthode), l’API fluent supplémentaires sont appelées pour configurer les éléments suivants :

-   Informations d’identification utilisées pour la signature.
-   API et l’identité des ressources que les utilisateurs peuvent demander l’accès.
-   Clients qui seront connectent à la demande de jetons.
-   Identité de ASP.NET Core.

>💡 **Conseil**: charger dynamiquement la configuration IdentityServer 4. API de 4 IdentityServer permettent la configuration IdentityServer à partir d’une liste en mémoire des objets de configuration. Dans l’application de référence eShopOnContainers, ces collections en mémoire sont codés en dur dans l’application. Toutefois, dans les scénarios de production ils peuvent être chargés dynamiquement à partir d’un fichier de configuration ou à partir d’une base de données.

Pour plus d’informations sur la configuration IdentityServer pour utiliser ASP.NET Core identité, consultez [à l’aide de ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) dans la documentation IdentityServer.

#### <a name="configuring-api-resources"></a>Configuration des ressources d’API

Lors de la configuration des ressources d’API, le `AddInMemoryApiResources` méthode attend un `IEnumerable<ApiResource>` collection. Montre l’exemple de code suit le `GetApis` méthode qui fournit cette collection dans l’eShopOnContainers référencer l’application :

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Cette méthode spécifie que IdentityServer doivent protéger les commandes et les API de panier. Par conséquent, les accès géré IdentityServer jetons sera nécessaire lors d’appels à ces API. Pour plus d’informations sur la `ApiResource` de type, consultez [ressource de l’API](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) dans la documentation IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Configuration des ressources d’identité

Lors de la configuration des ressources de l’identité, le `AddInMemoryIdentityResources` méthode attend un `IEnumerable<IdentityResource>` collection. Les ressources d’identité sont des données telles que l’ID d’utilisateur, nom ou adresse de messagerie. Chaque ressource de l’identité a un nom unique et revendication arbitraire types peuvent être affectés à ce dernier, qui sont alors incluses dans le jeton d’identité de l’utilisateur. Montre l’exemple de code suit le `GetResources` méthode qui fournit cette collection dans l’eShopOnContainers référencer l’application :

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

La spécification OpenID Connect spécifie certaines [ressources identity standard](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). La configuration minimale requise est que la prise en charge est fournie pour l’émission d’un ID unique pour les utilisateurs. Cela est possible en exposant les `IdentityResources.OpenId` ressource d’identité.

> [!NOTE]
> La `IdentityResources` classe prend en charge toutes les étendues définies dans la spécification OpenID Connect (openid, par courrier électronique, profil, téléphone et adresse).

IdentityServer prend également en charge la définition des ressources d’identité personnalisé. Pour plus d’informations, consultez [définition des ressources d’identité personnalisé](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) dans la documentation IdentityServer. Pour plus d’informations sur la `IdentityResource` de type, consultez [ressource identité](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) dans la documentation IdentityServer 4.

#### <a name="configuring-clients"></a>Configuration des Clients

Les clients sont des applications qui peuvent demander des jetons à partir de IdentityServer. En règle générale, les paramètres suivants doivent être définis pour chaque client à une valeur minimale :

-   Un ID client unique.
-   Les interactions autorisées avec le service de jeton (connue en tant que le type d’accès).
-   L’emplacement où sont envoyés les jetons d’accès et des identités (appelé un URI de redirection).
-   Une liste de ressources que le client est autorisé à accéder à (appelée étendues).

Lors de la configuration des clients, le `AddInMemoryClients` méthode attend un `IEnumerable<Client>` collection. L’exemple de code suivant montre la configuration de l’application mobile eShopOnContainers dans le `GetClients` méthode qui fournit cette collection dans l’eShopOnContainers référencer l’application :

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Cette configuration spécifie les données pour les propriétés suivantes :

-   `ClientId`: Un ID unique pour le client.
-   `ClientName`: Le nom complet de client, qui est utilisé pour la journalisation et l’écran de consentement.
-   `AllowedGrantTypes`: Spécifie la manière dont un client souhaite interagir avec IdentityServer. Pour plus d’informations, consultez [configurer le flux d’authentification](#configuring_the_authentication_flow).
-   `ClientSecrets`: Spécifie des informations d’identification secrète de client qui sont utilisées lors de la demande de jetons du point de terminaison de jeton.
-   `RedirectUris`: Spécifie l’URI autorisé pour lequel renvoyer des jetons ou des codes d’autorisation.
-   `RequireConsent`: Spécifie si un écran de consentement est requis.
-   `RequirePkce`: Spécifie si les clients à l’aide d’un code d’autorisation doivent envoyer une clé de vérification.
-   `PostLogoutRedirectUris`: Spécifie l’URI autorisé pour rediriger vers après la déconnexion.
-   `AllowedCorsOrigins`: Spécifie l’origine du client afin que IdentityServer peut autoriser des appels cross-origine à partir de l’origine.
-   `AllowedScopes`: Spécifie les ressources que le client a accès à. Par défaut, un client a pas accès à toutes les ressources.
-   `AllowOfflineAccess`: Spécifie si le client peut demander des jetons d’actualisation.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Configuration du flux d’authentification

Le flux d’authentification entre un client et un IdentityServer peut être configuré en spécifiant les types de licence dans le `Client.AllowedGrantTypes` propriété. Les spécifications OpenID Connect et OAuth 2.0 définissent un nombre de flux d’authentification, y compris :

-   Implicite. Ce flux est optimisé pour les applications basées sur un navigateur et doit être utilisé pour l’utilisateur d’authentification uniquement ou les demandes de jeton d’authentification et d’accès. Tous les jetons sont transmis via le navigateur et par conséquent avancés des fonctionnalités telles que les jetons d’actualisation ne sont pas autorisées.
-   Code d’autorisation. Ce flux offre la possibilité de récupérer des jetons sur un canal arrière, plutôt que le canal avant de navigateur, également la prise en charge l’authentification du client.
-   Hybride. Ce flux est une combinaison d’implicite et les types d’accès de code d’autorisation. Le jeton d’identité est transmis via le canal de navigateur et contient la réponse du protocole signé, ainsi que d’autres artefacts tels que le code d’autorisation. Après une validation réussie de la réponse, le canal arrière doit être utilisé pour récupérer l’accès et le jeton d’actualisation.

> [!TIP]
> Utilisez le flux d’authentification hybride. Le flux d’authentification hybride permet d’atténuer de nombreuses attaques qui s’appliquent à la chaîne du navigateur et est le flux recommandé pour les applications natives que souhaitez récupérer des jetons d’accès (et éventuellement les jetons d’actualisation).

Pour plus d’informations sur les flux d’authentification, consultez [Types d’accès](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) dans la documentation IdentityServer 4.

### <a name="performing-authentication"></a>Exécution de l’authentification

Pour IdentityServer pour émettre des jetons pour le compte d’un utilisateur, l’utilisateur doit connectez-vous au IdentityServer. Toutefois, IdentityServer ne fournit pas une interface utilisateur ou la base de données pour l’authentification. Par conséquent, dans l’application de référence eShopOnContainers, ASP.NET Core Identity est utilisé à cet effet.

L’application mobile eShopOnContainers authentifie avec IdentityServer avec le flux d’authentification hybride, qui est illustré dans la Figure 9-2.

![](authentication-and-authorization-images/sign-in.png "Vue d’ensemble du processus de connexion")

**Figure 9-2 :** vue d’ensemble du processus de connexion

Une demande de connexion est effectuée à `<base endpoint>:5105/connect/authorize`. Après une authentification réussie, IdentityServer renvoie une réponse d’authentification contenant un code d’autorisation et un jeton d’identité. Le code d’autorisation est ensuite envoyé au `<base endpoint>:5105/connect/token`, qui répond avec l’accès, d’identité et des jetons d’actualisation.

EShopOnContainers application mobile signes-hors IdentityServer en envoyant une demande à `<base endpoint>:5105/connect/endsession`, avec des paramètres supplémentaires. Après la déconnexion, IdentityServer répond en envoyant un URI de redirection de déconnexion post vers l’application mobile. Figure 9-3 illustre ce processus.

![](authentication-and-authorization-images/sign-out.png "Vue d’ensemble du processus de déconnexion")

**Figure 9-3 :** vue d’ensemble du processus de déconnexion

Dans l’application mobile eShopOnContainers, la communication avec IdentityServer est effectuée par le `IdentityService` classe qui implémente le `IIdentityService` interface. Cette interface spécifie que la classe d’implémentation doit fournir `CreateAuthorizationRequest`, `CreateLogoutRequest`, et `GetTokenAsync` méthodes.

#### <a name="signing-in"></a>Ouverture de session

Lorsque l’utilisateur appuie sur le **connexion** bouton sur le `LoginView`, le `SignInCommand` dans le `LoginViewModel` classe est exécutée, qui exécute à son tour le `SignInAsync` (méthode). L’exemple de code suivant illustre cette méthode :

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Cette méthode appelle la `CreateAuthorizationRequest` méthode dans le `IdentityService` (classe), qui est affichée dans l’exemple de code suivant :

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Cette méthode crée l’URI pour IdentityServer [point de terminaison d’autorisation](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), avec les paramètres requis. Le point de terminaison d’autorisation est à `/connect/authorize` sur le port 5105 du point de terminaison base exposée comme un paramètre utilisateur. Pour plus d’informations sur les paramètres utilisateur, consultez [gestion de la Configuration](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> La surface d’attaque de l’application mobile eShopOnContainers est réduite en implémentant la clé de preuve pour l’extension de Code Exchange (PKCE) pour OAuth. PKCE protège le code d’autorisation d’être utilisée si elle est interceptée. Pour cela, le client de génération d’un vérificateur de secret principal, un hachage qui est passé dans la demande d’autorisation, et qui est présentée non hachée en échangeant le code d’autorisation. Pour plus d’informations sur PKCE, consultez [clé de preuve pour l’échange de Code par les Clients publics OAuth](https://tools.ietf.org/html/rfc7636) sur le site web Internet Engineering Task Force.

L’URI retourné est stocké dans le `LoginUrl` propriété de la `LoginViewModel` classe. Lorsque le `IsLogin` propriété devient `true`, le [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) dans le `LoginView` devient visible. Le `WebView` lie son [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) propriété le `LoginUrl` propriété de la `LoginViewModel` classe et par conséquent, est une demande de connexion à IdentityServer lorsque le `LoginUrl` est définie sur Point de terminaison d’autorisation de IdentityServer. Lorsque IdentityServer reçoit cette demande et de l’utilisateur n’est pas authentifié, le `WebView` sera redirigé vers la page de connexion configurée, ce qui est indiquée dans la Figure 9-4.

![](authentication-and-authorization-images/login.png "Page de connexion affichée par le mode d’affichage Web")

**Figure 9-4 :** page de connexion affichée par le mode d’affichage Web

Une fois que la connexion est terminée, le [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) sont redirigées vers un URI de retour. Cela `WebView` navigation entraîne la `NavigateAsync` méthode dans la `LoginViewModel` classe doit être exécuté, ce qui est indiqué dans l’exemple de code suivant :

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Cette méthode analyse la réponse d’authentification qui est contenue dans l’URI de retour, et si un code d’autorisation valide est présent, il effectue une demande à IdentityServer [point de terminaison de jeton](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), en passant le code d’autorisation, le Vérificateur de PKCE secret principal et d’autres paramètres sont obligatoires. Le point de terminaison de jeton est à `/connect/token` sur le port 5105 du point de terminaison base exposée comme un paramètre utilisateur. Pour plus d’informations sur les paramètres utilisateur, consultez [gestion de la Configuration](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Conseil**: valider retourner l’URI. Bien que l’application mobile eShopOnContainers ne valide pas l’URI de retour, la meilleure pratique consiste à valider que le retour URI fait référence à un emplacement connu, pour empêcher les attaques de redirection de l’ouvrir.

Si le point de terminaison de jeton reçoit un code d’autorisation valide et le vérificateur de secret principal PKCE, il répond avec un jeton d’accès, le jeton d’identité et le jeton d’actualisation. Le jeton d’accès (ce qui permet d’accéder aux ressources d’API) et un jeton d’identité sont ensuite stockées en tant que paramètres de l’application, et la navigation entre les pages est effectuée. Par conséquent, l’impact global de l’application mobile eShopOnContainers s’agit-il : condition que les utilisateurs peuvent s’authentifier correctement avec IdentityServer, navigation vers le `MainView` page, qui est un [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) qui affiche le `CatalogView` en tant que son onglet sélectionné.

Pour plus d’informations sur la navigation entre les pages, consultez [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Pour plus d’informations sur la façon [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) navigation provoque une méthode de modèle de vue être exécutée, consultez [à l’aide de comportements de Navigation appel](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Pour plus d’informations sur les paramètres de l’application, consultez [gestion de la Configuration](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> L’eShopOnContainers permet également une connexion à fictive lors de l’application est configurée pour utiliser les services fictives dans le `SettingsView`. Dans ce mode, l’application ne communique pas avec IdentityServer, à la place pour autoriser l’utilisateur à se connecter à l’aide des informations d’identification.

#### <a name="signing-out"></a>Hors connexion

Lorsque l’utilisateur appuie sur le **LOG OUT** situé dans le `ProfileView`, le `LogoutCommand` dans le `ProfileViewModel` classe est exécutée, qui exécute à son tour le `LogoutAsync` (méthode). Cette méthode exécute la navigation entre les pages à la `LoginView` page, en passant un `LogoutParameter` la valeur instance `true` en tant que paramètre. Pour plus d’informations sur le passage de paramètres lors de la navigation de page, consultez [en passant les paramètres au cours de Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Lorsqu’une vue est créée et cible, le `InitializeAsync` méthode du modèle de vue associée de la vue est exécutée, qui exécute ensuite le `Logout` méthode de la `LoginViewModel` (classe), qui est affichée dans l’exemple de code suivant :

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Cette méthode appelle la `CreateLogoutRequest` méthode dans la `IdentityService` classe, en passant le jeton d’identité est récupérée à partir des paramètres de l’application en tant que paramètre. Pour plus d’informations sur les paramètres de l’application, consultez [gestion de la Configuration](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Le code suivant exemple illustre la `CreateLogoutRequest` méthode :

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Cette méthode crée l’URI de IdentityServer [fin du point de terminaison de session](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), avec les paramètres requis. Le point de terminaison de session de fin est à `/connect/endsession` sur le port 5105 du point de terminaison base exposée comme un paramètre utilisateur. Pour plus d’informations sur les paramètres utilisateur, consultez [gestion de la Configuration](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

L’URI retourné est stocké dans le `LoginUrl` propriété de la `LoginViewModel` classe. Alors que le `IsLogin` propriété est `true`, le [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) dans le `LoginView` est visible. Le `WebView` lie son [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) propriété le `LoginUrl` propriété de la `LoginViewModel` classe et par conséquent, est une demande de déconnexion à IdentityServer lorsque le `LoginUrl` est définie sur Point de terminaison de IdentityServer fin session. Lorsque IdentityServer reçoit cette demande, sous réserve que l’utilisateur est connecté, déconnexion se produit. Le suivi de l’authentification est effectuée avec un cookie géré par l’intergiciel (middleware) d’authentification de cookie à partir de ASP.NET. Par conséquent, la déconnexion de IdentityServer supprime le cookie d’authentification et envoie une URI au client de redirection après déconnexion.

Dans l’application mobile, le [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) sera redirigé vers l’URI de redirection de déconnexion post. Cela `WebView` navigation entraîne la `NavigateAsync` méthode dans la `LoginViewModel` classe doit être exécuté, ce qui est indiqué dans l’exemple de code suivant :

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Cette méthode efface le jeton d’identité et le jeton d’accès à partir des paramètres d’application et définit le `IsLogin` propriété `false`, ce qui entraîne la [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) sur le `LoginView` page occupait . Enfin, le `LoginUrl` est définie sur l’URI de IdentityServer [point de terminaison d’autorisation](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), avec les paramètres requis, en vue de la prochaine fois que l’utilisateur lance une connexion à.

Pour plus d’informations sur la navigation entre les pages, consultez [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Pour plus d’informations sur la façon [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) navigation provoque une méthode de modèle de vue être exécutée, consultez [à l’aide de comportements de Navigation appel](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Pour plus d’informations sur les paramètres de l’application, consultez [gestion de la Configuration](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> L’eShopOnContainers permet également un fictifs déconnexion lorsque l’application est configurée pour utiliser les services fictives de la SettingsView. Dans ce mode, l’application ne communique pas avec IdentityServer et au lieu de cela efface tous les jetons stockées à partir des paramètres de l’application.

<a name="authorization" />

## <a name="authorization"></a>Autorisation

Après l’authentification, web ASP.NET Core API doivent souvent pour autoriser l’accès, qui permet à un service rendre les API disponibles pour certains utilisateurs authentifiés, mais pas à l’ensemble.

Restreindre l’accès à un itinéraire ASP.NET MVC de base peut être obtenue en appliquant un attribut de l’autoriser à un contrôleur ou d’action, ce qui limite l’accès au contrôleur ou à des utilisateurs authentifiés, comme indiqué dans l’exemple de code suivant :

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Si un utilisateur non autorisé tente d’accéder à un contrôleur ou une action qui est marquée avec la `Authorize` attribut, l’infrastructure MVC retourne un code d’état HTTP 401 (non autorisé).

> [!NOTE]
> Les paramètres peuvent être spécifiés sur la `Authorize` attribut pour restreindre une API à des utilisateurs spécifiques. Pour plus d’informations, consultez [autorisation](/aspnet/core/security/authorization/introduction/).

IdentityServer peut être intégré dans le flux de travail d’autorisation afin que les jetons d’accès, il fournit l’autorisation de contrôle. Cette approche est illustrée dans la Figure 9-5.

![](authentication-and-authorization-images/authorization.png "Autorisation de jeton d’accès")

**Figure 9-5 :** autorisation de jeton d’accès

L’application mobile eShopOnContainers communique avec l’identité microservice et demande un jeton d’accès dans le cadre du processus d’authentification. Le jeton d’accès est ensuite transmis aux API exposées par le microservices du panier d’achat et de classement dans le cadre des demandes d’accès. Les jetons d’accès contiennent des informations sur le client et l’utilisateur. Ensuite, les API utiliser ces informations pour autoriser l’accès à leurs données. Pour plus d’informations sur la configuration IdentityServer pour protéger des API, consultez [configuration des ressources d’API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Configuration IdentityServer d’octroyer des autorisations

Pour effectuer une autorisation avec IdentityServer, son intergiciel (middleware) d’autorisation doit être ajouté au pipeline de demande HTTP de l’application web. L’intergiciel (middleware) est ajouté dans le `ConfigureAuth` méthode dans l’application web `Startup` (classe), qui est appelé à partir de la `Configure` (méthode) et est illustrée dans l’exemple de code suivant à partir de l’application de référence eShopOnContainers :

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Cette méthode garantit que l’API n’est accessible avec un jeton d’accès valide. L’intergiciel (middleware) valide le jeton entrant pour vous assurer qu’il est envoyé à partir d’un émetteur approuvé et vérifie que le jeton est valide pour être utilisé avec l’API qui le reçoit. Par conséquent, l’exploration vers le contrôleur de classement ou du panier d’achat retournera 401 (non autorisé) code d’état HTTP, indiquant qu’un jeton d’accès est nécessaire.

> [!NOTE]
> Intergiciel (middleware) de IdentityServer autorisation doit être ajouté au pipeline de demande HTTP de l’application web avant d’ajouter MVC avec `app.UseMvc()` ou `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Rendre les demandes d’accès aux API

Lors de demandes microservices du panier d’achat et de classement, l’accès au jeton, obtenu à partir de IdentityServer pendant le processus d’authentification doit être inclus dans la demande, comme indiqué dans l’exemple de code suivant :

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Le jeton d’accès est stocké en tant que paramètre d’application, récupéré à partir de stockage spécifiques de la plate-forme et inclus dans l’appel à la `GetOrderAsync` méthode dans la `OrderService` classe.

De même, le jeton d’accès doit être inclus lors de l’envoi de données à un IdentityServer protégé API, comme indiqué dans l’exemple de code suivant :

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Le jeton d’accès est récupéré à partir de stockage spécifiques de la plateforme et inclus dans l’appel à la `UpdateBasketAsync` méthode dans la `BasketService` classe.

Le `RequestProvider` (classe), dans l’application mobile eShopOnContainers, utilise le `HttpClient` classe pour effectuer des demandes aux API RESTful exposé par l’application de référence eShopOnContainers. Lorsque des demandes de fabrication à l’ordre et le panier d’achat d’API qui requiert une autorisation, un jeton d’accès valide doit être inclus dans la demande. Cela est possible en ajoutant le jeton d’accès aux en-têtes de la `HttpClient` de l’instance, comme illustré dans l’exemple de code suivant :

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

Le `DefaultRequestHeaders` propriété de la `HttpClient` classe expose les en-têtes qui sont envoyés avec chaque demande, et le jeton d’accès est ajouté à la `Authorization` en-tête préfixé avec la chaîne `Bearer`. Lorsque la demande est envoyée à une API RESTful, la valeur de la `Authorization` en-tête est extraites et validé pour garantir qu’il a envoyé à partir d’un émetteur approuvé, et permet de déterminer si l’utilisateur est autorisé à appeler l’API qui reçoit.

Pour plus d’informations sur la façon dont l’application mobile eShopOnContainers effectue des requêtes web, consultez [l’accès à des données distantes](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Récapitulatif

Il existe plusieurs approches pour l’intégration de l’authentification et autorisation dans une application de Xamarin.Forms qui communique avec une application de web ASP.NET MVC. L’application mobile eShopOnContainers effectue l’authentification et autorisation avec un microservice d’identité en conteneur qui utilise IdentityServer 4. IdentityServer est une infrastructure open source qui OpenID Connect et OAuth 2.0 pour ASP.NET Core qui s’intègre avec ASP.NET Core Identity pour effectuer une authentification de jeton de support.

L’application mobile demande des jetons de sécurité IdentityServer, pour authentifier un utilisateur ou pour accéder à une ressource. Pour accéder à une ressource, un jeton d’accès doit être inclus dans la demande d’API qui requièrent une autorisation. Intergiciel (middleware) de IdentityServer valide les jetons d’accès entrant pour vous assurer qu’elles sont envoyées à partir d’un émetteur approuvé, et qu’ils sont valides pour être utilisé avec l’API qui les reçoit.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
