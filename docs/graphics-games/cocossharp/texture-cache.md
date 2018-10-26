---
title: La mise en cache de texture à l’aide de CCTextureCache
description: Classe de CCTextureCache de CocosSharp fournit un moyen standard pour organiser, cache et le déchargement de contenu. Il est particulièrement utile pour les jeux volumineux qui ne correspondent pas entièrement dans la mémoire vive, ce qui simplifie le processus de regroupement et de suppression des textures.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 232363d6ce1cb93499716b2c1247c48403cf6cea
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117380"
---
# <a name="texture-caching-using-cctexturecache"></a>La mise en cache de texture à l’aide de CCTextureCache

_Classe de CCTextureCache de CocosSharp fournit un moyen standard pour organiser, cache et le déchargement de contenu. Il est particulièrement utile pour les jeux volumineux qui ne correspondent pas entièrement RAM, ce qui simplifie le processus de regroupement et de suppression des textures._

Le `CCTextureCache` classe est une partie essentielle du développement de jeux de CocosSharp. Utilisation de CocosSharp la plupart des jeux le `CCTextureCache` de l’objet, même si pas explicitement, autant de méthodes CocosSharp utilise en interne un *partagé* cache de texture.

Ce guide couvre le `CCTextureCache` et pourquoi il est important pour le développement de jeux. Plus précisément, il est décrit :

 - Pourquoi les questions de mise en cache de texture
 - Durée de vie de texture
 - À l’aide de SharedTextureCache
 - Le chargement et préchargement avec AddImage différé
 - Suppression des textures


## <a name="why-texture-caching-matters"></a>Pourquoi les questions de mise en cache de texture

La mise en cache de texture d’est un aspect important de développement de jeux comme le chargement de la texture est une opération longue et textures nécessitent une quantité importante de mémoire vive lors de l’exécution.

Comme toute opération de fichier, le chargement des textures à partir du disque peut être une opération coûteuse. Chargement de texture peut prendre du temps si le fichier chargé requiert un traitement, telles que la décompression (comme c’est le cas pour les images png et jpg). La mise en cache de texture peut réduire le nombre de fois que l’application doit charger des fichiers à partir du disque.

Comme mentionné ci-dessus, les textures occupent également une grande quantité de mémoire du runtime. Par exemple, une image d’arrière-plan dimensionnée en fonction de la résolution d’un iPhone 6 (1344 x 750) occuperait 4 mégaoctets de RAM – même si le fichier PNG est de taille uniquement de quelques kilo-octets. La mise en cache de texture fournit un moyen de partager des références de texture au sein d’une application et également un moyen simple de décharger tout le contenu lors de la transition entre les différents États de jeu.


## <a name="texture-lifespan"></a>Durée de vie de texture

CocosSharp textures peuvent être conservées en mémoire pendant toute la durée de l’exécution d’une application, ou ils peuvent être courte durée de vie. Pour minimiser la mémoire utilisation d’une application doit supprimer les textures lorsque vous n’avez plus besoin. Bien entendu, cela signifie que les textures peuvent être supprimés et chargées à nouveau ultérieurement, ce qui peut augmenter le temps de chargement ou de nuire aux performances au cours des charges. 

Chargement de texture souvent nécessite un compromis entre les heures d’utilisation et de charge mémoire / performances d’exécution. Jeux, qui utilisent une petite quantité de mémoire de texture peuvent conserver toutes les textures en mémoire en fonction des besoins, mais les jeux plus volumineux peuvent devoir décharger des textures pour libérer de l’espace.

Le diagramme suivant illustre un jeu simple qui charge les textures en fonction des besoins et les conserve en mémoire pendant toute la durée d’exécution :

![](texture-cache-images/image1.png "Ce diagramme illustre un jeu simple qui charge les textures en fonction des besoins et les conserve en mémoire pendant toute la durée d’exécution")

Les deux premières barres représentent les textures qui sont nécessaires immédiatement lors de l’exécution du jeu. Les trois barres suivants représentent des textures pour chaque niveau, chargés en fonction des besoins.

Si le jeu était grand suffisamment il chargerait finalement assez textures pour remplir toute la mémoire RAM fourni par l’appareil et le système d’exploitation. Pour résoudre ce problème, un jeu peut décharger les données de texture lorsqu’il n’est plus nécessaire. Par exemple, le diagramme suivant montre un jeu qui décharge Level1Texture lorsqu’il n’est plus nécessaire, puis charge Level2Texture pour le niveau suivant. Le résultat final est que seules trois textures sont conservées en mémoire à un moment donné : 

![](texture-cache-images/image2.png "Le résultat final est que seules trois textures sont conservées en mémoire à un moment donné")


Le diagramme ci-dessus indique que l’utilisation de mémoire de texture peut être réduite en déchargeant, mais cela peut nécessiter des temps de chargement supplémentaires si un lecteur décide d’un niveau de la relecture. Il est également important de noter que les textures UITexture et MainCharacter sont chargés et déchargés jamais. Cela implique que ces textures sont nécessaires dans tous les niveaux, afin qu’ils sont toujours conservées en mémoire. 


## <a name="using-sharedtexturecache"></a>À l’aide de SharedTextureCache

