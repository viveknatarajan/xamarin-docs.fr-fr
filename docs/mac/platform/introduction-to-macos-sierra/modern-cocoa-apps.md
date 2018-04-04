---
title: Génération macOS moderne applications
description: Cet article décrit plusieurs conseils, fonctionnalités et techniques qu'un développeur peut utiliser pour générer une application moderne macOS dans Xamarin.Mac.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4eb4ff4a9e4784d816e2cbe8734e0422573cad92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="building-modern-macos-apps"></a>Génération macOS moderne applications

_Cet article décrit plusieurs conseils, fonctionnalités et techniques qu'un développeur peut utiliser pour générer une application moderne macOS dans Xamarin.Mac._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Création de recherche modernes avec les vues modernes

Une présentation moderne inclura une apparence moderne de fenêtre et la barre d’outils tels que l’exemple d’application indiqué ci-dessous :

[![](modern-cocoa-apps-images/content08.png "Un exemple d’une interface utilisateur d’application Mac moderne")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>L’activation d’intégral dimensionné affichage du contenu

Pour obtenir cette recherche dans une application Xamarin.Mac, le développeur vous voulez utiliser un _vue de contenu de taille complète_, ce qui signifie que le contenu s’étend sous les zones de l’outil et de la barre de titre et s’être automatiquement floues macOS.

Pour activer cette fonctionnalité dans le code, créez une classe personnalisée pour le `NSWindowController` et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

Cette fonctionnalité peut également être activée dans le Générateur de Xcode Interface en sélectionnant la fenêtre et en vérifiant **affichage du contenu de taille complète**:

[![](modern-cocoa-apps-images/content01.png "Modifier le storyboard principal dans le Générateur de Xcode Interface")](modern-cocoa-apps-images/content01.png#lightbox)

Lorsque vous utilisez une vue de contenu de taille complète, le développeur peut-être décaler le contenu sous les zones de barre de titre et l’outil afin que le contenu spécifique (par exemple, les étiquettes) ne faites glisser sous eux.

Pour compliquer ce problème, les zones de titre et la barre d’outils peuvent avoir une hauteur dynamique en fonction de l’action que l’utilisateur est en train de, la version de macOS que l’utilisateur a installé et/ou le matériel Mac que l’application est en cours d’exécution.

Par conséquent, tout simplement irréversible le décalage lors de la disposition de l’Interface utilisateur ne fonctionnera pas. Le développeur doit adopter une approche dynamique.

Apple a inclus le [clé-valeur Observable](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` propriété de la `NSWindow` classe pour obtenir la zone de contenu actuel dans le code. Le développeur peut utiliser cette valeur pour positionner manuellement les éléments requis lorsque la zone de contenu change.

La meilleure solution consiste à utiliser des Classes de taille et de disposition automatique pour positionner les éléments d’interface utilisateur dans le code ou le constructeur d’Interface.

Code à l’exemple suivant peut être utilisé pour placer des éléments d’interface utilisateur à l’aide de la mise en forme automatique et les Classes de taille dans-View-Controller l’application :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

Ce code crée le stockage pour une contrainte principale qui est appliquée à une étiquette (`ItemTitle`) pour vous assurer qu’il ne glisse sous la zone de titre et la barre d’outils :

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

En remplaçant le contrôleur de vue `UpdateViewConstraints` (méthode), le développeur peut tester pour voir si la contrainte requise a déjà été créée et créer si nécessaire.

Si une nouvelle contrainte doit être généré, le `ContentLayoutGuide` propriété de la fenêtre, le contrôle qui doit être limité est accessible et effectuer une conversion en un `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

La propriété TopAnchor de la `NSLayoutGuide` est accessible et si elle est disponible, il est utilisé pour générer une nouvelle contrainte avec la valeur de décalage souhaitée et la nouvelle contrainte devient active pour l’appliquer :

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>L’activation de barres d’outils simplifiées

Une fenêtre de macOS normal inclut une norme de que barre de titre en s’exécute le long au bord supérieur de la fenêtre. Si la fenêtre inclut également une barre d’outils, il s’affichera sous cette zone de la barre de titre :

[![](modern-cocoa-apps-images/content02.png "Une barre d’outils standard de Mac")](modern-cocoa-apps-images/content02.png#lightbox)

Lorsqu’un simplifiée de la barre d’outils disparaît de la zone de titre et la barre d’outils déplace vers le haut dans la position de la barre de titre, en ligne avec les boutons de fermeture de fenêtre, réduire et agrandir :

[![](modern-cocoa-apps-images/content03.png "Une barre d’outils Mac simplifiée")](modern-cocoa-apps-images/content03.png#lightbox)

La barre d’outils simplifiée est activée en remplaçant le `ViewWillAppear` méthode de la `NSViewController` et rendant ressembler à ce qui suit :

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Cela est généralement utilisé pour _des Applications de bibliothèque d’images_ (une seule fenêtre applications) comme des cartes, calendrier, Notes et préférences système. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>À l’aide de contrôleurs de la vue accessoire

Selon la conception de l’application, le développeur peut également compléter la barre de titre de zone avec un contrôleur de vue accessoire qui s’affiche juste en dessous de la zone de barre de titre et d’outils afin de fournir sensibles au contexte contrôles à l’utilisateur en fonction de l’activité qu’ils sont en cours :

[![](modern-cocoa-apps-images/content04.png "Un exemple accessoire-View-Controller")](modern-cocoa-apps-images/content04.png#lightbox)

Le contrôleur accessoire vue être floues et automatiquement redimensionné par le système, sans intervention des développeurs.

Pour ajouter un contrôleur de vue accessoire, procédez comme suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Main.storyboard` pour l’ouvrir et le modifier.
2. Faites glisser un **personnalisé-View-Controller** dans la hiérarchie de la fenêtre : 

    [![](modern-cocoa-apps-images/content05.png "Ajoutez un nouveau contrôleur de la vue personnalisée")](modern-cocoa-apps-images/content05.png#lightbox)
3. De disposition de la vue accessoire l’interface utilisateur : 

    [![](modern-cocoa-apps-images/content06.png "Conception de la nouvelle vue")](modern-cocoa-apps-images/content06.png#lightbox)
4. Exposer la vue accessoire comme un **prise** ainsi que tout autre **Actions** ou **prises** pour son interface utilisateur : 

    [![](modern-cocoa-apps-images/content07.png "Ajout de la sortie requise")](modern-cocoa-apps-images/content07.png#lightbox)
5. Enregistrez les modifications.
6. Revenez à Visual Studio pour Mac synchroniser les modifications.

Modifier le `NSWindowController` et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

Les points clés de ce code sont où la vue est définie sur la vue personnalisée qui a été définie dans le constructeur d’Interface et exposée comme un **prise**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

Et le `LayoutAttribute` qui définit _où_ l’accesseur s’affichera :

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Étant donné que macOS est maintenant entièrement localisé, le `Left` et `Right` `NSLayoutAttribute` propriétés ont été déconseillées et doit être remplacées par `Leading` et `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>À l’aide des fenêtres avec onglets

En outre, le système macOS peut ajouter des contrôleurs de la vue accessoire à la fenêtre d’application. Par exemple, créer des fenêtres avec onglet où plusieurs Windows de l’application sont fusionnés dans une fenêtre virtuel :

[![](modern-cocoa-apps-images/content08.png "Un exemple d’une fenêtre à onglets de Mac")](modern-cocoa-apps-images/content08.png#lightbox)

En règle générale, le développeur doit prendre l’utilisation de l’action limitée Windows avec onglets dans leurs applications Xamarin.Mac, le système puisse les gérer automatiquement comme suit :

- Windows sera automatiquement avec onglet lorsque la `OrderFront` méthode est appelée.
- Windows sera automatiquement Untabbed quand le `OrderOut` méthode est appelée.
- Dans le code de toutes les fenêtres avec onglets sont encore considérées comme « visibles », toutefois les onglets non au premier plan sont masqués par le système à l’aide de CoreGraphics.
- Utilisez le `TabbingIdentifier` propriété du `NSWindow` au groupe Windows en onglets.
- S’il s’agit une `NSDocument` application, plusieurs de ces fonctionnalités est activée automatiquement (par exemple, le bouton plus ajouté à la barre d’onglet) sans aucune intervention du développeur.
- Non -`NSDocument` applications peuvent activer le bouton « plus » dans le groupe d’onglets pour ajouter un nouveau document en remplaçant le `GetNewWindowForTab` méthode de la `NSWindowsController`.

Tous les éléments réunissant, le `AppDelegate` d’une application qui voulait utiliser Windows d’onglets système en fonction peut se présenter comme suit :

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

Où les `NewDocumentNumber` propriété assure le suivi du nombre de nouveaux documents créés et `NewDocument` méthode crée un nouveau document et l’affiche.

La `NSWindowController` pourrait puis ressembler à :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

Où la méthode statique `App` propriété fournit un raccourci pour accéder à la `AppDelegate`. Le `SetDefaultDocumentTitle` méthode définit un nouveau titre de documents en fonction du nombre de nouveaux documents créés.

Le code suivant, indique macOS que l’application préfère utiliser des onglets et fournit une chaîne qui permet à Windows de l’application d’être regroupés en onglets :

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

Et la méthode de substitution suivante ajoute un bouton plus sur la barre d’onglet qui créera un nouveau document lorsque l’utilisateur clique sur :

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>À l’aide de l’Animation Core

Animation de base est un moteur de rendu graphique haute tension intégrée à macOS. Animation de base a été optimisée pour tirer parti du GPU (GPU) disponible dans le matériel moderne macOS plutôt que les opérations graphiques sur l’UC, ce qui peut ralentir l’ordinateur en cours d’exécution.

Le `CALayer`, fourni par l’Animation Core, peut être utilisé pour des tâches telles que le défilement rapide et fluide et les animations. L’Interface utilisateur d’une application doit être composé de plusieurs sous-vues et des couches pour tirer pleinement parti des principales Animation.

A `CALayer` objet fournit plusieurs propriétés qui permettent au développeur de contrôler ce qui est présenté à l’écran à l’utilisateur telles que :

- `Content` -Peut être un `NSImage` ou `CGImage` qui fournit le contenu de la couche.
- `BackgroundColor` -Définit la couleur d’arrière-plan de la couche en tant qu’un `CGColor`
- `BorderWidth` -Définit la largeur de bordure.
- `BorderColor` -Définit la couleur de bordure.

Pour utiliser des graphiques de base dans l’interface utilisateur de l’application, il doit être à l’aide de _couche sauvegardé_ les vues, Apple suggère que le développeur doit toujours activer dans Affichage la fenêtre de contenu. De cette manière, toutes les vues enfants héritent automatiquement de sauvegarde de la couche ainsi.

En outre, Apple suggère à l’aide de vues de couche soutenu par opposition à l’ajout d’un nouveau `CALayer` comme une sous-couche, car le système gère automatiquement plusieurs paramètres obligatoires (tels que ceux requis par un écran Retina).

Couche de stockage peut être activée en définissant le `WantsLayer` d’un `NSView` à `true` ou à l’intérieur du constructeur d’Interface de Xcode sous le **vue Effets inspecteur** en vérifiant **Core Animation couche**:

[![](modern-cocoa-apps-images/content09.png "L’inspecteur d’effets d’affichage")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Redessiner les vues avec des couches

Une autre étape importante lors de la définition de l’utilisation des vues de soutenu de couche dans une application Xamarin.Mac la `LayerContentsRedrawPolicy` de la `NSView` à `OnSetNeedsDisplay` dans le `NSViewController`. Par exemple :

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Si le développeur ne définissez pas cette propriété, la vue sera redessinée à chaque modification de l’origine de l’image, ce qui n’est pas souhaité pour des raisons de performances. Avec cette propriété la valeur `OnSetNeedsDisplay` le développeur devra manuellement définir `NeedsDisplay` à `true` pour forcer le contenu à redessiner, toutefois.

Lorsqu’une vue est marquée comme modifiée, le système vérifie le `WantsUpdateLayer` propriété de la vue. Si elle retourne `true` le `UpdateLayer` méthode est appelée, sinon la `DrawRect` méthode de la vue est appelée pour mettre à jour le contenu.

Apple a les suggestions suivantes pour mettre à jour d’un contenu de vues à la demande :

- Apple préfère à l’aide de `UpdateLater` sur `DrawRect` chaque fois que possible, car il fournit une amélioration significative des performances.
- Utiliser le même `layer.Contents` pour les éléments d’interface utilisateur semblables.
- Apple préfère également au développeur de composer leur interface utilisateur à l’aide des affichages standard tels que `NSTextField`, à nouveau chaque fois que possible.

Pour utiliser `UpdateLayer`, créez une classe personnalisée pour la `NSView` et rendre le code de l’aspect suivant :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>À l’aide de moderne glisser- déposer

Pour présenter une expérience de glisser- déposer moderne pour l’utilisateur, le développeur doit adopter _glisser flocage_ dans de leur application opérations glisser-déplacer. Faites glisser Flocking est où chaque fichier individuel ou un élément glissé initialement apparaît comme un élément individuel qui troupeaux (groupe sous le curseur avec le nombre d’éléments) que l’utilisateur poursuit l’opération de glissement.

Si l’utilisateur arrête l’opération de glissement, les éléments individuels seront Unflock et leurs emplacements d’origine.

L’exemple de code suivant permet de glisser flocage sur une vue personnalisée :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

L’effet simulant a été accompli par envoi de chaque élément glissé vers le `BeginDraggingSession` méthode de la `NSView` comme un élément distinct dans un tableau.

Lorsque vous travaillez avec un `NSTableView` ou `NSOutlineView`, utilisez le `PastboardWriterForRow` méthode de la `NSTableViewDataSource` classe pour démarrer l’opération de déplacement :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

Cela permet au développeur de fournir un individu `NSDraggingItem` pour chaque élément dans la table qui est glissée par opposition à l’ancienne méthode `WriteRowsWith` qui écrivent des toutes les lignes en un seul groupe à la table de montage.

Lorsque vous travaillez avec `NSCollectionViews`, utilisez à nouveau le `PasteboardWriterForItemAt` méthode par opposition à la `WriteItemsAt` méthode lorsque vous faites glisser commence.

Le développeur doit toujours éviter de placer des fichiers volumineux sur la table de montage. Vous débutez avec macOS Sierra, _promesses du fichier_ permettent au développeur de placer les références donné fichiers sur la table de montage sera effectuée ultérieurement lorsque l’utilisateur termine l’opération de suppression à l’aide de la nouvelle `NSFilePromiseProvider` et `NSFilePromiseReceiver` classes.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>À l’aide du suivi des événements récents

Pour un élément d’Interface utilisateur (comme un `NSButton`) qui a été ajouté à une zone de titre ou une barre d’outils, l’utilisateur doit être en mesure de cliquer sur l’élément et de sorte qu’il déclenche un événement comme normal (par exemple, pour afficher une fenêtre contextuelle). Toutefois, étant donné que l’élément est également dans la zone de titre ou une barre d’outils, l’utilisateur doit être en mesure de cliquez et faites glisser l’élément pour déplacer la fenêtre.

Pour ce faire, dans le code, créez une classe personnalisée pour l’élément (tel que `NSButton`) et remplacez le `MouseDown` événement comme suit :

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

Ce code utilise le `TrackEventsMatching` méthode de la `NSWindow` associé à l’élément d’interface utilisateur d’intercepter le `LeftMouseUp` et `LeftMouseDragged` événements. Pour un `LeftMouseUp` événement, l’élément d’interface utilisateur répond comme d’habitude. Pour le `LeftMouseDragged` événement, l’événement est passé à la `NSWindow`de `PerformWindowDrag` méthode pour déplacer la fenêtre à l’écran.

Appel de la `PerformWindowDrag` méthode de la `NSWindow` classe offre les avantages suivants :

- Il permet la fenêtre à déplacer, même si l’application est bloquée (par exemple lors du traitement d’une boucle approfondie).
- Espace de commutation fonctionnera comme prévu.
- La barre d’espace s’affichera sous la forme normale.
- Fenêtre de l’alignement et l’alignement fonctionnent normalement.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>À l’aide de contrôles conteneur moderne

macOS Sierra fournit les nombreuses améliorations modernes pour les contrôles de vue conteneur existant dans une version antérieure du système d’exploitation.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Améliorations des vues de table

Le développeur doit toujours utiliser le nouveau `NSView` en fonction de la version des contrôles d’affichage conteneur telles que `NSTableView`. Par exemple :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

Cela permet à des Actions personnalisées de ligne de Table à joindre à donné de lignes dans la table (par exemple, le glissement de droite à supprimer la ligne). Pour activer ce comportement, vous devez remplacer le `RowActions` méthode de la `NSTableViewDelegate`:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

La méthode statique `NSTableViewRowAction.FromStyle` est utilisé pour créer une nouvelle Action de ligne de Table des styles suivants :

- `Regular` -Effectue une action non destructives standard telles que modifier lors de contenu de la ligne.
- `Destructive` -Effectue une action destructrice telles que supprimer la ligne de la table. Ces actions seront rendues avec un arrière-plan rouge.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Améliorations des vues de défilement 

Lorsque vous utilisez une vue de défilement (`NSScrollView`) directement, ou en tant que partie d’un autre contrôle (tels que `NSTableView`), le contenu de l’affichage à défilement peut glisser sous les zones de titre et la barre d’outils dans une application Xamarin.Mac à l’aide d’une recherche moderne et des vues.

Par conséquent, le premier élément dans la zone de contenu de défilement peut être partiellement masqué par la zone de titre et la barre d’outils.

Pour corriger ce problème, Apple a ajouté deux nouvelles propriétés à la `NSScrollView` classe :

- `ContentInsets` : Permet au développeur de fournir un `NSEdgeInsets` objet définissant le décalage qui est appliqué en haut de la vue de défilement.
- `AutomaticallyAdjustsContentInsets` -If `true` l’affichage à défilement gère automatiquement la `ContentInsets` pour le développeur.

À l’aide de la `ContentInsets` le développeur peut ajuster le début de l’affichage à défilement pour permettre l’inclusion d’accessoires tels que :

- Un indicateur de tri comme celui illustré dans l’application de messagerie.
- Un champ de recherche.
- Un bouton d’actualisation ou de mise à jour.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Disposition automatique et localisation dans les applications modernes

Apple a inclus plusieurs technologies dans Xcode qui permettent au développeur de créer facilement une application macOS internationaux. Xcode maintenant permet au développeur de séparer le texte de la direction de l’utilisateur à partir de la conception de l’Interface utilisateur de l’application dans ses fichiers de plan conceptuel et fournit des outils pour mettre à jour cette séparation si l’interface utilisateur change.

Pour plus d’informations, consultez le site d’Apple [d’internationalisation et localisation Guide](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Implémentation de Base d’internationalisation 

En implémentant l’internationalisation de Base, le développeur peut fournir un seul fichier de table de montage séquentiel pour représenter l’interface utilisateur de l’application et de séparer toutes les chaînes rencontrées par les utilisateurs. 

Lorsque le développeur crée le fichier de plan conceptuel initial (ou fichiers) qui définissent l’Interface utilisateur de l’application, ils seront générés dans la Base d’internationalisation (la langue que le développeur parle).

Ensuite, le développeur peut exporter les localisations et les chaînes d’internationalisation de Base (dans la conception de l’interface utilisateur de la table de montage séquentiel) qui peuvent être traduites dans plusieurs langues.

Une version ultérieure, ces localisations peuvent être importées et Xcode génère les fichiers de chaîne spécifique au langage pour le plan conceptuel.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Implémentation de la mise en page automatique pour prendre en charge la localisation

Étant donné que les versions localisées de chaîne de valeurs peuvent avoir des tailles très différentes et/ou le sens de lecture, le développeur doit utiliser disposition automatique pour positionner et dimensionner l’Interface utilisateur de l’application dans un fichier d’animation.

Apple suggérer effectuant les opérations suivantes :

- **Supprimer des contraintes de largeur fixe** -toutes les vues de base de texte doivent être autorisés à redimensionner en fonction de leur contenu. Largeur fixe de vue peut rogner le contenu dans des langues spécifiques.
- **Utiliser des tailles de contenu intrinsèque** - par défaut textuel vues seront redimensionnement automatique en fonction de leur contenu. Pour une vue basée sur le texte qui ne sont pas correctement dimensionnement, sélectionnez-les dans le Générateur de Xcode Interface, puis choisissez **modifier** > **taille pour faire tenir le contenu**.
- **Appliquer de début et fin des attributs** - parce que la direction du texte peut changer en fonction de la langue de l’utilisateur, utilisez la nouvelle `Leading` et `Trailing` attributs d’une contrainte par opposition à l’existant `Right` et `Left` attributs. `Leading` et `Trailing` ajuste automatiquement en fonction de la direction de langues.
- **Vues de code confidentiel aux vues adjacents** -ainsi, les vues à repositionnez et redimensionnez les vues autour d’elles changent en réponse à la langue sélectionnée.
- **Ne définissez pas un Minimum de Windows et/ou les tailles maximales** -autoriser les fenêtres pour modifier la taille que la langue sélectionnée redimensionne les zones de contenu.
- **Test de mise en page modifications en permanence** - pendant le développement à l’application doit être testé en permanence dans différentes langues. Consultez d’Apple [application de test de votre Internationalized](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) documentation pour plus de détails.
- **Utilisez NSStackViews pour le code confidentiel vues ensemble**  -  `NSStackViews` permet leur contenu à la touche MAJ enfoncée et augmenter de manière prévisible et le contenu de modifier la taille en fonction de la langue sélectionnée.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Localisation dans le Générateur de Xcode Interface

Apple a fourni plusieurs fonctionnalités dans le Générateur de Xcode Interface que le développeur peut utiliser lors de la création ou la modification de l’interface utilisateur d’une application pour prendre en charge la localisation. Le **l’orientation du texte** section de la **inspecteur de l’attribut** permet au développeur de fournir des indications sur la direction doit être utilisée et mis à jour dans un vue basée sur le texte, sélectionnez (tel que `NSTextField`) :

[![](modern-cocoa-apps-images/content10.png "Les options de l’orientation du texte")](modern-cocoa-apps-images/content10.png#lightbox)

Il existe trois valeurs possibles pour le **l’orientation du texte**:

- **Naturelle** -la disposition est basée sur la chaîne assignée au contrôle.
- **De gauche à droite** -la disposition est toujours forcée de gauche à droite.
- **De droite à gauche** -la disposition est toujours forcée de droite à gauche.

Il existe deux valeurs possibles pour le **disposition**:

- **De gauche à droite** -la disposition est toujours de gauche à droite.
- **De droite à gauche** -la disposition est toujours de droite à gauche.

En général, celles-ci ne doivent pas être modifiées, sauf si un alignement spécifique est requis.

Le **miroir** propriété indique au système pour retourner les propriétés de contrôle spécifique (par exemple, la Position d’Image de cellule). Il a trois valeurs possibles :

- **Automatiquement** -la position change automatiquement en fonction de la direction de la langue sélectionnée.
- **Dans l’Interface de droite à gauche** -sera modifiée la position de droite à gauche des langues en fonction.
- **Jamais** -la position ne changera jamais.

Si le développeur a spécifié **Center**, **Justify** ou **complète** alignement sur le contenu d’une vue basée sur le texte, ces jamais sera retournée en fonction de langue sélectionnée.

Avant macOS Sierra, les contrôles créés dans le code ne seraient pas être automatiquement mises en miroir. Le développeur devait utiliser le code comme suit pour gérer la mise en miroir :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

Où les `Alignment` et `ImagePosition` sont définies selon le `UserInterfaceLayoutDirection` du contrôle.

macOS Sierra ajoute plusieurs nouveaux constructeurs commodité (via la méthode statique `CreateButton` méthode) qui prennent plusieurs paramètres (tels que le titre, l’Image et Action) et automatiquement reflète correctement. Par exemple :

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>À l’aide des apparences de système

Les applications modernes macOS peuvent adopter une nouvelle apparence Interface sombre qui fonctionne bien pour les applications de création, modification ou une présentation image :

[![](modern-cocoa-apps-images/content11.png "Un exemple d’une interface utilisateur de fenêtre Mac foncé")](modern-cocoa-apps-images/content11.png#lightbox)

Cela est possible en ajoutant une ligne de code avant que la fenêtre s’affiche. Par exemple :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

La méthode statique `GetAppearance` méthode de la `NSAppearance` classe est utilisée pour obtenir une apparence nommée à partir du système (dans ce cas `NSAppearance.NameVibrantDark`).

Apple a les suggestions suivantes pour l’utilisation des aspects du système :

- Préférez des couleurs nommées à des valeurs codées en dur (tel que `LabelColor` et `SelectedControlColor`).
- Utiliser le style de contrôle standard système lorsque cela est possible.

Une application de macOS qui utilise les apparences du système sera automatiquement fonctionnent correctement pour les utilisateurs qui ont activé les fonctionnalités d’accessibilité de l’application de vos préférences système. Par conséquent, Apple suggère que le développeur doit toujours utiliser des apparences de système dans leurs applications macOS.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Conception d’interfaces utilisateur avec des plans conceptuels

Storyboards permettent au développeur de non seulement de flux de l’interface utilisateur de l’application, mais pour visualiser et concevoir l’interface utilisateur dont les éléments individuels de conception et de la hiérarchie des éléments donnés.

Contrôleurs permettent au développeur de regrouper des éléments dans une unité de composition et Segues abstraite et supprimer le classique « collez code » requis pour vous déplacer dans la hiérarchie :

[![](modern-cocoa-apps-images/content12.png "Modification de l’interface utilisateur dans le Générateur de Xcode Interface")](modern-cocoa-apps-images/content12.png#lightbox)

Pour plus d’informations, consultez notre [Introduction à des plans conceptuels](~/mac/platform/storyboards/index.md) documentation.

Il existe de nombreux cas où une scène donnée définie dans un plan conceptuel nécessitera des données à partir d’une scène précédente dans la hiérarchie. Apple a les suggestions suivantes pour passer des informations entre les scènes :

- Dépendances de données doivent toujours en cascade vers le bas dans la hiérarchie.
- Évitez les dépendances structurelles de coder en dur l’interface utilisateur, comme cela limite la flexibilité de l’interface utilisateur.
- Utiliser les Interfaces c# pour fournir les dépendances de données génériques.

Le contrôleur de vue qui agit en tant que source de la Segue, peuvent remplacer la `PrepareForSegue` méthode et effectuez toute initialisation requise (par exemple, le passage de données) avant le Segue est exécutée pour afficher le contrôleur de la vue de destination. Par exemple :

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Pour plus d’informations, consultez notre [Segues](~/mac/platform/storyboards/indepth.md#Segues) documentation.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Propagation des Actions

En fonction de la conception de l’application macOS, il peut arriver lorsque le meilleures Gestionnaire d’une Action sur un contrôle d’interface utilisateur peut être dans un autre emplacement dans la hiérarchie de l’interface utilisateur. C’est le cas des Menus et des éléments de Menu qui se trouvent dans leur propre scène, distincte du reste de l’interface utilisateur de l’application.

Pour gérer cette situation, le développeur peut créer une Action personnalisée et passer l’Action en haut de la chaîne de répondeur. Pour plus d’informations, veuillez consulter notre [l’utilisation d’Actions de fenêtre personnalisée](~/mac/user-interface/menu.md) documentation.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Fonctionnalités Mac moderne

Apple a inclus plusieurs fonctionnalités de la direction de l’utilisateur dans macOS Sierra qui permettent au développeur de tirer parti de la plateforme Mac, de telles que :

- **NSUserActivity** -Cela permet à l’application décrire l’activité de l’utilisateur est actuellement impliqué dans. `NSUserActivity` a été initialement créé pour prendre en charge la remise, où une activité a démarré sur l’un des périphériques de l’utilisateur a récupéré et continue sur un autre périphérique. `NSUserActivity` fonctionne de la même dans macOS tel qu’il effectue dans iOS, consultez notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) documentation iOS pour plus de détails.
- **Siri sur Mac** -Siri utilise l’activité en cours (`NSUserActivity`) pour fournir un contexte pour les commandes d’un utilisateur peut émettre.
- **Restauration de l’état** : quand l’utilisateur se ferme une application puis plus tard et macOS relaunches il, l’application sera automatiquement être renvoyé à son état précédent. Le développeur peut utiliser l’API d’état de restauration pour encoder et restaurer des états transitoires de l’interface utilisateur avant l’affichage de l’Interface utilisateur à l’utilisateur. Si l’application est `NSDocument` basée, restauration de l’état est gérée automatiquement. Pour activer la restauration de l’état de non -`NSDocument` basée sur les applications, définissez la `Restorable` de la `NSWindow` classe `true`.
- **Documents dans le Cloud** -avant macOS Sierra, une application devait explicitement opter pour l’utilisation de documents dans iCloud de l’utilisateur lecteur. Dans macOS Sierra l’utilisateur **Desktop** et **Documents** dossiers peuvent être synchronisées avec leur iCloud lecteur automatiquement par le système. Par conséquent, les copies locales de documents peuvent être supprimés pour libérer de l’espace sur l’ordinateur de l’utilisateur. `NSDocument` applications gère automatiquement ce changement. Tous les autres types d’application vous devrez utiliser un `NSFileCoordinator` pour lire et écrire des documents de synchronisation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert plusieurs conseils, fonctionnalités et techniques qu'un développeur peut utiliser pour générer une application moderne macOS dans Xamarin.Mac.



## <a name="related-links"></a>Liens associés

- [Exemples de macOS](https://developer.xamarin.com/samples/mac/)
