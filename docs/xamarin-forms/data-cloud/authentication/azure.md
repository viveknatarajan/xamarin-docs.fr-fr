---
title: Authentification des utilisateurs avec Azure Mobile Apps
description: Cet article explique comment utiliser Azure Mobile Apps pour gérer le processus d’authentification dans une application Xamarin.Forms.
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 428e536d6895ff16a928f8cc40a8a7976d087471
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53060489"
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Authentification des utilisateurs avec Azure Mobile Apps

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)

_Azure Mobile Apps utiliser divers fournisseurs d’identité externes pour prendre en charge l’authentification et l’autorisation des utilisateurs de l’application, y compris Facebook, Google, Microsoft, Twitter et Azure Active Directory. Autorisations peuvent être définies sur les tables pour restreindre l’accès aux utilisateurs authentifiés uniquement. Cet article explique comment utiliser Azure Mobile Apps pour gérer le processus d’authentification dans une application Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

Le processus de la présence d’Azure Mobile Apps gère le processus d’authentification dans une application Xamarin.Forms est comme suit :

1. Inscrire votre application Azure Mobile App au site d’un fournisseur d’identité et définissez les informations d’identification généré par le fournisseur dans le serveur principal Mobile Apps. Pour plus d’informations, consultez [inscrire votre application pour l’authentification et configurer les Services d’application](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Définir un nouveau schéma d’URL pour votre application Xamarin.Forms, ce qui permet au système d’authentification rediriger vers Xamarin.Forms application une fois que le processus d’authentification est terminé. Pour plus d’informations, consultez [ajouter votre application à l’URL de redirection d’externes autorisés](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Restreindre l’accès au serveur principal d’Azure Mobile Apps pour uniquement les clients authentifiés. Pour plus d’informations, consultez [restreindre les autorisations aux utilisateurs authentifiés](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Appeler l’authentification à partir de l’application Xamarin.Forms. Pour plus d’informations, consultez [ajouter l’authentification à la bibliothèque de classes portable](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [ajouter l’authentification à l’application iOS](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [ajouter l’authentification à l’application Android](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)et [ Ajout de l’authentification pour les projets d’application Windows 10](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> Dans iOS 9 et supérieur, App Transport Security (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et l’application, ce qui empêche la divulgation accidentelle d’informations sensibles. Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9, toutes les connexions seront soumis à des exigences de sécurité ATS. Si les connexions ne répondent pas à ces exigences, ils échoueront avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser la communication de ressources internet. Cela est possible en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [App Transport Security](~/ios/app-fundamentals/ats.md).

Historiquement, les applications mobiles ont utilisé les vues web incorporées pour effectuer l’authentification auprès du fournisseur d’identité. Cela n’est plus recommandé pour les raisons suivantes :

- L’application qui héberge l’affichage web peut accéder aux informations d’identification de l’utilisateur une authentification complète, pas seulement l’octroi d’autorisation qui a été conçu pour l’application. Cela viole le principe du moindre privilège, que l’application ait un accès aux informations d’identification plus puissants qu’il requiert, augmenter potentiellement la surface d’attaque de l’application.
- L’application hôte peut capturer les noms d’utilisateur et mots de passe, automatiquement soumettez des formulaires et contourner le consentement de l’utilisateur et copiez les cookies de session et les utiliser pour effectuer des actions authentifiées en tant que l’utilisateur.
- Les vues web incorporées ne partagent pas l’état de l’authentification avec les autres applications ou navigateur web de l’appareil, lui demandant de connexion à chaque demande d’autorisation qui est considérée comme une expérience utilisateur de moindre qualité.
- Certains points de terminaison d’autorisation prennent des mesures pour détecter et bloquer les demandes d’autorisation qui proviennent des vues web.

L’alternative consiste à utiliser le navigateur web de l’appareil pour effectuer l’authentification, qui est l’approche adoptée par la dernière version de la [kit SDK Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). À l’aide du navigateur de l’appareil pour les demandes d’authentification d’améliore la facilité d’utilisation d’une application, comme les utilisateurs doivent uniquement se connecter au fournisseur d’identité une fois par appareil, améliorer le taux de conversion de flux de connexion et l’autorisation de l’application. Navigateur de l’appareil fournit également une sécurité améliorée comme les applications sont en mesure d’inspecter et modifier du contenu dans un affichage web, mais pas le contenu affiché dans le navigateur.

## <a name="using-an-azure-mobile-apps-instance"></a>À l’aide d’une Instance d’Azure Mobile Apps

Le [kit SDK Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) fournit le `MobileServiceClient` (classe), qui est utilisé par une application Xamarin.Forms pour accéder à l’instance d’Azure Mobile Apps.

L’exemple d’application utilise Google comme fournisseur d’identité, qui permet aux utilisateurs avec des comptes Google pour vous connecter à l’application Xamarin.Forms. Tandis que Google est utilisé comme fournisseur d’identité dans cet article, l’approche s’applique aux autres fournisseurs d’identité.

<a name="logging-in" />

### <a name="logging-in-users"></a>Connexion des utilisateurs

L’écran de connexion dans l’exemple d’application est indiqué dans les captures d’écran suivante :

![](azure-images/login.png "Page de connexion")

Tandis que Google est utilisé comme fournisseur d’identité, une variété d’autres fournisseurs d’identité utilisable, y compris Facebook, Microsoft, Twitter et Azure Active Directory.

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

Le `App.Authenticator` propriété est un `IAuthenticate` instance qui est définie par chaque projet spécifique à la plateforme. Le `IAuthenticate` interface spécifie un `AuthenticateAsync` opération doit être fournie par chaque projet de plateforme. Par conséquent, appeler le `App.Authenticator.AuthenticateAsync` méthode s’exécute le `IAuthenticate.AuthenticateAsync` méthode dans un projet de plateforme.

Tous de la plateforme `IAuthenticate.AuthenticateAsync` appel de méthodes le `MobileServiceClient.LoginAsync` méthode pour afficher une interface et le cache des données de connexion. Le code suivant montre l’exemple le `LoginAsync` méthode pour la plateforme iOS :

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

Le code suivant montre l’exemple le `LoginAsync` méthode pour la plateforme Android :

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

Le code suivant montre l’exemple le `LoginAsync` méthode pour la plateforme Windows universelle :

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

Sur toutes les plateformes, les `MobileServiceAuthenticationProvider` énumération est utilisée pour spécifier le fournisseur d’identité qui sera utilisé dans le processus d’authentification. Lorsque le `MobileServiceClient.LoginAsync` méthode est appelée, Azure Mobile Apps lance un flux d’authentification en affichant la page de connexion du fournisseur sélectionné et en générant un jeton d’authentification après avoir établi une connexion avec le fournisseur d’identité. Le `MobileServiceClient.LoginAsync` méthode retourne un `MobileServiceUser` instance est stockée dans le `MobileServiceClient.CurrentUser` propriété. Cette propriété fournit `UserId` et `MobileServiceAuthenticationToken` propriétés. Ces éléments représentent l’utilisateur authentifié et un jeton d’authentification pour l’utilisateur. Le jeton d’authentification sera inclus dans toutes les demandes effectuées à l’instance d’Azure Mobile Apps, permettant à l’application Xamarin.Forms effectuer des actions sur l’instance d’application Mobile Azure qui nécessitent des autorisations de l’utilisateur authentifié.

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

Le `App.Authenticator` propriété est un `IAuthenticate` instance qui est définie par chaque platformproject. Le `IAuthenticate` interface spécifie un `LogoutAsync` opération doit être fournie par chaque projet de plateforme. Par conséquent, appeler le `App.Authenticator.LogoutAsync` méthode s’exécute le `IAuthenticate.LogoutAsync` méthode dans un projet de plateforme.

Tous de la plateforme `IAuthenticate.LogoutAsync` appel de méthodes le `MobileServiceClient.LogoutAsync` méthode d’annulation authentifier l’utilisateur connecté avec le fournisseur d’identité. Le code suivant montre l’exemple le `LogoutAsync` méthode pour la plateforme iOS :

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

Le code suivant montre l’exemple le `LogoutAsync` méthode pour la plateforme Android :

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Le code suivant montre l’exemple le `LogoutAsync` méthode pour la plateforme Windows universelle :

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Lorsque le `IAuthenticate.LogoutAsync` méthode est appelée, les cookies définis par le fournisseur d’identité sont effacés avant la `MobileServiceClient.LogoutAsync` méthode est appelée pour annuler authentifier l’utilisateur connecté avec le fournisseur d’identité.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser Azure Mobile Apps pour gérer le processus d’authentification dans une application Xamarin.Forms. Azure Mobile Apps utiliser divers fournisseurs d’identité externes pour prendre en charge l’authentification et l’autorisation des utilisateurs de l’application, y compris Facebook, Google, Microsoft, Twitter et Azure Active Directory. Le `MobileServiceClient` classe est utilisée par l’application Xamarin.Forms pour contrôler l’accès à l’instance d’Azure Mobile Apps.


## <a name="related-links"></a>Liens associés

- [TodoAzureAuth (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Ajout de l’authentification à votre application Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Client Mobile Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
