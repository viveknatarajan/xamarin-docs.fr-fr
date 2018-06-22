---
title: Gestion de plusieurs résolutions dans CocosSharp
description: Ce guide montre comment travailler avec CocosSharp pour développer des jeux qui s’affiche correctement sur les appareils des résolutions différentes.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 577a3edbd106b6fba298b3ee5999265ef955f9dd
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33920817"
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Gestion de plusieurs résolutions dans CocosSharp

_Ce guide montre comment travailler avec CocosSharp pour développer des jeux qui s’affiche correctement sur les appareils des résolutions différentes._

CocosSharp fournit des méthodes pour standardiser les dimensions de l’objet dans votre jeu indépendamment du nombre physique de pixels sur l’affichage. En règle générale, les jeux développés pour les PC et consoles de jeu peut cibler une solution unique. Les jeux modernes – et en particulier les jeux pour les appareils mobiles – doivent être générés pour prendre en charge un large éventail de périphériques. Ce guide montre comment normaliser CocosSharp jeux indépendamment les caractéristiques de la résolution de l’affichage physique.

Le comportement de résolution par défaut de CocosSharp est à faire correspondre des pixels physiques avec les coordonnées dans le jeu. Le tableau suivant montre comment différents appareils sont d’entraîner un composant d’environnement en arrière-plan avec la largeur et la hauteur de 368 x 240. La première ligne est techniquement pas un appareil réel, mais au lieu de cela, le rendu attendu du sprite, quelle que soit la résolution de l’appareil :


| **Périphérique** | **Résolution d’affichage** | **Capture d’écran** |
|--- | --- |--- |
|Mise en forme|368 x 240 (avec noires pour proportions)| ![368 x 240 (avec noires pour proportions)](resolutions-images/image1.png) |
|iPhone 4 s|960 x 640| ![iPhone 4 s 960 x 640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920 x 1080 pixels](resolutions-images/image3.png) |

Ce document explique comment utiliser CocosSharp pour résoudre le problème indiqué dans le tableau ci-dessus. Autrement dit, nous aborderons comment rendre n’importe quel appareil comme indiqué dans la première ligne, quelle que soit la résolution d’écran de rendu.


## <a name="working-with-setdesignresolutionsize"></a>Utilisation de SetDesignResolutionSize

Le `CCScene` classe est généralement utilisée en tant que le conteneur racine pour tous les objets visuels, mais il fournit également une méthode statique nommée `SetDesignResolutionSize` pour spécifier la taille par défaut pour toutes les séquences. En d’autres termes `SetDesignResolutionSize` méthode permet aux développeurs de créer des jeux qui seront affichent correctement sur plusieurs résolutions de matériel. Les modèles de projet CocosSharp utilisent cette méthode pour définir la taille par défaut du projet à 1024 x 768, comme indiqué dans le code suivant :


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

Vous pouvez modifier le `desiredWidth` et `desiredHeight` ci-dessus pour modifier l’affichage pour correspondre à la résolution de votre choix de votre jeu de variables. Par exemple, le code ci-dessus peut être modifié comme suit pour afficher un arrière-plan 368 x 240 plein écran pour :


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` permet de spécifier la manière dont la fenêtre de jeu ajuste la résolution souhaitée. Les sections suivantes expliquent comment une image de 500 x 500 est affichée avec différents `CCSceneResolutonPolicy` valeurs passées à la `SetDesignResolutionSize` (méthode). Les valeurs suivantes sont fournies par le `CCSceneResolutionPolicy` enum :

 - `ShowAll` : Affiche la résolution demandée entière, en conservant les proportions.
 - `ExactFit` : Affiche la résolution de l’ensemble demandée, mais ne conserve pas les proportions.
 - `FixedWidth` : Affiche la largeur demandée entière, en conservant les proportions.
 - `FixedHeight` : Affiche la hauteur demandée entière, en conservant les proportions.
 - `NoBorder` : Affiche la hauteur entière ou la totalité de la largeur, en conservant les proportions. Si les proportions de l’appareil est supérieure aux proportions de la résolution de votre choix, toute la largeur est indiquée. Si les proportions de l’appareil est inférieure aux proportions de la résolution de votre choix, toute la hauteur est indiquée.
 - `Custom` – L’affichage dépend du `Viewport` propriété actuelles `CCScene`.

Captures d’écran de tous les produits résolutions iPhone 4 s (960 x 640) en mode paysage et utilisent cette image :

![](resolutions-images/image4.png "Captures d’écran de tous les produits à résolution iPhone 4 s 960 x 640 en mode paysage et utilisent cette image")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` Spécifie que la résolution du jeu entier sera visible à l’écran, mais peut afficher *cadre* (noires) pour ajuster les proportions différentes. Cette stratégie est généralement utilisée comme elle garantit que l’affichage de jeu entier sera affiché à l’écran sans aucune distorsion.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Cadre est visible à gauche et à droite de l’image pour prendre en compte les proportions physique qui est plus large que la résolution de votre choix :

