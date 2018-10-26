---
title: conception de l’interface utilisateur.Storyboard/.XIB-less dans Xamarin.Mac
description: Cet article décrit la création d’interface d’utilisateur d’une application Xamarin.Mac directement à partir de C# code, sans les fichiers .storyboard, fichiers .xib ou Interface Builder.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: e41f19c1a2d02537f300ae82b7f3d45bc6571e1b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112453"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>conception de l’interface utilisateur.Storyboard/.XIB-less dans Xamarin.Mac

_Cet article décrit la création d’interface d’utilisateur d’une application Xamarin.Mac directement à partir de C# code, sans les fichiers .storyboard, fichiers .xib ou Interface Builder._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec C# et .NET dans une application Xamarin.Mac, vous avez accès aux mêmes éléments d’interface utilisateur et des outils fournis par un développeur qui travaille *Objective-C* et *Xcode* est. En règle générale, lorsque vous créez une application Xamarin.Mac, vous utiliserez Interface Builder de Xcode avec fichiers .storyboard ou .xib pour créer et gérer vous interface utilisateur de l’application.

Vous avez également la possibilité de créer tout ou partie de l’interface utilisateur de votre application Xamarin.Mac directement dans C# code. Dans cet article, nous aborderons les principes fondamentaux de la création d’interfaces utilisateur et les éléments d’interface utilisateur dans C# code.

