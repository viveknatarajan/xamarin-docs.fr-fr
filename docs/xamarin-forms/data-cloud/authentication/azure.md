---
title: L’authentification des utilisateurs avec les applications mobiles Azure
description: Cet article explique comment utiliser des applications mobiles Azure pour gérer le processus d’authentification dans une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: fc6206a22d7527ea38a39ab034c424bfe7730abb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241713"
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>L’authentification des utilisateurs avec les applications mobiles Azure

_Azure Mobile Apps utiliser divers modules fournisseurs d’identité externe pour prendre en charge l’authentification et l’autorisation des utilisateurs de l’application, notamment Facebook, Google, Microsoft, Twitter et Azure Active Directory. Autorisations peuvent être définies sur les tables pour restreindre l’accès uniquement aux utilisateurs authentifiés. Cet article explique comment utiliser des applications mobiles Azure pour gérer le processus d’authentification dans une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

Le processus de gérer le processus d’authentification dans une application Xamarin.Forms des applications mobiles Azure est la suivante :

1. Inscrire votre application Mobile de Azure chez d’un fournisseur identité et définissez les informations d’identification généré par le fournisseur dans l’applications mobiles back-end. Pour plus d’informations, consultez [inscrire votre application pour l’authentification et de configurer les Services d’application](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Définir un nouveau schéma d’URL pour votre application de Xamarin.Forms, qui permet au système d’authentification rediriger vers Xamarin.Forms application une fois le processus d’authentification est terminé. Pour plus d’informations, consultez [ajouter votre application à l’URL de redirection d’externe autorisée](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Restreindre l’accès pour le principal pour les applications mobiles Azure uniquement les clients authentifiés. Pour plus d’informations, consultez [restreindre les autorisations aux utilisateurs authentifiés](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Appel de l’authentification à partir de l’application de Xamarin.Forms. Pour plus d’informations, consultez [authentification ajouter à la bibliothèque de classes portable](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [Ajout de l’authentification à l’application iOS](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Ajout de l’authentification à l’application Android](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)et [ Ajouter l’authentification à des projets d’application Windows 10](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

Historiquement, des applications mobiles ont utilisé les vues web intégré pour effectuer une authentification auprès du fournisseur d’identité. Cela n’est plus recommandé pour les raisons suivantes :

- L’application qui héberge l’affichage web peut accéder aux informations d’identification de l’utilisateur l’authentification complète, pas seulement l’octroi d’autorisation qui a été conçue pour l’application. Cela viole le principe de privilège minimum, que l’application a accès aux informations d’identification plus puissantes qu’il requiert, éventuellement augmenter la surface d’attaque de l’application.
- L’application hôte peut capturer les noms d’utilisateur et mots de passe, automatiquement soumettez des formulaires et contourner le consentement de l’utilisateur et copier les cookies de session et les utiliser pour effectuer des actions authentifiées en tant que l’utilisateur.
- Affichages web incorporé ne partagent l’état de l’authentification avec les autres applications ou navigateur du périphérique, demandant à l’utilisateur pour se connecter pour chaque demande d’autorisation qui est considérée comme une expérience utilisateur inférieur.
- Certains points de terminaison d’autorisation prennent des mesures pour détecter et bloquer les demandes d’autorisation provenant de vues web.

L’alternative consiste à utiliser le navigateur web de l’appareil pour effectuer l’authentification, qui est l’approche adoptée par la dernière version de la [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). À l’aide du navigateur de l’appareil pour les demandes d’authentification permet d’améliorer la facilité d’utilisation d’une application, comme les utilisateurs doivent uniquement se connecter au fournisseur d’identité qu’une seule fois par périphérique, améliorer le taux de conversion de flux de connexion et d’autorisation de l’application. Le navigateur de l’appareil fournit également améliorer la sécurité que les applications peuvent inspecter et modifier du contenu dans un affichage web, mais pas le contenu affiché dans le navigateur.

## <a name="using-an-azure-mobile-apps-instance"></a>À l’aide d’une Instance des applications mobiles Azure

Le [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) fournit le `MobileServiceClient` (classe), qui est utilisé par une application de Xamarin.Forms pour accéder à l’instance Azure Mobile Apps.

L’exemple d’application utilise Google comme fournisseur d’identité, qui permet aux utilisateurs avec des comptes Google pour se connecter à l’application de Xamarin.Forms. Bien que Google est utilisé comme fournisseur d’identité dans cet article, l’approche s’applique également à autres fournisseurs d’identité.

<a name="logging-in" />

### <a name="logging-in-users"></a>Connexion des utilisateurs

L’écran de connexion dans l’exemple d’application est indiqué dans les captures d’écran suivants :

![](azure-images/login.png "Page de connexion")

Tandis que Google est utilisé comme fournisseur d’identité, une variété d’autres fournisseurs d’identité utilisable, y compris Azure Active Directory, Twitter, Facebook et Microsoft.

L’exemple de code suivant montre comment le processus de connexion est appelé :

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

Le `App.Authenticator` propriété est un `IAuthenticate` instance définie par chaque projet spécifique à la plateforme. Le `IAuthenticate` interface spécifie une `AuthenticateAsync` opération doit être fournie par chaque projet de plateforme. Par conséquent, appelez le `App.Authenticator.AuthenticateAsync` méthode s’exécute le `IAuthenticate.AuthenticateAsync` méthode dans un projet de plateforme.

Tous les de la plateforme `IAuthenticate.AuthenticateAsync` appel de méthodes le `MobileServiceClient.LoginAsync` méthode pour afficher une interface et le cache des données de connexion. Montre l’exemple de code suit le `LoginAsync` méthode pour la plateforme iOS :

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Le code suivant exemple illustre la `LoginAsync` méthode pour la plateforme Android :

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Montre l’exemple de code suit le `LoginAsync` méthode pour la plateforme Windows universelle :

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Sur toutes les plateformes, les `MobileServiceAuthenticationProvider` énumération est utilisée pour spécifier le fournisseur d’identité qui sera utilisé dans le processus d’authentification. Lorsque la `MobileServiceClient.LoginAsync` méthode est appelée, les applications mobiles Azure démarre un flux d’authentification en affichant la page de connexion du fournisseur sélectionné et en générant un jeton d’authentification après l’ouverture de session réussie avec le fournisseur d’identité. Le `MobileServiceClient.LoginAsync` méthode retourne un `MobileServiceUser` instance qui est stocké dans le `MobileServiceClient.CurrentUser` propriété. Cette propriété fournit `UserId` et `MobileServiceAuthenticationToken` propriétés. Il s’agit d’un jeton d’authentification pour l’utilisateur et de l’utilisateur authentifié. Le jeton d’authentification sera inclus dans toutes les demandes effectuées à l’instance d’applications mobiles Azure, permettant à l’application de Xamarin.Forms effectuer des actions sur l’instance de l’application Mobile Azure qui nécessitent des autorisations de l’utilisateur authentifié.

<a name="logging-out" />

### <a name="logging-out-users"></a>La déconnexion des utilisateurs

L’exemple de code suivant montre comment le processus de déconnexion est appelé :

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

Le `App.Authenticator` propriété est un `IAuthenticate` instance définie par chaque platformproject. Le `IAuthenticate` interface spécifie une `LogoutAsync` opération doit être fournie par chaque projet de plateforme. Par conséquent, appelez le `App.Authenticator.LogoutAsync` méthode s’exécute le `IAuthenticate.LogoutAsync` méthode dans un projet de plateforme.

Tous les de la plateforme `IAuthenticate.LogoutAsync` appel de méthodes le `MobileServiceClient.LogoutAsync` méthode pour annuler authentifier l’utilisateur connecté avec le fournisseur d’identité. Montre l’exemple de code suit le `LogoutAsync` méthode pour la plateforme iOS :

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Le code suivant exemple illustre la `LogoutAsync` méthode pour la plateforme Android :

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Montre l’exemple de code suit le `LogoutAsync` méthode pour la plateforme Windows universelle :

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Lorsque le `IAuthenticate.LogoutAsync` méthode est appelée, tous les cookies définis par le fournisseur d’identité sont effacés avant la `MobileServiceClient.LogoutAsync` méthode est appelée pour annuler authentifier l’utilisateur connecté avec le fournisseur d’identité.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser des applications mobiles Azure pour gérer le processus d’authentification dans une application de Xamarin.Forms. Azure Mobile Apps utiliser divers modules fournisseurs d’identité externe pour prendre en charge l’authentification et l’autorisation des utilisateurs de l’application, notamment Facebook, Google, Microsoft, Twitter et Azure Active Directory. La `MobileServiceClient` classe est utilisée par l’application de Xamarin.Forms pour contrôler l’accès à l’instance Azure Mobile Apps.


## <a name="related-links"></a>Liens associés

- [TodoAzureAuth (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Ajouter une authentification à votre application de Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Client Mobile Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
