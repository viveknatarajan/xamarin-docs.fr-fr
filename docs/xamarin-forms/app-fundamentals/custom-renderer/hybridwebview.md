---
title: Implémentation d’un HybridWebView
description: Cet article montre comment créer un convertisseur personnalisé pour un contrôle personnalisé HybridWebView, qui montre comment améliorer les contrôles spécifiques à la plateforme web pour autoriser le code c# à appeler à partir de JavaScript.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: f0b21277b91c44edbb574aece92664de2e49a65a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996358"
---
# <a name="implementing-a-hybridwebview"></a>Implémentation d’un HybridWebView

_Contrôles d’interface utilisateur personnalisée Xamarin.Forms doivent dériver de la classe de vue, qui est utilisée pour placer des dispositions et des contrôles sur l’écran. Cet article montre comment créer un convertisseur personnalisé pour un contrôle personnalisé HybridWebView, qui montre comment améliorer les contrôles spécifiques à la plateforme web pour autoriser le code c# à appeler à partir de JavaScript._

Chaque vue Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Quand un [ `View` ](xref:Xamarin.Forms.View) est restitué par une application Xamarin.Forms dans iOS, le `ViewRenderer` classe est instanciée, ce qui instancie à son tour native `UIView` contrôle. Sur la plateforme Android, le `ViewRenderer` classe instancie un `View` contrôle. Sur la plateforme de Windows universelle (UWP), le `ViewRenderer` classe instancie native `FrameworkElement` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `View` ](xref:Xamarin.Forms.View) et les contrôles natifs correspondants qui l’implémentent :

![](hybridwebview-images/view-classes.png "Relation entre la classe d’affichage et son implémentation des Classes natives")

Le processus de rendu peut être utilisé pour implémenter les personnalisations spécifiques à la plateforme en créant un convertisseur personnalisé pour un [ `View` ](xref:Xamarin.Forms.View) sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_HybridWebView) le `HybridWebView` contrôle personnalisé.
1. [Consommer](#Consuming_the_HybridWebView) le `HybridWebView`à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour le `HybridWebView` sur chaque plateforme.

Chaque élément maintenant nous aborderons tour pour implémenter un `HybridWebView` convertisseur qui améliore les contrôles spécifiques à la plateforme web pour autoriser le code c# à appeler à partir de JavaScript. Le `HybridWebView` instance doit être utilisée pour afficher une page HTML qui invite l’utilisateur à entrer leur nom. Ensuite, lorsque l’utilisateur clique sur un bouton HTML, une fonction JavaScript appelle C# `Action` qui affiche une fenêtre contextuelle contenant le nom des utilisateurs.

Pour plus d’informations sur le processus appelant en c# à partir de JavaScript, consultez [appel c# à partir de JavaScript](#Invoking_C_from_JavaScript). Pour plus d’informations sur la page HTML, consultez [création de la Page Web](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Création de la HybridWebView

Le `HybridWebView` contrôle personnalisé peut être créé en sous-classant la [ `View` ](xref:Xamarin.Forms.View) classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

Le `HybridWebView` contrôle personnalisé est créé dans le projet de bibliothèque .NET Standard et définit l’API suivante pour le contrôle :

- Un `Uri` propriété qui spécifie l’adresse de la page web à charger.
- Un `RegisterAction` méthode inscrit une `Action` avec le contrôle. L’action inscrite doit être appelée à partir de JavaScript contenu dans le fichier HTML référencé par le biais du `Uri` propriété.
- Un `CleanUp` méthode qui supprime la référence au nom `Action`.
- Un `InvokeAction` méthode qui appelle inscrit `Action`. Cette méthode est appelée à partir d’un convertisseur personnalisé dans chaque projet spécifique à la plateforme.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Utilisation de la HybridWebView

Le `HybridWebView` contrôle personnalisé qui peut être référencé dans le XAML dans le projet de bibliothèque .NET Standard en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur le contrôle personnalisé. Le code suivant montre l’exemple comment la `HybridWebView` contrôle personnalisé peut être consommé par une page XAML :

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour référencer le contrôle personnalisé.

Le code suivant montre l’exemple comment la `HybridWebView` contrôle personnalisé peut être consommé par une page c# :

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

Le `HybridWebView` instance sera utilisée pour afficher un contrôle web natifs sur chaque plateforme. Il a `Uri` propriété est définie dans un fichier HTML qui est stocké dans chaque projet spécifique à la plateforme, et qui sera affiché par le contrôle web natifs. Le code HTML restitué invite l’utilisateur à entrer son nom, avec une fonction JavaScript appelant C# `Action` en réponse à un clic de bouton HTML.

Le `HybridWebViewPage` enregistre l’action à appeler à partir de JavaScript, comme indiqué dans l’exemple de code suivant :

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

Cette action appelle le [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) méthode pour afficher une fenêtre contextuelle modale qui présente le nom entré dans la page HTML affichée par le `HybridWebView` instance.

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour améliorer les contrôles spécifiques à la plateforme web en autorisant le code c# pour être appelée à partir de JavaScript.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `ViewRenderer<T1,T2>` classe qui restitue le contrôle personnalisé. Le premier argument de type doit être le contrôle personnalisé, le convertisseur est dans ce cas, `HybridWebView`. Le deuxième argument de type doit être le contrôle natif qui implémentera la vue personnalisée.
1. Remplacer le `OnElementChanged` méthode qui restitue le contrôle et écriture une logique personnalisée pour le personnaliser. Cette méthode est appelée lorsque le contrôle personnalisé Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour indiquer qu’il est utilisée pour restituer le contrôle personnalisé Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Pour la plupart des éléments de Xamarin.Forms, il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, le convertisseur par défaut pour la classe de base du contrôle sera être utilisé. Toutefois, les convertisseurs personnalisés sont nécessaires dans chaque projet de plateforme lors du rendu d’un [vue](xref:Xamarin.Forms.View) élément.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](hybridwebview-images/solution-structure.png "Responsabilités de projet de convertisseur HybridWebView personnalisé")

