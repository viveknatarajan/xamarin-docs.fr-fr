---
title: Modifications Web dans iOS 11
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: 5cbf1d2f6c7b8a110cb65cad81df18f9f0568fda
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="web-changes-in-ios-11"></a>Modifications Web dans iOS 11

iOS 11 introduit une nouvelle version du navigateur web Safari – Safari 11.0 – qui inclut les modifications apportées aux WebKit et SafariServices. Ce guide explore ces modifications.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` a été introduite dans iOS 9 comme une option de l’affichage du contenu web ou l’authentification des utilisateurs à partir de votre application. Vous trouverez plus d’informations sur ses fonctionnalités dans le [affichages Web](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) guide.

iOS 11 a introduit les mises à jour de style vers le contrôleur de vue Safari, en donnant à vos utilisateurs une expérience plus transparente entre une application et le web. Par exemple, la suppression de l’adresse barre maintenant donne le contrôleur de vue la convivialité d’un navigateur dans l’application, plutôt que d’un mini-navigateur Safari. Vous pouvez également personnaliser le jeu de couleurs pour accueillir le jeu de couleurs de votre application en définissant le `preferredBarTintColor` et `PreferredControlTintColor` propriétés :

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

L’extrait de code suivant affiche les barres en violet et blanc, tel qu’affiché dans l’image suivante :

![Barres de SFSafariViewController rendues en violet et blanc](web-images/image1.png)

Le bouton Ignorer présenté dans le contrôleur de la vue Safari peut également être modifié en définissant le `DismissButtonStyle` propriété `Done`, `Close`, ou `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Ignorer le texte du bouton modifié](web-images/image2.png)

Cette valeur peut être modifiée pendant que `SFSafariViewController` est présenté.


Selon le contenu qui s’affiche à l’intérieur d’un contrôleur de vue Safari, il peut être nécessaire pour garantir que les barres de menus ne réduire en tant que l’utilisateur fait défiler. Cette option est activée en définissant la nouvelle `BarCollapsedEnabled` propriété `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Barre de réduction désactivé](web-images/image3.png)

Apple a également mis à jour la confidentialité dans le contrôleur de la vue Safari dans iOS 11. À présent, exploration de données telles que les cookies et stockage local n’existent que sur une base par application, plutôt qu’à toutes les instances de contrôleur d’affichage Safari. Cela permet de conserver l’activité des utilisateurs navigation privé au sein de votre application.

Des fonctionnalités supplémentaires telles que faites glisser-déplacer prise en charge pour les URL et prise en charge de `window.open()` ont également été ajoutés à `SFSafariViewController` dans iOS 11. Vous trouverez plus d’informations sur ces nouvelles fonctionnalités dans [documentation de SFSafariViewController d’Apple](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` a été introduite dans le cadre de WebKit dans iOS 8 comme un moyen de l’affichage du contenu web à votre utilisateur. Il est beaucoup plus personnalisable que `SFSafariViewController`, ce qui vous permet de créer votre propre interface utilisateur et de navigation.

Apple a introduit les trois principales améliorations pour `WKWebView` avec iOS 11 : 

- La possibilité de gérer les cookies
- Filtrage de contenu
- Chargement de la ressource personnalisée. 

Gestion des cookies est effectuée via le nouveau [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) (classe), ce qui vous permet d’ajouter et supprimer les cookies, pour obtenir tous les cookies stockés dans un WKWebView et permet d’observer le cookie à stocker les modifications.

Contenu de filtrage vous permet de gérer le type de contenu que votre utilisateur s’affiche, ce qui vous permet de vous assurer qu’il est sécurisé, famille et, si nécessaire, disponible uniquement pour un groupe d’utilisateurs. Cela est implémenté via le nouveau [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) (classe), en fournissant des paires de déclencheurs et actions dans JSON. Vous trouverez plus d’informations sur les déclencheurs et les actions dans Apple [les règles de blocage de contenu](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) guide.

iOS 11 maintenant vous permet de personnaliser `WKWebView` avec la ressource personnalisée, le chargement de votre contenu web. Cela est implémenté via le `IWKUrlSchemeHandler` interface, ce qui vous permet de gérer les schémas d’URL qui ne sont pas natives Web Kit. Cette interface a une méthode de début et de fin qui doit être implémentée :

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

Une fois que le gestionnaire a été implémenté, utilisez-la pour définir le `SetUrlSchemeHandler` propriété sur le `WKWebViewConfiguration`. Ensuite, chargez l’URL d’un objet qui utilise le schéma personnalisé :

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

