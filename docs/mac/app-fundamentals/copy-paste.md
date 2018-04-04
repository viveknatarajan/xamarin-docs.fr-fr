---
title: en effectuant un copier-coller ;
description: Cet article décrit l’utilisation de la table de montage pour fournir de copie et de coller dans une application Xamarin.Mac. Il montre comment travailler avec les types de données standard qui peuvent être partagés entre plusieurs applications et la prise en charge des données personnalisées dans une application donnée.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cf81666403f687ce997e20f6f5f097dc9fcf1421
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="copy-and-paste"></a>en effectuant un copier-coller ;

_Cet article décrit l’utilisation de la table de montage pour fournir de copie et de coller dans une application Xamarin.Mac. Il montre comment travailler avec les types de données standard qui peuvent être partagés entre plusieurs applications et la prise en charge des données personnalisées dans une application donnée._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même table de montage (copier et coller) prise en charge dont dispose un développeur travaillant dans Objective-C.

Dans cet article que nous aborderons les deux façons d’utiliser la table de montage dans une application Xamarin.Mac :

1. **Types de données standard** -étant donné que les opérations de table de montage sont généralement effectuées entre les deux applications non liées, aucune application connaît les types de données prenant en charge l’autre. Pour optimiser le risque de partage, la table de montage peut contenir plusieurs représentations sous forme d’un élément donné (à l’aide d’un jeu standard de types de données courants), ainsi l’application consommatrice de choisir la version qui convient le mieux à ses besoins.
2. **Données personnalisées** - pour prendre en charge la copie et collage de données complexes dans votre Xamarin.Mac que vous pouvez définir un type de données personnalisé qui sera géré par la table de montage. Par exemple, une application de dessin vectoriel qui permet à l’utilisateur de copier et coller des formes complexes qui sont composées de plusieurs types de données et les points.

