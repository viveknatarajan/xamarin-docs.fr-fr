---
title: Création de contrôles personnalisés
description: Cet article décrit comment créer des contrôles personnalisés et les utiliser dans le constructeur d’Interface.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e1ab3977df96e241fa2a5a80f6cabd74d7d775f8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-custom-controls"></a>Création de contrôles personnalisés

_Cet article décrit comment créer des contrôles personnalisés et les utiliser dans le constructeur d’Interface._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès au même utilisateur qui contrôle un développeur travaillant dans *Objective-C*, *Swift* et *Xcode* est . Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos contrôles utilisateur (ou éventuellement de les créer directement dans le code C#).

Alors que macOS fournit une multitude de contrôles utilisateur intégrés, il peut arriver que vous devez créer un contrôle personnalisé pour fournir la fonctionnalité ne fournie pas out of box ou pour correspondre à un thème personnalisé de l’interface utilisateur (par exemple, une interface de jeu).

[![](custom-controls-images/intro01.png "Exemple d’un contrôle d’interface utilisateur personnalisé")](custom-controls-images/intro01.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de la création d’un contrôle d’Interface utilisateur personnalisés réutilisables dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Introduction aux contrôles personnalisés

Comme indiqué ci-dessus, il peut arriver que vous deviez créer un réutilisable, contrôle d’Interface utilisateur personnalisée pour fournir des fonctionnalités uniques pour l’interface utilisateur de votre application Xamarin.Mac ou pour créer un thème personnalisé de l’interface utilisateur (par exemple, une interface de jeu).

Dans ces situations, vous pouvez hériter facilement `NSControl` et créer un outil personnalisé qui peut être ajouté à l’interface utilisateur de votre application via le code c# ou par l’intermédiaire Interface Builder de Xcode. En héritant de `NSControl` votre contrôle personnalisé aura automatiquement toutes les fonctionnalités standards qui dispose d’un contrôle d’Interface utilisateur intégrés (tels que `NSButton`).

Si votre contrôle personnalisé de l’Interface utilisateur affiche simplement des informations (par exemple, un graphique personnalisé et un outil graphique), vous pouvez souhaiter hériter `NSView` au lieu de `NSControl`.

Quelle que soit la classe de base qui est utilisé, les étapes de base pour la création d’un contrôle personnalisé est le même.

Dans cet article a, créer un composant personnalisé de retourner un commutateur qui fournit un thème de l’Interface utilisateur unique et un exemple de création d’un contrôle d’Interface utilisateur personnalisée entièrement fonctionnelle.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Création du contrôle personnalisé

Étant donné que le contrôle personnalisé, nous allons créer répondra à l’entrée d’utilisateur (clics de bouton gauche de la souris), nous allons hériter `NSControl`. De cette façon, notre contrôle personnalisé sera automatiquement ont toutes les fonctionnalités standards qui dispose d’un contrôle d’Interface utilisateur intégrée et répondre à un contrôle standard macOS.

Dans Visual Studio pour Mac, ouvrez le projet Xamarin.Mac que vous souhaitez créer un contrôle d’Interface utilisateur personnalisée pour (ou créez-en un). Ajoutez une nouvelle classe et l’appeler `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Ajout d’une nouvelle classe")](custom-controls-images/custom01.png#lightbox)

Modifiez ensuite la `NSFlipSwitch.cs` classe et qu’elle ressemble à ce qui suit :

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

La première chose à remarquer sur notre classe personnalisée dans la mesure où nous sommes héritant de `NSControl` et à l’aide de la **inscrire** commande d’exposer cette classe pour Objective-C et du Xcode Générateur d’Interface :

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

Dans les sections suivantes, nous allons examiner le reste du code ci-dessus en détail.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Suivi de l’état du contrôle

Étant donné que notre contrôle personnalisé est un commutateur, nous avons besoin d’un moyen de suivre l’état de marche/arrêt du commutateur. Nous nous occupons que par le code suivant dans `NSFlipSwitch`:

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

Lorsque l’état du commutateur change, nous devons mis à jour de l’interface utilisateur. Pour ce faire, nous forcer le contrôle à redessiner son interface utilisateur avec `NeedsDisplay = true`.

Si notre contrôle nécessaire plus qu’un seul état (par exemple un commutateur état multiples avec 3 positions) activé/désactivé, nous pourrions avoir utilisé un **Enum** pour effectuer le suivi de l’état. Dans notre exemple, un simple **bool** effectuera.

Nous avons également ajouté une méthode d’assistance pour remplacer l’état du commutateur entre et désactiver :

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Nous allons étendre ultérieurement, cette classe d’assistance pour informer l’appelant lorsque l’état de commutateurs a changé.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>L’Interface du contrôle de dessin

Nous allons utiliser des routines de dessin du graphique principal pour dessiner l’Interface utilisateur de votre contrôle personnalisé lors de l’exécution. Avant que nous pouvons faire cela, nous devons mettre sous tension couches pour notre contrôle. Nous le faire avec la méthode privée suivante :

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Cette méthode est appelée à partir de chacun des constructeurs d' un contrôle pour s’assurer que le contrôle est configuré correctement. Par exemple :

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Ensuite, nous devons remplacer le `DrawRect` (méthode) et ajoutez les routines Core graphique pour dessiner le contrôle :

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Nous allons ajuster la représentation visuelle du contrôle lorsque son état est modifié (tels qu’allant **sur** à **hors**). Chaque fois que les modifications d’état, nous pouvons utiliser le `NeedsDisplay = true` pour forcer le contrôle à redessiner pour le nouvel état.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Répondre à l’entrée utilisateur

Il existe deux manière de base que nous pouvons ajouter une entrée utilisateur vers notre contrôle personnalisé : **remplacer les Routines de gestion de la souris** ou **reconnaissances de mouvement**. La méthode que nous utilisons, doit reposer sur les fonctionnalités requises par notre contrôle.

> [!IMPORTANT]
> Pour n’importe quel contrôle personnalisé que vous créez, vous devez utiliser soit **substituer les méthodes** _ou_ **reconnaissances de mouvement**, mais pas les deux en même temps qu’ils peuvent entrer en conflit entre eux.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Gestion des entrées d’utilisateur avec les méthodes Override

Les objets qui héritent de `NSControl` (ou `NSView`) avoir plusieurs substituer des méthodes pour la gestion de la souris ou du clavier d’entrée. Pour notre exemple de contrôle, vous souhaitez retourner l’état du commutateur entre **sur** et **hors** lorsque l’utilisateur clique sur le contrôle avec le bouton gauche de la souris. Nous pouvons ajouter les éléments suivants substituer des méthodes pour la `NSFliwSwitch` classe pour gérer cette situation :

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

Dans le code ci-dessus, nous appelons le `FlipSwitchState` (méthode) (défini ci-dessus) pour retourner l’activation/désactivation de l’état du commutateur dans le `MouseDown` (méthode). Cela force également le contrôle est redessiné pour refléter l’état actuel.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Gestion des entrées d’utilisateur avec des modules de reconnaissance de mouvement

Si vous le souhaitez, vous pouvez utiliser des modules de reconnaissance de mouvement pour gérer l’utilisateur interagit avec le contrôle. Supprimer les remplacements ajoutées ci-dessus, modifiez le `Initialize` (méthode) et le rendre l’aspect suivant :

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

Ici, nous allons créer un nouveau `NSClickGestureRecognizer` et appel de notre `FlipSwitchState` méthode pour modifier l’état de son lorsque l’utilisateur clique dessus avec le bouton gauche de la souris. Le `AddGestureRecognizer (click)` méthode ajoute la reconnaissance de mouvement au contrôle.

Là encore, nous utilisons méthode dépend de ce que nous essayons d’accomplir grâce au contrôle personnalisé. Si nous devons faible niveau d’accès à une interaction utilisateur, utilisez les méthodes à substituer. Si nous avons besoin de fonctionnalités prédéfinies, telles que des clics de souris, utilisez les reconnaissances de mouvement.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Réponse aux événements de changement d’état

Lorsque l’utilisateur modifie l’état de votre contrôle personnalisé, nous avons besoin d’un moyen pour répondre à la modification d’état dans le code (par exemple faire quelque chose lorsque clique sur un bouton personnalisé). 

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

Modifiez ensuite le `FlipSwitchState` (méthode) et le rendre l’aspect suivant :

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Tout d’abord, nous fournissons un `ValueChanged` événement que nous pouvons ajouter un gestionnaire de code c# afin que nous pouvons effectuer une action quand l’utilisateur modifie l’état du commutateur.

Ensuite, étant donné que notre contrôle personnalisé hérite de `NSControl`, elle a automatiquement un **Action** qui peuvent être attribuées dans le Générateur de Xcode Interface. Pour appeler cette **Action** lorsque l’état change, nous utilisons le code suivant :

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Tout d’abord, nous vérifions si une **Action** a été assigné au contrôle. Ensuite, nous appelons le **Action** si elle a été définie.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Utilisation du contrôle personnalisé

Grâce au contrôle personnalisé entièrement défini, nous pouvons ajouter il à l’interface utilisateur de l’application de nos Xamarin.Mac à l’aide de code c# ou dans le Générateur de Xcode Interface.

Pour ajouter le contrôle à l’aide du constructeur d’Interface, commencez par effectuer un nettoyage de build du projet Xamarin.Mac, puis double-cliquez sur le `Main.storyboard` fichier pour l’ouvrir dans l’Interface générateur pour les modifier :

[![](custom-controls-images/custom02.png "Modification du plan conceptuel dans Xcode")](custom-controls-images/custom02.png#lightbox)

Ensuite, faites glisser un `Custom View` dans la conception de l’Interface utilisateur :

[![](custom-controls-images/custom03.png "Sélection d’un affichage personnalisé à partir de la bibliothèque")](custom-controls-images/custom03.png#lightbox)

Avec la vue personnalisée étant toujours sélectionnée, basculez vers le **inspecteur d’identité** et modifier la vue **classe** à `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Définition de la classe de vue")](custom-controls-images/custom04.png#lightbox)

Basculez vers le **Assistant éditeur** et créer un **prise** pour le contrôle personnalisé (en veillant à lier dans le `ViewControler.h` fichier et non le `.m` fichier) :

[![](custom-controls-images/custom05.png "Configuration d’un nouveau magasin")](custom-controls-images/custom05.png#lightbox)

Enregistrer vos modifications, revenez à Visual Studio pour Mac et autoriser les modifications à synchroniser. Modifier la `ViewController.cs` et effectuez le `ViewDidLoad` détaillée de la méthode comme suit :

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

Ici, nous répondre à la `ValueChanged` événement nous définis ci-dessus sur le `NSFlipSwitch` de classe et l’écriture en cours **valeur** lorsque l’utilisateur clique sur le contrôle.

Si vous le souhaitez, nous pouvons retourner à l’Interface générateur et définir un **Action** sur le contrôle :

[![](custom-controls-images/custom06.png "Configuration d’une nouvelle Action")](custom-controls-images/custom06.png#lightbox)

Là encore, modifier le `ViewController.cs` et ajoutez la méthode suivante :

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Vous devez utiliser soit le **événement** ou définir un **Action** dans le constructeur d’Interface, mais vous ne devez pas utiliser les deux méthodes en même temps ou ils peuvent entrer en conflit entre eux.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée de la création d’un contrôle d’Interface utilisateur personnalisés réutilisables dans une application Xamarin.Mac. Nous avons vu comment dessiner les contrôles personnalisés l’interface utilisateur, les deux méthodes principales pour répondre à la souris et l’entrée d’utilisateur et exposer le nouveau contrôle pour les Actions dans le Générateur de Xcode Interface.

## <a name="related-links"></a>Liens associés

- [MacCustomControl (exemple)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [La gestion des événements de souris](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