Le `HybridWebView` contrôle personnalisé est affiché par les classes de renderer spécifique à la plateforme, qui dérivent toutes de la `ViewRenderer` classe pour chaque plateforme. Ainsi, chaque `HybridWebView` contrôle personnalisé rendu avec des contrôles spécifiques à la plateforme web, comme indiqué dans les captures d’écran suivante :

![](hybridwebview-images/screenshots.png "HybridWebView sur chaque plateforme")

Le `ViewRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque le contrôle personnalisé Xamarin.Forms est créé pour restituer le contrôle web natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms qui le convertisseur *a été* associée et l’élément Xamarin.Forms qui le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contiendra une référence à la `HybridWebView` instance.

Une version substituée de la `OnElementChanged` (méthode), dans chaque classe de renderer spécifique à la plateforme, est l’endroit où effectuer l’instanciation des contrôles web natifs et la personnalisation. Le `SetNativeControl` méthode doit être utilisée pour instancier le contrôle web natifs, et cette méthode attribue également la référence de contrôle à la `Control` propriété. En outre, une référence au contrôle Xamarin.Forms qui est rendu peut être obtenue via la `Element` propriété.

Dans certains cas le `OnElementChanged` méthode peut être appelée plusieurs fois. Par conséquent, pour éviter les fuites de mémoire, être vigilant lors de l’instanciation d’un nouveau contrôle natif. L’approche à utiliser lors de l’instanciation d’un nouveau contrôle natif dans un renderer personnalisé est indiquée dans l’exemple de code suivant :

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Un nouveau contrôle natif doit uniquement être instancié une seule fois, quand la propriété `Control` a la valeur `null`. Vous devez uniquement configurer le contrôle et vous abonner aux gestionnaires d’événements quand le renderer personnalisé est attaché à un nouvel élément Xamarin.Forms. De même, vous devez vous désabonner des gestionnaires d’événements auxquels vous vous êtes abonné uniquement quand l’élément auquel le renderer est attaché change. Adopter cette approche vous aidera à créer un convertisseur personnalisé performante qui ne connaît pas les fuites de mémoire.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle personnalisé Xamarin.Forms en cours d’affichage et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes décrivent la structure de la page web chargée par chaque contrôle web natifs, le processus appelant en c# à partir de JavaScript et l’implémentation de cela dans chaque classe de convertisseur personnalisé spécifique à la plateforme.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Création de la Page Web

L’exemple de code suivant montre la page web qui sera affichée par le `HybridWebView` contrôle personnalisé :

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
          log(err);
    }
}
</script>
</body>
</html>
```

La page web permet à un utilisateur d’entrer leur nom dans un `input` élément et fournit un `button` élément qui appelle le code c# lorsque vous cliquez sur. Le processus d’y parvenir est la suivante :

- Lorsque l’utilisateur clique sur le `button` élément, le `invokeCSCode` fonction JavaScript est appelée, avec la valeur de la `input` élément passé à la fonction.
- Le `invokeCSCode` appels de fonction le `log` (fonction) pour afficher les données qu’il envoie à celle de C# `Action`. Il appelle ensuite la `invokeCSharpAction` méthode à appeler le C# `Action`, en transmettant le paramètre reçu à partir de la `input` élément.

