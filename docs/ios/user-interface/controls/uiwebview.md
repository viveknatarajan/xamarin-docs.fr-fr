---
title: Affichages Web du Xamarin.iOS
description: Ce document décrit les différentes méthodes qu’une application Xamarin.iOS peut afficher le contenu web. Elle explique UIWebView, WKWebView, SFSafariViewController, Safari et la sécurité de transport d’application.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f720eae68415ab9efe021e53c9da4875209cd221
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790494"
---
# <a name="web-views-in-xamarinios"></a>Affichages Web du Xamarin.iOS

La durée de vie d’e/s, Apple a publié de plusieurs façons pour les développeurs d’applications d’incorporer la fonctionnalité d’affichage web dans leurs applications. La plupart des utilisateurs utilisent le navigateur web Safari intégré sur son appareil iOS et donc vous attendre que la fonctionnalité web-view à partir d’autres applications est cohérente avec cette expérience. Ils attendent les mouvements mêmes fonctionne, les performances soit de par et des fonctionnalités identiques.

Dans cet article, nous allons examiner chacun des trois vues Web fournies par Apple : `UIWebView`, `WKWebview`, et `SFSafariViewController`, les similitudes et différences et comment ils peuvent être utilisés. 

iOS 11 a introduit de nouvelles modifications à la fois à `WKWebView` et `SFSafariViewController`. Pour plus d’informations, consultez la [Web les modifications dans le guide d’iOS 11](~/ios/platform/introduction-to-ios11/web.md) guide.

## <a name="uiwebview"></a>UIWebView

`UIWebView` consiste d’Apple hérité de fourniture de contenu web dans votre application. Il a été publiée en iOS 2.0 et a été déconseillée à compter de 8.0.

Si vous envisagez de prendre en charge des versions d’iOS antérieure à 8.0, vous devrez utiliser `UIWebView`. Dû au fait que `UIWebView` est moins optimisée pour les performances que les alternatives, il est recommandé que vous devez vérifier la version iOS de l’utilisateur. Si elle 8.0 ou version ultérieure, à l’aide des options expliquent ci-dessous crée une meilleure expérience utilisateur.
 
Pour ajouter un UIWebView à votre application Xamarin.iOS, utilisez le code suivant :
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Cela génère l’affichage web suivant :

[![](uiwebview-images/webview.png "L’effet de ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Pour plus d’informations sur l’utilisation de `UIWebView`, consultez les recettes suivantes :


- [Charger une Page Web](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [Charger le contenu Local](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [Charger les Documents Non Web](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` a été introduit dans iOS 8 autorise les développeurs d’applications pour implémenter une interface similaire à celle de Safari mobile de navigation web. Il s’agit, en partie, le fait que `WKWebView` utilise le moteur Nitro Javascript, le même moteur utilisé par mobile Safari. `WKWebView` doit toujours être utilisé over UIWebView ont été possible à la [des performances accrues](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), intégrés mouvements conviviales et la facilité d’interaction entre votre application et de la page web.
  
`WKWebView` peuvent être ajoutés à votre application de manière quasiment identique à UIWebView, et Toutefois, tant que développeur, vous avez beaucoup plus de contrôle sur les fonctionnalités et l’interface utilisateur/UX. Création et affichage de l’objet de vue web affichera la page demandée, mais vous pouvez contrôler la présentation de la vue, l’utilisateur peut naviguer et comment l’utilisateur quitte la vue.  

Le code ci-dessous peut être utilisé pour lancer une `WKWebView` dans votre application Xamarin.iOS :

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Cela génère l’affichage web suivant :

[![](uiwebview-images/wkwebview.png "Une vue de web exemple sans ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Il est important de noter que `WKWebView` se trouve dans l’espace de noms WebKit, vous devrez ajouter cela à l’aide de la directive au début de votre classe.

`WKWebView` peut également servir dans les applications Xamarin.Mac, et vous avez donc envisager de l’utiliser si vous créez une application de Mac/iOS inter-plateformes.

Le [gérer les alertes JavaScript](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) recette fournit également des informations sur l’utilisation de WKWebView avec Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` est la façon la plus récente pour fournir du contenu web à partir de votre application et est disponible dans iOS 9 et versions ultérieures. Contrairement aux `UIWebView` ou `WKWebView`, `SFSafariViewController` est un contrôleur de vue et ne peut donc pas être utilisé avec d’autres vues. Vous devez présenter `SFSafariViewController` en tant que nouvelle vue contrôleur, de la même façon vous les présente n’importe quel contrôleur de la vue.
 
 `SFSafariViewController` est essentiellement une « mini safari » qui peut être incorporé dans votre application. Comme WKWebView, il utilise le même moteur Javascript Nitro, mais offre également une gamme de fonctionnalités Safari supplémentaires telles que le remplissage automatique, de lecteur et la capacité à partager les cookies et les données avec Safari mobile. Interaction entre l’utilisateur et le `SFSafariViewController` n’est pas accessible à votre application. Votre application n’aura pas accès à une des fonctionnalités par défaut Safari.
 
Elle, par défaut, implémente également un **fait** bouton, ce qui permet à l’utilisateur de revenir facilement à votre application et précédent et boutons de navigation, qui permet à votre utilisateur de naviguer dans une pile de pages web. En outre, il fournit également l’utilisateur avec une adresse de la barre en leur donnant la tranquillité d’esprit qu’ils sont sur la page web attendu. La barre d’adresse n’autorise pas l’utilisateur de modifier l’url. 

Ces implémentations ne peut pas être modifié, donc `SFSafariViewController` est idéale pour l’utiliser comme navigateur par défaut, si votre application souhaite présenter une page Web sans aucune personnalisation.

Le code ci-dessous peut être utilisé pour lancer une `SFSafariViewController` dans votre application Xamarin.iOS :

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Cela génère l’affichage web suivant :

[![](uiwebview-images/sfsafariviewcontroller.png "Une vue web d’exemple avec SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Il est également possible d’ouvrir l’application mobile Safari à partir de votre application, en utilisant le code ci-dessous :

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Cela génère l’affichage web suivant :

[![](uiwebview-images/safari.png "Une page web présentée dans Safari")](uiwebview-images/safari.png#lightbox)

Navigation dans les utilisateurs en dehors de votre application à Safari doit généralement toujours être évitée. La plupart des utilisateurs s’attendront pas navigation en dehors de votre application, donc si vous naviguez en dehors de votre application, les utilisateurs ne peuvent jamais retourner, essentiellement tuer engagement.

améliorations d’iOS 9 Autoriser l’utilisateur facilement revenir à votre application via un bouton précédent qui est fourni dans l’angle supérieur gauche de la page Safari.

## <a name="app-transport-security"></a>Sécurité de Transport de l’application

Application de sécurité de Transport, ou *ATS* a été introduite par Apple dans iOS 9 pour vous assurer que toutes les communications internet sont conformes pour sécuriser des meilleures pratiques de connexion.

Pour plus d’informations sur les couches d’application, y compris comment l’implémenter dans votre application, reportez-vous à la [sécurité du Transport application](~/ios/app-fundamentals/ats.md) guide.

## <a name="related-links"></a>Liens associés

- [Vues Web (exemple)](https://developer.xamarin.com/samples/monotouch/WebView/)
