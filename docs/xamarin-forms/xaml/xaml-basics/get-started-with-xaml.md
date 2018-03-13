---
title: Partie 1. Prise en main de XAML
description: "Dans une application de Xamarin.Forms, XAML est principalement utilisé pour définir le contenu visuel d’une page. Un fichier XAML est toujours associé à un fichier de code c# qui fournit la prise en charge du code pour le balisage. Ensemble, ces deux fichiers contribuent à une définition de classe qui inclut l’initialisation des propriétés et des vues enfants. Dans le fichier XAML, classes et propriétés sont référencées avec des attributs et éléments XML, et établir des liens entre le balisage et le code."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 55c4fa73044b1c37e879ed7731161269bdbf8ba8
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="part-1-getting-started-with-xaml"></a>Partie 1. Prise en main de XAML

_Dans une application de Xamarin.Forms, XAML est principalement utilisé pour définir le contenu visuel d’une page. Un fichier XAML est toujours associé à un fichier de code c# qui fournit la prise en charge du code pour le balisage. Ensemble, ces deux fichiers contribuent à une définition de classe qui inclut l’initialisation des propriétés et des vues enfants. Dans le fichier XAML, classes et propriétés sont référencées avec des attributs et éléments XML, et établir des liens entre le balisage et le code._

## <a name="creating-the-solution"></a>Création de la solution

Pour commencer votre premier fichier XAML, utilisez Visual Studio ou Visual Studio pour Mac pour créer une solution Xamarin.Forms. (Sélectionnez l’onglet en haut de cette page correspondant à votre environnement.)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dans Windows, utilisez Visual Studio pour sélectionner **fichier > Nouveau > projet** à partir du menu. Dans le **nouveau projet** boîte de dialogue, sélectionnez **Visual c# > multiplateforme** situé à gauche, puis **Cross-Platform App (Xamarin.Forms ou natif)** à partir de la liste dans le centre. 

![](get-started-with-xaml-images/win/newprojectdialog.png "Boîte de dialogue Nouveau projet")

Sélectionnez un emplacement pour la solution, attribuez-lui un nom de **XamlSamples** (ou tout ce que vous préférez), puis appuyez sur **OK**.

Dans l’écran suivant, sélectionnez le **application vide** modèle, le **Xamarin.Forms** technologie d’interface utilisateur et le **bibliothèque de classes Portable (PCL)** stratégie de partage de code :

![](get-started-with-xaml-images/win/newcrossplatformapp.png "Boîte de dialogue nouvelle application")

Press **OK**. 

Quatre projets sont créés dans la solution : le **XamlSamples** bibliothèque de classes portable (PCL), **XamlSamples.Android**, **XamlSamples.iOS**et Windows universel Solution de plateforme, **XamlSamples.UWP**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans Visual Studio pour Mac, sélectionnez **fichier > Nouvelle Solution** à partir du menu. Dans le **nouveau projet** boîte de dialogue, sélectionnez **multiplateforme > application** situé à gauche, et **application Forms vide** (*pas* **Forms application** ) à partir de la liste des modèles :

![](get-started-with-xaml-images/mac/newprojectdialog1.png "Nouvelle boîte de dialogue projet 1")

Appuyez sur **suivant**.

Dans la boîte de dialogue suivante, nommez le projet de **XamlSamples** (ou tout ce que vous préférez). Assurez-vous que le **bibliothèque de classes portables utilisation** case d’option est sélectionnée et qui **XAML d’utilisation pour les fichiers d’interface utilisateur** est vérifiée :

![](get-started-with-xaml-images/mac/newprojectdialog2.png "Nouvelle boîte de dialogue projet 2")

Appuyez sur **suivant**. 

Dans la boîte de dialogue suivante, vous pouvez sélectionner un emplacement pour le projet :

![](get-started-with-xaml-images/mac/newprojectdialog3.png "Nouvelle boîte de dialogue projet 3")

Appuyez sur **créer**

Trois projets sont créés dans la solution : le **XamlSamples** bibliothèque de classes portable (PCL), **XamlSamples.Android**, et **XamlSamples.iOS**. 

