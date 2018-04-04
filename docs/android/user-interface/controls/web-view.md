---
title: Affichage Web
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2baf7dae71ce7607c629b570ad25f477dec66c17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="web-view"></a>Affichage Web

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) vous permet de créer votre propre fenêtre pour l’affichage des pages web (ou même développer un navigateur terminé). Dans ce didacticiel, vous allez créer un simple [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) qui peut afficher et parcourir des pages web.

Créez un projet nommé **HelloWebView**.

Ouvrez **Resources/Layout/Main.axml** et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Étant donné que cette application doit accéder à Internet, vous devez ajouter que les autorisations appropriées pour le Android du fichier manifeste. Ouvrez les propriétés de votre projet pour spécifier les autorisations nécessaires au fonctionnement de votre application. Activer la `INTERNET` autorisation comme indiqué ci-dessous :

![Définition de l’autorisation de l’INTERNET dans le manifeste Android](web-view-images/01-set-internet-permissions.png)

À présent ouvrir **MainActivity.cs** et ajouter une à l’aide de la directive pour Webkit :

```csharp
using Android.Webkit;
```

En haut de la `MainActivity` classe, déclarez un [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) objet :

```csharp
WebView web_view;
```

Lorsque le **WebView** est invité à charger une URL, il sera par défaut déléguer la demande dans le navigateur par défaut. Pour que le **WebView** de charger l’URL (plutôt que le navigateur par défaut), vous devez sous-classe `Android.Webkit.WebViewClient` et remplacez le `ShouldOverriderUrlLoading` (méthode). Une instance de cette personnalisation `WebViewClient` est fournie pour le `WebView`. Pour ce faire, ajoutez suit imbriqués `HelloWebViewClient` à l’intérieur de classe `MainActivity`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Lorsque `ShouldOverrideUrlLoading` retourne `false`, il signale au Android qui actuel `WebView` instance géré la demande et aucune action supplémentaire n’est nécessaire. 

Si vous ciblez le niveau de l’API 24 ou version ultérieur, utilisez la surcharge de `ShouldOverrideUrlLoading` qui accepte un `IWebResourceRequest` pour le deuxième argument au lieu d’un `string`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

Ensuite, utilisez le code suivant pour le [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) méthode :

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Ce code initialise le membre [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) avec celui de la [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) mise en page et permet le JavaScript pour le [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) avec [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (consultez la [C d’appeler\# à partir de JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript) recette pour plus d’informations sur la façon d’appel C\# fonctions à partir de JavaScript). Enfin, une page web initiale est chargée avec [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Générez et exécutez l’application. Vous devez voir une application de visionneuse de page web simple que celui affiché dans la capture d’écran suivante :

[![Exemple d’application affichant un WebView](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Pour gérer les **précédent** bouton sur une touche, ajoutez le code suivant à l’aide d’instruction :

```csharp
using Android.Views;
```

Ensuite, ajoutez la méthode suivante à l’intérieur de la `HelloWebView` activité :

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

Cela [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) méthode de rappel est appelée chaque fois qu’un bouton est enfoncé alors que l’activité est en cours d’exécution. La condition à l’intérieur d’utilise le [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) pour vérifier si la touche activée est la **précédent** bouton et si le [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) est en mesure de navigation (si elle a un historique). Si les deux sont true, le [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) méthode est appelée, ce qui permet d’accéder arrière d’une étape le [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) l’historique. Retour de `true` indique que l’événement a été géré. Si cette condition n’est pas remplie, l’événement est envoyé sur le système.

Exécutez de nouveau l'application. Vous devez maintenant être en mesure de suivre les liens et de naviguer en arrière dans l’historique de la page :

[![Captures d’écran du bouton précédent dans l’action](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Des parties de cette page sont des modifications en fonction de travail créés et partagés par projet Android Open Source utilisés en fonction des conditions décrites dans le*
[*Creative Commons 2.5 Attribution de licence* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Liens associés

- [Appel c# à partir de JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