[![L’éditeur Visual Studio pour Mac code](xibless-ui-images/intro01.png "l’éditeur Visual Studio pour Mac code")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Une fenêtre pour utiliser le code de commutation

Lorsque vous créez une nouvelle application Xamarin.Mac Cocoa, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un **Main.storyboard** (ou traditionnellement un **MainWindow.xib**) fichier automatiquement inclus dans le projet. Cela inclut également un **ViewController.cs** fichier qui gère la vue principale de l’application (ou à nouveau traditionnellement un **MainWindow.cs** et un **MainWindowController.cs** fichier).

Pour basculer vers une fenêtre Xibless pour une application, procédez comme suit :

1. Ouvrez l’application que vous souhaitez cesser d’utiliser `.stroyboard` ou fichiers .xib pour définir l’interface utilisateur dans Visual Studio pour Mac.
2. Dans le **panneau solutions**, avec le bouton droit sur le **Main.storyboard** ou **MainWindow.xib** fichier et sélectionnez **supprimer**: 

    ![Suppression de la fenêtre ou storyboard principal](xibless-ui-images/switch01.png "suppression de la fenêtre ou storyboard principal")
3. À partir de la **supprimer la boîte de dialogue**, cliquez sur le **supprimer** bouton pour supprimer le .storyboard ou .xib complètement à partir du projet : 

    ![Confirmer la suppression](xibless-ui-images/switch02.png "confirmant la suppression")

Maintenant nous allons devoir modifier le **MainWindow.cs** fichier pour définir la disposition de fenêtres et modifier le **ViewController.cs** ou **MainWindowController.cs** fichier pour créer un instance de notre `MainWindow` classe puisque nous utilisons n’est plus le fichier .storyboard ou .xib.

Les applications Xamarin.Mac modernes qui utilisent des tables de montage séquentiel pour leur interface utilisateur ne peut pas inclure automatiquement le **MainWindow.cs**, **ViewController.cs** ou **MainWindowController.cs** fichiers. Si nécessaire, ajoutez simplement un nouveau vide C# classe au projet (**ajouter** > **nouveau fichier...**   >  **Général** > **classe vide**) et il le même nom que le fichier manquant. 


### <a name="defining-the-window-in-code"></a>Définition de la fenêtre de code

Ensuite, modifiez le **MainWindow.cs** fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Nous allons aborder quelques éléments clés.

Tout d’abord, nous avons ajouté quelques _propriétés calculées_ qui agit comme des prises de courant (comme si la fenêtre a été créée dans un fichier .storyboard ou .xib) :

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Ceux-ci nous donnera accès aux éléments d’interface utilisateur que nous allons afficher dans la fenêtre. Étant donné que la fenêtre n’est pas en cours agrandie à partir d’un fichier .storyboard ou .xib, nous avons besoin d’un moyen de l’instancier (comme nous le verrons plus tard dans le `MainWindowController` classe). C’est ce que fait cette nouvelle méthode de constructeur :

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Il s’agit où nous concevoir la disposition de la fenêtre et placer les éléments d’interface utilisateur nécessaires pour créer l’interface utilisateur requis. Avant de pouvoir ajouter des éléments d’interface utilisateur à une fenêtre, il a besoin un _affichage du contenu_ pour contenir les éléments :

```csharp
ContentView = new NSView (Frame);
```

Cela crée une vue de contenu qui remplira la fenêtre. Maintenant nous ajouter notre premier élément d’interface utilisateur, un `NSButton`, dans la fenêtre :

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

La première chose à noter ici est que, contrairement à iOS, Mac OS utilise la notation mathématique pour définir son système de coordonnées de fenêtre. Par conséquent, le point d’origine est dans le coin inférieur gauche de la fenêtre, avec des valeurs croissantes de droite et vers le coin supérieur droit de la fenêtre. Lorsque nous créons la nouvelle `NSButton`, nous en tenir compte que nous définissons sa position et la taille à l’écran.

Le `AutoresizingMask = NSViewResizingMask.MinYMargin` propriété indique au bouton que nous voulons qu’il reste au même emplacement à partir du haut de la fenêtre lorsque la fenêtre est redimensionnée verticalement. Là encore, cela est nécessaire car (0,0) est en bas à gauche de la fenêtre.

Enfin, le `ContentView.AddSubview (ClickMeButton)` méthode ajoute la `NSButton` à l’affichage du contenu afin qu’elle s’affichera sur l’écran lorsque l’application est exécutée et la fenêtre affichée.

Ensuite, une étiquette est ajoutée à la fenêtre qui affiche le nombre de fois que le `NSButton` a été cliqué : 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

Étant donné que macOS n’a pas un spécifique _étiquette_ élément d’interface utilisateur, nous avons ajouté un style spécialement, non modifiable `NSTextField` d’agir en tant qu’étiquette. Tout comme le bouton avant, la taille et l’emplacement le prend en compte cette (0,0) est en bas à gauche de la fenêtre. Le `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` à l’aide de la propriété le **ou** opérateur pour combiner deux `NSViewResizingMask` fonctionnalités. Cela rendra l’étiquette de rester dans le même emplacement à partir du haut de la fenêtre lorsque la fenêtre est redimensionnée verticalement et réduire ainsi que leurs largeur quand la fenêtre est redimensionnée horizontalement.

Là encore, le `ContentView.AddSubview (ClickMeLabel)` méthode ajoute la `NSTextField` à l’affichage du contenu afin qu’elle s’affichera sur l’écran lorsque l’application est exécutée et la fenêtre ouverte.


### <a name="adjusting-the-window-controller"></a>Ajuster le contrôleur de fenêtre

Depuis la conception de la `MainWindow` n’est plus en cours de chargement à partir d’un fichier .storyboard ou .xib, nous allons devoir effectuer quelques ajustements pour le contrôleur de fenêtre. Modifier le **MainWindowController.cs** de fichiers et qu’elle ressemble à ce qui suit :

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Permettent d’aborder les éléments clés de cette modification.

Tout d’abord, nous définissons une nouvelle instance de la `MainWindow` classe et l’affecter au contrôleur de fenêtre de base `Window` propriété :

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Nous définissons l’emplacement de la fenêtre de l’écran avec un `CGRect`. Tout comme le système de coordonnées de la fenêtre, l’écran définit (0,0) comme le coin inférieur gauche. Ensuite, nous définissons le style de la fenêtre à l’aide de la **ou** opérateur pour combiner deux ou plusieurs `NSWindowStyle` fonctionnalités :

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Ce qui suit `NSWindowStyle` fonctionnalités sont disponibles :

- **Sans bordure** -la fenêtre n’aura aucune bordure.
- **Intitulée** -la fenêtre possédera une barre de titre.
- **Fermeture** -la fenêtre possède un bouton Fermer et peut être fermée.
- **Miniaturizable** -la fenêtre possède un bouton Miniaturize et peut être réduite.
- **Redimensionnable** -la fenêtre aura un bouton redimensionner et être redimensionnable.
- **Utilitaire** -la fenêtre est une fenêtre de style d’utilitaire (panneau).
- **DocModal** -si la fenêtre est un panneau, il sera modale Document au lieu de système modale. 
- **NonactivatingPanel** -si la fenêtre est un panneau, il ne sera pas automatiquement la fenêtre principale.
- **TexturedBackground** -la fenêtre aura un arrière-plan texturé.
- **Sans échelle** -la fenêtre n’est pas à l’échelle.
- **UnifiedTitleAndToolbar** -zones de titre et la barre d’outils de la fenêtre seront joint.
- **HUD** -la fenêtre s’affichera sous la forme d’un panneau d’affichage tête haute.
- **FullScreenWindow** -la fenêtre peut entrer en mode plein écran.
- **FullSizeContentView** -affichage du contenu de la fenêtre est derrière le titre et la barre d’outils de zone.

Les deux dernières propriétés définissent la _mise en mémoire tampon de Type_ de la fenêtre et si le dessin de la fenêtre est reportée. Pour plus d’informations sur `NSWindows`, consultez d’Apple [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) documentation.

Enfin, étant donné que la fenêtre n’est pas en cours agrandie à partir d’un fichier .storyboard ou .xib, nous devons simuler dans notre **MainWindowController.cs** en appelant les fenêtres `AwakeFromNib` méthode :

```csharp
Window.AwakeFromNib ();
```

Ainsi, vous permettent de code par rapport à la fenêtre simplement comme une fenêtre standard chargée à partir d’un fichier .storyboard ou .xib.


### <a name="displaying-the-window"></a>Affichage de la fenêtre

Avec le fichier .storyboard ou .xib supprimé et le **MainWindow.cs** et **MainWindowController.cs** fichiers modifiés, vous allez utiliser la fenêtre comme vous le feriez pour n’importe quelle fenêtre normale qui a été créé dans Interface Builder de Xcode avec un fichier .xib.

Les éléments suivants seront créer une nouvelle instance de la fenêtre et son contrôleur et affiche la fenêtre sur l’écran :

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

À ce stade, si l’application est exécutée et le bouton cliqué deux fois, ce qui suit s’affiche :

![Exécution d’une application exemple](xibless-ui-images/run01.png "une exécution de l’application exemple")


## <a name="adding-a-code-only-window"></a>Ajout d’une seule fenêtre de code

Si nous souhaitons ajouter un code uniquement, de la fenêtre de xibless à une application Xamarin.Mac, avec le bouton droit sur le projet dans le **panneau solutions** et sélectionnez **ajouter** > **nouveau fichier...** . Dans le **nouveau fichier** boîte de dialogue Choisir **Xamarin.Mac** > **fenêtre Cocoa avec contrôleur**, comme illustré ci-dessous :

![Ajoutez un nouveau contrôleur de fenêtre](xibless-ui-images/add01.png "ajoutez un nouveau contrôleur de fenêtre") 

Comme auparavant, nous allons supprimer le fichier par défaut .storyboard ou .xib à partir du projet (dans ce cas **SecondWindow.xib**) et suivez les étapes décrites dans le [une fenêtre pour utiliser le Code de commutation](#Switching_a_Window_to_use_Code) section ci-dessus pour couvrir le définition de la fenêtre de code.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Ajout d’un élément d’interface utilisateur dans une fenêtre de code

Si une fenêtre a été créée dans le code ou chargée à partir d’un fichier .storyboard ou .xib, il peut arriver dans lequel nous souhaitons ajouter un élément d’interface utilisateur à une fenêtre à partir du code. Exemple :

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Le code ci-dessus crée un nouveau `NSButton` et l’ajoute à la `MyWindow` instance de fenêtre pour l’affichage. Essentiellement n’importe quel élément d’interface utilisateur qui peut être définie dans l’Interface Builder de Xcode dans un fichier .storyboard ou .xib peut être créée dans le code et affichée dans une fenêtre.


## <a name="defining-the-menu-bar-in-code"></a>Définition de la barre de menus dans le code

En raison des limitations actuelles dans Xamarin.Mac, il n’est pas conseillée que vous créez votre application Xamarin.Mac Menu barre –`NSMenuBar`– dans du code, mais continuer à utiliser le **Main.storyboard** ou **MainMenu.xib** fichier à la définir. Ceci dit, vous pouvez ajouter et supprimer des Menus et éléments de Menu dans C# code.

Par exemple, modifiez le **AppDelegate.cs** et effectuez le `DidFinishLaunching` méthode ressemble ce qui suit :

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

La méthode ci-dessus crée un menu de barre d’état à partir du code et il s’affiche lorsque l’application est lancée. Pour plus d’informations sur l’utilisation des Menus, consultez notre [Menus](~/mac/user-interface/menu.md) documentation.


## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail la création d’interface d’utilisateur d’une application Xamarin.Mac dans C# code au lieu d’utiliser Interface Builder de Xcode avec fichiers .storyboard ou .xib.



## <a name="related-links"></a>Liens associés

- [MacXibless (exemple)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Menus](~/mac/user-interface/menu.md)
- [macOS Human Interface Guidelines pour](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Introduction à Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