-----

Après avoir créé le **XamlSamples** solution, vous pouvez à votre environnement de développement de test en sélectionnant les différents projets de plateforme en tant que projet de démarrage de solution, et créer et déployer l’application créée par le modèle de projet sur les émulateurs de téléphone ou de périphériques réels.

Sauf si vous avez besoin d’écrire du code spécifique à la plateforme, partagé **XamlSamples** est de projet de bibliothèque PCL où vous allez passer pratiquement tout votre temps de programmation. Ces articles ne seront pas entreprise en dehors de ce projet.

### <a name="anatomy-of-a-xaml-file"></a>Anatomie d’un fichier XAML

Dans le **XamlSamples** bibliothèque de classes portable sont une paire de fichiers avec les noms suivants :

- **App.XAML**, le fichier XAML ; et
- **App.Xaml.cs**, C# *code-behind* fichier associé au fichier XAML.

Vous devez cliquer sur la flèche à côté **App.xaml** pour afficher le fichier code-behind. 

Les deux **App.xaml** et **App.xaml.cs** contribuent à une classe nommée `App` qui dérive de `Application`. La plupart des autres classes avec des fichiers XAML contribuent à une classe qui dérive de `ContentPage`; ces fichiers utilisent XAML pour définir le contenu visuel d’une page entière. Cela est vrai pour les deux autres fichiers dans le **XamlSamples** projet :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **MainPage.xaml**, le fichier XAML ; et
- **MainPage.xaml.cs**, le fichier code-behind c#.

Le **MainPage.xaml** fichier ressemble à ceci :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

- **XamlSamplesPage.xaml**, le fichier XAML ; et
- **XamlSamplesPage.xaml.cs**, le fichier code-behind c#.

Le **XamlSamplesPage.xaml** fichier ressemble à ceci :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
             xmlns:local="clr-namespace:XamlSamples" 
             x:Class="XamlSamples.XamlSamplesPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

-----

L’espace de noms XML deux ( `xmlns`) déclarations font référence à l’URI, le premier apparemment sur le site web de Xamarin et le deuxième de Microsoft. Ne vous préoccupez pas quel point de ces URI pour la vérification de la. Il en est rien. Elles sont simplement des URI détenus par Xamarin et Microsoft, et ils fonctionnent essentiellement en tant qu’identificateurs de version.

La première déclaration d’espace de noms XML signifie que les balises définies dans le fichier XAML sans le préfixe font référence à des classes dans Xamarin.Forms, par exemple `ContentPage`. La seconde déclaration d’espace de noms définit un préfixe de `x`. Il est utilisé pour plusieurs éléments et attributs qui font partie intégrante XAML lui-même et qui sont prises en charge par les autres implémentations de XAML. Toutefois, ces éléments et attributs sont légèrement différentes selon l’année incorporée dans l’URI. Xamarin.Forms prend en charge la spécification de XAML 2009, mais pas tous.

Le `local` déclaration d’espace de noms permet d’accéder aux autres classes à partir du projet de bibliothèque de classes portables.

À la fin de cette première balise, le `x` préfixe est utilisé pour un attribut nommé `Class`. Étant donné que l’utilisation de ce `x` préfixe est pratiquement universel pour l’espace de noms XAML, attributs XAML tel que `Class` sont presque toujours appelé `x:Class`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le `x:Class` attribut spécifie un nom qualifié complet de la classe .NET : le `MainPage` classe dans le `XamlSamples` espace de noms. Cela signifie que ce fichier XAML définit une classe nommée `MainPage` dans les `XamlSamples` espace de noms qui dérive de `ContentPage`: la balise dans laquelle le `x:Class` attribut apparaît.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le `x:Class` attribut spécifie un nom qualifié complet de la classe .NET : le `XamlSamplesPage` classe dans le `XamlSamples` espace de noms. Cela signifie que ce fichier XAML définit une classe nommée `XamlSamplesPage` dans les `XamlSamples` espace de noms qui dérive de `ContentPage`: la balise dans laquelle le `x:Class` attribut apparaît.

