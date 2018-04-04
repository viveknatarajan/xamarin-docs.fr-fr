---
title: Introduction aux OpenTK
description: Cet article fournit une introduction à l’utilisation dans une application Xamarin.Mac OpenTK. Elle couvre la création et gestion d’une fenêtre de jeu, rendu d’un objet simple et l’affichage de l’objet à l’utilisateur.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d5f19dac8dc362e1ac4a36cbe5cf5db3e31ae363
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-opentk"></a>Introduction aux OpenTK

OpenTK (ouvrir le Toolkit) est une avancée de bas niveau c# bibliothèque qui facilite l’utilisation de OpenGL et OpenCL de OpenAL. OpenTK peut être utilisé pour les jeux, les applications scientifiques ou autres projets qui nécessitent des graphiques 3D, les fonctionnalités audio ou calcul. Cet article fournit une brève introduction à l’aide de OpenTK dans une application Xamarin.Mac.

[![](opentk-images/intro01.png "Un exemple d’application exécuter")](opentk-images/intro01.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de OpenTK dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>À propos de OpenTK

Comme indiqué ci-dessus, OpenTK (ouvrir le Toolkit) est une avancée de bas niveau c# bibliothèque qui facilite l’utilisation de OpenGL et OpenCL de OpenAL. À l’aide de OpenTK dans une application Xamarin.Mac fournit les fonctionnalités suivantes :

- **Développement rapide** -OpenTK fournit les types de données puissant et documentation en ligne pour améliorer votre flux de travail de codage et d’intercepter des erreurs plus facilement et plus tôt.
- **Une intégration facile** -OpenTK a été conçu pour s’intégrer facilement avec les applications .NET.
- **Licence permissive** -OpenTK est distribué sous les licences MIT/X11 et est totalement gratuite.
- **Riches, les liaisons de Type sécurisé** -OpenTK prend en charge les dernières versions de OpenGL, OpenGL | ES, OpenAL et OpenCL avec le chargement de l’extension automatique, documentation en ligne et de la vérification d’erreur.
- **Les Options de l’interface graphique utilisateur flexible** -OpenTK fournit des natif, la fenêtre de jeu de hautes performances conçu spécifiquement pour les jeux et Xamarin.Mac.
- **Entièrement géré, un Code conforme CLS** -OpenTK prend en charge les versions 32 bits et 64 bits de macOS sans bibliothèques non managées.
- **Outils d’analyse mathématique 3D** OpenTK fournit `Vector`, `Matrix`, `Quaternion` et `Bezier` structs via sa boîte à outils mathématiques 3D.

OpenTK peut être utilisé pour les jeux, les applications scientifiques ou autres projets qui nécessitent des graphiques 3D, les fonctionnalités audio ou calcul.

Pour plus d’informations, consultez [le Toolkit ouvrir](http://www.opentk.com) site Web.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>Démarrage rapide de OpenTK

Comme une présentation rapide à l’aide de OpenTK dans une application Xamarin.Mac, nous allons créer une application simple qui ouvre une vue de jeu, restitue un triangle simple dans un affichage et l’affichage du jeu d’attachs à la fenêtre principale de l’application Mac pour afficher le triangle à l’utilisateur.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Démarrage d’un nouveau projet

Démarrez Visual Studio pour Mac et créer une nouvelle solution Xamarin.Mac. Sélectionnez **Mac** > **application** > **général** > **/Cocoa application**:

[![](opentk-images/sample01.png "Ajout d’une nouvelle application/Cocoa")](opentk-images/sample01.png#lightbox)

Entrez `MacOpenTK` pour le **nom du projet**:

[![](opentk-images/sample02.png "Définition du nom de projet")](opentk-images/sample02.png#lightbox)

Cliquez sur le **créer** pour générer le nouveau projet.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Y compris OpenTK

Avant de pouvoir utiliser TK ouvert dans une application Xamarin.Mac, vous devez inclure une référence à l’assembly OpenTK. Dans le **l’Explorateur de solutions**, avec le bouton droit le **références** et sélectionnez **modifier les références...** .

Cochez la case en `OpenTK` et cliquez sur le **OK** bouton :

[![](opentk-images/sample03.png "Modifier les références de projet")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>À l’aide de OpenTK

Le nouveau projet créé, puis double-cliquez sur le `MainWindow.cs` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier. Rendre le `MainWindow` détaillée de la classe comme suit :

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Attardons-nous sur ce code en détail ci-dessous.

<a name="Required_APIs" />

### <a name="required-apis"></a>API obligatoire

Plusieurs références sont requis pour utiliser OpenTK dans une classe Xamarin.Mac. Nous avons inclus des suivantes au début de la définition de `using` instructions :

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
Ce jeu minimal sera requis pour toute classe à l’aide de OpenTK.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Ajout de la vue de jeu

Ensuite, nous devons créer un jeu pour contenir tous nos interaction avec OpenTK et afficher les résultats. Nous avons utilisé le code suivant :

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Nous avons fait ici la même taille que notre fenêtre principale de Mac l’affichage du jeu et remplacé l’affichage du contenu de la fenêtre avec le nouveau `MonoMacGameView`. Étant donné que nous avons remplacé le contenu existant de la fenêtre, notre vue donné doit être automatiquement redimensionnée lorsque les fenêtres principal est redimensionné.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Réponse aux événements

Il existe plusieurs événements par défaut chaque vue jeu doit répondre aux. Dans cette section décrit les principaux événements requis.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>L’événement de chargement

Le `Load` événement est l’endroit pour charger les ressources à partir du disque telles que des images, des textures ou musique. Pour notre simple, l’application de test, nous n’utilisons pas la `Load` événement, mais ont inclus à titre de référence :

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>L’événement de redimensionnement

Le `Resize` événement doit être appelé chaque fois que l’affichage du jeu est redimensionné. Pour notre exemple d’application, nous apportons de la fenêtre d’affichage GL la même taille que notre vue jeu (qui est automatiquement redimensionné à la fenêtre principale de Mac) avec le code suivant :

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>L’événement UpdateFrame

Le `UpdateFrame` événement est utilisé pour gérer l’entrée d’utilisateur, de mettre à jour les positions de l’objet, exécution physique ou les calculs AI. Pour notre simple, l’application de test, nous n’utilisons pas la `UpdateFrame` événement, mais ont inclus à titre de référence : 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> L’implémentation de Xamarin.Mac de OpenTK n’inclut pas le `Input API`, donc vous devez utiliser l’Apple fournies prend en charge les API pour ajouter le clavier et la souris. Si vous le souhaitez, vous pouvez créer une instance personnalisée de la `MonoMacGameView` et remplacez le `KeyDown` et `KeyUp` méthodes.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>L’événement RenderFrame

Le `RenderFrame` événement contient le code qui est utilisé pour restituer (dessin) vos graphiques. Pour notre exemple d’application, nous allons remplissant la vue de jeu avec un triangle simple : 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

En général, le code de rendu est en cours avec un appel à `GL.Clear` pour supprimer les éléments existants avant de dessiner les nouveaux éléments.

> [!IMPORTANT]
> Pour obtenir la version Xamarin.Mac de OpenTK **pas** appeler le `SwapBuffers` méthode de votre `MonoMacGameView` instance à la fin de votre code de rendu. Cela provoque l’affichage de jeu pour strobe rapidement à la place de votre affichage.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>La vue de jeu en cours d’exécution

Tous les événements nécessaires de définir et l’affichage du jeu est attaché à la fenêtre principale de Mac de notre application, nous sommes en lecture pour exécuter la vue de jeu et de notre graphiques. Utilisez le code suivant :

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Nous passons à la fréquence d’images souhaité que nous souhaitons l’affichage du jeu pour mettre à jour à, dans notre exemple, nous avons choisi `60` images par seconde (la même fréquence d’actualisation en tant que TV normal).

Nous allons exécuter notre application et afficher la sortie :

[![](opentk-images/intro01.png "Un exemple de la sortie d’applications")](opentk-images/intro01.png#lightbox)

Si nous redimensionner notre fenêtre, l’affichage du jeu sera également résider et le triangle sera redimensionné et mis à jour en temps réel ainsi.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Où suivant ?

Les bases de l’utilisation de OpenTk dans une application Xamarin.mac terminée, voici quelques suggestions quant à la tester suivant :

- Essayez de modifier la couleur du triangle et la couleur d’arrière-plan de la vue de jeu dans le `Load` et `RenderFrame` les événements.
- Modifier le triangle couleur lorsque l’utilisateur appuyez sur une touche dans le `UpdateFrame` et `RenderFrame` événements ou rendre vos propres `MonoMacGameView` classe et substituer les `KeyUp` et `KeyDown` méthodes.
- Rendre le triangle à déplacer sur l’écran à l’aide de clés prenant en charge dans les `UpdateFrame` événement. Conseil : utilisez le `Matrix4.CreateTranslation` pour créer une matrice de translation et l’appel de méthode le `GL.LoadMatrix` pour le charger dans le `RenderFrame` événement.
- Utilisez un `for` boucle pour restituer plusieurs triangles dans le `RenderFrame` événement.
- Faire pivoter l’appareil photo pour donner une vue différente du triangle dans l’espace 3D. Conseil : utilisez le `Matrix4.CreateTranslation` pour créer une matrice de translation et l’appel de méthode le `GL.LoadMatrix` pour le charger. Vous pouvez également utiliser le `Vector2`, `Vector3`, `Vector4` et `Matrix4` des classes pour les manipulations de la caméra.

Pour plus d’exemples, consultez la [GitHub d’exemples OpenTK](https://github.com/opentk/opentk/tree/master/Source/Examples) référentiel. Il contient une liste officielle d’exemples d’utilisation de OpenTK. Vous devrez adapter ces exemples d’utilisation avec la version Xamarin.Mac de OpenTK.

Pour un exemple plus complexe Xamarin.Mac d’une implémentation OpenTK, veuillez consulter notre [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) exemple.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris un coup de œil rapide à l’utilisation de OpenTK dans une application Xamarin.Mac. Nous avons vu comment créer une fenêtre de jeu, comment attacher la fenêtre de jeu dans une fenêtre Mac et le rendu d’une forme simple dans la fenêtre de jeu.

## <a name="related-links"></a>Liens associés

- [MacOpenTK (exemple)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (exemple)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation des fenêtres](~/mac/user-interface/window.md)
- [La boîte à outils ouverte](http://www.opentk.com)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
