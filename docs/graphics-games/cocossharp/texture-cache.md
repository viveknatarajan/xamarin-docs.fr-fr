---
title: La mise en cache de texture à l’aide de CCTextureCache
description: Classe de CCTextureCache de CocosSharp fournit un moyen standard de les organiser, de cache et le déchargement de contenu. Il est particulièrement utile pour les jeux de grande taille qui peut ne pas correspondre entièrement en mémoire vive, ce qui simplifie le processus de regroupement et à la suppression des textures.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: c217d8a935ae971aab472b05968c0251366362b2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783686"
---
# <a name="texture-caching-using-cctexturecache"></a>La mise en cache de texture à l’aide de CCTextureCache

_Classe de CCTextureCache de CocosSharp fournit un moyen standard de les organiser, de cache et le déchargement de contenu. Il est particulièrement utile pour les jeux de grande taille qui peut ne pas correspondre entièrement en mémoire vive, ce qui simplifie le processus de regroupement et à la suppression des textures._

La `CCTextureCache` classe est une partie essentielle du développement de jeux CocosSharp. CocosSharp la plupart des jeux d’utiliser le `CCTextureCache` de l’objet, même si ne sont pas explicitement, autant de méthodes CocosSharp utilise en interne un *partagé* du cache de texture.

Ce guide couvre le `CCTextureCache` et pourquoi il est important pour le développement de jeux. Il traite en particulier :

 - Pourquoi questions mise en cache de texture
 - Durée de vie de texture
 - À l’aide de SharedTextureCache
 - Chargement et précharge avec AddImage différé
 - Suppression des textures


## <a name="why-texture-caching-matters"></a>Pourquoi questions mise en cache de texture

La mise en cache de texture d’est un facteur important pour le développement de jeux lors du chargement de la texture est une opération chronophage et textures nécessitent une quantité importante de mémoire RAM lors de l’exécution.

Comme avec toute opération de fichier, le chargement des textures à partir du disque peut être une opération coûteuse. Lors du chargement de texture peut prendre du temps si le fichier chargé requiert un traitement, telles que la décompression (comme c’est le cas pour les images png et jpg). La mise en cache de texture peut réduire le nombre de fois que l’application doit charger des fichiers à partir du disque.

Comme indiqué ci-dessus, les textures occupent également une grande quantité de mémoire d’exécution. Par exemple, une image d’arrière-plan à la résolution d’un iPhone 6 (1344 x 750) occuperait 4 mégaoctets de mémoire RAM – même si le fichier PNG est de taille uniquement quelques kilo-octets. La mise en cache de texture fournit un moyen de partager des références de texture dans une application et également un moyen simple de décharger tout le contenu lors de la transition entre les différents États de jeu.


## <a name="texture-lifespan"></a>Durée de vie de texture

CocosSharp textures peuvent être conservées en mémoire pendant toute la durée d’exécution d’une application, ou ils peuvent être courte durées. Utilisation d’une application doit supprimer pour réduire la mémoire textures lorsque vous n’avez plus besoin. Bien entendu, cela signifie que les textures peuvent être supprimés et chargés à nouveau ultérieurement, ce qui peut augmenter le temps de chargement ou dégrader les performances pendant le chargement. 

Chargement de texture souvent requiert un compromis entre les heures d’utilisation et de charge mémoire / performances d’exécution. Jeux qui utilisent une petite quantité de mémoire de texture peuvent conserver toutes les textures en mémoire en fonction des besoins, mais plus grandes jeux devrez décharger des textures à libérer de l’espace.

Le diagramme suivant illustre un jeu simple qui charge des textures en fonction des besoins et les conserve en mémoire pendant toute la durée d’exécution :

![](texture-cache-images/image1.png "Ce diagramme illustre un jeu simple qui charge des textures en fonction des besoins et les conserve en mémoire pendant toute la durée d’exécution")

Les deux premières barres représentent des textures nécessaires immédiatement lors de l’exécution de du jeu. Les trois barres suivantes représentent les textures pour chaque niveau, chargées en fonction des besoins.

Si le jeu est grand suffisamment il charge finalement suffisamment textures pour remplir toute la mémoire RAM fournies par le périphérique et le système d’exploitation. Pour résoudre ce problème, un jeu peut décharger les données de texture lorsqu’il n’est plus nécessaire. Par exemple, le diagramme suivant illustre un jeu qui décharge Level1Texture lorsqu’il n’est plus nécessaire, puis charge Level2Texture pour le niveau suivant. Le résultat final est que seuls trois textures sont conservées en mémoire à un moment donné : 

![](texture-cache-images/image2.png "Le résultat final est que seuls trois textures sont conservées en mémoire à un moment donné")


Le diagramme ci-dessus indique que l’utilisation de mémoire de texture peut être réduite en déchargeant, mais cette opération peut nécessiter le temps de chargement supplémentaires si un lecteur décide d’un niveau de la relecture. Il est également important de noter que les textures UITexture et MainCharacter sont chargés et déchargés jamais. Cela implique que ces textures sont nécessaires dans tous les niveaux, afin qu’ils sont toujours conservés en mémoire. 


## <a name="using-sharedtexturecache"></a>À l’aide de SharedTextureCache

