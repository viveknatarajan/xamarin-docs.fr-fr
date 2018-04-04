---
title: L’accès à l’API Graph
description: À l’aide d’Active Directory pour interroger l’API Graph à l’aide de Xamarin
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 59e26c95c3f08467b837341043545ce0e1f2cbe5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="accessing-the-graph-api"></a>L’accès à l’API Graph

_À l’aide d’Active Directory pour interroger l’API Graph à l’aide de Xamarin_

Suivez ces étapes pour utiliser l’API Graph à partir d’une application Xamarin :

1. [L’inscription auprès d’Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) sur la *windowsazure.com* portal, puis
2. [Configurer les services](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Étape 3. Ajout de l’authentification Active Directory à une application

Dans votre application, ajoutez une référence à **bibliothèque Azure Active Directory Authentication (Azure ADAL)** à l’aide du Gestionnaire de Package NuGet dans Visual Studio ou Visual Studio pour Mac.
Veillez à sélectionner **afficher les packages de la version préliminaire** à inclure ce package, car il est toujours en version préliminaire.

> [!IMPORTANT]
> Remarque : Azure ADAL 3.0 est actuellement un aperçu et il peut y avoir des modifications avec rupture avant la publication de la version finale. 


![](graph-images/06.-adal-nuget-package.jpg "Ajoutez une référence à la bibliothèque Azure Active Directory Authentication (Azure ADAL)")

Dans votre application, vous devez maintenant ajouter les variables de niveau classe suivants sont requis pour le flux d’authentification.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Une chose à noter ici est `commonAuthority`. Lorsque le point de terminaison d’authentification est `common`, votre application devient **mutualisée**, ce qui signifie que n’importe quel utilisateur peut utiliser une connexion avec leurs informations d’identification Active Directory. Après l’authentification, cet utilisateur fonctionne sur le contexte de leur propre répertoire actif : autrement dit, ils verront les détails relatifs à son Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Write, méthode pour acquérir un jeton d’accès

Le code suivant (pour Android) démarre l’authentification et à l’achèvement assigner le résultat dans `authResult`. Les e/s et les implémentations de Windows Phone légèrement diffèrent : le deuxième paramètre (`Activity`) est différent sur iOS et d’absence sur Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

Dans le code ci-dessus, le `AuthenticationContext` est responsable de l’authentification avec commonAuthority. Il a un `AcquireTokenAsync` (méthode), qui prennent des paramètres en tant que ressource qui doit être accessible dans ce cas `graphResourceUri`, `clientId`, et `returnUri`. Revenez à l’application le `returnUri` lorsque l’authentification est terminée. Ce code est identique pour toutes les plateformes, toutefois, le dernier paramètre, `AuthorizationParameters`, seront différents sur différentes plateformes et est chargé de régissant le flux d’authentification.

Dans le cas d’Android ou iOS, nous passons `this` paramètre `AuthorizationParameters(this)` pour partager le contexte, alors que dans Windows, il est passé sans n’importe quel paramètre en tant que nouvelle `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Gérer la continuation pour Android

Une fois l’authentification terminée, le flux doit retourner à l’application. Dans le cas d’Android elle est gérée par le code suivant, qui doit être ajouté à **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Gérer la continuation pour Windows Phone

Pour Windows Phone modifier le `OnActivated` méthode dans le **App.xaml.cs** de fichiers avec le code ci-dessous :

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Maintenant si vous exécutez l’application, vous devez voir une boîte de dialogue d’authentification.
Après une authentification réussie, elle demande des autorisations pour accéder aux ressources (dans notre cas, l’API Graph) :

![](graph-images/08.-authentication-flow.jpg "Après une authentification réussie, il demande des autorisations pour accéder aux ressources dans notre cas, l’API Graph")

Si l’authentification réussit et que vous avez autorisé l’application à accéder aux ressources, vous devez obtenir un `AccessToken` et `RefreshToken` zone de liste déroulante dans `authResult`. Ces jetons sont requis pour les appels d’API supplémentaire et d’autorisation avec Azure Active Directory en arrière-plan.

![](graph-images/07.-access-token-for-authentication.jpg "Ces jetons sont requis pour les appels d’API supplémentaire et d’autorisation avec Azure Active Directory en arrière-plan")

Par exemple, le code ci-dessous permet de vous permet d’obtenir une liste des utilisateurs à partir d’Active Directory. Vous pouvez remplacer l’URL d’API Web avec votre API Web qui est protégé par Azure AD.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