-----

Le `x:Class` attribut peut uniquement apparaître dans l’élément racine d’un fichier XAML pour définir une classe dérivée de c#. Il s’agit de la classe seulement les nouvelles définie dans le fichier XAML. Tout ce qui apparaît dans le fichier XAML est à la place simplement instancié à partir de classes existantes et initialisé.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le **MainPage.xaml.cs** fichier ressemble à ceci (à l’exception d’inutilisé `using` directives) :

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

Le `MainPage` dérive de la classe `ContentPage`, mais notez le `partial` définition de classe. Par conséquent, il doit y avoir une autre définition de classe partielle pour `MainPage`, mais sur lesquels il ? Et ce qui est qui `InitializeComponent` méthode ? 

Lorsque Visual Studio génère le projet, il analyse le fichier XAML pour générer un fichier de code c#. Si vous regardez dans le **XamlSamples\XamlSamples\obj\Debug** active, vous trouverez un fichier nommé **XamlSamples.MainPage.xaml.g.cs**. Le « g » est l’acronyme de générée. Voici la définition de classe partielle de `MainPage` qui contient la définition de la `InitializeComponent` méthode appelée à partir de la `MainPage` constructeur. Ces deux partielle `MainPage` définitions de classe peuvent ensuite être compilées ensemble. Selon que le code XAML est compilé ou non, le fichier XAML ou une forme binaire du fichier XAML est incorporée dans le fichier exécutable.

