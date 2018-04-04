---
title: À l’aide de la MonoGame PipelineTool
description: L’outil de Pipeline MonoGame est utilisé pour créer et gérer des projets de contenu MonoGame. Les fichiers dans les projets de contenu sont traités par l’outil Monogame Pipeline et envoyées sous forme de fichiers .xnb pour une utilisation dans les applications CocosSharp et MonoGame.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 672c57aded149200b32501a6b48165ca88726ee1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>À l’aide de l’outil MonoGame Pipeline

_L’outil de Pipeline MonoGame est utilisé pour créer et gérer des projets de contenu MonoGame. Les fichiers dans les projets de contenu sont traités par l’outil Monogame Pipeline et envoyées sous forme de fichiers .xnb pour une utilisation dans les applications CocosSharp et MonoGame._

L’outil de Pipeline MonoGame fournit un environnement facile à utiliser pour la conversion des fichiers de contenu dans **.xnb** fichiers à utiliser dans les applications CocosSharp et MonoGame. Pour plus d’informations sur le contenu des pipelines et pourquoi ils sont utiles dans le développement de jeux, consultez [cette introduction sur les pipelines de contenu](~/graphics-games/cocossharp/content-pipeline/introduction.md)

Cette procédure pas à pas couvre les éléments suivants :

 - Installation de l’outil MonoGame Pipeline
 - Création d’un projet CocosSharp
 - Création d’un projet de contenu
 - Traitement des fichiers dans l’outil de Pipeline MonoGame
 - L’utilisation de fichiers lors de l’exécution

Cette procédure pas à pas utilise un projet CocosSharp pour illustrer comment **.xnb** fichiers peuvent être chargés et utilisés dans une application. Les utilisateurs de MonoGame sera également en mesure de faire référence à cette procédure pas à pas comme CocosSharp et MonoGame utilisent toutes deux le même **.xnb** fichiers de contenu.

Affiche un seul composant affichant une texture à partir de l’application terminée une **.xnb** une étiquette unique affichant une police sprite à partir de fichiers et d’un **.xnb** fichier :

![](walkthrough-images/image1.png "L’application finie affichera un seul composant affichant une texture à partir d’un fichier .xnb")


## <a name="monogame-pipeline-tool-discussion"></a>Présentation de l’outil de Pipeline MonoGame

