---
title: Menus
description: "Cet article décrit l’utilisation de menus dans une application Xamarin.Mac. Il décrit la création et gestion des menus et des éléments de menu dans Xcode et Interface générateur et leur utilisation par programmation."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 52a9fc206a2c303d13d80be4de743d98056f7684
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="menus"></a>Menus

_Cet article décrit l’utilisation de menus dans une application Xamarin.Mac. Il décrit la création et gestion des menus et des éléments de menu dans Xcode et Interface générateur et leur utilisation par programmation._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès aux menus/Cocoa mêmes qu’un développeur travaillant dans Objective-C et Xcode effectue. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser le Générateur de Xcode Interface pour créer et gérer des barres de menus, des menus et des éléments de menu (ou éventuellement de les créer directement dans le code C#).

Menus font partie intégrante de l’expérience utilisateur de l’application d’un Mac et apparaissent généralement dans les différentes parties de l’interface utilisateur :

- **Barre de menus de l’application** -il s’agit du menu principal s’affiche en haut de l’écran pour toutes les applications Mac.
- **Menus contextuels** -apparaissent lorsque l’utilisateur clique ou contrôle-clique sur un élément dans une fenêtre.
- **La barre d’état** -c’est la zone à l’extrême droite de la barre de menus d’application qui s’affiche en haut de l’écran (à gauche de l’horloge de barre de menu) et augmente à mesure à gauche que les éléments sont ajoutés à ce dernier.
- **Ancrer menu** -menu pour chaque application de la station d’accueil qui apparaît lorsque l’utilisateur clique ou contrôle-clique sur l’icône de l’application, ou lorsque l’utilisateur left-clicks l’icône et appuie sur le bouton de la souris.
- **Bouton contextuelle et les listes déroulantes** -un bouton déroulant affiche un élément sélectionné et présente la liste des options à sélectionner à partir de clic de l’utilisateur. Une liste déroulante est un type de bouton déroulant généralement utilisé pour la sélection des commandes spécifiques au contexte de la tâche en cours. Les deux peuvent apparaître n’importe où dans une fenêtre.

[![Un exemple de menu](menu-images/intro01.png "un exemple de menu")](menu-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation des éléments de menu dans une application Xamarin.Mac, les menus et barres de menus/Cocoa. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` attributs permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

## <a name="the-applications-menu-bar"></a>Barre de menus de l’application 

Contrairement aux applications en cours d’exécution sur le système d’exploitation Windows où chaque fenêtre peut avoir sa propre barre de menus attaché, chaque application s’exécutant sur macOS a une barre de menus unique qui s’exécute sur le haut de l’écran qui est utilisé pour chaque fenêtre de l’application :

[![Une barre de menus](menu-images/appmenu01.png "une barre de menus")](menu-images/appmenu01-large.png#lightbox)

Éléments dans cette barre de menus sont activées ou désactivées selon le contexte actuel ou l’état de l’application et son interface utilisateur à un moment donné. Par exemple : si l’utilisateur sélectionne un champ de texte, des éléments sur le **modifier** menu seront activé comme **copie** et **couper**.

En fonction d’Apple et, par défaut, toutes les applications de macOS ont un jeu standard des menus et des éléments de menu qui s’affichent dans la barre de menus de l’application :

- **Menu Pomme** -ce menu donne l’accès au système larges éléments qui sont disponibles pour l’utilisateur à tout moment, quel que soit l’application est en cours d’exécution. Ces éléments ne peuvent pas être modifiés par le développeur.
- **Menu application** -ce menu affiche le nom de l’application en gras et aide l’utilisateur à identifier l’application est en cours d’exécution. Il contient des éléments qui s’appliquent à l’application dans son ensemble et non en tant que document donné ou processus tels que la fermeture de l’application.
- **Menu fichier** - éléments utilisés pour créer, ouvrir ou enregistrer les documents que votre application fonctionne avec. Si votre application n’est pas basé sur un document, ce menu peut être renommé ou supprimé.
- **Menu Edition** -contient des commandes comme **couper**, **copie**, et **coller** qui permettent de modifier ou de modifier les éléments d’interface utilisateur de l’application.
- **Menu format** - si l’application fonctionne avec du texte, ce menu contient les commandes pour ajuster la mise en forme de texte.
- **Menu Affichage** -contient des commandes qui affectent l’affichage du contenu (affiché) dans l’interface utilisateur de l’application.
- **Les menus spécifiques à l’application** -il s’agit des menus qui sont spécifiques à votre application (par exemple, un menu de signets pour un navigateur web). Ils doivent apparaître entre le **vue** et **fenêtre** la barre de menus.
- **Menu fenêtre** -contient des commandes pour l’utilisation des fenêtres dans votre application, ainsi que la liste des fenêtres actives en cours.
- **Menu Aide** -si votre application fournit une aide à l’écran, le menu d’aide doit être le menu le plus à droite dans la barre. 

Pour plus d’informations sur la barre de menus d’application et les menus standard et les éléments de menu, consultez le site d’Apple [indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>La barre de menus d’application par défaut

Chaque fois que vous créez un nouveau projet Xamarin.Mac, vous obtenez automatiquement une barre de menus d’application par défaut standard, qui comporte les éléments de types qui a généralement une application macOS (comme nous l’avons vu dans la section ci-dessus). Barre de menus de votre application par défaut est défini dans le **Main.storyboard** fichier (ainsi que le reste de l’interface utilisateur de votre application) sous le projet dans le **Solution remplissage**:  

![Sélectionnez la table de montage séquentiel principal](menu-images/appmenu02.png "sélectionner le storyboard principal")

Double-cliquez sur le **Main.storyboard** fichier pour l’ouvrir pour le modifier dans le Générateur de Xcode Interface et vous verrez l’interface de l’éditeur de menu :

[![Modification de l’interface utilisateur dans Xcode](menu-images/defaultbar01.png "modification de l’interface utilisateur dans Xcode")](menu-images/defaultbar01-large.png#lightbox)

À partir d’ici vous pouvez cliquer sur des éléments tels que les **ouvrir** élément de menu dans le **fichier** menu et de modifier ou ajuster ses propriétés dans le **inspecteur d’attributs**:

[![Modification des attributs d’un menu](menu-images/defaultbar02.png "la modification des attributs d’un menu")](menu-images/defaultbar02-large.png#lightbox)

Nous aborderons Ajout, modification et suppression des menus et des éléments plus loin dans cet article. Maintenant, nous voulons que juste voir les menus et les éléments de menu sont disponibles par défaut et la façon dont ils ont été exposés automatiquement au code via un ensemble de sorties prédéfinies et les actions (pour plus d’informations, consultez notre [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) documentation).

Par exemple, si nous cliquons sur le **connexion inspecteur** pour le **ouvrir** élément de menu, nous pouvons voir automatiquement de connexion à la `openDocument:` action : 

[![Affichage de l’action attachée](menu-images/defaultbar03.png "affichage de l’action attachée")](menu-images/defaultbar03-large.png#lightbox)

Si vous sélectionnez le **répondeur première** dans le **hiérarchie de l’Interface** et faites défiler la **inspecteur de connexion**, et vous verrez la définition de la `openDocument:` action qui le **ouvrir** élément de menu est attaché à (ainsi que plusieurs autres actions par défaut pour l’application et ne sont pas automatiquement liées au contrôle) :

[![Affichage des actions reliées](menu-images/defaultbar04.png "affichage de toutes les actions attachées")](menu-images/defaultbar04-large.png#lightbox) 

Pourquoi est-ce important ? Dans la prochaine section verrez le fonctionnement de ces actions définies automatiquement avec d’autres éléments d’interface utilisateur/Cocoa automatiquement activer et désactiver les éléments de menu, ainsi que, fournir des fonctionnalités intégrées pour les éléments.

Plus tard nous utiliserons ces actions intégrées pour activer et désactiver les éléments à partir du code et fournir notre propre fonctionnalité lorsqu’ils sont sélectionnés.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Fonctionnalités de menu intégré

Si vous étiez à l’exécution, une application Xamarin.Mac nouvellement créé avant d’ajouter les éléments d’interface utilisateur ou le code, vous remarquerez que certains éléments sont automatiquement accès câblé et activées (avec complet des fonctionnalités intégrées automatiquement), telles que le **Quit** d’élément dans le **application** menu :

![Un élément de menu activé](menu-images/appmenu03.png "un élément de menu activé")

Même si d’autres éléments de menu, tels que **couper**, **copie**, et **coller** ne sont pas :

![Les éléments de menu désactivés](menu-images/appmenu04.png "désactivé des éléments de menu")

Nous allons arrêter l’application et double-cliquez sur le **Main.storyboard** de fichiers dans le **Solution remplissage** pour ouvrir pour le modifier dans Xcode du constructeur d’Interface. Ensuite, faites glisser un **affichage texte** à partir de la **bibliothèque** sur le contrôleur de vue de la fenêtre de la **éditeur de l’Interface**:

[![Sélectionnez une vue de texte dans la bibliothèque](menu-images/appmenu05.png "en sélectionnant une vue de texte à partir de la bibliothèque")](menu-images/appmenu05-large.png#lightbox)

Dans le **éditeur de contrainte** nous allons épingler l’affichage de texte sur les bords de la fenêtre et la définir où il se développe et réduit la fenêtre en cliquant sur toutes les quatre rouge I-FAISCEAUX en haut de l’éditeur sur le **ajouter des contraintes 4** bouton :

[![Modifier les contraintes](menu-images/appmenu06.png "modifier les contraintes")](menu-images/appmenu06-large.png#lightbox)

Enregistrez vos modifications dans la conception de l’interface utilisateur et revenir à Visual Studio pour Mac synchroniser les modifications apportées à votre projet Xamarin.Mac. Démarrer l’application, tapez du texte dans l’affichage de texte, sélectionnez-le, puis ouvrez maintenant le **modifier** menu :

![Les éléments de menu sont automatiquement activées/désactivées](menu-images/appmenu07.png "les éléments de menu sont automatiquement activées/désactivées")

Notez comment la **couper**, **copie**, et **coller** éléments sont automatiquement activées et entièrement fonctionnelles, tout cela sans écrire une seule ligne de code. 

Qu'est-ce qui se passe ? N’oubliez pas de la fonction intégrée prédéfinir les actions qui viennent câblées jusqu'à les éléments de menu par défaut (comme indiqué ci-dessus), la plupart des éléments d’interface utilisateur/Cocoa qui font partie de macOS ont créé dans raccordements à des actions spécifiques (tels que `copy:`). Par conséquent, lorsqu’ils sont ajoutés à une fenêtre, active, sélectionnés, l’élément de menu correspondant ou les éléments associés à cette action sont automatiquement activés. Si l’utilisateur sélectionne cet élément de menu, les fonctionnalités intégrées de l’élément d’interface sont appelée et exécutée, tout cela sans intervention des développeurs.

### <a name="enabling-and-disabling-menus-and-items"></a>Activation et désactivation des menus et des éléments

Par défaut, chaque fois qu’un événement se produit, `NSMenu` automatiquement Active et désactive chaque menu visible et élément en fonction du contexte de l’application. Il existe trois façons pour activer ou désactiver un élément :

- **L’activation de menus automatique** -un élément de menu est activé si `NSMenu` rechercher un objet approprié qui répond à l’action que l’élément est câblé-jusqu'à. Par exemple, l’affichage de texte ci-dessus qui avait un raccordement intégré à la `copy:` action.
- **Actions personnalisées et validateMenuItem :** - pour n’importe quel élément de menu qui est lié à un [action personnalisée de fenêtre ou une vue contrôleur](#Working-with-Custom-Window-Actions), vous pouvez ajouter la `validateMenuItem:` action et activer ou désactiver manuellement des éléments de menu.
- **L’activation de menu manuelle** -vous définissez manuellement la `Enabled` propriété de chaque `NSMenuItem` pour activer ou désactiver individuellement de chaque élément dans un menu.

Pour choisir un système, affectez la `AutoEnablesItems` propriété d’un `NSMenu`. `true` est automatique (comportement par défaut) et `false` est manuel. 

> [!IMPORTANT]
> Si vous choisissez d’utiliser l’activation de menu manuelle, aucun du menu d’éléments, même ceux contrôlé par les classes AppKit comme `NSTextView`, sont automatiquement mis à jour. Vous serez responsable de l’activation et désactivation de tous les éléments manuellement dans le code.

#### <a name="using-validatemenuitem"></a>À l’aide de validateMenuItem

Comme indiqué précédemment, pour n’importe quel élément de menu qui est lié à un [fenêtre ou une Action personnalisée vue contrôleur](#Working-with-Custom-Window-Actions), vous pouvez ajouter la `validateMenuItem:` action et activer ou désactiver manuellement des éléments de menu.

Dans l’exemple suivant, la `Tag` propriété sera utilisée pour déterminer le type d’élément de menu qui sera être activé ou désactivé par le `validateMenuItem:` action basée sur l’état du texte sélectionné dans un `NSTextView`. Le `Tag` propriété a été définie dans le constructeur d’Interface pour chaque élément de menu :

![Définition de la propriété de la balise](menu-images/validate01.png "définition de la propriété de balise")

Et le code suivant est ajouté au contrôleur de vue :

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

Lorsque ce code est exécuté, et aucun texte n’est sélectionné dans le `NSTextView`, les éléments de menu de deux de type wrap sont désactivées (même s’ils sont rattachés à des actions sur le contrôleur d’affichage) :

![Désactivé l’affichage d’éléments](menu-images/validate02.png "montrant les éléments désactivés")

Si une section de texte est sélectionnée et de rouvrir le menu, les éléments de menu de type deux wrap seront disponibles :

![Activé l’affichage d’éléments](menu-images/validate03.png "montrant les articles activée")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>L’activation et répondre aux éléments de menu dans le code

Comme nous l’avons vu ci-dessus, simplement en ajoutant des éléments d’interface utilisateur/Cocoa spécifiques à notre conception de l’interface utilisateur (par exemple, un champ de texte), plusieurs des éléments de menu par défaut seront activés et fonctionnent automatiquement, sans avoir à écrire du code. Suivant Examinons sur l’ajout de notre propre code c# à notre projet Xamarin.Mac pour activer un élément de menu et de fournir une fonctionnalité lorsque l’utilisateur le sélectionne.

Par exemple, laisser prononcer nous souhaitons l’utilisateur pour être en mesure d’utiliser le **ouvrir** d’élément dans le **fichier** menu pour sélectionner un dossier. Étant donné que nous voulons qu’il s’agit d’une fonction de l’application et ne pas seulement un permettent de fenêtre ou un élément d’interface utilisateur, nous allons ajouter le code pour gérer cette situation à notre délégué de l’application.

Dans le **Solution remplissage**, double-cliquez sur le `AppDelegate.CS` fichier à ouvrir pour le modifier :

![En sélectionnant le délégué de l’application](menu-images/appmenu08.png "en sélectionnant le délégué de l’application")

Ajoutez le code suivant ci-dessous le `DidFinishLaunching` méthode :

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Nous allons exécuter l’application maintenant, puis ouvrez le **fichier** menu : 

![Le menu fichier](menu-images/appmenu09.png "le menu fichier")

Notez que la **ouvrir** élément de menu est maintenant activé. Si vous le sélectionnez, la boîte de dialogue Ouvrir s’affichera :

![Une boîte de dialogue Ouvrir](menu-images/appmenu10.png "une boîte de dialogue Ouvrir")

Si vous cliquez sur le **ouvrir** bouton, notre message d’alerte s’affichera :

![Un exemple de message de boîte de dialogue](menu-images/appmenu11.png "un exemple de message de boîte de dialogue")

La ligne de clé a été `[Export ("openDocument:")]`, elle indique `NSMenu` qui notre **AppDelegate** a une méthode `void OpenDialog (NSObject sender)` qui répond à la `openDocument:` action. Si vous devez vous rappeler ci-dessus, le **ouvrir** élément de menu est automatiquement câblé accès à cette action par défaut dans le Générateur de Interface :

[![Afficher les actions attachées](menu-images/defaultbar03.png "afficher les actions attachées")](menu-images/defaultbar03-large.png#lightbox)

Suivant Examinons créer notre propre menu, les éléments de menu et les actions et répondre les uns aux autres dans le code.

### <a name="working-with-the-open-recent-menu"></a>Utilisation de l’ouverture du menu récents

Par défaut, le **fichier** menu contient un **ouvrir récente** élément qui effectue le suivi des dernière plusieurs fichiers que l’utilisateur a ouvert avec votre application. Si vous créez un `NSDocument` Xamarin.Mac application, en ce menu est traitée pour vous automatiquement. Pour tout autre type d’application de Xamarin.Mac, vous serez chargé de gérer et de répondre manuellement à cet élément de menu.

Pour traiter manuellement le **ouvrir récente** menu, vous devez d’abord informe qu’un nouveau fichier a été ouvert ou enregistré à l’aide de ce qui suit :

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Même si votre application n’utilise pas `NSDocuments`, toujours utiliser le `NSDocumentController` pour maintenir le **ouvrir récente** menu en envoyant un `NSUrl` avec l’emplacement du fichier à la `NoteNewRecentDocumentURL` méthode de la `SharedDocumentController`.

Ensuite, vous devez remplacer le `OpenFile` méthode du délégué d’application pour ouvrir n’importe quel fichier que l’utilisateur sélectionne à partir de la **ouvrir récente** menu. Exemple :

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

Retourner `true` si le fichier peut être ouvert, sinon retourne `false` et un avertissement intégré s’affichera à l’utilisateur que le fichier ne peut pas être ouvert.

Car le nom de fichier et le chemin d’accès retourné à partir du **ouvrir récente** menu, peut inclure un espace, nous devons échappe correctement ce caractère avant de créer un `NSUrl` ou nous obtenons une erreur. Cela se fait par le code suivant :

```csharp
filename = filename.Replace (" ", "%20");
```

Enfin, nous créons un `NSUrl` qui pointe vers le fichier, utiliser une méthode d’assistance dans l’application déléguer à ouvrir une nouvelle fenêtre et y charger le fichier :

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Pour rassembler tous les éléments, examinons un exemple d’implémentation dans une **AppDelegate.cs** fichier :

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
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

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = true;
            dlg.CanChooseDirectories = false;

            if (dlg.RunModal () == 1) {
                // Nab the first file
                var url = dlg.Urls [0];

                if (url != null) {
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

Selon les besoins de votre application, vous pouvez ouvrir le même fichier dans plusieurs fenêtres en même temps. Dans notre exemple d’application, si l’utilisateur choisit un fichier qui est déjà ouvert (à partir de la **ouvrir récente** ou **ouvrir...** éléments de menu), la fenêtre qui contient le fichier est mises à l’avant.

Pour ce faire, nous avons utilisé le code suivant dans notre méthode d’assistance :

```csharp
var path = url.Path;

// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Nous avons conçu notre `ViewController` classe pour contenir le chemin d’accès au fichier dans son `Path` propriété. Ensuite, nous parcourir toutes les fenêtres actuellement ouvertes dans l’application. Si le fichier est déjà ouvert dans une fenêtre du, elle est placée au début de toutes les autres fenêtres à l’aide de :

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Si aucune correspondance n’est trouvée, une nouvelle fenêtre s’ouvre avec le fichier chargé et le fichier est indiqué dans le **ouvrir récente** menu :

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>L’utilisation d’actions de fenêtres personnalisées

Tout comme la fonction intégrée **premier répondeur** les actions qui sont d’aux éléments de menu standard, vous pouvez créer de nouvelles actions personnalisées et les associer aux éléments de menu dans le Générateur de l’Interface.

Tout d’abord, définissez une action personnalisée sur l’un des contrôleurs de fenêtre de votre application. Exemple :

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Ensuite, double-cliquez sur le fichier de plan conceptuel de l’application dans le **Solution remplissage** pour ouvrir pour le modifier dans Xcode du constructeur d’Interface. Sélectionnez le **répondeur première** sous le **Application scène**, puis basculer vers le **inspecteur d’attributs**:

![L’inspecteur attributs](menu-images/action01.png "l’inspecteur d’attributs")

Cliquez sur le  **+**  situé en bas de la **attributs inspecteur** pour ajouter une nouvelle action personnalisée :

![Ajout d’une nouvelle action](menu-images/action02.png "Ajout d’une nouvelle action")

Lui donner le même nom que l’action personnalisée que vous avez créé sur votre contrôleur de fenêtre :

![Modification du nom d’action](menu-images/action03.png "modification du nom d’action")

La touche CTRL enfoncée et faire glisser à partir d’un élément de menu pour le **premier répondeur** sous le **Application scène**. Dans la liste de la fenêtre contextuelle, sélectionnez la nouvelle action que vous venez de créer (`defineKeyword:` dans cet exemple) :

![Attachement d’une action](menu-images/action04.png "attachement d’une action")

Enregistrez les modifications dans la table de montage séquentiel et revenir à Visual Studio pour Mac synchroniser les modifications. Si vous exécutez l’application, l’élément de menu que vous avez connecté l’action personnalisée sera automatiquement être activé ou désactivé (basé sur la fenêtre avec l’action ne soit ouverte) et en sélectionnant l’élément de menu déclenche l’action :

[![Pour tester l’action nouvelle](menu-images/action05.png "pour tester la nouvelle action")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Ajout, modification et suppression de menus

Comme nous l’avons vu dans les sections précédentes, une application de Xamarin.Mac est fourni avec un nombre prédéfini de menus par défaut et les éléments de menu des contrôles d’interface utilisateur seront automatiquement activer et y répondre. Nous avons également vu comment ajouter du code à notre application qui sera également activer et répondre à ces éléments par défaut.

Dans cette section, nous allons examiner des éléments de menu qui nous n’avez pas besoin, la réorganisation des menus et l’ajout de nouveaux menus, des éléments de menu et des actions.

Double-cliquez sur le **Main.storyboard** de fichiers dans le **Solution remplissage** à ouvrir pour le modifier :

[![Modification de l’interface utilisateur dans Xcode](menu-images/maint01.png "modification de l’interface utilisateur dans Xcode")](menu-images/maint01-large.png#lightbox)

Pour notre application Xamarin.Mac spécifique nous n’allons pas à la valeur par défaut **vue** menu, nous allons supprimer. Dans le **hiérarchie de l’Interface** sélectionner le **vue** élément de menu qui fait partie de la barre de menus principale :

![En sélectionnant l’élément de menu Afficher](menu-images/maint02.png "en sélectionnant l’élément de menu Affichage")

Appuyez sur SUPPR ou retour arrière pour supprimer le menu. Ensuite, nous n’allons pas être à l’aide de tous les éléments dans le **Format** menu et souhaitez déplacer les éléments que nous allons utiliser figurant dans les sous-menus. Dans le **hiérarchie de l’Interface** sélectionner les éléments suivants :

![Plusieurs éléments de mise en surbrillance](menu-images/maint03.png "plusieurs éléments de mise en surbrillance")

Faites glisser les éléments sous le parent **Menu** dans le sous-menu là où ils sont actuellement :

[![En faisant glisser des éléments de menu au menu parent](menu-images/maint04.png "en faisant glisser des éléments de menu au menu parent")](menu-images/maint04-large.png#lightbox)

Votre menu doit maintenant ressembler à :

[![Les éléments dans le nouvel emplacement](menu-images/maint05.png "les éléments dans le nouvel emplacement")](menu-images/maint05-large.png#lightbox)

Suivant faisons glisser le **texte** sous-menu out sous le **Format** menu et placez-la sur la barre de menus principale entre la **Format** et **fenêtre** menus :

[![Le menu de texte](menu-images/maint06.png "menu le texte")](menu-images/maint06-large.png#lightbox)

Passons sous le **Format** menu et supprimer la **police** élément de sous-menu. Ensuite, sélectionnez le **Format** menu et renommez-la « Police » :

[![Le menu police](menu-images/maint07.png "menu la police")](menu-images/maint07-large.png#lightbox)

Ensuite, nous allons créer un menu personnalisé d’expressions prédéfini qui sera obtenir automatiquement ajouté à du texte dans l’affichage de texte lorsqu’ils sont sélectionnés. Dans la zone de recherche en bas sur la **bibliothèque inspecteur** type dans « menu ». Cela rend plus faciles à trouver et fonctionne avec tous les éléments d’interface utilisateur de menu :

![L’inspecteur de bibliothèque](menu-images/maint08.png "l’inspecteur de bibliothèque")

Maintenant nous allons suivre la commande suivante pour créer notre menu :

1. Faites glisser un **élément de Menu** à partir de la **bibliothèque inspecteur** sur la barre de menus entre le **texte** et **fenêtre** menus : 

    ![Sélection d’un élément de menu dans la bibliothèque](menu-images/maint10.png "en sélectionnant un élément de menu dans la bibliothèque")
2. Renommer l’élément « Expressions » : 

    [![Définition du nom de menu](menu-images/maint09.png "la définition du nom de menu")](menu-images/maint09-large.png#lightbox)
3. Ensuite faire glisser un **Menu** à partir de la **bibliothèque inspecteur**: 

    ![Sélection d’un menu de la bibliothèque](menu-images/maint11.png "en sélectionnant un menu de la bibliothèque")
4. Liste déroulante, puis **Menu** sur le nouveau **élément de Menu** nous venez de créer et modifier son nom « Expressions » : 

    [![Modification du nom de menu](menu-images/maint12.png "modification du nom de menu")](menu-images/maint12-large.png#lightbox)
5. Maintenant nous allons renommer la valeur par défaut trois **des éléments de Menu** « Address », « Date » et « Greeting » : 

    [![Le menu de Phrases](menu-images/maint13.png "menu de Phrases le")](menu-images/maint13-large.png#lightbox)
6. Ajoutons un quatrième **élément de Menu** en faisant glisser un **élément de Menu** à partir de la **bibliothèque inspecteur** et l’appeler « Signature » : 

    [![Modification du nom d’élément de menu](menu-images/maint14.png "modification du nom d’élément de menu")](menu-images/maint14-large.png#lightbox)
7. Enregistrez les modifications dans la barre de menus.

Maintenant nous allons créer un jeu d’actions personnalisées, afin que nos nouveaux éléments de menu sont exposés au code c#. Dans Xcode passons à la **Assistant** vue :

[![Créer les actions requises](menu-images/maint15.png "création les actions requises")](menu-images/maint15-large.png#lightbox)

Nous allons effectuer les opérations suivantes :

1. Faites glisser le contrôle à partir de la **adresse** élément de menu pour le **AppDelegate.h** fichier.
2. Commutateur le **connexion** type **Action**: 

    [![En sélectionnant le type d’action](menu-images/maint17.png "en sélectionnant le type d’action")](menu-images/maint17-large.png#lightbox)
3. Entrez un **nom** de « phraseAddress » et appuyez sur la **Connect** bouton permettant de créer la nouvelle action : 

    [![Configuration de l’action](menu-images/maint18.png "configuration de l’action")](menu-images/maint18-large.png#lightbox)
4. Répétez les étapes ci-dessus pour le **Date**, **salutation**, et **Signature** des éléments de menu : 

    [![Les actions terminées](menu-images/maint19.png "les actions terminées")](menu-images/maint19-large.png#lightbox)
5. Enregistrez les modifications dans la barre de menus.

Ensuite, nous avons besoin créer une sortie pour notre vue de texte afin que nous pouvons ajuster son contenu à partir du code. Sélectionnez le **ViewController.h** de fichiers dans le **Assistant éditeur** et créer un nouveau magasin appelé `documentText`:

[![Création d’une prise de courant](menu-images/maint20.png "création d’une prise de courant")](menu-images/maint20-large.png#lightbox)

Revenez à Visual Studio pour Mac synchroniser les modifications de Xcode. Modifiez ensuite la **ViewController.cs** de fichier et le rendre l’aspect suivant :

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

Cela expose le texte de notre affichage de texte en dehors de la `ViewController` classe et informe le délégué de l’application lorsque la fenêtre les gains ou perd le focus. À présent modifier le **AppDelegate.cs** de fichier et le rendre l’aspect suivant :

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
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

        #region Custom actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = false;
            dlg.CanChooseDirectories = true;

            if (dlg.RunModal () == 1) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
                    MessageText = "Folder Selected"
                };
                alert.RunModal ();
            }
        }

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

Ici, nous avons la `AppDelegate` un partiel afin que nous pouvons utiliser les actions et les sorties que nous avons défini dans le Générateur de l’Interface de classe. Nous présentons également un `textEditor` pour effectuer le suivi de la fenêtre qui est actuellement active.

Les méthodes suivantes sont utilisées pour gérer nos menu personnalisé et les éléments de menu :

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

Maintenant, si nous exécutons notre application, tous les éléments dans le **expression** menu sera actif et ajoute l’expression de donner à l’affichage du texte sélectionné :

![Un exemple de l’application en cours d’exécution](menu-images/maint21.png "un exemple de l’application en cours d’exécution")

Maintenant que nous avons les principes fondamentaux de l’utilisation de la barre de menus d’application vers le bas, examinons la création d’un menu contextuel personnalisé.

### <a name="creating-menus-from-code"></a>Création de menus à partir du code

Outre la création de menus et des éléments de menu avec le Générateur de Xcode Interface, il peut arriver lorsqu’une application Xamarin.Mac doit créer, modifier ou supprimer un menu, un sous-menu ou un élément de menu à partir du code.

Dans l’exemple suivant, une classe est créée pour contenir les informations sur les éléments de menu et les sous-menus qui seront créés dynamiquement à la volée :

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>Ajout de menus et des éléments

Avec cette classe est définie, la routine suivante analyse une collection de `LanguageFormatCommand`objets et manière récursive générer nouveaux menus et éléments de menu en les ajoutant au bas du menu existant (créé dans le Générateur de Interface) qui a été passé dans :

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

Pour toute `LanguageFormatCommand` objet qui a une valeur vide `Title` propriétés, cette routine génère un **élément de menu séparateur** (une fine ligne grise) entre des sections du menu :

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Si un titre est fourni, un nouvel élément de menu avec ce titre est créé :

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Si le `LanguageFormatCommand` objet contient des enfants `LanguageFormatCommand` objets, un sous-menu est créé et le `AssembleMenu` méthode est appelée pour générer ce menu de manière récursive :

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Pour tout nouvel élément de menu qui ne dispose pas des sous-menus, le code est ajouté à l’élément de menu sélectionné par l’utilisateur de gérer :

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Test de la création de menu

Avec tout le code ci-dessus en place, si la collection suivante de `LanguageFormatCommand` objets ont été créés :

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

Et que la collection passée à la `AssembleMenu` (fonction) (avec la **Format** Menu définie en tant que la base), les menus dynamiques suivants et les éléments de menu sont créés :

![Les nouveaux éléments de menu dans l’application en cours d’exécution](menu-images/dynamic01.png "les nouveaux éléments de menu dans l’application en cours d’exécution")

#### <a name="removing-menus-and-items"></a>Suppression des menus et des éléments

Si vous avez besoin supprimer un menu ou un élément de menu à partir de l’interface utilisateur de l’application, vous pouvez utiliser la `RemoveItemAt` méthode de la `NSMenu` classe simplement en lui donnant le zéro index de base de l’élément à supprimer.

Par exemple, pour supprimer les menus et les éléments de menu créés par la routine ci-dessus, vous pouvez utiliser le code suivant :

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

Dans le cas le code ci-dessus, les premiers éléments de quatre menu sont créés dans du Xcode constructeur d’Interface et rangements disponibles dans l’application, afin qu’ils ne sont pas supprimés de manière dynamique.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Menus contextuels

Menus contextuels s’affichent lorsque l’utilisateur clique ou contrôle-clique sur un élément dans une fenêtre. Par défaut, plusieurs éléments d’interface utilisateur intégrées macOS déjà possèdent des menus contextuels qui leur sont attachées (par exemple, l’affichage de texte). Toutefois, il peut arriver que vous souhaitiez créer notre propre contextuels personnalisés pour un élément d’interface utilisateur que nous avons ajouté à une fenêtre.

Permet de modifier notre **Main.storyboard** dans Xcode et ajoutez un **fenêtre** fenêtre à notre conception, définissez son **classe** à « NSPanel » dans le **inspecteur d’identité**, ajouter un nouveau **Assistant** d’élément à la **fenêtre** menu et l’associer à la nouvelle fenêtre en utilisant un **afficher Segue**:

[![La définition du type segue](menu-images/context01.png "la définition du type segue")](menu-images/context01-large.png#lightbox)

Nous allons effectuer les opérations suivantes :

1. Faites glisser un **étiquette** à partir de la **inspecteur de bibliothèque** sur le **panneau** fenêtre et définir son texte sur « Propriété » : 

    [![Modification de la valeur de l’étiquette](menu-images/context03.png "modification de la valeur de l’étiquette")](menu-images/context03-large.png#lightbox)
2. Ensuite faire glisser un **Menu** à partir de la **bibliothèque inspecteur** sur le contrôleur de vue des afficher la hiérarchie et les éléments de menu par défaut de renommer les trois **Document**, **texte**  et **police**:

    [![Les éléments de menu requis](menu-images/context02.png "les éléments de menu requis")](menu-images/context02-large.png#lightbox)
3. Maintenant contrôle, faites glisser le **étiquette de la propriété** sur la **Menu**:

    [![Déplacement pour créer un segue](menu-images/context04.png "déplacement pour créer un segue")](menu-images/context04-large.png#lightbox)
4. Dans la boîte de dialogue contextuelle, sélectionnez **Menu**: 

    ![La définition du type segue](menu-images/context05.png "la définition du type segue")
5. À partir de la **identité inspecteur**, définir la classe du contrôleur de la vue à « PanelViewController » : 

    [![Définition de la classe segue](menu-images/context10.png "définition de la classe segue")](menu-images/context10-large.png#lightbox)
6. Revenez à Visual Studio pour Mac à synchroniser, puis revenir à l’Interface générateur.
7. Basculez vers le **Assistant éditeur** et sélectionnez le **PanelViewController.h** fichier.
8. Créer une action pour le **Document** élément de menu appelé `propertyDocument`: 

    [![Configuration de l’action](menu-images/context06.png "configuration de l’action")](menu-images/context06-large.png#lightbox)
9. Répétez les actions de création pour les autres éléments de menu : 

    [![Les actions requises](menu-images/context07.png "les actions requises")](menu-images/context07-large.png#lightbox)
10. Pour finir, une prise pour créer le **étiquette de la propriété** appelée `propertyLabel`: 

    [![Configuration de la prise de](menu-images/context08.png "configuration de la sortie")](menu-images/context08-large.png#lightbox)
11. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Modifier la **PanelViewController.cs** et ajoutez le code suivant :

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

Maintenant, si nous exécuter l’application et cliquez sur l’étiquette de la propriété dans le panneau de configuration, nous verrons notre menu contextuel personnalisé. Si nous sélectionner et d’élément dans le menu, valeur de l’étiquette changent :

![Le menu contextuel en cours d’exécution](menu-images/context09.png "le menu contextuel en cours d’exécution")

Suivant Examinons création de menus de barre d’état.

## <a name="status-bar-menus"></a>Menus de barre d’état

Menus de barre d’état affichent une collection d’éléments de menu d’état qui fournissent une interaction avec ou des commentaires à l’utilisateur, par exemple un menu ou d’une image de refléter l’état de l’application. Menu de barre d’état d’une application est actif et même si l’application s’exécute en arrière-plan. La barre d’état du système à l’échelle se situe à droite de la barre de menus d’application et est la barre d’état uniquement actuellement disponibles dans macOS.

Permet de modifier notre **AppDelegate.cs** et effectuez le `DidFinishLaunching` détaillée de la méthode comme suit :

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` nous permet d’accéder à la barre d’état du système. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` Crée un nouvel élément de barre d’état. À partir de là, nous créer un menu et un nombre d’éléments de menu et attacher le menu à l’élément de barre d’état que nous venons de créer. 

Si nous exécutons l’application, le nouvel élément de barre d’état s’affichera. Sélection d’un élément dans le menu modifiera le texte dans l’affichage de texte : 

![Le menu de barre d’état en cours d’exécution](menu-images/statusbar01.png "le menu de barre d’état en cours d’exécution")

Ensuite, examinons création dock personnalisée des éléments de menu.

## <a name="custom-dock-menus"></a>Menus de la station d’accueil personnalisée

Le menu d’ancrage s’affiche pour vous application Mac lorsque l’utilisateur clique ou contrôle-clique sur l’icône de l’application de la station d’accueil :

![Personnalisé ancrer menu](menu-images/dock01.png "personnalisé ancrer menu")

Nous allons créer un menu d’ancrage personnalisé pour notre application en procédant comme suit :

1. Dans Visual Studio pour Mac, cliquez sur de l’application projet et sélectionnez **ajouter** > **nouveau fichier...** Dans la boîte de dialogue Nouveau fichier, sélectionnez **Xamarin.Mac** > **définition d’Interface vide**, utilisez « DockMenu » pour le **nom** et cliquez sur le **New**  pour créer le nouveau bouton **DockMenu.xib** fichier :

    ![Ajout d’une définition d’interface vide](menu-images/dock02.png "Ajout d’une définition d’interface vide")
2. Dans le **Solution remplissage**, double-cliquez sur le **DockMenu.xib** fichier à ouvrir pour le modifier dans Xcode. Créer un nouveau **Menu** avec les éléments suivants : **adresse**, **Date**, **salutation**, et **Signature** 

    [![Disposition de l’interface utilisateur](menu-images/dock03.png "disposition de l’interface utilisateur")](menu-images/dock03-large.png#lightbox)
3. Ensuite, nous allons connecter nos nouveaux éléments de menu à notre actions existantes que vous avez créée pour notre menu personnalisé dans le [Ajout, modification et suppression des Menus](#Adding,_Editing_and_Deleting_Menus) section ci-dessus. Basculer vers le **connexion inspecteur** et sélectionnez le **répondeur première** dans le **hiérarchie de l’Interface**. Faites défiler vers le bas et recherchez le `phraseAddress:` action. Faites glisser une ligne à partir du cercle sur l’action pour le **adresse** élément de menu :

    [![En faisant glisser permet de rattacher une action](menu-images/dock04.png "en faisant glisser permet de rattacher une action")](menu-images/dock04-large.png#lightbox)
4. Répétez pour tous les autres éléments de menu les joindre à leurs actions correspondantes : 

    [![Les actions requises](menu-images/dock05.png "les actions requises")](menu-images/dock05-large.png#lightbox)
5. Ensuite, sélectionnez le **Application** dans les **hiérarchie de l’Interface**. Dans le **connexion inspecteur**, faites glisser une ligne à partir du cercle le `dockMenu` prise au menu nous venons de créer :

    [![En faisant glisser la rattacher à la prise de](menu-images/dock06.png "en faisant glisser la rattacher à la sortie")](menu-images/dock06-large.png#lightbox)
6. Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.
7. Double-cliquez sur le **Info.plist** fichier à ouvrir pour le modifier : 

    [![Modification du fichier Info.plist](menu-images/dock07.png "Modification du fichier Info.plist")](menu-images/dock07-large.png#lightbox)
8. Cliquez sur le **Source** onglet en bas de l’écran : 

    [![En sélectionnant la vue de Source](menu-images/dock08.png "en sélectionnant la vue de Source")](menu-images/dock08-large.png#lightbox)
9. Cliquez sur **ajouter une nouvelle entrée**définir le nom de propriété à « AppleDockMenu » et la valeur à « DockMenu » (le nom de notre nouveau fichier .xib sans l’extension), cliquez sur le bouton plus vert : 

    [![Ajout de l’élément DockMenu](menu-images/dock09.png "Ajout de l’élément DockMenu")](menu-images/dock09-large.png#lightbox)

Si nous exécuter notre application et avec le bouton droit sur son icône dans la station d’accueil, nos nouveaux éléments de menu apparaît à présent :

![Un exemple du menu d’ancrage en cours d’exécution](menu-images/dock10.png "un exemple du menu d’ancrage en cours d’exécution")

Si vous sélectionnez un des éléments personnalisés à partir du menu, le texte dans la vue de texte est modifié.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Bouton contextuelle et les listes déroulantes

Un bouton déroulant affiche un élément sélectionné et présente la liste des options à sélectionner à partir de clic de l’utilisateur. Une liste déroulante est un type de bouton déroulant généralement utilisé pour la sélection des commandes spécifiques au contexte de la tâche en cours. Les deux peuvent apparaître n’importe où dans une fenêtre.

Nous allons créer un bouton déroulant personnalisé pour notre application en procédant comme suit :

1. Modifier le **Main.storyboard** fichier dans Xcode et faites glisser un **bouton de menu contextuel** à partir de la **inspecteur de bibliothèque** sur le **panneau** fenêtre que nous avons créé dans le [des menus contextuels](#Contextual_Menus) section : 

    [![Ajout d’un bouton de menu contextuel](menu-images/popup01.png "Ajout d’un bouton de menu contextuel")](menu-images/popup01-large.png#lightbox)
2. Ajouter un nouvel élément de menu et définir les titres des éléments dans le menu contextuel pour : **adresse**, **Date**, **salutation**, et **Signature** 

    [![Configurer les éléments de menu](menu-images/popup02.png "configurant les éléments de menu")](menu-images/popup02-large.png#lightbox)
3. Ensuite, nous allons connecter nos nouveaux éléments de menu pour les actions que vous avez créée pour notre menu personnalisé dans le [Ajout, modification et suppression des Menus](#Adding,_Editing_and_Deleting_Menus) section ci-dessus. Basculer vers le **connexion inspecteur** et sélectionnez le **répondeur première** dans le **hiérarchie de l’Interface**. Faites défiler vers le bas et recherchez le `phraseAddress:` action. Faites glisser une ligne à partir du cercle sur l’action pour le **adresse** élément de menu : 

    [![En faisant glisser permet de rattacher une action](menu-images/popup03.png "en faisant glisser permet de rattacher une action")](menu-images/popup03-large.png#lightbox)
4. Répétez pour tous les autres éléments de menu les joindre à leurs actions correspondantes : 

    [![Toutes les actions nécessaires](menu-images/popup04.png "toutes les actions nécessaires")](menu-images/popup04-large.png#lightbox)
5. Enregistrez vos modifications et revenez à Visual Studio pour Mac pour la synchronisation avec Xcode.

Maintenant, si nous exécuter notre application et que vous sélectionnez un élément dans la fenêtre contextuelle, le texte dans la vue de texte change :

![Un exemple de la fenêtre en cours d’exécution](menu-images/popup05.png "un exemple de la fenêtre en cours d’exécution")

Vous pouvez créer et travailler avec les listes déroulantes dans la même façon sous forme de boutons publicitaires. Au lieu d’attacher à des actions existant, vous pouvez créer vos propres actions comme nous l’avons fait pour notre menu contextuel dans le [des menus contextuels](#Contextual_Menus) section.

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des menus et des éléments de menu dans une application Xamarin.Mac. Tout d’abord, nous avons examiné la barre de menus, puis nous avons étudié la création des menus contextuels, ensuite nous avons examiné les menus de barre d’état personnalisé ancrer des menus. Enfin, nous avons abordé les menus déroulants et listes déroulantes.


## <a name="related-links"></a>Liens associés

- [MacMenus (exemple)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Indications de l’Interface humaine - Menus](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Présentation des Menus de l’Application et les listes contextuelles](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
