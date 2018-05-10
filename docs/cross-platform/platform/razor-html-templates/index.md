---
title: Vues HTML de génération à l’aide de modèles Razor
description: " À l’aide d’une page Web plein écran pour le rendu HTML peut être un moyen simple et efficace pour restituer la mise en forme complexe d’une manière inter-plateformes, surtout si vous disposez déjà HTML, Javascript et CSS à partir d’un projet de site Web."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 48d7778bf3225401f2819909ae6be320cfa881e3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="building-html-views-using-razor-templates"></a>Vues HTML de génération à l’aide de modèles Razor

Dans l’environnement de développement pour appareils mobiles la terme « application hybride » fait généralement référence à une application qui présente certaines de ses écrans (ou toutes) comme des pages HTML à l’intérieur d’un contrôle de visionneuse web hébergé.

Il existe certains environnements de développement qui vous permettent de générer votre application mobile entièrement en HTML et Javascript, à ces applications peuvent subir des problèmes de performance lorsque vous tentez d’effectuer un traitement complexe ou les effets d’interface utilisateur et sont également limitées dans la plateforme fonctionnalités qu’ils peuvent accéder.

Xamarin propose le meilleur des deux mondes, en particulier lors de l’utilisation du moteur de création de modèles Razor HTML. Avec Xamarin, vous avez la possibilité pour générer des vues HTML basé sur un modèle inter-plateformes utilisent Javascript et CSS, mais également ont un accès complet à l’API de la plateforme sous-jacente et le traitement rapide à l’aide de c#.

Ce document explique comment utiliser des vues HTML + Javascript + CSS qui peuvent être utilisés sur des plates-formes mobiles à l’aide de Xamarin de version du moteur de création de modèles Razor.

## <a name="using-web-views-programmatically"></a>À l’aide de vues Web par programme

Avant que nous découvrons Razor cette section couvre l’utilisation des vues web pour afficher le contenu HTML directement – plus précisément le contenu HTML est généré au sein d’une application.

Xamarin fournit un accès complet à l’API de la plateforme sous-jacente sur iOS et Android, il est facile de créer et afficher le code HTML à l’aide de c#. Vous trouverez ci-dessous la syntaxe de base pour chaque plateforme.

### <a name="ios"></a>iOS

Affichage HTML dans un contrôle UIWebView dans Xamarin.iOS prend également quelques lignes de code :

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Consultez le [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) recettes pour plus d’informations sur l’utilisation du contrôle UIWebView.

### <a name="android"></a>Android

