---
title: L’intégration d’Azure Active Directory B2C dans Azure Mobile Apps
description: Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs. Cet article montre comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance d’Azure Mobile Apps avec Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815675"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>L’intégration d’Azure Active Directory B2C dans Azure Mobile Apps

_Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs. Cet article montre comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance d’Azure Mobile Apps avec Xamarin.Forms._

![](~/media/shared/preview.png "Cette API est actuellement en version préliminaire")

> [!NOTE]
> Le [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) est toujours en version préliminaire, mais convient pour une utilisation dans un environnement de production. Toutefois, il peut être modifications avec rupture à l’API, format de cache interne et d’autres mécanismes de la bibliothèque, ce qui peut avoir un impact sur votre application.

## <a name="overview"></a>Vue d'ensemble

Azure Mobile Apps vous permet de vous permet de développer des applications avec les serveurs principaux évolutives hébergées dans Azure App Service, avec prise en charge pour l’authentification d’appareils mobiles, de synchronisation hors connexion et de notifications push. Pour plus d’informations sur Azure Mobile Apps, consultez [consommer une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md), et [authentification des utilisateurs avec Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure B2C Active de répertoire est un service de gestion d’identité pour les applications destinées aux consommateurs, qui permet aux consommateurs pour se connecter à votre application par :

- À l’aide de leurs comptes sociaux existants (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Création de nouvelles informations d’identification (adresse de messagerie et mot de passe, ou nom d’utilisateur et mot de passe). Ces informations d’identification sont appelées *local* comptes.

Pour plus d’informations sur Azure Active Directory B2C, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure B2C Active de répertoire peut servir à gérer le flux de travail de l’authentification pour une application Mobile Azure. Avec cette approche, l’expérience de gestion d’identité est entièrement défini dans le cloud et peut être modifié sans modifier votre code d’application mobile.

Il existe deux workflows d’authentification qui peuvent être adoptés lors de l’intégration d’un locataire Azure Active Directory B2C avec une instance d’Azure Mobile Apps :

- [Client géré](#client_managed) : dans cette approche l’initiée par l’application mobile Xamarin.Forms le processus d’authentification avec le locataire Azure Active Directory B2C et transmet le jeton d’authentification reçu à l’instance d’Azure Mobile Apps.
- [Géré par le serveur](#server_managed) – dans cette approche l’application Azure Mobile Apps instance utilise le locataire Azure Active Directory B2C pour lancer le processus d’authentification via un workflow basé sur le web.

Dans les deux cas, l’expérience d’authentification est fournie par le locataire Azure Active Directory B2C. Dans l’exemple d’application, il en résulte dans l’écran de connexion indiqué dans les captures d’écran suivante :

![](azure-ad-b2c-mobile-app-images/screenshots.png "Page de connexion")

Connectez-vous avec les fournisseurs d’identité sociale, ou avec un compte local, sont autorisés. Tandis que Microsoft, Google et Facebook sont utilisés comme fournisseurs d’identité sociale dans cet exemple, autres fournisseurs d’identité peuvent également être utilisés.

## <a name="setup"></a>Installation

Quel que soit le workflow d’authentification utilisé, le processus initial pour l’intégration d’un locataire Azure Active Directory B2C avec une instance d’Azure Mobile Apps est la suivante :

1. Créez une instance d’Azure Mobile Apps. Pour plus d’informations, consultez [consommer une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Activer l’authentification dans l’instance d’Azure Mobile Apps et de l’application Xamarin.Forms. Pour plus d’informations, consultez [authentification des utilisateurs avec Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Créer un locataire Azure Active Directory B2C. Pour plus d’informations, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Notez que la bibliothèque d’authentification Microsoft (MSAL) est requis lors de l’utilisation d’un flux de travail de l’authentification de client géré. MSAL utilise le navigateur web de l’appareil pour effectuer l’authentification. Cela améliore la facilité d’utilisation d’une application, comme les utilisateurs doivent uniquement se connecter une fois par appareil, améliorer le taux de conversion de l’authentification et l’autorisation de flux dans l’application. Navigateur de l’appareil fournit également une sécurité améliorée. Une fois que l’utilisateur a terminé le processus d’authentification, contrôle retourne à l’application à partir de l’onglet du navigateur web. Pour cela, vous devez l’inscription d’un schéma d’URL personnalisé pour l’URL de redirection qui est retourné par le processus d’authentification, puis détecter et gérer l’URL personnalisée une fois qu’elle est envoyée. Pour plus d’informations sur l’utilisation de MSAL pour communiquer avec un locataire Azure Active Directory B2C, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Authentification de client géré

Dans l’authentification de client géré, une application mobile Xamarin.Forms contacte un locataire Azure Active Directory B2C pour lancer un flux d’authentification. Après authentification réussie l’Active Directory B2C du Azure client renvoie un jeton d’identité qui est ensuite fourni pendant la connexion à l’instance d’Azure Mobile Apps. Cela permet à l’application Xamarin.Forms effectuer des actions sur l’instance d’Azure Mobile Apps qui nécessite des autorisations de l’utilisateur authentifié.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Configuration du client Azure Active Directory B2C

Pour un flux de travail géré par le client de l’authentification, le locataire Azure Active Directory B2C doit être configuré comme suit :

- Inclure un client natif.
- Définir l’URI de redirection personnalisé à un schéma d’URL qui identifie de façon unique l’application mobile suivie `://auth/`. Pour plus d’informations sur le choix d’un schéma d’URL personnalisé, consultez [en choisissant un URI de redirection application native](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

La capture d’écran suivante illustre cette configuration :

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C Configuration")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C Configuration")

La stratégie utilisée dans l’Azure Active Directory B2C client doit également être configuré pour que l’URL de réponse est définie sur le même schéma d’URL personnalisé, suivi par `://auth/`. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Stratégies d’Azure Active Directory B2C")

### <a name="azure-mobile-app-configuration"></a>Configuration des applications mobiles Azure

Pour un flux de travail géré par le client de l’authentification, l’instance d’Azure Mobile Apps doit être configuré comme suit :

- L’authentification App Service doit être activée.
- L’action à entreprendre lorsqu’une demande n’est pas authentifiée doit être définie sur **se connecter avec Azure Active Directory**.

La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Configuration d’Azure Mobile Apps")

L’instance d’Azure Mobile Apps doit également être configuré pour communiquer avec le locataire Azure Active Directory B2C. Cela est possible en activant **avancé** mode pour le fournisseur d’authentification Azure Active Directory, avec la **ID Client** en cours la **ID d’Application** de Azure Active Directory B2C locataire et le **Url de l’émetteur** qui est le point de terminaison de métadonnées pour la stratégie d’Azure Active Directory B2C. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure Mobile Apps Configuration avancée")

### <a name="signing-in"></a>La connexion

L’exemple de code suivant montre comment initialiser un workflow d’authentification client géré :

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

La bibliothèque d’authentification Microsoft (MSAL) est utilisé pour lancer un workflow d’authentification avec le locataire Azure Active Directory B2C. Le `AcquireTokenAsync` méthode lance le navigateur web de l’appareil et affiche les options d’authentification définies dans la stratégie d’Azure Active Directory B2C est spécifiée par la stratégie référencée par le biais du `Constants.Authority` constante. Cette stratégie définit les expériences clients seront pendant l’inscription et la connexion et les revendications de que l’application recevra après son inscription ou de connexion.

Le résultat de la `AcquireTokenAsync` appel de méthode est un `AuthenticationResult` instance. Si l’authentification réussit, le `AuthenticationResult` instance contiendra un jeton d’identité, qui sera mis en cache localement. Si l’authentification échoue, le `AuthenticationResult` instance contiendra les données qui indique pourquoi l’authentification a échoué. Pour plus d’informations sur comment utiliser MSAL pour communiquer avec un locataire Azure Active Directory B2C, consultez [authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Lorsque le `MobileServiceClient.LoginAsync` méthode est appelée, l’instance d’Azure Mobile Apps reçoit le jeton d’identité encapsulé dans un `JObject`. La présence d’un moyen de jeton valide, l’instance d’Azure Mobile Apps n’a pas besoin pour lancer son propre flux d’authentification OAuth 2.0. Au lieu de cela, le `MobileServiceClient.LoginAsync` méthode retourne un `MobileServiceUser` instance est stockée dans le `MobileServiceClient.CurrentUser` propriété. Cette propriété fournit `UserId` et `MobileServiceAuthenticationToken` propriétés. Ces éléments représentent l’utilisateur authentifié et un jeton d’authentification pour l’utilisateur, ce qui peut être utilisé jusqu'à son expiration. Le jeton d’authentification sera inclus dans toutes les demandes effectuées à l’instance d’Azure Mobile Apps, permettant à l’application Xamarin.Forms effectuer des actions sur l’instance d’Azure Mobile Apps qui nécessitent des autorisations de l’utilisateur authentifié.

### <a name="signing-out"></a>Déconnexion

L’exemple de code suivant montre comment le processus de déconnexion géré par le client est appelé :

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

Le `MobileServiceClient.LogoutAsync` méthode authentifie l’utilisateur avec l’instance d’Azure Mobile Apps à retirer, et ensuite tous les jetons d’authentification sont effacés à partir du cache local créé par MSAL.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Authentification de serveur géré

Dans l’authentification de serveur géré, une application Xamarin.Forms contacte une instance d’Azure Mobile Apps, qui utilise le locataire Azure Active Directory B2C pour gérer le flux d’authentification OAuth 2.0 en affichant une page de connexion, tel que défini dans la stratégie B2C. Après authentification réussie, l’instance d’Azure Mobile Apps retourne un jeton qui permet à l’application Xamarin.Forms effectuer des actions sur l’instance d’Azure Mobile Apps qui nécessitent des autorisations de l’utilisateur authentifié.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Configuration du client Azure Active Directory B2C

Pour un flux de travail géré par le serveur de l’authentification, le locataire Azure Active Directory B2C doit être configuré comme suit :

- Incluent une API web app/web et autoriser le flux implicite.
- Définir l’URL de réponse à l’adresse de l’application Mobile Azure, suivie de `/.auth/login/aad/callback`.

La capture d’écran suivante illustre cette configuration :

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C Configuration")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C Configuration")

La stratégie utilisée dans l’Azure Active Directory B2C client doit également être configuré pour que l’URL de réponse est définie sur l’adresse de l’application Mobile Azure, suivie `/.auth/login/aad/callback`. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Stratégies d’Azure Active Directory B2C")

### <a name="azure-mobile-apps-instance-configuration"></a>Configuration de l’Instance Azure Mobile Apps

Pour un flux de travail géré par le serveur de l’authentification, l’instance d’Azure Mobile Apps doit être configuré comme suit :

- L’authentification App Service doit être activée.
- L’action à entreprendre lorsqu’une demande n’est pas authentifiée doit être définie sur **se connecter avec Azure Active Directory**.

La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Configuration d’Azure Mobile Apps")

L’instance d’Azure Mobile Apps doit également être configuré pour communiquer avec le locataire Azure Active Directory B2C. Cela est possible en activant **avancé** mode pour le fournisseur d’authentification Azure Active Directory, avec la **ID Client** en cours la **ID d’Application** de Azure Active Directory B2C locataire et le **Url de l’émetteur** qui est le point de terminaison de métadonnées pour la stratégie d’Azure Active Directory B2C. La capture d’écran suivante illustre cette configuration :

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure Mobile Apps Configuration avancée")

### <a name="signing-in"></a>La connexion

L’exemple de code suivant montre comment initialiser un workflow d’authentification géré par le serveur :

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

Lorsque le `MobileServiceClient.LoginAsync` méthode est appelée, l’instance d’Azure Mobile Apps exécute la stratégie Azure Active Directory B2C liée, ce qui lance le flux d’authentification OAuth 2.0. Notez que chaque `AuthenticateAsync` méthode est spécifique à la plateforme. Toutefois, chaque `AuthenticateAsync` méthode utilise le `MobileServiceClient.LoginAsync` (méthode) et spécifie qu’un client Azure Active Directory sera utilisé dans le processus d’authentification. Pour plus d’informations, consultez [connexion des utilisateurs](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

Le `MobileServiceClient.LoginAsync` méthode retourne un `MobileServiceUser` instance est stockée dans le `MobileServiceClient.CurrentUser` propriété. Cette propriété fournit `UserId` et `MobileServiceAuthenticationToken` propriétés. Ces éléments représentent l’utilisateur authentifié et un jeton d’authentification pour l’utilisateur, ce qui peut être utilisé jusqu'à son expiration. Le jeton d’authentification sera inclus dans toutes les demandes effectuées à l’instance d’Azure Mobile Apps, permettant à l’application Xamarin.Forms effectuer des actions sur l’instance d’Azure Mobile Apps qui nécessitent des autorisations de l’utilisateur authentifié.

### <a name="signing-out"></a>Déconnexion

L’exemple de code suivant montre comment le processus de déconnexion géré par le serveur est appelé :

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

Le `MobileServiceClient.LogoutAsync` méthode authentifie l’utilisateur avec l’instance d’Azure Mobile Apps à retirer. Pour plus d’informations, consultez [journalisation Out utilisateurs](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance d’Azure Mobile Apps avec Xamarin.Forms. Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs.


## <a name="related-links"></a>Liens associés

- [TodoAzureAuth ServerFlow (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentification des utilisateurs avec Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Authentification des utilisateurs avec Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Bibliothèque d’authentification Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)
