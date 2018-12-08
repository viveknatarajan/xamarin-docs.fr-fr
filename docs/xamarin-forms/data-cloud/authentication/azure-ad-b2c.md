---
title: Authentification des utilisateurs avec Azure Active Directory B2C
description: Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs. Cet article montre comment utiliser la bibliothèque d’authentification de Microsoft et Azure Active Directory B2C pour intégrer la gestion des identités de consommateur dans une application mobile.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 7c12136a0dad0165c46f1559e7a2d61abaf7af1e
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059702"
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Authentification des utilisateurs avec Azure Active Directory B2C

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)

_Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs. Cet article montre comment utiliser la bibliothèque d’authentification de Microsoft et Azure Active Directory B2C pour intégrer la gestion des identités de consommateur dans une application mobile._

![](~/media/shared/preview.png "Cette API est actuellement en version préliminaire")

> [!NOTE]
> Le [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) est toujours en version préliminaire, mais convient pour une utilisation dans un environnement de production. Toutefois, il peut être modifications avec rupture à l’API, format de cache interne et d’autres mécanismes de la bibliothèque, ce qui peut avoir un impact sur votre application.

## <a name="overview"></a>Vue d'ensemble

Azure B2C Active de répertoire est un service de gestion d’identité pour les applications destinées aux consommateurs, qui permet aux consommateurs pour se connecter à votre application par :

