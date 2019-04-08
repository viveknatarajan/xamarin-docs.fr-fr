---
title: Images dans Xamarin.Mac
description: Cet article aborde l’utilisation des images et icônes dans une application Xamarin.Mac. Il décrit la création et la gestion des images nécessaires pour créer l’icône de votre application et l’utilisation d’images dans le code C# et Interface Builder de Xcode.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 719efc87b8843d0d2fcd2643aab23aa6849d940a
ms.sourcegitcommit: 190808013249005ceffbc798f9f4570e8cdc943a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54841378"
---
# <a name="images-in-xamarinmac"></a>Images dans Xamarin.Mac

_Cet article aborde l’utilisation des images et icônes dans une application Xamarin.Mac. Il décrit la création et la gestion des images nécessaires pour créer l’icône de votre application et l’utilisation d’images dans le code C# et Interface Builder de Xcode._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec C# et .NET dans une application Xamarin.Mac, vous avez accès à la même Image et l’icône des outils fournis par un développeur qui travaille *Objective-C* et *Xcode* est.

Il existe plusieurs façons de cette image actifs sont utilisés à l’intérieur d’une application macOS (anciennement Mac OS X). À partir de simplement afficher une image dans le cadre de l’interface utilisateur de votre application, affectez à un contrôle d’interface utilisateur comme une barre d’outils ou un élément de liste Source, à la fourniture des icônes, Xamarin.Mac permet de facilement ajouter des illustrations exceptionnelles à vos applications de macOS comme suit : 

- **Éléments d’interface utilisateur** -Images peuvent être affichées en tant qu’arrière-plan ou en tant que partie de votre application dans une vue de l’Image (`NSImageView`).
- **Bouton** -Images peuvent être affichées dans les boutons (`NSButton`).
- **Image de cellule** - dans le cadre d’un contrôle en fonction de table (`NSTableView` ou `NSOutlineView`), images peuvent être utilisées dans une cellule d’Image (`NSImageCell`).
- **Élément de barre d’outils** -Images peuvent être ajoutés à une barre d’outils (`NSToolbar`) en tant qu’un élément de barre d’outils Image (`NSToolbarItem`).
- **Icône de liste source** - dans le cadre d’une liste de Source (un spécialement mise en forme `NSOutlineView`).
- **Icône de l’application** -une série d’images peut être regroupée dans un `.icns` définie et utilisée comme icône de votre application. Consultez notre [icône d’Application](~/mac/deploy-test/app-icon.md) documentation pour plus d’informations.

En outre, macOS fournit un ensemble d’images prédéfinies qui peuvent être utilisées dans votre application.

