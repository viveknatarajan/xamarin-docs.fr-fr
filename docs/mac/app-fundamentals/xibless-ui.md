---
title: conception de l’interface utilisateur.Storyboard/.XIB-less
description: Cet article décrit la création d’interface utilisateur d’une application Xamarin.Mac directement à partir de code c#, sans .storyboard fichiers, les fichiers .xib ou constructeur d’Interface.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 66725b02d3e351e74fa79ae5336a7db3a9f2b534
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>conception de l’interface utilisateur.Storyboard/.XIB-less

_Cet article décrit la création d’interface utilisateur d’une application Xamarin.Mac directement à partir de code c#, sans .storyboard fichiers, les fichiers .xib ou constructeur d’Interface._


## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès aux mêmes éléments d’interface utilisateur et des outils fournis par un développeur travaillant dans *Objective-C* et *Xcode* est. En règle générale, lors de la création d’une application Xamarin.Mac, vous utiliserez le Générateur de Xcode Interface avec fichiers .storyboard ou .xib pour créer et maintenir vous interface utilisateur de l’application.

Vous avez également la possibilité de créer tout ou partie de l’interface utilisateur de votre application Xamarin.Mac directement en code c#. Dans cet article, nous aborderons les principes fondamentaux de la création d’interfaces utilisateur et les éléments d’interface utilisateur en code c#.

