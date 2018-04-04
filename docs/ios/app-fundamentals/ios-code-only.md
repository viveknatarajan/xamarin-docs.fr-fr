---
title: Création d’Interfaces utilisateur d’iOS dans le Code
description: Xamarin.iOS fournit deux méthodes de création d’une Interface utilisateur pour votre application, avec le Concepteur de Xamarin pour iOS ou dans le code. Cet article explique comment créer des interfaces utilisateur iOS entièrement dans le code. Il montre comment démarrer à partir d’un modèle de projet pour créer un écran de l’application dans un contrôleur en créant une hiérarchie des vues à partir de UIKit. Ensuite, il explique comment créer des vues personnalisées qui peuvent être chargées dans un contrôleur.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7e8460d2c946159a9869322d6d4944d213d3d801
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-ios-user-interfaces-in-code"></a>Création d’Interfaces utilisateur d’iOS dans le Code

_Xamarin.iOS fournit deux méthodes de création d’une Interface utilisateur pour votre application, avec le Concepteur de Xamarin pour iOS ou dans le code. Cet article explique comment créer des interfaces utilisateur iOS entièrement dans le code. Il montre comment démarrer à partir d’un modèle de projet pour créer un écran de l’application dans un contrôleur en créant une hiérarchie des vues à partir de UIKit. Ensuite, il explique comment créer des vues personnalisées qui peuvent être chargées dans un contrôleur._

L’interface utilisateur d’une application iOS est comme une vitrine – l’application obtient généralement une fenêtre, mais il peut remplir la fenêtre avec comme de nombreux objets il a besoin, et les objets et peut être modifié en fonction de ce que l’application souhaite afficher. Les objets dans ce scénario, les éléments que l’utilisateur voit, sont appelés des affichages. Pour générer un seul écran dans une application, les vues sont empilées les unes sur les autres dans une hiérarchie de vue de contenu, et la hiérarchie est gérée par un seul contrôleur de la vue. Les applications à plusieurs écrans ont plusieurs hiérarchies d’affichage de contenu, chacune avec son propre contrôleur d’affichage. L’application place les affichages dans la fenêtre pour créer une hiérarchie d’affichage de contenu différente selon l’écran sur lequel se trouve l’utilisateur.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le diagramme ci-dessous illustre les relations entre la fenêtre, les affichages, les sous-affichages et le contrôleur d’affichage qui font apparaître l’interface utilisateur dans l’écran de l’appareil : 