Lors de l’exécution, le code dans les appels de projet de plateforme spécifique un `LoadApplication` méthode, en lui passant une nouvelle instance de la `App` classe dans la bibliothèque PCL. Le `App` instancie du constructeur de classe `MainPage`. Le constructeur de cette classe appelle `InitializeComponent`, qui appelle ensuite la `LoadFromXaml` méthode qui extrait le fichier XAML (ou son fichier binaire compilé) à partir de la bibliothèque PCL. `LoadFromXaml` Initialise tous les objets définis dans le fichier XAML, les connecte ensemble dans des relations parent-enfant, attache des gestionnaires d’événements définis dans le code à des événements dans le fichier XAML et définit l’arborescence résultant d’objets en tant que le contenu de la page.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le **XamlSamplesPage.xaml.cs** fichier ressemble à ceci :

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class XamlSamplesPage : ContentPage
    {
        public XamlSamplesPage()
        {
            InitializeComponent();
        }
    }
}
```

Le `XamlSamplesPage` dérive de la classe `ContentPage`, mais notez le `partial` définition de classe. Il suggère qu’il doit y avoir un autre fichier c# avec une autre définition de classe partielle pour `XamlSamplesPage`, mais sur lesquels il ? Et ce qui est qui `InitializeComponent` méthode ?

Lorsque Visual Studio pour Mac génère le projet, il analyse le fichier XAML pour générer un fichier de code c#. Si vous regardez dans le **XamlSamples\XamlSamples\obj\Debug** active, vous trouverez un fichier nommé **XamlSamples.XamlSamplesPage.xaml.g.cs**. Le « g » est l’acronyme de générée. Voici la définition de classe partielle de `XamlSamplesPage` qui contient la définition de la `InitializeComponent` méthode appelée à partir de la `XamlSamplesPage` constructeur.  Ces deux partielle `XamlSamplesPage` définitions de classe peuvent ensuite être compilées ensemble. Selon que le code XAML est compilé ou non, le fichier XAML ou une forme binaire du fichier XAML est incorporée dans le fichier exécutable.

Lors de l’exécution, le code dans les appels de projet de plateforme spécifique un `LoadApplication` méthode, en lui passant une nouvelle instance de la `App` classe dans la bibliothèque PCL. Le `App` instancie du constructeur de classe `XamlSamplesPage`. Le constructeur de cette classe appelle `InitializeComponent`, qui appelle ensuite la `LoadFromXaml` méthode qui extrait le fichier XAML (ou son fichier binaire compilé) à partir de la bibliothèque PCL. `LoadFromXaml` Initialise tous les objets définis dans le fichier XAML, les connecte ensemble dans des relations parent-enfant, attache des gestionnaires d’événements définis dans le code à des événements dans le fichier XAML et définit l’arborescence résultant d’objets en tant que le contenu de la page.

-----

Bien que vous normalement n’avez pas besoin de passer du temps avec les fichiers de code générés, parfois runtime exceptions sont levées sur le code dans les fichiers générés, donc vous devez être familiarisé avec eux.

Lorsque vous compilez et exécutez ce programme, le `Label` élément apparaît dans le centre de la page, comme l’indique le code XAML. Les trois plateformes de gauche à droite sont iOS, Android et Windows 10 Mobile :

[![](get-started-with-xaml-images/xamlsamples.png "Par défaut d’affichage de Xamarin.Forms")](get-started-with-xaml-images/xamlsamples-large.png#lightbox "affichage de la valeur par défaut de Xamarin.Forms")

Pour les éléments visuels de plus intéressants, il vous suffit de plus intéressant de XAML.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="preliminaries"></a>Préliminaires

Pour rendre les noms de fichiers dans Visual Studio pour Mac cohérent avec les fichiers créés par Visual Studio s’exécutant sous Windows, renommez **XamlSamplesPage.xaml** à **MainPage.xaml**, et  **XamlSamplesPage.xaml.cs** à **MainPage.xaml.cs**. Dans le **XamlSamplesPage.xaml** , modifiez `XamlSamplesPage` à `MainPage`. Dans le **XamlSamplesPage.xaml.cs** , modifiez les deux occurrences de `XamlSamplesPage` à `MainPage`. Dans le **App.xaml.cs** , modifiez l’instruction

```csharp
MainPage = new XamlSamplesPage();
```

à :

```csharp
MainPage = new MainPage();
```

-----

Vérifiez que le programme compile et déploie avant de continuer.

## <a name="adding-new-xaml-pages"></a>Ajout de nouvelles Pages XAML

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour ajouter d’autres en XAML `ContentPage` classes à votre projet, sélectionnez le **XamlSamples** PCL de projet et d’appeler le **projet > Ajouter un nouvel élément** élément de menu. À gauche de la **ajouter un nouvel élément** boîte de dialogue, sélectionnez **Visual C#** et **Xamarin.Forms**. Dans la liste Sélectionnez **Page de contenu** (pas **Page de contenu (c#)**, ce qui crée une page de code uniquement, ou **affichage du contenu**, qui n’est pas une page). Nommez la page, par exemple, **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.png "Ajouter la boîte de dialogue Nouvel élément")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour ajouter d’autres en XAML `ContentPage` classes à votre projet, sélectionnez le **XamlSamples** PCL de projet et d’appeler le **fichier > nouveau fichier** élément de menu. À gauche de la **nouveau fichier** boîte de dialogue, sélectionnez **Forms** situé à gauche, et **Forms ContentPage Xaml** (pas **Forms ContentPage**, qui Crée une page de code uniquement, ou **affichage du contenu**, qui n’est pas une page). Nommez la page, par exemple, **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "Boîte de dialogue Nouveau fichier")

-----

Deux fichiers sont ajoutés au projet, **HelloXamlPage.xaml** et le fichier code-behind **HelloXamlPage.xaml.cs**. 

## <a name="setting-page-content"></a>Définition du contenu de la Page

Modifier la **HelloXamlPage.xaml** de fichiers afin que les balises uniquement sont celles pour `ContentPage` et `ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

Le `ContentPage.Content` balises font partie de la syntaxe unique du code XAML. Dans un premier temps, ils peuvent sembler XML non valide, mais ils sont valides. La période n’est pas un caractère spécial dans XML.

Le `ContentPage.Content` les balises sont appelées *élément de propriété* balises. `Content` est une propriété de `ContentPage`et généralement défini sur une vue ou une mise en page avec les vues enfants. Normalement propriétés deviennent des attributs en XAML, mais il est difficile de définir un `Content` d’attribut à un objet complexe. Pour cette raison, la propriété est exprimée comme un élément XML comprenant le nom de classe et le nom de propriété séparés par un point. Maintenant le `Content` propriété peut être définie entre la `ContentPage.Content` balises, comme suit :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

