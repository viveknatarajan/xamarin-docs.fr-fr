---
title: Xamarin.Forms WebView
description: Cet article explique comment utiliser la classe Xamarin.Forms WebView pour présenter local ou de contenu web de réseau et de documents pour les utilisateurs.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: df004bd2a580e48137162d28ca3974521266ae7a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245642"
---
# <a name="xamarinforms-webview"></a>Xamarin.Forms WebView

[WebView](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) n’est pas un affichage pour l’affichage web et du code HTML contenu dans votre application. Contrairement aux `OpenUri`, qui dirige l’utilisateur vers le navigateur web sur l’appareil, `WebView` affiche le contenu HTML à l’intérieur de votre application.

Ce guide est composé des sections suivantes :

- **[Contenu](#Content)**  &ndash; WebView prend en charge de diverses sources de contenu, y compris HTML incorporé, les pages web et les chaînes HTML.
- **[Navigation](#Navigation)**  &ndash; WebView inclut la prise en charge pour la navigation vers une page particulière et revenir.
- **[Événements](#Events)**  &ndash; écouter et répondre aux actions effectuées par l’utilisateur dans l’affichage Web.
- **[Performances](#Performance)**  &ndash; en savoir plus sur les caractéristiques de performances du WebView sur chaque plateforme.
- **[Autorisations](#Permissions)**  &ndash; apprendre à définir des autorisations afin que WebView fonctionne dans votre application.
- **[Disposition](#Layout)**  &ndash; WebView a des exigences de matière très particuliers de la façon dont il est disposé. Découvrez comment garantir que WebView affiche correctement :

![](webview-images/in-app-browser.png "Dans le navigateur de l’application")

## <a name="content"></a>Contenu

WebView est fourni avec prise en charge pour les types de contenu suivants :

- Sites Web HTML et CSS &ndash; WebView a une prise en charge complète pour les sites Web écrites à l’aide de HTML et CSS, y compris la prise en charge JavaScript.
- Documents &ndash; comme WebView est implémenté à l’aide des composants natifs sur chaque plateforme, WebView est capable de l’affichage des documents qui sont visibles sur chaque plateforme. Cela signifie que les fichiers PDF fonctionnent sur iOS et Android.
- Chaînes HTML &ndash; WebView peut afficher des chaînes HTML de la mémoire.
- Fichiers locaux &ndash; WebView peut présenter un des types de contenu ci-dessus incorporé dans l’application.

> [!NOTE]
> `WebView` sur Windows ne prend pas en charge Silverlight, Flash ou tous les contrôles ActiveX, même si elles sont prises en charge par Internet Explorer sur cette plateforme.

### <a name="websites"></a>Sites Web

Pour afficher un site Web à partir d’internet, définissez la `WebView`de [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) propriété vers une URL de la chaîne :

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL doivent être formées avec le protocole spécifié (autrement dit, il doit être « http:// » ou « https:// » au début de liste).

#### <a name="ios-and-ats"></a>iOS et les couches d’application

Depuis la version 9, iOS vous permet uniquement de votre application de communiquer avec les serveurs qui implémentent les meilleures pratiques sécurité par défaut. Valeurs doivent être définies `Info.plist` pour permettre la communication avec des serveurs non sécurisés.

> [!NOTE]
> Si votre application nécessite une connexion à un site Web non sécurisé, vous devez toujours entrer le domaine comme une exception à l’aide `NSExceptionDomains` au lieu de la désactivation ATS complètement à l’aide de `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` doit uniquement être utilisé dans des situations extrêmes d’urgence.

La commande suivante montre comment activer un domaine spécifique (dans ce cas xamarin.com) pour ignorer les exigences de couches d’application :

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
```

Il est recommandé d’activer uniquement certains domaines de contourner les couches d’application, ce qui vous permet d’utiliser des sites de confiance tout en bénéficiant de la sécurité supplémentaire sur des domaines non approuvés. L’exemple suivant présente la méthode la moins sécurisée de la désactivation des couches d’application pour l’application :

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md) pour plus d’informations sur cette nouvelle fonctionnalité dans iOS 9.

### <a name="html-strings"></a>Chaînes HTML

Si vous souhaitez présenter une chaîne de code HTML défini dynamiquement dans le code, vous devez créer une instance de [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Chaîne au format HTML affichage WebView")

Dans le code ci-dessus, `@` est utilisé pour marquer le code HTML sous forme de chaîne littérale, ce qui signifie que tous les caractères d’échappement habituels sont ignorés.

### <a name="local-html-content"></a>Contenu HTML local

WebView peut afficher du contenu à partir de HTML, CSS et Javascript incorporé au sein de l’application. Exemple :

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS :

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Notez que les polices spécifiées dans le code CSS ci-dessus seront doivent être personnalisées pour chaque plate-forme, comme pas chaque plateforme possède les mêmes polices.

À l’affichage local contenu à l’aide un `WebView`, vous devez ouvrir le fichier HTML comme tout autre, puis de charger le contenu sous forme de chaîne dans le `Html` propriété d’un `HtmlWebViewSource`. Pour plus d’informations sur l’ouverture de fichiers, consultez [utilisation des fichiers](~/xamarin-forms/app-fundamentals/files.md).

Les captures d’écran suivantes affichent le résultat de l’affichage du contenu local sur chaque plateforme :

![](webview-images/local-content.png "Affichage du contenu Local WebView")

Bien que la première page a été chargée, le `WebView` n’a aucune connaissance de d'où provenance le code HTML. C’est un problème lors du traitement de pages qui font référence à des ressources locales. Lorsque le lien pages locales uns des autres, une page permet de recourir à un fichier JavaScript distinct, ou une page est liée à une feuille de style CSS constituent des exemples de lorsque cela peut se produire.  

Pour résoudre ce problème, vous devez indiquer le `WebView` où trouver les fichiers sur le système de fichiers. Cela en définissant le `BaseUrl` propriété sur le `HtmlWebViewSource` utilisé par le `WebView`.

Étant donné que le système de fichiers sur chacun des systèmes d’exploitation est différent, vous devez déterminer cette URL sur chaque plateforme. Xamarin.Forms expose le `DependencyService` pour la résolution des dépendances à l’exécution sur chaque plateforme.

Pour utiliser le `DependencyService`, tout d’abord définir une interface qui peut être implémentée sur chaque plateforme :

```csharp
public interface IBaseUrl { string Get(); }
```

Notez que jusqu'à ce que l’interface est implémentée sur chaque plateforme, l’application s’exécutera pas. Dans le projet common, assurez-vous que vous n’oubliez pas de définir la `BaseUrl` à l’aide de la `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Les implémentations de l’interface pour chaque plate-forme doivent alors être fournies.

#### <a name="ios"></a>iOS

Sur iOS, le contenu web doit se trouver dans le répertoire du projet racine ou **ressources** répertoire avec l’action de génération *BundleResource*, comme illustré ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Fichiers locaux sur iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Fichiers locaux sur iOS")

-----

Le `BaseUrl` doit être défini sur le chemin d’accès du fichier principal :

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

Sur Android, placez le HTML, CSS et des images dans le dossier de ressources avec l’action de génération *AndroidAsset* comme illustré ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Fichiers locaux sur Android")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](webview-images/android-xs.png "Fichiers locaux sur Android")

-----

Sur Android, le `BaseUrl` doit être défini sur `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

Sur Android, les fichiers dans le **actifs** dossier est également accessibles via le contexte actuel Android, ce qui est exposé par le `MainActivity.Instance` propriété :

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Sur les projets de plateforme Windows universelle (UWP), placez HTML, CSS et des images dans la racine du projet avec l’action de build définie sur *contenu*.

Le `BaseUrl` doit être défini sur `"ms-appx-web:///"`:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Navigation

Affichage Web prend en charge la navigation via plusieurs méthodes et propriétés disponibles :

- **GoForward()** &ndash; si `CanGoForward` a la valeur true, l’appel de `GoForward` accède à la page visitée suivante.
- **GoBack()** &ndash; si `CanGoBack` a la valeur true, l’appel de `GoBack` permet d’accéder à la dernière page visitée.
- **CanGoBack** &ndash; `true` s’il existe des pages pour revenir au `false` si le navigateur est à l’adresse de départ.
- **CanGoForward** &ndash; `true` si l’utilisateur a navigué vers l’arrière et peut accéder à une page qui a été déjà visitée.

Dans les pages, `WebView` ne prend pas en charge les entrées tactiles multipoints. Il est important pour vous assurer que le contenu est optimisé pour les mobiles et s’affiche sans nécessiter de zoom.

Il est courant pour les applications afficher un lien dans un `WebView`, plutôt que le navigateur du périphérique. Dans ces situations, il est utile permettre la navigation normale, mais lors de l’accès utilisateur lorsqu’ils sont sur le lien de départ, l’application doit retourner à la vue de l’application normal.

Pour activer ce scénario, utilisez les méthodes de navigation intégrés et les propriétés.

Commencez par créer la page de l’affichage du navigateur :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Dans notre code-behind :

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

C’est tout !

![](webview-images/in-app-browser.png "Boutons de Navigation WebView")

## <a name="events"></a>Événements

WebView déclenche deux événements pour vous aider à répondre aux modifications d’état :

- **Navigation** &ndash; événement déclenché lorsque la WebView commence à se charger une nouvelle page.
- **Navigation** &ndash; événement déclenché lorsque la page est chargée et la navigation s’est arrêté.

Si vous prévoyez d’utiliser les pages Web qui prennent beaucoup de temps à charger, envisagez d’utiliser ces événements pour implémenter un indicateur d’état. Par exemple, le code XAML ressemble à ceci :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
  <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

Les deux gestionnaires d’événements :

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

Ainsi, la sortie suivante (chargement) :

![](webview-images/loading-start.png "Exemple d’événement de navigation WebView")

Chargement terminé :

![](webview-images/loading-end.png "Exemple d’événements de navigation WebView")

## <a name="performance"></a>Performances

Avancées récentes ont chacun des navigateurs web les plus courants vu à adopter les technologies telles que l’accélération matérielle rendu et la compilation JavaScript. Malheureusement, en raison de restrictions de sécurité, la plupart de ces améliorations n’étaient pas disponible dans le l’iOS-equaivalent de `WebView`, `UIWebView`. Xamarin.Forms `WebView` utilise `UIWebView`. Si c’est un problème, vous devez écrire un convertisseur personnalisé qui utilise `WKWebView`, qui prend en charge la navigation plus rapide. Notez que `WKWebView` est uniquement pris en charge sur iOS 8 et les versions ultérieures.

WebView sur Android par défaut est presque aussi rapide que le navigateur intégré.

Le [UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) utilise le moteur de rendu Microsoft Edge. Les appareils de bureau et des tablettes devraient normalement voir les mêmes performances à l’aide du navigateur Edge lui-même.

## <a name="permissions"></a>Autorisations

Dans l’ordre de `WebView` , de travail, vous devez vous assurer que les autorisations sont définies pour chaque plateforme. Notez que sur certaines plates-formes, `WebView` fonctionne en mode débogage, mais ne pas lors de la génération de version. C’est parce que certaines autorisations, telles que celles pour l’accès internet sur Android, sont définies par défaut par Visual Studio pour Mac, en mode débogage.

- **UWP** &ndash; nécessite la fonctionnalité Internet (Client & serveur) lors de l’affichage du contenu du réseau.
- **Android** &ndash; requiert `INTERNET` uniquement lorsque l’affichage du contenu à partir du réseau. Le contenu local nécessite des autorisations spéciales.
- **iOS** &ndash; requiert des autorisations spéciales.

## <a name="layout"></a>Mise en page

Contrairement à la plupart des autres vues de Xamarin.Forms, `WebView` requiert que `HeightRequest` et `WidthRequest` sont spécifiés lorsque contenue dans StackLayout ou RelativeLayout. Si vous ne pouvez pas spécifier ces propriétés, les `WebView` ne sont pas rendus.

Les exemples suivants illustrent des dispositions qui résultent de travail, rendu `WebView`%s :

StackLayout avec WidthRequest & HeightRequest :

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout avec WidthRequest & HeightRequest :

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

DispositionAbsolue *sans* WidthRequest & HeightRequest :

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Grille *sans* WidthRequest & HeightRequest. Grille est une des quelques dispositions qui ne requiert pas de spécification demandées hauteurs et largeurs. :

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```


## <a name="related-links"></a>Liens associés

- [Utilisation de WebView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [WebView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