CocosSharp met automatiquement en cache les textures lors de leur chargement via la `CCSprite` constructeur. Par exemple le code suivant crée uniquement une instance de la texture :


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp met en cache automatiquement le `star.png` texture afin d’éviter la coûteuse de créer de nombreux identiques `CCTexture2D` instances. Cela `AddImage` appelée sur un partage `CCTextureCache` de l’instance, en particulier `CCTextureCache.SharedTextureCache.Shared`. Pour comprendre comment la `SharedTextureCache` sert nous pouvons examiner le code suivant qui est fonctionnellement identique à l’appel le `CCSprite` constructeur avec un paramètre de chaîne :


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` vérifie si le fichier d’arguments (dans ce cas `star.png`) a déjà été chargé. Dans ce cas, l’instance mise en cache est retourné. Si pas, il est chargé à partir du système de fichiers, et une référence à la texture est stockée en interne pour suivantes `AddImage` appels. En d’autres termes les `star.png` image est chargée uniquement une fois, et les appels suivants ne nécessitent aucun accès au disque supplémentaire ou la mémoire de texture supplémentaires.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Le chargement et préchargement avec AddImage différé

`AddImage` permet au code à écrire le même si la texture demandée est déjà chargée ou non. Cela signifie que le contenu ne sera pas chargé jusqu'à ce qu’il est nécessaire ; Toutefois, cela peut également entraîner des problèmes de performances lors de l’exécution en raison du chargement de contenu imprévisible.

Par exemple, considérez un jeu où arme du lecteur peut être mis à niveau. Lors de la mise à niveau, l’arme et projectiles visiblement changeront, ce qui entraîne des textures nouveau utilisés. Si le contenu est chargée en différé puis les textures associés armes mis à niveau ne seront pas chargées initialement, mais plutôt ultérieurement lorsque le joueur acquiert les mises à niveau. 

Ce chargement mid jeu peut provoquer le jeu pour *pop*, qui est un blocage court mais notable dans l’exécution. Pour éviter ce problème, le code peut prédire les textures peuvent être nécessaires avant de configurer et de les charger préalablement. Par exemple, ce qui suit peut être utilisé pour précharger des textures :


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Ce préchargement peut entraîner un gaspillage de mémoire et peut augmenter le temps de démarrage. Par exemple, le joueur ne peut jamais réellement obtenir un power-up représenté par le `powerup3.png` de texture, afin qu’il sera chargé inutilement. Bien sûr, cela peut être un coût nécessaire de payer pour éviter un point de présence potentielle dans le jeu, il est généralement préférable de préchargement contenu si elle tient dans la mémoire RAM.


## <a name="disposing-textures"></a>Suppression des textures

Si un jeu ne nécessite pas de plus de mémoire de texture qu’est disponible sur l’appareil spec minimale textures n’avez pas besoin d’être supprimée. Quant à eux, jeux plus volumineux, peut-être libérer la mémoire de texture pour faire place à nouveau contenu. Par exemple, un jeu peut utiliser une grande quantité de mémoire stockant des textures pour un environnement. Si l’environnement est uniquement utilisé dans un niveau spécifique puis il doit être déchargé lorsque le niveau se termine.


### <a name="disposing-a-single-texture"></a>Suppression d’une texture unique

La suppression d’une texture unique nécessite tout d’abord appeler la `Dispose` (méthode), puis suppression manuelle à partir de la `CCTextureCache`.

Ce qui suit montre comment supprimer complètement le sprite, ainsi que sa texture d’arrière-plan :


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Disposing directement les textures peut être efficace lorsque le traitement avec un petit nombre de textures, mais cela peut devenir susceptibles d’engendrer des erreurs lors du traitement de jeux de texture plus volumineux.

Les textures peuvent être regroupés en personnalisé (non partagée) `CCTextureCache` instances pour simplifier le nettoyage de la texture.

Par exemple, considérons un exemple où le contenu est préchargé à l’aide d’un niveau spécifique `CCTextureCache` instance. Le `CCTextureCache` instance peut être définie dans la classe qui définit le niveau (qui peut être un `CCLayer` ou `CCScene`) :


```csharp
CCTextureCache levelTextures; 
```

Le `levelTextures` instance peut ensuite être utilisée pour précharger les textures de niveau spécifiques : 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Enfin lorsque le niveau se termine, les textures peuvent être tout supprimées à la fois via le `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

La méthode Dispose supprime toutes les textures internes, éliminant la mémoire utilisée par ces textures. Combinaison de `CCTextureCache.Shared` avec un niveau spécifiques ou au jeu mode `CCTextureCache` instance entraîne des textures persistent tout au long de la partie entière et d’autres en cours de déchargement comme niveaux se terminer, similaire au diagramme présenté au début de ce guide : 

![](texture-cache-images/image2.png "Combinant CCTextureCache.Shared avec un niveau ou de jeux spécifiques au mode CCTextureCache instance entraîne des textures persistent tout au long de la partie entière et d’autres en cours de déchargement comme niveaux se terminer, similaire au diagramme présenté au début de ce guide")




## <a name="summary"></a>Récapitulatif

Ce guide montre comment utiliser le `CCTextureCache` classe aux performances de l’utilisation et de runtime de la mémoire solde. `CCTexturCache.SharedTextureCache` Vous pouvez explicitement ou implicitement utilisée pour charger et mettre en cache les textures pour la durée de vie de l’application, alors que `CCTextureCache` instances peuvent être utilisées pour décharger des textures pour réduire l’utilisation de la mémoire.

## <a name="related-links"></a>Liens connexes

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
