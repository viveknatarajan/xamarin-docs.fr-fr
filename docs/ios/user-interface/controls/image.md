---
title: Afficher des Images
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 716189fbf1518e9100a78cc5ae64e9e63a24c949
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="displaying-images"></a>Afficher des Images

Ajout d’images à votre application passe en deux étapes : tout d’abord, ajoutez les images à votre projet ; Ensuite, ajoutez des contrôles et du code pour les afficher sur un écran. Reportez-vous à la [utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) article pour plus de couverture d’image de la gestion dans Xamarin.iOS.

## <a name="adding-images-to-your-app"></a>Ajout d’Images à votre application

Images peuvent être ajoutées à n’importe quel dossier dans votre Visual Studio pour la solution de Mac et si le **Action de génération** a la valeur **contenu** puis le fichier sera inclus dans votre application et peut être affiché.

Visual Studio pour Mac prend également en charge un répertoire spécial appelé des ressources qui peuvent également contenir des fichiers image. Les fichiers dans le dossier de ressources doivent avoir le **Action de génération** la valeur **BundleResource**.

Cette capture d’écran montre le **Action de génération** s’affichent quand les options qui s’affichent lorsqu’un fichier :

 [![](image-images/image30a.png "Menu Action de génération")](image-images/image30a.png#lightbox)

Visual Studio pour Mac allez généralement choisir la bonne **Action de génération** automatiquement, mais vous devez être conscient de ces paramètres, en particulier si vous déplacez des fichiers dans votre projet.

### <a name="adding-an-image-file"></a>Ajout d’un fichier Image

Pour ajouter un fichier image à votre projet, tout d’abord droit sur le projet et choisissez **ajouter des fichiers...**

 [![](image-images/image31a.png "Ajouter des fichiers... menu")](image-images/image31a.png#lightbox)

Sélectionnez l’image (ou images) à inclure dans la boîte de dialogue standard. Action de génération par défaut pour les images seront **BundleResource** – ne pas remplacer cette valeur sauf si vous avez une raison spécifique.

 [![](image-images/image32a.png "Fichiers de boîte de dialogue Ajouter")](image-images/image32a.png#lightbox)

L’image sera ajoutée à votre projet et disponibles pour être chargés et affichés dans le code. Cette capture d’écran montre une image ajoutée à un projet d’application iOS :

 [![](image-images/image33a.png "Image dans un projet")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Qu’est le répertoire des ressources ?

Fichiers placés dans le répertoire des ressources sont traités différemment à partir de fichiers régulières : le contenu du dossier de ressources est copié à la racine de l’application et peut être référencé à partir de là, dans votre code. Cela peut être utile pour de nombreuses raisons :

-  Stocker les images configurés dans les propriétés de l’application, tels que les images de démarrage par défaut et les icônes de l’application.
-  Stockage autres images et des fichiers séparément à partir du code, par conséquent, ils sont plus faciles à gérer (les sous-répertoires sont conservés lorsque le contenu du répertoire ressources est copié).


Le répertoire des ressources est particulièrement utile dans un projet de bibliothèque, étant donné que le code peut supposer que les images sont copiées dans la racine de l’application consommatrice, rendant ainsi plus facile d’écrire des bibliothèques de code partagé qui nécessitent une image, audio, vidéo, XML ou autre fichiers.



Le répertoire des ressources doit être nommé, et tous les fichiers doivent avoir l’Action de génération la valeur **BundleResource**

## <a name="displaying-the-image"></a>Affichage de l’Image

Pour afficher une image à l’aide du concepteur, une vue de l’Image doit être utilisée comme un conteneur et peuvent afficher une image ou une animation d’images. Le **affichage Image** icône à partir de la boîte à outils est indiqué ci-dessous :

 [![](image-images/image35a.png "ImageView dans la boîte à outils")](image-images/image35.png#lightbox)

Faites glisser le **affichage de l’Image** à partir de la **Toobox** sur le contrôleur de la vue. Ensuite, sous ** vue de l’Image > Image ** la liste déroulante fournit une liste de tous les fichiers image disponible dans votre projet. Sélectionnez une de ces pour l’ajouter à votre vue de l’image.

 [![](image-images/image36a.png "ImageView dans la boîte à outils")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Affichage de l’image par programmation

Étant donné que blocks.jpg se trouve dans la racine du répertoire de ressources, il sera disponible lors de l’exécution dans la racine de l’application application. Pour afficher cette image dans un ImageView contrôle utiliser le code suivant :

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

Si nous avions placé l’image dans `/Resources/Pics/blocks.jpg` puis le code inclut le dossier Pics dans le chemin d’accès :

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

Fichier de ressources fait référence à jamais besoin d’inclure le `Resources` dossier.


## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
