---
title: Listes sources
description: "Cet article traite de l’utilisation de listes de sources dans une application Xamarin.Mac. Il décrit la création et gestion des listes de sources dans Xcode et Générateur de l’Interface et interagir avec elles dans le code c#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1fba2219a10a83043097d1b5ca6fb293de4d6c70
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="source-lists"></a>Listes sources

_Cet article traite de l’utilisation de listes de sources dans une application Xamarin.Mac. Il décrit la création et gestion des listes de sources dans Xcode et Générateur de l’Interface et interagir avec elles dans le code c#._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même Source qui répertorie un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer votre Source de listes (ou éventuellement de les créer directement dans le code C#).

Une liste de Source est un type spécial de mode plan utilisé pour afficher la source d’une action, telles que la barre de recherche ou iTunes.

[![](source-list-images/source05.png "Un exemple de liste source")](source-list-images/source05.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de listes de code Source dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Introduction aux listes sources

Comme indiqué ci-dessus, une liste de Source est qu'un type spécial de mode plan utilisé pour afficher la source d’une action, telles que la barre de recherche ou iTunes. Une liste de Source est un type de Table qui permet à l’utilisateur de développer ou réduire des lignes de données hiérarchiques. Contrairement à une vue de Table, des éléments dans une liste des sources ne sont pas dans une liste plate, ils sont organisés dans une hiérarchie, tels que les fichiers et dossiers sur un disque dur. Si un élément dans une liste Source contient d’autres éléments, peuvent être développé ou réduit par l’utilisateur.

La liste Source est une vue hiérarchique spécialement style (`NSOutlineView`), qui est lui-même une sous-classe de la vue de Table (`NSTableView`) et par conséquent, beaucoup de son comportement hérite de sa classe parente. Par conséquent, de nombreuses opérations prises en charge par une vue hiérarchique, sont également pris en charge une liste Source. Une application Xamarin.Mac possède le contrôle de ces fonctionnalités et pouvez configurer les paramètres de la liste Source (soit dans le code ou le constructeur d’Interface) pour autoriser ou interdire certaines opérations.

Une liste de sources ne stocke pas ses propres données, au lieu de cela, elle s’appuie sur une Source de données (`NSOutlineViewDataSource`) pour fournir les lignes et colonnes nécessaires sur un en fonction des besoins.

Comportement d’une liste Source peut être personnalisé en fournissant une sous-classe du délégué vue hiérarchique (`NSOutlineViewDelegate`) pour prendre en charge du type de plan pour sélectionner des fonctionnalités, élément de sélection et de modification, de suivi personnalisé et des vues personnalisées pour des éléments individuels.

Dans la mesure où une liste de sources de son comportement et les fonctionnalités partage avec une vue de Table et une vue hiérarchique, vous pouvez souhaiter passer par notre [tableaux](~/mac/user-interface/table-view.md) et [mode plan](~/mac/user-interface/outline-view.md) documentation avant de continuer Dans cet article.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Utilisation des listes de code Source

Une liste de Source est un type spécial de mode plan utilisé pour afficher la source d’une action, telles que la barre de recherche ou iTunes. Contrairement aux vues de plan, avant que nous définissons notre liste Source dans le Générateur de Interface, nous allons créer les classes de stockage dans Xamarin.Mac.

Tout d’abord, nous allons créer un nouveau `SourceListItem` classe pour contenir les données de notre liste Source. Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `SourceListItem` pour le **nom** et cliquez sur le **nouveau** bouton :

[![](source-list-images/source01.png "Ajout d’une classe vide")](source-list-images/source01.png#lightbox)

Rendre le `SourceListItem.cs` fichier ressemble les éléments suivants : 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `SourceListDataSource` pour le **nom** et cliquez sur le **nouveau** bouton. Rendre le `SourceListDataSource.cs` fichier ressemble les éléments suivants :

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

Cela fournit les données de notre liste Source.

Dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `SourceListDelegate` pour le **nom** et cliquez sur le **nouveau** bouton. Rendre le `SourceListDelegate.cs` fichier ressemble les éléments suivants :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

Cela fournit le comportement de notre liste Source.

Enfin, dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **nouveau fichier...** Sélectionnez **général** > **classe vide**, entrez `SourceListView` pour le **nom** et cliquez sur le **nouveau** bouton. Rendre le `SourceListView.cs` fichier ressemble les éléments suivants :

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Cette opération crée une sous-classe personnalisée et réutilisable de `NSOutlineView` (`SourceListView`) que nous pouvons utiliser pour piloter la liste Source dans n’importe quelle application Xamarin.Mac que nous prenons.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Créer et maintenir les listes sources dans Xcode

Maintenant, nous allons créer notre liste Source dans le Générateur de l’Interface. Double-cliquez sur le `Main.storyboard` fichier à ouvrir pour le modifier dans le Générateur de Interface et faites glisser à partir de fractionnement la **bibliothèque inspecteur**, ajoutez-le au contrôleur de vue et la définir à redimensionner avec la vue dans le **éditeur de contraintes** :

[![](source-list-images/source00.png "Modification des contraintes")](source-list-images/source00.png#lightbox)

Ensuite, faites glisser une liste de sources de la **bibliothèque inspecteur**, ajoutez-le au côté gauche de la vue de fractionnement et la définir à redimensionner avec la vue dans le **éditeur de contraintes**:

[![](source-list-images/source02.png "Modification des contraintes")](source-list-images/source02.png#lightbox)

Ensuite, basculez vers le **identité vue**, sélectionnez la liste des sources et le modifier de **classe** à `SourceListView`:

[![](source-list-images/source03.png "Définition du nom de classe")](source-list-images/source03.png#lightbox)

Enfin, créez un **prise** pour notre liste Source appelé `SourceList` dans le `ViewController.h` fichier :

[![](source-list-images/source04.png "Configuration d’une prise de courant")](source-list-images/source04.png#lightbox)

Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Remplissage de la liste Source

Permet de modifier le `RotationWindow.cs` de fichiers dans Visual Studio pour Mac et le rendre de `AwakeFromNib` détaillée de la méthode comme suit :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

Le `Initialize ()` méthode doit être appelée par rapport à notre liste de Source **prise** _avant_ tous les éléments sont ajoutés à ce dernier. Pour chaque groupe d’articles, nous créer un élément parent et puis ajoutez les éléments de sub à cet élément de groupe. Chaque groupe est ensuite ajoutée à la collection de la liste des sources `SourceList.AddItem (...)`. Les deux dernières lignes de charger les données pour la liste des sources et développe tous les groupes :

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Enfin, modifiez le `AppDelegate.cs` et effectuez le `DidFinishLaunching` détaillée de la méthode comme suit :

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Si nous exécutons notre application, les informations suivantes s’affichent :

[![](source-list-images/source05.png "Un exemple d’application exécuter")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation de listes de code Source dans une application Xamarin.Mac. Nous avons vu comment créer et tenir à jour la Source de listes dans le Générateur de Xcode Interface et utiliser des listes de code Source en code c#.

## <a name="related-links"></a>Liens associés

- [MacOutlines (sample)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vues Table](~/mac/user-interface/table-view.md)
- [Modes Plan](~/mac/user-interface/outline-view.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Présentation de montrer les vues](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