Le `invokeCSharpAction` fonction JavaScript n’est pas définie dans la page web et seront injectée dans celle-ci par chaque convertisseur personnalisé.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Appel de c# à partir de JavaScript

Le processus appelant en c# à partir de JavaScript est identique sur chaque plateforme :

- Le convertisseur personnalisé crée un contrôle web natifs et charge le fichier HTML spécifié par le `HybridWebView.Uri` propriété.
- Une fois la page web est chargé, le convertisseur personnalisé injecte le `invokeCSharpAction` fonction JavaScript dans la page web.
- Lorsque l’utilisateur entre son nom et clique sur le code HTML `button` élément, le `invokeCSCode` fonction est appelée, qui appelle à son tour le `invokeCSharpAction` (fonction).
- Le `invokeCSharpAction` fonction appelle une méthode dans le convertisseur personnalisé, qui à son tour appelle la `HybridWebView.InvokeAction` (méthode).
- Le `HybridWebView.InvokeAction` méthode appelle inscrit `Action`.

Les sections suivantes explique comment ce processus est implémenté sur chaque plateforme.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme iOS :

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

Le `HybridWebViewRenderer` classe charge la page web spécifiée dans le `HybridWebView.Uri` propriété un code natif [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) contrôle et le `invokeCSharpAction` fonction JavaScript est injectée dans la page web. Une fois que l’utilisateur entre son nom et clique sur le code HTML `button` élément, le `invokeCSharpAction` fonction JavaScript est exécutée, avec la `DidReceiveScriptMessage` méthode appelée après la réception d’un message à partir de la page web. À son tour, cette méthode appelle la `HybridWebView.InvokeAction` (méthode), qui appelle l’action inscrite pour afficher la fenêtre contextuelle.

Cette fonctionnalité s’effectue comme suit :

