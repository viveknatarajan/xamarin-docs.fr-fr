---
title: Affichage d’une Image
description: Cet article couvre notamment une ressource d’image dans une application Xamarin.iOS et affichage de cette image à l’aide de code c# ou en l’assignant à un contrôle dans le concepteur iOS.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8afc15a75b6d34d9b3f094fe2c92f2b6623ee63f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="displaying-an-image"></a>Affichage d’une Image

_Cet article couvre notamment une ressource d’image dans une application Xamarin.iOS et affichage de cette image à l’aide de code c# ou en l’assignant à un contrôle dans le concepteur iOS._

Cet article aborde les rubriques suivantes en détail :

- [Ajout et organisation des Images dans une application Xamarin.iOS](#adding-assets) - couvre les composants de l’image et comment ils peuvent être inclus, organisées et gérées à l’intérieur d’un projet Xamarin.iOS.
- [Ajout d’Images pour les catalogues Asset](#asset-catalogs) -gestion des images avec des catalogues d’élément multimédia.
    - [À l’aide des Images vectorielles dans des catalogues Asset](#Using-Vector-Images-in-Asset-Catalogs) -toutes les tailles d’image offrant un vecteur unique.
- [Utilisation des Images de modèle](#Working-with-Template-Images) -en définissant une ressource Image à une Image de modèle, le développeur peut facilement Coloriser pour correspondre à des modifications apportées au thème de l’interface utilisateur d’une application en définissant l’image `Tint` propriété.
- [L’utilisation d’Images avec des contrôles](#controls) - traite de l’utilisation des ressources d’image inclus dans un projet de Xamarin.iOS avec les contrôles d’interface utilisateur tels que `UIButton` et `UIImageView` et comment travailler avec des images en c# à l’aide de la `UIImage` l’objet [FromBundle](#frombundle) (méthode).
- [Affichage d’une Image dans un Storyboards](#Displaying-an-Image-in-a-Storyboards) -fournit un exemple d’affichage d’une image à l’aide d’un plan conceptuel.
- [Affichage d’une Image dans le Code](#Displaying-an-Image-in-Code) -fournit un exemple d’affichage d’une image à l’aide de code c#.

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Ajout et l’organisation des Images dans une application Xamarin.iOS

Lorsque vous ajoutez une image à utiliser dans une application Xamarin.iOS, le développeur utilise une _catalogue_ pour prendre en charge de chaque appareil iOS et la résolution requise par une application.

Ajouté dans iOS 7, **ensembles d’images catalogues Asset** contiennent toutes les versions ou les représentations sous forme d’une image qui est nécessaires pour prendre en charge de différents appareils et mettre à l’échelle des facteurs pour une application. Au lieu d’utiliser le nom de fichier de ressources image (consultez [Images indépendantes de la résolution et Image Nomenclature](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **ensembles d’images** utiliser un fichier Json pour spécifier l’image qui appartient à quel appareil et/ou de la résolution . Il s’agit de la meilleure façon de gérer et prendre en charge des images dans iOS (à partir d’iOS 9 ou supérieure).

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>Ajout d’Images à une Image du catalogue Asset défini

Comme indiqué précédemment, un **ensembles d’images catalogues Asset** contiennent toutes les versions ou les représentations sous forme d’une image qui est nécessaires pour prendre en charge de différents appareils et mettre à l’échelle des facteurs pour une application. Au lieu d’utiliser le nom du fichier de ressources d’image, **ensembles d’images** utiliser un fichier Json pour spécifier l’image qui appartient à quel appareil et/ou de la résolution.

Pour créer un nouvel ensemble de l’image et ajouter des images, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Assets.xcassets` fichier à ouvrir pour le modifier :

    ![](displaying-an-image-images/imageset01.png "Le Assets.xcassets dans l’Explorateur de solutions")
2. Avec le bouton droit sur le **liste de biens** et sélectionnez **nouveau jeu d’Image**:

    ![](displaying-an-image-images/imageset02.png "Ajout d’un nouvel ensemble de l’Image")
3. Sélectionnez l’ensemble d’images et l’éditeur s’affiche :

    ![](displaying-an-image-images/imageset03.png "L’éditeur d’ensemble de l’Image")
4. À ce stade, faites glisser dans des images pour chacun des différents appareils et et résolutions requis. (Remarque : ces résolutions correspondant aux résolutions spécifiées dans le [tailles d’Image et les noms de fichiers](~/ios/app-fundamentals/images-icons/displaying-an-image.md) document.)
5. Double-cliquez sur le nouveau jeu image **nom** dans les **liste de biens** pour le modifier : ![ ] (displaying-an-image-images/imageset04.png "modification nom de l’ensemble d’images nouveau")

Lorsque vous utilisez un **Image définie** dans le Concepteur d’iOS, il suffit de sélectionner le nom dans la liste déroulante dans l’éditeur de propriétés :

![](displaying-an-image-images/imageset06.png "Sélectionnez le nom de l’ensemble d’une image dans la liste déroulante")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ouvrir le catalogue à partir de la **l’Explorateur de solutions**, dans le coin supérieur gauche, cliquez sur le **Plus** bouton :

    ![](displaying-an-image-images/asset5.png "Cliquez sur le signe Plus bouton")
2. Sélectionnez **ajouter l’Image définie** et l’éditeur d’ensemble de l’Image s’affiche pour l’ensemble d’images. À ce stade, faites glisser dans des images pour chacun des différents appareils et et résolutions requis. (Remarque : ces résolutions correspondant aux résolutions spécifiées dans le [tailles d’Image et les noms de fichiers](~/ios/app-fundamentals/images-icons/displaying-an-image.md) document) :

    ![](displaying-an-image-images/asset7.png "Éditeur d’ensembles de l’image")

### <a name="renaming-an-image-set"></a>Modification du nom d’un ensemble d’images

Pour renommer un ensemble d’images, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le **catalogue** fichier à ouvrir pour le modifier :

    ![](displaying-an-image-images/rename01.png "Le catalogue dans l’Explorateur de solutions")
2. Sélectionnez le **Image définie** à renommer :

    ![](displaying-an-image-images/rename02.png "Sélectionnez l’ensemble de l’Image à renommer")
3. Dans le **l’Explorateur de propriétés**, faites défiler vers le bas et sélectionnez **nom**(sous la **divers** section) :

    ![](displaying-an-image-images/rename03.png "Sélectionnez le nom sous la section divers")
4. Entrez un nouveau **nom** pour le **Image définie** et enregistrez les modifications.

-----

Lorsque vous utilisez un **Image définie** dans le code, référencer par nom en appelant le `FromBundle` méthode de la `UIImage` classe. Par exemple :

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> Si les images assignées à un ensemble d’images sont affichent pas correctement, vérifiez que le nom de fichier est utilisée avec la `FromBundle` (méthode) (le **Image définie** et non le parent **catalogue** nom). Pour les images PNG, le `.png` extension peut être omise. Pour les autres formats d’image, l’extension est requise (par exemple). `PurpleMonkey.jpg`).

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>À l’aide des Images vectorielles dans des catalogues Asset

À partir d’iOS 8, spéciaux **vecteur** classe qu’ajoutée au **ensembles d’images** qui permet au développeur d’inclure un **PDF** au format image vectorielle dans les cassettes notamment à la place fichiers bitmap individuelles pour les différentes résolutions. À l’aide de cette méthode, fournissez un fichier de vecteur unique pour le `@1x` résolution (au format de fichier PDF vecteur) et le `@2x` et `@3x` versions du fichier seront générées au moment de la compilation et incluses dans le groupe de l’application.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "Images vectorielles dans l’éditeur de ressources catalogues")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "Images vectorielles dans l’éditeur de ressources catalogues")

-----

Par exemple, si le développeur inclut un `MonkeyIcon.pdf` fichier que le vecteur d’un catalogue avec une résolution de 150 px x 150px, la bitmap suivante actifs sont inclus dans l’offre groupée final lorsqu’il a été compilé avec :

- `MonkeyIcon@1x.png` -150 px x 150px résolution.
- `MonkeyIcon@2x.png` -300px x 300px résolution.
- `MonkeyIcon@3x.png` -à 450 px x 450px résolution.

Les éléments suivants doivent être prises en considération lors de l’utilisation d’images vectorielles PDF dans des catalogues Asset :

- N’est pas prise en charge complète de vecteur et le fichier PDF seront rastérisée à une image bitmap au moment de la compilation et les bitmaps inclus dans l’application finale.
- La taille de l’image ne peuvent pas être ajustée une fois qu’elle a été définie dans le catalogue. Si le développeur tentera de redimensionner l’image (soit dans le code ou à l’aide de la disposition automatique et les Classes de taille), l’image sera formée comme n’importe quel autre image bitmap.
- Les catalogues Asset sont uniquement compatibles avec iOS 7 et versions ultérieures, si une application a besoin prendre en charge d’iOS 6 ou inférieur, il ne peut pas utiliser les catalogues Asset.

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>Utilisation des Images de modèle

En fonction de la conception d’une application iOS, il peut arriver lorsque le développeur doit personnaliser une icône ou une image à l’intérieur de l’Interface utilisateur pour correspondre à une modification dans le jeu de couleurs (par exemple, en fonction des préférences de l’utilisateur).

Pour parvenir facilement à cet effet, basculez le _Mode de rendu_ de la ressource d’Image pour **Image de modèle**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "Le Mode de rendu à l’Image de modèle")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "Le Mode de rendu définie sur le modèle")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

Dans le Concepteur d’iOS, affecter la ressource Image à un contrôle d’interface utilisateur, puis définissez le **teinte** pour colorer l’image :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Définir la teinte à mettre en couleur de l’image")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Définir la teinte à mettre en couleur de l’image")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

Si vous le souhaitez, vous peuvent définir le composant de l’Image et la teinte directement dans le code :

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

Pour utiliser une Image de modèle complètement à partir du code, procédez comme suit :

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

Étant donné que la `RenderMode` propriété d’un `UIImage` est en lecture seule, utilisez la `ImageWithRenderingMode` méthode pour créer une nouvelle instance de l’image avec le paramètre de Mode de rendu souhaité.

Il existe trois éventuellement les paramètres pour `UIImage.RenderMode` via la `UIImageRenderingMode` enum :

* `AlwaysOriginal` -Force l’image à afficher en tant que le fichier image source d’origine sans aucune modification.
* `AlwaysTemplate` -Force l’image se présentent sous la forme d’une Image de modèle par la colorisation des pixels avec l’objet `Tint` couleur.
* `Automatic` -Soit restitue l’image en tant qu’un modèle ou origine sur laquelle elle est utilisée dans l’environnement de. Par exemple, si l’image est utilisée dans un `UIToolBar`, `UINavigationBar`, `UITabBar` ou `UISegmentControl` il sera traité comme un modèle.

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>Ajout de nouvelles Collections de ressources

Lorsque vous travaillez avec des images dans les catalogues de ressources il est possible que certaines quand une nouvelle collection sera nécessaire, au lieu d’ajouter toutes les images de l’application à la `Assets.xcassets` collection. Par exemple, lorsque vous concevez les ressources à la demande.

Pour ajouter un nouveau catalogue de ressources au projet :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Avec le bouton droit sur le **nom du projet** dans les **l’Explorateur de solutions** et sélectionnez **ajouter** > **nouveau fichier...**
2. Sélectionnez **iOS** > **catalogue**, entrez un **nom** pour la collection et cliquez sur le **nouveau** bouton :

    ![](displaying-an-image-images/asset01.png "Création d’un nouveau catalogue")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans l’Explorateur de solutions, cliquez sur **Asset catalogues** dossier, puis sélectionnez **Ajouter > Nouveau catalogue**.
2. Donnez-lui un nom et cliquez sur **ajouter**:

    ![](displaying-an-image-images/asset1.png "Création d’un nouveau catalogue")

-----


À ce stade, la collection peut être manipulée dans la même façon que la valeur par défaut `Assets.xcassets` collection automatiquement incluse dans le projet.

<a name="controls" />

## <a name="using-images-with-controls"></a>L’utilisation d’Images avec des contrôles

Outre l’utilisation d’images pour prendre en charge une application iOS utilise également des images avec des types de contrôle d’application telles que les barres de l’onglet, les barres d’outils, les barres de navigation, les tables et les boutons. Un moyen simple pour afficher une image sur un contrôle est d’assigner un `UIImage` instance du contrôle `Image` propriété.

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

Le `FromBundle` appel de méthode est un appel synchrone (bloquant) qui a plusieurs image lors du chargement et gestion des fonctionnalités intégrées, telles que la mise en cache de la prise en charge et la gestion automatique des fichiers image pour différentes résolutions.  

L’exemple suivant montre comment définir l’image d’un `UITabBarItem` sur une `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

En supposant que `MyImage` est le nom d’une ressource Image ajoutée à un catalogue ci-dessus. Lors de l’utilisation des images de catalogue, spécifiez simplement le nom de l’ensemble de l’Image dans le `FromBundle` méthode pour **PNG** mis en forme des images :

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

Pour tout autre format d’image, incluez l’extension avec le nom. Par exemple :

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

Pour plus d’informations sur les icônes et les images, consultez la documentation Apple sur [personnaliser les icônes et les instructions de création d’Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html).

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>Affichage d’une Image dans un Storyboards

Une fois qu’une image a été ajoutée à un projet de Xamarin.iOS à l’aide d’un élément multimédia de catalogues, il peut être facilement affichée sur une table de montage séquentiel à l’aide un `UIImageView` dans le concepteur iOS. Par exemple, si le composant de l’Image suivante a été ajoutée :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "Un exemple de que composant de l’Image a été ajoutée.")

Procédez comme suit pour l’afficher dans un plan conceptuel :

1. Double-cliquez sur le `Main.storyboard` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier dans le concepteur iOS.
2. Sélectionnez un **affichage de l’Image** à partir de la **boîte à outils**:

     ![](displaying-an-image-images/display02.png "Sélectionnez l’affichage d’une Image à partir de la boîte à outils")
3. Faites glisser de la vue de l’Image sur l’aire de conception et la position et la taille en fonction des besoins :

    ![](displaying-an-image-images/display03.png "Une nouvelle vue de l’Image sur l’aire de conception")
4. Dans le **Widget** section de la **Explorateur de propriétés** sélectionnez l’élément **Image** actif à afficher :

    ![](displaying-an-image-images/display04.png "Sélectionnez le composant souhaité de l’Image à afficher")
5. Dans le **vue** section, utilisez la **Mode** pour contrôler la façon dont l’image sera redimensionné quand la **vue de l’Image** est redimensionné.
6. Avec la **affichage Image** sélectionné, cliquez à nouveau pour ajouter **contraintes**:

    ![](displaying-an-image-images/display05.png "Ajout de contraintes")
7. Faites glisser la poignée de « T » en forme chaque bord de la **affichage Image** sur le côté correspondant de l’écran pour l’image sur les côtés « épingler ». De cette façon, le **affichage Image** sera réduire et augmente à mesure que l’écran est redimensionné.
8. Enregistrez les modifications dans la table de montage séquentiel.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "Un exemple de que composant de l’Image a été ajoutée.")

Procédez comme suit pour l’afficher dans un plan conceptuel :

1. Double-cliquez sur le `Main.storyboard` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier dans le concepteur iOS.
2. Sélectionnez un **affichage de l’Image** à partir de la **boîte à outils**:

     ![](displaying-an-image-images/display02vs.png "Sélectionnez l’affichage d’une Image à partir de la boîte à outils")
3. Faites glisser de la vue de l’Image sur l’aire de conception et la position et la taille en fonction des besoins :

    ![](displaying-an-image-images/display03vs.png "Une nouvelle vue de l’Image sur l’aire de conception")
4. Dans le **Widget** section de la **Explorateur de propriétés** sélectionnez l’élément **Image** actif à afficher :

    ![](displaying-an-image-images/display04vs.png "Sélectionnez le composant souhaité de l’Image à afficher")
5. Dans le **vue** section, utilisez la **Mode** pour contrôler la façon dont l’image sera redimensionné quand la **vue de l’Image** est redimensionné.
6. Avec la **affichage Image** sélectionné, cliquez à nouveau pour ajouter **contraintes**:

    ![](displaying-an-image-images/display05vs.png "Ajout de contraintes")
7. Faites glisser la poignée de « T » en forme chaque bord de la **affichage Image** sur le côté correspondant de l’écran pour l’image sur les côtés « épingler ». De cette façon, le **affichage Image** sera réduire et augmente à mesure que l’écran est redimensionné.
8. Enregistrez les modifications dans la table de montage séquentiel.

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>Affichage d’une Image dans le Code

Tout comme l’affichage d’une image dans un plan conceptuel, une fois qu’une image a été ajoutée à un projet de Xamarin.iOS à l’aide d’un élément multimédia des catalogues, il peut être facilement affiché à l’aide de code c#.

Prenons l’exemple suivant :

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

Ce code crée un nouveau `UIImageView` et lui donne une taille initiale et la position. Puis il charge l’image à partir d’un composant de l’Image ajoutée au projet et ajoute les `UIImageView` au parent `UIView` pour l’afficher.

## <a name="related-links"></a>Liens associés

- [Utilisation d’Images (exemple)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Icône personnalisée et les instructions de création d’Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