Notez également qu’un `Title` attribut a été défini sur la balise racine.

À ce stade, la relation entre les classes, propriétés et XML doit être évidente : Xamarin.Forms d’une classe (tel que `ContentPage` ou `Label`) apparaît dans le fichier XAML comme un élément XML. Propriétés de cette classe, y compris `Title` sur `ContentPage` et sept propriétés de `Label`— apparaissent généralement sous la forme d’attributs XML.

Il existe de nombreux raccourcis pour définir les valeurs de ces propriétés. Certaines propriétés sont des types de base de données : par exemple, le `Title` et `Text` propriétés sont de type `String`, `Rotation` est de type `Double`, et `IsVisible` (qui est `true` par défaut et est définie ici uniquement pour illustration) est de type `Boolean`.

Le `HorizontalTextAlignment` propriété est de type `TextAlignment`, qui est une énumération. Pour une propriété de n’importe quel type d’énumération, vous devez fournir est un nom de membre.

Pour les propriétés de types plus complexes, toutefois, les convertisseurs sont utilisés pour l’analyse du code XAML. Ce sont les classes dans Xamarin.Forms qui dérivent de `TypeConverter`. La plupart sont des classes publiques, mais certains ne sont pas. Pour ce fichier XAML particulier, plusieurs de ces classes jouent un rôle en arrière-plan :

-  `LayoutOptionsConverter` pour le `VerticalOptions` propriété
-  `FontSizeConverter` pour le `FontSize` propriété
-  `ColorTypeConverter` pour le `TextColor` propriété

Ces convertisseurs régissent la syntaxe autorisée de paramètres de la propriété.

Le `ThicknessTypeConverter` peut gérer une, deux ou quatre nombres séparés par des virgules. Si un nombre est fourni, elle s’applique à toutes les quatre côtés. Avec deux nombres, le premier est de gauche et à droite de la marge intérieure et la seconde se trouve en haut et bas. Quatre nombres sont dans l’ordre gauche, haut, droite et en bas.

Le `LayoutOptionsConverter` peut convertir les noms des champs statiques publics de la `LayoutOptions` structure aux valeurs de type `LayoutOptions`.

Le `FontSizeConverter` peut gérer un `NamedSize` membre ou une taille de police numériques.