- Condition que le `Control` propriété est `null`, les opérations suivantes sont effectuées :
  - Un [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance est créée, ce qui permet de publier des messages et l’injection de scripts de l’utilisateur dans une page web.
  - Un [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) instance est créée pour injecter le `invokeCSharpAction` fonction JavaScript dans la page web une fois que la page web est chargée.
  - Le [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) méthode ajoute la [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) instance au contrôleur de contenu.
  - Le [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) méthode ajoute un gestionnaire de messages de script nommé `invokeAction` à la [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance, ce qui provoque la fonction JavaScript `window.webkit.messageHandlers.invokeAction.postMessage(data)` être définis dans toutes les frames dans toutes les vues web qui utiliseront le `WKUserContentController` instance.
  - Un [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) instance est créée, avec le [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) instance définie en tant que le contrôleur de contenu.
  - Un [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) contrôle est instancié et le `SetNativeControl` méthode est appelée pour affecter une référence à la `WKWebView` le contrôle à la `Control` propriété.
- Si le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :
  - Le [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) méthode charge le fichier HTML qui est spécifié par le `HybridWebView.Uri` propriété. Le code spécifie que le fichier est stocké dans le `Content` dossier du projet. Une fois que la page web s’affiche, le `invokeCSharpAction` fonction JavaScript est injectée dans la page web.
- Lorsque l’élément auquel le renderer est attaché change :
  - Les ressources sont libérées.

> [!NOTE]
> Le `WKWebView` classe est uniquement pris en charge dans iOS 8 et versions ultérieures.

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Android :

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

Le `HybridWebViewRenderer` classe charge la page web spécifiée dans le `HybridWebView.Uri` propriété un code natif [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) contrôle et le `invokeCSharpAction` fonction JavaScript est injectée dans la page web, une fois que la page web chargée, avec le `InjectJS` (méthode). Une fois que l’utilisateur entre son nom et clique sur le code HTML `button` élément, le `invokeCSharpAction` fonction JavaScript est exécutée. Cette fonctionnalité s’effectue comme suit :

- Condition que le `Control` propriété est `null`, les opérations suivantes sont effectuées :
  - Native [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) instance est créée et JavaScript est activé dans le contrôle.
  - Le `SetNativeControl` méthode est appelée pour affecter une référence à natif [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) le contrôle à la `Control` propriété.
- Si le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :
  - Le [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) méthode injecte un nouveau `JSBridge` instance dans le cadre de principal du contexte de JavaScript de l’affichage Web, nommez-la `jsBridge`. Ainsi, les méthodes dans la `JSBridge` classe accessible à partir de JavaScript.
  - Le [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) méthode charge le fichier HTML qui est spécifié par le `HybridWebView.Uri` propriété. Le code spécifie que le fichier est stocké dans le `Content` dossier du projet.
  - Le `InjectJS` méthode est appelée pour injecter le `invokeCSharpAction` fonction JavaScript dans la page web.
- Lorsque l’élément auquel le renderer est attaché change :
  - Les ressources sont libérées.

Lorsque le `invokeCSharpAction` fonction JavaScript est exécutée, elle appelle à son tour le `JSBridge.InvokeAction` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

La classe doit dériver de `Java.Lang.Object`, et les méthodes qui sont exposées à JavaScript doivent être décorées avec le `[JavascriptInterface]` et `[Export]` attributs. Par conséquent, lorsque le `invokeCSharpAction` fonction JavaScript est injectée dans la page web et est exécutée, il appellera le `JSBridge.InvokeAction` méthode en raison d’être décorée avec le `[JavascriptInterface]` et `[Export("invokeAction")]` attributs. À son tour, le `InvokeAction` méthode appelle le `HybridWebView.InvokeAction` méthode, qui vous appelé l’action inscrite pour afficher la fenêtre contextuelle.

> [!NOTE]
> Des projets qui utilisent le `[Export]` attribut doit inclure une référence à `Mono.Android.Export`, ou une erreur du compilateur se produit.

Notez que le `JSBridge` classe maintient une `WeakReference` à la `HybridWebViewRenderer` classe. Il s’agit d’éviter de créer une référence circulaire entre les deux classes. Pour plus d’informations, consultez [références faibles](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) sur MSDN.

> [!IMPORTANT]
> Sur Android Oreo, assurez-vous que le manifeste Android définit le **version Android cible** à **automatique**. Sinon, exécution de ce code génère l’erreur message « invokeCSharpAction n'est pas définie ».

### <a name="creating-the-custom-renderer-on-uwp"></a>Création du convertisseur personnalisé sur UWP

L’exemple de code suivant montre le convertisseur personnalisé pour UWP :

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

Le `HybridWebViewRenderer` classe charge la page web spécifiée dans le `HybridWebView.Uri` propriété un code natif `WebView` contrôle et le `invokeCSharpAction` fonction JavaScript est injectée dans la page web, une fois que la page web chargée, avec la `WebView.InvokeScriptAsync` (méthode). Une fois que l’utilisateur entre son nom et clique sur le code HTML `button` élément, le `invokeCSharpAction` fonction JavaScript est exécutée, avec la `OnWebViewScriptNotify` méthode appelée après une notification est reçue à partir de la page web. À son tour, cette méthode appelle la `HybridWebView.InvokeAction` (méthode), qui appelle l’action inscrite pour afficher la fenêtre contextuelle.

Cette fonctionnalité s’effectue comme suit :

- Condition que le `Control` propriété est `null`, les opérations suivantes sont effectuées :
  - Le `SetNativeControl` méthode est appelée pour instancier un nouveau natif `WebView` contrôler et d’affecter une référence à celui-ci par le `Control` propriété.
- Si le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms :
  - Gestionnaires d’événements pour le `NavigationCompleted` et `ScriptNotify` événements sont inscrits. Le `NavigationCompleted` événement est déclenché quand le natif `WebView` contrôle a terminé le chargement du contenu actuel ou si la navigation a échoué. Le `ScriptNotify` événement est déclenché lorsque le contenu de natif `WebView` contrôle utilise JavaScript pour passer une chaîne à l’application. La page web déclenche le `ScriptNotify` événement en appelant `window.external.notify` durant le passage un `string` paramètre.
  - Le `WebView.Source` propriété est définie sur l’URI du fichier HTML qui est spécifié par le `HybridWebView.Uri` propriété. Le code suppose que le fichier est stocké dans le `Content` dossier du projet. Une fois que la page web s’affiche, le `NavigationCompleted` événement est déclenché et le `OnWebViewNavigationCompleted` méthode sera appelée. Le `invokeCSharpAction` fonction JavaScript sera ensuite être injectée dans la page web avec le `WebView.InvokeScriptAsync` (méthode), à condition que le volet de navigation s’est terminée correctement.
- Lorsque l’élément auquel le renderer est attaché change :
  - Événements êtes désabonnés.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur personnalisé pour un `HybridWebView` contrôle personnalisé, qui montre comment améliorer les contrôles spécifiques à la plateforme web pour autoriser le code c# à appeler à partir de JavaScript.


## <a name="related-links"></a>Liens associés

- [CustomRendererHybridWebView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [Appel de c# à partir de JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
