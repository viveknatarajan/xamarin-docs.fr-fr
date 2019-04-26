---
title: Introduction à OpenTK dans Xamarin.Mac
description: Cet article fournit une introduction à l’utilisation avec OpenTK dans une application Xamarin.Mac. Il couvre la création et gestion d’une fenêtre de jeu, rendu d’un objet simple et affichage de l’objet à l’utilisateur.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 835b8cd0f2e689c4d7d4cace1d846543863b7393
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032655"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Introduction à OpenTK dans Xamarin.Mac

OpenTK (le Toolkit Open) est une avancée, de bas niveau c# bibliothèque qui facilite l’utilisation avec OpenGL, OpenCL et OpenAL. OpenTK peut être utilisé pour les jeux, les applications scientifiques ou autres projets qui nécessitent des graphiques 3D, les fonctionnalités audio ou de calcul. Cet article donne une brève introduction à l’utilisation de OpenTK dans une application Xamarin.Mac.

[![](opentk-images/intro01.png "Exécution d’une application exemple")](opentk-images/intro01.png#lightbox)

Dans cet article, nous traiterons les notions de base de OpenTK dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous utilisons dans cet article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, il explique le `Register` et `Export` commandes utilisé pour AutoEventWireup vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>À propos de OpenTK

Comme indiqué ci-dessus, OpenTK (le Toolkit Open) est une avancée, de bas niveau c# bibliothèque qui facilite l’utilisation avec OpenGL, OpenCL et OpenAL. À l’aide de OpenTK dans une application Xamarin.Mac propose les options suivantes :

- **Développement rapide** -OpenTK fournit des types de données puissant et documentation en ligne pour améliorer votre flux de travail de codage et de détecter les erreurs plus facilement et plus tôt.
- **Intégration facile** -OpenTK a été conçu pour intégrer facilement avec les applications .NET.
- **Licence d’autorisation** -OpenTK est distribué sous les licences MIT/X11 et est totalement gratuit.
- **Riche, les liaisons de Type sécurisé** -OpenTK prend en charge les dernières versions de OpenGL, OpenGL | ES, OpenAL et OpenCL avec le chargement d’extensions automatiques, documentation en ligne et de la vérification d’erreur.
- **Options flexibles de l’interface graphique utilisateur** -OpenTK fournit natif, la fenêtre de jeu de hautes performances conçu spécifiquement pour les jeux et Xamarin.Mac.
- **Entièrement géré, un Code conforme CLS** -OpenTK prend en charge les versions 32 bits et 64 bits de macOS sans bibliothèques non managées.
- **Kit de ressources 3D mathématiques** OpenTK fournit `Vector`, `Matrix`, `Quaternion` et `Bezier` structs via leur Kit de ressources 3D mathématiques.

OpenTK peut être utilisé pour les jeux, les applications scientifiques ou autres projets qui nécessitent des graphiques 3D, les fonctionnalités audio ou de calcul.

Pour plus d’informations, consultez [le Kit de ressources Open](http://www.opentk.com) site Web.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>Guide de démarrage rapide OpenTK

Une introduction rapide à l’utilisation de OpenTK dans une application Xamarin.Mac, nous allons créer une application simple qui ouvre un affichage de jeu, assure un rendu un triangle simple dans cette vue et l’attachs la vue de jeu dans la fenêtre principale de l’application Mac pour afficher le triangle à l’utilisateur.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Démarrez un nouveau projet

Démarrez Visual Studio pour Mac et créer une nouvelle solution de Xamarin.Mac. Sélectionnez **Mac** > **application** > **général** > **application Cocoa**:

[![](opentk-images/sample01.png "Ajoutez une nouvelle application Cocoa")](opentk-images/sample01.png#lightbox)

Entrez `MacOpenTK` pour le **nom du projet**:

[![](opentk-images/sample02.png "Définition du nom de projet")](opentk-images/sample02.png#lightbox)

Cliquez sur le **créer** pour générer le nouveau projet.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Y compris OpenTK

Avant de pouvoir utiliser TK ouvert dans une application Xamarin.Mac, vous devez inclure une référence à l’assembly OpenTK. Dans le **l’Explorateur de solutions**, avec le bouton droit le **références** dossier et sélectionnez **modifier les références...** .

Cochez la case par `OpenTK` et cliquez sur le **OK** bouton :

[![](opentk-images/sample03.png "Modifier les références de projet")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>À l’aide de OpenTK

Le nouveau projet créé, puis double-cliquez sur le `MainWindow.cs` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour modification. Rendre la `MainWindow` classe ressemble ce qui suit :

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

### <a name="required-apis"></a>API nécessaires

Plusieurs références sont nécessaire pour utiliser OpenTK dans une classe de Xamarin.Mac. Au début de la définition, nous avons inclus les éléments suivants `using` instructions :

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
Ce jeu minimal sera nécessaire pour n’importe quelle classe à l’aide de OpenTK.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Ajout de la vue de jeu

Ensuite, nous devons créer une vue de jeu pour contenir tous nos interactions avec OpenTK et afficher les résultats. Nous avons utilisé le code suivant :

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Ici nous avons apporté la vue de la partie la même taille que notre fenêtre principale de Mac et remplacé l’affichage du contenu de la fenêtre avec le nouveau `MonoMacGameView`. Étant donné que nous avons remplacé le contenu existant de la fenêtre, notre vue a donné doit être automatiquement redimensionnée lorsque le principal Windows est redimensionné.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Réponse aux événements

Il existe plusieurs événements par défaut que chaque vue de jeu doit répondre à. Dans cette section couvre les principaux événements requis.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>L’événement de chargement

Le `Load` événement est l’endroit où charger les ressources à partir du disque comme des images, des textures ou musique. Pour notre simple, l’application de test, nous n’utilisons pas la `Load` événement, mais vous l’avez incluse pour référence :

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>L’événement de redimensionnement

Le `Resize` événement doit être appelé chaque fois que la vue de jeu est redimensionnée. Pour notre exemple d’application, nous avons apporté de la fenêtre d’affichage GL la même taille que notre mode de jeu (qui est d’être automatiquement redimensionnée par la fenêtre principale de Mac) avec le code suivant :

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>L’événement UpdateFrame

Le `UpdateFrame` événement est utilisé pour gérer l’entrée d’utilisateur, de mettre à jour les positions de l’objet, exécution physique ou calculs d’intelligence artificielle. Pour notre simple, l’application de test, nous n’utilisons pas la `UpdateFrame` événement, mais vous l’avez incluse pour référence : 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> L’implémentation de Xamarin.Mac de OpenTK n’inclut pas le `Input API`, de sorte que vous devez utiliser Apple fourni la prise en charge API pour ajouter le clavier et souris. Si vous le souhaitez, vous pouvez créer une instance personnalisée de la `MonoMacGameView` et remplacer le `KeyDown` et `KeyUp` méthodes.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>L’événement RenderFrame

Le `RenderFrame` événement contient le code qui est utilisé pour restituer (dessin) vos graphiques. Pour notre exemple d’application, nous allons remplir la vue de jeu avec un triangle simple : 

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

En général, le code de rendu s’étant avec un appel à `GL.Clear` pour supprimer les éléments existants avant de dessiner les nouveaux éléments.

> [!IMPORTANT]
> Pour la version de Xamarin.Mac de OpenTK **ne le faites pas** appeler le `SwapBuffers` méthode de votre `MonoMacGameView` instance à la fin de votre code de rendu. Cela provoque l’affichage de jeu à strobe rapidement au lieu de votre affichage rendu.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>La vue de jeu en cours d’exécution

Avec tous les événements requis définir et la vue de jeu attaché à la fenêtre de Mac Main de notre application, nous allons lire pour exécuter la vue de jeu et afficher des notre graphique. Utilisez le code suivant :

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Nous transmettons la fréquence d’images souhaitée que nous voulons la vue de jeu à mettre à jour à, dans notre exemple, nous avons choisi `60` images par seconde (la même fréquence d’actualisation en tant que TV normal).

Nous allons exécuter notre application et afficher la sortie :

[![](opentk-images/intro01.png "Un exemple de la sortie d’applications")](opentk-images/intro01.png#lightbox)

Si nous redimensionnons notre fenêtre, la vue de jeu sera également résider et le triangle sera redimensionné et mis à jour en temps réel ainsi.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Où suivant ?

Principes de base de l’utilisation de OpenTk dans une application Xamarin.mac terminée, voici quelques suggestions de ce qu’il faut essayer suivant :

- Essayez de modifier la couleur du triangle et la couleur d’arrière-plan de la vue de jeu dans le `Load` et `RenderFrame` événements.
- Modifiez le triangle couleur lorsque l’utilisateur appuie sur une clé dans le `UpdateFrame` et `RenderFrame` événements ou effectuer votre propre `MonoMacGameView` classe et substituer les `KeyUp` et `KeyDown` méthodes.
- Rendre le triangle traversent l’écran à l’aide de clés prenant en charge dans le `UpdateFrame` événement. Conseil : utilisez le `Matrix4.CreateTranslation` méthode pour créer une matrice de translation et l’appel de la `GL.LoadMatrix` pour le charger dans le `RenderFrame` événement.
- Utilisez un `for` boucle pour restituer des triangles plusieurs dans le `RenderFrame` événement.
- Faire pivoter l’appareil photo pour donner une vue différente du triangle dans l’espace 3D. Conseil : utilisez le `Matrix4.CreateTranslation` méthode pour créer une matrice de translation et l’appel de la `GL.LoadMatrix` pour le charger. Vous pouvez également utiliser le `Vector2`, `Vector3`, `Vector4` et `Matrix4` des classes pour les manipulations de l’appareil photo.

Pour plus d’exemples, consultez le [GitHub d’exemples OpenTK](https://github.com/opentk/opentk/tree/master/Source/Examples) dépôt. Il contient une liste officielle des exemples d’utilisation de OpenTK. Vous devrez adapter ces exemples pour l’utilisation avec la version de Xamarin.Mac de OpenTK.

Pour un exemple plus complexe de Xamarin.Mac d’une implémentation OpenTK, veuillez consulter notre [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) exemple.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en rapide à l’utilisation de OpenTK dans une application Xamarin.Mac. Nous avons vu comment créer une fenêtre de jeu, comment attacher la fenêtre de jeu à une fenêtre de Mac et le rendu d’une forme simple dans la fenêtre de jeu.

## <a name="related-links"></a>Liens associés

- [MacOpenTK (exemple)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (exemple)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation de Windows](~/mac/user-interface/window.md)
- [La boîte à outils ouverte](http://www.opentk.com)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
