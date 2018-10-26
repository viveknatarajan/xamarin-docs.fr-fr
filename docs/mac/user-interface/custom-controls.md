---
title: Création de contrôles personnalisés dans Xamarin.Mac
description: Ce document décrit comment créer des contrôles personnalisés dans Xamarin.Mac. Il montre comment générer le contrôle personnalisé, le suivi de son état, dessiner son interface, répondre aux entrées d’utilisateur et utiliser le contrôle dans une application.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 7fde60d48c23bc48ce1602a0643a3af8ad492ec6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104009"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Création de contrôles personnalisés dans Xamarin.Mac

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès au même utilisateur qui contrôle un développeur travaillant dans *Objective-C*, *Swift* et *Xcode* est . Comme Xamarin.Mac s’intègre directement à Xcode, vous pouvez utiliser de Xcode _Interface Builder_ pour créer et tenir à jour vos contrôles utilisateur (ou éventuellement pour les créer directement en code c#).

Bien que macOS offre une multitude de contrôles utilisateur intégrés, il peut arriver que vous devez créer un contrôle personnalisé pour fournir la fonctionnalité ne fournie pas out-of-the-box ou pour faire correspondre un thème personnalisé de l’interface utilisateur (par exemple, une interface de jeu).

[![](custom-controls-images/intro01.png "Exemple d’un contrôle d’interface utilisateur personnalisé")](custom-controls-images/intro01.png#lightbox)

Dans cet article, nous traiterons les notions de base de la création d’un contrôle d’Interface utilisateur personnalisés réutilisables dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous utilisons dans cet article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, il explique le `Register` et `Export` commandes utilisé pour AutoEventWireup vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Introduction aux contrôles personnalisés

Comme indiqué ci-dessus, il peut arriver que vous deviez créer un réutilisable, contrôle d’Interface utilisateur personnalisée pour fournir des fonctionnalités uniques pour l’interface utilisateur de votre application Xamarin.Mac ou pour créer un thème de l’interface utilisateur personnalisé (par exemple, une interface de jeu).

Dans ces situations, vous pouvez hériter facilement `NSControl` et créer un outil personnalisé qui peut être ajouté à l’interface utilisateur de votre application par le biais de code c# ou par le biais Interface Builder de Xcode. En héritant de `NSControl` votre contrôle personnalisé auront automatiquement toutes les fonctionnalités standards a un contrôle d’Interface utilisateur intégrés (tels que `NSButton`).

Si votre contrôle personnalisé de l’Interface utilisateur affiche simplement les informations (telles qu’un graphiques personnalisés et un outil graphique), vous souhaiterez héritent `NSView` au lieu de `NSControl`.

Quelle que soit la classe de base qui est utilisé, les étapes de base pour la création d’un contrôle personnalisé est le même.

Dans cet article a, créer un composant de retourner le commutateur personnalisé qui fournit un thème de l’Interface utilisateur unique et un exemple de la création d’un contrôle d’Interface utilisateur personnalisé entièrement fonctionnel.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Création du contrôle personnalisé

Dans la mesure où le contrôle personnalisé, nous allons créer répondra à une entrée utilisateur (clics de bouton gauche de la souris), nous allons héritent `NSControl`. De cette façon, notre contrôle personnalisé sera automatiquement toutes les fonctionnalités standard a un contrôle d’Interface utilisateur intégrés et répondent comme un contrôle standard macOS.

Dans Visual Studio pour Mac, ouvrez le projet Xamarin.Mac que vous souhaitez créer un contrôle d’Interface utilisateur personnalisée pour (ou créez-en un). Ajoutez une nouvelle classe et appelez-le `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Ajout d’une nouvelle classe")](custom-controls-images/custom01.png#lightbox)

Ensuite, modifiez la `NSFlipSwitch.cs` classe et qu’elle ressemble à ce qui suit :

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

La première chose à remarquer concernant notre classe personnalisée dans la mesure où nous avons héritant de `NSControl` et à l’aide de la **inscrire** commande pour exposer cette classe à Objective-C et de Xcode Interface Builder :

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

Dans les sections suivantes, nous allons examiner le reste du code ci-dessus en détail.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Suivi de l’état du contrôle

Étant donné que notre contrôle personnalisé est un commutateur, nous avons besoin d’un moyen de suivre l’état de marche/arrêt du commutateur. Nous gérons que par le code suivant dans `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

Lorsque l’état du commutateur change, nous avons besoin d’un moyen de mise à jour de l’interface utilisateur. Cela en forçant le contrôle à redessiner son interface utilisateur avec `NeedsDisplay = true`.

Si notre contrôle plus que nécessaire un seul état (par exemple un commutateur plusieurs état avec 3 positions) activé/désactivé, nous pourrions avoir utilisé un **Enum** pour effectuer le suivi de l’état. Dans notre exemple, une simple **bool** effectuera.

Nous avons également ajouté une méthode d’assistance pour remplacer l’état du commutateur entre et désactiver :

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Plus tard, nous développerons cette classe d’assistance pour informer l’appelant lorsque l’état de commutateurs a changé.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>L’Interface du contrôle de dessin

Nous allons utiliser les routines de dessin Core graphiques pour dessiner l’Interface utilisateur de notre contrôle personnalisé lors de l’exécution. Avant que nous pouvons faire cela, nous devons activer couches pour notre contrôle. Nous faisons cela avec la méthode privée suivante :

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Cette méthode est appelée à partir de chacun des constructeurs du contrôle pour vous assurer que le contrôle est correctement configuré. Exemple :

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Ensuite, nous devons remplacer le `DrawRect` (méthode) et ajoutez les routines Core graphiques pour dessiner le contrôle :

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Nous allons ajuster la représentation visuelle du contrôle lorsque son état est modifié (tels qu’allant **sur** à **hors**). Dès que les modifications d’état, nous pouvons utiliser le `NeedsDisplay = true` pour forcer le contrôle à redessiner pour le nouvel état.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Répondre aux entrées utilisateur

Il existe deux méthodes de base que nous pouvons ajouter l’entrée utilisateur et notre contrôle personnalisé : **remplacer les Routines de gestion de la souris** ou **modules de reconnaissance de mouvement**. Quelle méthode que nous utilisons, doit reposer sur les fonctionnalités requises par notre contrôle.

> [!IMPORTANT]
> Pour n’importe quel contrôle personnalisé que vous créez, vous devez utiliser soit **substituer les méthodes** _ou_ **modules de reconnaissance de mouvement**, mais pas les deux en même temps qu’ils peuvent entrer en conflit entre eux.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Gestion des entrées d’utilisateur avec les méthodes Override

Objets qui héritent de `NSControl` (ou `NSView`) possède plusieurs substituer des méthodes pour la gestion de la souris ou clavier d’entrée. Pour notre exemple de contrôle, nous souhaitons retourner l’état du commutateur entre **sur** et **hors** lorsque l’utilisateur clique sur le contrôle avec le bouton gauche de la souris. Nous pouvons ajouter les éléments suivants substituer des méthodes pour la `NSFliwSwitch` classe gérer cette situation :

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

Dans le code ci-dessus, nous appelons le `FlipSwitchState` (méthode) (défini ci-dessus) pour retourner la On/Off d’état du commutateur dans le `MouseDown` (méthode). Cela forcera également le contrôle soit redessiné pour refléter l’état actuel.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Gestion des entrées d’utilisateur avec des modules de reconnaissance de mouvement

Si vous le souhaitez, vous pouvez utiliser des modules de reconnaissance de mouvement pour gérer l’utilisateur interagit avec le contrôle. Supprimer les actions prioritaires ajoutées ci-dessus, modifiez le `Initialize` (méthode) et qu’elle ressemble à ce qui suit :

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Ici, nous créons un nouveau `NSClickGestureRecognizer` et en appelant notre `FlipSwitchState` méthode pour modifier l’état de son lorsque l’utilisateur clique dessus avec le bouton gauche de la souris. Le `AddGestureRecognizer (click)` méthode ajoute la reconnaissance de mouvement au contrôle.

Là encore, la méthode que nous utilisons dépend de ce que nous essayons d’accomplir avec notre contrôle personnalisé. Si nous avons besoin de faible niveau d’accès à une interaction utilisateur, utilisez les méthodes à substituer. Si nous avons besoin de fonctionnalités prédéfinies, telles que des clics de souris, utiliser les modules de reconnaissance de mouvement.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Répondre aux événements de changement d’état

Lorsque l’utilisateur modifie l’état de notre contrôle personnalisé, nous avons besoin d’un moyen pour répondre à la modification d’état dans le code (par exemple faire quelque chose lorsque clique sur un bouton personnalisé). 

Pour fournir cette fonctionnalité, vous devez modifier le `NSFlipSwitch` classe et ajoutez le code suivant :

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Ensuite, modifiez le `FlipSwitchState` (méthode) et qu’elle ressemble à ce qui suit :

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Tout d’abord, nous fournissons un `ValueChanged` événement que nous pouvons ajouter un gestionnaire de code c# afin que nous pouvons effectuer une action lorsque l’utilisateur modifie l’état du commutateur.

Ensuite, étant donné que notre contrôle personnalisé hérite de `NSControl`, elle a automatiquement un **Action** qui peuvent être attribuées d’Interface Builder de Xcode. Pour appeler cette **Action** lorsque l’état change, nous utilisons le code suivant :

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Tout d’abord, nous vérifions si une **Action** a été assigné au contrôle. Ensuite, nous appelons le **Action** si elle a été définie.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Utilisation du contrôle personnalisé

Grâce au contrôle personnalisé entièrement défini, nous pouvons soit l’ajouter à l’interface utilisateur de notre application Xamarin.Mac à l’aide de code c# ou dans l’Interface Builder de Xcode.

Pour ajouter le contrôle à l’aide du Générateur d’Interface, d’abord effectuer un nettoyage de build du projet Xamarin.Mac, puis double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir pour modification dans l’Interface Builder :

[![](custom-controls-images/custom02.png "Modification de la table de montage séquentiel dans Xcode")](custom-controls-images/custom02.png#lightbox)

Ensuite, faites glisser un `Custom View` dans la conception de l’Interface utilisateur :

[![](custom-controls-images/custom03.png "Sélection d’une vue personnalisée à partir de la bibliothèque")](custom-controls-images/custom03.png#lightbox)

La vue personnalisée étant toujours sélectionné, basculez vers le **inspecteur d’identité** et modifier l’affichage **classe** à `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Définition de la classe de vue")](custom-controls-images/custom04.png#lightbox)

Basculez vers le **éditeur de l’Assistant** et créer un **Outlet** pour le contrôle personnalisé (veillant à lier dans le `ViewControler.h` fichier et non le `.m` fichier) :

[![](custom-controls-images/custom05.png "Configuration d’un nouveau magasin")](custom-controls-images/custom05.png#lightbox)

Enregistrez vos modifications, retournez dans Visual Studio pour Mac et autoriser les modifications à synchroniser. Modifier le `ViewController.cs` et effectuez le `ViewDidLoad` méthode ressemble ce qui suit :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

Ici, nous répondons à la `ValueChanged` événement défini plus tôt sur les `NSFlipSwitch` classe et l’écriture en cours **valeur** lorsque l’utilisateur clique sur le contrôle.

Si vous le souhaitez, nous pouvons revenir à Interface Builder et définir un **Action** sur le contrôle :

[![](custom-controls-images/custom06.png "Configuration d’une nouvelle Action")](custom-controls-images/custom06.png#lightbox)

Là encore, modifier le `ViewController.cs` fichier, puis ajoutez la méthode suivante :

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Vous devez utiliser soit le **événement** ou définir un **Action** dans Interface Builder, mais vous ne devez pas utiliser les deux méthodes simultanément ou ils peuvent entrer en conflit entre eux.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail la création d’un contrôle d’Interface utilisateur personnalisés réutilisables dans une application Xamarin.Mac. Nous avons vu comment dessiner les contrôles personnalisés l’interface utilisateur, les deux méthodes principales pour répondre aux entrées d’utilisateur et de la souris et exposer le nouveau contrôle à des Actions dans l’Interface Builder de Xcode.

## <a name="related-links"></a>Liens associés

- [MacCustomControl (exemple)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Gestion des événements de souris](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
