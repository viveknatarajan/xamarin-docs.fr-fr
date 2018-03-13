---
title: "Infrastructure de réseaux sociaux"
description: "Le Framework Social fournit une API unifiée pour interagir avec les réseaux sociaux, y compris Twitter et Facebook, ainsi que SinaWeibo pour les utilisateurs en Chine."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 9892577d7e0ed3d3f622f881cc51db09eb44a8fd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="social-framework"></a>Infrastructure de réseaux sociaux

_Le Framework Social fournit une API unifiée pour interagir avec les réseaux sociaux, y compris Twitter et Facebook, ainsi que SinaWeibo pour les utilisateurs en Chine._


À l’aide de l’infrastructure sociale permet aux applications d’interagir avec les réseaux sociaux à partir d’une seule API sans avoir à gérer l’authentification. Il inclut un système fourni contrôleur d’affichage pour composer des publications ainsi que d’une abstraction qui permet de consommer des API de chaque réseau social sur HTTP.

> [!IMPORTANT]
> **Remarque :** pour une API inter-plateformes pour se connecter à différents réseaux sociaux, consultez le [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) composant dans le magasin de composants Xamarin.

## <a name="connecting-to-twitter"></a>Connexion à Twitter

### <a name="twitter-account-settings"></a>Paramètres de compte Twitter

