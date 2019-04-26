---
title: Gestion de résolutions multiples dans CocosSharp
description: Ce guide montre comment travailler avec CocosSharp pour développer des jeux qui s’affiche correctement sur les périphériques de résolution différente.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 6803dc2668b89ee2d037da8b34e202191dd5465d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307745"
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Gestion de résolutions multiples dans CocosSharp

_Ce guide montre comment travailler avec CocosSharp pour développer des jeux qui s’affiche correctement sur les périphériques de résolution différente._

CocosSharp fournit des méthodes pour standardiser les dimensions de l’objet dans votre jeu, quel que soit le numéro de pixels sur l’affichage d’un périphérique physique. En règle générale, les jeux développés pour les PC et consoles de jeu peut cibler une résolution unique. Les jeux modernes – et en particulier les jeux pour les appareils mobiles – doivent être générés pour prendre en charge un large éventail d’appareils. Ce guide montre comment standardiser CocosSharp jeux, quel que soit les caractéristiques de la résolution de l’affichage physique.

Le comportement de résolution par défaut de CocosSharp consiste à faire correspondre des pixels physiques avec les coordonnées dans le jeu. Le tableau suivant montre comment divers appareils affichait un sprite d’environnement en arrière-plan avec la largeur et hauteur de 368 x 240. La première ligne est techniquement pas un appareil réel, mais plutôt le rendu attendu du sprite, quelle que soit la résolution de l’appareil :


| **Appareil** | **Résolution d’affichage** | **Capture d’écran de l’exemple** |
|--- | --- |--- |
|Affichage souhaité|368 x 240 (avec des barres noires pour le rapport hauteur / largeur)| ![368 x 240 (avec des barres noires pour le rapport hauteur / largeur)](resolutions-images/image1.png) |
|iPhone 4s|960 x 640| ![iPhone 4 s 960 x 640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920x1080](resolutions-images/image3.png) |

Ce document décrit l’utilisation de CocosSharp pour résoudre le problème indiqué dans le tableau ci-dessus. Autrement dit, nous allons aborder le s’afficher comme indiqué dans la première ligne, quelle que soit la résolution d’écran de n’importe quel appareil.


## <a name="working-with-setdesignresolutionsize"></a>Utilisation de SetDesignResolutionSize

Le `CCScene` classe est généralement utilisée en tant que le conteneur racine pour tous les objets visuels, mais il fournit également une méthode statique nommée `SetDesignResolutionSize` pour spécifier la taille par défaut pour toutes les séquences. En d’autres termes `SetDesignResolutionSize` méthode permet aux développeurs développer des jeux qui seront affiche correctement sur plusieurs résolutions de matériel. Les modèles de projet CocosSharp utilisent cette méthode pour définir la taille du projet par défaut à 1 024 x 768, comme indiqué dans le code suivant :


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

Vous pouvez modifier le `desiredWidth` et `desiredHeight` ci-dessus pour modifier l’affichage pour correspondre à la résolution de votre choix de votre jeu de variables. Par exemple, le code ci-dessus peut être modifié comme suit pour afficher l’arrière-plan 368 x 240 comme plein écran :


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

`SetDesignResolutionSize` nous permet de spécifier comment la fenêtre de jeu s’ajuste à la résolution souhaitée. Les sections suivantes expliquent comment une image de 500 x 500 est affichée avec différents `CCSceneResolutonPolicy` valeurs passées à la `SetDesignResolutionSize` (méthode). Les valeurs suivantes sont fournies par le `CCSceneResolutionPolicy` enum :

 - `ShowAll` : Affiche la résolution demandée entière, en conservant les proportions.
 - `ExactFit` : Affiche la résolution demandée entière, mais ne conserve pas les proportions.
 - `FixedWidth` : Affiche la largeur entière demandée, en conservant les proportions.
 - `FixedHeight` : Affiche la hauteur demandée entière, en conservant les proportions.
 - `NoBorder` : Affiche la hauteur totale ou la largeur entière, conservant ses proportions. Si les proportions de l’appareil est supérieure aux proportions de la résolution souhaitée, toute la largeur est indiquée. Si les proportions de l’appareil est inférieure aux proportions de la résolution souhaitée, toute la hauteur est indiquée.
 - `Custom` – L’affichage varie selon le `Viewport` propriété du courant `CCScene`.

Toutes les captures d’écran de production à résolution iPhone 4 s (960 x 640) en mode paysage et d’utilisent cette image :

![](resolutions-images/image4.png "Toutes les captures d’écran de production à résolution iPhone 4 s 960 x 640 en mode paysage et d’utilisent cette image")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` Spécifie que la résolution du jeu entier sera visible à l’écran, mais peut afficher *cadre* (barres noires) pour ajuster les proportions différentes. Cette stratégie est généralement utilisée comme elle garantit que toute la vue de jeu sera affichée à l’écran sans toute distorsion.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Cadre est visible à gauche et à droite de l’image pour prendre en compte les proportions physique qui est plus large que la résolution de votre choix :

![](resolutions-images/image5.png "Cadre est visible à gauche et à droite de l’image pour prendre en compte les proportions physique qui est plus large que la résolution souhaitée")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` Spécifie que la résolution du jeu entier sera visible à l’écran avec aucun cadre. La zone visible peut être déformée (rapport hauteur / largeur ne peut pas être maintenu) en fonction des proportions de matériel.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Aucun cadre n’est visible, mais étant donné que la résolution de l’appareil est rectangulaire la vue de jeu est déformée :