Affichage HTML dans un contrôle WebView à l’aide de Xamarin.Android s’effectue en quelques lignes de code :

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Consultez le [WebView Android](http://docs.xamarin.com/recipes/android/controls/webview/) recettes pour plus d’informations sur l’utilisation du contrôle WebView.

### <a name="specifying-the-base-directory"></a>Spécification du répertoire de Base

Sur les deux plateformes, il existe un paramètre qui spécifie le répertoire de base pour la page HTML. Il s’agit de l’emplacement sur le système de fichiers du périphérique qui est utilisé pour résoudre des références relatives aux ressources telles que des images et des fichiers CSS. Par exemple, comme les balises

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

faire référence à ces fichiers : **style.css**, **monkey.jpg** et **jscript.js**. Le paramètre de répertoire de base indique à l’affichage web où ces fichiers se trouvent afin qu’ils peuvent être chargés dans la page.

#### <a name="ios"></a>iOS

La sortie de modèle est rendue dans iOS avec le code c# suivant :

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Le répertoire de base est spécifié en tant que `NSBundle.MainBundle.BundleUrl` qui fait référence à l’application est installée dans le répertoire. Tous les fichiers dans le **ressources** dossier sont copiés vers cet emplacement, tel que le **style.css** illustré ici :

 ![solution d’iPhoneHybrid](images/image1_240x163.png)

L’Action de génération pour tous les fichiers de contenu statiques doit être **BundleResource**:

 ![action de génération de projet iOS : BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android requiert également un répertoire de base à passer lorsque les chaînes html sont affichés dans un affichage web en tant que paramètre.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

La chaîne spéciale **file:///android_asset/** fait référence au dossier Android actifs dans votre application, affichée ici contenant le **style.css** fichier.

 ![Solution de AndroidHybrid](images/image3_240x167.png)

L’Action de génération pour tous les fichiers de contenu statiques doit être **AndroidAsset**.

 ![Action de génération de projet Android : AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Appel de c# à partir de HTML et Javascript

Lorsqu’une page html est chargée dans un affichage web, il traite les liens et les formulaires comme c’est le cas si la page a été chargée à partir d’un serveur. Cela signifie que si l’utilisateur clique sur un lien ou envoie un formulaire l’affichage web tente de naviguer vers la cible spécifiée.

Si la liaison est à un serveur externe (par exemple, google.com) l’affichage web tente de charger le site Web externe (en supposant qu’il existe une connexion internet).

```html
<a href="http://google.com/">Google</a>
```

Si le lien est relatif l’affichage web tente de charger ce contenu à partir du répertoire de base. Évidemment, aucune connexion réseau n’est requise pour ce faire, comme le contenu est stocké dans l’application sur l’appareil.

```html
<a href="somepage.html">Local content</a>
```

Les actions de formulaire suivent la même règle.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Vous n’allez pas héberger un serveur web sur le client ; Toutefois, vous pouvez utiliser les mêmes techniques de communication de serveur utilisés dans les modèles de conception réactive aujourd'hui permet d’appeler des services via HTTP GET et gèrent les réponses de manière asynchrone en émettant Javascript (ou Javascript appelant déjà hébergée dans l’affichage web). Cela vous permet de passer facilement des données du code HTML dans le code c# pour le traitement, puis afficher que les résultats de la sauvegarde sur la page HTML.

IOS et Android fournissent un mécanisme pour le code d’application pour intercepter ces événements de navigation afin que le code d’application peut répondre (si nécessaire). Cette fonctionnalité est essentielle pour créer des applications hybrides, car elle permet le code natif pour interagir avec l’affichage web.

#### <a name="ios"></a>iOS

L’événement ShouldStartLoad sur l’affichage web dans iOS peut être substituée pour autoriser le code d’application pour traiter une demande de navigation (par exemple, un clic de lien). Les paramètres de méthode fournissent toutes les informations

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

puis d’affecter le Gestionnaire d’événements :

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

Sur Android simplement sous-classe WebViewClient, puis implémentez de code pour répondre à la demande de navigation.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

puis définissez le client sur l’affichage web :

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Appeler Javascript à partir de c#

Outre la possibilité informe un affichage web pour charger une nouvelle page HTML, code c# permettre exécuter Javascript dans la page actuellement affichée. Les blocs de code Javascript entières peuvent être créés à l’aide de chaînes de c# et exécutés, ou vous pouvez créer des appels de méthode Javascript déjà disponible sur la page via `script` balises.

#### <a name="android"></a>Android

Créer le code Javascript pour être exécuté et faites-le précéder « javascript : » et demandez à l’affichage web pour charger cette chaîne :

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

affichages web du iOS fournissent une méthode spécifiquement pour appeler Javascript :

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Récapitulatif

Cette section présente les fonctionnalités des contrôles web vue sur Android et iOS qui nous permettent de développer des applications hybrides avec Xamarin, y compris :

-  La capacité à charger HTML à partir de chaînes générées dans le code,
-  La capacité de référencer des fichiers locaux (CSS, Javascript, Images ou autres fichiers HTML)
-  La possibilité d’intercepter les demandes de navigation dans le code c#,
-  Permet d’appeler Javascript à partir de code c#.


La section suivante présente Razor, ce qui le rend facile de créer le code HTML à utiliser dans des applications hybrides.

## <a name="what-is-razor"></a>Nouveautés Razor

Razor est un moteur de création de modèles qui a été introduit avec ASP.NET MVC, à l’origine pour s’exécuter sur le serveur et générer du code HTML pour être pris en charge dans les navigateurs web.

Le moteur de création de modèles Razor étend la syntaxe HTML standard avec c# afin que vous pouvez exprimer la disposition et intégrer facilement des feuilles de style CSS et Javascript. Le modèle peut faire référence à une classe de modèle, ce qui peut être n’importe quel type personnalisé et dont les propriétés sont accessibles directement à partir du modèle. Un de ses avantages est la possibilité de mélanger la syntaxe HTML et c# facilement.

Modèles Razor ne sont pas limités à utilisation côté serveur, elles peuvent également être incluses dans des applications Xamarin. À l’aide de modèles Razor, ainsi que la possibilité de travailler par programmation avec les vues web permet d’applications sophistiquées inter-plateformes hybrides sont créées avec Xamarin.

### <a name="razor-template-basics"></a>Principes de base de modèle Razor

Fichiers de modèle Razor ont un **.cshtml** extension de fichier. Ils peuvent être ajoutés à un projet Xamarin à partir de la section Création de modèles de texte dans le **nouveau fichier** boîte de dialogue :

 ![Nouveau fichier - modèle Razor](images/image5_400x201.png)

Un modèle Razor simple ( **RazorView.cshtml**) est indiqué ci-dessous.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Notez les différences suivantes à partir d’un fichier HTML normal :

-  Le `@` symbole a une signification spéciale dans les modèles Razor, il indique que l’expression suivante est c# à évaluer.
- `@model` la directive apparaît toujours comme première ligne d’un fichier de modèle Razor.
-  Le `@model` la directive doit être suivie d’un Type. Dans cet exemple, une chaîne simple est transmise au modèle, mais cela peut être n’importe quelle classe personnalisée.
-  Lorsque `@Model` est référencé dans le modèle, il fournit une référence à l’objet passé au modèle lorsqu’il est généré (dans cet exemple, il sera une chaîne).
-  L’IDE génère automatiquement une classe partielle pour les modèles (fichiers avec le **.cshtml** extension). Vous pouvez afficher ce code, mais il ne doit pas être modifié.
 ![RazorView.cshtml](images/image6_125x34.png) la classe partielle est nommée RazorView pour correspondre au nom de fichier .cshtml modèle. Il est de ce nom est utilisé pour faire référence au modèle de code c#.
- `@using` les instructions peuvent également être incluses en haut d’un modèle Razor pour inclure des espaces de noms supplémentaires.


La sortie HTML finale peut être générée par le code c# suivant. Notez que nous spécifions le modèle comme une chaîne « Hello World » qui est incorporée dans la sortie de modèle de rendu.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Voici la sortie affichée dans un affichage web sur iOS Simulator et l’émulateur Android :

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Plus la syntaxe Razor

Dans cette section, que nous allons présenter une syntaxe Razor base pour vous aider à commencer l’utilisez. Les exemples de cette section remplissent la classe suivante avec des données et les affichent à l’aide de Razor :

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Tous les exemples utilisent le code d’initialisation de données suivant

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Affichage des propriétés de modèle

Lorsque le modèle est une classe avec des propriétés, ils sont facilement référencés dans le modèle Razor comme indiqué dans cet exemple de modèle :

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Cela peut être rendu en une chaîne utilisant le code suivant :

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

La sortie finale est illustrée ici dans un affichage web sur iOS Simulator et l’émulateur Android :

 ![RUPERT](images/image8_516x160.png)

#### <a name="c-statements"></a>Instructions c#

C# plus complexes peuvent être inclus dans le modèle, telles que les mises à jour de la propriété modèle et le calcul de la durée de vie dans cet exemple :

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

Vous pouvez écrire une ligne c# des expressions complexes (par exemple, la durée de vie de la mise en forme) en entourant le code avec `@()`.

Plusieurs instructions c# peuvent être écrits en mettant les avec `@{}`.

#### <a name="if-else-statements"></a>Instructions if-else

Branches de code peuvent être exprimées avec `@if` comme indiqué dans cet exemple de modèle.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>Boucles

Bouclage des constructions comme `foreach` peuvent également être ajoutés. Le `@` préfixe peut être utilisé sur la variable de boucle ( `@food` dans ce cas) pour effectuer le rendu en HTML.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

La sortie du modèle ci-dessus s’affiche en cours d’exécution dans iOS Simulator et l’émulateur Android :

 ![Rupert X Monkey](images/image9_520x277.png)

Cette section a couvert les principes fondamentaux de l’utilisation de modèles Razor pour effectuer le rendu des vues en lecture seule simples. La section suivante explique comment créer des applications plus complets, à l’aide de Razor qui peuvent accepter l’entrée d’utilisateur et interagir entre Javascript en mode HTML et c#.

## <a name="using-razor-templates-with-xamarin"></a>À l’aide de modèles Razor avec Xamarin

Cette section explique comment utiliser génération de votre propre application hybride à l’aide des modèles de la solution dans Visual Studio pour Mac. Il existe trois modèles disponibles à partir de la **fichier > Nouveau > Solution...**  fenêtre :

- **Android > applications > Application WebView Android**
- **iOS > applications > Application WebView**
- **Projet ASP.NET MVC**



Le **nouvelle Solution** fenêtre ressemble à ceci pour iPhone et les projets Android - la description de la solution sur la droite met en évidence la prise en charge pour le moteur de création de modèles Razor.

 ![Création d’iPhone et les solutions Android](images/image13_1139x959.png)

Notez que vous pouvez facilement ajouter une **.cshtml** modèle Razor à *tout* existant de projet Xamarin, il n’est pas nécessaire d’utiliser ces modèles de solution. les projets iOS ne nécessitent pas une table de montage séquentiel utiliser Razor. Ajoutez simplement un contrôle UIWebView à n’importe quelle vue par programmation et vous pouvez restituer les modèles Razor entière en code c#.

Le contenu de solution de modèle par défaut pour iPhone et les projets Android figurent ci-dessous :

 ![iPhone et les modèles Android](images/image10_428x310.png)

Les modèles vous permettent d’infrastructure prêt à aller application pour charger un modèle Razor avec un objet de modèle de données, de traiter l’entrée d’utilisateur et de communiquer à l’utilisateur à l’aide de Javascript.

Les parties importantes de la solution sont :

-  Contenu statique comme le **style.css** fichier.
-  Comme les fichiers de modèles Razor .cshtml **RazorView.cshtml** .
-  Les classes qui sont référencés dans les modèles Razor, telles que de modèle **ExampleModel.cs** .
-  La classe spécifique à la plateforme qui crée l’affichage web et restitue le modèle, telles que la `MainActivity` sur Android et le `iPhoneHybridViewController` sur iOS.


La section suivante explique comment les projets fonctionnent.

### <a name="static-content"></a>Contenu statique

Contenu statique inclut des feuilles de style CSS, des images, des fichiers Javascript ou tout autre contenu pouvant être lié à partir d’ou référencé par un fichier HTML affiché dans un affichage web.

Les projets de modèle incluent une feuille de style minimal pour montrer comment inclure du contenu statique dans votre application hybride. La feuille de style CSS est référencé dans le modèle comme suit :

```html
<link rel="stylesheet" href="style.css" />
```

Vous pouvez ajouter les fichiers Javascript que vous avez besoin, y compris les infrastructures de JQuery et toute feuille de style.

### <a name="razor-cshtml-templates"></a>Razor cshtml modèles

Le modèle inclut un Razor **.cshtml** fichier qui a déjà écrit du code pour communiquer des données entre le HTML/Javascript et c#. Cela vous permettra de génération d’applications hybrides sophistiquées qui ne simplement afficher des données en lecture seule à partir du modèle, mais également accepter l’entrée d’utilisateur dans le code HTML et le passer au code c# pour le traitement ou de stockage.

#### <a name="rendering-the-template"></a>Le modèle de rendu

Appel de la `GenerateString` sur un modèle de rendu HTML prêt pour l’affichage dans un affichage web. Si le modèle utilise un modèle il doit être fourni avant le rendu. Ce diagramme illustre le rendu fonctionne – pas les ressources statiques sont résolus par l’affichage web lors de l’exécution, à l’aide du répertoire de base fourni pour rechercher les fichiers spécifiés.

 ![Organigramme de Razor](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Appeler du code c# à partir du modèle

Communication à partir d’un affichage web rendu rappel du langage c# est effectuée par la définition de l’URL pour l’affichage web, puis en interceptant la demande en c# pour gérer la demande native sans recharger l’affichage web.

Un exemple sont consultables dans modalités de bouton de RazorView. Le bouton a le code HTML suivant :

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

Le `InvokeCSharpWithFormValues` fonction Javascript lit toutes les valeurs à partir du formulaire HTML et les jeux de la `location.href` pour l’affichage web :

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

Il tente de naviguer dans l’affichage web vers une URL avec un schéma personnalisé (par exemple). `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Lorsque l’affichage web natif traite cette requête de navigation, nous avons la possibilité d’intercepter. Dans iOS, cela en gérant les événements de HandleShouldStartLoad de la UIWebView. Dans Android, nous avons simplement sous-classe le WebViewClient utilisé dans le formulaire et remplacer ShouldOverrideUrlLoading.

Les mécanismes internes de ces deux intercepteurs de navigation sont essentiellement le même.

Vérifiez tout d’abord, l’URL de l’affichage web tente de charger, et si elle ne commence pas par le schéma personnalisé (`hybrid:`), autoriser la navigation normalement.

Pour le schéma d’URL personnalisé, tous les éléments de l’URL entre le schéma et les « ? » est le nom de la méthode d’être géré (dans ce cas, « UpdateLabel »). Tous les éléments de la chaîne de requête seront traité comme paramètres pour l’appel de méthode :

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` Dans cet exemple effectue une quantité minimale de manipulation de chaîne sur le paramètre de la zone de texte (en ajoutant « c# intitulée « à la chaîne) et puis rappelle à l’affichage web.

Après le traitement de l’URL, la méthode abandonne le volet de navigation afin que l’affichage web ne tente pas à terminer la navigation vers l’URL personnalisée.

#### <a name="manipulating-the-template-from-c"></a>Manipuler le modèle à partir de c#

Communication avec un affichage web de rendu HTML à partir de c# est effectuée en appelant Javascript dans l’affichage web. Sur iOS, cela est effectué en appelant `EvaluateJavascript` sur la UIWebView :

```csharp
webView.EvaluateJavascript (js);
```

Sur Android, Javascript peut être appelée dans l’affichage web en chargeant le Javascript comme une URL en utilisant le `"javascript:"` le modèle d’URL :

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>En faisant une application véritablement hybride

Ces modèles ne rendez pas utiliser de contrôles natifs sur chaque plateforme – la totalité de l’écran est remplie avec une vue web unique.

HTML peut être intéressante pour le prototypage et afficher les types d’éléments web est mieux comme texte enrichi et de présentation interactive. Cependant pas toutes les tâches sont adaptés au format HTML et Javascript – défiler de longues listes de données, par exemple, exécute le mieux à l’aide de contrôles d’interface utilisateur natives comme (UITableView sur iOS) ou ListView sur Android.

Les vues web dans le modèle peuvent facilement être augmentés avec contrôles spécifiques à la plateforme : il vous suffit de modifier le **MainStoryboard.storyboard** dans le concepteur iOS ou **Resources/layout/Main.axml** sur Android.

### <a name="razortodo-sample"></a>Exemple de RazorTodo

Le [RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) référentiel contient deux solutions distinctes pour afficher les différences entre une application entièrement piloté par HTML et une application qui combine HTML avec les contrôles natifs :

-  **RazorTodo** -application pilotée par complètement HTML à l’aide de modèles Razor.
-  **RazorNativeTodo** - utilise les contrôles d’affichage de liste natif pour iOS et Android, mais affiche l’écran de modification en HTML et Razor.


Ces applications de Xamarin s’exécutent sur iOS et Android, en utilisant les bibliothèques de classes portables (PCLs) pour partager le code commun, telles que les classes de base de données et le modèle. Razor **.cshtml** modèles peuvent également être inclus dans la bibliothèque PCL afin qu’ils sont facilement partagés entre les plateformes.

Les deux exemples d’applications incorporent Twitter de partage et des API synthèse vocale à partir de la plateforme native, démontrer qu’applications hybrides avec Xamarin ont toujours accès à toutes les fonctionnalités sous-jacentes à partir des vues de HTML Razor pilotées par modèle.

Le **RazorTodo** application utilise des modèles de HTML Razor pour les affichages de listes et les modifier. Cela signifie que nous pouvons générer l’application presque entièrement dans une bibliothèque de classes Portable partagée (y compris la base de données et **.cshtml** modèles Razor). Les captures d’écran ci-dessous montrent les applications Android et iOS.

 ![RazorTodo](images/Both_700x290.png)

Le **RazorNativeTodo** application utilise un modèle HTML Razor pour le mode édition, mais elle implémente une liste déroulante native sur chaque plateforme. Cela fournit de nombreux avantages, notamment :

-  Performances - les contrôles de défilement native utilisent la virtualisation pour vous assurer de défilement rapide et sans heurts même avec très longues listes de données.
-  Expérience native - éléments d’interface utilisateur spécifique à la plateforme sont facilement activé, telles que la prise en charge des index défilement rapide dans iOS et Android.


Des principaux avantages de la création d’applications hybrides avec Xamarin sont que vous pouvez démarrer avec une interface utilisateur entièrement basée sur le HTML (par exemple, le premier exemple) et ajouter des fonctionnalités spécifiques à une plateforme à la demande (comme le montre l’exemple deuxième). Les écrans de liste natif HTML Razor modifier les écrans sur les deux iOS et Android sont indiquées ci-dessous.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Récapitulatif

Cet article a expliqué les fonctionnalités des contrôles d’affichage web disponibles sur iOS et Android qui facilitent la création d’applications hybrides.

Il décrit ensuite le moteur de création de modèles Razor et la syntaxe qui peut être utilisée pour générer facilement du code HTML dans des applications Xamarin à l’aide. **cshtml** les fichiers de modèles Razor. Il décrit également le Visual Studio pour Mac, des modèles de solution qui vous permettent de rapidement commencer à générer des applications hybrides avec Xamarin.

Enfin, il introduit les exemples RazorTodo qui montrent comment combiner des vues web des interfaces utilisateur natives et des API.

### <a name="related-links"></a>Liens associés

- [Exemple de RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 : le moteur d’affichage Razor (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