![](resolutions-images/image5.png "Cadre est visible à gauche et à droite de l’image pour prendre en compte les proportions physique qui est plus large que la résolution de votre choix")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` Spécifie que la résolution du jeu entier sera visible à l’écran avec aucun cadre. La zone affichable sont déformée (proportions ne peut pas être maintenue) en fonction des proportions de matériel.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Aucun cadre n’est visible, mais étant donné que la résolution de l’appareil est rectangulaire la vue de jeu est déformée :

![](resolutions-images/image6.png "Aucun cadre n’est visible, mais étant donné que la résolution de l’appareil est rectangulaire la vue de jeu est déformée")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Spécifie que la largeur de la vue correspondra à la valeur de largeur passée à `SetDesignResolutionSize`, mais la hauteur visible est soumis aux proportions de l’unité physique. La valeur de hauteur passée à `SetDesignResolutionSize` est ignorée, car elle sera calculée lors de l’exécution selon les proportions de l’unité physique. Cela signifie que la hauteur calculée peut être plus petite que la hauteur souhaitée (ce qui entraîne des parties de la vue de jeu qui est en dehors de l’écran) ou la hauteur calculée peut être supérieure à la hauteur souhaitée (ce qui se traduit en plus de la vue de jeu affichée). Étant donné que cela peut entraîner plus de la partie affichée, puis il peut s’afficher comme si cadre s’est produite ; Toutefois, l’espace supplémentaire sera pas nécessairement noir s’il n’importe quel objet visuel. 

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

IPhone 4 s a des proportions de 3:2, par conséquent, la hauteur calculée est environ 333 unités :

![](resolutions-images/image7.png "IPhone 4 s a des proportions de 3:2, par conséquent, la hauteur calculée est environ 333 unités")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Point de vue conceptuel, `FixedHeight` se comporte de la même façon `FixedWidth` – le jeu tient compte de la valeur de hauteur passée à `SetDesignResolutionSize,` mais calcule la largeur lors de l’exécution sur la base de la résolution physique. Comme indiqué ci-dessus, cela signifie que la largeur affichée est inférieure ou supérieure à la largeur souhaitée, ce qui entraîne une partie du jeu en cours hors tension écran ou plus du jeu apparaissent, respectivement.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Étant donné que la capture d’écran suivante a été créé avec l’application en cours d’exécution en mode paysage, la largeur calculée est supérieure à 500 – 750 spécifiquement. Cette stratégie conserve la valeur 0 X aligné à gauche, donc la résolution supplémentaire est visible sur le côté droit de l’écran.

![](resolutions-images/image8.png "Cette stratégie conserve la valeur 0 X aligné à gauche, donc la résolution supplémentaire est visible sur le côté droit de l’écran")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` tente d’afficher l’application avec aucun cadre tout en conservant les proportions d’origine (sans distorsion). Si les proportions de la résolution demandée correspond à des proportions de l’unité physique, aucun extrait ne se produit. Si les proportions ne correspondent pas, puis extrait se produit.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