L’outil de Pipeline MonoGame est disponible sur Windows, OS X et Linux. Cette procédure pas à pas s’exécute l’outil sur Windows, mais il peut être suivi le long sur Mac et Linux ainsi. Pour plus d’informations sur l’obtention de l’outil sur Max ou Linux, consultez [cette page](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

L’outil de Pipeline MonoGame est en mesure de créer du contenu pour les applications iOS même lorsque s’exécutant sur Windows, pour permettre aux développeurs utilisant [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) pourront continuer à développer sur Windows.


## <a name="installing-the-monogame-pipeline-tool"></a>Installation de l’outil MonoGame Pipeline

Nous allons commencer en installant le MonoGame, ce qui inclut le Pipeline de contenu MonoGame. Notez que le Pipeline de contenu MonoGame est disponible en téléchargement séparé pour Mac. Tous les programmes d’installation de MonoGame se trouvent sur le [MonoGame télécharge la page](http://www.monogame.net/downloads/). Nous allons télécharger MonoGame pour Visual Studio, mais une fois installé le développeur peut utiliser MonoGame dans Visual Studio pour Mac trop :

![](walkthrough-images/image2.png "Télécharger MonoGame pour Visual Studio, mais une fois installé trop l’utilisation de développeur MonoGame dans Visual Studio pour Mac")

Une fois téléchargé, nous exécuter via le programme d’installation et acceptez les options par défaut. Une fois le programme d’installation est terminée, l’outil de Pipeline MonoGame est installé et peut être trouvé dans la recherche du menu Démarrer :

![](walkthrough-images/image3.png "Une fois le programme d’installation est terminée, l’outil de Pipeline MonoGame est installé et peut être trouvé dans la recherche du menu Démarrer")

Lancez l’outil MonoGame Pipeline :

![](walkthrough-images/image4.png "Lancez l’outil MonoGame Pipeline")

Une fois que l’outil de Pipeline MonoGame est en cours d’exécution, nous pouvons commencer à rendre nos projets jeu et le contenu.


## <a name="creating-an-empty-cocossharp-project"></a>Création d’un projet CocosSharp vide

L’étape suivante consiste à créer un projet de CocosSharp. Il est important que nous tout d’abord créer le projet CocosSharp afin que nous pouvons enregistrer notre projet de contenu dans la structure de dossiers créée par le projet CocosSharp. Pour comprendre la structure d’un projet CocosSharp, examinons la [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), qui est à l’aide de ce guide. Toutefois, si vous avez un projet CocosSharp existant que vous souhaitez ajouter du contenu à, n’hésitez pas à utiliser ce projet à la place BouncingGame.

Une fois que le projet a été créé, nous exécuterons afin de vérifier qu’il génère et que nous que tout est correctement configuré :

![](walkthrough-images/image5.png "Une fois que le projet a été créé, exécutez pour vérifier qu’il se génère et que tous les éléments configurés correctement")


## <a name="creating-a-content-project"></a>Création d’un projet de contenu

Maintenant que nous avons un projet de jeu, nous pouvons créer un projet de Pipeline de MonoGame. Pour ce faire, dans la sélection de l’outil de Pipeline MonoGame **fichier > Nouveau...**  et accédez au dossier de contenu de votre projet. Pour Android, se trouve dans le dossier **[projet root]\BouncingGame.Android\Assets\Content\**. Pour iOS, le dossier se trouve dans **[projet root]\BouncingGame.iOS\Content\**.

Modifier la **nom de fichier** à **ContentProject** et cliquez sur le **enregistrer** bouton :

![](walkthrough-images/image8.png "Remplacez le nom de fichier ContentProject et cliquez sur le bouton Enregistrer")

Une fois que le projet est créé, l’outil de Pipeline MonoGame affiche des informations sur le projet lors de la racine **ContentProject** élément est sélectionné :

![](walkthrough-images/image9.png "Une fois que le projet est créé, l’outil de Pipeline MonoGame affiche des informations sur le projet lorsque l’élément de ContentProject racine est sélectionnée.")

Examinons quelques-unes des options plus importantes pour le projet de contenu.


### <a name="output-folder"></a>Dossier de sortie

C’est le dossier (relatif au projet de contenu lui-même) dans lequel la sortie **.xnb** fichiers seront enregistrés. Pour simplifier les choses, nous allons utiliser le même dossier afin de contenir notre entrée et de fichiers de sortie. En d’autres termes, nous allons modifier le **dossier de sortie** être **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Plateforme

Définit la plateforme cible pour le contenu. Notez que cela est **Windows** par défaut, par conséquent, nous vous souhaitez remplacer par notre plateforme cible qui est **Android** (ou iOS, si vous avez ainsi que d’un projet iOS).

![](walkthrough-images/image11.png "Notez qu’il s’agit Windows par défaut, donc cela sur la plateforme cible qui est Android ou iOS, si vous avez ainsi que d’un projet iOS")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>Traitement des fichiers dans l’outil de Pipeline MonoGame

Ensuite, nous allons ajouter de contenu à notre **ContentProject**. Pour ce projet, nous ajouterons les fichiers à la racine du projet, mais leur contenu dans les dossiers sont généralement organiser les projets de grande taille.

Nous allons ajouter deux fichiers à notre projet :

 - A **.png** fichier qui sera utilisé pour dessiner un composant. Ce fichier peut [téléchargé ici](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - A **.spritefont** fichier qui sera utilisé pour dessiner le texte sur l’écran. L’outil de Pipeline contenu prend en charge la création de fichiers .spritefont, donc il n’existe aucun fichier à télécharger.


### <a name="adding-a-png-file"></a>Ajout d’un fichier .png

Pour ajouter un **.png** fichier au projet, nous allons tout d’abord copier dans le même répertoire que le projet de pipeline, qui a le **.mgcb** extension.

![](walkthrough-images/image12.png "Ajouter un fichier .png au projet")

Ensuite, nous allons ajouter le fichier au projet de pipeline. Pour ce faire, dans l’outil de Pipeline MonoGame, sélectionnez **Modifier > Ajouter un élément en cours...** , sélectionnez le **ball.png** de fichier et cliquez sur **ouvrir**. Le fichier sera désormais partie du contenu du projet et, lorsque sélectionné, affiche ses propriétés :

![](walkthrough-images/image13.png "Le fichier sera désormais partie du contenu du projet et, lorsque sélectionné, affiche ses propriétés")

Nous allons en cours en laissant toutes les valeurs par défaut comme aucune modification n’est requise pour charger le fichier .xnb dans CocosSharp. Nous pouvons créer le fichier en sélectionnant le **Générer > Générer** option de menu, qui démarre une build et afficher la sortie de la build. Nous pouvons vérifier que la build a fonctionné correctement en vérifiant le **contenu** dossier pour un nouveau **ball.xnb** fichier :

![](walkthrough-images/image14.png "Vérifiez que la build a fonctionné correctement en vérifiant le dossier de contenu d’un nouveau fichier ball.xnb")


### <a name="adding-a-spritefont-file"></a>Ajout d’un fichier .spritefont

Nous pouvons créer le fichier .spritefont via l’outil de Pipeline MonoGame. CocosSharp nécessite des polices dans une **polices** dossier et les modèles CocosSharp automatiquement créer automatiquement un dossier de polices. Nous pouvons ajouter ce dossier à l’outil de Pipeline MonoGame en sélectionnant **Modifier > Ajouter > dossier existant...** . Accédez à la **contenu** dossier et sélectionnez le **polices** dossier puis cliquez sur **OK**:

![](walkthrough-images/browsetofonts.png "Accédez au dossier de contenu et sélectionnez le dossier polices, cliquez sur OK")

Pour ajouter un nouveau fichier .sprintefont, avec le bouton droit sur le dossier polices, puis sélectionnez **Ajouter > nouvel élément...** , sélectionnez le **SpriteFont Description** option, entrez le nom **arial-36**, puis cliquez sur **Ok**. CocosSharp nécessite très spécifique d’affectation de noms de fichiers de polices : ils doivent être au format [FontType]-[FontSize]. Si une police ne correspond pas à ce format d’affectation de noms, qu'il ne sera pas chargé par CocosSharp lors de l’exécution.

![](walkthrough-images/image15.png "Si une police ne correspond pas à ce format d’affectation de noms, qu'il ne sera pas chargé par CocosSharp lors de l’exécution")

Le fichier .spritefont est en fait un fichier XML qui peut être modifié dans un éditeur de texte, y compris Visual Studio pour Mac. Les variables courantes modifiées dans un fichier .spritefont sont les `FontName` et `Size` propriété :


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Nous allons ouvrir le fichier dans un éditeur de texte. En tant que notre **arial-36.spritefont** nom le suggère, nous laisserons le `FontName` en tant que `Arial` mais modifier le `Size` valeur `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>L’utilisation de fichiers lors de l’exécution

Les fichiers .xnb sont désormais intégrées et prêt à être utilisé dans notre projet. Nous allons ajouter les fichiers à Visual Studio pour Mac, puis nous allons ajouter le code à notre `GameScene.cs` fichier à charger ces fichiers et de les afficher.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Ajout de fichiers .xnb à Visual Studio pour Mac

Tout d’abord, nous allons ajouter les fichiers à votre projet. Dans Visual Studio pour Mac, nous allons développer le **BouncingGame.Android** de projet, développez le **actifs** dossier, avec le bouton droit sur le **contenu** dossier, puis sélectionnez **Ajouter > ajouter des fichiers...** Tout d’abord, nous allons sélectionner le **ball.xnb** nous précédemment créé et cliquez sur **ouvrir**. Puis répétez les étapes ci-dessus, mais ajouter la **arial-36.xnb** fichier. Nous allons sélectionner le **conserver le fichier dans son sous-répertoire actif** option si Visual Studio pour Mac vous demande comment ajouter le fichier. Une fois terminé, les deux fichiers doivent faire partie de notre projet :

![](walkthrough-images/image20.png "Une fois terminé, les deux fichiers doivent faire partie du projet")


### <a name="adding-gamescenecs"></a>Ajout de **GameScene.cs**

Nous allons créer une classe appelée `GameScene,` qui contiendra les objets sprite et le texte. Pour ce faire, cliquez sur le **BouncingGame** (pas BouncingGame.Android) du projet et sélectionnez **Ajouter > nouveau fichier...** . Sélectionnez le **général** catégorie, sélectionnez le **classe vide** option, puis entrez le nom **GameScene**.

Une fois créé, nous allons modifier le `GameScene.cs` fichier pour contenir le code suivant :


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

Nous ne décrivons le code ci-dessus, car le travail avec des objets visual CocosSharp comme CCSprite et CCLabelTtf est abordé dans le [BouncingGame guide](~/graphics-games/cocossharp/bouncing-game.md).

Nous devons également ajouter du code pour charger notre nouvellement créé `GameScene`. Pour cela, nous allons ouvrir le `GameAppDelegate.cs` fichier (qui se trouve dans le **BouncingGame** PCL) et de modifier le `ApplicationDidFinishLaunching` méthode afin d’obtenir :


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

Lors de l’exécution, notre jeu doit ressembler à :

![](walkthrough-images/image1.png "Lors de l’exécution, le jeu doit ressembler à")


## <a name="summary"></a>Récapitulatif

Cette procédure pas à pas a montré comment utiliser l’outil de Pipeline MonoGame pour créer des fichiers de .xnb à partir d’un fichier .png d’entrée, ainsi que la création d’un nouveau fichier .xnb à partir d’un fichier qui vient d’être créé de .sprintefont. Il décrit également la structure de projets CocosSharp pour utiliser les fichiers .xnb et comment charger ces fichiers lors de l’exécution.

## <a name="related-links"></a>Liens connexes

- [Téléchargements de MonoGame](http://www.monogame.net/downloads/)
- [Documentation du Pipeline de MonoGame](http://www.monogame.net/documentation/?page=Pipeline)
- [Projet de départ BouncingGame pour Android (exemple)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png graphique (exemple)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