Pour vous connecter à Twitter à l’aide de l’infrastructure de réseaux sociaux, un compte doit être configuré dans les paramètres de périphérique, comme indiqué ci-dessous :

 [![](social-framework-images/twitter01.png "Paramètres de compte Twitter")](social-framework-images/twitter01.png#lightbox)

Une fois qu’un compte a été entré et vérifié avec Twitter, n’importe quelle application sur le périphérique qui utilise les classes de réseaux sociaux Framework pour accéder à Twitter utilisera ce compte.

### <a name="sending-tweets"></a>Envoi de Tweets

Le Framework sociaux inclut un contrôleur appelé `SLComposeViewController` qui présente une vue fourni par le système pour la modification et envoyer un tweet. La capture d’écran suivante montre un exemple de cette vue :

 [![](social-framework-images/twitter02.png "Cette capture d’écran montre un exemple de la SLComposeViewController")](social-framework-images/twitter02.png#lightbox)

Pour utiliser un `SLComposeViewController` avec Twitter, une instance du contrôleur doit être créée en appelant le `FromService` méthode avec `SLServiceType.Twitter` comme indiqué ci-dessous :

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Après le `SLComposeViewController` instance est retournée, il peut être utilisé pour présenter une interface utilisateur pour publier sur Twitter. Toutefois, la première chose à faire est de vérifier la disponibilité du réseau social Twitter dans ce cas, en appelant `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` n’envoie jamais un tweet directement sans intervention de l’utilisateur. Toutefois, il peut être initialisé avec les méthodes suivantes :

-   `SetInitialText` : Ajoute le texte initial à afficher dans celui-ci. 
-  `AddUrl` : Ajoute une Url à celui-ci.
-  `AddImage` : Ajoute une image à celui-ci.


Une fois initialisée, appelant `PresentVIewController` affiche la vue créée par le `SLComposeViewController`. L’utilisateur peut éventuellement modifier et envoyer le tweet ou annuler l’envoi. Dans les deux cas, le contrôleur doit être fermé dans la `CompletionHandler`, où le résultat peut également être vérifié pour voir si celui-ci a été envoyé ou annulé, comme indiqué ci-dessous :

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Exemple de tweet

Le code suivant illustre l’utilisation de la `SLComposeViewController` pour présenter une vue permet d’envoyer un tweet :

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Appel d’API Twitter

Le Framework sociaux prend également en charge des demandes HTTP à des réseaux sociaux. Elle encapsule la requête dans un `SLRequest` classe qui est utilisé pour cibler les API de réseau social particulier.

Par exemple, le code suivant effectue une demande à Twitter pour obtenir la chronologie publique (de développement sur le code ci-dessus) :

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access 
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Examinons ce code en détail. Tout d’abord, il accède au magasin de comptes et obtient le type d’un compte Twitter :

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Ensuite, il demande à l’utilisateur si votre application peut avoir accès à leur compte Twitter et, si l’accès est accordé, le compte est chargé dans la mémoire et de l’interface utilisateur de mise à jour :

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

Lorsque l’utilisateur demande des données de la chronologie (en cliquant sur un bouton dans l’interface utilisateur), l’application forms tout d’abord une demande à accéder aux données de Twitter :

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
Cet exemple montre comment limiter les résultats retournés pour les dix dernières entrées en incluant `?count=10` dans l’URL. Enfin, elle attache la demande pour le compte Twitter (qui a été chargé ci-dessus) et effectue l’appel à Twitter pour extraire les données :

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Si les données a été chargées avec succès, les données JSON brutes seront affichera (comme dans l’exemple de sortie ci-dessous) :

[![](social-framework-images/twitter03.png "Un exemple de l’affichage des données JSON brut")](social-framework-images/twitter03.png#lightbox)

Dans une application réelle, les résultats JSON peuvent ensuite être analysées comme normal et les résultats présentés à l’utilisateur. Consultez [présentation des Services Web](~/cross-platform/data-cloud/web-services/index.md) pour plus d’informations sur l’analyse de JSON.

## <a name="connecting-to-facebook"></a>Connexion à Facebook

### <a name="facebook-account-settings"></a>Paramètres de compte Facebook

La connexion à Facebook avec l’infrastructure de réseaux sociaux est presque identique au processus utilisé pour Twitter indiqué ci-dessus. Un compte d’utilisateur Facebook doit être configuré dans les paramètres de périphérique, comme indiqué ci-dessous :

[![](social-framework-images/facebook01.png "Paramètres de compte Facebook")](social-framework-images/facebook01.png#lightbox)

Une fois configuré, n’importe quelle application sur le périphérique qui utilise l’infrastructure de réseaux sociaux utilisera ce compte pour se connecter à Facebook.

### <a name="posting-to-facebook"></a>Validation à Facebook

Comme l’infrastructure sociale est une API unifiée conçue pour accéder à plusieurs réseaux sociaux, le code reste presque identique, quel que soit le réseau social utilisé.

Par exemple, le `SLComposeViewController` peut être utilisé exactement comme dans l’exemple Twitter indiqué précédemment, les différents uniquement revient aux options et paramètres spécifiques à Facebook. Exemple :

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

Lorsqu’il est utilisé avec Facebook, le `SLComposeViewController` affiche une vue qui est quasiment identique à l’exemple de Twitter, affichant **Facebook** comme titre dans ce cas :

[![](social-framework-images/facebook02.png "L’affichage SLComposeViewController")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Appel d’API Graph de Facebook

Similaire à Twitter, le Framework sociaux l’exemple `SLRequest` objet peut être utilisé avec l’API graph de Facebook. Par exemple, le code suivant retourne des informations à partir de l’API graph sur le compte Xamarin (de développement sur le code ci-dessus) :

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access 
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

La seule différence entre ce code et la version de Twitter présentées ci-dessus, est la spécification de Facebook pour obtenir un développeur/spécifique ID d’application (que vous pouvez générer à partir du portail des développeurs de Facebook) qui doit être défini en tant qu’option lors de l’établissement de la demande :

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Pour définir cette option (ou à l’aide d’une clé non valide) entraînera une erreur ou Aucun renvoi des données.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser l’infrastructure de réseaux sociaux pour interagir avec Twitter et Facebook. Il a montré permettant de configurer des comptes pour chaque réseau social dans les paramètres du périphérique. Il explique également comment utiliser le `SLComposeViewController` pour présenter une vue unifiée pour la validation des réseaux sociaux. En outre, elle examinait la `SLRequest` classe qui est utilisé pour appeler l’API de chaque réseau social.


## <a name="related-links"></a>Liens associés

- [SocialFrameworkDemo (sample)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Introduction aux services web](~/cross-platform/data-cloud/web-services/index.md)
