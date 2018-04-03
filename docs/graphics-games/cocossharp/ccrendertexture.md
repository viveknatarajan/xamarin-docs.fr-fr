---
title: Performances et des effets visuels avec CCRenderTexture
description: CCRenderTexture permet aux développeurs d’améliorer les performances de leurs jeux CocosSharp en réduisant les appels de dessin et peut être utilisé pour créer des effets visuels. Ce guide est fournie avec l’exemple CCRenderTexture pour fournir un exemple pratique de l’utilisation efficace de cette classe.
ms.topic: article
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 36661344fc0f4b9e132e3f721c50f82f3a8db057
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Performances et des effets visuels avec CCRenderTexture

_CCRenderTexture permet aux développeurs d’améliorer les performances de leurs jeux CocosSharp en réduisant les appels de dessin et peut être utilisé pour créer des effets visuels. Ce guide est fournie avec l’exemple CCRenderTexture pour fournir un exemple pratique de l’utilisation efficace de cette classe._

La `CCRenderTexture` classe fournit les fonctionnalités pour le rendu de plusieurs objets CocosSharp une texture unique. Une fois créé, `CCRenderTexture` instances peuvent être utilisées pour restituer des graphiques de manière efficace et pour implémenter des effets visuels. `CCRenderTexture` permet à plusieurs objets à restituer une texture unique une seule fois. Ensuite, cette texture peut être réutilisée chaque image, ce qui réduit le nombre total d’appels de dessin.