- À l’aide de leurs comptes sociaux existants (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Création de nouvelles informations d’identification (adresse de messagerie et mot de passe, ou nom d’utilisateur et mot de passe). Ces informations d’identification sont appelées *local* comptes.

Le processus pour intégrer le service de gestion d’identité Azure Active Directory B2C dans une application mobile est comme suit :

1. Créer un locataire Azure Active Directory B2C. Pour plus d’informations, consultez [créer un locataire Azure Active Directory B2C dans le portail Azure](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Inscrire votre application mobile avec le locataire Azure Active Directory B2C. Le processus d’inscription attribue un **ID d’Application** qui identifie de façon unique votre application et un **l’URL de redirection** qui peut être utilisé pour diriger les réponses à votre application. Pour plus d’informations, consultez [Azure Active Directory B2C : inscription de votre application](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Créer une stratégie d’inscription et la connexion. Cette stratégie définit les expériences clients seront pendant l’inscription et la connexion et spécifie également le contenu des jetons que l’application recevra après son inscription ou de connexion. Pour plus d’informations, consultez [Azure Active Directory B2C : stratégies intégrées](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Utilisez le [bibliothèque d’authentification Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) dans votre application mobile pour initier un flux de travail de l’authentification auprès de votre client Azure Active Directory B2C.

> [!NOTE]
> Ainsi que l’intégration de gestion des identités Azure Active Directory B2C dans les applications mobiles, MSAL peut également servir à intégrer la gestion des identités Azure Active Directory dans les applications mobiles. Cela est possible en inscrivant une application mobile avec Azure Active Directory à la [portail d’inscription des applications](https://apps.dev.microsoft.com/). Le processus d’inscription attribue un **ID d’Application** qui identifie de façon unique votre application, qui doit être spécifiée lors de l’utilisation de MSAL. Pour plus d’informations, consultez [comment inscrire une application avec le point de terminaison v2.0](/azure/active-directory/develop/active-directory-v2-app-registration/), et [authentifier votre Mobile Apps à l’aide de Microsoft Authentication Library](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) sur le blog de Xamarin.

MSAL utilise le navigateur web de l’appareil pour effectuer l’authentification. Cela améliore la facilité d’utilisation d’une application, comme les utilisateurs doivent uniquement se connecter une fois par appareil, améliorer le taux de conversion de l’authentification et l’autorisation de flux dans l’application. Navigateur de l’appareil fournit également une sécurité améliorée. Une fois que l’utilisateur a terminé le processus d’authentification, contrôle retourne à l’application à partir de l’onglet du navigateur web. Pour cela, vous devez l’inscription d’un schéma d’URL personnalisé pour l’URL de redirection qui est retourné par le processus d’authentification, puis détecter et gérer l’URL personnalisée une fois qu’elle est envoyée. Pour plus d’informations sur le choix d’un schéma d’URL personnalisé, consultez [en choisissant un URI de redirection application native](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Le mécanisme d’inscription d’un schéma d’URL personnalisé avec le système d’exploitation et le schéma de manutention est spécifique à chaque plateforme.

Chaque requête est envoyée à un locataire Azure Active Directory B2C spécifie une *stratégie*. Stratégies décrivent les expériences d’identité tels que l’inscription, ou de connexion. Par exemple, une stratégie d’inscription permet le comportement du client Azure Active Directory B2C être configuré via les paramètres suivants :

- Types de compte que les clients peuvent utiliser pour se connecter à l’application.
- Données à recueillir auprès du client lors de l’inscription.
- Authentification multifacteur.
- Contenu de la page d’inscription.
- Revendications de jeton que l’application mobile reçoit lorsque la stratégie a été exécutée.

Un locataire Azure Active Directory peut contenir plusieurs stratégies de types différents, ce qui peuvent ensuite être utilisées dans votre application en fonction des besoins. En outre, les stratégies peuvent être réutilisées entre les applications, ce qui vous permet de définir et modifier les expériences d’identité sans modifier votre code. Pour plus d’informations sur les stratégies, consultez [Azure Active Directory B2C : stratégies intégrées](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Installation

La bibliothèque Microsoft Authentication Library (MSAL) NuGet doit être ajoutée au projet de bibliothèque de classes Portable (PCL) et les projets de plateforme dans une solution Xamarin.Forms. Les sections suivantes fournissent des instructions d’installation supplémentaires pour l’utilisation de MSAL pour communiquer avec un locataire Azure Active Directory B2C depuis une application mobile.

### <a name="portable-class-library"></a>Bibliothèque de classes portable

Bibliothèques de classes portables qui utilisent la bibliothèque MSAL devrez être reciblé pour utiliser Profile7. Pour plus d’informations sur les bibliothèques de classes portables, consultez [Introduction aux bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

Sur iOS, le schéma d’URL personnalisé qui a été inscrit avec Azure Active Directory B2C doit être inscrite dans **Info.plist**, comme illustré dans la capture d’écran suivante :

![](azure-ad-b2c-images/customurl-ios.png "L’inscription d’un schéma d’URL personnalisée sur iOS")

Azure Active Directory B2C issue de la demande d’autorisation, il redirige vers l’URL de redirection inscrits. Étant donné que l’URL utilise un schéma personnalisé, il en résulte dans iOS lancement de l’application mobile, en passant l’URL comme un paramètre de lancement, où elle est traitée par le `OpenUrl` la substitution de l’application `AppDelegate` (classe), ce qui est illustré dans le code suivant exemple :

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

Le code dans le `OpenURL` méthode garantit que le contrôle retourne à la bibliothèque MSAL une fois que la partie interactive du flux de travail d’authentification s’est terminée.

### <a name="android"></a>Android

Sur Android, le schéma d’URL personnalisé qui a été inscrit avec Azure Active Directory B2C doit être inscrite dans **AndroidManifest.xml**, en ajoutant un `<activity>` élément à l’intérieur existant `<application>` élément. Le `<activity>` élément spécifie la `IntentFilter` sur le `Activity` qui gère le schéma et est illustré dans l’exemple suivant :

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Azure Active Directory B2C issue de la demande d’autorisation, il redirige vers l’URL de redirection inscrits. Étant donné que l’URL utilise un schéma personnalisé, il en résulte dans Android lancement de l’application mobile, en passant l’URL comme un paramètre de lancement, où elle est traitée par le `microsoft.identity.client.BrowserTabActivity`. Notez que le `data android:scheme` propriété doit être définie sur le schéma d’URL personnalisé qui est inscrit avec l’application Azure Active Directory B2C.

En outre, la `MainActivity` classe doit être modifiée, comme indiqué dans l’exemple de code suivant :

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

Le `OnCreate` méthode est modifiée en assignant un `UIParent` l’instance à la `App.UiParent` propriété. Cela garantit que le flux d’authentification se produit dans le contexte de l’activité actuelle.

Le code dans le `OnActivityResult` méthode garantit que le contrôle retourne à la bibliothèque MSAL une fois que la partie interactive du flux de travail d’authentification s’est terminée.

### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Sur la plateforme Windows universelle, sans configuration supplémentaire est nécessaire pour utiliser la bibliothèque MSAL.

## <a name="initialization"></a>Initialisation

La bibliothèque d’authentification Microsoft utilise des membres de la `PublicClientApplication` classe pour lancer un workflow d’authentification. L’exemple d’application déclare et initialise un `public` propriété de ce type, nommé `ADB2CClient`, dans le `AuthenticationProvider` classe. L’exemple de code suivant montre comment cette propriété est initialisée :

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Lorsque l’application mobile a été inscrit avec le locataire Azure Active Directory B2C, le processus d’inscription affecté un **ID d’Application**. Cet ID doit être spécifié dans le `PublicClientApplication` constructeur, avec un `Authority` constante qui comprend une URL de base et la stratégie Azure Active Directory B2C doit être exécuté.

## <a name="signing-in"></a>La connexion

L’écran de connexion dans l’exemple d’application est indiqué dans les captures d’écran suivante :

![](azure-ad-b2c-images/login.png "Page de connexion")

Connectez-vous avec les fournisseurs d’identité sociale, ou avec un compte local, sont autorisés. Alors que Microsoft, Google et Facebook, comme indiqué ci-dessus, sont utilisés comme fournisseurs d’identité sociale, autres fournisseurs d’identité peuvent également servir.

L’exemple de code suivant montre comment le processus de connexion est appelé :

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

Le `AcquireTokenAsync` méthode lance le navigateur web de l’appareil et affiche les options d’authentification définies dans la stratégie d’Azure Active Directory B2C est spécifiée par la stratégie référencée par le biais du `Constants.Authority` constante. Cette stratégie définit les expériences clients seront pendant l’inscription et la connexion et les revendications de que l’application recevra après son inscription ou de connexion.

Le résultat de la `AcquireTokenAsync` appel de méthode est un `AuthenticationResult` instance. Si l’authentification réussit, le `AuthenticationResult` instance contiendra un jeton d’identité, qui sera mis en cache localement. Si l’authentification échoue, le `AuthenticationResult` instance contiendra les données qui indique pourquoi l’authentification a échoué.

Dans l’exemple d’application, si l’authentification réussit, le `TodoList` page cible.

## <a name="silent-re-authentication"></a>Nouvelle authentification en mode silencieux

Lorsque le `LoginPage` dans l’exemple application s’affiche, une tentative est effectuée pour récupérer un jeton d’utilisateur sans afficher d’interface utilisateur de l’authentification. Cela est possible avec la `AcquireTokenSilentAsync` méthode, comme illustré dans l’exemple de code suivant :

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

Le `AcquireTokenSilentAsync` méthode tente de récupérer un jeton d’utilisateur dans le cache, sans que l’utilisateur pour se connecter. Cela gère le scénario où un jeton approprié peut déjà être présent dans le cache de sessions précédentes. Si la tentative d’obtention d’un jeton est réussie, le `TodoList` page cible. Si la tentative d’obtention d’un jeton échoue, rien ne se produit et l’utilisateur aura la possibilité de lancer un nouveau flux de travail de l’authentification.

## <a name="signing-out"></a>Déconnexion

L’exemple de code suivant montre comment le processus de déconnexion est appelé :

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Cette opération efface tous les jetons d’authentification à partir du cache local.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser Microsoft Authentication Library (MSAL) et Azure Active Directory B2C pour intégrer la gestion des identités de consommateur dans une application mobile. Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs.


## <a name="related-links"></a>Liens associés

- [AzureADB2CAuth (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Bibliothèque d’authentification Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client)