La capture d’écran suivante affiche les parties supérieure et inférieure de l’affichage détouré, tandis que toutes les 500 pixels de la largeur d’affichage sont affichés :

![](resolutions-images/image9.png "Cette capture d’écran affiche les parties supérieure et inférieure de l’affichage détouré, tandis que toutes les 500 pixels de la largeur d’affichage sont affichés.")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` permet à chaque `CCScene` pour spécifier sa propre fenêtre d’affichage personnalisé par rapport à la résolution spécifiée dans `SetDesignResolutionSize`.

Définissent des scènes leurs `Viewport` propriété en construisant un `CCViewport`, qui à son tour est construit avec un `CCRect`. Pour plus d’informations sur `CCViewport` et `CCRect` voir le [CocosSharp Documentation de l’API](https://developer.xamarin.com/api/namespace/CocosSharp/). Dans le contexte de la création d’un `CCViewport` le `CCRect` spécifient des paramètres du constructeur (dans l’ordre) :

 - x – la quantité de décalage horizontal de la fenêtre d’affichage, où chaque unité représente une largeur de la totalité de l’écran. Par exemple, utilisant une valeur de .5f résultats de la scène décalée vers la droite de la moitié de la largeur d’écran.
 - y : quantité de décalage vertical de la fenêtre d’affichage, où chaque unité représente une hauteur de l’écran entier. Par exemple, utilisant une valeur de .5f résultats de la scène décalée vers le bas de la moitié de la hauteur de l’écran.
 - largeur – la portion horizontale de la scène doit occuper. Par exemple, à l’aide d’une valeur de 1 / version 3.0f entraîne la scène horizontalement occupée par un tiers de l’écran.
 - hauteur – la portion verticale de la scène doit occuper. Par exemple, à l’aide d’une valeur de 1 / version 3.0f entraîne la scène verticalement occupée par un tiers de l’écran.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

Le code ci-dessus génère les éléments suivants :

![](resolutions-images/image10.png "Le code ci-dessus génère cette capture d’écran")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

Le `DefaultTexelToContentSizeRatio` simplifie l’utilisation de textures de résolution plus élevée sur les appareils avec des écrans haute résolution. Plus précisément, cette propriété permet de jeux pour utiliser des ressources de résolution plus élevée sans avoir besoin de modifier la taille ou le positionnement des éléments visuels. 

Par exemple, un jeu peut inclure une ressource de bibliothèque pour un jeu de caractère qui est de 200 pixels de hauteur et l’écran de jeu (tel que spécifié par `SetDesignResolutionSize`) peut être 400 pixels en hauteur. Dans ce cas, le caractère occupe la moitié de l’écran. Toutefois, si une ressource de pixel-taller 400 ont été utilisés pour le caractère pour les appareils haute résolution, le caractère occuperait toute la hauteur de l’affichage. Si l’objectif est de conserver le caractère de la même taille pour tirer parti des périphériques de résolution plus élevées, du code supplémentaire est nécessaire pour maintenir le sprite de caractère à la moitié de la hauteur de l’écran.

L’exemple simple fourni ci-dessus représente un problème courant dans le développement de jeux – Ajout d’éléments multimédias de grande taille sans obliger le développeur à effectuer le redimensionnement sur chaque élément visuel en fonction de la résolution de l’appareil. `DefaultTexelToContentSizeRatio` une propriété globale est utilisée pour le redimensionnement d’éléments visuels. Cette valeur redimensionne tous les éléments visuels d’un certain type par la valeur assignée.

Cette propriété n’est présente dans les classes suivantes : 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp Visual Studio pour l’ensemble de modèles Mac `CCSprite.DefaultTexelToContentSizeRatio` en fonction de la largeur de l’appareil en tant qu’un exemple de la façon dont il peut être utilisé. Le code suivant est contenu dans `CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```


### <a name="defaulttexeltocontentsizeratio-example"></a>Exemple de DefaultTexelToContentSizeRatio

Pour voir comment `DefaultTexelToContentSizeRatio` a un impact sur la taille d’élément visuel des éléments, considérez le code présenté ci-dessus :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Ce qui entraînera l’image suivante à l’aide d’une texture de 500 x 500 :

![](resolutions-images/image5.png "Ce qui produit cette image à l’aide d’une texture de 500 x 500")

IPad rétine s’exécute avec une résolution physique de 2 048 x 1 536. Cela signifie que le jeu comme indiqué ci-dessus serait stretch 500 pixels sur les pixels physiques 1536. Jeux peuvent tirer parti de cette résolution en créant ce qui sont généralement appelé *hd* actifs – actifs qui sont conçus pour s’exécuter sur les écrans haute résolution. Par exemple, ce jeu peut utiliser une version hd de cette texture dimensionné à 1 000 x 1 000. Toutefois, à l’aide de l’image plus grande entraînerait la `CCSprite` ayant une largeur et une hauteur de 1 000 unités. Étant donné que notre jeu affiche toujours les 500 unités (en raison du `SetDesignResolutioSize` appeler), notre sprite 1000 x 1000 est trop grande (affiche uniquement la partie inférieure gauche de celui-ci) :

![](resolutions-images/image11.png "Étant donné que le jeu affiche toujours 500 unités en raison de l’appel de SetDesignResolutioSize, sprite 1000 x 1000 serait trop grand qu'uniquement la partie inférieure gauche de celui-ci affiche")

Nous pouvons définir `CCSprite.DefaultTexelToContentSizeRatio` pour prendre en compte la texture de 1000 x 1000 en cours à deux reprises comme largeur et la hauteur de la texture de 500 x 500 d’origine :


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Maintenant si nous exécutons la partie la texture 1000 x 1000 être complètement visible :

![](resolutions-images/image12.png "Si nous exécutons la partie la texture 1000 x 1000 sont désormais complètement visible")


### <a name="defaulttexeltocontentsizeratio-details"></a>Détails de DefaultTexelToContentSizeRatio

Le `DefaultTexelToContentSizeRatio` propriété `static,` ce qui signifie que les composants ensemble de l’application partagent la même valeur. L’approche habituelle pour les jeux de ressources pour différentes résolutions doit contenir un ensemble complet de composants pour chaque catégorie de résolution. CocosSharp Visual Studio pour les modèles de Mac par défaut fournissent **%ld** et **hd** dossiers pour les ressources qui pourraient être utiles pour les jeux de prise en charge deux jeux de textures. Un exemple de dossier contenu avec le contenu peut ressembler à :

![](resolutions-images/image13.png "Un exemple de dossier contenu avec le contenu peut avoir l’aspect")

Notez que le modèle par défaut inclut également le code à ajouter à l’application de façon conditionnelle `ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```

Cela signifie que l’application doit rechercher les fichiers dans les chemins d’accès en fonction de si elle s’exécute en haute résolution ou le mode de résolution régulière. Par exemple, si le **hd** et **%ld** dossiers contiennent un fichier appelé **background.png** le code suivant serait exécuter, puis sélectionnez le fichier approprié pour la résolution :


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>Récapitulatif

Cet article explique comment créer des jeux qui s’affiche correctement, quelle que soit la résolution de l’appareil. Il montre des exemples d’utilisation de différents `CCSceneResolutionPolicy` valeurs pour le redimensionnement du jeu en fonction de la résolution de l’appareil. Il fournit également un exemple de procédure `DefaultTexelToContentSizeRatio` peut être utilisé pour prendre en charge de plusieurs ensembles de contenu sans nécessiter d’éléments visuels doit être redimensionné individuellement.

## <a name="related-links"></a>Liens connexes

- [Présentation de CocosSharp](~/graphics-games/cocossharp/index.md)
- [Documentation de l’API de CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
