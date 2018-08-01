---
title: Images dans Xamarin.Mac
description: Cet article décrit l’utilisation des images et des icônes dans une application Xamarin.Mac. Il décrit la création et la maintenance des images nécessaires pour créer l’icône de votre application et l’utilisation d’images dans le code c# et Interface Builder de Xcode.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: ae0a42051d56eb8bf002c61dfbc60c99924ff301
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792094"
---
# <a name="images-in-xamarinmac"></a>Images dans Xamarin.Mac

_Cet article décrit l’utilisation des images et des icônes dans une application Xamarin.Mac. Il décrit la création et la maintenance des images nécessaires pour créer l’icône de votre application et l’utilisation d’images dans le code c# et Interface Builder de Xcode._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même Image et icône des outils fournis par un développeur travaillant dans *Objective-C* et *Xcode* est.

Il existe plusieurs façons de cette image actifs sont utilisés à l’intérieur d’une application macOS (anciennement Mac OS X). À partir de simplement afficher une image dans le cadre de l’interface utilisateur de votre application, assigner à un contrôle d’interface utilisateur tel qu’une barre d’outils ou un élément de liste Source, à la fourniture des icônes, Xamarin.Mac facilite la tâche Ajouter des illustrations excellente à vos applications macOS comme suit : 

- **Éléments d’interface utilisateur** -Images peuvent être affichées comme arrière-plan ou dans le cadre de votre application dans une vue de l’Image (`NSImageView`).
- **Bouton** -Images peuvent être affichées dans les boutons (`NSButton`).
- **L’image de cellule** - dans le cadre d’un contrôle en fonction de table (`NSTableView` ou `NSOutlineView`), images peuvent être utilisées dans une cellule d’Image (`NSImageCell`).
- **Élément de barre d’outils** -Images peuvent être ajoutés à une barre d’outils (`NSToolbar`) comme un élément de barre d’outils d’Image (`NSToolbarItem`).
- **Icône de liste source** - comme partie d’une liste de Source (une mise en forme spéciale `NSOutlineView`).
- **Icône de l’application** -une série d’images peut être regroupée dans un `.icns` défini et utilisé en tant qu’icône de votre application. Consultez notre [icône de l’Application](~/mac/deploy-test/app-icon.md) documentation pour plus d’informations.

En outre, macOS fournit un ensemble d’images prédéfinies qui peuvent être utilisés dans votre application.

