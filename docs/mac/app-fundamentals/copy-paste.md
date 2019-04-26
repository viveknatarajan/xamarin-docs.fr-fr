---
title: Copiez et collez dans Xamarin.Mac
description: Cet article aborde l’utilisation avec la table de montage pour fournir les copier et coller dans une application Xamarin.Mac. Il montre comment travailler avec des types de données standard qui peuvent être partagés entre plusieurs applications et comment prendre en charge des données personnalisées dans une application donnée.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: f9e05b6d16210021257fe3958966739e526aed18
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378558"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Copiez et collez dans Xamarin.Mac

_Cet article aborde l’utilisation avec la table de montage pour fournir les copier et coller dans une application Xamarin.Mac. Il montre comment travailler avec des types de données standard qui peuvent être partagés entre plusieurs applications et comment prendre en charge des données personnalisées dans une application donnée._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même table de montage (copier et coller) prise en charge qui a un développeur travaillant en Objective-C.

Dans cet article que nous aborderons les deux méthodes principales pour utiliser la table de montage dans une application Xamarin.Mac :

1. **Types de données standard** -étant donné que la table de montage généralement effectués entre les deux applications non liées, ni application connaît les types de données prenant en charge l’autre. Pour optimiser le potentiel pour le partage, la table de montage peut contenir plusieurs représentations sous forme d’un élément donné (à l’aide d’un ensemble standard de types de données courants), ainsi l’application consommatrice de choisir la version qui convient le mieux à ses besoins.
2. **Données personnalisées** - pour prendre en charge la copie et collage de données complexes au sein de votre Xamarin.Mac, vous pouvez définir un type de données personnalisé qui sera géré par la table de montage. Par exemple, une application de dessin vectoriel qui permet à l’utilisateur copier et coller des formes complexes sont composées de plusieurs types de données et des points.

