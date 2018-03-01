---
title: "L’authentification des utilisateurs avec un fournisseur d’identité"
description: "Xamarin.Auth est un kit de développement multiplateforme pour l’authentification des utilisateurs et le stockage de leurs comptes. Il inclut les authentificateurs OAuth qui fournissent la prise en charge pour l’utilisation de fournisseurs d’identité tels que Google, Microsoft, Facebook et Twitter. Cet article explique comment utiliser Xamarin.Auth pour gérer le processus d’authentification dans une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: ff0403fedf75ab22986f5fc83d16db3dbf8d92b6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-users-with-an-identity-provider"></a>L’authentification des utilisateurs avec un fournisseur d’identité

_Xamarin.Auth est un kit de développement multiplateforme pour l’authentification des utilisateurs et le stockage de leurs comptes. Il inclut les authentificateurs OAuth qui fournissent la prise en charge pour l’utilisation de fournisseurs d’identité tels que Google, Microsoft, Facebook et Twitter. Cet article explique comment utiliser Xamarin.Auth pour gérer le processus d’authentification dans une application de Xamarin.Forms._

OAuth est une norme ouverte pour l’authentification et permet à un propriétaire de la ressource notifier à un fournisseur de ressources que l’autorisation doit être accordée à un tiers pour accéder à leurs informations sans partager l’identité de propriétaires de ressources. Un exemple de ce serait d’activer un utilisateur à notifier à un fournisseur d’identité (par exemple, Google, Microsoft, Facebook et Twitter) que l’autorisation doit être accordée à une application d’accéder à leurs données, sans partage l’identité de l’utilisateur. Elle est généralement utilisée en tant qu’approche pour les utilisateurs à se connecter aux sites Web et applications à l’aide d’un fournisseur d’identité, mais sans exposer son mot de passe à l’application ou le site Web.

Une vue d’ensemble du flux d’authentification lors de l’utilisation d’un fournisseur d’identité OAuth est comme suit :

1. L’application accède à un navigateur vers une URL de fournisseur d’identité.
1. Le fournisseur d’identité gère l’authentification utilisateur et retourne un code d’autorisation à l’application.
1. L’application échange le code d’autorisation pour un jeton d’accès du fournisseur d’identité.
1. L’application utilise le jeton d’accès pour accéder aux API sur le fournisseur d’identité, par exemple une API pour demander des données de l’utilisateur de base.

