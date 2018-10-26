---
title: Création d’applications macOS modernes
description: Cet article décrit plusieurs conseils, fonctionnalités et techniques qu'un développeur peut utiliser pour créer une application de macOS modernes dans Xamarin.Mac.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 53bfc9f147b6cf369b8f5ce8d1257dbaf6b0f807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113519"
---
# <a name="building-modern-macos-apps"></a>Création d’applications macOS modernes

_Cet article décrit plusieurs conseils, fonctionnalités et techniques qu'un développeur peut utiliser pour créer une application de macOS modernes dans Xamarin.Mac._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Création de recherche modernes avec des vues modernes

Une apparence moderne inclura une apparence de la fenêtre et la barre d’outils moderne tels que l’exemple d’application indiqué ci-dessous :

[![](modern-cocoa-apps-images/content08.png "Un exemple d’une interface utilisateur des applications Mac moderne")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>L’activation intégral en taille réelle des vues de contenu

Pour faire cette recherche dans une application Xamarin.Mac, le développeur souhaiterez utiliser un _affichage de contenu complet taille_, ce qui signifie que le contenu s’étend dans les zones de l’outil et de la barre de titre et sera automatiquement floue par macOS.

Pour activer cette fonctionnalité dans le code, créez une classe personnalisée pour le `NSWindowController` et qu’elle ressemble à ce qui suit :

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

Cette fonctionnalité peut également être activée dans l’Interface Builder de Xcode en sélectionnant la fenêtre, cochez **affichage du contenu en taille réelle complète**:

[![](modern-cocoa-apps-images/content01.png "Modification du storyboard principal dans l’Interface Builder de Xcode")](modern-cocoa-apps-images/content01.png#lightbox)

Lorsque vous utilisez une vue de contenu de taille complète, le développeur peut disposer décaler le contenu sous les zones de barre de titre et l’outil afin que le contenu spécifique (par exemple, les étiquettes) ne diapositive sous les.

Pour compliquer ce problème, les zones de titre et la barre d’outils peuvent avoir une hauteur dynamique en fonction de l’action de l’utilisateur est en train de, la version de macOS que l’utilisateur a installé et/ou le matériel Mac que l’application est en cours d’exécution.

Par conséquent, le décalage de codage simplement dur lorsque vous présentez à l’Interface utilisateur ne fonctionnera pas. Le développeur doit adopter une approche dynamique.

Apple a inclus la [clé-valeur Observable](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` propriété de la `NSWindow` classe pour obtenir la zone de contenu actuel dans le code. Le développeur peut utiliser cette valeur pour positionner manuellement les éléments requis lorsque la zone de contenu est modifié.

La meilleure solution consiste à utiliser les Classes de taille et de disposition automatique pour positionner les éléments d’interface utilisateur dans le code ou d’Interface Builder.

Code comme dans l’exemple suivant peut être utilisé pour positionner des éléments d’interface utilisateur à l’aide des Classes de taille et de mise en forme automatique dans le contrôleur d’affichage de l’application :

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

Ce code crée le stockage pour une contrainte principaux qui est appliqué à une étiquette (`ItemTitle`) pour vous assurer qu’il ne glisse sous la zone de titre et la barre d’outils :

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

En substituant le contrôleur d’affichage `UpdateViewConstraints` (méthode), le développeur peut tester pour voir si la contrainte nécessaire a déjà été générée et créer si nécessaire.

Si une nouvelle contrainte doit être généré, le `ContentLayoutGuide` propriété de la fenêtre de contrôle qui doit être contrainte est accessibles et effectuer une conversion en un `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

La propriété TopAnchor de la `NSLayoutGuide` est accessible et s’il est disponible, il est utilisé pour créer une nouvelle contrainte avec la quantité de décalage souhaitée et la nouvelle contrainte devient active pour l’appliquer :

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>L’activation de barres d’outils rationalisés

Un fenêtre de macOS normal inclut une norme de que barre de titre en s’exécute le long au bord supérieur de la fenêtre. Si la fenêtre inclut également une barre d’outils, il s’affichera sous cette zone de barre de titre :

[![](modern-cocoa-apps-images/content02.png "Une barre d’outils Mac standard")](modern-cocoa-apps-images/content02.png#lightbox)

Lorsque disparaît de la zone de titre à l’aide d’une barre d’outils rationalisé, et remonte dans la barre d’outils dans sa position de la barre de titre, en ligne avec les boutons de la fenêtre Fermer, réduire et agrandir :

[![](modern-cocoa-apps-images/content03.png "Une barre d’outils Mac rationalisée")](modern-cocoa-apps-images/content03.png#lightbox)

La barre d’outils simplifiée est activée en substituant le `ViewWillAppear` méthode de la `NSViewController` et rend la recherche comme suit :

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Cet effet est généralement utilisé pour _Shoebox Applications_ (applications d’une fenêtre) comme cartes, calendrier, Notes et préférences système. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>À l’aide de contrôleurs d’affichage accessoire

Selon la conception de l’application, le développeur peut souhaiter également compléter la barre de titre zone avec un contrôleur d’affichage accessoire qui s’affiche juste en dessous de la zone de barre de titre/outil fournir sensible au contexte de contrôles à l’utilisateur en fonction de l’activité qu’ils sont en cours :

[![](modern-cocoa-apps-images/content04.png "Un exemple de contrôleur d’affichage accessoire")](modern-cocoa-apps-images/content04.png#lightbox)

Le contrôleur d’affichage d’accessoire automatiquement être floues et redimensionné par le système sans intervention des développeurs.

Pour ajouter un contrôleur d’affichage accessoire, procédez comme suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Main.storyboard` pour l’ouvrir et le modifier.
2. Faites glisser un **contrôleur d’affichage personnalisé** dans la hiérarchie de la fenêtre : 

    [![](modern-cocoa-apps-images/content05.png "Ajoutez un nouveau contrôleur d’affichage personnalisé")](modern-cocoa-apps-images/content05.png#lightbox)
3. De disposition de la vue accessoire l’interface utilisateur : 

    [![](modern-cocoa-apps-images/content06.png "Conception de la nouvelle vue")](modern-cocoa-apps-images/content06.png#lightbox)
4. Exposer la vue accessoire comme un **Outlet** et tout autre **Actions** ou **Outlets** pour son interface utilisateur : 

    [![](modern-cocoa-apps-images/content07.png "Ajout de l’OUtlet requis")](modern-cocoa-apps-images/content07.png#lightbox)
5. Enregistrez les modifications.
6. Revenez à Visual Studio pour Mac synchroniser les modifications.

Modifier le `NSWindowController` et qu’elle ressemble à ce qui suit :

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

Les points clés de ce code sont où la vue est définie sur la vue personnalisée qui a été définie dans l’Interface Builder et exposée comme un **Outlet**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

Et le `LayoutAttribute` qui définit _où_ l’accessoire s’affichera :

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Étant donné que macOS est maintenant entièrement localisé, le `Left` et `Right` `NSLayoutAttribute` propriétés ont été déconseillées et doit être remplacées par `Leading` et `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>À l’aide de Windows à onglets

En outre, le système macOS peut-être ajouter des contrôleurs d’affichage accessoire à la fenêtre d’application. Par exemple, pour créer des Windows avec onglets dans lequel plusieurs de Windows l’application sont fusionnées en une seule fenêtre virtuelle :

[![](modern-cocoa-apps-images/content08.png "Un exemple d’une fenêtre à onglets de Mac")](modern-cocoa-apps-images/content08.png#lightbox)

En règle générale, le développeur doit prendre l’utilisation de l’action limitée Windows avec onglets dans leurs applications Xamarin.Mac, le système les gère automatiquement comme suit :

- Windows sera automatiquement à onglets lorsque le `OrderFront` méthode est appelée.
- Windows sera automatiquement Untabbed quand le `OrderOut` méthode est appelée.
- Dans le code de toutes les fenêtres avec onglet sont toujours considérées comme « visibles », toutefois non avant tous les onglets sont masqués par le système à l’aide de CoreGraphics.
- Utilisez le `TabbingIdentifier` propriété du `NSWindow` à Windows de groupe en onglets.
- S’il s’agit une `NSDocument` en application, plusieurs de ces fonctionnalités sera activée automatiquement (par exemple, le bouton plus (+) qui est ajouté à la barre d’onglet) sans aucune intervention du développeur.
- Non -`NSDocument` applications peuvent activer le bouton « plus » dans le groupe d’onglets à ajouter un nouveau document en substituant le `GetNewWindowForTab` méthode de la `NSWindowsController`.

Tous les éléments rassemblant, le `AppDelegate` d’une application qui souhaitait utiliser système basé à onglets Windows se présenterait comme suit :

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

Le `NSWindowController` puis se présenterait :

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

Le code suivant, indique à macOS que l’application préfère utiliser des onglets et fournit une chaîne qui permet à Windows de l’application d’être regroupés en onglets :

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

Et la méthode de remplacement suivante ajoute un bouton plus (+) à la barre d’onglet qui créera un nouveau document lorsque l’utilisateur clique sur :

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>À l’aide de Core Animation

Core Animation est un moteur de rendu graphique puissant qui est intégré à Mac OS. Core Animation a été optimisée pour tirer parti du GPU (Graphics Processing Unit) disponible dans le matériel de macOS modernes plutôt que les opérations graphiques sur l’UC, ce qui peut ralentir l’ordinateur en cours d’exécution.

Le `CALayer`, fournis par Core Animation, peut être utilisé pour des tâches telles que le défilement rapide et fluide et animations. L’Interface utilisateur d’une application doit être composé de plusieurs couches pour tirer pleinement parti d’Animation de base et des sous-vues.

Un `CALayer` objet fournit plusieurs propriétés qui permettent au développeur de contrôler ce qui est présenté à l’écran à l’utilisateur, tels que :

- `Content` -Peut être un `NSImage` ou `CGImage` qui fournit le contenu de la couche.
- `BackgroundColor` : Définit la couleur d’arrière-plan de la couche comme un `CGColor`
- `BorderWidth` : Définit la largeur de bordure.
- `BorderColor` : Définit la couleur de bordure.

Pour utiliser des graphismes de base dans l’interface utilisateur de l’application, il doit être à l’aide de _couche soutenu_ les affichages, Apple suggère que le développeur doit toujours activer dans la fenêtre de contenu. De cette façon, toutes les vues enfants héritent automatiquement de sauvegarde de la couche ainsi.

En outre, les suggestions d’Apple à l’aide de la couche soutenu vues par opposition à l’ajout d’une nouvelle `CALayer` comme une sous-couche, car le système gère automatiquement plusieurs des paramètres requis (tels que ceux requis par un affichage Retina).

Couche de stockage peut être activée en affectant la `WantsLayer` d’un `NSView` à `true` ou à l’intérieur Interface Builder de Xcode sous le **View Effects Inspector** en vérifiant **Core Animation couche**:

[![](modern-cocoa-apps-images/content09.png "L’inspecteur d’effets de vue")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Nouveau dessin de vues avec des couches

Une autre étape importante lors de l’utilisation des vues de soutenu de couche dans une application Xamarin.Mac consiste à définir le `LayerContentsRedrawPolicy` de la `NSView` à `OnSetNeedsDisplay` dans le `NSViewController`. Exemple :

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Si le développeur ne définissez pas cette propriété, la vue est redessinée chaque fois que son origine frame change, ce qui n’est pas souhaité pour des raisons de performances. Avec cette propriété définie sur `OnSetNeedsDisplay` le développeur aura manuellement définir `NeedsDisplay` à `true` pour forcer le contenu à se redessiner, toutefois.

Lorsqu’une vue est marquée comme modifiée, le système vérifie la `WantsUpdateLayer` propriété de la vue. Si elle retourne `true` le `UpdateLayer` méthode est appelée, sinon la `DrawRect` méthode de la vue est appelée pour mettre à jour le contenu.

Apple a un contenu de vues à la demande de mise à jour les suggestions suivantes :

- Apple préfère à l’aide de `UpdateLater` sur `DrawRect` chaque fois que possible, car il fournit une amélioration significative des performances.
- Utiliser le même `layer.Contents` pour les éléments d’interface utilisateur similaires.
- Apple préfère également au développeur de composer leur interface utilisateur à l’aide des affichages standard tels que `NSTextField`, à nouveau chaque fois que possible.

Pour utiliser `UpdateLayer`, créer une classe personnalisée pour la `NSView` et rendre le code de l’aspect suivant :

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

## <a name="using-modern-drag-and-drop"></a>À l’aide de glisser- déplacer moderne

Pour présenter une expérience moderne de glisser -déplacer pour l’utilisateur, le développeur doit adopter _glisser flocage_ de leur application opérations glisser-déplacer. Faites glisser Flocking est où chaque fichier individuel ou un élément glissé initialement apparaît comme un élément individuel qui troupeaux (groupe sous le curseur avec le nombre d’éléments) que l’utilisateur continue l’opération glisser.

Si l’utilisateur arrête l’opération glisser, les éléments individuels seront Unflock et leurs emplacements d’origine.

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

L’effet simulant a été obtenue en envoyant chaque élément glissé vers le `BeginDraggingSession` méthode de la `NSView` comme un élément distinct dans un tableau.

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

Cela permet au développeur de fournir un individu `NSDraggingItem` pour chaque élément dans la table qui est glissée par opposition à l’ancienne méthode `WriteRowsWith` qui écrivent toutes les lignes en un seul groupe à la table de montage.

Lorsque vous travaillez avec `NSCollectionViews`, utilisez à nouveau le `PasteboardWriterForItemAt` (méthode), par opposition à la `WriteItemsAt` méthode quand vous faites glisser commence.

Le développeur doit toujours éviter de placer des fichiers volumineux sur la table de montage. Nouveau dans macOS Sierra, _promesses de fichier_ permettent au développeur de placer des références à selon les fichiers sur la table de montage est alors exécutée ultérieurement lorsque l’utilisateur termine l’opération de déplacement à l’aide de la nouvelle `NSFilePromiseProvider` et `NSFilePromiseReceiver` classes.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Utilisation du suivi des événements moderne

Pour un élément d’Interface utilisateur (comme un `NSButton`) qui a été ajouté à une zone de titre ou la barre d’outils, l’utilisateur doit être en mesure de cliquer sur l’élément et de sorte qu’il déclenche un événement comme d’habitude (par exemple, l’affichage d’une fenêtre contextuelle). Toutefois, étant donné que l’élément est également dans la zone de titre ou la barre d’outils, l’utilisateur doit pouvoir cliquez et faites glisser l’élément pour déplacer la fenêtre.

Pour ce faire, dans le code, créez une classe personnalisée pour l’élément (tel que `NSButton`) et remplacer le `MouseDown` événements comme suit :

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

Ce code utilise le `TrackEventsMatching` méthode de la `NSWindow` l’élément d’interface associé pour intercepter le `LeftMouseUp` et `LeftMouseDragged` événements. Pour un `LeftMouseUp` événement, l’élément d’interface utilisateur répond comme d’habitude. Pour le `LeftMouseDragged` événement, l’événement est passé à la `NSWindow`de `PerformWindowDrag` méthode pour déplacer la fenêtre sur l’écran.

Appel de la `PerformWindowDrag` méthode de la `NSWindow` classe offre les avantages suivants :

- Il permet la fenêtre à déplacer, même si l’application ne répond pas (par exemple quand une boucle approfondie de traitement).
- Espace de commutation fonctionnera comme prévu.
- La barre d’espace s’affichera comme d’habitude.
- Fenêtre de l’alignement et l’alignement fonctionnent normalement.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>À l’aide de contrôles d’affichage conteneur modernes

macOS Sierra offre de nombreuses améliorations modernes pour les contrôles d’affichage conteneur existants disponibles dans la version précédente du système d’exploitation.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Améliorations des vues de table

Le développeur doit toujours utiliser le nouveau `NSView` en fonction de version des contrôles d’affichage conteneur comme `NSTableView`. Exemple :

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

Ainsi, pour les Actions de ligne de Table personnalisées à joindre à un donné de lignes dans la table (par exemple, faire glisser vers la droite pour supprimer la ligne). Pour activer ce comportement, vous devez remplacer le `RowActions` méthode de la `NSTableViewDelegate`:

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

- `Regular` -Effectue une action standard non destructif, par exemple modifier les contenu de la ligne.
- `Destructive` -Exécute une action destructrice telles que supprimer la ligne de la table. Ces actions seront affichera avec un arrière-plan rouge.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Améliorations des vues de défilement 

Lorsque vous utilisez une vue de défilement (`NSScrollView`) directement, ou en tant que partie d’un autre contrôle (tel que `NSTableView`), le contenu de l’affichage du défilement peut glisser sous les zones de titre et de barre d’outils dans une application Xamarin.Mac à l’aide d’une zone de recherche modernes et des vues.

Par conséquent, le premier élément dans la zone de contenu d’affichage du défilement peut être partiellement obscurci par la zone de titre et la barre d’outils.

Pour corriger ce problème, Apple a ajouté deux nouvelles propriétés à la `NSScrollView` classe :

- `ContentInsets` -Permet au développeur de fournir un `NSEdgeInsets` objet définissant le décalage qui est appliqué en haut de l’affichage du défilement.
- `AutomaticallyAdjustsContentInsets` -If `true` l’affichage du défilement gère automatiquement la `ContentInsets` pour le développeur.

À l’aide de la `ContentInsets` le développeur peut ajuster le début de la vue de défilement pour permettre l’inclusion d’accessoires tels que :

- Un indicateur de tri, comme celle illustrée dans l’application de messagerie.
- Un champ de recherche.
- Un bouton d’actualisation ou de mise à jour.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Disposition automatique et la localisation dans les applications modernes

Apple a inclus plusieurs technologies dans Xcode qui permettent au développeur de créer facilement une application internationalisée macOS. Maintenant, Xcode permet au développeur de séparer le texte de visibles par l’utilisateur à partir de la conception d’Interface utilisateur de l’application dans ses fichiers de la table de montage séquentiel et fournit des outils pour maintenir cette séparation si l’interface utilisateur change.

Pour plus d’informations, consultez le site d’Apple [internationalisation et localisation Guide](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Implémentation de Base d’internationalisation 

En implémentant l’internationalisation de Base, le développeur peut fournir un seul fichier de Storyboard pour représenter l’interface utilisateur de l’application et de séparer toutes les chaînes visibles par l’utilisateur. 

Quand le développeur crée le fichier de Storyboard initial (ou fichiers) qui définissent l’Interface utilisateur de l’application, ils seront générés dans la Base d’internationalisation (le langage qui parle le développeur).

Ensuite, le développeur peut exporter des localisations et les chaînes d’internationalisation de Base (dans la conception de l’interface utilisateur de la table de montage séquentiel) qui peuvent être traduites en plusieurs langues.

Une version ultérieure, ces localisations peuvent être importées et Xcode génère les fichiers de chaîne spécifique au langage pour la table de montage séquentiel.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Mise en œuvre de la disposition automatique pour prendre en charge de la localisation

Étant donné que les versions localisées de chaîne de valeurs peuvent avoir des tailles très différentes et/ou le sens de lecture, le développeur doit utiliser disposition automatique pour positionner et dimensionner l’Interface utilisateur de l’application dans un fichier de Storyboard.

Apple suggère procédant comme suit :

- **Supprimer des contraintes de largeur fixe** -toutes les vues de texte doivent être autorisés à redimensionner en fonction de leur contenu. Largeur fixe vue peut-être survenir à leur contenu dans des langues spécifiques.
- **Utiliser les tailles de contenu intrinsèque** - par défaut textuel vues seront redimensionnement automatique en fonction de leur contenu. Pour une vue basée sur le texte qui ne sont pas correctement dimensionnement, sélectionnez-les dans Interface Builder de Xcode, puis choisissez **modifier** > **taille pour faire tenir le contenu**.
- **Appliquer le début et fin des attributs** - parce que la direction du texte peut changer selon la langue de l’utilisateur, utilisez la nouvelle `Leading` et `Trailing` attributs d’une contrainte par opposition à existant `Right` et `Left` attributs. `Leading` et `Trailing` s’ajustent automatiquement en fonction de la direction de langues.
- **Vues de code confidentiel aux vues adjacents** -ainsi, les vues à repositionner et redimensionner comme les vues autour d’elles changent en fonction de la langue sélectionnée.
- **Ne définissez pas un Minimum de Windows et/ou les tailles maximales** -autoriser de Windows pour modifier la taille que la langue sélectionnée est redimensionné à leurs zones de contenu.
- **Test de mise en page modifications en permanence** - pendant développement l’application doit être testée en permanence dans différentes langues. Consultez d’Apple [application test de votre Internationalized](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) documentation pour plus d’informations.
- **Utilisez NSStackViews à épingler des vues ensemble**  -  `NSStackViews` permet leur contenu à déplacer et de croissance de façon prévisible et le contenu de modifier la taille en fonction de la langue sélectionnée.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Localisation dans Interface Builder de Xcode

Apple propose plusieurs fonctionnalités d’Interface Builder de Xcode que le développeur peut utiliser lors de la création ou la modification de l’interface utilisateur d’une application pour prendre en charge la localisation. Le **l’orientation du texte** section de la **inspecteur d’attributs** permet au développeur de fournir des indications sur la direction doit être utilisée et mis à jour sur un affichage textuel, sélectionnez (tel que `NSTextField`) :

[![](modern-cocoa-apps-images/content10.png "Les options de l’orientation du texte")](modern-cocoa-apps-images/content10.png#lightbox)

Il existe trois valeurs possibles pour le **l’orientation du texte**:

- **Naturelle** -la disposition est basée sur la chaîne assignée au contrôle.
- **De gauche à droite** -la disposition est toujours obligée de gauche à droite.
- **De droite à gauche** -la disposition est toujours obligée de droite à gauche.

Il existe deux valeurs possibles pour le **disposition**:

- **De gauche à droite** -la disposition est toujours de gauche à droite.
- **De droite à gauche** -la disposition est toujours de droite à gauche.

En général, il ne doivent pas être modifiées, sauf si un alignement spécifique est requis.

Le **miroir** propriété indique au système pour retourner les propriétés de contrôle spécifique (par exemple, la Position d’Image de cellule). Il possède trois valeurs possibles :

- **Automatiquement** -la position est automatiquement modifié en fonction de la direction de la langue sélectionnée.
- **Dans l’Interface de droite à gauche** -la position sera modifiée de droite à gauche des langues en fonction.
- **Jamais** -la position ne changera jamais.

Si le développeur a spécifié **Center**, **Justify** ou **complète** alignement sur le contenu d’un affichage textuel, ceux-ci ne sera jamais retournée langue en fonction sélectionnée.

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

macOS Sierra ajoute plusieurs nouveaux constructeurs de commodité (via la méthode statique `CreateButton` méthode) qui prennent plusieurs paramètres (par exemple, le titre, l’Image et Action) et automatiquement reflète correctement. Exemple :

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>À l’aide des apparences de système

Applications macOS modernes peuvent adopter une nouvelle apparence Interface foncé qui fonctionne bien pour les applications de création, modification ou de présentation image :

[![](modern-cocoa-apps-images/content11.png "Un exemple d’une interface utilisateur de fenêtre de Mac foncé")](modern-cocoa-apps-images/content11.png#lightbox)

Cela est possible en ajoutant une ligne de code avant que la fenêtre s’affiche. Exemple :

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

Apple a les suggestions suivantes pour l’utilisation d’apparences du système :

- Préférer les couleurs nommées de valeurs codées en dur (tel que `LabelColor` et `SelectedControlColor`).
- Utiliser le style de contrôle standard système lorsque cela est possible.

Une application macOS qui utilise les apparences du système automatiquement fonctionnera correctement pour les utilisateurs qui ont activé les fonctionnalités d’accessibilité de l’application Préférences système. Par conséquent, Apple suggère que le développeur doit toujours utiliser des apparences de système dans leurs applications macOS.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Conception d’interfaces utilisateur avec des Storyboards

Storyboards permettent au développeur non seulement de flux de l’interface utilisateur de l’application, mais pour visualiser et concevoir l’interface utilisateur dont les éléments individuels de conception et de la hiérarchie des éléments donnés.

Contrôleurs permettent au développeur de regroupe des éléments dans une unité de composition et Segues abstraite et supprimer le classique » de type glue code » nécessaires pour déplacer tout au long de la hiérarchie d’affichage :

[![](modern-cocoa-apps-images/content12.png "Modification de l’interface utilisateur dans l’Interface Builder de Xcode")](modern-cocoa-apps-images/content12.png#lightbox)

Pour plus d’informations, consultez notre [Introduction aux Storyboards](~/mac/platform/storyboards/index.md) documentation.

Il existe de nombreux cas où une scène donnée définie dans une table de montage séquentiel nécessitera des données à partir d’une scène précédente dans la hiérarchie d’affichage. Apple a les suggestions suivantes pour passer des informations entre les scènes :

- Dépendances de données doivent toujours en cascade vers le bas dans la hiérarchie.
- Évitez les dépendances structurelles de coder en dur l’interface utilisateur, car cela limite la flexibilité de l’interface utilisateur.
- Utilisez C# Interfaces pour fournir les dépendances de données génériques.

Le contrôleur d’affichage qui joue le rôle de la source du Segue, peuvent remplacer la `PrepareForSegue` (méthode) et effectuer toute initialisation nécessaire (par exemple, le passage de données) avant le Segue est exécutée pour afficher la contrôleur d’affichage de la cible. Exemple :

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

Selon la conception de l’application macOS, il peut arriver lorsque le meilleure gestionnaire pour une Action sur un contrôle d’interface utilisateur peut être dans un autre emplacement dans la hiérarchie de l’interface utilisateur. C’est le cas des Menus et des éléments de Menu qui se trouvent dans leur propre scène, distincte du reste de l’interface utilisateur de l’application.

Pour gérer cette situation, le développeur peut créer une Action personnalisée et passer de l’Action de la chaîne de répondeur. Pour plus d’informations, consultez notre [l’utilisation d’Actions de fenêtre personnalisée](~/mac/user-interface/menu.md) documentation.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Fonctionnalités modernes Mac

Apple a inclus plusieurs fonctionnalités orientées utilisateur dans macOS Sierra qui permettent au développeur de tirer parti de la plateforme Mac, tel que :

- **NSUserActivity** -Cela permet à l’application décrire l’activité de l’utilisateur est impliqué dans. `NSUserActivity` a été initialement créé pour prendre en charge le transfert de travail, où une activité démarrée sur l’un des périphériques de l’utilisateur peut être sélectionnée et continue sur un autre appareil. `NSUserActivity` Par conséquent, fonctionne de la même dans macOS comme il le fait dans iOS, consultez notre [Introduction à la procédure de transfert](~/ios/platform/handoff.md) documentation iOS pour plus d’informations.
- **Siri sur Mac** -Siri utilise l’activité en cours (`NSUserActivity`) pour fournir un contexte aux commandes un utilisateur peut émettre.
- **Restauration d’état** : quand l’utilisateur se ferme une application sur Mac OS et puis plus tard redémarrage il, l’application sera automatiquement renvoyé à son état précédent. Le développeur peut utiliser l’API de restauration d’état pour encoder et restaurer des États de l’interface utilisateur temporaires avant que l’Interface utilisateur est affiché à l’utilisateur. Si l’application est `NSDocument` en, restauration de l’état est gérée automatiquement. Pour activer la restauration de l’état de non -`NSDocument` basé sur les applications, définissez le `Restorable` de la `NSWindow` classe `true`.
- **Documents dans le Cloud** -avant macOS Sierra, une application devait explicitement participer à l’utilisation de documents dans iCloud de l’utilisateur lecteur. Dans macOS Sierra l’utilisateur **Desktop** et **Documents** dossiers peuvent être synchronisées avec leur iCloud Drive automatiquement par le système. Par conséquent, les copies locales des documents peuvent être supprimées pour libérer de l’espace sur l’ordinateur de l’utilisateur. `NSDocument` applications gère automatiquement ce changement. Tous les autres types d’application devez utiliser un `NSFileCoordinator` pour synchroniser la lecture et l’écriture de documents.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté plusieurs conseils, fonctionnalités et techniques qu'un développeur peut utiliser pour créer une application de macOS modernes dans Xamarin.Mac.



## <a name="related-links"></a>Liens associés

- [Exemples de macOS](https://developer.xamarin.com/samples/mac/)
