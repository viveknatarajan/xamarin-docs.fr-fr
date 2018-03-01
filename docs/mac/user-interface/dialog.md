---
title: "Boîtes de dialogue"
description: "Cet article décrit l’utilisation des boîtes de dialogue et fenêtres modales dans une application Xamarin.Mac. Il décrit la création de fenêtres modales dans le générateur Xcode et d’Interface, utilisation des boîtes de dialogue standard et d’interagir avec ces contrôles dans le code c#."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9b65e870fae0074726d0bdd46d9eecbe99240e98
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="dialogs"></a>Boîtes de dialogue

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès aux boîtes de dialogue et fenêtres modales même qui un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos fenêtres modales (ou éventuellement de les créer directement dans le code C#).

Une boîte de dialogue s’affiche en réponse à une action de l’utilisateur et généralement fournit aux utilisateurs des moyens d’effectuer l’action. Une boîte de dialogue requiert une réponse de l’utilisateur avant d’être fermée.

Windows peut être utilisée dans un état non modal (par exemple, un éditeur de texte qui peut ouvrir plusieurs documents en une seule fois) ou modale (par exemple, une boîte de dialogue d’exportation et doit être fermée avant que l’application peut continuer).

[ ![](dialog-images/dialog03.png "Une boîte de dialogue Ouvrir")](dialog-images/dialog03.png)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation des boîtes de dialogue et fenêtres modales dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Introduction aux boîtes de dialogue

Une boîte de dialogue s’affiche en réponse à une action de l’utilisateur (par exemple, l’enregistrement d’un fichier) et offre un moyen pour les utilisateurs effectuer cette action. Une boîte de dialogue requiert une réponse de l’utilisateur avant d’être fermée.

En fonction d’Apple, il existe trois façons de présenter une boîte de dialogue :

- **Document Modal** -boîte de dialogue modale de Document A empêche l’utilisateur à partir de rien faire d’autre au sein d’un document donné jusqu'à ce qu’il a été abandonnée.
- **Application modale** : une application de boîte de dialogue modale empêche l’utilisateur d’interagir avec l’application jusqu'à ce qu’il a été abandonnée.
- **Non modal** boîte de dialogue non modale A permet aux utilisateurs de modifier les paramètres dans la boîte de dialogue lors de l’interaction toujours avec la fenêtre de document.

### <a name="modal-window"></a>Fenêtre modale

Norme `NSWindow` peut être utilisé comme une boîte de dialogue personnalisée en l’affichant sous forme modale :

[ ![](dialog-images/modal01.png "Une fenêtre modale exemple")](dialog-images/modal01.png)

### <a name="document-modal-dialog-sheets"></a>Feuilles de boîte de dialogue modale de document

A _feuille_ est une boîte de dialogue modale qui est attaché à une fenêtre de document donné, empêchant les utilisateurs d’interagir avec la fenêtre jusqu'à ce qu’ils faire disparaître la boîte de dialogue. Une feuille est attachée à la fenêtre à partir duquel il ressort, et qu’une seule feuille peut être ouverte pour une fenêtre à tout moment.

[ ![](dialog-images/sheet08.png "Un exemple de feuille modale")](dialog-images/sheet08.png)

### <a name="preferences-windows"></a>Préférences Windows

Une fenêtre de préférences est une boîte de dialogue non modale qui contient les paramètres d’application que l’utilisateur change rarement. Préférences Windows incluent souvent une barre d’outils qui permet à l’utilisateur basculer entre les différents groupes de paramètres :

[ ![](dialog-images/dialog02.png "Une fenêtre de préférence d’exemple")](dialog-images/dialog02.png)

### <a name="open-dialog"></a>Boîte de dialogue Ouvrir

La boîte de dialogue Ouvrir fournit aux utilisateurs un moyen cohérent de rechercher et ouvrir un élément dans une application :

[ ![](dialog-images/dialog03.png "Une boîte de dialogue Ouvrir")](dialog-images/dialog03.png)


### <a name="print-and-page-setup-dialogs"></a>Impression et boîtes de dialogue Mise en Page

macOS fournit impression standard et expérience de la Page le programme d’installation boîtes de dialogue que votre application peut afficher afin que les utilisateurs peuvent avoir une impression cohérente dans toutes les applications qu’ils utilisent.

La boîte de dialogue d’impression peuvent être affiché à la fois une boîte de dialogue flottante libre :

[ ![](dialog-images/print01.png "Une boîte de dialogue Imprimer")](dialog-images/print01.png)

Ou bien, il peut être affiché sous forme de tableau :

[ ![](dialog-images/print02.png "Une feuille d’impression")](dialog-images/print02.png)

La boîte de dialogue de configuration de Page peut être affichée en tant que les deux une boîte de dialogue flottante libre :

[ ![](dialog-images/print03.png "Une boîte de dialogue de configuration de page")](dialog-images/print03.png)

Ou bien, il peut être affiché sous forme de tableau :

[ ![](dialog-images/print04.png "Une feuille le programme d’installation")](dialog-images/print04.png)

### <a name="save-dialogs"></a>Enregistrer des boîtes de dialogue

La boîte de dialogue Enregistrer offre aux utilisateurs un moyen cohérent pour enregistrer un élément dans une application. La boîte de dialogue Enregistrer a deux états : **minimale** (également appelé réduite) :

[ ![](dialog-images/save01.png "Une boîte de dialogue d’enregistrement")](dialog-images/save01.png)

Et le **développée** état :

[ ![](dialog-images/save02.png "Boîte de dialogue Enregistrer une étendue")](dialog-images/save02.png)

Le **minimale** boîte de dialogue Enregistrer peut également être affiché sous forme de tableau :

[ ![](dialog-images/save03.png "Un nombre minimal d’enregistrement feuille")](dialog-images/save03.png)

Comme vous pouvez le **développée** boîte de dialogue Enregistrer :

[ ![](dialog-images/save04.png "Enregistrer une étendue")](dialog-images/save04.png)

Pour plus d’informations, consultez la [boîtes de dialogue](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Ajout d’une fenêtre modale à un projet

À l’exception de la fenêtre de document principal, une application Xamarin.Mac devrez peut-être afficher d’autres types de windows à l’utilisateur, telles que les préférences ou les panneaux de l’inspecteur.

Pour ajouter une nouvelle fenêtre, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, ouvrez le `Main.storyboard` fichier pour le modifier dans le Générateur de Xcode Interface.
2. Faites glisser une nouvelle **View Controller** dans l’aire de conception :

    [ ![](dialog-images/new01.png "Sélection d’un contrôleur de vue de la bibliothèque")](dialog-images/new01.png)
3. Dans le **inspecteur d’identité**, entrez `CustomDialogController` pour le **nom de la classe**: 

    [ ![](dialog-images/new02.png "Définition du nom de classe")](dialog-images/new02.png)
4. Basculez vers Visual Studio pour Mac, qu’il puisse synchroniser avec Xcode et créer le `CustomDialogController.h` fichier.
5. Revenir à Xcode et concevoir votre interface : 

    [ ![](dialog-images/new03.png "Conception de l’interface utilisateur dans Xcode")](dialog-images/new03.png)
6. Créer un **Segue modale** à partir de la fenêtre principale de votre application sur le nouveau contrôleur de vue en faisant glisser le contrôle à partir de l’élément d’interface utilisateur qui ouvre la boîte de dialogue dans une fenêtre de la boîte de dialogue. Affecter le **identificateur** `ModalSegue`: 

    [ ![](dialog-images/new06.png "Un segue modale")](dialog-images/new06.png)
6. Câble à distance les **Actions** et **prises**: 

    [ ![](dialog-images/new04.png "Configuration d’une Action")](dialog-images/new04.png)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Rendre le `CustomDialogController.cs` fichier ressemble les éléments suivants :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Ce code expose plusieurs propriétés pour définir le titre et la description de la boîte de dialogue et quelques événements pour réagir à la boîte de dialogue qui est annulée ou acceptée.

Modifiez ensuite la `ViewController.cs` de fichiers, substituez le `PrepareForSegue` (méthode) et qu’elle ressemble à ce qui suit :

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Ce code initialise le segue que nous avons défini dans le Générateur de Xcode Interface à notre boîte de dialogue et définit le titre et la description. Il gère également le choix de que l’utilisateur, dans la boîte de dialogue.

Nous pouvons exécuter notre application et afficher la boîte de dialogue personnalisée :

[ ![](dialog-images/new05.png "Une boîte de dialogue d’exemple")](dialog-images/new05.png)

Pour plus d’informations sur l’utilisation de windows dans une application Xamarin.Mac, veuillez consulter notre [utilisation des fenêtres](~/mac/user-interface/window.md) documentation.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Création d’une feuille de calcul personnalisé

A _feuille_ est une boîte de dialogue modale qui est attaché à une fenêtre de document donné, empêchant les utilisateurs d’interagir avec la fenêtre jusqu'à ce qu’ils faire disparaître la boîte de dialogue. Une feuille est attachée à la fenêtre à partir duquel il ressort, et qu’une seule feuille peut être ouverte pour une fenêtre à tout moment. 

Pour créer une feuille personnalisée dans Xamarin.Mac, nous allons effectuer les opérations suivantes :

1. Dans le **l’Explorateur de solutions**, ouvrez le `Main.storyboard` fichier pour le modifier dans le Générateur de Xcode Interface.
2. Faites glisser une nouvelle **View Controller** dans l’aire de conception :

    [ ![](dialog-images/new01.png "Sélection d’un contrôleur de vue de la bibliothèque")](dialog-images/new01.png)
2. Concevoir votre interface utilisateur :

    [ ![](dialog-images/sheet01.png "La conception de l’interface utilisateur")](dialog-images/sheet01.png)
3. Créer un **feuille Segue** à partir de votre fenêtre principale pour le nouveau contrôleur de vue : 

    [ ![](dialog-images/sheet02.png "Sélection du type de segue feuille")](dialog-images/sheet02.png)
4. Dans le **inspecteur d’identité**, nommez le contrôleur de vue **classe** `SheetViewController`: 

    [ ![](dialog-images/sheet03.png "Définition du nom de classe")](dialog-images/sheet03.png)
5. Définir les nécessaires **prises** et **Actions**: 

    [ ![](dialog-images/sheet04.png "Définition des prises et des Actions requises")](dialog-images/sheet04.png)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac à synchroniser.

Modifiez ensuite le `SheetViewController.cs` de fichier et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Modifiez ensuite la `ViewController.cs` de fichier, modifiez le `PrepareForSegue` (méthode) et le rendre l’aspect suivant :

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Si nous exécuter notre application et ouvrez la feuille, il sera attachée à la fenêtre :

[ ![](dialog-images/sheet08.png "Un exemple de feuille")](dialog-images/sheet08.png)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Création d’une boîte de dialogue Préférences

Avant de nous disposer de la vue de préférence dans le Générateur de Interface, nous devrons ajouter un type segue personnalisé pour gérer les préférences de commutation. Ajouter une nouvelle classe à votre projet et appelez-le `ReplaceViewSeque `. Modifiez la classe et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
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

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

Le segue personnalisé créé, nous pouvons ajouter une nouvelle fenêtre dans le Générateur de Xcode Interface pour gérer vos préférences.

Pour ajouter une nouvelle fenêtre, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, ouvrez le `Main.storyboard` fichier pour le modifier dans le Générateur de Xcode Interface.
2. Faites glisser une nouvelle **fenêtre contrôleur** dans l’aire de conception :

    [ ![](dialog-images/pref01.png "Sélectionnez un contrôleur de fenêtre à partir de la bibliothèque")](dialog-images/pref01.png)
3. Réorganiser la fenêtre près le **barre de menus** concepteur :

    [ ![](dialog-images/pref02.png "Ajout de la nouvelle fenêtre")](dialog-images/pref02.png)
4. Créer des copies du contrôleur Vue attachée car il sera onglets dans votre affichage de préférence :

    [ ![](dialog-images/pref03.png "Ajout de l’affichage des contrôleurs requis")](dialog-images/pref03.png)
5. Faites glisser une nouvelle **contrôleur de la barre d’outils** à partir de la **bibliothèque**:

    [ ![](dialog-images/pref04.png "Sélectionnez un contrôleur de barre d’outils à partir de la bibliothèque")](dialog-images/pref04.png)
6. Et déposez-la sur la fenêtre dans l’aire de conception :

    [ ![](dialog-images/pref05.png "Ajoutez un nouveau contrôleur de barre d’outils")](dialog-images/pref05.png)
7. La conception de votre barre d’outils de disposition :

    [ ![](dialog-images/pref06.png "La barre d’outils de disposition")](dialog-images/pref06.png)
8. Cliquez sur le contrôle et faites glisser à partir de chaque **bouton de barre d’outils** pour les vues que vous avez créé précédemment. Sélectionnez un **personnalisé** segue type :

    [ ![](dialog-images/pref07.png "La définition du type segue")](dialog-images/pref07.png)
9. Sélectionnez le nouveau Segue et définissez la **classe** à `ReplaceViewSegue`:

    [ ![](dialog-images/pref08.png "Définition de la classe segue")](dialog-images/pref08.png)
10. Dans le **Menubar concepteur** sur l’aire de conception, dans le Menu Application, sélectionnez **préférences...** la touche CTRL enfoncée et faire glisser vers la fenêtre de préférences pour créer un **afficher** segue :

    [ ![](dialog-images/pref09.png "La définition du type segue")](dialog-images/pref09.png)
11. Enregistrez vos modifications et revenir à Visual Studio pour Mac à synchroniser.

Si nous exécuter le code et sélectionnez le **préférences...**  à partir de la **Menu Application**, la fenêtre s’affichera :

[ ![](dialog-images/pref10.png "Une fenêtre de préférences de l’exemple")](dialog-images/pref10.png)

Pour plus d’informations sur l’utilisation de Windows et des barres d’outils, consultez notre [Windows](~/mac/user-interface/window.md) et [barres d’outils](~/mac/user-interface/toolbar.md) documentation.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Enregistrement et chargement des préférences

Dans une application de macOS classique, lorsque l’utilisateur apporte des modifications à un des préférences de l’utilisateur de l’application, ces modifications sont automatiquement enregistrées. Pour gérer cette situation dans une application Xamarin.Mac, le plus simple consiste à créer une classe unique pour gérer toutes les préférences de l’utilisateur et le partager à l’échelle du système.

Tout d’abord, ajoutez un nouveau `AppPreferences` classe au projet et hérite de `NSObject`. Les préférences seront conçus pour utiliser [une liaison de données et le codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) qui effectue le processus de création et de maintenance de la préférence forms beaucoup plus simple. Étant donné que les préférences seront compose d’une petite quantité des types de données simple, utilisez intégrée dans `NSUserDefaults` pour stocker et récupérer des valeurs.

Modifier la `AppPreferences.cs` de fichier et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Cette classe contient quelques routines d’assistance telles que `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, etc. pour rendre l’utilisation de `NSUserDefaults` plus facile. En outre, depuis `NSUserDefaults` n’a pas de méthode intégrée pour gérer les `NSColors`, le `NSColorToHexString` et `NSColorFromHexString` méthodes sont utilisées pour convertir les couleurs web basée sur des chaînes hexadécimales (`#RRGGBBAA` où `AA` est la transparence alpha) qui peut être facilement stockée et récupérée.

Dans le `AppDelegate.cs` de fichiers, créez une instance de la **AppPreferences** objet qui sera utilisé à l’échelle de l’application :

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Préférences de câblage aux vues de préférence

Connectez-vous ensuite la classe de préférence pour les éléments d’interface utilisateur dans la fenêtre de préférence et de vues créé ci-dessus. Dans le Générateur de l’Interface, sélectionnez un contrôleur de vue de préférence et basculer vers le **inspecteur d’identité**, créez une classe personnalisée pour le contrôleur : 

[ ![](dialog-images/prefs12.png "L’inspecteur de l’identité")](dialog-images/prefs12.png)

Revenez à Visual Studio pour Mac synchroniser vos modifications et ouvrez la classe qui vient d’être créée pour la modification. Rendre la classe ressembler aux éléments suivants :

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Notez que cette classe a fait deux choses ici : tout d’abord, est une application d’assistance `App` propriété afin que l’accès à la **AppDelegate** plus facile. Ensuite, le `Preferences` propriété expose global **AppPreferences** classe pour la liaison de données avec des contrôles d’IU est placé sur cette vue.

Ensuite, double-cliquez sur le fichier d’animation pour rouvrir dans le Générateur de Interface (et voir les modifications apportées juste au-dessus). Faites glisser les contrôles d’interface utilisateur requis pour générer l’interface Préférences dans la vue. Pour chaque contrôle, basculez vers le **l’inspecteur de liaison** et lier les propriétés individuelles de la **AppPreference** classe :

[ ![](dialog-images/prefs13.png "L’inspecteur de liaison")](dialog-images/prefs13.png)

Répétez les étapes ci-dessus pour tous les panneaux (affichage des contrôleurs) et les propriétés de préférence requises.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>L’application de préférence modifie pour toutes les fenêtres ouvertes

Comme indiqué ci-dessus, dans un type macOS application, lorsque l’utilisateur apporte des modifications à un des préférences de l’utilisateur de l’application, ces modifications sont enregistrées automatiquement et appliquées à toutes les fenêtres de l’utilisateur peut avoir ouvert dans l’application.

Planification et conception de windows et des préférences de votre application autorise ce processus se déroule sans heurts et en toute transparence à l’utilisateur final, avec un minimum de travail de codage.

Pour n’importe quelle fenêtre qui consommeront des préférences de l’application, ajouter la propriété d’assistance suivante à son contenu View Controller pour rendre l’accès à notre **AppDelegate** plus facile :

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Ensuite, ajoutez une classe pour configurer le contenu ou le comportement en fonction des préférences de l’utilisateur :

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Vous devez appeler la méthode de configuration lors de la première ouverture de la fenêtre pour vous assurer qu’il est conforme aux préférences de l’utilisateur :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Modifiez ensuite le `AppDelegate.cs` et ajoutez la méthode suivante à appliquer aucune préférence de modifications apportées à toutes les fenêtres ouvertes :

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Ensuite, ajoutez un `PreferenceWindowDelegate` classe au projet et rendre l’aspect suivant :

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

Cela entraîne des modifications de préférence à envoyer à toutes les fenêtres ouvertes lorsque la préférence de la fenêtre se ferme.

Enfin, modifier le contrôleur de la fenêtre de préférence et ajouter le délégué créé ci-dessus :

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Avec toutes ces modifications en place, si l’utilisateur modifie les préférences de l’application et ferme la fenêtre de préférence, les modifications seront appliquées à toutes les fenêtres ouvertes :

[ ![](dialog-images/prefs14.png "Une fenêtre de préférences de l’exemple")](dialog-images/prefs14.png)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>La boîte de dialogue Ouvrir

La boîte de dialogue Ouvrir offre aux utilisateurs un moyen cohérent de rechercher et ouvrir un élément dans une application. Pour afficher une boîte de dialogue Ouvrir dans une application Xamarin.Mac, utilisez le code suivant :

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

Dans le code ci-dessus, nous allons ouvrir une nouvelle fenêtre de document pour afficher le contenu du fichier. Vous devez remplacer ce code avec des fonctionnalités requises par votre application.

Les propriétés suivantes sont disponibles lorsque vous travaillez avec un `NSOpenPanel`:

- **CanChooseFiles** - si `true` l’utilisateur peut sélectionner des fichiers.
- **CanChooseDirectories** - si `true` l’utilisateur peut sélectionner des répertoires.
- **AllowsMultipleSelection** - si `true` l’utilisateur peut sélectionner plusieurs fichiers à la fois.
- **ResolveAliases** - si `true` sélection et l’alias, le résout en chemin d’accès du fichier d’origine.
- **AllowedFileTypes** -est un tableau de chaînes des types de fichiers que l’utilisateur peut sélectionner soit en tant qu’extension ou _utilitaire_. La valeur par défaut est `null`, ce qui permet à un fichier à ouvrir.

Le `RunModal ()` méthode affiche la boîte de dialogue Ouvrir et autoriser l’utilisateur à sélectionner les fichiers ou répertoires (comme spécifié par les propriétés) et retourne `1` si l’utilisateur clique sur le **ouvrir** bouton.

La boîte de dialogue Ouvrir retourne les fichiers sélectionnés ou les répertoires de l’utilisateur sous forme de tableau d’URL dans le `URL` propriété.

Si nous exécuter le programme et sélectionnez le **ouvrir...**  d’élément à partir de la **fichier** menu, le texte suivant s’affiche : 

[ ![](dialog-images/dialog03.png "Une boîte de dialogue Ouvrir")](dialog-images/dialog03.png)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>L’impression et boîtes de dialogue Mise en Page

macOS fournit impression standard et expérience de la Page le programme d’installation boîtes de dialogue que votre application peut afficher afin que les utilisateurs peuvent avoir une impression cohérente dans toutes les applications qu’ils utilisent.

Le code suivant affiche la boîte de dialogue Imprimer standard :

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Si vous définissez la `ShowPrintAsSheet` propriété `false`, exécutez l’application et afficher la boîte de dialogue d’impression, ce qui suit s’affiche :

[ ![](dialog-images/print01.png "Une boîte de dialogue Imprimer")](dialog-images/print01.png)

Si défini le `ShowPrintAsSheet` propriété `true`, exécutez l’application et afficher la boîte de dialogue d’impression, ce qui suit s’affiche :

[ ![](dialog-images/print02.png "Une feuille d’impression")](dialog-images/print02.png)

Le code suivant affiche la boîte de dialogue Mise en Page :

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Si vous définissez la `ShowPrintAsSheet` propriété `false`, exécutez l’application et afficher la boîte de dialogue d’impression, ce qui suit s’affiche :

[ ![](dialog-images/print03.png "Une boîte de dialogue de configuration de page")](dialog-images/print03.png)

Si défini le `ShowPrintAsSheet` propriété `true`, exécutez l’application et afficher la boîte de dialogue d’impression, ce qui suit s’affiche :

[ ![](dialog-images/print04.png "Une feuille le programme d’installation")](dialog-images/print04.png)

Pour plus d’informations sur l’utilisation de l’impression et de boîtes de dialogue de Page le programme d’installation, consultez le site d’Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) et [présentation de l’impression](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) documentation.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>La boîte de dialogue

La boîte de dialogue Enregistrer offre aux utilisateurs un moyen cohérent pour enregistrer un élément dans une application.

Le code suivant affiche la boîte de dialogue standard Enregistrer :

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

Le `AllowedFileTypes` propriété est un tableau de types de fichiers que l’utilisateur peut sélectionner pour enregistrer le fichier en tant que chaînes. Le type de fichier peut être spécifié soit en tant qu’extension ou _utilitaire_. La valeur par défaut est `null`, qui permet de n’importe quel type de fichier à utiliser.

Si vous définissez la `ShowSaveAsSheet` propriété `false`, exécutez l’application et sélectionnez **Enregistrer sous...**  à partir de la **fichier** menu, ce qui suit s’affiche :

[ ![](dialog-images/save01.png "Une boîte de dialogue d’enregistrement")](dialog-images/save01.png)

L’utilisateur peut développer la boîte de dialogue :

[ ![](dialog-images/save02.png "Boîte de dialogue Enregistrer une étendue")](dialog-images/save02.png)

Si vous définissez la `ShowSaveAsSheet` propriété `true`, exécutez l’application et sélectionnez **Enregistrer sous...**  à partir de la **fichier** menu, ce qui suit s’affiche :

[ ![](dialog-images/save03.png "Un enregistrement feuille")](dialog-images/save03.png)

L’utilisateur peut développer la boîte de dialogue :

[ ![](dialog-images/save04.png "Enregistrer une étendue")](dialog-images/save04.png)

Pour plus d’informations sur l’utilisation de la boîte de dialogue Enregistrer, consultez le site d’Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) documentation.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des fenêtres modales, feuilles et le système standard de boîtes de dialogue dans une application Xamarin.Mac. Nous avons vu les différents types et les utilisations des fenêtres modales, feuilles et boîtes de dialogue, comment créer et gérer des fenêtres modales et les feuilles dans Xcode Interface Builder et l’utilisation des fenêtres modales, feuilles et boîtes de dialogue en code c#.

## <a name="related-links"></a>Liens associés

- [MacWindows (sample)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Menus](~/mac/user-interface/menu.md)
- [Fenêtres](~/mac/user-interface/window.md)
- [Barres d’outils](~/mac/user-interface/toolbar.md)
- [Indications de l’Interface du système d’exploitation X humaines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Présentation des feuilles](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Présentation de l’impression](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