[![Exemple de l’application en cours d’exécution](copy-paste-images/intro01.png "exemple de l’application en cours d’exécution")](copy-paste-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de la table de montage dans une application Xamarin.Mac pour prendre en charge la copie et les opérations de collage. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` attributs utilisé pour associer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

## <a name="getting-started-with-the-pasteboard"></a>Prise en main de la table de montage

La table de montage présente un mécanisme normalisé pour échanger des données dans une application donnée, ou entre des applications. L’utilisation classique pour une table de montage dans une application Xamarin.Mac consiste à gérer les copier et coller, mais un nombre d’autres opérations est également prises en charge (par exemple, faites glisser & déplacer et les Services d’Application).

Pour vous aider à rien rapidement, nous allons commencer par une présentation simple et pratique, à l’aide des tables de montage dans une application Xamarin.Mac. Une version ultérieure, nous fournissons une explication détaillée du fonctionne de la table de montage et les méthodes utilisées.

Pour cet exemple, nous créerez une application en fonction de document simple qui gère une fenêtre contenant une vue de l’image. L’utilisateur sera en mesure de copier et coller des images entre des documents dans l’application et vers ou à partir d’autres applications ou de plusieurs fenêtres à l’intérieur de la même application.

### <a name="creating-the-xamarin-project"></a>Création du projet Xamarin

Tout d’abord, nous allons créer une nouvelle application Xamarin.Mac en fonction de document que nous allons Ajout de copie et collez la prise en charge de.

Effectuez ce qui suit :

1. Démarrez Visual Studio pour Mac et cliquez sur le **nouveau projet...**  lien.
2. Sélectionnez **Mac** > **application** > **/Cocoa application**, puis cliquez sur le **suivant** bouton : 

    [![Création d’un projet d’application/Cocoa](copy-paste-images/sample01.png "création d’un projet d’application/Cocoa")](copy-paste-images/sample01-large.png#lightbox)
3. Entrez `MacCopyPaste` pour le **nom du projet** et conserver tout le reste comme valeur par défaut. Cliquez sur suivant : 

    [![Définition du nom du projet](copy-paste-images/sample01a.png "la définition du nom du projet")](copy-paste-images/sample01a-large.png#lightbox)

4. Cliquez sur le **créer** bouton : 

    [![Confirmer les nouveaux paramètres de projet](copy-paste-images/sample02.png "confirmer les nouveaux paramètres de projet")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Ajouter un NSDocument

Nous allons ajouter ensuite personnalisé `NSDocument` classe qui agira en tant que le stockage en arrière-plan pour l’interface utilisateur de l’application. Il contient une seule vue de l’Image et savoir comment copier une image à partir de la vue dans la table de montage par défaut et comment capturer une image à partir de la table de montage par défaut et l’afficher dans la vue de l’Image.

Avec le bouton droit sur le projet Xamarin.Mac dans les **Solution remplissage** et sélectionnez **ajouter** > **nouveau fichier...** :

![Ajout d’un NSDocument au projet](copy-paste-images/sample03.png "Ajout d’un NSDocument au projet")

Entrez `ImageDocument` comme **Nom**, puis cliquez sur le bouton **Nouveau**. Modifier la **ImageDocument.cs** de classe et le rendre l’aspect suivant :

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

Examinons une partie du code en détail ci-dessous.

Le code suivant fournit une propriété pour tester l’existence de données de l’image sur la table de montage par défaut, si une image est disponible, `true` est retourné else `false`:

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

Double-cliquez sur le **Main.storyboard** fichier et ouvrez-le dans Xcode. Ensuite, ajoutez une barre d’outils et d’une image de bien et configurez-les comme suit :

[![La barre d’outils](copy-paste-images/sample04.png "la barre d’outils")](copy-paste-images/sample04-large.png#lightbox)

Ajouter une copie et collez **élément de barre d’outils Image** sur le côté gauche de la barre d’outils. Nous allez utiliser ces raccourcis pour copier et coller dans le menu Edition. Ensuite, ajoutez quatre **des éléments de barre d’outils Image** à droite de la barre d’outils. Nous allons utiliser ceux-ci pour remplir l’image correctement avec certaines images par défaut.

Pour plus d’informations sur l’utilisation des barres d’outils, consultez notre [barres d’outils](~/mac/user-interface/toolbar.md) documentation.

Ensuite, nous allons également exposer les prises de courant et les actions pour les éléments de barre d’outils et de l’image suivante :

[![Création de points de vente et les actions](copy-paste-images/sample05.png "création prises et actions")](copy-paste-images/sample05-large.png#lightbox)

Pour plus d’informations sur l’utilisation des points de vente et les actions, consultez la [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) section de notre [Hello, Mac](~/mac/get-started/hello-mac.md) documentation.

### <a name="enabling-the-user-interface"></a>L’activation de l’interface utilisateur

Avec l’interface utilisateur créé dans Xcode et notre élément d’interface utilisateur exposés via les sorties et les actions, nous devons ajouter le code pour activer l’interface utilisateur. Double-cliquez sur le **ImageWindow.cs** de fichiers dans le **Solution remplissage** et qu’elle ressemble à ce qui suit :

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

Examinons ce code en détail ci-dessous.

Tout d’abord, nous présentons une instance de la `ImageDocument` classe que nous avons créés ci-dessus :

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

À l’aide de `Export`, `WillChangeValue` et `DidChangeValue`, nous avons le programme d’installation le `Document` propriété permettant de codage de la clé-valeur et la liaison de données dans Xcode.

Nous présentons également l’Image à partir de l’image, bien que nous avons ajouté à notre interface utilisateur dans Xcode avec la propriété suivante :

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

Lorsque la fenêtre principale est chargée et affichée, nous créons une instance de notre `ImageDocument` classe et attacher les images de l’interface utilisateur bien avec le code suivant :

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

Enfin, en réponse à l’utilisateur en cliquant sur les éléments de barre d’outils de copie et collage, nous appelons l’instance de la `ImageDocument` classe pour effectuer le travail réel :

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Activer les menus fichier et Edition

La dernière chose que nous devons faire est de permettre la **nouveau** élément de menu à partir de la **fichier** menu (pour créer des instances de notre fenêtre principale) et pour activer la **couper**, **copie**  et **coller** des éléments de menu à partir de la **modifier** menu.

Pour activer la **nouveau** menu item, modifier le **AppDelegate.cs** et ajoutez le code suivant :

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

Pour plus d’informations, consultez la [fonctionne avec plusieurs fenêtres](~/mac/user-interface/window.md) section de notre [Windows](~/mac/user-interface/window.md) documentation.

Pour activer la **couper**, **copie** et **coller** modifier les éléments de menu, le **AppDelegate.cs** et ajoutez le code suivant :

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

Pour chaque élément de menu, nous obtenir la fenêtre en cours, au premier plan, key et effectuez un cast à notre `ImageWindow` classe :

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

À partir de là, nous appelons le `ImageDocument` instance de la classe de cette fenêtre pour gérer la copie et collez les actions. Par exemple : 

```csharp
window.Document.CopyImage (sender);
```

Nous voulons uniquement **couper**, **copie** et **coller** données sur la table de montage par défaut ou dans l’image de la fenêtre active de l’image doit être accessible s’il existe des éléments de menu.

Ajoutons un **EditMenuDelegate.cs** de fichiers au projet Xamarin.Mac et le rendre l’aspect suivant :

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

Là encore, nous obtenir la fenêtre au premier plan actuelle et ses `ImageDocument` instance de la classe pour voir si les données d’image requis existent. Ensuite, nous utilisons le `MenuWillHighlightItem` méthode pour activer ou désactiver chaque élément en fonction de cet état.

Modifier la **AppDelegate.cs** et effectuez le `DidFinishLaunching` détaillée de la méthode comme suit :
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Tout d’abord, nous désactivons l’activation automatique et la désactivation des éléments de menu dans le menu Edition. Ensuite, nous attacher une instance de la `EditMenuDelegate` classe que nous avons créés ci-dessus.

Pour plus d’informations, consultez notre [Menus](~/mac/user-interface/menu.md) documentation.

### <a name="testing-the-app"></a>Test de l’application

Tous les éléments en place, nous sommes prêts tester l’application. Générer et exécuter l’application et l’interface principale s’affiche :

![Exécution de l’application](copy-paste-images/run01.png "l’application en cours d’exécution")

Si vous ouvrez le menu Edition, notez que **couper**, **copie** et **coller** sont désactivés, car il n’existe aucune image dans l’image bien ou dans la table de montage par défaut :

![Ouvrir le menu Edition](copy-paste-images/run02.png "ouvrir le menu Edition")

Si vous ajoutez une image à l’image correctement et que vous rouvrez le menu Edition, les éléments seront maintenant activés :

![Montrant les modifier des éléments de menu sont activés](copy-paste-images/run03.png "montrant les modifier des éléments de menu sont activés.")

Si vous copiez l’image et que vous sélectionnez **nouveau** dans le menu fichier, vous pouvez coller cette image dans la nouvelle fenêtre :

![Coller une image dans une nouvelle fenêtre](copy-paste-images/run04.png "coller une image dans une nouvelle fenêtre")

Dans les sections suivantes, nous allons prendre une présentation détaillée sur l’utilisation de la table de montage dans une application Xamarin.Mac.

## <a name="about-the-pasteboard"></a>Sur la table de montage

Dans macOS (anciennement OS X) la table de montage (`NSPasteboard`) fournissent la prise en charge pour plusieurs serveur traite comme copier et coller, glisser-déplacer et les Services d’Application. Dans les sections suivantes, nous allons étudier plusieurs concepts clés de table de montage.

### <a name="what-is-a-pasteboard"></a>Qu’est une table de montage ?

La `NSPasteboard` classe fournit un mécanisme normalisé pour échanger des informations entre des applications ou dans une application donnée. La principale fonction d’une table de montage est pour la gestion des opérations de copier- coller :

1. Lorsque l’utilisateur sélectionne un élément dans une application et qu’il utilise le **couper** ou **copie** élément de menu, un ou plusieurs des représentations sous forme de l’élément sélectionné est placés sur la table de montage.
2. Lorsque l’utilisateur utilise le **coller** élément de menu (dans la même application ou une autre), la version des données qu’il peut gérer est copiée à partir de la table de montage et ajoutée à l’application.

Utilisations de table de montage moins évidentes incluent rechercher, faites glisser, glisser -déplacer, et les opérations des services d’application :

- Lorsque l’utilisateur lance une opération glisser, les données de glissement sont copiées vers la table de montage. Si l’opération de glissement se termine par une suppression sur une autre application, cette application copie les données à partir de la table de montage.
- Services de traduction, les données doivent être converties sont copiées dans la table de montage par l’application demandeuse. Le service d’application, récupère les données à partir de la table de montage, effectue la traduction, colle les données de nouveau sur la table de montage.

Dans leur forme la plus simple, les tables de montage sont utilisés pour déplacer des données à l’intérieur d’une application donnée, ou entre les applications et, par conséquent, existent dans une zone de mémoire globale spéciale en dehors du processus de l’application. Alors que les concepts des tables de montage sont facilement saisit, il existe plusieurs détails plus complexes doivent être prises en compte. Ils seront traités en détail ci-dessous.

### <a name="named-pasteboards"></a>Tables de montage nommés

Une table de montage peut être public ou privé et peut-être servir à différentes fins dans une application ou entre plusieurs applications. macOS fournit les tables de montage de standards plusieurs, chacun avec une utilisation spécifique et bien définie :

- `NSGeneralPboard` -La table de montage par défaut pour **couper**, **copie** et **coller** operations.
- `NSRulerPboard` -Prend en charge **couper**, **copie** et **coller** opérations sur **des règles**.
- `NSFontPboard` -Prend en charge **couper**, **copie** et **coller** opérations sur `NSFont` objets.
- `NSFindPboard` -Prend en charge le spécifiques à l’application de trouver des panneaux peuvent partager rechercher du texte.
- `NSDragPboard` -Prend en charge **glisser -déplacer** operations.

Pour la plupart des cas, vous allez utiliser parmi les tables de montage définies par le système. Mais il peut arriver que vous oblige à créer vos propres tables de montage. Dans ces situations, vous pouvez utiliser la `FromName (string name)` méthode de la `NSPasteboard` classe pour créer une table de montage personnalisé portant le nom donné.

Si vous le souhaitez, vous pouvez appeler la `CreateWithUniqueName` méthode de la `NSPasteboard` classe pour créer une table de montage nommé de manière unique.

### <a name="pasteboard-items"></a>Éléments de la table de montage

Chaque élément de données une application écrit dans une table de montage est considéré comme un _élément de la table de montage_ et une table de montage peut contenir plusieurs éléments en même temps. De cette façon, une application peut écrire plusieurs versions des données sont copiées vers une table de montage (par exemple, texte brut et texte mis en forme) et de l’application lors de la récupération peuvent lire uniquement les données qu’il peut traiter (par exemple, le texte brut uniquement).

### <a name="data-representations-and-uniform-type-identifiers"></a>Représentations sous forme de données et les identificateurs de type uniforme

Opérations de table de montage prennent généralement lieu entre les deux (ou plus) les applications qui n’ont aucune connaissance de l’autre ou les types de données que chacun peut gérer. Comme indiqué dans la section ci-dessus, pour optimiser le potentiel pour partager des informations, une table de montage peut contenir plusieurs représentations des données qui est copié et collé.

Chaque représentation est identifiée via un uniforme Type identificateur (utilitaire), qui n’est rien de plus qu’une simple chaîne qui identifie de façon unique le type de date en cours de présentation (pour plus d’informations, consultez le site d’Apple [vue d’ensemble des identificateurs de Type uniforme ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) documentation). 

Si vous créez un type de données personnalisé (par exemple, un objet de dessin dans un application de dessin de vecteur), vous pouvez créer votre propre utilitaire pour identifier dans la copie de manière unique et les opérations de collage.

Lorsqu’une application se prépare à coller des données copiées à partir d’une table de montage, il doit rechercher la représentation sous forme de qui répond le mieux à ses capacités (le cas échéant). En général, il s’agit du type riche disponible (par exemple au format texte pour une application de traitement de texte), retour aux plus simple de formulaires disponibles requis (en texte brut pour un simple éditeur de texte).

<a name="Promised_Data" />

### <a name="promised-data"></a>Données promises

En règle générale, vous devez fournir des représentations autant de données copiées que possible afin d’optimiser le partage entre les applications. Toutefois, en raison des contraintes de temps ou de mémoire, il peut être difficile d’écrire chaque type de données dans la table de montage.

Dans ce cas, vous pouvez placer la première représentation des données sur la table de montage et l’application réceptrice peut demander une représentation différente, ce qui peut être généré à la volée juste avant l’opération de collage.

Lorsque vous placez l’élément initial dans la table de montage, vous spécifiez qu’un ou plusieurs des autres représentations disponibles sont fournies par un objet qui est conforme à la `NSPasteboardItemDataProvider` interface. Ces objets fournit les représentations sous forme supplémentaires à la demande, tel que demandé par l’application réceptrice.

### <a name="change-count"></a>Nombre de modifications

Chaque table de montage conserve un _nombre de modifications_ qu’est incrémentée chaque fois un nouveau propriétaire est déclaré. Une application peut déterminer si le contenu de la table de montage ont été modifiés depuis la dernière fois qu’il l’examiné en vérifiant la valeur du nombre de modifications.

Utilisez le `ChangeCount` et `ClearContents` méthodes de la `NSPasteboard` classe pour modifier le nombre de modifications d’une table de montage donné.

## <a name="copying-data-to-a-pasteboard"></a>Copie de données dans une table de montage

Vous effectuez une opération de copie en premier l’accès à une table de montage, effacer le contenu existant et l’écriture des représentations autant de données que vous êtes obligé de la table de montage.

Par exemple :

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

En règle générale, vous allez simplement écrire dans la table de montage général, comme nous l’avons fait dans l’exemple ci-dessus. N’importe quel objet que vous envoyez à la `WriteObjects` méthode *doit* est conforme à la `INSPasteboardWriting` interface. Plusieurs, classes intégrés (tels que `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, et `NSPasteboardItem`) sont automatiquement conformes à cette interface.

