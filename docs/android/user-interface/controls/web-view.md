---
title: Affichage Web
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8d7b0e1abc8eb11bf812a111764b9cccfb41e041
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241173"
---
# <a name="web-view"></a>Affichage Web

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) vous permet de créer votre propre fenêtre d’affichage des pages web (ou même développer un navigateur terminé). Dans ce didacticiel, vous allez créer une simple [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) qui peut afficher et parcourir des pages web.

Créez un projet nommé **HelloWebView**.

Ouvrez **Resources/Layout/Main.axml** et insérez le code suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Étant donné que cette application accède à Internet, vous devez ajouter le que fichier de manifeste les autorisations appropriées pour Android. Ouvrez les propriétés de votre projet pour spécifier les autorisations nécessaires au fonctionnement de votre application. Activer la `INTERNET` autorisation comme indiqué ci-dessous :

![En définissant l’autorisation d’INTERNET dans le manifeste Android](web-view-images/01-set-internet-permissions.png)

Ouvrez maintenant **MainActivity.cs** et ajoutez une directive pour Webkit :

```csharp
using Android.Webkit;
```

En haut de la `MainActivity` classe, déclarez un [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) objet :

```csharp
WebView web_view;
```

Lorsque le **WebView** est invité à charger une URL, il sera par défaut déléguer la demande pour le navigateur par défaut. Pour que le **WebView** charger l’URL (plutôt que le navigateur par défaut), vous devez sous-classe `Android.Webkit.WebViewClient` et remplacer le `ShouldOverriderUrlLoading` (méthode). Une instance de cette valeur personnalisée `WebViewClient` est fourni pour le `WebView`. Pour ce faire, ajoutez imbriqués de ce qui suit `HelloWebViewClient` classe à l’intérieur de `MainActivity`:

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

Si vous ciblez le niveau d’API 24 ou version ultérieur, utilisez la surcharge de `ShouldOverrideUrlLoading` qui accepte un `IWebResourceRequest` pour le deuxième argument à la place d’un `string`:

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

Ce code initialise le membre [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) par celui de la [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) mise en page et permet de JavaScript pour le [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) avec [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (voir la [C appeler\# à partir de JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) Recipe (Recette) pour plus d’informations sur l’appel de C\# fonctions à partir de JavaScript). Enfin, une page web initiale est chargée avec [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Générez et exécutez l’application. Vous devez voir une application de visionneuse de page web simple en tant que celui indiqué dans la capture d’écran suivante :

[![Exemple d’application affichant une WebView](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Pour gérer les **retour** bouton appui sur une touche, ajoutez le code suivant à l’aide d’instruction :

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

Cela [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) méthode de rappel sera appelée chaque fois qu’un bouton est enfoncé alors que l’activité est en cours d’exécution. La condition à l’intérieur utilise le [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) pour vérifier si la touche enfoncée est la **retour** bouton et si le [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) est en mesure de navigation (si elle a un historique). Si les deux sont true, puis le [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) méthode est appelée, ce qui permet d’accéder étape précédent dans le [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) historique. Retour `true` indique que l’événement a été géré. Si cette condition n’est pas remplie, l’événement est envoyé au système.

Exécutez de nouveau l'application. Vous devez maintenant être en mesure de suivre les liens et reviennent en arrière dans l’historique de la page :

[![Captures d’écran du bouton retour en action](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Certaines parties de cette page sont des modifications basées sur le travail créé et partagé par Android Open Source Project et utilisé conformément aux conditions décrites dans le*
[*licence Creative Commons 2.5 Attribution* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Liens associés

- [Appel de c# à partir de JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