[![Un exemple d’exécution de l’application](image-images/intro01.png "un exemple d’exécution de l’application")](image-images/intro01-large.png#lightbox)

Dans cet article, nous traiterons les notions de base de l’utilisation des Images et icônes dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous utilisons dans cet article.


## <a name="adding-images-to-a-xamarinmac-project"></a>Ajout d’images à un projet Xamarin.Mac

Lorsque vous ajoutez une image pour une utilisation dans une application Xamarin.Mac, il existe plusieurs emplacements et les méthodes que le développeur peut inclure un fichier image à la source du projet :

- **Arborescence du projet principal [déconseillé]** -Images peuvent être ajoutés directement à l’arborescence de projets. Lorsque vous appelez des images stockées dans l’arborescence du projet principal à partir du code, aucun emplacement de dossier est spécifié. Par exemple : `NSImage image = NSImage.ImageNamed("tags.png");`. 
- **Dossier de ressources [déconseillé]** -spéciale **ressources** dossier est pour n’importe quel fichier qui feront partie de l’Application de l’offre groupée telles que l’icône, écran de lancement générale ou d’Images (ou toute autre image ou un fichier au développeur souhaite ajouter). Lors de l’appel des images stockées dans le **ressources** dossier à partir du code, à l’instar des images stockées dans l’arborescence du projet principal, aucun emplacement de dossier est spécifié. Par exemple : `NSImage.ImageNamed("tags.png")`.
- **Personnalisées dossier ou sous-dossier [déconseillé]** -le développeur peut ajouter un dossier personnalisé à l’arborescence de source de projets et de stocker les images il. L’emplacement où le fichier est ajouté peut être imbriquée dans un sous-dossier davantage pour aider à organiser le projet. Par exemple, si le développeur ajouté un `Card` dossier au projet et un sous-dossier de `Hearts` à ce dossier, puis stocker une image **Jack.png** dans le `Hearts` dossier, `NSImage.ImageNamed("Card/Hearts/Jack.png")` chargerait l’image en Runtime.
- **Ensembles d’images de catalogue Asset [recommandé]** - ajouté dans OS X El Capitan, **ensembles d’images catalogues Asset** contiennent toutes les versions ou les représentations sous forme d’une image qui est nécessaires pour prendre en charge de divers périphériques et facteurs de mise à l’échelle votre application. Au lieu d’utiliser le nom de fichier de ressources image (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>Ajout d’images à une image de catalogue actif défini

Comme indiqué ci-dessus, un **ensembles d’images catalogues Asset** contiennent toutes les versions ou les représentations sous forme d’une image qui est nécessaires pour prendre en charge de divers périphériques et facteurs pour votre application d’échelle. Au lieu d’utiliser le nom de fichier de ressources image (voir la résolution des Images indépendantes et Nomenclature Image ci-dessus), **ensembles d’images** utiliser l’éditeur de ressource pour spécifier quelle image appartient à quels appareils et/ou la résolution.

1. Dans le **panneau solutions**, double-cliquez sur le **Assets.xcassets** fichier à ouvrir pour modification : 

    ![En sélectionnant le Assets.xcassets](image-images/imageset01.png "en sélectionnant le Assets.xcassets")
2. Avec le bouton droit sur le **liste Assets** et sélectionnez **nouveau jeu d’Image**: 

    [![Ajout d’un nouveau jeu d’images](image-images/imageset02.png "Ajout d’un nouveau jeu d’images")](image-images/imageset02-large.png#lightbox)
3. Sélectionnez le nouveau jeu d’images et l’éditeur s’affichera : 

    [![En sélectionnant le nouveau jeu d’images](image-images/imageset03.png "en sélectionnant le nouveau jeu d’images")](image-images/imageset03-large.png#lightbox)
4. À ce stade nous pouvons faire glisser dans des images pour chacun des différents appareils et résolutions nécessaires. 
5. Double-cliquez sur le nouvel ensemble image **nom** dans le **liste Assets** pour le modifier : 

    [![Modification de l’image de définir le nom](image-images/imageset04.png "édition de l’image de définir le nom")](image-images/imageset04-large.png#lightbox)
    
Un spécial **vecteur** classe qu’ajoutée au **ensembles d’images** qui nous permet d’inclure un _PDF_ mis en forme d’image vectorielle dans le casset au lieu de cela, y compris les fichiers bitmap individuels à les différentes résolutions. À l’aide de cette méthode, vous fournissez un fichier de vecteur unique pour le **@1x** résolution (mis en forme en tant que fichier PDF vecteur) et le **@2x** et **@3x** versions du fichier seront générées au moment de la compilation et incluses dans le bundle de l’application.

[![Définie l’interface de l’éditeur sur l’image](image-images/imageset05.png "l’image de définie l’interface de l’éditeur")](image-images/imageset05-large.png#lightbox)

Par exemple, si vous incluez un `MonkeyIcon.pdf` fichier en tant que le vecteur d’un catalogue de ressources avec une résolution de 150 px x 150px, la bitmap suivante actifs seraient inclus dans le bundle d’applications finale lorsqu’il a été compilé :

1. **MonkeyIcon@1x.png** -150 px x 150px résolution.
2. **MonkeyIcon@2x.png** -résolution x 300px de 300 pixels.
3. **MonkeyIcon@3x.png** -résolution de x 450px à 450 px.

Les éléments suivants doivent être prises en considération lors de l’utilisation d’images vectorielles PDF dans les catalogues de composants :

- N’est pas prise en charge complète de vecteur et le fichier PDF seront rastérisé vers une bitmap au moment de la compilation et les bitmaps fournies dans l’application finale.
- Vous ne pouvez pas ajuster la taille de l’image une fois qu’il a été défini dans le catalogue de ressources. Si vous essayez de redimensionner l’image (soit dans le code ou à l’aide de la disposition automatique et les Classes de taille) l’image est déformée comme toute autre image bitmap.

Lorsque vous utilisez un **Image définie** dans Xcode Interface Builder, vous pouvez simplement sélectionner le nom dans la liste déroulante dans le **inspecteur d’attributs**:

![Sélection d’une image défini dans l’Interface Builder de Xcode](image-images/imageset06.png "vous sélectionnez une image haute dans Interface Builder de Xcode")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>Ajout de nouvelles collections de ressources

Lorsque vous travaillez avec des images dans les catalogues de ressources il peut arriver lorsque vous souhaitez créer une nouvelle collection, au lieu d’ajouter toutes vos images à le **Assets.xcassets** collection. Par exemple, lors de la conception des ressources de la demande.

Pour ajouter un nouveau catalogue de ressources à votre projet :

1. Avec le bouton droit sur le projet dans le **panneau solutions** et sélectionnez **ajouter** > **nouveau fichier...**
2. Sélectionnez **Mac** > **catalogue**, entrez un **nom** pour la collection et cliquez sur le **New** bouton : 

    ![Ajout d’un nouveau catalogue d’actifs](image-images/asset01.png "Ajout d’un nouveau catalogue d’actifs")

À partir de là, vous pouvez travailler avec la collection dans la même façon que la valeur par défaut **Assets.xcassets** collection automatiquement incluse dans le projet.


### <a name="adding-images-to-resources"></a>Ajout d’images aux ressources

> [!IMPORTANT]
> Cette méthode d’utilisation des images dans une application macOS a été déconseillée par Apple. Vous devez utiliser [ensembles d’images catalogue Asset](#asset-catalogs) au Gestionnaire de votre application d’images à la place.

Avant de pouvoir utiliser un fichier Image dans votre application Xamarin.Mac (soit en code C# ou à partir de l’Interface Builder) il doit être inclus dans le projet **ressources** dossier comme un **groupe de ressources**. Pour ajouter un fichier à un projet, procédez comme suit :

1. Avec le bouton droit sur le **ressources** dossier dans votre projet dans le **panneau solutions** et sélectionnez **ajouter** > **ajouter des fichiers...** : 

    ![Ajout d’un fichier](image-images/add01.png "Ajout d’un fichier")
2. À partir de la **ajouter des fichiers** boîte de dialogue, sélectionnez les images de fichiers à ajouter au projet, sélectionnez `BundleResource` pour le **action de génération de remplacement** et cliquez sur le **Open** bouton :

    [![Sélectionner les fichiers à ajouter](image-images/add02.png "en sélectionnant les fichiers à ajouter")](image-images/add02-large.png#lightbox)
3. Si les fichiers ne sont pas déjà dans le **ressources** dossier, vous demandera si vous souhaitez **copie**, **déplacer** ou **lien** les fichiers. Laquelle choisir chaque couleurs vos besoins, en général, qui seront **copie**:

    ![En sélectionnant l’action Ajouter](image-images/add04.png "en sélectionnant Ajouter une action")
4. Les nouveaux fichiers seront inclus dans le projet et lire pour une utilisation : 

    ![Les nouveaux fichiers d’image ajoutées pour le panneau Solution](image-images/add03.png "les nouveaux fichiers d’image ajoutées pour le panneau Solution")
5. Répétez le processus pour tous les fichiers image requis.

Vous pouvez utiliser n’importe quel fichier pdf, jpg ou png comme une image source dans votre application Xamarin.Mac. Dans la section suivante, nous examinerons d’ajout des versions haute résolution de nos Images et icônes pour prendre en charge de la rétine les ordinateurs Mac.

> [!IMPORTANT]
> Si vous ajoutez des Images à le **ressources** dossier, vous pouvez laisser le **action de génération de remplacement** définie sur **par défaut**. La valeur par défaut Action de génération pour ce dossier est `BundleResource`.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>Fournir des versions haute résolution de toutes les ressources graphiques des applications

Les graphiques actifs que vous ajoutez à une application Xamarin.Mac (icônes, des contrôles personnalisés, des curseurs personnalisés, des graphiques personnalisés, etc.) devez disposer des versions haute résolution en plus de leurs versions de résolution standard. Cela est nécessaire afin que votre application se présentera de son mieux lors de l’exécution sur un écran Retina équipé d’ordinateur Mac.


### <a name="adopt-the-2x-naming-convention"></a>Adopter la @2x convention de dénomination

> [!IMPORTANT]
> Cette méthode d’utilisation des images dans une application macOS a été déconseillée par Apple. Vous devez utiliser [ensembles d’images catalogue Asset](#asset-catalogs) au Gestionnaire de votre application d’images à la place.

Lorsque vous créez les versions standards et haute résolution d’une image, suivez cette convention d’affectation de noms pour la paire d’images lorsque vous les incluez dans votre projet Xamarin.Mac :

- **Résolution standard**  - **ImageName.filename-extension** (exemple : **tags.png**)
- **Haute résolution**   -  **ImageName@2x.filename-extension** (exemple : **tags@2x.png**)

Lors de l’ajout à un projet, qu’ils apparaîtraient comme suit :

![Les fichiers image dans le panneau Solution](image-images/add03.png "les fichiers image dans le panneau Solution")

Lorsqu’une image est affectée à un élément d’interface utilisateur dans l’Interface Builder choisis simplement le fichier dans le _ImageName_**.** _nomfichier-extension_ format (exemple : **tags.png**). Identiques pour l’utilisation d’une image dans le code C#, vous allez choisir le fichier dans le _ImageName_**.** _nomfichier-extension_ format.

Lorsque votre application de Xamarin.Mac est exécuté sur un Mac, le _ImageName_**.** _nomfichier-extension_ image au format sera utilisé sur les écrans de résolution Standard, le **ImageName@2x.filename-extension** image sera automatiquement sélectionnée affichage Retina bases des ordinateurs Mac.


## <a name="using-images-in-interface-builder"></a>L’utilisation d’images dans Interface Builder

N’importe quelle ressource image que vous avez ajouté à la **ressources** dossier dans votre Xamarin.Mac de projet et ont la valeur est l’action de génération **BundleResource** apparaissent automatiquement dans le Générateur d’Interface et peut être sélectionné en tant que partie d’un élément d’interface utilisateur (si la gestion des images).

Pour utiliser une image dans le Générateur d’interface, procédez comme suit :

1. Ajouter une image à la **ressources** dossier avec un **Action de génération** de `BundleResource`: 

     ![Une ressource image dans le panneau Solution](image-images/ib00.png "une ressource image dans le panneau Solution")
2. Double-cliquez sur le **Main.storyboard** fichier à ouvrir pour modification dans l’Interface Builder : 

     [![Modification de la table de montage séquentiel principal](image-images/ib01.png "modification le storyboard principal")](image-images/ib01-large.png#lightbox)
3. Faites glisser un élément d’interface utilisateur qui accepte des images sur l’aire de conception (par exemple, un **élément de barre d’outils Image**) : 

     ![Modification d’un élément de barre d’outils](image-images/ib02.png "modification d’un élément de barre d’outils")
4. Sélectionnez l’Image que vous avez ajouté à la **ressources** dossier dans le **nom de l’Image** liste déroulante : 

     [![Sélection d’une image pour un élément de barre d’outils](image-images/ib03.png "sélection d’une image pour un élément de barre d’outils")](image-images/ib03-large.png#lightbox)
5. L’image sélectionnée s’affiche dans l’aire de conception : 

     ![L’image qui est affichée dans l’éditeur de la barre d’outils](image-images/ib04.png "l’image qui est affichée dans l’éditeur de la barre d’outils")
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac se synchroniser avec Xcode.

Les étapes ci-dessus fonctionnent pour n’importe quel élément d’interface utilisateur qui permet leur propriété de l’image à définir le **inspecteur d’attributs**. Là encore, si vous avez inclus un **@2x** version de votre fichier image, il sera automatiquement utilisée sur Affichage Retina en fonction des ordinateurs Mac.

> [!IMPORTANT]
> Si l’Image n’est pas disponible dans le **nom de l’Image** liste déroulante, fermez votre projet .storyboard dans Xcode et le rouvrir à partir de Visual Studio pour Mac. Si l’image n’est pas toujours disponible, vérifiez que son **Action de génération** est `BundleResource` et que l’image a été ajoutée à la **ressources** dossier.

## <a name="using-images-in-c-code"></a>L’utilisation d’images dans le code C#

Lors du chargement d’une image dans la mémoire à l’aide de code C# dans votre application Xamarin.Mac, l’image sera stockée dans un `NSImage` objet. Si le fichier image a été inclus dans le bundle d’applications Xamarin.Mac (inclus dans les ressources), utilisez le code suivant pour charger l’image :

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

Le code ci-dessus utilise la méthode statique `ImageNamed("...")` méthode de la `NSImage` classe pour charger l’image donnée en mémoire à partir de la **ressources** dossier, si l’image ne peut pas être trouvée, `null` sera retourné. Comme les Images assignées dans Interface Builder, si vous avez inclus un **@2x** version de votre fichier image, il sera automatiquement utilisée sur Affichage Retina en fonction des ordinateurs Mac.

Pour charger des images en dehors de l’offre groupée de l’application (à partir du système de fichiers Mac), utilisez le code suivant :

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>Utilisation des images de modèle

Selon la conception de votre application macOS, il peut arriver que vous deviez personnaliser une icône ou une image à l’intérieur de l’Interface utilisateur afin de refléter une modification dans le jeu de couleurs (par exemple, en fonction des préférences de l’utilisateur).

Pour obtenir cet effet, basculez le _Mode de rendu_ de votre élément multimédia Image **Image de modèle**:

[![Définition d’une image de modèle](image-images/templateimage01.png "définition d’une image de modèle")](image-images/templateimage01-large.png#lightbox)

À partir Interface Builder de Xcode, affecter la ressource Image à un contrôle d’interface utilisateur :

![Sélection d’une image dans l’Interface Builder de Xcode](image-images/templateimage02.png "sélection d’une image dans l’Interface Builder de Xcode")

Ou, si vous le souhaitez définir la source de l’image dans le code :

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

Ajoutez la fonction publique suivante à votre contrôleur d’affichage :

```csharp
public NSImage ImageTintedWithColor(NSImage sourceImage, NSColor tintColor)
    => NSImage.ImageWithSize(sourceImage.Size, false, rect => {
        // Draw the original source image
        sourceImage.DrawInRect(rect, CGRect.Empty, NSCompositingOperation.SourceOver, 1f);

        // Apply tint
        tintColor.Set();
        NSGraphics.RectFill(rect, NSCompositingOperation.SourceAtop);

        return true;
    });
```

> [!IMPORTANT]
> En particulier avec l’arrivée du Mode foncé dans macOS Mojave, il est important d’éviter le `LockFocus` API lors du rendu de personnalisé réation au `NSImage` objets. Ces images deviennent statiques et ne seront pas modifiés automatiquement au compte pour les modifications de densité apparence ou affichage.
>
> En utilisant le mécanisme basé sur le gestionnaire ci-dessus, nouveau rendu pour les conditions dynamiques se produit automatiquement lorsque le `NSImage` est hébergé, par exemple, dans un `NSImageView`.

Enfin, pour colorer une Image de modèle, vous devez appeler cette fonction par rapport à l’image à mettre en couleur :

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>L’utilisation d’images avec des vues de table

Pour inclure une image dans le cadre de la cellule dans un `NSTableView`, vous devrez modifier la façon dont les données sont retournées par la vue Table `NSTableViewDelegate's` `GetViewForItem` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

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

Il existe quelques lignes d’intérêt ici. Pour les colonnes que nous souhaitons inclure une image, nous commençons par créer un nouveau `NSImageView` de la taille requise et l’emplacement, nous créons également un nouveau `NSTextField` et placer sa position par défaut basée sur ou non, nous utilisons une image :

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

Enfin, nous devons indiquer le champ de texte qu’il peut réduire et augmenter avec la cellule d’affichage de Table :

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Exemple de sortie :

[![Un exemple d’affichage d’une image dans une application](image-images/tables01.png "un exemple d’affichage d’une image dans une application")](image-images/tables01-large.png#lightbox)

Pour plus d’informations sur l’utilisation des vues de Table, consultez notre [affichages tableau](~/mac/user-interface/table-view.md) documentation.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>L’utilisation d’images avec des modes plan

Pour inclure une image dans le cadre de la cellule dans un `NSOutlineView`, vous devrez modifier la façon dont les données sont retournées par la vue hiérarchique `NSTableViewDelegate's` `GetView` méthode à utiliser un `NSTableCellView` au lieu du type `NSTextField`. Exemple :

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

Il existe quelques lignes d’intérêt ici. Pour les colonnes que nous souhaitons inclure une image, nous commençons par créer un nouveau `NSImageView` de la taille requise et l’emplacement, nous créons également un nouveau `NSTextField` et placer sa position par défaut basée sur ou non, nous utilisons une image :

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

Enfin, nous devons indiquer le champ de texte qu’il peut réduire et augmenter avec la cellule d’affichage de Table :

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

Exemple de sortie :

[![Un exemple d’une image qui est affichée en mode plan](image-images/outline01.png "un exemple d’une image qui est affichée en mode plan")](image-images/outline01-large.png#lightbox)

Pour plus d’informations sur l’utilisation des modes plan, consultez notre [modes plan](~/mac/user-interface/outline-view.md) documentation.


## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation des Images et icônes dans une application Xamarin.Mac. Nous avons vu les différents types et utilise des Images, l’utilisation des Images et icônes dans Interface Builder de Xcode et comment travailler avec des Images et icônes dans le code C#.



## <a name="related-links"></a>Liens associés

- [MacImages (exemple)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vues de table](~/mac/user-interface/table-view.md)
- [Modes plan](~/mac/user-interface/outline-view.md)
- [Mac OS X Human Interface Guidelines pour](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [À propos de la haute résolution pour OS X](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