Si vous écrivez une classe de données personnalisée dans la table de montage il doit être conforme à la `INSPasteboardWriting` interface ou être encapsulées dans une instance de la `NSPasteboardItem` classe (voir le [des Types de données personnalisés](#Custom_Data_Types) section ci-dessous).

## <a name="reading-data-from-a-pasteboard"></a>La lecture des données à partir d’une table de montage

Comme indiqué ci-dessus, pour optimiser le potentiel de partage des données entre les applications, plusieurs représentations des données copiées peuvent être écrit à la table de montage. C’est à l’application réceptrice pour sélectionner la version la plus riche possible pour ses fonctionnalités (le cas échéant).

### <a name="simple-paste-operation"></a>Opération de collage simple

Lire des données à partir de la table de montage à l’aide du `ReadObjectsForClasses` (méthode). Il requiert deux paramètres :

1. Un tableau de `NSObject` en fonction des types de classe que vous souhaitez lire à partir de la table de montage. Vous devez commander ce avec le type de données plus souhaité en premier lieu, avec les types restants dans préférentiel décroissant.
2. Un dictionnaire contenant des contraintes supplémentaires (par exemple pour limiter à certains types de contenu URL) ou un dictionnaire vide si aucune des contraintes supplémentaires ne sont nécessaires.

La méthode retourne un tableau d’éléments qui répondent aux critères que nous avons passés dans et par conséquent contient au maximum le même nombre de types de données qui sont demandées. Il est également possible qu’aucun des types demandés sont présents et un tableau vide est renvoyé.

Par exemple, le code suivant vérifie si un `NSImage` existe dans la table de montage général et l’affiche dans une image bien dans ce cas :

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

1. Effectuez un appel unique à la `ReadObjectsForClasses` (méthode) et en fournissant un tableau de toutes les représentations sous forme que vous voulez (dans l’ordre de préférence).
2. Effectuer plusieurs appels à la `ReadObjectsForClasses` méthode lui demandant d’un autre tableau de types de chaque fois.

Consultez le **Simple opération de collage** section ci-dessus pour plus d’informations sur la récupération de données à partir d’une table de montage.

### <a name="checking-for-existing-data-types"></a>La vérification des types de données existants

Il se peut que vous pouvez souhaiter vérifier si une table de montage contient une représentation sous forme de données spécifiée sans le lire réellement les données à partir de la table de montage (telles que l’activation du **coller** élément de menu seulement s’il existe des données valides).

Appelez le `CanReadObjectForClasses` (méthode) de la table de montage pour voir s’il contient un type donné.

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

### <a name="reading-urls-from-the-pasteboard"></a>URL de la lecture à partir de la table de montage

En fonction de la fonction d’une application Xamarin.Mac donnée, il peut être nécessaires pour lire les URL à partir d’une table de montage, mais uniquement si elles répondent à un ensemble donné de critères (par exemple, en pointant sur les fichiers ou les URL d’un type de données spécifique). Dans ce cas, vous pouvez spécifier des critères de recherche supplémentaires à l’aide de la deuxième paramètre de la `CanReadObjectForClasses` ou `ReadObjectsForClasses` méthodes.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Types de données personnalisés

Il existe des cas lorsque vous devez enregistrer vos propres types personnalisés pour la table de montage à partir d’une application Xamarin.Mac. Par exemple, un dessin vectoriel application qui lui permet de copier et coller dessiner des objets.

Dans ce cas, vous devez concevoir votre classe personnalisée de données afin qu’elle hérite de `NSObject` et il est conforme aux interfaces quelques (`INSCoding`, `INSPasteboardWriting` et `INSPasteboardReading`). Si vous le souhaitez, vous pouvez utiliser un `NSPasteboardItem` pour encapsuler les données pour être copiées ou collées.

Ces deux options seront abordées en détail ci-dessous.

### <a name="using-a-custom-class"></a>À l’aide d’une classe personnalisée

Dans cette section nous être développant sur l’exemple simple d’application que vous avez créée au début de ce document et ajout d’une classe personnalisée pour le suivi des informations sur l’image que nous allons copier et coller entre les fenêtres.

Ajoutez une nouvelle classe au projet et appelez-le **ImageInfo.cs**. Modifiez le fichier et qu’elle ressemble à ce qui suit :

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

Dans les sections suivantes, nous allons prendre une présentation détaillée de cette classe.

#### <a name="inheritance-and-interfaces"></a>L’héritage et interfaces

Avant d’une classe de données personnalisée peut être écrite ou lu à partir d’une table de montage, il doit être conforme à la `INSPastebaordWriting` et `INSPasteboardReading` interfaces. En outre, il doit hériter de `NSObject` et être conforme à la `INSCoding` interface :

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

La classe doit également être exposée pour Objective-C à l’aide de la `Register` directive et elle doivent exposer les propriétés requises ou les méthodes à l’aide de `Export`. Par exemple :

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Nous sommes exposé par les deux champs de données qui contient cette classe - nom de l’image et son type (jpg, png, etc.). 

Pour plus d’informations, consultez la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) documentation, elle explique la `Register` et `Export` attributs utilisé pour associer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

#### <a name="constructors"></a>Constructeurs

Deux constructeurs (correctement exposées pour Objective-C) sera nécessaire pour notre classe de données personnalisée afin qu’il peut être lu à partir d’une table de montage :

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

Tout d’abord, nous présentons les _vide_ constructeur sous la méthode Objective-C par défaut `init`.

Ensuite, nous présentons un `NSCoding` constructeur conforme permettant de créer une nouvelle instance de l’objet à partir de la table de montage lors du collage sous le nom exporté de `initWithCoder`.

Ce constructeur prend un `NSCoder` (tels que créés par un `NSKeyedArchiver` dès l’écriture de la table de montage), extrait les données de clé/valeur associé et l’enregistre dans les champs de propriété de la classe de données.

#### <a name="writing-to-the-pasteboard"></a>Écriture dans la table de montage

En répondant à la `INSPasteboardWriting` interface, nous devons exposer deux méthodes et éventuellement une troisième méthode, afin que la classe peut être écrite dans la table de montage.

Tout d’abord, nous devons indiquer à la table de montage le type de données représentations sous forme de la classe personnalisée peut être écrite dans :

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Chaque représentation est identifiée via un uniforme Type identificateur (lecture), qui n’est rien de plus qu’une simple chaîne qui identifie de façon unique le type de données présentées (pour plus d’informations, consultez le site d’Apple [vue d’ensemble des identificateurs de Type uniforme ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) documentation).

Pour notre format personnalisé, nous créons notre propre utilitaire : « com.xamarin.image-info » (Notez que la notation d’inverse comme un identificateur de l’application). Notre classe est également capable d’écrire une chaîne standard pour la table de montage (`public.text`). 

Ensuite, nous avons besoin créer l’objet dans le format demandé réellement écrite sur la table de montage :

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

Pour le `public.text` type, nous allons retournant une simple, au format `NSString` objet. Personnalisée du `com.xamarin.image-info` type, nous utilisons un `NSKeyedArchiver` et `NSCoder` interface pour encoder la classe de données personnalisées à un archivage de clé/valeur associé. Nous devrons implémenter la méthode suivante pour gérer l’encodage :

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Les paires clé/valeur individuelles sont écrits à l’encodeur et décoder à l’aide de la deuxième constructeur que nous avons ajouté au-dessus.

Si vous le souhaitez, nous pouvons incluent la méthode suivante pour définir des options lors de l’écriture des données à la table de montage :

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Actuellement, seuls les `WritingPromised` option est disponible et doit être utilisée lorsqu’un type donné est prévue uniquement et pas réellement écrit dans la table de montage. Pour plus d’informations, consultez la [prévue des données](#Promised_Data) section ci-dessus.

Avec ces méthodes en place, le code suivant peut être utilisé pour écrire de notre classe personnalisée dans la table de montage :

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>La lecture à partir de la table de montage

En répondant à la `INSPasteboardReading` interface, nous avons besoin d’exposer les trois méthodes pour que la classe de données personnalisées peut être lus à partir de la table de montage.

Tout d’abord, nous devons savoir quelles sont les données de la table de montage type représentations sous forme de la classe personnalisée capable de lire à partir du Presse-papiers :

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Là encore, ceux-ci sont définis UTIs simples et sont les mêmes types que nous avons défini dans le **l’écriture dans la table de montage** section ci-dessus.

Ensuite, nous avons besoin indiquer à la table de montage _comment_ chacun des types de lecture est lues à l’aide de la méthode suivante :

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

Pour le `com.xamarin.image-info` type, nous indiquons la table de montage pour décoder la paire clé/valeur que vous avez créée avec le `NSKeyedArchiver` lorsque l’écriture de la classe pour la table de montage en appelant le `initWithCoder:` constructeur que nous avons ajouté à la classe.

Enfin, nous devons ajouter la méthode suivante pour lire d’autres représentations de données de lecture à partir de la table de montage :

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

Avec toutes ces méthodes en place, la classe de données personnalisées peut être lues depuis la table de montage à l’aide du code suivant :

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

Il peut arriver lorsque vous avez besoin d’écrire des éléments personnalisés de la table de montage qui ne garantissent pas la création d’une classe personnalisée ou que vous souhaitez fournir des données dans un format commun, comme requis. Pour ces situations, vous pouvez utiliser un `NSPasteboardItem`.

A `NSPasteboardItem` fournit un contrôle affiné sur les données sont écrites dans la table de montage et sont conçues pour un accès temporaire - il doit être supprimé après qu’il a été écrit dans la table de montage.

#### <a name="writing-data"></a>L’écriture de données

Pour écrire vos données personnalisées à un `NSPasteboardItem` vous devez fournir un personnalisé `NSPasteboardItemDataProvider`. Ajoutez une nouvelle classe au projet et appelez-le **ImageInfoDataProvider.cs**. Modifiez le fichier et qu’elle ressemble à ce qui suit :

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

Comme nous l’avons fait avec la classe de données personnalisée, nous devons utiliser la `Register` et `Export` directives pour l’exposer aux objectif-C. La classe doit hériter de `NSPasteboardItemDataProvider` et vous devez implémenter la `FinishedWithDataProvider` et `ProvideDataForType` méthodes.

Utilisez le `ProvideDataForType` méthode pour fournir les données à encapsuler dans le `NSPasteboardItem` comme suit :

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

Dans ce cas, nous sommes le stockage de deux types d’informations sur notre image (nom et ImageType) et l’écriture de celles utilisées pour une chaîne simple (`public.text`).

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

#### <a name="reading-data"></a>La lecture des données

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

Cet article a pris une présentation détaillée sur l’utilisation de la table de montage dans une application Xamarin.Mac pour prendre en charge la copie et les opérations de collage. Tout d’abord, il a introduit un exemple simple pour vous familiariser avec les opérations de tables de montage standard. Ensuite, il a fallu examine en détail la table de montage et comment lire et écrire des données à partir de celui-ci. Enfin, il se présentait à l’aide d’un type de données personnalisé pour prendre en charge la copie et le collage des types de données complexes au sein d’une application.



## <a name="related-links"></a>Liens associés

- [MacCopyPaste (sample)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Table de montage Guide de programmation](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