[![Un exemple d’exécution de l’application](image-images/intro01.png "un exemple d’exécution de l’application")](image-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation des Images et des icônes dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.


## <a name="adding-images-to-a-xamarinmac-project"></a>Ajout d’images à un projet Xamarin.Mac

Lorsque vous ajoutez une image à utiliser dans une application Xamarin.Mac, il existe plusieurs emplacements et les méthodes que le développeur peut inclure un fichier image à la source du projet :

- **Arborescence du projet principal [déconseillé]** -Images peuvent être ajoutés directement à l’arborescence de projets. Lorsque vous appelez des images stockées dans l’arborescence du projet principal à partir du code, aucun emplacement de dossier n’est spécifié. Par exemple : `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Dossier de ressources [déconseillé]** -spéciale **ressources** dossier est pour n’importe quel fichier qui feront partie de l’Application de l’offre groupée telles que l’icône, lancez l’écran ou général Images (ou un autre fichier ou image le développeur souhaite ajouter). Lors de l’appel des images stockées dans le **ressources** dossier à partir du code, stockées à l’instar des images dans l’arborescence du projet principal, aucun emplacement de dossier est spécifié. Par exemple : `NSImage.ImageNamed("tags.png")`.
- **Personnalisé dossier ou sous-dossier [déconseillé]** -le développeur peut ajouter un dossier personnalisé dans l’arborescence source de projets et de stocker les images il. L’emplacement où le fichier est ajouté peut être imbriquée dans un sous-dossier supplémentaire pour aider à organiser le projet. Par exemple, si le développeur ajouté un `Card` au projet et un sous-dossier du dossier de `Hearts` à ce dossier, puis stocker une image **Jack.png** dans le `Hearts` dossier `NSImage.ImageNamed("Card/Hearts/Jack.png")` charge l’image à Runtime.
- **Jeux d’Image de catalogue Asset [recommandé]** - ajouté dans OS X El Capitan **ensembles d’images catalogues Asset** contiennent toutes les versions ou les représentations sous forme d’une image qui sont nécessaires pour prendre en charge de différents appareils et à l’échelle de facteurs pour votre application. Au lieu d’utiliser le nom de fichier de ressources image (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Ajout d’images à une image du catalogue asset défini

Comme indiqué précédemment, un **ensembles d’images catalogues Asset** contiennent toutes les versions ou les représentations sous forme d’une image qui est nécessaires pour prendre en charge de différents appareils et mettre à l’échelle des facteurs de votre application. Au lieu d’utiliser le nom de fichier de ressources image (voir les Images d’indépendant de résolution et la Nomenclature Image ci-dessus), **ensembles d’images** l’éditeur de ressources permet de spécifier quelle image appartient à quel appareil et/ou de la résolution.

1. Dans le **Solution remplissage**, double-cliquez sur le **Assets.xcassets** fichier à ouvrir pour le modifier : 

    ![En sélectionnant le Assets.xcassets](image-images/imageset01.png "en sélectionnant le Assets.xcassets")
2. Avec le bouton droit sur le **liste de biens** et sélectionnez **nouveau jeu d’Image**: 

    [![Ajout d’un nouvel ensemble de l’image](image-images/imageset02.png "Ajout d’un nouvel ensemble de l’image")](image-images/imageset02-large.png#lightbox)
3. Sélectionnez l’ensemble d’images et l’éditeur s’affiche : 

    [![Sélection de l’ensemble d’images](image-images/imageset03.png "en sélectionnant l’ensemble d’images")](image-images/imageset03-large.png#lightbox)
4. À partir d’ici nous pouvons faire glisser dans des images pour chacun des différents appareils et des résolutions requis. 
5. Double-cliquez sur le nouveau jeu image **nom** dans les **liste de ressources** à modifier : 

    [![Modification de l’image de définir le nom](image-images/imageset04.png "modification de l’image de définir le nom")](image-images/imageset04-large.png#lightbox)
    
Une spéciale **vecteur** classe qu’ajoutée au **ensembles d’images** qui nous permet d’inclure un _PDF_ au format image vectorielle dans le casset au lieu de cela, y compris des fichiers bitmap individuels à résolutions différentes. À l’aide de cette méthode, vous fournissez un fichier de vecteur unique pour le **@1x** résolution (au format de fichier PDF vecteur) et le **@2x** et **@3x** versions du fichier seront générées au moment de la compilation et incluses dans le groupe de l’application.

[![L’image de définie l’interface de l’éditeur](image-images/imageset05.png "l’image de définie l’interface de l’éditeur")](image-images/imageset05-large.png#lightbox)

Par exemple, si vous incluez un `MonkeyIcon.pdf` fichier que le vecteur d’un catalogue avec une résolution de 150 px x 150px, la bitmap suivante actifs sont inclus dans l’offre groupée final lorsqu’il a été compilé avec :

1. **MonkeyIcon@1x.png** -150 px x 150px résolution.
2. **MonkeyIcon@2x.png** -300px x 300px résolution.
3. **MonkeyIcon@3x.png** -à 450 px x 450px résolution.

Les éléments suivants doivent être prises en considération lors de l’utilisation d’images vectorielles PDF dans des catalogues Asset :

- N’est pas prise en charge complète de vecteur et le fichier PDF seront rastérisée à une image bitmap au moment de la compilation et les bitmaps inclus dans l’application finale.
- Vous ne pouvez pas ajuster la taille de l’image une fois qu’elle a été définie dans le catalogue. Si vous essayez de redimensionner l’image (soit dans le code ou à l’aide de la disposition automatique et les Classes de taille) l’image sera formée comme n’importe quel autre image bitmap.

Lorsque vous utilisez un **Image définie** dans le Générateur de Xcode Interface, vous pouvez simplement sélectionner le nom dans la liste déroulante dans le **inspecteur de l’attribut**: **

![Sélection d’une image définie dans le Générateur de Xcode Interface](image-images/imageset06.png "en sélectionnant une image définie dans le Générateur de Xcode Interface")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Ajout de nouvelles collections de ressources

Lorsque vous travaillez avec des images dans les catalogues de ressources lorsque vous souhaitez créer une nouvelle collection, au lieu d’ajouter toutes vos images à peut être le **Assets.xcassets** collection. Par exemple, lorsque vous concevez les ressources à la demande.

Pour ajouter un nouveau catalogue de ressources à votre projet :

1. Avec le bouton droit sur le projet dans le **Solution remplissage** et sélectionnez **ajouter** > **nouveau fichier...**
2. Sélectionnez **Mac** > **catalogue**, entrez un **nom** pour la collection et cliquez sur le **nouveau** bouton : 

    ![Ajout d’un nouveau catalogue](image-images/asset01.png "Ajout d’un nouveau catalogue")

À ce stade, vous pouvez travailler avec la collection dans la même façon que la valeur par défaut **Assets.xcassets** collection automatiquement incluse dans le projet.


### <a name="adding-images-to-resources"></a>Ajout d’images à des ressources

> [!IMPORTANT]
> Cette méthode d’utilisation des images dans une application macOS a été déconseillée par Apple. Vous devez utiliser [Asset catalogues Image](#asset-catalogs) au Gestionnaire de votre application des images à la place.

Avant de pouvoir utiliser un fichier Image dans votre application Xamarin.Mac (soit dans le code c# ou à partir de l’Interface générateur) il doit être inclus dans le fichier **ressources** dossier comme un **groupe de ressources**. Pour ajouter un fichier à un projet, procédez comme suit :

1. Avec le bouton droit sur le **ressources** dossier dans votre projet dans le **Solution remplissage** et sélectionnez **ajouter** > **ajouter des fichiers...** : 

    ![Ajout d’un fichier](image-images/add01.png "Ajout d’un fichier")
2. À partir de la **ajouter des fichiers** boîte de dialogue, sélectionnez les images de fichiers à ajouter au projet, sélectionnez `BundleResource` pour le **action de génération de remplacement** et cliquez sur le **ouvrir** bouton :

    [![Sélection de fichiers à ajouter](image-images/add02.png "en sélectionnant les fichiers à ajouter")](image-images/add02-large.png#lightbox)
3. Si les fichiers ne sont pas déjà dans le **ressources** dossier, vous êtes invité si vous souhaitez **copie**, **déplacer** ou **lien** les fichiers. Lequel choisir chaque suites de vos besoins, en général, qui seront **copie**:

    ![En sélectionnant l’action Ajouter](image-images/add04.png "en sélectionnant l’action Ajouter")
4. Les nouveaux fichiers seront inclus dans le projet et lire à utiliser : 

    ![Les nouveaux fichiers image ajoutées à la zone Solution](image-images/add03.png "les nouveaux fichiers image ajoutées à la zone de la Solution")
5. Répétez le processus pour tous les fichiers image requis.

Vous pouvez utiliser n’importe quel format png, jpg ou fichier pdf comme une image source dans votre application Xamarin.Mac. Dans la section suivante, nous allons ajouter des versions haute résolution de nos Images et icônes pour prendre en charge rétine en fonction Mac.

> [!IMPORTANT]
> Si vous ajoutez des Images à le **ressources** dossier, vous pouvez laisser le **action de génération de remplacement** la valeur **par défaut**. Action de génération pour ce dossier de la valeur par défaut est `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Fournissez des versions haute résolution de toutes les ressources de graphiques d’application

N’importe quel élément de graphique que vous ajoutez à une application Xamarin.Mac (icônes, des contrôles personnalisés, des curseurs personnalisés, des graphiques personnalisés, etc.) doivent avoir des versions haute résolution en plus des versions de leurs résolution standard. Cela est nécessaire afin que votre application recherche son mieux lorsqu’il est exécuté sur un écran Retina équipé l’ordinateur Mac.


### <a name="adopt-the-2x-naming-convention"></a>Adopter les @2x convention d’affectation de noms

> [!IMPORTANT]
> Cette méthode d’utilisation des images dans une application macOS a été déconseillée par Apple. Vous devez utiliser [Asset catalogues Image](#asset-catalogs) au Gestionnaire de votre application des images à la place.

Lorsque vous créez les versions standard et haute résolution d’une image, suivent cette convention d’affectation de noms pour la paire d’images en les incluant dans votre projet Xamarin.Mac :

- **Résolution standard**  - **ImageName.filename-extension** (exemple : **tags.png**)
- **Haute résolution**   -  **ImageName@2x.filename-extension** (exemple : **tags@2x.png**)

Lors de l’ajout à un projet, ils sont affiche comme suit :

![Les fichiers image dans la zone Solution](image-images/add03.png "les fichiers image dans la zone de la Solution")

Lorsqu’une image est assignée à un élément d’interface utilisateur dans le Générateur de Interface vous allez simplement choisir le fichier dans le _ImageName_**.** _extension de nom de fichier_ format (exemple : **tags.png**). Le même pour l’utilisation d’une image dans le code c#, vous allez choisir le fichier dans le _ImageName_**.** _extension de nom de fichier_ format.

Lorsque vous Xamarin.Mac application êtes exécutée sur un Mac, le _ImageName_**.** _extension de nom de fichier_ image au format à utiliser dans les affichages de la résolution Standard, le **ImageName@2x.filename-extension** image sera automatiquement sélectionnée affichage Retina bases Mac.


## <a name="using-images-in-interface-builder"></a>L’utilisation d’images dans le constructeur d’Interface

N’importe quelle ressource image que vous avez ajouté à la **ressources** dossier dans votre Xamarin.Mac de projet et avez défini l’action de génération **BundleResource** apparaît automatiquement dans le Générateur de Interface et peut être sélectionné en tant que partie d’un élément d’interface utilisateur (si la gestion des images).

Pour utiliser une image dans le Générateur d’interface, procédez comme suit :

1. Ajouter une image à la **ressources** dossier avec un **Action de génération** de `BundleResource`: 

     ![Une ressource d’image dans la zone Solution](image-images/ib00.png "une ressource d’image dans la zone de la Solution")
2. Double-cliquez sur le **Main.storyboard** fichier à ouvrir pour le modifier dans le Générateur de Interface : 

     [![Modification de la table de montage séquentiel principal](image-images/ib01.png "modification le storyboard principal")](image-images/ib01-large.png#lightbox)
3. Faites glisser un élément d’interface utilisateur qui accepte des images sur l’aire de conception (par exemple, un **élément de barre d’outils Image**) : 

     ![Modification d’un élément de barre d’outils](image-images/ib02.png "modification d’un élément de barre d’outils")
4. Sélectionnez l’Image que vous avez ajouté à la **ressources** dossier dans le **nom de l’Image** liste déroulante : 

     [![Sélectionner une image pour un élément de barre d’outils](image-images/ib03.png "en sélectionnant une image pour un élément de barre d’outils")](image-images/ib03-large.png#lightbox)
5. L’image sélectionnée s’affichera dans l’aire de conception : 

     ![L’image s’affiche dans l’éditeur de la barre d’outils](image-images/ib04.png "l’image qui est affichée dans l’éditeur de la barre d’outils")
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Les étapes ci-dessus fonctionnent pour n’importe quel élément d’interface utilisateur qui permet de leur propriété de l’image à définir dans le **inspecteur de l’attribut**. Là encore, si vous avez inclus un **@2x** version de votre fichier image, il est automatiquement utilisé sur Affichage Retina basé Mac.

> [!IMPORTANT]
> Si l’Image n’est pas disponible dans le **nom de l’Image** liste déroulante, fermez votre projet .storyboard dans Xcode et rouvrez-le à partir de Visual Studio pour Mac. Si l’image n’est toujours pas disponible, vérifiez que son **Action de génération** est `BundleResource` et que l’image a été ajoutée à la **ressources** dossier.

## <a name="using-images-in-c-code"></a>L’utilisation d’images dans le code c#

Lors du chargement d’une image en mémoire à l’aide de code c# dans votre application Xamarin.Mac, l’image sera stockée dans un `NSImage` objet. Si le fichier image a été inclus dans l’offre groupée d’application Xamarin.Mac (incluse dans les ressources), utilisez le code suivant pour charger l’image :

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Le code ci-dessus utilise la méthode statique `ImageNamed("...")` méthode de la `NSImage` classe pour charger l’image donnée en mémoire à partir de la **ressources** dossier, si l’image ne peut pas être trouvée, `null` sera retourné. Comme les Images assignées dans le constructeur d’Interface, si vous avez inclus un **@2x** version de votre fichier image, il est automatiquement utilisé sur Affichage Retina basé Mac.

Pour charger des images en dehors de l’offre groupée de l’application (à partir du système de fichier Mac), utilisez le code suivant :

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Utilisation des images de modèle

En fonction de la conception de votre application macOS, il peut arriver lorsque vous devez personnaliser une icône ou une image à l’intérieur de l’Interface utilisateur pour correspondre à une modification dans le jeu de couleurs (par exemple, en fonction des préférences de l’utilisateur).

Pour parvenir à cet effet, basculez le _Mode de rendu_ de votre composant de l’Image à **Image de modèle**:

[![Définition d’une image de modèle](image-images/templateimage01.png "définition d’une image de modèle")](image-images/templateimage01-large.png#lightbox)

À partir Interface Builder de la Xcode, affecter la ressource Image à un contrôle de l’interface utilisateur :

![Sélection d’une image dans le Générateur de Xcode Interface](image-images/templateimage02.png "en sélectionnant une image dans le Générateur de Xcode Interface")

Ou, si vous le souhaitez définir la source de l’image dans le code :

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Ajoutez la fonction publique suivante à votre contrôleur de vue :

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

Enfin, pour colorer une Image de modèle, vous devez appeler cette fonction par rapport à l’image à mettre en couleur :

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>L’utilisation d’images avec des vues de table

Pour inclure une image dans le cadre de la cellule dans une `NSTableView`, vous devez modifier la façon dont les données sont retournées par la vue de Table `NSTableViewDelegate's` `GetViewForItem` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Il existe quelques lignes de présentant un intérêt ici. Tout d’abord, pour les colonnes que vous souhaitez inclure une image, nous créons un nouveau `NSImageView` de la taille requise et l’emplacement, nous avons également créer un nouveau `NSTextField` et placer sa position par défaut basée sur ou non, nous utilisons une image :

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Deuxièmement, nous devons inclure la nouvelle vue de l’Image et le même champ de texte de la page parente `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Enfin, nous devons à indiquer le champ de texte qu’elle peut réduire augmente avec la cellule du tableau :

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Exemple de sortie :

[![Un exemple d’affichage d’une image dans une application](image-images/tables01.png "un exemple d’affichage d’une image dans une application")](image-images/tables01-large.png#lightbox)

Pour plus d’informations sur l’utilisation des vues de Table, consultez notre [tableaux](~/mac/user-interface/table-view.md) documentation.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>Utilisation d’images avec le mode plan

Pour inclure une image dans le cadre de la cellule dans une `NSOutlineView`, vous devez modifier la façon dont les données sont retournées par la vue de structure du `NSTableViewDelegate's` `GetView` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Il existe quelques lignes de présentant un intérêt ici. Tout d’abord, pour les colonnes que vous souhaitez inclure une image, nous créons un nouveau `NSImageView` de la taille requise et l’emplacement, nous avons également créer un nouveau `NSTextField` et placer sa position par défaut basée sur ou non, nous utilisons une image :

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

Deuxièmement, nous devons inclure la nouvelle vue de l’Image et le même champ de texte de la page parente `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

Enfin, nous devons à indiquer le champ de texte qu’elle peut réduire augmente avec la cellule du tableau :

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Exemple de sortie :

[![Un exemple d’une image qui est affichée en mode plan](image-images/outline01.png "un exemple d’une image qui est affichée en mode plan")](image-images/outline01-large.png#lightbox)

Pour plus d’informations sur l’utilisation des vues de plan, consultez notre [mode plan](~/mac/user-interface/outline-view.md) documentation.


## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation des Images et des icônes dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des Images, l’utilisation des Images et des icônes dans le Générateur de Xcode Interface et l’utilisation des Images et des icônes dans le code c#.



## <a name="related-links"></a>Liens associés

- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vues de table](~/mac/user-interface/table-view.md)
- [Mode plan](~/mac/user-interface/outline-view.md)
- [macOS X indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [À propos de la haute résolution pour OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