[![Visual Studio pour l’éditeur de code Mac](xibless-ui-images/intro01.png "Visual Studio pour l’éditeur de code Mac")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Passage d’une fenêtre pour utiliser du code

Lorsque vous créez une nouvelle application/Cocoa de Xamarin.Mac, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un **Main.storyboard** (ou en règle générale un **MainWindow.xib**) fichier inclus automatiquement dans le projet. Cela inclut également un **ViewController.cs** fichier qui gère la vue principale de l’application (ou à nouveau en règle générale un **MainWindow.cs** et un **MainWindowController.cs** fichier).

Pour basculer vers une fenêtre Xibless pour une application, procédez comme suit :

1. Ouvrez l’application que vous souhaitez cesser d’utiliser `.stroyboard` ou fichiers .xib pour définir l’interface utilisateur dans Visual Studio pour Mac.
2. Dans le **Solution remplissage**, avec le bouton droit sur le **Main.storyboard** ou **MainWindow.xib** fichier et sélectionnez **supprimer**: 

    ![Suppression de la fenêtre ou storyboard principal](xibless-ui-images/switch01.png "supprimer le storyboard principal ou la fenêtre")
3. À partir de la **supprimer la boîte de dialogue**, cliquez sur le **supprimer** bouton pour supprimer le .storyboard ou .xib complètement à partir du projet : 

    ![Confirmer la suppression](xibless-ui-images/switch02.png "confirmant la suppression")

Maintenant nous allons devoir modifier le **MainWindow.cs** fichier pour définir la disposition de la fenêtre et modifier le **ViewController.cs** ou **MainWindowController.cs** pour créer un instance de notre `MainWindow` classe étant donné que nous utilisons n’est plus le fichier .storyboard ou .xib.

Les applications modernes Xamarin.Mac qui utilisent des tables de montage séquentiel pour l’interface utilisateur ne peut pas inclure automatiquement le **MainWindow.cs**, **ViewController.cs** ou **MainWindowController.cs** fichiers. Si nécessaire, ajoutez simplement une classe c# vide nouveau au projet (**ajouter** > **nouveau fichier...**   >  **Général** > **classe vide**) et nommez le même que le fichier manquant. 


### <a name="defining-the-window-in-code"></a>Définition de la fenêtre de code

Modifiez ensuite la **MainWindow.cs** de fichier et le rendre l’aspect suivant :

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

Examinons quelques-uns des principaux éléments.

Tout d’abord, nous avons ajouté quelques _calculées des propriétés_ qui agit comme prises (comme si la fenêtre a été créée dans un fichier .storyboard ou .xib) :

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Ces donne accès aux éléments d’interface utilisateur que nous allons à afficher dans la fenêtre. Étant donné que la fenêtre n’est pas en cours agrandie à partir d’un fichier .storyboard ou .xib, nous avons besoin d’un moyen de l’instancier (comme nous le verrons plus tard dans le `MainWindowController` classe). C’est ce que fait cette nouvelle méthode de constructeur :

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Il s’agit où nous allons concevoir la disposition de la fenêtre et placer les éléments d’interface utilisateur nécessaires pour créer l’interface utilisateur requis. Pour que nous pouvons ajouter les éléments d’interface utilisateur à une fenêtre, elle doit un _affichage du contenu_ pour contenir les éléments :

```csharp
ContentView = new NSView (Frame);
```

Cette opération crée une vue de contenu qui remplit la fenêtre. Maintenant nous ajoutons notre premier élément d’interface utilisateur, un `NSButton`, dans la fenêtre :

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

La première chose à noter ici est que, contrairement à iOS, macOS utilise la notation mathématique pour définir son système de coordonnées de fenêtre. Par conséquent, le point d’origine est dans le coin inférieur gauche de la fenêtre, avec des valeurs croissantes à droite et vers le coin supérieur droit de la fenêtre. Lorsque nous créer le nouveau `NSButton`, nous en tenir compte comme nous définissons sa position et la taille à l’écran.

Le `AutoresizingMask = NSViewResizingMask.MinYMargin` propriété indique au bouton que nous souhaitons reste dans le même emplacement à partir du haut de la fenêtre lorsque la fenêtre est redimensionnée verticalement. Là encore, cela est nécessaire car (0,0) est en bas à gauche de la fenêtre.

Enfin, le `ContentView.AddSubview (ClickMeButton)` méthode ajoute la `NSButton` à l’affichage du contenu afin qu’elle s’affichera sur l’écran lorsque l’application est exécutée et que la fenêtre affichée.

Ensuite, une étiquette est ajoutée à la fenêtre qui affiche le nombre de fois qui le `NSButton` a été activé : 

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

Puisque macOS n’a pas un spécifique _étiquette_ élément d’interface utilisateur, nous avons ajouté un style spécialement, non modifiable `NSTextField` d’agir en tant qu’étiquette. À l’instar du bouton avant, la taille et l’emplacement prend en compte que (0,0) est en bas à gauche de la fenêtre. Le `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` à l’aide de la propriété du **ou** opérateur pour combiner deux `NSViewResizingMask` fonctionnalités. Cela facilitera l’étiquette restent dans le même emplacement à partir du haut de la fenêtre lorsque la fenêtre est redimensionnée verticalement et les variations de largeur de la fenêtre est redimensionnée horizontalement.

Là encore, la `ContentView.AddSubview (ClickMeLabel)` méthode ajoute la `NSTextField` à l’affichage du contenu afin qu’elle s’affichera sur l’écran lorsque l’application est exécutée et la fenêtre ouverte.


### <a name="adjusting-the-window-controller"></a>Ajuster le contrôleur de fenêtre

Depuis la conception de la `MainWindow` n’est plus en cours de chargement à partir d’un fichier .storyboard ou .xib, nous devons apporter quelques modifications au contrôleur de fenêtre. Modifier la **MainWindowController.cs** de fichier et le rendre l’aspect suivant :

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

Permettent de traitent les éléments clés de cette modification.

Tout d’abord, nous définissons une nouvelle instance de la `MainWindow` classe et l’affecter au contrôleur de fenêtre de base `Window` propriété :

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Nous allons définir l’emplacement de la fenêtre de l’écran avec un `CGRect`. Tout comme le système de coordonnées de la fenêtre de l’écran définit (0,0) en tant que le coin inférieur gauche. Ensuite, nous définissons le style de la fenêtre à l’aide de la **ou** opérateur pour combiner deux ou plusieurs `NSWindowStyle` fonctionnalités :

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Les éléments suivants `NSWindowStyle` les fonctionnalités sont disponibles :

- **Sans bordure** -la fenêtre n’aura aucune bordure.
- **Intitulée** -la fenêtre possédera une barre de titre.
- **Fermeture** -la fenêtre possède un bouton Fermer et peut être fermée.
- **Miniaturizable** -la fenêtre a un bouton Miniaturize et peut être réduite.
- **Redimensionnable** -un bouton redimensionner et d’être redimensionnables de la fenêtre.
- **Utilitaire** -la fenêtre est un utilitaire style (Panneau de configuration).
- **DocModal** -si la fenêtre est un panneau de configuration, il sera modale Document au lieu de modale système. 
- **NonactivatingPanel** -si la fenêtre est un panneau de configuration, il ne sera pas automatiquement la fenêtre principale.
- **TexturedBackground** -la fenêtre aura un arrière-plan de texture.
- **Le** -la fenêtre n’est pas à l’échelle.
- **UnifiedTitleAndToolbar** -zones de titre et la barre d’outils de la fenêtre seront joint.
- **HUD** -la fenêtre s’affichera sous la forme d’un panneau d’affichage tête haute.
- **FullScreenWindow** -la fenêtre peut entrer en mode plein écran.
- **FullSizeContentView** -affichage du contenu de la fenêtre est derrière le titre et la barre d’outils de zone.

Les deux dernières propriétés définissent la _mise en mémoire tampon de Type_ de la fenêtre et si le dessin de la fenêtre sera être différé. Pour plus d’informations sur `NSWindows`, consultez Apple [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) documentation.

Enfin, la fenêtre n’est pas en cours agrandie à partir d’un fichier .storyboard ou .xib, nous devons simuler dans notre **MainWindowController.cs** en appelant les fenêtres `AwakeFromNib` méthode :

```csharp
Window.AwakeFromNib ();
```

Ainsi, vous permet de code par rapport à la fenêtre simplement comme une fenêtre standard chargée à partir d’un fichier .storyboard ou .xib.


### <a name="displaying-the-window"></a>Affichage de la fenêtre

Le fichier .storyboard ou .xib la suppression et la **MainWindow.cs** et **MainWindowController.cs** fichiers modifiés, vous allez utiliser la fenêtre comme vous le feriez pour n’importe quelle fenêtre normal qui a été créé dans Générateur d’Interface de Xcode avec un fichier .xib.

Les éléments suivants crée une nouvelle instance de la fenêtre et son contrôleur et afficher la fenêtre à l’écran :

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

À ce stade, si l’application est exécutée et quelques fois clique sur le bouton, le texte suivant s’affichera :

![Un exemple d’application exécuter](xibless-ui-images/run01.png "un exemple d’application exécuter")


## <a name="adding-a-code-only-window"></a>Ajout d’une seule fenêtre de code

Si vous souhaitez ajouter un code uniquement, de la fenêtre de xibless à une application Xamarin.Mac existante, avec le bouton droit sur le projet dans le **Solution remplissage** et sélectionnez **ajouter** > **nouveau fichier...** . Dans le **nouveau fichier** boîte de dialogue Choisir **Xamarin.Mac** > **fenêtre/Cocoa avec contrôleur**, comme illustré ci-dessous :

![Ajoutez un nouveau contrôleur de fenêtre](xibless-ui-images/add01.png "Ajout d’un nouveau contrôleur de fenêtre") 

Comme auparavant, nous allons supprimer le fichier de .storyboard ou .xib de valeur par défaut à partir du projet (dans ce cas **SecondWindow.xib**) et suivez les étapes de la [une fenêtre pour utiliser le Code de commutation](#Switching_a_Window_to_use_Code) ci-dessus pour couvrir la définition de la fenêtre de code.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Ajout d’un élément d’interface utilisateur dans une fenêtre de code

Si une fenêtre a été créée dans le code ou chargée à partir d’un fichier .storyboard ou .xib, il peut arriver que nous voulons pour ajouter un élément d’interface utilisateur dans une fenêtre de code. Par exemple :

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Le code ci-dessus crée un `NSButton` et l’ajoute à la `MyWindow` instance de la fenêtre d’affichage. Fondamentalement n’importe quel élément d’interface utilisateur qui peut être défini dans le Générateur de Xcode Interface dans un fichier .storyboard ou .xib permettre être créée dans le code et affiché dans une fenêtre.


## <a name="defining-the-menu-bar-in-code"></a>Définition de la barre de menus dans le code

En raison des limitations actuelles dans Xamarin.Mac, il n’est pas conseillée que vous créez votre application de Xamarin.Mac Menu barre –`NSMenuBar`– dans le code, mais continuer à utiliser le **Main.storyboard** ou **MainMenu.xib** fichier pour le définir. Ceci dit, vous pouvez ajouter et supprimer des Menus et des éléments de Menu dans le code c#.

Par exemple, modifiez le **AppDelegate.cs** et effectuez le `DidFinishLaunching` détaillée de la méthode comme suit :

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

La commande ci-dessus crée un menu de barre d’état à partir du code et l’affiche lorsque l’application est lancée. Pour plus d’informations sur l’utilisation des Menus, consultez notre [Menus](~/mac/user-interface/menu.md) documentation.


## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée de la création d’interface utilisateur d’une application Xamarin.Mac dans le code c# au lieu d’utiliser le Générateur de Xcode Interface avec les fichiers .storyboard ou .xib.



## <a name="related-links"></a>Liens associés

- [MacXibless (sample)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Fenêtres](~/mac/user-interface/window.md)
- [Menus](~/mac/user-interface/menu.md)
- [macOS indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Introduction à Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
