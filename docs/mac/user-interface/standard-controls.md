---
title: Contrôles standard
description: Cet article traite de l’utilisation avec les contrôles AppKit standards tels que les boutons, les étiquettes, les champs de texte, les cases à cocher et segmenté des contrôles dans une application Xamarin.Mac. Il décrit en les ajoutant à une interface avec le Générateur de l’Interface et interagir avec elles dans le code.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3fe155508b60cbe502c3beca58426528d6f49c9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="standard-controls"></a>Contrôles standard

_Cet article traite de l’utilisation avec les contrôles AppKit standards tels que les boutons, les étiquettes, les champs de texte, les cases à cocher et segmenté des contrôles dans une application Xamarin.Mac. Il décrit en les ajoutant à une interface avec le Générateur de l’Interface et interagir avec elles dans le code._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même AppKit qui contrôle un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos contrôles Appkit (ou éventuellement de les créer directement dans le code C#).

Les contrôles de AppKit sont les éléments d’interface utilisateur qui sont utilisées pour créer l’Interface utilisateur de votre application Xamarin.Mac. Ils se composent des éléments tels que des boutons, des étiquettes, des champs de texte, des cases à cocher et des contrôles segmentés et provoquent des actions instantanées ou résultats visibles lorsqu’un utilisateur manipule les.

[![](standard-controls-images/intro01.png "L’écran principal de l’application exemple")](standard-controls-images/intro01.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de contrôles de AppKit dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Introduction aux contrôles et aux vues

macOS (anciennement Mac OS X) fournit un ensemble standard de contrôles d’Interface utilisateur via la structure AppKit. Ils se composent des éléments tels que des boutons, des étiquettes, des champs de texte, des cases à cocher et des contrôles segmentés et provoquent des actions instantanées ou résultats visibles lorsqu’un utilisateur manipule les.

Tous les contrôles AppKit ont une apparence intégrée, standard, qui conviendrait à la plupart des utilisations, certaines spécifient une autre apparence pour une utilisation dans une zone de frame de fenêtre ou dans un _Vibrance effet_ contexte, tel que dans une zone de la barre latérale ou dans un Widget de centre de notifications.

Apple suggérer les instructions suivantes lorsque vous travaillez avec des contrôles de AppKit :

- Éviter de mélanger des tailles de contrôle dans la même vue.
- En règle générale, évitez de redimensionnement de contrôles verticalement.
- Utiliser la police système et la taille du texte approprié au sein d’un contrôle.
- Utiliser l’espacement entre les contrôles approprié.

Pour plus d’informations, veuillez consultez le [sur les contrôles et les vues](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Utilisation de contrôles dans un Frame de fenêtre

Il existe un sous-ensemble de contrôles AppKit qui incluent un style d’affichage qui leur permet d’être inclure dans la zone du cadre d’une fenêtre. Pour obtenir un exemple, consultez la barre d’outils de l’application de messagerie :

[![](standard-controls-images/mailapp.png "Un frame de fenêtre de Mac")](standard-controls-images/mailapp.png#lightbox)

- **Arrondir une texture de bouton** : `NSButton` avec un style de `NSTexturedRoundedBezelStyle`.
- **Une texture arrondi de contrôle segmenté** : `NSSegmentedControl` avec un style de `NSSegmentStyleTexturedRounded`.
- **Une texture arrondi de contrôle segmenté** : `NSSegmentedControl` avec un style de `NSSegmentStyleSeparated`.
- **Arrondir une texture de Menu contextuel** : `NSPopUpButton` avec un style de `NSTexturedRoundedBezelStyle`.
- **Arrondir une texture un Menu déroulant** : `NSPopUpButton` avec un style de `NSTexturedRoundedBezelStyle`.
- **Barre de recherche** : `NSSearchField`.

Apple suggérer les instructions suivantes lorsque vous travaillez avec des contrôles AppKit dans un Frame de fenêtre :

- N’utilisez pas dans le corps de la fenêtre styles de contrôle spécifique de Frame de fenêtre.
- N’utilisez pas les contrôles de corps de la fenêtre ou les styles dans la fenêtre Frame.

Pour plus d’informations, veuillez consultez le [sur les contrôles et les vues](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Création d’une Interface utilisateur dans le constructeur d’Interface

Lorsque vous créez une nouvelle application/Cocoa de Xamarin.Mac, vous obtenez une fenêtre vide, standard par défaut. Ce paramètre, windows est défini dans un `.storyboard` fichier inclus automatiquement dans le projet. Pour modifier votre conception de windows, dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` fichier :

[![](standard-controls-images/edit01.png "En sélectionnant le Storyboard principal dans l’Explorateur de solutions")](standard-controls-images/edit01.png#lightbox)

La conception de la fenêtre s’ouvre dans le Générateur de Xcode Interface :

[![](standard-controls-images/edit02.png "Modification du plan conceptuel dans Xcode")](standard-controls-images/edit02.png#lightbox)

Pour créer votre Interface utilisateur, vous devez faire glisser des éléments d’interface utilisateur (contrôles AppKit) de la **bibliothèque inspecteur** à la **éditeur de l’Interface** dans le constructeur d’Interface. Dans l’exemple ci-dessous, un **fractionnement Vertical** contrôle a été médicament à partir de la **bibliothèque inspecteur** et placé dans la fenêtre de la **éditeur de l’Interface**:

[![](standard-controls-images/edit03.png "Sélection d’un affichage fractionné à partir de la bibliothèque")](standard-controls-images/edit03.png#lightbox)

Pour plus d’informations sur la création d’une Interface utilisateur dans l’Interface générateur, consultez notre [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) documentation.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Dimensionnement et de positionnement

Une fois qu’un contrôle a été inclus dans l’Interface utilisateur, utilisez la **éditeur de contrainte** définir son emplacement et la taille en entrant manuellement les valeurs et de contrôler comment le contrôle est automatiquement positionné et taille lorsque la fenêtre parent ou la vue redimensionnement :

[![](standard-controls-images/edit04.png "Définition de contraintes")](standard-controls-images/edit04.png#lightbox)

Utilisez le **rouge I-FAISCEAUX** autour de l’extérieur de la **capable** zone _stick_ un contrôle à un emplacement donné (x, y). Par exemple : 

[![](standard-controls-images/edit05.png "Modification d’une contrainte")](standard-controls-images/edit05.png#lightbox)

Spécifie que le contrôle sélectionné (dans le **vue hiérarchie** & **éditeur de l’Interface**) sera bloquée à l’emplacement supérieure et droite de la fenêtre ou la vue comme il est redimensionné ou déplacé. 

Autres éléments de l’éditeur de propriétés telles que la hauteur et la largeur de contrôle :

[![](standard-controls-images/edit06.png "Définition de la hauteur")](standard-controls-images/edit06.png#lightbox)

Vous pouvez également contrôler l’alignement des éléments avec des contraintes à l’aide de la **alignement éditeur**:

[![](standard-controls-images/edit07.png "L’éditeur d’alignement")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> Contrairement à iOS où (0,0) est le supérieur coin gauche de l’écran, dans macOS (0,0) est le coin inférieur gauche. Il s’agit comme macOS utilise un système de coordonnées mathématique avec les valeurs de nombre augmentant de valeur vers le haut et vers la droite. Vous devez prendre cela en compte lors du placement de contrôles AppKit sur une Interface utilisateur.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Définition d’une classe personnalisée

Il existe des heures d’utilisation de contrôles de AppKit que vous avez sera devez sous-classe et contrôle existant et créer votre propre version personnalisée de cette classe. Par exemple, définissez une version personnalisée de la liste des sources :

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
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

Où les `[Register("SourceListView")]` instruction expose la `SourceListView` classe pour Objective-C, afin que celle-ci peut être utilisée dans le constructeur d’Interface. Pour plus d’informations, consultez la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes utilisées à câble une sauvegarde de vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

Avec le code ci-dessus en place, vous pouvez faire glisser un contrôle AppKit, du type de base que vous étendez, sur l’aire de conception (dans l’exemple ci-dessous, un **liste Source**), basculez vers le **identité inspecteur** et définissez le **classe personnalisée** au nom que vous avez exposée au Objective-C (exemple `SourceListView`) :

[![](standard-controls-images/edit10.png "Définition d’une classe personnalisée dans Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Exposition prises et Actions

Avant qu’un contrôle AppKit sont accessibles dans le code c#, elle doit être exposée en tant qu’un **prise** ou et **Action**. Faire cela sélectionnez contrôle donné, que ce soit le **hiérarchie de l’Interface** ou **éditeur de l’Interface** et basculez vers le **Assistant vue** (Assurez-vous d’avoir le `.h`de la fenêtre sélectionnée pour le modifier) :

[![](standard-controls-images/edit11.png "En sélectionnant le fichier approprié à modifier")](standard-controls-images/edit11.png#lightbox)

Faites glisser le contrôle à partir du contrôle AppKit sur le champ `.h` fichier pour commencer à créer un **prise** ou **Action**:

[![](standard-controls-images/edit12.png "En faisant glisser pour créer une sortie ou une Action")](standard-controls-images/edit12.png#lightbox)

Sélectionnez le type de l’exposition à créer et donner la **prise** ou **Action** un **nom**: 

[![](standard-controls-images/edit13.png "Configuration de la prise de courant ou l’Action")](standard-controls-images/edit13.png#lightbox)


Pour plus d’informations sur l’utilisation de **prises** et **Actions**, consultez la [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) section de notre [Introduction à Xcode et d’Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) documentation.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronisation des modifications avec Xcode

Lorsque vous basculez vers Visual Studio pour Mac à partir de Xcode, toutes les modifications que vous avez apportées dans Xcode sont automatiquement synchronisées avec votre projet Xamarin.Mac.

Si vous sélectionnez le `SplitViewController.designer.cs` dans les **l’Explorateur de solutions** vous serez en mesure de voir comment votre **prise** et **Action** câblées dans notre code c# :

[![](standard-controls-images/sync01.png "Synchronisation des modifications avec Xcode")](standard-controls-images/sync01.png#lightbox)

Notez comment la définition dans le `SplitViewController.designer.cs` fichier :

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Aligner sur la définition de la `MainWindow.h` fichier dans Xcode :

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

Comme vous pouvez le voir, Visual Studio pour Mac écoute les modifications apportées à la `.h` de fichiers, puis synchronise automatiquement ces modifications dans la `.designer.cs` fichier les exposer à votre application. Vous pouvez également remarquer que `SplitViewController.designer.cs` est une classe partielle, afin que Visual Studio pour Mac ne doit pas nécessairement modifier `SplitViewController.cs ` qui remplace toutes les modifications que nous avons apporté à la classe.

Normalement, vous n’avez jamais besoin ouvrir le `SplitViewController.designer.cs` vous-même, il a été présenté ici à des fins pédagogiques uniquement.

> [!IMPORTANT]
> Dans la plupart des cas, Visual Studio pour Mac voir les modifications effectuées dans Xcode automatiquement et les synchroniser à votre projet Xamarin.Mac. Dans le cas isolé où la synchronisation ne se produit pas automatiquement, revenez à Xcode, puis à nouveau à Visual Studio pour Mac. Cela déclenche normalement un cycle de synchronisation.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Utilisation des boutons

AppKit fournit plusieurs types de bouton qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [boutons](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Un exemple des différents types de boutons")](standard-controls-images/buttons01.png#lightbox)

Si un bouton a été exposé un **prise**, il appuie sur doit répondre le code suivant :

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Pour les boutons qui ont été exposées via **Actions**, un `public partial` méthode sera automatiquement créée pour vous avec le nom que vous avez choisi dans Xcode. Pour répondre à la **Action**, terminer la méthode partielle dans la classe qui le **Action** a été défini sur. Par exemple :

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Pour les boutons qui ont un état (telles que **sur** et **hors**), l’état peut être vérifié ou défini avec la `State` propriété par rapport à la `NSCellStateValue` enum. Par exemple :

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Où `NSCellStateValue` peut être :

- **Sur** - le bouton est enfoncé ou le contrôle est sélectionné (par exemple, la case à cocher une case à cocher).
- **Désactiver** - le bouton n’est pas enfoncé ou le contrôle n’est pas sélectionné.
- **Mixte** -un mélange de **sur** et **hors** États.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Marquer un bouton par défaut et définir des clés d’équivalent

Pour un bouton que vous avez ajoutés à une conception de l’interface utilisateur, vous pouvez marquer ce bouton, comme la _par défaut_ bouton est activé lorsque l’utilisateur appuie sur le **retour/entrée** touches du clavier. Dans macOS, ce bouton recevra une couleur d’arrière-plan bleu par défaut.

Pour définir un bouton par défaut, sélectionnez-le dans le Générateur de Xcode Interface. Ensuite, dans le **inspecteur de l’attribut**, sélectionnez le **clé équivalent** et appuyez sur la **retour/entrée** clé :

[![](standard-controls-images/buttons03.png "Modification de l’équivalent de clé")](standard-controls-images/buttons03.png#lightbox)

De même, vous pouvez affecter n’importe quelle séquence clé qui peut être utilisé pour activer le bouton à l’aide du clavier au lieu de la souris. Par exemple, en appuyant sur les touches de commande-C dans l’image ci-dessus.

Lorsque l’application est exécutée et la fenêtre avec le bouton est la clé et le focus, si l’utilisateur appuie sur la commande-C, l’Action pour le bouton est activée (en tant que-si l’utilisateur clique sur le bouton).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Utilisation des cases à cocher et des cases d’option

AppKit fournit plusieurs types de cases à cocher et les groupes de cases d’option qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [boutons](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Un exemple des types de case à cocher disponibles")](standard-controls-images/buttons02.png#lightbox)


Cases à cocher et des cases d’option (exposée via **prises**) ont un état (comme **sur** et **hors**), l’état peut être vérifié ou défini avec la `State` propriété par rapport à la `NSCellStateValue` enum. Par exemple :

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Où `NSCellStateValue` peut être :

- **Sur** - le bouton est enfoncé ou le contrôle est sélectionné (par exemple, la case à cocher une case à cocher).
- **Désactiver** - le bouton n’est pas enfoncé ou le contrôle n’est pas sélectionné.
- **Mixte** -un mélange de **sur** et **hors** États.

Pour sélectionner un bouton dans un groupe de cases d’option, exposer le bouton Radio pour sélectionner comme un **prise** et définir son `State` propriété. Par exemple :

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Pour obtenir une collection de cases d’option à agir en tant que groupe et gère automatiquement l’état sélectionné, créez un **Action** et attacher chaque bouton dans le groupe :

![](standard-controls-images/buttons04.png "Création d’une nouvelle Action")

Ensuite, affectez une valeur unique `Tag` pour chaque case d’option dans le **inspecteur de l’attribut**:

![](standard-controls-images/buttons05.png "Modification d’une étiquette de bouton radio")

Enregistrez vos modifications et revenir à Visual Studio pour Mac, ajoutez le code pour gérer les **Action** que tous les boutons radio sont attachés à :

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Vous pouvez utiliser le `Tag` propriété pour afficher le bouton d’option a été sélectionnée.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Utilisation des contrôles de Menu

AppKit fournit plusieurs types de contrôles de Menu qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [les contrôles Menu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Exemples de contrôles de menu")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Fournit les données de contrôle de Menu

Les contrôles de Menu disponibles pour macOS peut être définis pour remplir la liste déroulante à partir d’une liste interne (qui permettre être prédéfinie dans le Générateur de Interface ou remplie via du code) ou en fournissant votre propre source de données externe personnalisée.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Utilisation des données internes

Outre la définition des éléments dans le constructeur d’Interface, les contrôles de Menu (tels que `NSComboBox`), fournissent un ensemble complet de méthodes qui vous permettent d’ajouter, modifier ou supprimer les éléments de la liste interne qu’ils gèrent :

- `Add` -Ajoute un nouvel élément à la fin de la liste.
- `GetItem` -Retourne l’élément à l’index spécifié.
- `Insert` -Insère un nouvel élément dans la liste à l’emplacement donné.
- `IndexOf` -Retourne l’index de l’élément donné.
- `Remove` -Supprime l’élément de la liste.
- `RemoveAll` -Supprime tous les éléments de la liste.
- `RemoveAt` -Supprime l’élément à l’index spécifié.
- `Count` -Retourne le nombre d’éléments dans la liste.

> [!IMPORTANT]
> Si vous utilisez une Source de données externe (`UsesDataSource = true`), appel à l’une de ces méthodes lèvent une exception.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Utilisez une Source de données externes

Au lieu d’utiliser les données internes intégrées pour fournir les lignes de votre contrôle de Menu, vous pouvez éventuellement utiliser une Source de données externe et fournir votre propre magasin de stockage pour les éléments (par exemple, une base de données SQLite).

Pour utiliser une Source de données externe, vous allez créer une instance de la Source de données du contrôle de Menu (`NSComboBoxDataSource` par exemple) et remplacent plusieurs méthodes pour fournir les données nécessaires :

- `ItemCount` -Retourne le nombre d’éléments dans la liste.
- `ObjectValueForItem` -Retourne la valeur de l’élément pour un index donné.
- `IndexOfItem` -Retourne l’index de la valeur de l’élément donné.
- `CompletedString` -Retourne la première valeur d’élément correspondant pour la valeur de l’élément partiellement typé. Cette méthode est appelée uniquement si la saisie semi-automatique a été activée (`Completes = true`).

Consultez le [bases de données et des zones de liste modifiable](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) section de la [des bases de données](~/mac/app-fundamentals/databases.md) document pour plus d’informations.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Ajustement de l’apparence

Les méthodes suivantes sont disponibles pour ajuster l’apparence du contrôle de Menu :

- `HasVerticalScroller` -If `true`, le contrôle affiche une barre de défilement verticale. 
- `VisibleItems` -Ajustez le nombre d’éléments affichés lorsque le contrôle est ouvert. La valeur par défaut est cinq (5).
- `IntercellSpacing` -Ajuster la quantité d’espace autour d’un élément donné en fournissant un `NSSize` où le `Width` spécifie les marges gauche et droite et `Height` Spécifie l’espace avant et après un élément.
- `ItemHeight` -Spécifie la hauteur de chaque élément dans la liste.

Pour les types de liste déroulante de `NSPopupButtons`, le premier élément de Menu fournit le titre du contrôle. Par exemple : 

[![](standard-controls-images/menu02.png "Un exemple de contrôle menu")](standard-controls-images/menu02.png#lightbox)

Pour modifier le titre, exposer cet élément comme une **prise** et utilisez du code comme suit :

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Manipuler les éléments sélectionnés

Les méthodes et propriétés suivantes permettent de manipuler les éléments sélectionnés dans la liste de contrôle de Menu :

- `SelectItem` -Sélectionne l’élément à l’index spécifié.
- `Select` -Sélectionnez la valeur de l’élément donné.
- `DeselectItem` -Désélectionne l’élément à l’index spécifié.
- `SelectedIndex` -Retourne l’index de l’élément actuellement sélectionné.
- `SelectedValue` -Retourne la valeur de l’élément actuellement sélectionné.

Utilisez le `ScrollItemAtIndexToTop` pour présenter l’élément à l’index donné en haut de la liste et le `ScrollItemAtIndexToVisible` pour accéder à la liste jusqu'à ce que l’élément à l’index spécifié est visible.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Réponse aux événements

Les contrôles de menu fournissent les événements suivants pour répondre à une interaction utilisateur :

- `SelectionChanged` -Est appelé lorsque l’utilisateur a sélectionné une valeur dans la liste.
- `SelectionIsChanging` -Est appelée avant que le nouvel élément sélectionné par l’utilisateur devienne la sélection active.
- `WillPopup` -Est appelée avant l’affichage de la liste déroulante d’éléments.
- `WillDismiss` -Est appelée avant la fermeture de la liste déroulante d’éléments.

Pour `NSComboBox` contrôles, ils incluent tous les événements mêmes comme le `NSTextField`, telles que le `Changed` événement qui est appelé chaque fois que l’utilisateur modifie la valeur du texte dans la zone de liste déroulante.

Si vous le souhaitez, vous pouvez répondre le des éléments de Menu données internes définies dans le constructeur d’Interface sélectionné en attachant l’élément à un **Action** et utiliser du code semblable au suivant pour répondre à **Action** en cours déclenché par l’utilisateur :

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Pour plus d’informations sur l’utilisation des Menus et des contrôles de Menu, consultez notre [Menus](~/mac/user-interface/menu.md) et [bouton contextuel et le menu déroulant répertorie](~/mac/user-interface/menu.md) documentation.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Utilisation des contrôles de sélection

AppKit fournit plusieurs types de contrôles de sélection qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [contrôles de sélection](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Exemples de contrôles de sélection")](standard-controls-images/select01.png#lightbox)

Il existe deux façons pour savoir quand un contrôle de sélection ayant une interaction utilisateur, en les exposant en tant qu’un **Action**. Par exemple :

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Ou en attachant un **délégué** à la `Activated` événement. Par exemple :

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Pour définir ou lire la valeur d’un contrôle de sélection, utilisez le `IntValue` propriété. Par exemple :

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Les contrôles de spécialisation (par exemple, couleur et de Image bien) ont des propriétés spécifiques de leurs types de valeur. Par exemple :

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

Le `NSDatePicker` a les propriétés suivantes pour travailler directement avec les Date et heure :

- **DateValue** -la valeur de date et l’heure actuelle en tant qu’un `NSDate`.
- **Local** -emplacement de l’utilisateur comme un `NSLocal`.
- **TimeInterval** -la valeur d’heure comme un `Double`.
- **Fuseau horaire** -fuseau horaire de l’utilisateur comme un `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Utilisation des contrôles de l’indicateur

AppKit fournit plusieurs types de contrôles d’indicateur qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [indicateur contrôles](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Exemples de contrôles indicateur")](standard-controls-images/level01.png#lightbox)

Il existe deux façons pour savoir quand un contrôle de l’indicateur a l’intervention de l’utilisateur, soit en les exposant en tant qu’un **Action** ou un **prise** et l’attachement d’un **délégué** à la `Activated`événement. Par exemple :

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Pour lire ou définir la valeur de l’indicateur de contrôle, utilisez le `DoubleValue` propriété. Par exemple :

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Les indicateurs de progression asynchrone indéterminé doivent être animées lorsque affichée. Utilisez la `StartAnimation` méthode pour lancer l’animation lorsqu’ils sont affichés. Par exemple :

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Appel de la `StopAnimation` méthode arrête l’animation.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Utilisation des contrôles de texte

AppKit fournit plusieurs types de contrôles de texte qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [contrôles de texte](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Exemples de contrôles de texte")](standard-controls-images/text01.png#lightbox)

Pour les champs de texte (`NSTextField`), les événements suivants peuvent être utilisés pour effectuer le suivi d’intervention de l’utilisateur :

- **Modifié** -est déclenché à tout moment, l’utilisateur modifie la valeur du champ. Par exemple, sur chaque caractère tapé.
- **EditingBegan** -est déclenché lorsque l’utilisateur sélectionne le champ pour la modification.
- **EditingEnded** - lorsque l’utilisateur appuie sur la touche entrée dans le champ ou le quitte.

Utilisez le `StringValue` propriété pour lire ou définir la valeur du champ. Par exemple :

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Pour les champs à afficheront ou modifier des valeurs numériques, vous pouvez utiliser le `IntValue` propriété. Par exemple :

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

Un `NSTextView` fournit une recherche en texte intégral proposée modifier et afficher la zone de mise en forme intégrés. Comme un `NSTextField`, utilisez le `StringValue` à lire ou définir la valeur de la zone propriété.

Pour obtenir un exemple d’un exemple complex de l’utilisation de vues de texte dans une application Xamarin.Mac, consultez la [SourceWriter, exemple d’application](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter est un éditeur de code source simple qui assure la prise en charge de la complétion de code et de la coloration syntaxique de base.

Le code SourceWriter a été entièrement commenté et, le cas échéant, des liens ont être fournis entre les principales technologies ou méthodes et des informations pertinentes dans la documentation des guides Xamarin.Mac.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Utilisation des vues de contenu

AppKit fournit plusieurs types de contenu des vues qui peut être utilisé dans votre conception de l’Interface utilisateur. Pour plus d’informations, consultez la [affichage du contenu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) section d’Apple [indications de l’Interface du système d’exploitation X humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Une vue de contenu d’exemple")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popover

Un popover est un élément d’interface utilisateur temporaire qui fournit une fonctionnalité qui est directement liée à un contrôle spécifique ou à une zone à l’écran. Un popover flotte au-dessus de la fenêtre qui contient le contrôle ou une région qui lui sont associés, ainsi que sa bordure une flèche pour indiquer le point à partir duquel il est apparu.

Pour créer un popover, procédez comme suit :

1. Ouvrez le `.storyboard` fichier de la fenêtre que vous souhaitez ajouter un popover à en double-cliquant dessus dans le **l’Explorateur de solutions**
2. Faites glisser un **afficher le contrôleur de** à partir de la **bibliothèque inspecteur** sur la **éditeur de l’Interface**: 

    [![](standard-controls-images/content02.png "Sélection d’un contrôleur de vue de la bibliothèque")](standard-controls-images/content02.png#lightbox)
4. Définir la taille et la disposition de la **vue personnalisée**: 

    [![](standard-controls-images/content04.png "Modification de la disposition")](standard-controls-images/content04.png#lightbox)
5. Cliquez sur le contrôle et faites glisser à partir de la source de la fenêtre contextuelle sur le **View Controller**: 

    [![](standard-controls-images/content05.png "Glisser-déplacer pour créer un segue")](standard-controls-images/content05.png#lightbox)
6. Sélectionnez **Popover** dans le menu contextuel : 

    [![](standard-controls-images/content06.png "La définition du type segue")](standard-controls-images/content06.png#lightbox)
7. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

<a name="Tab_Views" />

### <a name="tab-views"></a>Onglet affichages

Vues de l’onglet se compose d’une liste d’onglet (qui ressemble à un contrôle segmenté) combinées avec un ensemble de vues qui sont appelées _volets_. Lorsque l’utilisateur sélectionne un nouvel onglet, le volet qui lui est attaché s’affichera. Chaque volet contient son propre ensemble de contrôles.

Lorsque vous travaillez avec une vue de l’onglet dans le Générateur de Xcode Interface, utilisez la **inspecteur de l’attribut** pour définir le nombre d’onglets :

[![](standard-controls-images/content08.png "Modifier le nombre d’onglets")](standard-controls-images/content08.png#lightbox)

Sélectionnez chaque onglet dans le **hiérarchie de l’Interface** pour définir son **titre** et ajouter des éléments d’interface utilisateur pour son **volet**:

[![](standard-controls-images/content09.png "Modifier les onglets dans Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Contrôles de AppKit de liaison de données

À l’aide de techniques de codage de la clé-valeur et la liaison de données dans votre application Xamarin.Mac, vous pouvez réduire considérablement la quantité de code que vous devez écrire et conserver pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage de vos données de sauvegarde (_modèle de données_) à partir de votre premier plan mettre fin à l’Interface utilisateur (_Model-View-Controller_), début au plus facile à gérer, une application plus souple conception.

Clé-valeur de codage (KVM) est un mécanisme pour accéder aux propriétés d’un objet indirectement, à l’aide de clés (spécialement mise en forme des chaînes) pour identifier les propriétés au lieu d’y accéder via des variables d’instance ou des méthodes d’accesseur (`get/set`). En implémentant la clé-valeur de codage des accesseurs conformes dans votre application Xamarin.Mac, pour accéder à d’autres fonctionnalités macOS telles que la clé-valeur en observant (KVO), la liaison de données, données de base, les liaisons/Cocoa et scriptability.

Pour plus d’informations, consultez la [liaison de données Simple](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) section de notre [une liaison de données et le codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) documentation.


<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée de travailler avec les contrôles AppKit standards tels que des boutons, des étiquettes, des champs de texte, des cases à cocher et des contrôles segmentés dans une application Xamarin.Mac. Il couvert en les ajoutant à une conception de l’Interface utilisateur dans le Générateur de Xcode Interface, les exposer au code par le biais des prises de courant et les Actions et l’utilisation des contrôles AppKit en Code c#.

## <a name="related-links"></a>Liens associés

- [MacControls (exemple)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Fenêtres](~/mac/user-interface/window.md)
- [Liaison de données et codage de clé-valeur](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [À propos des contrôles et des vues](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
