---
title: Vues de collection
description: Cet article décrit l’utilisation des vues de collection dans une application Xamarin.Mac. Elle couvre la création et vues de collection dans Xcode et Générateur de l’Interface de gestion et leur utilisation par programmation.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: f391a9df8a0d4012637c503bc95c6de2185dd2f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="collection-views"></a>Vues de collection

_Cet article décrit l’utilisation des vues de collection dans une application Xamarin.Mac. Elle couvre la création et vues de collection dans Xcode et Générateur de l’Interface de gestion et leur utilisation par programmation._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, le développeur a accès à la même vue de Collection AppKit qui contrôle un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, le développeur utilise de Xcode _Interface Générateur_ pour créer et gérer des vues de Collection.

A `NSCollectionView` affiche une grille de sous-vues organisés en utilisant un `NSCollectionViewLayout`. Chaque sous-affichage dans la grille est représenté par un `NSCollectionViewItem` qui gère le chargement du contenu de la vue à partir d’un `.xib` fichier.

[![Un exemple d’application exécuter](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

Cet article décrit les principes fondamentaux de l’utilisation des vues de Collection dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques qui sont utilisés dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>À propos des vues de Collection

L’objectif principal d’une vue de Collection (`NSCollectionView`) consiste à regrouper visuellement un groupe d’objets dans une structure organisée à l’aide d’une mise en page de la vue de Collection (`NSCollectionViewLayout`), chaque objet (`NSCollectionViewItem`) mise en route de sa propre vue de la collection de plus grande. Vues de collection de travail via les techniques de liaison de données et le codage de la clé-valeur et par conséquent, vous devez lire le [une liaison de données et le codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) documentation avant de continuer avec cet article.

La vue de Collection n’a aucun intégrée, standard, Collection vue élément (comme le fait d’un plan ou une vue de Table), le développeur est responsable de la conception et implémentation d’un _Prototype vue_ à l’aide d’autres contrôles AppKit tels que des champs d’Image , Les champs de texte, les étiquettes, etc. Cette vue de Prototype sera utilisée pour afficher et travailler avec chaque élément géré par la vue de Collection et est stockée dans un `.xib` fichier.

Étant donné que le développeur est responsable de l’apparence d’un élément de la vue de Collection, la vue de Collection n’a aucune prise en charge intégrée pour mettre en surbrillance un élément sélectionné dans la grille. Implémentation de cette fonctionnalité est abordée dans cet article.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Définition du modèle de données

Avant la liaison d’une vue de Collection dans l’Interface générateur, une clé-valeur de codage (KVM) de données / classe conforme d’observation clé-valeur (KVO) doit être défini dans l’application Xamarin.Mac en tant que le _modèle de données_ pour la liaison. Le modèle de données fournit toutes les données qui seront affichera dans la collection et reçoit toutes les modifications des données effectués par l’utilisateur dans l’interface utilisateur pendant l’exécution de l’application.

Prenons l’exemple d’une application qui gère un groupe d’employés, la classe suivante peut être utilisée pour définir le modèle de données :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Le `PersonModel` modèle de données sera utilisé dans le reste de cet article.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Utilisation d’une vue de Collection

Liaison de données avec une vue de Collection est très similaire comme liaison avec une vue de Table, en tant que `NSCollectionViewDataSource` est utilisé pour fournir des données pour la collection. Étant donné que la vue de collection n’a pas un format d’affichage prédéfini, un travail supplémentaire est requis pour fournir des commentaires d’interaction utilisateur et pour effectuer le suivi de la sélection de l’utilisateur.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Création du Prototype de cellule

Étant donné que la vue de Collection n’inclut pas un prototype de cellule par défaut, le développeur doit ajouter un ou plusieurs `.xib` fichiers à l’application Xamarin.Mac pour définir la disposition et le contenu des cellules individuelles.

Effectuez ce qui suit :

1. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le nom du projet et sélectionnez **ajouter** > **nouveau fichier...**
2. Sélectionnez **Mac** > **View Controller**, lui donner un nom (tel que `EmployeeItem` dans cet exemple) et cliquez sur le **nouveau** bouton permettant de créer : 

    ![Ajoutez un nouveau contrôleur de vue](collection-view-images/proto01.png)

    Cette opération ajoute une `EmployeeItem.cs`, `EmployeeItemController.cs` et `EmployeeItemController.xib` fichier à la solution du projet.
3. Double-cliquez sur le `EmployeeItemController.xib` fichier à ouvrir pour le modifier dans le Générateur de Xcode Interface.
4. Ajouter un `NSBox`, `NSImageView` et deux `NSLabel` contrôles à la vue et les présenter comme suit :

    ![Définition de la disposition du prototype de cellule](collection-view-images/proto02.png)
5. Ouvrez le **Assistant éditeur** et créer un **prise** pour la `NSBox` afin qu’il peut être utilisé pour indiquer l’état de sélection d’une cellule :

    ![Exposer le NSBox dans une prise de courant](collection-view-images/proto03.png)
6. Retour à la **éditeur Standard** et sélectionnez la vue de l’Image.
7. Dans le **l’inspecteur de liaison**, sélectionnez **lier à** > **propriétaire du fichier** et entrez un **chemin de la clé** de `self.Person.Icon`:

    ![L’icône de liaison](collection-view-images/proto04.png)
8. Sélectionnez la première étiquette et dans le **l’inspecteur de liaison**, sélectionnez **lier à** > **propriétaire du fichier** et entrez un **modèle de chemin de la clé**de `self.Person.Name`:

    ![Le nom de la liaison](collection-view-images/proto05.png)
9. Sélectionnez le deuxième contrôle Label et dans le **l’inspecteur de liaison**, sélectionnez **lier à** > **propriétaire du fichier** et entrez un **modèle de chemin de la clé**de `self.Person.Occupation`:

    ![L’activité de liaison](collection-view-images/proto06.png)
10. Enregistrer les modifications apportées à la `.xib` de fichiers et de revenir à Visual Studio pour synchroniser les modifications.

Modifier la `EmployeeItemController.cs` de fichier et le rendre l’aspect suivant :

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

Examinez ce code en détail, la classe hérite de `NSCollectionViewItem` afin qu’il puisse agir en tant que prototype pour une cellule de la vue de Collection. Le `Person` propriété expose la classe qui a été utilisée pour lier les données à la vue de l’Image et les étiquettes dans Xcode. Il s’agit d’une instance de la `PersonModel` créé ci-dessus.

Le `BackgroundColor` propriété est un raccourci vers le `NSBox` du contrôle `FillColor` qui sera utilisé pour afficher l’état de sélection d’une cellule. En remplaçant le `Selected` propriété de la `NSCollectionViewItem`, le code suivant définit ou efface l’état de cette sélection :

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>Création de la Source de données de vue de Collection

Une Source de données de vue de Collection (`NSCollectionViewDataSource`) fournit toutes les données pour une vue de Collection, crée et remplit une cellule de la vue de Collection (à l’aide de la `.xib` prototype) selon les besoins pour chaque élément dans la collection.

Ajouter une nouvelle classe le projet, appeler `CollectionViewDataSource` et qu’elle ressemble à ce qui suit :

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

Examinez ce code en détail, la classe hérite de `NSCollectionViewDataSource` et expose une liste de `PersonModel` instances via son `Data` propriété.

Étant donné que cette collection a uniquement une section, le code substitue la `GetNumberOfSections` méthode et retourne toujours `1`. En outre, le `GetNumberofItems` méthode est substituée à elle renvoie le nombre d’éléments dans le `Data` liste de propriétés.

Le `GetItem` méthode est appelée chaque fois qu’une nouvelle cellule est obligatoire et se présente comme suit :

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

Le `MakeItem` méthode de la vue de Collection est appelée pour créer ou de retourner une instance réutilisable de le `EmployeeItemController` et son `Person` est définie sur élément affiché dans la cellule demandée. 

Le `EmployeeItemController` doit être inscrit auprès du contrôleur de vue de Collection au préalable à l’aide du code suivant :

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

Le **identificateur** (`EmployeeCell`) utilisé dans le `MakeItem` appeler _doit_ correspond au nom du contrôleur de vue qui a été inscrit avec la vue de Collection. Cette étape est abordée en détail ci-dessous.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Sélection d’éléments de gestion

Pour gérer la sélection et la désélection d’éléments de la collection, une `NSCollectionViewDelegate` sera nécessaire. Étant donné que cet exemple utilisera intégrée dans `NSCollectionViewFlowLayout` le type de disposition, une `NSCollectionViewDelegateFlowLayout` une version spécifique de ce délégué sera nécessaire.

Ajoutez une nouvelle classe au projet, appelez `CollectionViewDelegate` et qu’elle ressemble à ce qui suit :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

Le `ItemsSelected` et `ItemsDeselected` les méthodes sont substituées et utilisés pour définir ou désactivez le `PersonSelected` propriété du contrôleur d’affichage qui gère la vue de Collection lorsque l’utilisateur sélectionne ou désélectionne un élément. Elle s’affiche en détail ci-dessous.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Création de la vue de Collection dans le constructeur d’Interface

Tous les éléments de prise en charge requis en place, le storyboard principal peut être modifié et une vue de Collection est ajouté à celle-ci.

Effectuez ce qui suit :

1. Double-cliquez sur le `Main.Storyboard` de fichiers dans le **l’Explorateur de solutions** pour ouvrir pour le modifier dans Xcode du constructeur d’Interface.
2. Faites glisser une vue de Collection dans la vue principale et la redimensionner pour remplir la vue :

    ![Ajout d’une vue de Collection à la disposition](collection-view-images/collection01.png)
3. Avec la vue de Collection sélectionné, utilisez l’éditeur de contrainte pour l’épingler à la vue lorsqu’il est redimensionné :

    ![Ajout de contraintes](collection-view-images/collection02.png)
4. Vérifiez que la vue de Collection est sélectionnée dans le **aire de conception** (et non le **affichage délimité** ou **affichage de l’élément** qui le contient), basculer vers le  **L’Assistant éditeur** et créer un **prise** pour la vue de collection :

    ![Ajout de contraintes](collection-view-images/collection03.png)
5. Enregistrer les modifications et revenir à Visual Studio à synchroniser.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Mettre tous ces objets

Tous les éléments de la prise en charge maintenant placés dans un emplacement avec une classe en tant que le modèle de données (`PersonModel`), un `NSCollectionViewDataSource` a été ajouté pour fournir des données, un `NSCollectionViewDelegateFlowLayout` a été créé pour gérer la sélection de l’élément et un `NSCollectionView` a été ajouté à la table de montage séquentiel principal et exposé comme une prise de courant (`EmployeeCollection`).

L’étape finale consiste à modifier le contrôleur de la vue qui contient la vue de Collection et de mettre tous les éléments ensemble pour remplir la collection et de gérer la sélection de l’élément.

Modifier la `ViewController.cs` de fichier et le rendre l’aspect suivant :

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

En consultant ce code dans les détails, un `Datasource` propriété est définie pour contenir une instance de la `CollectionViewDataSource` qui fournira les données pour la vue de Collection. A `PersonSelected` propriété est définie pour contenir le `PersonModel` qui représente l’élément actuellement sélectionné dans la vue de Collection. Cette propriété déclenche également la `SelectionChanged` événement lorsque la sélection change.

La `ConfigureCollectionView` classe est utilisée pour inscrire le contrôleur d’affichage qui agit en tant que le prototype de la cellule avec la vue de Collection à l’aide de la ligne suivante :

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Notez que la **identificateur** (`EmployeeCell`) permet d’inscrire les correspondances de prototype celle appelée dans le `GetItem` méthode de la `CollectionViewDataSource` définie ci-dessus :

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

En outre, le type du contrôleur vue **doit** correspond au nom de la `.xib` fichier qui définit le prototype **exactement**. Dans le cas de cet exemple, `EmployeeItemController` et `EmployeeItemController.xib`.

La disposition réelle des éléments dans la vue de Collection est contrôlée par une classe de mise en page de vue de Collection et peut être modifiée dynamiquement pendant l’exécution en affectant une nouvelle instance de la `CollectionViewLayout` propriété. Modification de cette propriété met à jour l’apparence de la vue de Collection sans animation de la modification.

Apple est fourni à deux types de mise en page intégré avec la vue de Collection qui va gérer les utilisations les plus courantes : `NSCollectionViewFlowLayout` et `NSCollectionViewGridLayout`. Si le développeur nécessaire à un format personnalisé, telles que la disposition des éléments dans un cercle, ils peuvent créer une instance personnalisée de `NSCollectionViewLayout` et substituez les méthodes requises pour obtenir l’effet souhaité.

Cet exemple utilise la disposition du flux par défaut de sorte qu’il crée une instance de la `NSCollectionViewFlowLayout` classe et le configure comme suit :

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

Le `ItemSize` propriété définit la taille de chaque cellule individuelle dans la collection. Le `SectionInset` propriété définit les marges du bord de la collection de cellules seront disposés dans. `MinimumInteritemSpacing` définit l’espacement minimal entre les éléments et `MinimumLineSpacing` définit l’espacement minimal entre les lignes dans la collection.

La disposition est affectée à la vue de Collection et une instance de la `CollectionViewDelegate` est attaché pour gérer la sélection de l’élément :

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

Le `PopulateWithData` méthode crée une nouvelle instance de la `CollectionViewDataSource`, remplit avec des données, joint à la vue de Collection et appelle la `ReloadData` méthode pour afficher les éléments :

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

Le `ViewDidLoad` méthode est substituée et appelle le `ConfigureCollectionView` et `PopulateWithData` méthodes pour afficher la vue de Collection finale à l’utilisateur :

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des vues de Collection dans une application Xamarin.Mac. Tout d’abord, il se présentait à exposer une classe c# pour Objective-C à l’aide de la clé-valeur de codage (KVM) et clé-valeur en observant (KVO). Ensuite, il vous a montré comment utiliser une classe conforme KVO et données lier à des vues de Collection dans le Générateur de Xcode Interface. Enfin, elle a montré comment interagir avec les vues de Collection dans le code c#.

## <a name="related-links"></a>Liens associés

- [MacCollectionNew (exemple)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