CocosSharp met automatiquement en cache les textures lors de leur chargement par le biais du `CCSprite` constructeur. Par exemple le code suivant crée uniquement une instance de la texture :


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp met automatiquement en cache le `star.png` texture afin d’éviter l’alternative coûteux de créer de nombreux identiques `CCTexture2D` instances. Cela est accompli par `AddImage` appelée sur un élément partagé `CCTextureCache` de l’instance, en particulier `CCTextureCache.SharedTextureCache.Shared`. Pour comprendre comment la `SharedTextureCache` sert nous pouvons examiner le code suivant qui est fonctionnellement identique à l’appel du `CCSprite` constructeur avec un paramètre de chaîne :


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` vérifie si le fichier d’argument (dans ce cas `star.png`) a déjà été chargé. Dans ce cas, l’instance mise en cache est retourné. Si pas puis il est chargé à partir du système de fichiers, et une référence à la texture est stockée en interne pour ultérieures `AddImage` appels. En d’autres termes la `star.png` image est chargée uniquement une seule fois, et les appels suivants ne nécessitent aucun accès au disque supplémentaires ou la mémoire de texture supplémentaires.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Chargement et précharge avec AddImage différé

`AddImage` permet à du code à écrire la même que la texture demandée est déjà chargée ou non. Cela signifie que le contenu ne sera pas chargé tant qu’il est nécessaire ; Toutefois, cela peut également entraîner des problèmes de performances lors de l’exécution en raison du chargement de contenu imprévisible.

Par exemple, considérez un jeu où arme du joueur peut être mis à niveau. Lors de la mise à niveau, l’arme et projectiles visiblement changera, aboutissant à un nouveau textures utilisés. Si le contenu est chargée en différé puis les textures associées armes mis à niveau ne seront pas chargées initialement, mais plutôt ultérieurement lorsque le joueur acquiert les mises à niveau. 

Ce chargement mid jeu peut entraîner le jeu pour *pop*, qui est un blocage court, mais néanmoins notable, dans l’exécution. Pour éviter cela, le code peut prédire les textures peuvent être nécessaires et avant de les charger. Par exemple, les éléments suivants peuvent être utilisées pour précharger des textures :


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Cette précharge peut en mémoire perdu et peut augmenter le temps de démarrage. Par exemple, le lecteur ne peut en réalité jamais obtenir une tension représentée par le `powerup3.png` de texture, afin qu’il sera chargé inutilement. Bien entendu, cela peut être un coût nécessaire pour payer pour éviter un pop potentiel dans le jeu, il est généralement préférable de préchargement contenu si elle tient dans la mémoire RAM.


## <a name="disposing-textures"></a>Suppression des textures

Si un jeu ne requiert pas plus de mémoire de texture qu’est disponible sur l’appareil spec minimale textures n’avez pas besoin d’être supprimés. En revanche, jeux supérieure peut-être afin de libérer la mémoire de texture à libérer de l’espace pour le nouveau contenu. Par exemple, un jeu peut utiliser une grande quantité de mémoire de stockage des textures pour un environnement. Si l’environnement est utilisé uniquement dans un niveau spécifique, il doit avoir déchargé lorsque le niveau se termine.


### <a name="disposing-a-single-texture"></a>Suppression d’une texture unique

La suppression d’une texture unique nécessite tout d’abord appeler la `Dispose` (méthode), puis la suppression manuelle de la `CCTextureCache`.

Voici comment supprimer complètement un composant en arrière-plan, ainsi que sa texture :


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Supprimant directement les textures peut être efficace lorsque le traitement avec un petit nombre de textures, mais il peut devenir sujettes aux erreurs lorsque vous traitez des jeux de texture plus grands.

Les textures peuvent être regroupés en personnalisés (non partagés) `CCTextureCache` instances pour simplifier le nettoyage de la texture.

Par exemple, prenons un exemple où le contenu est préchargé à l’aide d’un niveau spécifique `CCTextureCache` instance. Le `CCTextureCache` instance peut-être être définie dans la classe qui définit le niveau (qui peut être un `CCLayer` ou `CCScene`) :


```csharp
CCTextureCache levelTextures; 
```

Le `levelTextures` instance puis peut être utilisée pour précharger les textures de niveau spécifique : 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Enfin lorsque le niveau se termine, les textures peuvent être tout supprimés à la fois via la `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

La méthode Dispose supprimera toutes les textures internes, en supprimant la mémoire utilisée par ces textures. Combinaison de `CCTextureCache.Shared` avec un niveau spécifiques ou au jeu mode `CCTextureCache` instance entraîne des textures persistent tout au long de la partie entière et d’autres en cours de déchargement comme niveaux, similaire au diagramme présenté au début de ce guide : 

![](texture-cache-images/image2.png "Combinaison de CCTextureCache.Shared avec un niveau ou un jeu spécifique au mode CCTextureCache instance dans certaines textures persistent tout au long de la partie entière et d’autres en cours de déchargement comme niveaux, similaire au diagramme présenté au début de ce guide")




## <a name="summary"></a>Récapitulatif

Ce guide montre comment utiliser la `CCTextureCache` classe pour les performances d’utilisation et d’exécution de la mémoire de solde. `CCTexturCache.SharedTextureCache` Vous pouvez explicitement ou implicitement utilisée pour charger et mettre en cache les textures pour la durée de vie de l’application, alors que `CCTextureCache` instances peuvent être utilisées pour décharger les textures pour réduire l’utilisation de mémoire.

## <a name="related-links"></a>Liens connexes

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
