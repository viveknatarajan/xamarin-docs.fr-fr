---
title: Affichages Web dans Xamarin.iOS
description: Ce document décrit les différentes façons qu'une application Xamarin.iOS peut afficher le contenu web. Il aborde UIWebView, WKWebView, SFSafariViewController, Safari et la sécurité de transport d’application.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242038"
---
# <a name="web-views-in-xamarinios"></a>Affichages Web dans Xamarin.iOS

Pendant la durée de vie d’e/s, Apple a publié de plusieurs façons pour les développeurs d’applications d’incorporer des fonctionnalités d’une vue web dans leurs applications. La plupart des utilisateurs utilisent le navigateur web Safari intégré sur leur appareil iOS et donc vous attendre que les fonctionnalités d’affichage web à partir d’autres applications sont cohérentes avec cette expérience. Ils s’attendent les mouvements mêmes fonctionne, les performances doivent se trouver sur par et les fonctionnalités que les mêmes.

Dans cet article, nous allons examiner chacun des trois vues Web fournies par Apple : `UIWebView`, `WKWebview`, et `SFSafariViewController`, leurs similitudes et différences et comment elles peuvent être utilisées. 

iOS 11 introduit de nouvelles modifications à la fois aux `WKWebView` et `SFSafariViewController`. Pour plus d’informations, consultez le [Web des modifications dans iOS 11 guide](~/ios/platform/introduction-to-ios11/web.md) guide.

## <a name="uiwebview"></a>UIWebView

`UIWebView` Permet d’Apple hérité de fourniture de contenu web dans votre application. Il a été publié dans iOS 2.0 et est déprécié à compter de 8.0.

Si vous envisagez de prendre en charge des versions d’iOS antérieure à 8.0, vous devez utiliser `UIWebView`. Dû au fait que `UIWebView` est moins optimisée pour les performances que les autres solutions, il est recommandé que vous devez vérifier la version iOS de l’utilisateur. Si elle 8.0 ou version ultérieure, à l’aide des options Voici pourquoi créera une meilleure expérience utilisateur.
 
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


- [Charger une Page Web](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Charger le contenu Local](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Charger des Documents Non Web](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` a été introduit dans iOS 8 en permettant aux développeurs d’applications implémentent une interface similaire à celle de Safari mobile de navigation web. Cela est dû en partie au fait que `WKWebView` utilise le moteur Nitro Javascript, le même moteur utilisé par mobile Safari. `WKWebView` doit toujours être utilisé over UIWebView n’étaient pas possibles en raison le [des performances accrues](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), intégré mouvements conviviales et la facilité d’interaction entre la page web et de votre application.
  
`WKWebView` peuvent être ajoutés à votre application de façon quasiment identique à UIWebView, et Toutefois, tant que développeur, vous avez beaucoup plus de contrôle sur l’interface/expérience utilisateur et les fonctionnalités. Création et affichage de l’objet de vue web affichera la page demandée, mais vous pouvez contrôler la présentation de la vue, comment l’utilisateur peut accéder, et comment l’utilisateur quitte la vue.  

Le code ci-dessous peut être utilisé pour lancer une `WKWebView` dans votre application Xamarin.iOS :

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Cela génère l’affichage web suivant :

[![](uiwebview-images/wkwebview.png "Une vue web d’exemple sans ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Il est important de noter que `WKWebView` est dans l’espace de noms WebKit, donc vous devrez ajouter cela à l’aide de la directive au début de votre classe.

`WKWebView` peut également être utilisé dans les applications Xamarin.Mac, et vous avez donc envisager de l’utiliser si vous créez une application de Mac/iOS inter-plateformes.

Le [gérer les alertes de JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) Recipe (Recette) fournit également des informations sur l’utilisation de WKWebView avec Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` est la façon la plus récente pour fournir du contenu web à partir de votre application et est disponible dans iOS 9 et versions ultérieures. Contrairement aux `UIWebView` ou `WKWebView`, `SFSafariViewController` est un contrôleur d’affichage et ne peut donc pas être utilisé avec d’autres vues. Vous devez présenter `SFSafariViewController` comme un nouveau contrôleur d’affichage, de la même façon vous serez présenter n’importe quel contrôleur d’affichage.
 
 `SFSafariViewController` est essentiellement un « mini safari » qui peut être incorporé dans votre application. Comme WKWebView, il utilise le même moteur de Javascript Nitro, mais offre également une gamme de fonctionnalités de Safari supplémentaires telles que le remplissage automatique, lecteur et la capacité à partager des cookies et données avec Safari mobile. Interaction entre l’utilisateur et le `SFSafariViewController` n’est pas accessible à votre application. Votre application n’aura pas accès à aucune des fonctionnalités de Safari par défaut.
 
Il, par défaut, implémente également un **fait** bouton, ce qui permet à l’utilisateur de revenir facilement à votre application et précédent et boutons de navigation, qui permet à votre utilisateur de naviguer dans une pile de pages web. En outre, il fournit également l’utilisateur avec une adresse de la barre en leur donnant la tranquillité d’esprit qu’ils sont sur la page web attendu. La barre d’adresses n’autorise pas l’utilisateur de modifier l’url. 

Ces implémentations ne peut pas être modifié, donc `SFSafariViewController` est idéale à utiliser en tant que le navigateur par défaut, si votre application souhaite présenter une page Web sans aucune personnalisation.

Le code ci-dessous peut être utilisé pour lancer une `SFSafariViewController` dans votre application Xamarin.iOS :

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Cela génère l’affichage web suivant :

[![](uiwebview-images/sfsafariviewcontroller.png "Une vue web d’exemple avec SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Il est également possible d’ouvrir l’application Safari mobile à partir de votre application, en utilisant le code ci-dessous :

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Cela génère l’affichage web suivant :

[![](uiwebview-images/safari.png "Une page web présentée dans Safari")](uiwebview-images/safari.png#lightbox)

Navigation dans les utilisateurs à partir de votre application à Safari doit généralement toujours être évitée. La plupart des utilisateurs s’attendent pas de navigation en dehors de votre application, donc si vous quittez votre application, les utilisateurs ne peuvent jamais retourner, essentiellement tuer engagement.

améliorations d’iOS 9 Autoriser l’utilisateur à revenir facilement à votre application via un bouton précédent qui est fourni dans le coin supérieur gauche de la page de Safari.

## <a name="app-transport-security"></a>Sécurité de Transport de l’application

Sécurité de Transport d’application, ou *ATS* a été introduite par Apple dans iOS 9 pour vous assurer que toutes les communications internet sont conformes pour sécuriser des meilleures pratiques de connexion.

Pour plus d’informations sur les couches application, y compris comment l’implémenter dans votre application, reportez-vous à la [App Transport Security](~/ios/app-fundamentals/ats.md) guide.

## <a name="related-links"></a>Liens associés

- [Vues Web (exemple)](https://developer.xamarin.com/samples/monotouch/WebView/)