L’exemple d’application montre comment utiliser Xamarin.Auth pour implémenter un flux d’authentification natif contre Google. Bien que Google est utilisé comme fournisseur d’identité dans cette rubrique, l’approche s’applique également à autres fournisseurs d’identité. Pour plus d’informations sur l’authentification à l’aide du point de terminaison de Google OAuth 2.0, consultez [à l’aide d’OAuth2.0 aux accès Google APIs](https://developers.google.com/identity/protocols/OAuth2) sur le site Web de Google.

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>À l’aide de Xamarin.Auth pour authentifier les utilisateurs

Xamarin.Auth prend en charge les deux approches pour les applications d’interagir avec le point de terminaison du fournisseur d’identité d’autorisation :

1. À l’aide d’un affichage web intégré. Cela a été une pratique courante, il n’est plus recommandé pour les raisons suivantes :

    - L’application qui héberge l’affichage web peut accéder aux informations d’identification de l’utilisateur l’authentification complète, pas seulement l’octroi d’autorisation OAuth qui a été conçue pour l’application. Cela viole le principe de privilège minimum, que l’application a accès aux informations d’identification plus puissantes qu’il requiert, éventuellement augmenter la surface d’attaque de l’application.
    - L’application hôte peut capturer les noms d’utilisateur et mots de passe, automatiquement soumettez des formulaires et contourner le consentement de l’utilisateur et copier les cookies de session et les utiliser pour effectuer des actions authentifiées en tant que l’utilisateur.
    - Affichages web incorporé ne partagent l’état de l’authentification avec les autres applications ou navigateur du périphérique, demandant à l’utilisateur pour se connecter pour chaque demande d’autorisation qui est considérée comme une expérience utilisateur inférieur.
    - Certains points de terminaison d’autorisation prennent des mesures pour détecter et bloquer les demandes d’autorisation provenant de vues web.

1. À l’aide du navigateur web du périphérique, qui est l’approche recommandée. À l’aide du navigateur de l’appareil pour les demandes d’OAuth permet d’améliorer la facilité d’utilisation d’une application, comme les utilisateurs doivent uniquement se connecter au fournisseur d’identité qu’une seule fois par périphérique, améliorer le taux de conversion de flux de connexion et d’autorisation de l’application. Le navigateur de l’appareil fournit également améliorer la sécurité que les applications peuvent inspecter et modifier du contenu dans un affichage web, mais pas le contenu affiché dans le navigateur. Il s’agit de l’approche adoptée dans cet article et l’exemple d’application.

Une vue d’ensemble de la façon dont l’exemple d’application utilise Xamarin.Auth pour authentifier les utilisateurs et récupérer leurs données de base est illustré dans le diagramme suivant :

![](oauth-images/google-auth.png "À l’aide de Xamarin.Auth pour s’authentifier auprès de Google")

L’application effectue une demande d’authentification à l’aide de Google la `OAuth2Authenticator` classe. Une réponse d’authentification est retournée, une fois que l’utilisateur a été authentifié avec Google via leur page de connexion, qui inclut un jeton d’accès. L’application effectue une demande pour Google pour les données utilisateur de base, à l’aide de la `OAuth2Request` (classe), avec le jeton d’accès qui est inclus dans la demande.

### <a name="setup"></a>Installation

Un projet de Console des API Google doit être créé pour intégrer Google connectez-vous à une application de Xamarin.Forms. Cela peut être accompli de la façon suivante :

1. Accédez à la [Console des API Google](http://console.developers.google.com) le site Web et connectez-vous avec les informations d’identification du compte Google.
1. Dans la liste déroulante projet, sélectionnez un projet existant ou créez-en un.
1. Dans la barre latérale sous « API Manager », sélectionnez **informations d’identification**, puis sélectionnez le **onglet écran de consentement OAuth**. Choisissez un **adresse de messagerie**, spécifiez un **nom de produit présenté aux utilisateurs**, puis appuyez sur **enregistrer**.
1. Dans le **informations d’identification** onglet, sélectionnez le **créer les informations d’identification** déroulante liste, puis choisissez **ID de client OAuth**.
1. Sous **type d’Application**, sélectionnez la plateforme qui exécutera l’application mobile sur (**iOS** ou **Android**).
1. Renseignez les informations requises, sélectionnez le **créer** bouton.

> [!NOTE]
> Un ID Client permet à une application d’accéder à activé APIs Google et pour les applications mobiles est propre à une plate-forme unique. Par conséquent, un **ID de client OAuth** doit être créé pour chaque plateforme que va utiliser l’authentification dans Google.

Après avoir effectué ces étapes, Xamarin.Auth peut être utilisé pour lancer un flux d’authentification OAuth2 avec Google.

### <a name="creating-and-configuring-an-authenticator"></a>Création et configuration d’un authentificateur

De Xamarin.Auth `OAuth2Authenticator` classe est chargé de gérer le flux d’authentification OAuth. L’exemple de code suivant illustre l’instanciation de la `OAuth2Authenticator` classe lors de l’authentification à l’aide du navigateur web de l’appareil :

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

La `OAuth2Authenticator` classe nécessite un nombre de paramètres, qui sont les suivantes :

- **ID client** – identifie le client qui effectue la demande et peut être récupéré à partir du projet dans le [Console des API Google](http://console.developers.google.com).
- **Clé secrète client** : il doit s’agir `null` ou `string.Empty`.
- **Étendue** : ce nom identifie l’accès aux API demandée par l’application, et la valeur indique à l’écran de consentement est présentée à l’utilisateur. Pour plus d’informations sur les étendues, consultez [demande d’autorisation de l’API](https://developers.google.com/+/web/api/rest/oauth) sur le site Web de Google.
- **Autoriser les URL** – identifie l’URL où le code d’autorisation est obtenu depuis.
- **URL de redirection** – identifie l’URL où la réponse est envoyée. La valeur de ce paramètre doit correspondre à une des valeurs qui s’affiche dans le **informations d’identification** onglet pour le projet dans le [Console des développeurs Google](https://console.developers.google.com/).
- **Url de AccessToken** – identifie l’URL utilisée pour demander des jetons d’accès après obtention d’un code d’autorisation.
- **GetUserNameAsync Func** : facultatif `Func` qui servira à extraire le nom d’utilisateur du compte de façon asynchrone une fois qu’il est correctement authentifié.
- **Utilisez l’interface utilisateur Native** : un `boolean` valeur indiquant s’il faut utiliser le navigateur web de l’appareil pour effectuer la demande d’authentification.

### <a name="setup-authentication-event-handlers"></a>Le programme d’installation de gestionnaires d’événements d’authentification

Avant de présenter l’interface utilisateur, un gestionnaire d’événements pour le `OAuth2Authenticator.Completed` l’événement doit être inscrit, comme indiqué dans l’exemple de code suivant :

```csharp
authenticator.Completed += OnAuthCompleted;
```

Cet événement se déclenche lorsque l’utilisateur s’authentifie avec succès ou annule la connexion.

Si vous le souhaitez, un gestionnaire d’événements pour le `OAuth2Authenticator.Error` l’événement peut également être inscrit.

### <a name="presenting-the-sign-in-user-interface"></a>Présentation de l’Interface utilisateur de connexion

L’interface utilisateur de connexion peut être présentée à l’utilisateur à l’aide du présentateur connexion Xamarin.Auth, qui doit être initialisé dans chaque projet de plateforme. L’exemple de code suivant montre comment initialiser un présentateur de connexion dans la `AppDelegate` classe dans le projet iOS :

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

L’exemple de code suivant montre comment initialiser un présentateur de connexion dans la `MainActivity` classe dans le projet Android :

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Le projet de bibliothèque de classes Portable (PCL) peut ensuite appeler le présentateur de connexion comme suit :

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Notez que l’argument de la `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` méthode est la `OAuth2Authenticator` instance. Lorsque la `Login` méthode est appelée, l’interface utilisateur de connexion est présenté à l’utilisateur dans un onglet de navigateur web du périphérique, qui est indiqué dans les captures d’écran suivants :

![](oauth-images/login.png "Connexion à Google")

### <a name="processing-the-redirect-url"></a>Traitement de l’URL de redirection

Une fois que l’utilisateur termine le processus d’authentification, contrôle retourne à l’application à partir de l’onglet de navigateur web. Cela est possible en inscrivant un modèle d’URL personnalisé pour l’URL de redirection qui est retourné par le processus d’authentification, détection et gestion de l’URL personnalisée une fois qu’elle est envoyée.

Lorsque vous choisissez un modèle d’URL personnalisé à associer à une application, les applications doivent utiliser un modèle basé sur un nom sous leur contrôle. Cela peut être obtenue en utilisant le nom d’identificateur de groupe sur iOS et le nom du package sur Android et en inversant les pour rendre le schéma d’URL. Toutefois, certains fournisseurs d’identité, tels que Google, assignent des identificateurs de client basés sur les noms de domaine, qui sont ensuite inversées et utilisés en tant que le schéma d’URL. Par exemple, si Google crée un id client de `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, le schéma d’URL seront `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Notez que seul un seul `/` peuvent s’afficher après le composant de schéma. Par conséquent, un exemple complet d’une URL de redirection utilisant un modèle d’URL personnalisé est `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Lorsque le navigateur web reçoit une réponse du fournisseur d’identité qui contient un modèle d’URL personnalisé, il ne tente de charger l’URL, qui échoue. Au lieu de cela, le schéma d’URL est signalé dans le système d’exploitation en déclenchant un événement. Il vérifie ensuite le système d’exploitation pour les schémas inscrits, et s’il existe, le système d’exploitation sera lancer l’application qui inscrit le schéma et les envoyer à l’URL de redirection.

Le mécanisme de l’inscription d’un modèle d’URL personnalisé avec le système d’exploitation et le schéma de manutention est spécifique à chaque plateforme.

#### <a name="ios"></a>iOS

Sur iOS, un modèle d’URL personnalisé est enregistré dans **Info.plist**, comme illustré dans la capture d’écran suivante :

![](oauth-images/info-plist.png "Enregistrement du modèle d’URL")

Le **identificateur** valeur peut être n’importe quoi et le **rôle** doit avoir la valeur **visionneuse**. Le **schémas d’Url** valeur, qui commence par `com.googleusercontent.apps`, peut être obtenu à partir de l’id de client pour le projet iOS sur [Console des API Google](http://console.developers.google.com).

Lorsque le fournisseur d’identité termine à la demande d’autorisation, il redirige vers les URL de redirection de l’application. Étant donné que l’URL utilise un schéma personnalisé, il en résulte iOS lancement de l’application, en passant l’URL comme un paramètre de lancement, où il est traité par le `OpenUrl` la substitution de l’application `AppDelegate` (classe), qui est affichée dans l’exemple de code suivant :

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

Le `OpenUrl` méthode convertit l’URL reçu à partir une `NSUrl` à .NET `Uri`, avant le traitement de l’URL de redirection avec la `OnPageLoading` méthode publique `OAuth2Authenticator` objet. Cela provoque une Xamarin.Auth pour fermer l’onglet de navigateur web et analyser les données OAuth reçues.

#### <a name="android"></a>Android

Sur Android, un modèle d’URL personnalisé est enregistré en spécifiant un [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) de l’attribut le `Activity` qui gérera le schéma. Lorsque le fournisseur d’identité termine à la demande d’autorisation, il redirige vers les URL de redirection de l’application. Comme l’URL utilise un schéma personnalisé, il en résulte Android lancement de l’application, en passant l’URL comme un paramètre de lancement, où il est traité par le `OnCreate` méthode de la `Activity` inscrit pour traiter le modèle d’URL personnalisé. L’exemple de code suivant illustre la classe à partir de l’exemple d’application qui gère le schéma d’URL personnalisé :

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

Le `DataSchemes` propriété de la [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) doit être définie à l’identificateur de client inversé est obtenu à partir de l’id de client Android pour le projet sur [Console des API Google](http://console.developers.google.com).

Le `OnCreate` méthode convertit l’URL reçu à partir une `Android.Net.Url` à .NET `Uri`, avant le traitement de l’URL de redirection avec la `OnPageLoading` méthode publique `OAuth2Authenticator` objet. Cela provoque une Xamarin.Auth à fermer l’onglet de navigateur web et à analyser les données OAuth reçues.

> [!IMPORTANT]
> Sur Android, Xamarin.Auth utilise le `CustomTabs` API pour communiquer avec le navigateur web et le système d’exploitation. Toutefois, il n'est pas certain qu’un `CustomTabs` navigateur compatible sera installé sur l’appareil de l’utilisateur.

### <a name="examining-the-oauth-response"></a>Examen de la réponse OAuth

Après l’analyse des données reçues OAuth, Xamarin.Auth déclenchera la `OAuth2Authenticator.Completed` événement. Dans le Gestionnaire d’événements pour cet événement, le `AuthenticatorCompletedEventArgs.IsAuthenticated` propriété peut être utilisée pour identifier si l’authentification a réussi, comme indiqué dans l’exemple de code suivant :

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

Les données collectées à partir d’une authentification réussie seront disponibles dans le `AuthenticatorCompletedEventArgs.Account` propriété. Cela inclut un jeton d’accès, qui peut être utilisé pour signer des requêtes pour les données à une API fournie par le fournisseur d’identité.

### <a name="making-requests-for-data"></a>Effectuant des demandes de données

Une fois l’application obtient un jeton d’accès, il est utilisé pour effectuer une demande à la `https://www.googleapis.com/oauth2/v2/userinfo` API, pour demander des données utilisateur de base du fournisseur d’identité. Cette demande est faite avec de Xamarin.Auth `OAuth2Request` (classe), qui représente une demande qui est authentifiée en utilisant un compte extrait une `OAuth2Authenticator` de l’instance, comme indiqué dans l’exemple de code suivant :

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

Ainsi que la méthode HTTP et l’URL de l’API, le `OAuth2Request` instance spécifie également une `Account` instance qui contient le jeton d’accès qui signe la demande à l’URL spécifiée par la `Constants.UserInfoUrl` propriété. Le fournisseur d’identité retourne ensuite les données utilisateur de base en tant qu’une réponse JSON, y compris le nom de l’utilisateur et l’adresse de messagerie, sous réserve qu’il identifie le jeton d’accès comme étant valide. La réponse JSON est ensuite lu et désérialisée dans le `user` variable.

Pour plus d’informations, consultez [appel d’une API Google](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) sur le portail des développeurs de Google.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Le stockage et la récupération des informations de compte sur les appareils

Xamarin.Auth stocke en toute sécurité `Account` objets dans un compte de stockent afin que les applications n’ont pas toujours pour ré-authentifier les utilisateurs. Le `AccountStore` classe est chargée pour stocker les informations de compte et il est soutenu par les services de trousseau dans iOS et la `KeyStore` classe dans Android.

Le code suivant montre l’exemple comment un `Account` objet est enregistré en toute sécurité :

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Comptes enregistrées sont identifiées à l’aide d’une clé composée du compte `Username` propriété et un ID de service, qui est une chaîne qui est utilisée lors de l’extraction de comptes à partir du magasin de comptes. Si un `Account` a été précédemment enregistré, appelant le `Save` méthode nouveau remplace.

`Account` objets pour un service peut être récupéré en appelant le `FindAccountsForService` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

Le `FindAccountsForService` méthode retourne un `IEnumerable` collection de `Account` objets, avec le premier élément dans la collection qui est définie comme le compte de mise en correspondance.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser Xamarin.Auth pour gérer le processus d’authentification dans une application de Xamarin.Forms. Xamarin.Auth fournit le `OAuth2Authenticator` et `OAuth2Request` les classes qui sont utilisées par les applications de Xamarin.Forms pour consommer des fournisseurs d’identité tels que Google, Microsoft, Facebook et Twitter.


## <a name="related-links"></a>Liens associés

- [OAuthNativeFlow (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [OAuth 2.0 pour les applications natives](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [À l’aide d’OAuth2.0 pour accéder aux API de Google](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
