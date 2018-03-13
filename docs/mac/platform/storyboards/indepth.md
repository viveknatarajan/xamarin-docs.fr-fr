---
title: Utilisation des animations
description: "Création d’interfaces utilisateur macOS avec animations à l’aide de Xcode."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 388744d48799e84587a4f554a4eb67cd677d8c9a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="storyboards"></a>Animations

Un plan conceptuel définit l’ensemble de l’interface utilisateur pour une application donnée est divisée en une vue d’ensemble fonctionnelle de ses contrôleurs de la vue. Dans le Générateur de Xcode Interface, chacune de ces contrôleurs réside dans son propre scène.

[![](indepth-images/intro01.png "Un plan conceptuel dans le Générateur de Xcode Interface")](indepth-images/intro01.png#lightbox)

Le plan conceptuel est un fichier de ressources (avec les extensions de `.storyboard`) qui obtient inclus dans l’offre groupée de l’application Xamarin.Mac lorsqu’il est compilé et expédié. Pour définir l’animation de départ pour votre application, modifiez-le de `Info.plist` fichier et sélectionnez le **Interface principale** dans la zone de liste déroulante : 

[![](indepth-images/sb01.png "L’éditeur de fichier Info.plist.")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Chargement à partir du Code

Il peut arriver lorsque vous devez charger un Storyboard spécifique à partir de code et créer un contrôleur de la vue manuellement. Vous pouvez utiliser le code suivant pour effectuer cette action :

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

Le `FromName` charge le fichier d’animation avec le nom donné qui a été inclus dans le groupe de l’application. Le `InstantiateControllerWithIdentifier` crée une instance du contrôleur de vue avec l’identité donnée. Vous définissez l’identité dans le Générateur de Xcode Interface lors de la conception de l’interface utilisateur :

[![](indepth-images/sb02.png "Définition de l’ID de la table de montage séquentiel")](indepth-images/sb02.png#lightbox)

Si vous le souhaitez, vous pouvez utiliser la `InstantiateInitialController` méthode pour charger le contrôleur de vue qui a été attribué le contrôleur Initial dans le Générateur de Interface :

[![](indepth-images/sb03.png "Définition du contrôleur initial")](indepth-images/sb03.png#lightbox)

Il est marqué par le **Point d’entrée de table de montage séquentiel** et la flèche ouvertes ci-dessus.

<a name="View-Controllers" />

## <a name="view-controllers"></a>Affichage des contrôleurs

Affichage des contrôleurs définissent les relations entre une vue donnée d’informations dans une application Mac et le modèle de données qui fournit ces informations. Chaque séquence de niveau supérieur dans la table de montage séquentiel représente un contrôleur de vue dans le code de l’application Xamarin.Mac.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Le cycle de vie du contrôleur vue

Plusieurs nouvelles méthodes ont été ajoutés à la `NSViewController` classe pour prendre en charge les plans conceptuels dans macOS. Plus important encore sont que les méthodes de suivi utilisent pour répondre au cycle de vie de la vue contrôlée par le contrôleur d’affichage donné :

- `ViewDidLoad` -Cette méthode est appelée lorsque la vue est chargée à partir du fichier de plan conceptuel.
- `ViewWillAppear` -Cette méthode est appelée juste avant l’affichage sur l’écran.
- `ViewDidAppear` -Cette méthode est appelée directement après l’affichage de la vue à l’écran.
- `ViewWillDisappear` -Cette méthode est appelée juste avant que la vue est supprimée à partir de l’écran.
- `ViewDidDisappear` -Cette méthode est appelée directement après la suppression de la vue à partir de l’écran.
- `UpdateViewConstraints` -Cette méthode est appelée lorsque les contraintes qui définissent une vue automatiquement disposition position et la taille doivent être mis à jour.
- `ViewWillLayout` -Cette méthode est appelée juste avant que les sous-vues de cette vue sont placés sur l’écran.
- `ViewDidLayout` -Cette méthode est appelée directement après que les sous-vues d’affichage sont placés sur l’écran.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>La chaîne de répondeur

En outre, `NSViewControllers` font désormais partie de la fenêtre _répondeur chaîne_:

[![](indepth-images/vc01.png "La chaîne de répondeur")](indepth-images/vc01.png#lightbox)

Et sont par conséquent accès câblé à recevoir et répondre aux événements tels que les sélections d’élément de menu couper, copier et coller. Cet accès View Controller par câble automatique se produit uniquement sur les applications qui s’exécutent sur macOS Sierra (10.12) et supérieur.

<a name="Containment" />

### <a name="containment"></a>Imbrication

Dans les plans conceptuels, affichage des contrôleurs (par exemple, le fractionnement vue contrôleur et l’onglet Affichage) peuvent désormais implémenter _relation contenant-contenu_, tels qu’ils puissent « contenir » autres sub contrôleurs d’affichage :

[![](indepth-images/vc02.png "Un exemple de relation contenant-contenu de vue contrôleur")](indepth-images/vc02.png#lightbox)

Contrôleurs de la vue enfant contiennent des méthodes et propriétés pour lier les sauvegarder au contrôleur de vue de leur Parent et à travailler avec l’affichage et de suppression de vues à partir de l’écran.

Tous les contrôleurs de la vue conteneur intégrées macOS ont une disposition spécifique qui Apple suggèrent que vous suivez si la création de vos propres contrôleurs d’affichage conteneur personnalisé :

[![](indepth-images/vc03.png "La mise en page du contrôleur d’affichage")](indepth-images/vc03.png#lightbox)

Le contrôleur de la vue de Collection contient un tableau d’éléments de vue de Collection, chacun d’eux contient un ou plusieurs contrôleurs de vue qui contiennent leurs propres vues.

<a name="Segues" />

## <a name="segues"></a>Est parfait pour

Est parfait pour fournir les relations entre tous les scènes qui définissent l’interface utilisateur de votre application. Si vous êtes familiarisé avec l’utilisation dans les plans conceptuels dans iOS, vous savez qu’est parfait pour iOS définissent généralement les transitions entre les vues de plein écran. Cela diffère macOS, Segues définissent généralement «[relation contenant-contenu](#Containment)», où une scène est l’enfant d’un parent de scène.

Dans macOS, la plupart des applications ont tendance à leur point de vue dans la même fenêtre à l’aide des éléments d’interface utilisateur tels que les affichages fractionnés et onglets de groupe. Contrairement à iOS, où vues doivent être transférée sur et hors écran, en raison de la limite physique affiche l’espace.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Présentation est parfait pour

Étant donné les tendances de macOS vers la relation contenant-contenu, il existe des situations où _présentation est parfait pour_ sont utilisés, tels que des fenêtres modales, tableaux et Popover. types de macOS fournit segue de la fonction intégrée suivante :

- **Afficher** -affiche la cible de la Segue sous la forme d’une fenêtre non modale. Par exemple, utilisez ce type de Segue pour présenter une autre instance d’une fenêtre de Document dans votre application.
- **Modal** -affiche la cible de la Segue comme une fenêtre modale. Par exemple, utilisez ce type de Segue pour présenter la fenêtre de préférences pour votre application.
- **Feuille de** -affiche la cible de la Segue comme une feuille attaché à la fenêtre parente. Par exemple, utilisez ce type de segue pour présenter une recherche et la feuille de remplacer.
- **Popover** -affiche la cible de la Segue comme dans une fenêtre popover. Par exemple, utilisez ce type Segue pour présenter des options lorsque l’utilisateur clique sur un élément d’interface utilisateur.
- **Personnalisé** -affiche la cible de la Segue à l’aide d’un Type Segue personnalisé défini par le développeur. Consultez le [création de personnalisée est parfait pour](#Creating-Custom-Segues) section ci-dessous pour plus de détails.

Lorsque vous utilisez la présentation est parfait pour, vous pouvez remplacer le `PrepareForSegue` méthode du contrôleur de la vue parent pour la présentation d’initialisation et de variables et fournir des données au contrôleur de vue en cours de présentation.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Déclenchée est parfait pour

Segues déclenchées permettent de spécifier Segues nommée (via leurs **identificateur** propriété dans le Générateur de Interface) et demandez-lui de déclenchée par des événements tels que l’utilisateur clique sur un bouton ou en appelant le `PerformSegue` méthode dans le code :

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

L’ID Segue est défini à l’intérieur Interface Builder de Xcode lorsque vous disposez des interface utilisateur de l’application :

[![](indepth-images/sg02.png "Entrez un nom de Segue")](indepth-images/sg02.png#lightbox)

Dans le contrôleur de vue qui agit en tant que source de la Segue, vous devez substituer la `PrepareForSegue` (méthode) et effectuez toute initialisation requise avant l’exécution de la Segue et le contrôleur de vue spécifié s’affiche :

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

Si vous le souhaitez, vous pouvez remplacer le `ShouldPerfromSegue` (méthode) et contrôle ou non le Segue est effectivement exécutée via le code c#. Pour les contrôleurs de la vue manuellement présenté, appeler leurs `DismissController` méthode pour les supprimer à partir de l’affichage lorsqu’ils ne sont plus nécessaires.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Création personnalisée est parfait pour

Il peut arriver lorsque votre application nécessite un type Segue non fourni par le Segues de build définie dans macOS. Si c’est le cas, vous pouvez créer un Segue personnalisés qui peuvent être attribuées dans le Générateur de Xcode Interface lors de la disposition de l’interface utilisateur de votre application.

Par exemple, pour créer un nouveau type Segue qui remplace le contrôleur actuel de la vue à l’intérieur d’une fenêtre (au lieu d’ouvrir la séquence cible dans une nouvelle fenêtre), nous pouvons utiliser le code suivant :

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

Quelques points à noter ici :

- Nous utilisons le `Register` attribut d’exposer cette classe pour objectif-C/macOS.
- Nous sommes en remplaçant le `Perform` méthode pour effectuer l’action de notre Segue personnalisé.
- Nous sommes en remplaçant la fenêtre `ContentViewController` contrôleur avec celle définie par la cible (destination) de la Segue.
- Nous allons supprimer le contrôleur d’origine de la vue afin de libérer la mémoire à l’aide du `RemoveFromParentViewController` (méthode).

Pour utiliser ce nouveau type Segue dans le Générateur de Xcode Interface, nous devons compiler l’application en premier lieu, puis à Xcode et ajouter un nouveau Segue entre deux séquences. Définir le **Style** à **personnalisé** et le **Segue de classe** à `ReplaceViewSegue` (le nom de notre classe Segue personnalisée) :

[![](indepth-images/sg01.png "Définition de la classe Segue")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Contrôleurs de fenêtre

Les contrôleurs de fenêtre contiennent et contrôlent les différents types de fenêtre que votre application macOS peut créer. Pour les plans conceptuels, ils possèdent les fonctionnalités suivantes :

1. Il doit spécifier un contrôleur de vue de contenu. Ce sera le même contrôleur de vue de contenu dispose de la fenêtre enfant.
2. Le `Storyboard` propriété contiendra le plan conceptuel que le contrôleur de fenêtre a été chargé à partir, sinon `null` si ne pas chargé à partir d’un plan conceptuel.
3. Vous pouvez appeler la `DismissController` méthode pour fermer la fenêtre donnée et le supprimer de la vue.

Comme l’affichage des contrôleurs, les contrôleurs de fenêtre implémentent la `PerformSegue`, `PrepareForSegue` et le `ShouldPerfromSegue` méthodes et peut être utilisé comme source d’une opération Segue.

Contrôleur de fenêtre sont chargés pour les fonctionnalités suivantes d’une application macOS :

- Ils gèrent une fenêtre spécifique.
- Ils gèrent barre de titre de la fenêtre et la barre d’outils (si disponible).
- Ils gèrent le contrôleur de vue de contenu pour afficher le contenu de la fenêtre.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Modules de reconnaissance de mouvement

Module de reconnaissance de mouvement pour macOS est presque identiques à leurs équivalents dans iOS et permettre au développeur d’ajouter facilement des mouvements (par exemple, en cliquant sur le bouton de la souris) pour les éléments dans l’interface utilisateur de votre application.

Toutefois, où mouvements dans iOS sont déterminées par la conception de l’application (par exemple, si vous appuyez sur l’écran avec deux doigts), plus mouvements dans macOS sont déterminées par le matériel.

À l’aide de modules de reconnaissance de mouvement, vous pouvez réduire considérablement la quantité de code requis pour ajouter des interactions personnalisées à un élément dans l’interface utilisateur. Comme ils peuvent déterminer automatiquement entre les clics des doubles et uniques, cliquez et faites glisser les événements, etc.

Au lieu de remplacer le `MouseDown` événements dans votre contrôleur de la vue, vous devez utiliser un module de reconnaissance de mouvement à gérer l’événement d’entrée utilisateur lorsque vous travaillez avec des plans conceptuels.

Les modules de reconnaissance de mouvement suivantes sont disponibles dans macOS :

- `NSClickGestureRecognizer` -S’inscrire avec la souris vers le bas et les événements.
- `NSPanGestureRecognizer` -Registres bouton de la souris vers le bas, faites glisser et libérer des événements.
- `NSPressGestureRecognizer` -Inscrit un bouton de souris enfoncé pour une quantité donnée d’événement.
- `NSMagnificationGestureRecognizer` -Inscrit un événement de facteur d’agrandissement du pavé tactile matériel.
- `NSRotationGestureRecognizer` -Enregistre un événement de rotation à partir du pavé tactile matériel.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Utilisation de références à la table de montage séquentiel

Une référence de table de montage séquentiel vous permet de prendre une conception de plan conceptuel volumineuses et complexe et de décomposer en plus petits Storyboards obtient référencé à partir de la version d’origine, donc supprimer suppression de la complexité et d’apporter résultant des Storyboards plus facile à la conception et mettre à jour.

En outre, une référence de table de montage séquentiel peut fournir une _ancre_ à une autre séquence au sein de la même table de montage séquentiel ou une scène spécifique sur un autre.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Faisant référence à une table de montage séquentiel externe

Pour ajouter une référence à une table de montage séquentiel externe, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le nom du projet et sélectionnez **ajouter** > **nouveau fichier...**   >  **Mac** > **Storyboard**. Entrez un **nom** pour la table de montage séquentiel et cliquez sur le **nouveau** bouton : 

    [![](indepth-images/ref01.png "Ajout d’une table de montage séquentiel")](indepth-images/ref01.png#lightbox)
2. Dans le **l’Explorateur de solutions**, double-cliquez sur le nouveau nom de table de montage séquentiel à ouvrir pour le modifier dans le Générateur de Xcode Interface.
2. Concevoir la disposition de l’arrière-plan de la nouvelle table de montage séquentiel aurait normalement et enregistrer vos modifications : 

    [![](indepth-images/ref02.png "Conception de l’interface")](indepth-images/ref02.png#lightbox)
3. Basculez vers le plan conceptuel que vous allez ajouter la référence dans le Générateur de l’Interface.
4. Faites glisser un **Storyboard référence** à partir de la **de l’objet bibliothèque** sur l’aire de conception : 

    [![](indepth-images/ref03.png "Sélection d’une référence de l’animation dans la bibliothèque")](indepth-images/ref03.png#lightbox)
5. Dans le **inspecteur de l’attribut**, sélectionnez le nom de la **Storyboard** que vous avez créé précédemment : 

    [![](indepth-images/ref04.png "Configuration de la référence")](indepth-images/ref04.png#lightbox)
6. En cliquant sur un Widget de l’interface utilisateur (par exemple, un bouton) sur une scène existante et créer un nouveau Segue à la **référence de table de montage séquentiel** que vous venez de créer.  Dans le menu contextuel, sélectionnez **afficher** pour terminer le Segue : 

    [![](indepth-images/ref06.png "La définition du type Segue")](indepth-images/ref06.png#lightbox) 
8. Enregistrez vos modifications dans la table de montage séquentiel.
9. Revenez à Visual Studio pour Mac synchroniser vos modifications.

Lorsque l’application est exécutée et que l’utilisateur clique sur l’élément d’interface utilisateur que vous avez créé le Segue à partir, le contrôleur Initial de la fenêtre à partir de la table de montage séquentiel externe spécifié dans la référence de table de montage séquentiel seront affichera.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Faisant référence à une séquence spécifique dans une table de montage séquentiel externe

Pour ajouter une référence à une séquence spécifique un Storyboard externe (et non du contrôleur de fenêtre initiale), procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur la table de montage séquentiel externe pour l’ouvrir pour le modifier dans le Générateur de Xcode Interface.
2. Ajouter une nouvelle séquence et sa disposition de conception comme vous le feriez normalement : 

    [![](indepth-images/ref07.png "Définition de la disposition dans Xcode")](indepth-images/ref07.png#lightbox)
3. Dans le **inspecteur d’identité**, entrez un **ID de la table de montage séquentiel** pour fenêtre contrôleur de cette nouvelle séquence : 

    [![](indepth-images/ref08.png "Définition de l’ID de la table de montage séquentiel")](indepth-images/ref08.png#lightbox)
3. Ouvrez le plan conceptuel que vous vous apprêtez à ajouter la référence dans le Générateur de l’Interface.
4. Faites glisser un **Storyboard référence** à partir de la **de l’objet bibliothèque** sur l’aire de conception : 

    [![](indepth-images/ref03.png "Sélection d’une référence de table de montage séquentiel à partir de la bibliothèque")](indepth-images/ref03.png#lightbox)
5. Dans le **inspecteur d’identité**, sélectionnez le nom de la **Storyboard** et **ID de référence** (ID de la table de montage séquentiel) de la scène que vous avez créé précédemment : 

    [![](indepth-images/ref09.png "Définition de l’ID de référence")](indepth-images/ref09.png#lightbox)
6. En cliquant sur un Widget de l’interface utilisateur (par exemple, un bouton) sur une scène existante et créer un nouveau Segue à la **référence de table de montage séquentiel** que vous venez de créer. Dans le menu contextuel, sélectionnez **afficher** pour terminer le Segue : 

    [![](indepth-images/ref06.png "La définition du Type Segue")](indepth-images/ref06.png#lightbox) 
8. Enregistrez vos modifications dans la table de montage séquentiel.
9. Revenez à Visual Studio pour Mac synchroniser vos modifications.

Lorsque l’application est exécutée et que l’utilisateur clique sur l’élément d’interface utilisateur que vous avez créé le Segue à partir de la scène avec la donnée **ID de la table de montage séquentiel** à partir de la table de montage séquentiel externe spécifié dans la référence de table de montage séquentiel s’affichera.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Faisant référence à une séquence spécifique dans la même table de montage séquentiel

Pour ajouter une référence à une séquence spécifique de la même table de montage séquentiel, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le plan conceptuel pour l’ouvrir pour le modifier.
2. Ajouter une nouvelle séquence et sa disposition de conception comme vous le feriez normalement : 

    [![](indepth-images/ref11.png "Modification du plan conceptuel dans Xcode")](indepth-images/ref11.png#lightbox)
3. Dans le **inspecteur d’identité**, entrez un **ID de la table de montage séquentiel** pour fenêtre contrôleur de cette nouvelle séquence : 

    [![](indepth-images/ref12.png "Définition de l’ID de la table de montage séquentiel")](indepth-images/ref12.png#lightbox)
3. Faites glisser un **Storyboard référence** à partir de la **boîte à outils** sur l’aire de conception : 

    [![](indepth-images/ref03.png "Sélection d’une référence de table de montage séquentiel à partir de la bibliothèque")](indepth-images/ref03.png#lightbox)
5. Dans **inspecteur de l’attribut**, sélectionnez **ID de référence** (ID de la table de montage séquentiel) de la scène que vous avez créé précédemment : 

    [![](indepth-images/ref13.png "Définition de l’ID de référence")](indepth-images/ref13.png#lightbox)
6. En cliquant sur un Widget de l’interface utilisateur (par exemple, un bouton) sur une scène existante et créer un nouveau Segue à la **référence de table de montage séquentiel** que vous venez de créer. Dans le menu contextuel, sélectionnez **afficher** pour terminer le Segue : 

    [![](indepth-images/ref06.png "Sélection du Type Segue")](indepth-images/ref06.png#lightbox) 
8. Enregistrez vos modifications dans la table de montage séquentiel.
9. Revenez à Visual Studio pour Mac synchroniser vos modifications.

Lorsque l’application est exécutée et que l’utilisateur clique sur l’élément d’interface utilisateur que vous avez créé le Segue à partir de la scène avec la donnée **ID de la table de montage séquentiel** dans le même Storyboard spécifié dans la référence de table de montage séquentiel s’affiche.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Exemple de plan conceptuel complexe

Pour obtenir un exemple complexe de l’utilisation de plans conceptuels dans une application Xamarin.Mac, consultez la [SourceWriter, exemple d’application](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter est un éditeur de code source simple qui assure la prise en charge de la complétion de code et de la coloration syntaxique de base.

Le code SourceWriter a été entièrement commenté et, le cas échéant, des liens ont être fournis entre les principales technologies ou méthodes et des informations pertinentes dans la documentation des guides Xamarin.Mac.

## <a name="related-links"></a>Liens associés

- [MacStoryboard (exemple)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation des fenêtres](~/mac/user-interface/window.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