Ce guide explique comment utiliser le `CCRenderTexture` objet afin d’améliorer les performances de rendu des cartes dans un jeu de cartes SINV à regrouper (CCG). Il montre également comment `CCRenderTexture` peut être utilisé pour rendre une entité entière transparente. Ce guide fait référence à la `CCRenderTexture` [exemple de projet](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Ce guide fait référence à l’exemple de projet CCRenderTexture")


## <a name="card--a-typical-entity"></a>Une entité de type carte

Avant d’examiner l’utilisation de `CCRenderTexture` de l’objet, nous allons tout d’abord familiariser nous-mêmes avec la `Card` entité que nous allons utiliser tout au long de ce projet pour Explorer la `CCRenderTexture` classe. Le `Card` classe est une entité typiques, suivant le modèle d’entité décrit dans le [guide de l’entité](~/graphics-games/cocossharp/entities.md). La classe de carte a tous ses composants visual (instances de `CCSprite` et `CCLabel`) répertoriés en tant que champs :


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

Les instances de carte peuvent être rendus en utilisant un `CCRenderTexture`, ou en dessinant des individuellement chaque composant visuel. Bien que chaque composant est un objet indépendant, le `CCNode` apparenter système utilisé dans des entités rend le `Card` se comportent comme un objet unique : au moins la plupart du temps. Par exemple, si un `Card` entité est repositionnée, redimensionnée ou pivotée, puis tous les objets visuels contenus sont affectés pour rendre la carte semble être un objet unique. Pour afficher les cartes se comportent comme un objet unique, nous pouvons modifier le `GameLayer.AddedToScene` pour définir le `useRenderTextures` variable `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

Le `GameLayer` code ne déplace pas chaque élément visuel indépendamment, encore chaque élément visuel dans le `Card` entité est positionnée correctement :

![](ccrendertexture-images/image1.png "Le code GameLayer ne déplace pas indépendamment chaque élément visuel, mais chaque élément visuel dans l’entité de la carte est correctement positionné")

L’exemple est codé pour exposer deux problèmes qui peuvent se produire lors de chaque composant visuel s’affiche :

- Les performances peuvent être impactées en raison de plusieurs appels de dessin
- Certains effets visuels, tels que la transparence, ne peut pas être implémentés correctement, que nous allons examiner ultérieurement


### <a name="card-draw-calls"></a>Appels de dessin de carte

Notre code est une version simplifiée de ce qui peut être trouvé dans un complet *jeu de cartes SINV à regrouper* (CCG) tel que « Magic : la collecte » ou « Hearthstone ». Notre jeu uniquement affiche à la fois les trois cartes et a un petit nombre d’unités possibles (bleu, vert et orange). En revanche, un jeu complet peut avoir plus de vingt cartes à l’écran à un moment donné, et lecteurs peuvent avoir des centaines de cartes sélectionnables lors de la création de leurs ponts. Bien que notre jeu ne sont pas actuellement affectées à des problèmes de performances, un jeu complet avec une implémentation similaire peuvent l’être.

CocosSharp fournit une idée des performances de rendu en exposant les appels de dessin image par image. Notre `GameLayer.AddedToScene` méthode définit la `GameView.Stats.Enabled` à `true`, ce qui donne des informations de performances affichées en bas à gauche de l’écran :

![](ccrendertexture-images/image2.png "La méthode GameLayer.AddedToScene définit le GameView.Stats.Enabled sur true, ce qui entraîne des informations de performances affichées en bas à gauche de l’écran")

Notez que bien qu’ils aient des trois cartes sur l’écran, nous avons dix-neuf les appels de dessin (résultats de chaque carte six appels de dessin, le texte à afficher les comptes d’informations de performances pour l’une plus). Les appels de dessin ont un impact significatif sur les performances d’un jeu, donc CocosSharp fournit plusieurs façons de réduire les. Une technique est décrite dans le [CCSpriteSheet guide](~/graphics-games/cocossharp/ccspritesheet.md). Une autre technique consiste à utiliser le `CCRenderTexture` afin de réduire chaque entité jusqu'à un seul appel, comme nous allons examiner dans ce guide.


### <a name="card-transparency"></a>Transparence de la carte

Notre `Card` entité inclut un `Opacity` propriété transparence contrôle tel qu’indiqué dans l’extrait de code suivant :


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

Notez que la méthode setter prend en charge à l’aide de restituer des textures ou le rendu de chaque composant individuellement. Pour afficher ses effets, modifiez le `opacity` valeur `127` (environ deux fois opacité) dans `GameLayer.AddedToScene` qui se traduira par chaque composant ayant une `Opacity` valeur de `127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

Le jeu sera restitué maintenant les cartes avec une transparence, provoquant la plus foncés étant donné que l’arrière-plan est noir :

![](ccrendertexture-images/image3.png "Le jeu sera rendue maintenant les cartes avec une transparence, provoquant la plus foncés étant donné que l’arrière-plan est noir")

À première vue, il peut se présenter comme si nos cartes ont été correctement effectuées transparents. Toutefois, la capture d’écran affiche un nombre de problèmes visuels.

Étant donné que notre expérience est noire, nous pouvons prévoir que chaque partie de notre carte devient plus sombre en raison de la transparence. Autrement dit, le plus transparent devient d’une carte, devient sombre. Au niveau d’opacité 0, un `Card` est totalement transparent (totalement noir). Toutefois, certaines parties de notre carte n’a pas été deviennent plus sombre lors de l’opacité a été remplacée par `127`. Pire encore, certaines parties de notre carte réellement est devenu plus claires lorsqu’elles sont devenues plus transparentes. Examinons les parties de notre carte qui ont été noirs *avant* ils étaient transparents – en particulier le texte de détail et les contours noirs autour du graphique monster. Si nous placer côte à côte, nous pouvons voir l’impact de l’application de la transparence :

![](ccrendertexture-images/image4.png "Si placées côte à côte, l’impact de l’application de transparence peut être observé")

Comme indiqué ci-dessus, toutes les parties de la carte doivent devenir plus sombre lors de devenir plus transparents, mais le nombre de zones cela n’est pas le cas :

- Plan du robot devient plus clair (allant du noir, gris)
- Le texte de description devient plus clair (allant du noir, gris)
- La composante verte de l’automate devienne moins saturée, mais il ne devient pas plus sombre

Pour mieux visualiser pourquoi cela se produit, nous devons n’oubliez pas que chaque composant visuel est dessiné indépendamment, chaque partiellement transparent. Le premier composant visual dessiné est un arrière-plan de la carte. Transparent aux éléments suivants seront dessinés au-dessus de la carte et seront affectées par l’arrière-plan de la carte. Si nous supprimer du texte à partir de notre carte et descendre le graphique de robot, nous pouvons voir l’impact du robot de l’arrière-plan de la carte. Notez la ligne orange dans la zone supérieure peut être affichée dans le robot, et que la zone de l’automate qui chevauche la bande bleue dans le centre de la carte est dessinée plus sombre :

![](ccrendertexture-images/image5.png "Notez que la ligne orange dans la zone supérieure peut être affichée dans le robot, et que la zone de l’automate qui chevauche la bande bleue dans le centre de la carte est dessinée plus sombre")

À l’aide un `CCRenderTexture` permet de rendre la carte entière transparente sans impact sur le rendu des composants individuels au sein de la carte, comme nous le verrons plus loin dans ce guide.


## <a name="using-ccrendertexture"></a>À l’aide de CCRenderTexture

Maintenant que nous avons identifié les problèmes de rendu de chaque composant individuellement, nous allons activer le rendu à un `CCRenderTexture` et comparer le comportement.

Pour activer le rendu à un `CCRenderTexture`, modifiez le `userRenderTextures` variable `true` dans `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>Appels de dessin de carte

Si nous exécutons le jeu maintenant, nous allons voir les appels de dessin réduites de dix-neuf à quatre (chaque carte réduite de six une) :

![](ccrendertexture-images/image6.png "Si le jeu est exécuté maintenant, les appels de dessin réduite de dix-neuf à quatre à chaque carte réduite de six à un")

Comme mentionné précédemment, ce type de réduction peut avoir un impact significatif sur les jeux avec plusieurs entités visual sur l’écran.


### <a name="card-transparency"></a>Transparence de la carte

Une fois la `useRenderTextures` a la valeur `true`, cartes transparents restituera différemment :

![](ccrendertexture-images/image7.png "Une fois l’useRenderTextures est définie sur true, transparents cartes aura un autre")

Nous allons comparer la carte de robot transparent à l’aide de textures de rendu (à gauche) et sans (à droite) :

![](ccrendertexture-images/image8.png "Comparer la carte de robot transparent à l’aide de vs (gauche) de textures rendu sans (à droite)")

Différences les plus évidentes sont dans le texte de détails (noir au lieu de gris clair) et le sprite robot (foncé au lieu de la lumière et désaturée).


## <a name="ccrendertexture-details"></a>Détails de CCRenderTexture

Maintenant que nous avons vu les avantages de l’utilisation de `CCRenderTexture`, examinons à présent comment il est utilisé dans le `Card` entité.

Le `CCRenderTexture` est une zone qui peut être la cible de rendu. Elle comporte deux principales différences par rapport à l’écran de jeu :

1. Le `CCRenderTexture` persiste entre les frames. Cela signifie qu’un `CCRenderTexture` doit uniquement être affiché lorsque des modifications se produisent. Dans notre cas, le `Card` entité ne change jamais, donc il est rendu uniquement une seule fois. Le cas échéant `Card` composants modifiés, puis la carte devez redessiner à son `CCRenderTexture`. Par exemple, si la valeur de HP (points de contrôle d’intégrité) modifiée des cas d’attaque, la carte doit s’afficher pour refléter la nouvelle valeur de HP.
1. Le `CCRenderTexture` dimensions en pixels ne sont pas liées à l’écran. Un `CCRenderTexture` peut être supérieure ou inférieure à la résolution de l’appareil. Le `Card` code crée un `CCRenderTexture` à l’aide de la taille de son sprite en arrière-plan. La carte contient une référence à un `CCRenderTexture` appelée `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

Le `renderTexture` instance reste `null` jusqu'à ce que le `UseRenderTexture` est affectée à la valeur true, les appels `SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

Le `SwitchToRenderTexture` méthode peut être appelée chaque fois que la texture doit être actualisé. Elle peut être appelée si la carte est déjà à l’aide de son `CCRenderTexture` ou bascule vers le `CCRenderTexture` pour la première fois.

Les sections suivantes explorent le `SwitchToRenderTexture` (méthode). 


### <a name="ccrendertexture-size"></a>Taille de CCRenderTexture

Le constructeur CCRenderTexture requiert deux jeux de dimensions. Le premier contrôle la taille de la `CCRenderTexture` quand il est dessiné, et le second spécifie la largeur en pixels et la hauteur de son contenu. Le `Card` entité instancie sa `CCRenderTexture` à l’aide de l’arrière-plan [contentsize a](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Notre jeu a un `DesignResolution` de 512 par 384, comme indiqué dans `ViewController.LoadGame` sur iOS et `MainActivity.LoadGame` sur Android :


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

Le `CCRenderTexture` constructeur est appelé avec le `background.ContentSize` comme premier paramètre, ce qui indique que le `CCRenderTexture` doit être aussi volumineux que l’arrière-plan `CCSprite`. Depuis l’arrière-plan de carte `CCSprite` est 200 pixels de hauteur, de la carte occupera environ la moitié de la hauteur de l’écran.

Le deuxième paramètre passé à la `CCRenderTexture` constructeur spécifie la résolution en pixels de la `CCRenderTexture`. Comme indiqué dans le [guide de résolution de CocosSharp](~/graphics-games/cocossharp/resolutions.md), la largeur et la hauteur de la zone affichable en unités de jeu est souvent pas le même que la résolution en pixels de l’écran. De même, un CCRenderTexture peut utiliser une résolution supérieure à sa taille, les éléments visuels apparaissent donc nettes sur les appareils haute résolution.

La résolution de pixel est deux fois la taille de la CCRenderTexture pour éviter que le texte à partir de la granularité recherche :


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Pour comparer, nous pouvons modifier la valeur pixelResolution pour faire correspondre le `background.ContentSize` (sans être doublé) et comparer le résultat : 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Pour comparer, peut modifier la valeur pixelResolution pour correspondre à l’arrière-plan. ContentSize sans être doublé et comparer le résultat")


### <a name="rendering-to-a-ccrendertexture"></a>Rendu d’un CCRenderTexture

En règle générale, les objets visuels dans CocosSharp ne sont pas rendus explicitement. Au lieu de cela, les objets visuels sont ajoutés à un `CCLayer` qui fait partie d’un `CCScene`. CocosSharp affiche automatiquement le `CCScene` et sa hiérarchie visuelle dans chaque frame sans aucun code de rendu qui est appelée. 

En revanche, la `CCRenderTexture` doit être dessiné explicitement à. Ce rendu permettre être divisé en trois étapes :

1. `CCRenderTexture.BeginWithClear` est appelée, indiquant que tout le rendu suivantes cibleront appelant `CCRenderTexture`.
1. Objets héritant de `CCNode` (comme le `Card` entité) sont rendus dans le `CCRenderTexture` en appelant `Visit`.
1. `CCRenderTexture.End` est appelée, indiquant que le rendu à la `CCRenderTexture` est terminée.

N’importe quel nombre d’objets peut être rendu dans un `CCRenderTexture` entre ses `Begin` et `End` appels. Avant le rendu, tous les objets visibles nécessaires sont ajoutés en tant qu’enfants :


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

Le `renderTexture` ne doit pas faire partie de la carte lors du rendu, il est donc supprimé :


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Maintenant le `Card` instance peut afficher pour le `CCRenderTexture` instance :


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

Une fois le rendu est terminé, les composants individuels sont supprimés et le `CCRenderTexture` est rajouté.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

## <a name="summary"></a>Récapitulatif

Couverts par ce guide les `CCRenderTexture` classe à l’aide un `Card` entité peut être utilisé dans un jeu de cartes pouvant être collecté. Il vous a montré comment utiliser le `CCRenderTexture` classe pour améliorer la fréquence d’images et de mise en œuvre à l’échelle de l’entité transparence.

Bien que ce guide utilisé un `CCRenderTexture` contenues dans une entité, cette classe peut être utilisé pour restituer plusieurs entités ou complets `CCLayer` instances pour les améliorations des performances et les effets sur l’ensemble de l’écran.

## <a name="related-links"></a>Liens associés

- [Référence de l’API CCRenderTexture](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Projet complet (exemple)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