![](resolutions-images/image6.png "Aucun cadre n’est visible, mais étant donné que la résolution de l’appareil est rectangulaire la vue de jeu est déformée")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Spécifie que la largeur de la vue correspondra à la valeur de la largeur passée à `SetDesignResolutionSize`, mais la hauteur visible est soumise les proportions de l’appareil physique. La valeur de hauteur passée à `SetDesignResolutionSize` est ignoré, car il est calculé lors de l’exécution selon le rapport hauteur / largeur de l’appareil physique. Cela signifie que la hauteur calculée peut être inférieure à la hauteur souhaitée (ce qui entraîne des parties de la vue de jeu en cours hors écran), ou la hauteur calculée peut être supérieure à la hauteur souhaitée (ce qui entraîne plus de la vue de jeu affichée). Étant donné que cela peut entraîner plus de la partie affichée, puis il peut s’afficher comme si cadre s’est produite ; Toutefois, l’espace supplémentaire sera pas nécessairement noire s’il n’importe quel objet visuel. 

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

L’iPhone 4 s a des proportions de 3:2, la hauteur calculée est environ 333 unités :

![](resolutions-images/image7.png "L’iPhone 4 s a des proportions de 3:2, par conséquent, la hauteur calculée est environ 333 unités")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Conceptuellement, `FixedHeight` se comporte de la même façon que `FixedWidth` – le jeu tient compte de la valeur de hauteur passée à `SetDesignResolutionSize,` mais calcule la largeur lors de l’exécution basé sur la résolution physique. Comme mentionné ci-dessus, cela signifie que la largeur affichée être inférieures ou supérieures à la largeur souhaitée, ce qui entraîne une partie du jeu en cours de réduction sur écran ou plus du jeu apparaissent, respectivement.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Dans la mesure où la capture d’écran suivante a été créé avec l’application en cours d’exécution en mode paysage, la largeur calculée est supérieure à 500 – spécifiquement 750. Cette stratégie conserve la valeur 0 X aligné à gauche, par conséquent, la résolution supplémentaire est visible sur le côté droit de l’écran.

![](resolutions-images/image8.png "Cette stratégie conserve la valeur 0 X aligné à gauche, par conséquent, la résolution supplémentaire est visible sur le côté droit de l’écran")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` tente d’afficher l’application avec aucun cadre tout en conservant les proportions d’origine (sans distorsion). Si les proportions de la résolution demandée correspond à physique rapport hauteur / l’appareil largeur, aucun découpage ne se produit. Si les proportions ne correspondent pas, puis de détourage se produit.

Exemple de code :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

La capture d’écran suivante affiche les parties supérieure et inférieure de l’affichage découpée, tandis que toutes les 500 pixels de la largeur d’affichage sont affichés :

![](resolutions-images/image9.png "Cette capture d’écran affiche les parties supérieure et inférieure de l’affichage découpée, tandis que toutes les 500 pixels de la largeur d’affichage sont affichés.")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` permet à chacune `CCScene` pour spécifier sa propre fenêtre d’affichage personnalisé par rapport à la résolution spécifiée dans `SetDesignResolutionSize`.