[![](ios-code-only-images/image9.png "Ce diagramme illustre les relations entre la fenêtre, des vues, sous-vues et View Controller")](ios-code-only-images/image9.png#lightbox)

Ces hiérarchies de la vue peuvent être construits à l’aide de la [concepteur Xamarin pour iOS](~/ios/user-interface/designer/index.md) dans Visual Studio, cependant il est recommandé d’avoir une connaissance fondamentale de l’utilisation entièrement dans le code. Cet article explique les certains points de base pour obtenir et l’exécution avec le développement de l’interface utilisateur de code uniquement.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le diagramme ci-dessous illustre les relations entre la fenêtre, les affichages, les sous-affichages et le contrôleur d’affichage qui font apparaître l’interface utilisateur dans l’écran de l’appareil : 

[![](ios-code-only-images/image9.png "Ce diagramme illustre les relations entre la fenêtre, des vues, sous-vues et View Controller")](ios-code-only-images/image9.png#lightbox)


Ces hiérarchies de la vue peuvent être construits à l’aide de la [concepteur Xamarin pour iOS](~/ios/user-interface/designer/index.md) dans Visual Studio pour Mac, cependant il est recommandé d’avoir une connaissance fondamentale de l’utilisation entièrement dans le code. Cet article explique les certains points de base pour obtenir et l’exécution avec le développement de l’interface utilisateur de code uniquement.


-----

## <a name="creating-a-code-only-project"></a>Création d’un projet de Code uniquement

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS modèle projet vide

Tout d’abord, créez un projet iOS dans Visual Studio à l’aide de l’iPhone **projet vide** modèle, illustré ci-dessous, qui nous étendrons pour ajouter des contrôleurs et vues.


[![](ios-code-only-images/blankapp-vs.png "Boîte de dialogue Nouveau projet")](ios-code-only-images/blankapp-vs.png#lightbox)


Le modèle de projet vide ajoute 4 fichiers au projet :


[![](ios-code-only-images/empty-project.png "Fichiers projet")](ios-code-only-images/empty-project.png#lightbox)


1. **AppDelegate.cs** -contient un `UIApplicationDelegate` sous-classe, `AppDelegate` , qui est utilisé pour gérer les événements d’application à partir d’iOS. La fenêtre d’application est créée dans le `AppDelegate`de `FinishedLaunching` (méthode).
1. **Main.cs** -contient le point d’entrée pour l’application, qui spécifie la classe pour le `AppDelegate` .
1. **Info.plist** -fichier de liste de propriétés qui contient des informations de configuration d’application.
1. **Entitlements.plist** : fichier de liste de propriétés qui contient des informations sur les fonctions et les autorisations de l’application.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="ios-templates"></a>Modèles d’iOS


Visual Studio pour Mac ne fournit pas d’un modèle vide. Tous les modèles sont fournis avec prise en charge de plan conceptuel, qui vous recommande d’Apple comme moyen principal pour créer une interface utilisateur. Toutefois, il est possible de créer votre interface utilisateur entièrement dans le code. 

Les étapes ci-dessous vous guident lors de la suppression du plan conceptuel à partir d’une application : 


1. Utilisez le modèle d’application de vue unique pour créer un nouveau iOS projet :
    
    [![](ios-code-only-images/single-view-app.png "Utilisez le modèle application vue unique")](ios-code-only-images/single-view-app.png#lightbox)

1. Supprimer le `Main.Storyboard` et `ViewController.cs` fichiers. Faire **pas** supprimer le `LaunchScreen.Storyboard`. Le contrôleur de la vue doit être supprimé car il s’agit du code-behind pour le contrôleur de vue qui est créé dans le plan conceptuel :
1. Veillez à sélectionner **supprimer** à partir de la boîte de dialogue contextuelle :
    
    [![](ios-code-only-images/delete.png "Sélectionnez Supprimer dans la boîte de dialogue contextuelle")](ios-code-only-images/delete.png#lightbox)

1. Dans le fichier Info.plist, supprimer les informations à l’intérieur de la **informations de déploiement > Interface principale** option :
    
    [![](ios-code-only-images/main-interface.png "Supprimer les informations à l’intérieur de l’option de l’Interface principale")](ios-code-only-images/main-interface.png#lightbox)

1. Enfin, ajoutez le code suivant à votre `FinishedLaunching` méthode dans la classe AppDelegate :
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }


-----

## <a name="creating-a-window"></a>Création d’une fenêtre

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le code qui a été ajouté à la `FinishedLaunching` consiste à l’étape 3 ci-dessus, la quantité minimale de code requis pour créer une fenêtre de votre application iOS.  

-----

les applications iOS sont générées à l’aide de la [modèle MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Le premier écran qui affiche d’une application est créé à partir du contrôleur de la fenêtre racine de la vue. Consultez le [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) guide pour plus d’informations sur le MVC modèle lui-même.

L’implémentation pour la `AppDelegate` ajoutées par le modèle crée la fenêtre d’application, de laquelle il n’est qu’une pour chaque application iOS et le rend visible par le code suivant :

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

Si vous deviez exécuter cette application maintenant, vous obtiendriez probablement une exception indiquant que `Application windows are expected to have a root view controller at the end of application launch`. Nous allons ajouter un contrôleur et le rendre racine View Controller l’application.

## <a name="adding-a-controller"></a>Ajour d’un contrôleur

Votre application peut contenir plusieurs contrôleurs de vue, mais il doit disposer d’un contrôleur d’affichage racine pour contrôler tous les contrôleurs de vue.  Ajouter un contrôleur à la fenêtre en créant un `UIViewController` instance et lui affectant le `window.RootViewController` propriété :

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Chaque contrôleur possède une vue associée, qui est accessible à partir de la `View` propriété. Le code ci-dessus modifie la vue `BackgroundColor` propriété `UIColor.LightGray` afin qu’il soit visible, comme indiqué ci-dessous :

 [![](ios-code-only-images/image1.png "Arrière-plan de la vue est un gris clair visible")](ios-code-only-images/image1.png#lightbox)

Nous avons défini les `UIViewController` sous-classe en tant que le `RootViewController` de cette manière, y compris les contrôleurs de UIKit ainsi qu’aux sites, nous nous écrire. Par exemple, le code suivant ajoute un `UINavigationController` comme le `RootViewController`:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Ce code produit le contrôleur imbriqué dans le contrôleur de navigation, comme indiqué ci-dessous :

 [![](ios-code-only-images/image2.png "Le contrôleur est imbriqué dans le contrôleur de navigation")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Création d’un contrôleur d’affichage

Maintenant que nous avons vu comment ajouter un contrôleur en tant que le `RootViewController` de la fenêtre, nous allons voir comment créer un contrôleur de la vue personnalisée dans le code.

Ajouter une nouvelle classe nommée `CustomViewController` comme indiqué ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.png "Ajouter une nouvelle classe nommée CustomViewController")](ios-code-only-images/customviewcontroller.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Ajouter une nouvelle classe nommée CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

La classe doit hériter de `UIViewController`, qui se trouve dans le `UIKit` espace de noms, comme indiqué :

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>L’initialisation de la vue

`UIViewController` contient une méthode appelée `ViewDidLoad` qui est appelée lorsque le contrôleur de la vue est chargé en mémoire. Il s’agit d’un emplacement approprié pour effectuer l’initialisation de la vue, telles que la définition de ses propriétés.

Par exemple, le code suivant ajoute un bouton et un gestionnaire d’événements pour transmettre un nouveau contrôleur de vue dans la pile de navigation lorsque le bouton est activé :

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Pour charger ce contrôleur dans votre application et illustrent la navigation simple, créer une nouvelle instance de `CustomViewController`. Créer un nouveau contrôleur de navigation, passez dans votre instance de contrôleur d’affichage et le nouveau contrôleur de navigation la fenêtre de la valeur `RootViewController` dans le `AppDelegate` que précédemment :

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Lorsque l’application charge maintenant, le `CustomViewController` est chargé à l’intérieur d’un contrôleur de navigation :

 [![](ios-code-only-images/customvc.png "Le CustomViewController est chargé à l’intérieur d’un contrôleur de navigation")](ios-code-only-images/customvc.png#lightbox)
 
En cliquant sur le bouton, sera _push_ un nouveau contrôleur de vue dans la pile de navigation :

[![](ios-code-only-images/customvca.png "Un nouveau contrôleur de la vue est placé sur la pile de navigation")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Création de la hiérarchie

Dans l’exemple ci-dessus, nous avons commencé à créer une interface utilisateur dans le code en ajoutant un bouton sur le contrôleur de la vue.

e/s des interfaces utilisateur sont constitués d’une hiérarchie de la vue. Affichages supplémentaires, telles que des étiquettes, des boutons, des curseurs, etc. sont ajoutés comme sous-vues d’une vue parent.

Par exemple, nous allons modifier le `CustomViewController` pour créer un écran d’ouverture de session dans laquelle l’utilisateur peut entrer un nom d’utilisateur et un mot de passe. L’écran se compose de deux champs de texte et un bouton.

### <a name="adding-the-text-fields"></a>Ajouter les champs de texte

Supprimez d’abord le Gestionnaire de bouton et d’événement qui a été ajouté dans le [l’initialisation de la vue](#Initializing_the_View) section. 

Ajouter un contrôle pour le nom d’utilisateur en créant et en initialisant un `UITextField` et son ajout à la hiérarchie, comme indiqué ci-dessous :

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

Lorsque nous créons le `UITextField`, nous avons défini le `Frame` propriété pour définir son emplacement et sa taille. Dans iOS coordonnées 0,0 sont trouve dans le coin supérieur gauche avec + x vers la droite et + y vers le bas. Après avoir défini la `Frame` , ainsi que quelques autres propriétés, nous appelons `View.AddSubview` pour ajouter le `UITextField` à la hiérarchie de la vue. Cela rend la `usernameField` un sous-affichage de la `UIView` de l’instance qui le `View` références de propriété. Un sous-affichage est ajouté avec un ordre de plan est supérieur à la vue parent, afin qu’il s’affiche devant la vue parent à l’écran.

L’application avec le `UITextField` inclus est indiqué ci-dessous :

 [![](ios-code-only-images/image4.png "L’application avec le UITextField inclus")](ios-code-only-images/image4.png#lightbox)

Nous pouvons ajouter un `UITextField` le mot de passe de la même manière, seulement cette fois-ci que nous avons défini le `SecureTextEntry` propriété sur true, comme indiqué ci-dessous :

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField); 
      View.AddSubview(passwordField);
   }
}

```

Paramètre `SecureTextEntry = true` masque le texte entré dans le `UITextField` par l’utilisateur, comme indiqué ci-dessous :

 [![](ios-code-only-images/image4a.png "Paramètre SecureTextEntry true masque le texte entré par l’utilisateur")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Ajout du bouton

Ensuite, nous allons ajouter un bouton afin de l’utilisateur peut soumettre le nom d’utilisateur et un mot de passe. Le bouton est ajouté à la hiérarchie de la vue comme tout autre contrôle, en passant en tant qu’argument à la vue parent `AddSubview` méthode à nouveau.

Le code suivant ajoute le bouton et inscrit un gestionnaire d’événements pour le `TouchUpInside` événement :

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Tout cela en place, l’écran de connexion apparaît désormais comme indiqué ci-dessous :

 [![](ios-code-only-images/image5.png "L’écran de connexion")](ios-code-only-images/image5.png#lightbox)

Contrairement à dans les versions antérieures d’iOS, l’arrière-plan du bouton par défaut est transparent. Modification du bouton `BackgroundColor` propriété change cela :

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Cela entraîne un bouton carré plutôt que classique arrondi avec bords de bouton. Pour obtenir le bord arrondi, utilisez l’extrait de code suivant :

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Avec ces modifications, la vue doit ressembler à ceci :

[![](ios-code-only-images/image6.png "Une exécution de l’exemple de la vue")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Ajout de plusieurs vues pour afficher la hiérarchie

e/s fournit une fonctionnalité pour ajouter plusieurs vues à la hiérarchie de la vue à l’aide de `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Ajout de fonctionnalités de bouton

Lorsqu’un clic est effectué, vos utilisateurs attendent une opération. Par exemple, une alerte est affichée ou navigation est effectuée vers un autre écran. 

Vous allez ajouter du code pour transmettre un second contrôleur de vue dans la pile de navigation.

Commencez par créer le second contrôleur de vue :

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Ensuite, ajoutez les fonctionnalités à la `TouchUpInside` événement :

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Le volet de navigation est illustrée ci-dessous :

[![](ios-code-only-images/navigation.png "Le volet de navigation est illustrée dans ce graphique")](ios-code-only-images/navigation.png#lightbox)

Notez que par défaut, lorsque vous utilisez un contrôleur de Navigation, iOS donne à l’application une barre de navigation et un bouton précédent vous permet de parcourir la pile en arrière.

## <a name="iterating-through-the-view-hierarchy"></a>Itération au sein de la hiérarchie

Il est possible pour une itération au sein de la hiérarchie sous-affichage et choisir n’importe quelle vue particulière. Par exemple, pour rechercher chaque `UIButton` et appliquer ce bouton une autre `BackgroundColor`, l’extrait de code suivant peut être utilisé.

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

Toutefois ne fonctionne pas si la vue itérée pour est un `UIView` comme toutes les vues reviendra comme étant un `UIView` en tant que les objets ajoutés à la vue parent eux-mêmes héritent `UIView`.

## <a name="handling-rotation"></a>Rotation de la gestion

Si l’utilisateur fait pivoter l’appareil en mode paysage, les contrôles ne se redimensionnent pas correctement, comme l’illustre la capture d’écran suivante :

 [![](ios-code-only-images/image7.png "Si l’utilisateur fait pivoter l’appareil en mode paysage, les contrôles ne se redimensionnent pas correctement")](ios-code-only-images/image7.png#lightbox)

La première consiste à résoudre ce problème en définissant le `AutoresizingMask` propriété sur chaque vue. Dans ce cas nous souhaitons les contrôles d’étendre horizontalement, donc nous affectons chaque `AutoresizingMask`. L’exemple suivant est pour `usernameField`, mais le même doivent être appliquées à chaque gadget dans la hiérarchie.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Maintenant lorsque nous faire pivoter le simulateur ou un périphérique, tous les éléments s’étire pour occuper l’espace supplémentaire, comme indiqué ci-dessous :

 [![](ios-code-only-images/image8.png "Tous les contrôles s’étire pour remplir l’espace supplémentaire")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Création de vues personnalisées

Outre l’utilisation de contrôles qui font partie de UIKit, des vues personnalisées peuvent également servir. Une vue personnalisée peut être créée en héritant de `UIView` et en remplaçant `Draw`. Nous allons créer une vue personnalisée et l’ajouter à la hiérarchie pour illustrer.

### <a name="inheriting-from-uiview"></a>Héritage de UIView

La première chose que nous devons faire est de créer une classe pour la vue personnalisée. Nous verrons comment procéder à l’aide de la **classe** modèle dans Visual Studio pour ajouter une classe vide nommée `CircleView`. La classe de base doit être définie sur `UIView`, qui nous rappeler se trouve dans le `UIKit` espace de noms. Nous devons également le `System.Drawing` espace de noms. Les différentes autres `System.*` espaces de noms ne pourra plus être utilisé dans cet exemple, par conséquent, n’hésitez pas à les supprimer.

La classe doit ressembler à ceci :

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Dessin dans une UIView

Chaque `UIView` a un `Draw` méthode est appelée par le système lorsqu’il doit être dessiné. `Draw` ne doit jamais être appelée directement. Elle est appelée par le système lors du traitement de la boucle d’exécution. La première fois dans la boucle d’exécution après l’ajout d’une vue à la hiérarchie de la vue, sa `Draw` méthode est appelée. Les appels suivants à `Draw` se produisent lorsque la vue est marquée comme devant être dessiné en appelant `SetNeedsDisplay` ou `SetNeedsDisplayInRect` sur la vue.

Nous pouvons ajouter le code de dessin à notre vue en ajoutant ce code à l’intérieur de la `Draw` méthode comme indiqué ci-dessous :

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Étant donné que `CircleView` est un `UIView`, nous pouvons définir `UIView` également des propriétés. Par exemple, nous pouvons définir le `BackgroundColor` dans le constructeur :

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Pour utiliser le `CircleView` nous venons de créer, nous pouvons soit l’ajouter en tant qu’un sous-affichage à la hiérarchie dans un contrôleur existant, comme nous l’avons fait avec la `UILabels` et `UIButton` précédemment, ou vous pouvez le charger en tant que l’affichage d’un nouveau contrôleur. Nous allons suivre ce dernier.

### <a name="loading-a-view"></a>Le chargement d’une vue

 `UIViewController` a une méthode nommée `LoadView` qui est appelée par le contrôleur pour créer la vue. Il s’agit d’un emplacement approprié pour créer une vue et l’affecter à du contrôleur `View` propriété.

Tout d’abord, nous avons besoin d’un contrôleur, créez une classe vide nommée `CircleController`.

Dans `CircleController` ajoutez le code suivant pour définir le `View` à un `CircleView` (vous ne devez pas appeler la `base` mise en œuvre dans votre substitution) :

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Enfin, nous devons présenter le contrôleur lors de l’exécution. Nous allons faire cela en ajoutant un gestionnaire d’événements sur le bouton envoyer que nous avons ajouté précédemment, comme suit :

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Maintenant, exécutez l’application et appuyez sur le bouton Soumettre, la nouvelle vue avec un cercle s’affiche :

 [![](ios-code-only-images/circles.png "La nouvelle vue avec un cercle s’affiche.")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Création d’un écran de lancement

A [écran initial](~/ios/app-fundamentals/images-icons/launch-screens.md) s’affiche au démarrage de votre application comme un moyen d’afficher à vos utilisateurs, qu’il ne répond. Car un écran de démarrage s’affiche lors du chargement de votre application, il ne peut pas créé dans le code que l’application est toujours chargée en mémoire. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lorsque votre créer une iOS projet dans Visual Studio, un écran de lancement est fournie sous la forme d’un fichier .xib, qui se trouve dans le **ressources** dossier à l’intérieur de votre projet. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lorsque votre créer un projet iOS dans Visual Studio pour Mac, un écran de lancement est fournie sous la forme d’un fichier d’animation. 

-----

Cela peut être modifié en double cliquant dessus et en l’ouvrant dans le concepteur iOS.

Apple recommande un .xib ou un fichier d’animation est utilisé pour les applications qui ciblent iOS 8 ou une version ultérieure, lorsque vous lancez des fichiers dans le concepteur iOS, vous utiliserez les Classes de taille et de mise en page automatique pour l’adapter à votre disposition pour qu’il semble correct et s’affiche correctement pour tous les périphériques tailles. Une image statique de lancement peut servir en plus un .xib ou d’une table de montage séquentiel pour permettre la prise en charge pour les applications qui ciblent des versions antérieures.

Pour plus d’informations sur la création d’un écran de lancement, reportez-vous aux documents ci-dessous :

- [Création d’un écran de démarrage à l’aide d’un .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Gestion des écrans de démarrage avec les plans conceptuels](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> À partir d’iOS 9, Apple recommande que les animations doivent être utilisées comme principale méthode de création d’un écran de lancement.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Création d’une Image de lancement pour iOS préalable des 8 applications

Une image statique peut être utilisée en plus un .xib ou un écran de lancement de plan conceptuel si votre application cible des versions antérieures à iOS 8. 

Cette image statique peut être définie dans le fichier Info.plist, ou comme un catalogue (pour iOS 7) dans votre application. Vous devrez fournir des images distincts pour chaque taille de périphérique (320 x 480, 640 x 960, 640 x 1136) que votre application peut s’exécuter sur. Pour plus d’informations sur les tailles d’écran de lancement, affichez le [Images d’écran de lancement](~/ios/app-fundamentals/images-icons/launch-screens.md) guide.

> [!IMPORTANT]
> Si votre application n’a pas l’écran de lancement, vous pouvez remarquer qu’il ne tient pas complètement l’écran. Si c’est le cas, vous devez vous assurer d’inclure au moins une image de 640 x 1136 nommée `Default-568@2x.png` à votre fichier Info.plist. 



## <a name="summary"></a>Récapitulatif

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Cet article a expliqué comment développer des applications iOS par programmation dans Visual Studio. Nous avons à la création d’un projet à partir d’un modèle de projet vide, expliquant comment créer et ajouter un contrôleur racine de la vue dans la fenêtre. Ensuite, nous vous avons montré comment utiliser des contrôles à partir de UIKit pour créer une hiérarchie d’affichage d’un contrôleur pour développer un écran de l’application. Ensuite nous avons examiné comment pour rendre les vues de mise en page appropriée dans différentes orientations et nous avons vu comment créer un affichage personnalisé en sous-classant `UIView`, ainsi que la charger l’affichage d’un contrôleur. Enfin, nous Explorer l’ajout d’un écran de démarrage pour une application.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Cet article a expliqué comment développer des applications iOS par programmation dans Visual Studio pour Mac. Nous avons à la création d’un projet à partir d’un modèle de vue unique, qui expliquent comment créer et ajouter un contrôleur racine de la vue dans la fenêtre. Ensuite, nous vous avons montré comment utiliser des contrôles à partir de UIKit pour créer une hiérarchie d’affichage d’un contrôleur pour développer un écran de l’application. Ensuite nous avons examiné comment pour rendre les vues de mise en page appropriée dans différentes orientations et nous avons vu comment créer un affichage personnalisé en sous-classant `UIView`, ainsi que la charger l’affichage d’un contrôleur. Enfin, nous Explorer l’ajout d’un écran de démarrage pour une application.

-----




## <a name="related-links"></a>Liens associés

- [SimpleLogin (sample)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