[![Exemple de l’application en cours d’exécution](copy-paste-images/intro01.png "exemple de l’application en cours d’exécution")](copy-paste-images/intro01-large.png#lightbox)

Dans cet article, nous traiterons les notions de base de l’utilisation de la table de montage dans une application Xamarin.Mac pour prendre en charge la copie et les opérations de collage. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous utilisons dans cet article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, il explique le `Register` et `Export` attributs utilisé pour structurer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

## <a name="getting-started-with-the-pasteboard"></a>Bien démarrer avec la table de montage

La table de montage présente un mécanisme standardisé pour échanger des données au sein d’une application donnée, ou entre des applications. L’utilisation classique pour une table de montage dans une application Xamarin.Mac consiste à gérer la copie et collez des opérations, mais un nombre d’autres opérations est également pris en charge (par exemple, faites glisser & Drop et les Services d’Application).

Pour vous aider à décoller rapidement, nous allons commencer par une introduction simple et pratique à l’utilisation de tables de montage dans une application Xamarin.Mac. Plus tard, nous allons fournir une explication approfondie de fonctionne de la table de montage et les méthodes utilisées.

Pour cet exemple, nous allons créer une application en fonction de document simple qui gère une fenêtre contenant un affichage de l’image. L’utilisateur sera en mesure de copier et coller des images entre des documents dans l’application et vers ou à partir d’autres applications ou de plusieurs fenêtres à l’intérieur de la même application.

### <a name="creating-the-xamarin-project"></a>Création du projet Xamarin

Tout d’abord, nous allons créer une nouvelle application Xamarin.Mac en fonction de document que nous sera Ajout d’une copie et collez la prise en charge de.

Effectuez ce qui suit :

1. Démarrez Visual Studio pour Mac, puis cliquez sur le **nouveau projet...**  lien.
2. Sélectionnez **Mac** > **application** > **application Cocoa**, puis cliquez sur le **suivant** bouton : 

    [![Création d’un nouveau projet d’application Cocoa](copy-paste-images/sample01.png "création d’un nouveau projet d’application Cocoa")](copy-paste-images/sample01-large.png#lightbox)
3. Entrez `MacCopyPaste` pour le **nom_projet** et gardez toutes les autres comme valeur par défaut. Cliquez sur suivant : 

    [![Définition du nom du projet](copy-paste-images/sample01a.png "définition du nom du projet")](copy-paste-images/sample01a-large.png#lightbox)

4. Cliquez sur le **créer** bouton : 

    [![Confirmer les nouveaux paramètres de projet](copy-paste-images/sample02.png "confirmer les nouveaux paramètres de projet")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Ajouter un NSDocument

Nous allons maintenant ajouter personnalisé `NSDocument` classe qui agira en tant que le stockage en arrière-plan pour l’interface utilisateur de l’application. Il contient une seule vue de l’Image et savoir comment copier une image à partir de la vue dans la table de montage par défaut et comment prendre une image à partir de la table de montage par défaut et l’afficher dans la vue de l’Image.

Avec le bouton droit sur le projet Xamarin.Mac dans le **panneau solutions** et sélectionnez **ajouter** > **nouveau fichier...** :

![Ajout d’un NSDocument au projet](copy-paste-images/sample03.png "Ajout d’un NSDocument au projet")

Entrez `ImageDocument` comme **Nom**, puis cliquez sur le bouton **Nouveau**. Modifier le **ImageDocument.cs** classe et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Jetons un œil à la partie du code en détail ci-dessous.

Le code suivant fournit une propriété à vérifier l’existence des données d’image sur la table de montage par défaut, si une image est disponible, `true` est retourné ; sinon `false`:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Le code suivant copie une image à partir de la vue de l’image jointe dans la table de montage par défaut :

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

Et le code suivant colle une image à partir de la table de montage par défaut et l’affiche dans la vue de l’image jointe (si la table de montage contient une image valide) :

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

Avec ce document en place, nous allons créer l’interface utilisateur de l’application Xamarin.Mac.

### <a name="building-the-user-interface"></a>Création de l’interface utilisateur

Double-cliquez sur le **Main.storyboard** fichier pour l’ouvrir dans Xcode. Ensuite, ajoutez une barre d’outils et une image bien et configurez-les comme suit :

[![Modification de la barre d’outils](copy-paste-images/sample04.png "modification de la barre d’outils")](copy-paste-images/sample04-large.png#lightbox)

Ajoutez une copie et collez **élément de barre d’outils Image** vers le côté gauche de la barre d’outils. Nous allons utiliser en tant que raccourcis pour copier et coller dans le menu Edition. Ensuite, ajoutez quatre **éléments de barre d’outils Image** à droite de la barre d’outils. Nous allons utiliser ceux-ci pour remplir l’image correctement avec certaines images par défaut.

Pour plus d’informations sur l’utilisation des barres d’outils, consultez notre [barres d’outils](~/mac/user-interface/toolbar.md) documentation.

Ensuite, nous allons exposer également les prises de courant et les actions pour les éléments de barre d’outils et de l’image suivante :

[![Création des outlets et actions](copy-paste-images/sample05.png "création outlets et actions")](copy-paste-images/sample05-large.png#lightbox)

Pour plus d’informations sur l’utilisation des outlets et actions, consultez le [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) section de notre [Hello, Mac](~/mac/get-started/hello-mac.md) documentation.

### <a name="enabling-the-user-interface"></a>L’activation de l’interface utilisateur

Avec l’interface utilisateur créée dans Xcode et notre élément d’interface utilisateur exposés via les outlets et actions, nous devons ajouter le code pour activer l’interface utilisateur. Double-cliquez sur le **ImageWindow.cs** de fichiers dans le **panneau solutions** et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Jetons un œil à ce code en détail ci-dessous.

Tout d’abord, nous présentons une instance de la `ImageDocument` classe que nous avons créé ci-dessus :

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

À l’aide de `Export`, `WillChangeValue` et `DidChangeValue`, nous n’avons configuré le `Document` propriété à autoriser pour la liaison de données dans Xcode et le codage de la clé-valeur.

Nous présentons également l’Image à partir de l’image, bien que nous avons ajouté à l’interface utilisateur dans Xcode avec la propriété suivante :

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

Quand la fenêtre principale est chargée et affichée, nous créons une instance de notre `ImageDocument` classe et attacher les images de l’interface utilisateur bien avec le code suivant :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Enfin, en réponse à l’utilisateur clique sur les éléments de barre d’outils copier et coller, nous appelons l’instance de la `ImageDocument` classe pour effectuer le travail proprement dit :

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Activer les menus fichier et Edition

La dernière chose que nous devons faire est d’activer la **New** élément de menu à partir de la **fichier** menu (pour créer des instances de la fenêtre principale) et pour activer la **couper**, **copie**  et **coller** les éléments de menu à partir de la **modifier** menu.

Pour activer la **New** menu Modifier d’élément, le **AppDelegate.cs** fichier, puis ajoutez le code suivant :

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Pour plus d’informations, consultez le [fonctionne avec plusieurs Windows](~/mac/user-interface/window.md) section de notre [Windows](~/mac/user-interface/window.md) documentation.

Pour activer la **couper**, **copie** et **coller** modifier des éléments de menu, le **AppDelegate.cs** fichier, puis ajoutez le code suivant :

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Pour chaque élément de menu, nous obtenir la fenêtre actuelle, plus haut, clé et affectez-le à notre `ImageWindow` classe :

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

À partir de là, nous appelons le `ImageDocument` instance de classe de cette fenêtre pour gérer la copie et collez les actions. Exemple : 

```csharp
window.Document.CopyImage (sender);
```

Nous voulons uniquement **couper**, **copie** et **coller** accessible s’il existe des éléments de menu image des données sur la table de montage par défaut ou dans l’image de la fenêtre actuellement active.

Nous allons ajouter un **EditMenuDelegate.cs** de fichiers au projet Xamarin.Mac et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Là encore, nous obtenir la fenêtre en cours, au premier plan et utiliser ses `ImageDocument` instance de classe pour voir si les données d’image nécessaire existent. Ensuite nous utilisons le `MenuWillHighlightItem` méthode pour activer ou désactiver chaque élément en fonction de cet état.

Modifier le **AppDelegate.cs** et effectuez le `DidFinishLaunching` méthode ressemble ce qui suit :
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Tout d’abord, nous désactivons la mise en service automatique et la désactivation des éléments de menu dans le menu Edition. Ensuite, nous attachons une instance de la `EditMenuDelegate` classe que nous avons créé ci-dessus.

Pour plus d’informations, consultez notre [Menus](~/mac/user-interface/menu.md) documentation.

### <a name="testing-the-app"></a>Test de l’application

Tous les éléments en place, nous sommes prêts à tester l’application. Générer et exécuter l’application et l’interface principale s’affiche :

![Exécution de l’application](copy-paste-images/run01.png "l’application en cours d’exécution")

Si vous ouvrez le menu Edition, notez que **couper**, **copie** et **coller** sont désactivés, car aucune image dans l’image correctement ou existe dans la table de montage par défaut :

![Ouvrir le menu Edition](copy-paste-images/run02.png "ouvrant le menu Edition")

Si vous ajoutez une image à l’image bien et que vous rouvrez le menu Edition, les éléments seront désormais activés :

![Affichage des éléments de menu Modifier sont activés](copy-paste-images/run03.png "montrant les modifier des éléments de menu sont activés")

Si vous copiez l’image et sélectionnez **New** dans le menu fichier, vous pouvez coller cette image dans la nouvelle fenêtre :

![Coller une image dans une nouvelle fenêtre](copy-paste-images/run04.png "coller une image dans une nouvelle fenêtre")

Dans les sections suivantes, nous allons prendre un examen détaillé de l’utilisation de la table de montage dans une application Xamarin.Mac.

## <a name="about-the-pasteboard"></a>Sur la table de montage

Dans macOS (anciennement appelé OS X) la table de montage (`NSPasteboard`) prennent en charge pour plusieurs serveurs traite telles que copier et coller, glisser -déplacer et les Services d’Application. Dans les sections suivantes, nous allons examiner plus en détail plusieurs principaux concepts de table de montage.

### <a name="what-is-a-pasteboard"></a>Qu’est une table de montage ?

Le `NSPasteboard` classe fournit un mécanisme normalisé pour échanger des informations entre les applications ou dans une application donnée. La fonction principale d’une table de montage est pour la gestion des opérations copier et coller :

1. Lorsque l’utilisateur sélectionne un élément dans une application et utilise le **couper** ou **copie** élément de menu, un ou plusieurs représentations de l’élément sélectionné sont placées sur la table de montage.
2. Lorsque l’utilisateur utilise le **coller** élément de menu (dans la même application ou un autre), la version des données qu’il peut traiter est copiée à partir de la table de montage et ajoutée à l’application.

Les utilisations de table de montage moins évidentes incluent find, faites glisser, glisser -déplacer et opérations des services d’application :

- Lorsque l’utilisateur lance une opération glisser, les données de glissement sont copiées vers la table de montage. Si l’opération glisser se termine par une opération déplacer vers une autre application, cette application copie les données à partir de la table de montage.
- Pour les services de traduction, les données doivent être converties sont copiées vers la table de montage par l’application demandeuse. Le service d’application, récupère les données à partir de la table de montage, effectuent la conversion, colle les données de nouveau sur la table de montage.

Dans leur forme la plus simple, les tables de montage sont utilisés pour déplacer des données à l’intérieur d’une application donnée ou d’une application et c’est pourquoi existent dans une zone de mémoire globale spéciale en dehors du processus de l’application. Tandis que les concepts des tables de montage sont facilement saisit, il existe plusieurs détails plus complexes qui doivent être pris en compte. Ils seront traités en détail ci-dessous.

### <a name="named-pasteboards"></a>Tables de montage nommés

Une table de montage peut être publiques ou privées et peut-être être utilisé pour différents objectifs au sein d’une application ou entre plusieurs applications. macOS fournit les tables de montage de standards plusieurs, chacun avec une utilisation spécifique et bien définie :

- `NSGeneralPboard` -La table de montage par défaut pour **couper**, **copie** et **coller** operations.
- `NSRulerPboard` -Prend en charge **couper**, **copie** et **coller** opérations sur **règles**.
- `NSFontPboard` -Prend en charge **couper**, **copie** et **coller** opérations sur `NSFont` objets.
- `NSFindPboard` -Prend en charge l’application spécifique recherche des panneaux pouvant partager texte recherché.
- `NSDragPboard` -Prend en charge **glisser -déplacer** operations.

Pour la plupart des cas, vous allez utiliser une des tables de montage défini par le système. Mais il peut arriver que vous devez créer vos propres tables de montage. Dans ces situations, vous pouvez utiliser la `FromName (string name)` méthode de la `NSPasteboard` classe pour créer une table de montage personnalisé portant le nom spécifié.

Si vous le souhaitez, vous pouvez appeler la `CreateWithUniqueName` méthode de la `NSPasteboard` classe pour créer une table de montage nommé de manière unique.

### <a name="pasteboard-items"></a>Éléments de la table de montage

Chaque élément de données une application écrit dans une table de montage est considéré comme un _élément de la table de montage_ et une table de montage peut contenir plusieurs éléments en même temps. De cette façon, une application peut écrire plusieurs versions des données copiées dans une table de montage (par exemple, texte brut et texte mis en forme) et l’application lors de la récupération peuvent lire uniquement les données qu’il peut traiter (par exemple, le texte brut uniquement).

### <a name="data-representations-and-uniform-type-identifiers"></a>Représentations sous forme de données et les identificateurs de type uniforme

Opérations de table de montage prennent généralement lieu entre les deux (ou plus) les applications qui ont aucune connaissance des uns des autres ou les types de données que chacun peut gérer. Comme indiqué dans la section précédente, afin d’optimiser le potentiel pour le partage d’informations, une table de montage peut contenir plusieurs représentations sous forme de données copié et collé.

Chaque représentation est identifiée via un Uniform Type identificateur (UTI), qui n’est rien de plus qu’une simple chaîne qui identifie de façon unique le type de date en cours de présentation (pour plus d’informations, consultez le site d’Apple [vue d’ensemble des identificateurs de Type uniforme ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) documentation). 

Si vous créez un type de données personnalisé (par exemple, un objet de dessin d’un vecteur d’application de dessin), vous pouvez créer vos propres UTI pour identifier dans la copie et les opérations de collage de manière unique.

Lorsqu’une application prépare coller des données copiées à partir d’une table de montage, il doit rechercher la représentation qui convient le mieux à ses capacités (le cas échéant). En général, il s’agit du type plus riche disponible (par exemple de mise en forme de texte pour une application de traitement de texte), revenir aux écrans plus simple disponibles en tant que (brut texte requis pour un simple éditeur de texte).

<a name="Promised_Data" />

### <a name="promised-data"></a>Données promises

En règle générale, vous devez fournir des représentations autant de données copiées que possible afin d’optimiser le partage entre les applications. Toutefois, en raison de contraintes de temps ou de la mémoire, il peut être difficile d’écrire chaque type de données dans la table de montage.

Dans ce cas, vous pouvez placer la première représentation des données sur la table de montage et l’application réceptrice peut demander une représentation différente, ce qui peut être généré à la volée juste avant l’opération de collage.

Lorsque vous placez l’élément initial dans la table de montage, vous spécifiez qu’un ou plusieurs des autres représentations disponibles sont fournies par un objet qui est conforme à la `NSPasteboardItemDataProvider` interface. Ces objets fournit les représentations sous forme supplémentaires à la demande, comme demandé par l’application réceptrice.

### <a name="change-count"></a>Nombre de modifications

Chaque table de montage conserve un _nombre de modifications_ qu’augmente à chaque fois un nouveau propriétaire est déclaré. Une application peut déterminer si les contenu de la table de montage ont été modifiés depuis la dernière fois qu’il l’examiné en vérifiant la valeur du nombre de modifications.

Utilisez le `ChangeCount` et `ClearContents` méthodes de la `NSPasteboard` classe pour modifier le nombre de modifications d’une table de montage donné.

## <a name="copying-data-to-a-pasteboard"></a>Copie de données vers une table de montage

Vous effectuez une opération de copie en premier l’accès à une table de montage, effacer le contenu existant et en écrivant des représentations autant de données que vous êtes obligé de la table de montage.

Exemple :

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

En règle générale, vous serez prêt simplement à écrire à la table de montage général, comme nous l’avons fait dans l’exemple ci-dessus. N’importe quel objet que vous envoyez à la `WriteObjects` méthode *doit* est conforme à la `INSPasteboardWriting` interface. Plusieurs, classes intégrées (telles que `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, et `NSPasteboardItem`) se conformer à cette interface.

Si vous écrivez une classe de données personnalisée dans la table de montage il doit être conforme à la `INSPasteboardWriting` interface ou être encapsulés dans une instance de la `NSPasteboardItem` classe (voir la [des Types de données personnalisés](#Custom_Data_Types) section ci-dessous).

## <a name="reading-data-from-a-pasteboard"></a>Lecture des données à partir d’une table de montage

Comme indiqué ci-dessus, pour optimiser le potentiel pour partager des données entre les applications, plusieurs représentations des données copiées peuvent être écrites à la table de montage. C’est à l’application réceptrice pour sélectionner la version la plus riche possible pour ses fonctionnalités (le cas échéant).

### <a name="simple-paste-operation"></a>Opération de collage simple

Lire les données à partir de la table de montage à l’aide du `ReadObjectsForClasses` (méthode). Il requiert deux paramètres :

1. Un tableau de `NSObject` en fonction des types de classe que vous souhaitez lire à partir de la table de montage. Vous devez commander cela avec le type de données plus souhaité tout d’abord, avec tous les types restants en ordre de préférence décroissant.
2. Un dictionnaire qui contient des contraintes supplémentaires (par exemple pour limiter à certains types de contenu d’URL) ou un dictionnaire vide si aucune des contraintes ultérieures ne sont requis.

La méthode retourne un tableau d’éléments qui répondent aux critères que nous avons transmis dans et par conséquent contient au maximum le même nombre de types de données qui sont demandées. Il est également possible qu’aucun des types requis sont présents et un tableau vide s’affichera.

Par exemple, le code suivant vérifie si un `NSImage` existe dans la table de montage général et l’affiche dans une image bien si c’est le cas :

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Demande de plusieurs types de données

Selon le type d’application Xamarin.Mac en cours de création, il peut être en mesure de gérer plusieurs représentations des données en cours de collage. Dans ce cas, il existe deux scénarios de récupération des données à partir de la table de montage :

1. Effectuer un appel unique à la `ReadObjectsForClasses` (méthode) et en fournissant un tableau de toutes les représentations sous forme que vous désirez (dans l’ordre de préférence).
2. Effectuer plusieurs appels à la `ReadObjectsForClasses` méthode demandant un tableau différent de types chaque fois.

Consultez le **Simple opération de collage** section ci-dessus pour plus d’informations sur la récupération de données à partir d’une table de montage.

### <a name="checking-for-existing-data-types"></a>Vérification des types de données existants

Il se peut que vous pouvez souhaiter vérifier si une table de montage contient une représentation sous forme de données donnée sans le lire réellement les données à partir de la table de montage (telles que l’activation du **coller** élément de menu uniquement lorsqu’il existe des données valides).

Appelez le `CanReadObjectForClasses` (méthode) de la table de montage pour voir si elle contient un type donné.

Par exemple, le code suivant détermine si la table de montage général contient un `NSImage` instance :

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>URL de lecture à partir de la table de montage

En fonction de la fonction d’une application Xamarin.Mac donnée, il peut être nécessaire pour lire des URL à partir d’une table de montage, mais uniquement si elles répondent à un ensemble donné de critères (par exemple, pointant vers des fichiers ou des URL d’un type de données spécifique). Dans ce cas, vous pouvez spécifier des critères de recherche supplémentaires à l’aide de la deuxième paramètre de la `CanReadObjectForClasses` ou `ReadObjectsForClasses` méthodes.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Types de données personnalisés

Il est parfois lorsque vous devez enregistrer vos propres types personnalisés à la table de montage à partir d’une application Xamarin.Mac. Par exemple, un dessin vectoriel application qui lui permet de copier et coller les objets de dessin.

Dans ce cas, vous devez concevoir votre classe personnalisée de données afin qu’elle hérite de `NSObject` et il est conforme aux quelques interfaces (`INSCoding`, `INSPasteboardWriting` et `INSPasteboardReading`). Si vous le souhaitez, vous pouvez utiliser un `NSPasteboardItem` pour encapsuler les données pour être copiées ou collées.

Ces deux options seront abordées en détail ci-dessous.

### <a name="using-a-custom-class"></a>À l’aide d’une classe personnalisée

Dans cette section nous seront en développant l’exemple simple d’application que nous avons créé au début de ce document et ajout d’une classe personnalisée pour effectuer le suivi des informations sur l’image que nous allons copier et coller entre les fenêtres.

Ajoutez une nouvelle classe au projet et appelez-la **ImageInfo.cs**. Modifiez le fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

Dans les sections suivantes, nous allons description détaillée de cette classe.

#### <a name="inheritance-and-interfaces"></a>L’héritage et interfaces

Avant d’une classe de données personnalisées permettre être écrite ou lues à partir d’une table de montage, il doit être conforme à la `INSPastebaordWriting` et `INSPasteboardReading` interfaces. En outre, il doit hériter de `NSObject` et être conforme à la `INSCoding` interface :

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

La classe doit également être exposée à Objective-C à l’aide de la `Register` directive et il doivent exposer les propriétés requises ou les méthodes à l’aide de `Export`. Exemple :

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Nous avons exposé par les deux champs de données qui contient cette classe - nom de l’image et son type (jpg, png, etc.). 

Pour plus d’informations, consultez le [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) documentation, elle explique la `Register` et `Export` attributs utilisé pour structurer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

#### <a name="constructors"></a>Constructeurs

Deux constructeurs (exposés correctement et Objective-C) sera nécessaire pour notre classe de données personnalisée afin qu’il peut être lu à partir d’une table de montage :

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

Tout d’abord, nous exposons les _vide_ constructeur sous la méthode par défaut pour Objective-C `init`.

Ensuite, nous présentons un `NSCoding` constructeur conforme permettant de créer une nouvelle instance de l’objet à partir de la table de montage lors du collage sous le nom exporté de `initWithCoder`.

Ce constructeur prend un `NSCoder` (tel que créé par un `NSKeyedArchiver` lorsque écrit dans la table de montage), extrait les données de clé/valeur associé et l’enregistre dans les champs de propriété de la classe de données.

#### <a name="writing-to-the-pasteboard"></a>Écriture dans la table de montage

En se conformant à la `INSPasteboardWriting` interface, nous devons exposer deux méthodes et éventuellement une troisième méthode, afin que la classe peut être écrites dans la table de montage.

Tout d’abord, nous devons indiquer à la table de montage le type de données représentations sous forme de la classe personnalisée peut être écrits dans :

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Chaque représentation est identifiée via un Uniform Type identificateur (UTI), qui n’est rien de plus qu’une simple chaîne qui identifie de façon unique le type de données en cours de présentation (pour plus d’informations, consultez le site d’Apple [vue d’ensemble des identificateurs de Type uniforme ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) documentation).

Pour notre format personnalisé, nous créons notre propre UTI : « com.xamarin.image-info » (Remarque qui se trouve dans la notation inverse comme un identificateur d’application). Notre classe est également capable d’écrire une chaîne standard pour la table de montage (`public.text`). 

Ensuite, nous devons créer l’objet dans le format demandé en fait écrite sur la table de montage :

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

Pour le `public.text` type, nous allons retourner une simple, au format `NSString` objet. Personnalisée du `com.xamarin.image-info` type, nous utilisons un `NSKeyedArchiver` et `NSCoder` interface pour encoder la classe de données personnalisées à un archivage de clé/valeur associé. Nous devrons implémenter la méthode suivante pour gérer l’encodage :

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Les paires clé/valeur individuelles sont écrits à l’encodeur et va être décodés à l’aide de la deuxième constructeur que nous avons ajouté au-dessus.

Si vous le souhaitez, nous pouvons inclure la méthode suivante pour définir des options lors de l’écriture des données à la table de montage :

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Actuellement, seule la `WritingPromised` option est disponible et doit être utilisée quand un type donné est promis uniquement et pas réellement écrit dans la table de montage. Pour plus d’informations, consultez le [données promis](#Promised_Data) section ci-dessus.

Avec ces méthodes en place, le code suivant peut être utilisé pour écrire notre classe personnalisée dans la table de montage :

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Lecture à partir de la table de montage

En se conformant à la `INSPasteboardReading` interface, nous devons exposer trois méthodes pour que la classe de données personnalisées peut être lues à partir de la table de montage.

Tout d’abord, nous devons indiquer à la table de montage le type de données représentations sous forme de la classe personnalisée capable de lire à partir du Presse-papiers :

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Là encore, ceux-ci sont définis comme UTI simple et sont les mêmes types que nous avons défini dans le **écriture dans la table de montage** section ci-dessus.

Ensuite, nous devons indiquer la table de montage _comment_ chacun des types UTI est lues à l’aide de la méthode suivante :

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

Pour le `com.xamarin.image-info` type, nous indiquons à la table de montage pour décoder la paire clé/valeur que nous avons créé avec le `NSKeyedArchiver` pour la classe de l’écriture à la table de montage en appelant le `initWithCoder:` constructeur que nous avons ajouté à la classe.

Enfin, nous devons ajouter la méthode suivante pour lire les autres représentations de données UTI à partir de la table de montage :

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

Avec toutes ces méthodes en place, la classe de données personnalisées peut être lues depuis la table de montage en utilisant le code suivant :

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>À l’aide d’un NSPasteboardItem

Il peut arriver lorsque vous avez besoin écrire des éléments personnalisés dans la table de montage qui ne garantissent pas la création d’une classe personnalisée ou que vous souhaitez fournir des données dans un format commun, uniquement en fonction des besoins. Pour ces situations, vous pouvez utiliser un `NSPasteboardItem`.

Un `NSPasteboardItem` fournit un contrôle affiné sur les données qui sont écrites dans la table de montage et sont conçues pour un accès temporaire - il doit être supprimé après qu’il a été écrit dans la table de montage.

#### <a name="writing-data"></a>Écriture de données

Pour écrire vos données personnalisées à un `NSPasteboardItem` vous devez fournir un personnalisé `NSPasteboardItemDataProvider`. Ajoutez une nouvelle classe au projet et appelez-la **ImageInfoDataProvider.cs**. Modifiez le fichier et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Comme nous l’avons fait avec la classe de données personnalisée, nous devons utiliser le `Register` et `Export` directives pour l’exposer aux Objective-C. La classe doit hériter `NSPasteboardItemDataProvider` et doit implémenter la `FinishedWithDataProvider` et `ProvideDataForType` méthodes.

Utilisez le `ProvideDataForType` méthode pour fournir les données seront encapsulées dans le `NSPasteboardItem` comme suit :

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

Dans ce cas, nous utilisons les stocker deux types d’informations sur notre image (nom et ImageType) et ceux en une chaîne simple écriture (`public.text`).

Type d’écrire les données dans la table de montage, utilisez le code suivant :

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Lecture de données

Pour lire les données à partir de la table de montage, utilisez le code suivant :

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation de la table de montage dans une application Xamarin.Mac pour prendre en charge la copie et les opérations de collage. Tout d’abord, il introduit un exemple simple pour vous familiariser avec les opérations de tables de montage standard. Ensuite, il a fallu description détaillée de la table de montage et comment lire et écrire des données à partir de celui-ci. Enfin, il examiné à l’aide d’un type de données personnalisé pour prendre en charge la copie et le collage des types de données complexes au sein d’une application.



## <a name="related-links"></a>Liens associés

- [MacCopyPaste (sample)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Guide de programmation table de montage](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [Human Interface Guidelines pour macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