Le `ColorTypeConverter` accepte les noms des champs statiques publics de la `Color` structure ou valeurs RVB hexadécimales, avec ou sans un canal alpha, précédé d’un signe dièse (#). Voici la syntaxe sans un canal alpha :

 `TextColor="#rrggbb"`

Chacun des lettres peu est un chiffre hexadécimal. Voici comment un canal alpha est inclus :

 `TextColor="#aarrggbb">`

Pour le canal alpha, gardez à l’esprit que FF est complètement opaque et 00 est totalement transparent.

Deux autres formats permettent de ne spécifier qu’un seul chiffre hexadécimal pour chaque canal :

 `TextColor="#rgb"` `TextColor="#argb"`

Dans ces cas, le chiffre est répété pour former la valeur. Par exemple, #CF3 est la couleur RVB CC-FF-33.

## <a name="page-navigation"></a>Navigation entre les pages

Lorsque vous exécutez le **XamlSamples** programme, le `MainPage` s’affiche. Pour afficher le nouveau `HelloXamlPage` vous pouvez soit définir que comme le démarrage de nouveau la page dans le **App.xaml.cs** de fichier, ou accédez à la nouvelle page à partir de `MainPage`.

Pour implémenter la navigation, choisissez tout d’abord le code dans le **App.xaml.cs** constructeur afin qu’un `NavigationPage` objet est créé :

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

Dans le **MainPage.xaml.cs** constructeur, vous pouvez créer un simple `Button` et utilisez le Gestionnaire d’événements pour accéder à `HelloXamlPage`:

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

Définition de la `Content` propriété de la page remplace le paramètre de la `Content` propriété dans le fichier XAML. Lorsque vous compilez et déployez la nouvelle version de ce programme, un bouton apparaît sur l’écran. En appuyant sur elle accède à `HelloXamlPage`. Voici la page résultante sur l’iPhone, Android et Windows 10 Mobile appareils :

[![](get-started-with-xaml-images/helloxaml1.png "Rotation du texte d’étiquette")](get-started-with-xaml-images/helloxaml1-large.png#lightbox "pivoter le texte de l’étiquette")

Vous pouvez naviguer jusqu'à `MainPage` à l’aide de la **< retour** bouton sur iOS, à l’aide de la flèche gauche en haut de la page ou du bas du téléphone sur Android, ou à l’aide de la flèche gauche en bas de la page sur Windows 10 Mobile.

N’hésitez pas à faire des essais avec le code XAML de différentes façons restituer le `Label`. Si vous avez besoin incorporer des caractères Unicode dans le texte, vous pouvez utiliser la syntaxe XML standard. Par exemple, pour placer le message d’accueil dans des guillemets, utilisez :

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Voici à quoi elle ressemble :

[![](get-started-with-xaml-images/helloxaml2.png "Rotation du texte d’étiquette avec des caractères Unicode")](get-started-with-xaml-images/helloxaml2-large.png#lightbox "pivoter le texte d’étiquette avec des caractères Unicode")

## <a name="xaml-and-code-interactions"></a>XAML et les Interactions de Code

Le **HelloXamlPage** exemple contient un seul `Label` sur la page, mais il est très rare. La plupart des `ContentPage` dérivés ensemble la `Content` trier de propriété à une disposition de certaines, comme un `StackLayout`. Le `Children` propriété de la `StackLayout` est défini comme étant de type `IList<View>` mais il est en fait un objet de type `ElementCollection<View>`, et que la collection peut être remplie avec plusieurs vues ou d’autres configurations. En XAML, ces relations parent-enfant sont établies avec la hiérarchie XML classique. Voici un fichier XAML pour une nouvelle page nommée **XamlPlusCodePage**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Ce fichier XAML est syntaxiquement complète et Voici à quoi elle ressemble :

[![](get-started-with-xaml-images/xamlpluscode1.png "Plusieurs contrôles sur une Page")](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox "plusieurs contrôles sur une Page")

Toutefois, vous êtes susceptible de prendre en compte ce programme fonctionnellement insuffisantes. Peut-être le `Slider` est censée pour entraîner le `Label` pour afficher la valeur actuelle et le `Button` n’est probablement destinée à faire quelque chose dans le programme.

Comme vous le verrez dans [partie 4. Principes fondamentaux de liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md), le travail d’afficher un `Slider` à l’aide de la valeur un `Label` peut être géré entièrement en XAML avec une liaison de données. Mais il est utile d’afficher le code de la solution en premier. Même dans ce cas, la gestion de la `Button` cliquez définitivement nécessite que du code. Cela signifie que le fichier code-behind pour `XamlPlusCodePage` doit contenir des gestionnaires pour les `ValueChanged` l’événement de la `Slider` et le `Clicked` l’événement de la `Button`. Nous allons ajouter :

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

Ces gestionnaires d’événements n’avez pas besoin être public.

Dans le fichier XAML, le `Slider` et `Button` balises besoin d’inclure des attributs pour le `ValueChanged` et `Clicked` les événements qui font référence à ces gestionnaires :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

Notez que l’affectation d’un gestionnaire à un événement a la même syntaxe que l’affectation d’une valeur à une propriété.

Si le gestionnaire pour le `ValueChanged` l’événement de la `Slider` utilisera le `Label` pour afficher la valeur actuelle, le gestionnaire doit faire référence à cet objet à partir du code. Le `Label` a besoin d’un nom, qui est spécifié avec la `x:Name` attribut.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

Le `x` préfixe de le `x:Name` attribut indique que cet attribut est intrinsèque à XAML.

Le nom que vous attribuez à la `x:Name` attribut a les mêmes règles que les noms de variables c#. Par exemple, il doit commencer par une lettre ou un trait de soulignement et contenir des espaces incorporés.

Maintenant le `ValueChanged` Gestionnaire d’événements peut définir la `Label` pour afficher la nouvelle `Slider` valeur. La nouvelle valeur est disponible à partir des arguments d’événement :

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Ou, le gestionnaire peut obtenir le `Slider` objet qui génère cet événement à partir de la `sender` argument et obtenir le `Value` propriété à partir de ce :

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

Lorsque vous exécutez le programme, le `Label` n’affiche pas les `Slider` valeur, car le `ValueChanged` événement n’a pas encore déclenché. Mais toute manipulation de la `Slider` provoque la valeur à afficher :

[![](get-started-with-xaml-images/xamlpluscode2.png "Valeur de curseur affichée")](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox "valeur curseur affichée")

Pour le `Button`. Nous allons simuler une réponse à une `Clicked` événement en affichant une alerte avec le `Text` du bouton. Le Gestionnaire d’événements peut en toute sécurité converti le `sender` argument à un `Button` et accéder à ses propriétés :

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

La méthode est définie en tant que `async` , car le `DisplayAlert` méthode est asynchrone et doivent être précédé du `await` opérateur, qui renvoie à l’issue de la méthode. Étant donné que cette méthode obtient le `Button` déclenchant l’événement à partir de la `sender` argument, le même gestionnaire peut être utilisé pour plusieurs boutons.

Vous avez vu qu’un objet défini dans XAML peut se déclencher un événement qui est géré dans le fichier code-behind, et que le fichier code-behind peut accéder à un objet défini dans XAML en utilisant le nom qui lui est affecté avec la `x:Name` attribut. Il existe deux façons fondamentaux qui interagissent de code et XAML.

Des informations supplémentaires sur les comment fonctionne XAML permettre être collectées en examinant généré récemment **XamlPlusCode.xaml.g.cs fichier**, qui comprend désormais n’importe quel nom attribué à aucun `x:Name` attribut sous la forme d’un champ privé. Voici une version simplifiée de ce fichier :

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

La déclaration de ce champ permet à la variable à librement utiliser n’importe où dans le `XamlPlusCodePage` fichier de classe partielle sous votre juridiction. Lors de l’exécution, le champ est assigné après que le code XAML a été analysé. Cela signifie que la `valueLabel` champ est `null` lors de la `XamlPlusCodePage` constructeur commence mais valides après `InitializeComponent` est appelée.

Après avoir `InitializeComponent` rend le contrôle au constructeur, les éléments visuels de la page ont été construits, comme s’ils avaient été instanciés et initialisés dans le code. Le fichier XAML ne peut plus lire de n’importe quel rôle dans la classe. Vous pouvez manipuler ces objets dans la page de quelque manière qui vous intéressent, par exemple, en ajoutant des vues à le `StackLayout`, ou un paramètre le `Content` propriété de la page à autre chose entièrement. Vous pouvez « parcourir l’arborescence » en examinant le `Content` propriété de la page et les éléments de la `Children` collections dispositions. Vous pouvez définir des propriétés sur les vues de cette manière, ou leur attribuer dynamiquement des gestionnaires d’événements.

N’hésitez pas. Il s’agit de votre page et XAML est uniquement un outil pour générer son contenu.

## <a name="summary"></a>Récapitulatif

Avec cette présentation, vous avez vu comment un fichier XAML et un fichier de code contribuent à une définition de classe et l’interagissent entre les fichiers XAML et le code. Mais XAML possède également son propre syntaxiques fonctionnalités uniques qui lui permettent d’être utilisé de manière très souple. Vous pouvez commencer à Explorer dans [partie 2. La syntaxe XAML essentielles](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).



## <a name="related-links"></a>Liens associés

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Partie 2. Syntaxe XAML essentielle](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Partie 3. Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Partie 4. Notions de base sur la liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Partie 5. À partir de la liaison de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