Définissent des scènes leurs `Viewport` propriété en construisant un `CCViewport`, qui à son tour est construit avec un `CCRect`. Pour plus d’informations sur `CCViewport` et `CCRect` voir le [CocosSharp API Documentation](https://developer.xamarin.com/api/namespace/CocosSharp/). Dans le contexte de la création d’un `CCViewport` le `CCRect` spécifient des paramètres du constructeur (dans l’ordre) :

 - x – la translation horizontalement la fenêtre d’affichage, où chaque unité représente une largeur de l’écran entier. Par exemple, en utilisant une valeur de .5f résultats dans la scène décalée vers la droite de la moitié de la largeur d’écran.
 - y – la translation verticalement la fenêtre d’affichage, où chaque unité représente une hauteur de l’écran entier. Par exemple, en utilisant une valeur de .5f résultats dans la scène décalée vers le bas de la moitié de la hauteur de l’écran.
 - largeur – la partie horizontale que la scène doit occuper. Par exemple, en utilisant une valeur de 1 / version 3.0f entraîne la scène horizontalement occupée par un tiers de l’écran.
 - hauteur – la partie verticale que la scène doit occuper. Par exemple, en utilisant une valeur de 1 / version 3.0f entraîne la scène verticalement occupée par un tiers de l’écran.

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

Le `DefaultTexelToContentSizeRatio` simplifie l’utilisation des textures de résolution plus élevée sur les périphériques avec écrans haute résolution. Plus précisément, cette propriété permet de jeux utiliser des ressources de résolution plus élevée sans avoir à modifier la taille ou le positionnement des éléments visuels. 

Par exemple, un jeu peut inclure un élément multimédia art pour un jeu qui est de 200 pixels de hauteur et l’écran de jeu (tel que spécifié par `SetDesignResolutionSize`) peut être de 400 pixels de hauteur. Dans ce cas, le caractère occupe la moitié de l’écran. Toutefois, si une ressource en hauteur pixel 400 ont été utilisés pour le caractère pour les appareils de résolution plus élevée, le caractère occuperait toute la hauteur de l’affichage. Si l’objectif est de conserver le caractère de la même taille pour tirer parti des périphériques de résolution plus élevées, du code supplémentaire est nécessaire pour conserver le sprite de caractère à moitié de la hauteur de l’écran.

L’exemple simple présenté ci-dessus représente un problème courant dans le développement de jeux : ajout de ressources de grande taille sans obliger le développeur à effectuer de redimensionnement sur chaque élément visuel en fonction de la résolution de l’appareil. `DefaultTexelToContentSizeRatio` une propriété globale est utilisée pour le redimensionnement d’éléments visuels. Cette valeur redimensionne tous les éléments visuels d’un certain type en la valeur assignée.

Cette propriété est présente dans les classes suivantes : 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp de Visual Studio pour l’ensemble de modèles de Mac `CCSprite.DefaultTexelToContentSizeRatio` en fonction de la largeur de l’appareil en tant qu’un exemple de comment il peut être utilisé. Le code suivant est contenu dans `CCApplicationDelegate.ApplicationDidFinishLaunching`:


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

Pour voir comment `DefaultTexelToContentSizeRatio` a un impact sur la taille de visuel des éléments, prenons le code présenté ci-dessus :


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Ce qui entraînera l’image suivante à l’aide d’une texture de 500 x 500 :

![](resolutions-images/image5.png "Ce qui entraînera cette image à l’aide d’une texture de 500 x 500")

IPad Retina s’exécute à une résolution de 2048 x 1536 physique. Cela signifie que le jeu tel qu’affiché ci-dessus serait étirer 500 pixels sur pixels physiques 1536. Jeux peuvent tirer parti de cette résolution en créant ce que sont généralement appelé *hd* actifs – actifs qui sont conçus pour s’exécuter sur les écrans haute résolution. Par exemple, ce jeu peut utiliser une version hd de cette texture dimensionnée à 1 000 x 1 000. Toutefois, à l’aide de l’image plus grande entraînerait la `CCSprite` ayant une largeur et une hauteur de 1 000 unités. Étant donné que notre jeu affiche toujours les 500 unités (en raison du `SetDesignResolutioSize` appeler), notre sprite 1000 x 1000 serait trop grand (affiche uniquement la partie inférieure gauche de celui-ci) :

![](resolutions-images/image11.png "Dans la mesure où le jeu affiche toujours 500 unités en raison de l’appel de SetDesignResolutioSize, le sprite de 1 000 x 1000 serait trop grand qu'affiche uniquement la partie inférieure gauche de celui-ci")

Nous pouvons définir `CCSprite.DefaultTexelToContentSizeRatio` pour prendre en compte pour la texture de 1 000 x 1 000 en cours à deux reprises aussi large et la hauteur de la texture de 500 x 500 d’origine :


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Maintenant si nous exécutons le jeu la texture de 1 000 x 1000 être complètement visible :

![](resolutions-images/image12.png "Maintenant si nous exécutons le jeu la texture de 1 000 x 1000 sera entièrement visible")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio details

Le `DefaultTexelToContentSizeRatio` est propriété `static,` ce qui signifie que tous les composants de l’application partagent la même valeur. L’approche habituelle pour les jeux de ressources pour différentes résolutions doit contenir un ensemble complet de ressources pour chaque catégorie de résolution. Par défaut CocosSharp Visual Studio pour les modèles de Mac fournir **%ld** et **hd** dossiers pour les ressources, ce qui peut être utiles pour les jeux prenant en charge les deux ensembles de textures. Un exemple de dossier contenu avec le contenu peut ressembler :

![](resolutions-images/image13.png "Un exemple de dossier contenu avec le contenu peut ressembler à")

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

Cela signifie que l’application recherche les fichiers dans les chemins d’accès en fonction de si elle s’exécute dans haute résolution ou le mode de résolution standard. Par exemple, si le **hd** et **%ld** dossiers contiennent un fichier appelé **background.png** ensuite le code suivant est exécuté et sélectionner le fichier approprié pour la résolution :


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>Récapitulatif

Cet article explique comment créer des jeux qui s’affiche correctement, quel que soit la résolution de l’appareil. Elle montre des exemples d’utilisation de différents `CCSceneResolutionPolicy` valeurs pour le redimensionnement du jeu en fonction de la résolution de l’appareil. Il fournit également un exemple de `DefaultTexelToContentSizeRatio` peut être utilisé pour prendre en charge de plusieurs ensembles de contenu sans avoir besoin des éléments visuels à redimensionner individuellement.

## <a name="related-links"></a>Liens connexes

- [CocosSharp Introduction](~/graphics-games/cocossharp/index.md)
- [Documentation de l’API de CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
