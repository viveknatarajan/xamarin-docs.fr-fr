---
title: "Intégration d’Azure Active Directory B2C avec les applications mobiles Azure"
description: "Azure B2C Active Directory est une solution de gestion d’identité cloud pour les applications pour les consommateurs web et mobiles. Cet article explique comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance Azure Mobile Apps avec Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 3a7d89d9b0f383d365b18364e5d902ee0642f395
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Intégration d’Azure Active Directory B2C avec les applications mobiles Azure

_Azure B2C Active Directory est une solution de gestion d’identité cloud pour les applications pour les consommateurs web et mobiles. Cet article explique comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance Azure Mobile Apps avec Xamarin.Forms._

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!NOTE]
> **Remarque**: le [bibliothèque d’authentification Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) est encore en version préliminaire, mais peut être utilisé dans un environnement de production. Toutefois, il peut être modifications avec rupture à l’API, le format du cache interne et autres mécanismes de la bibliothèque, ce qui peut avoir un impact sur votre application.

## <a name="overview"></a>Vue d'ensemble

Azure Mobile Apps permettent de développer des applications avec les serveurs principaux évolutifs est hébergés dans Azure App Service, avec prise en charge pour l’authentification mobile, synchronisation hors connexion et des notifications push. Pour plus d’informations sur les applications mobiles Azure, consultez [consommation d’une application Mobile de Azure](~/xamarin-forms/data-cloud/consuming/azure.md), et [authentification des utilisateurs avec les applications mobiles Azure](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure B2C Active Directory est un service de gestion d’identité pour les applications consommateur, qui permet aux utilisateurs de se connecter à votre application par :

- À l’aide de leurs comptes sociaux existants (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Création de nouvelles informations d’identification (adresse de messagerie et un mot de passe ou nom d’utilisateur et mot de passe). Ces informations d’identification sont appelées *local* comptes.

Pour plus d’informations sur Azure Active Directory B2C, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure B2C Active Directory peut servir à gérer le flux de travail d’authentification pour une application Mobile de Azure. Avec cette approche, l’expérience de gestion d’identité est entièrement définie dans le cloud et peut être modifiée sans modifier votre code d’application mobile.

Il existe deux flux de travail d’authentification qui peut être adoptées lors de l’intégration d’un locataire Azure Active Directory B2C avec une instance Azure Mobile Apps :

- [Client géré](#client_managed) : dans cette approche de l’application mobile de Xamarin.Forms initialise le processus d’authentification avec le locataire Azure Active Directory B2C et transmet le jeton d’authentification reçu à l’instance Azure Mobile Apps.
- [Géré par le serveur](#server_managed) – dans cette approche, les applications mobiles Azure instance utilise le locataire Azure Active Directory B2C pour lancer le processus d’authentification via un workflow basé sur le web.

Dans les deux cas, l’expérience d’authentification est fournie par le locataire Azure Active Directory B2C. Dans l’exemple d’application, il en résulte dans l’écran de connexion indiqué dans les captures d’écran suivants :

![](azure-ad-b2c-mobile-app-images/screenshots.png "Page de connexion")

Connectez-vous avec les fournisseurs d’identité sociaux, ou avec un compte local, sont autorisés. Alors que Facebook, Google et Microsoft sont utilisés comme fournisseurs d’identité sociaux dans cet exemple, autres fournisseurs d’identité peuvent également servir.

## <a name="setup"></a>Installation

Quel que soit le flux de travail d’authentification utilisé, le processus initial pour l’intégration d’un locataire Azure Active Directory B2C avec une instance Azure Mobile Apps est comme suit :

1. Créer une instance Azure Mobile Apps. Pour plus d’informations, consultez [consommation d’une application Mobile de Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Activer l’authentification dans l’instance Azure Mobile Apps et l’application de Xamarin.Forms. Pour plus d’informations, consultez [authentification des utilisateurs avec les applications mobiles Azure](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Créer un locataire Azure Active Directory B2C. Pour plus d’informations, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Notez que la bibliothèque d’authentification Microsoft (MSAL) est requis lors de l’utilisation d’un flux de travail de l’authentification de client géré. MSAL utilise le navigateur web de l’appareil pour effectuer l’authentification. Cela permet d’améliorer la facilité d’utilisation d’une application, comme les utilisateurs doivent uniquement se connecter une fois par périphérique, améliorer le taux de conversion de l’authentification et l’autorisation de flux dans l’application. Le navigateur d’appareil offre également une sécurité accrue. Une fois que l’utilisateur termine le processus d’authentification, contrôle retourne à l’application à partir de l’onglet de navigateur web. Cela est possible en inscrivant un modèle d’URL personnalisé pour l’URL de redirection qui est retourné par le processus d’authentification, détection et gestion de l’URL personnalisée une fois qu’elle est envoyée. Pour plus d’informations sur l’utilisation de MSAL pour communiquer avec un locataire Azure Active Directory B2C, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Authentification de client géré

Dans l’authentification de client géré, une application mobile Xamarin.Forms contacte un locataire Azure Active Directory B2C pour lancer un flux d’authentification. Après avoir réussi l’authentification sur le B2C d’Active Directory de Azure client retourne un jeton d’identité qui est alors fourni pendant la connexion à l’instance Azure Mobile Apps. Cela permet à l’application de Xamarin.Forms effectuer des actions sur l’instance Azure Mobile Apps qui nécessite des autorisations de l’utilisateur authentifié.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Configuration du client Azure Active Directory B2C

Pour un flux de travail géré par le client de l’authentification, le locataire Azure Active Directory B2C doit être configuré comme suit :

- Inclure un client natif.
- Définir l’URI de redirection personnalisée à un modèle d’URL qui identifie de façon unique l’application mobile, suivie par `://auth/`. Pour plus d’informations sur le choix d’un modèle d’URL personnalisé, consultez [en choisissant un URI de redirection application native](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

La capture d’écran suivante illustre cette configuration :

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C Configuration")](azure-ad-b2c-mobile-app-images/client-flow-config.png "Azure Active Directory B2C Configuration")

La stratégie utilisée dans l’Azure Active Directory B2C client doit également être configuré afin que l’URL de réponse est défini sur le même schéma d’URL personnalisé, suivi par `://auth/`. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Stratégies d’Azure Active Directory B2C")

### <a name="azure-mobile-app-configuration"></a>Configuration des applications mobiles Azure

Pour un flux de travail de l’authentification de client géré, l’instance Azure Mobile Apps doit être configuré comme suit :

- L’authentification du Service de l’application doit être activée.
- L’action à entreprendre lorsqu’une demande n’est pas authentifiée doit être définie sur **se connecter avec Azure Active Directory**.

La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Configuration des applications mobiles Azure")

L’instance Azure Mobile Apps doit également être configuré pour communiquer avec le locataire Azure Active Directory B2C. Cela peut être accompli en activant **avancé** mode pour le fournisseur d’authentification Azure Active Directory, avec la **ID Client** étant le **ID d’Application** de Azure Active Directory B2C locataire et le **Url de l’émetteur** qui est le point de terminaison de métadonnées pour la stratégie d’Azure Active Directory B2C. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Applications mobiles Azure Configuration avancée")

### <a name="signing-in"></a>L’ouverture de session

L’exemple de code suivant montre comment initier un flux de travail de l’authentification de client géré :

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

La bibliothèque d’authentification Microsoft (MSAL) permet de lancer un flux de travail de l’authentification auprès du client Azure Active Directory B2C. Le `AcquireTokenAsync` méthode lance le navigateur web de l’appareil et affiche les options d’authentification définies dans la stratégie d’Azure Active Directory B2C qui est spécifiée par la stratégie référencée via la `Constants.Authority` constante. Cette stratégie définit les expériences consommateurs ira pendant l’inscription et de connexion et les revendications de que l’application recevra après inscription ou de la connexion.

Le résultat de la `AcquireTokenAsync` appel de méthode est un `AuthenticationResult` instance. Si l’authentification réussit, le `AuthenticationResult` instance contiendra un jeton d’identité, qui est mises en cache localement. Si l’authentification échoue, le `AuthenticationResult` instance contiendra les données qui indique la raison pour laquelle l’authentification a échoué. Pour plus d’informations sur l’utilisation de MSAL pour communiquer avec un locataire Azure Active Directory B2C, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Lorsque le `MobileServiceClient.LoginAsync` méthode est appelée, l’instance Azure Mobile Apps reçoit le jeton d’identité encapsulé dans un `JObject`. La présence d’un moyen de jeton valide qui n’est pas nécessaire que l’instance Azure Mobile Apps initier son propre flux d’authentification OAuth 2.0. Au lieu de cela, le `MobileServiceClient.LoginAsync` méthode retourne un `MobileServiceUser` instance qui est stocké dans le `MobileServiceClient.CurrentUser` propriété. Cette propriété fournit `UserId` et `MobileServiceAuthenticationToken` propriétés. Elles représentent l’utilisateur authentifié et un jeton d’authentification pour l’utilisateur, ce qui peut être utilisé jusqu'à son expiration. Le jeton d’authentification sera inclus dans toutes les demandes effectuées à l’instance d’applications mobiles Azure, permettant à l’application de Xamarin.Forms effectuer des actions sur l’instance d’applications mobiles Azure qui nécessitent des autorisations de l’utilisateur authentifié.

### <a name="signing-out"></a>Déconnecter

L’exemple de code suivant montre comment le processus de déconnexion gérés par le client est appelé :

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Le `MobileServiceClient.LogoutAsync` méthode authentifie annule l’utilisateur avec l’instance Azure Mobile Apps, puis tous les jetons d’authentification sont effacés du cache local créé par MSAL.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Authentification de serveur géré

Dans l’authentification de serveur géré, une application de Xamarin.Forms contacte une instance Azure Mobile Apps, qui utilise le locataire Azure Active Directory B2C pour gérer le flux d’authentification OAuth 2.0 en affichant une page de connexion, comme défini dans la stratégie B2C. Après authentification réussie, l’instance Azure Mobile Apps retourne un jeton qui permet à l’application de Xamarin.Forms effectuer des actions sur l’instance d’applications mobiles Azure qui nécessitent des autorisations de l’utilisateur authentifié.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Configuration du client Azure Active Directory B2C

Pour un flux de travail géré par le serveur de l’authentification, le locataire Azure Active Directory B2C doit être configuré comme suit :

- Inclure un site web application API et permettre le flux implicit.
- Définir l’URL de réponse à l’adresse de l’application Mobile Azure, suivi par `/.auth/login/aad/callback`.

La capture d’écran suivante illustre cette configuration :

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C Configuration")](azure-ad-b2c-mobile-app-images/server-flow-config.png "Azure Active Directory B2C Configuration")

La stratégie utilisée dans l’Azure Active Directory B2C client doit également être configuré afin que l’URL de réponse est défini à l’adresse de l’application Mobile Azure, suivi par `/.auth/login/aad/callback`. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Stratégies d’Azure Active Directory B2C")

### <a name="azure-mobile-apps-instance-configuration"></a>Configuration de l’Instance des applications mobiles Azure

Pour un flux de travail géré par le serveur de l’authentification, l’instance Azure Mobile Apps doit être configuré comme suit :

- L’authentification du Service de l’application doit être activée.
- L’action à entreprendre lorsqu’une demande n’est pas authentifiée doit être définie sur **se connecter avec Azure Active Directory**.

La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Configuration des applications mobiles Azure")

L’instance Azure Mobile Apps doit également être configuré pour communiquer avec le locataire Azure Active Directory B2C. Cela peut être accompli en activant **avancé** mode pour le fournisseur d’authentification Azure Active Directory, avec la **ID Client** étant le **ID d’Application** de Azure Active Directory B2C locataire et le **Url de l’émetteur** qui est le point de terminaison de métadonnées pour la stratégie d’Azure Active Directory B2C. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Applications mobiles Azure Configuration avancée")

### <a name="signing-in"></a>L’ouverture de session

L’exemple de code suivant montre comment initier un flux de travail géré par le serveur de l’authentification :

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

Lorsque la `MobileServiceClient.LoginAsync` méthode est appelée, l’instance Azure Mobile Apps exécute la stratégie d’Azure Active Directory B2C liée, ce qui lance le flux d’authentification OAuth 2.0. Notez que chaque `AuthenticateAsync` méthode est spécifique à la plateforme. Toutefois, chaque `AuthenticateAsync` utilise le `MobileServiceClient.LoginAsync` (méthode) et spécifie qu’un client Azure Active Directory sera utilisé dans le processus d’authentification. Pour plus d’informations, consultez [d’utilisateurs](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

Le `MobileServiceClient.LoginAsync` méthode retourne un `MobileServiceUser` instance qui est stocké dans le `MobileServiceClient.CurrentUser` propriété. Cette propriété fournit `UserId` et `MobileServiceAuthenticationToken` propriétés. Elles représentent l’utilisateur authentifié et un jeton d’authentification pour l’utilisateur, ce qui peut être utilisé jusqu'à son expiration. Le jeton d’authentification sera inclus dans toutes les demandes effectuées à l’instance d’applications mobiles Azure, permettant à l’application de Xamarin.Forms effectuer des actions sur l’instance d’applications mobiles Azure qui nécessitent des autorisations de l’utilisateur authentifié.

### <a name="signing-out"></a>Déconnecter

L’exemple de code suivant montre comment le processus de déconnexion géré par le serveur est appelé :

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

Le `MobileServiceClient.LogoutAsync` méthode authentifie annule l’utilisateur avec l’instance Azure Mobile Apps. Pour plus d’informations, consultez [journalisation des utilisateurs](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance Azure Mobile Apps avec Xamarin.Forms. Azure B2C Active Directory est une solution de gestion d’identité cloud pour les applications pour les consommateurs web et mobiles.


## <a name="related-links"></a>Liens associés

- [TodoAzureAuth ServerFlow (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [L’authentification des utilisateurs avec les applications mobiles Azure](~/xamarin-forms/data-cloud/authentication/azure.md)
- [L’authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Bibliothèque d’authentification de Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)
