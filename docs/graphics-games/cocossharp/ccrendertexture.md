---
title: Performances et des effets visuels avec CCRenderTexture
description: CCRenderTexture permet aux développeurs d’améliorer les performances de leurs jeux de CocosSharp en réduisant les appels de dessin et peut être utilisé pour la création d’effets visuels. Ce guide accompagne l’exemple CCRenderTexture pour fournir un exemple pratique de l’utilisation efficace de cette classe.
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 95227689303a8367785202956a6aaef921c1c593
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526440"
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Performances et des effets visuels avec CCRenderTexture

_CCRenderTexture permet aux développeurs d’améliorer les performances de leurs jeux de CocosSharp en réduisant les appels de dessin et peut être utilisé pour la création d’effets visuels. Ce guide accompagne l’exemple CCRenderTexture pour fournir un exemple pratique de l’utilisation efficace de cette classe._

Le `CCRenderTexture` classe fournit les fonctionnalités pour le rendu de plusieurs objets de CocosSharp à une texture unique. Une fois créé, `CCRenderTexture` instances peuvent être utilisées pour restituer des graphiques de manière efficace et pour implémenter des effets visuels. `CCRenderTexture` permet à plusieurs objets à restituer une texture unique pour une seule fois. Ensuite, cette texture peut être réutilisé chaque trame, ce qui réduit le nombre total d’appels de dessin.

Ce guide examine comment utiliser le `CCRenderTexture` objet afin d’améliorer les performances de rendu des cartes dans un jeu de cartes collectable (CCG). Il montre également comment `CCRenderTexture` peut être utilisé pour rendre une entité complète transparente. Ce guide fait référence le `CCRenderTexture` [exemple de projet](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Ce guide fait référence à l’exemple de projet CCRenderTexture")


## <a name="card--a-typical-entity"></a>Carte – une entité standard

Avant d’examiner comment utiliser `CCRenderTexture` de l’objet, nous allons tout d’abord vous de vous familiariser nous-mêmes avec le `Card` entité que nous allons utiliser tout au long de ce projet pour Explorer la `CCRenderTexture` classe. Le `Card` classe est une entité standard, suivant le modèle d’entité décrit dans le [guide de l’entité](~/graphics-games/cocossharp/entities.md). La classe de carte a tous ses composants visual (instances de `CCSprite` et `CCLabel`) répertorié en tant que champs :


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

Instances de la carte peuvent être rendus à l’aide un `CCRenderTexture`, ou en dessinant des individuellement chaque composant visuel. Bien que chaque composant est un objet indépendant, le `CCNode` apparenter système utilisé dans des entités rend le `Card` se comportent comme un objet unique : au moins la plupart du temps. Par exemple, si un `Card` entité est repositionnée, redimensionnée ou pivotée, puis tous les objets visuels sont affectées pour rendre la carte semble être un objet unique. Pour afficher les cartes se comportent comme un objet unique, nous pouvons modifier le `GameLayer.AddedToScene` méthode pour définir le `useRenderTextures` à la variable `false`:

    
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

- Les performances peuvent diminuer en raison de plusieurs appels de dessin
- Certains effets visuels, tels que la transparence, ne peuvent être implémentés correctement, comme nous explorerons plus tard


### <a name="card-draw-calls"></a>Appels de dessin de carte

Notre code est une simplification de ce qui peut être trouvé dans un intégral *jeu de cartes collectable* (CCG) tel que « Magic : la collecte » ou « Hearthstone ». Notre jeu affiche à la fois les trois cartes uniquement et a un petit nombre d’unités possibles (bleu, vert et orange). En revanche, un jeu complet peut avoir plus de vingt cartes à l’écran à un moment donné, et les joueurs peuvent avoir des centaines de cartes sélectionnables lors de la création de leurs jeux. Bien que notre jeu ne sont pas actuellement affectés par des problèmes de performances, un jeu complet de mise en œuvre similaire peut.

CocosSharp fournit des détails sur les performances de rendu en exposant les appels de dessin effectuées par trame. Notre `GameLayer.AddedToScene` méthode indique le `GameView.Stats.Enabled` à `true`, se traduisant par des informations de performances affichées en bas à gauche de l’écran :

![](ccrendertexture-images/image2.png "La méthode GameLayer.AddedToScene définit le GameView.Stats.Enabled sur true, ce qui entraîne des informations de performances affichées en bas à gauche de l’écran")

Notez que bien que trois cartes sur l’écran, nous les appels de dessin dix-neuf (résultats de chaque carte dans six appels de dessin, le texte affichant les comptes d’informations de performances pour un autre). Appels de dessin ont un impact significatif sur les performances d’un jeu, donc CocosSharp fournit plusieurs façons de les réduire. Une technique est décrite dans le [CCSpriteSheet guide](~/graphics-games/cocossharp/ccspritesheet.md). Une autre technique consiste à utiliser le `CCRenderTexture` afin de réduire chaque entité à un seul appel, comme nous allons examiner dans ce guide.


### <a name="card-transparency"></a>Transparence de la carte

Notre `Card` entité inclut un `Opacity` propriété transparence du contrôle comme indiqué dans l’extrait de code suivant :


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

Notez que la méthode setter prend en charge à l’aide de restituer des textures ou le rendu de chaque composant individuellement. Pour voir son effet, modifier la `opacity` valeur `127` (environ la moitié opacité) dans `GameLayer.AddedToScene` ce qui entraînera chaque composant ayant un `Opacity` valeur `127`:


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

Le jeu s’affiche maintenant les cartes avec certains transparence, provoquant la plus foncés dans la mesure où l’arrière-plan est noir :

![](ccrendertexture-images/image3.png "Le jeu s’affichent maintenant les cartes avec certains transparence, provoquant la plus foncés dans la mesure où l’arrière-plan est noir")

À première vue, il peut se présenter comme si nos cartes ont été correctement effectuées transparents. Toutefois, la capture d’écran affiche un nombre de problèmes visuels.

Étant donné que notre en arrière-plan est noir, nous pouvons prévoir que chaque partie de notre carte deviendrait plus sombre en raison de la transparence. Autrement dit, plus transparent devient d’une carte, devient sombre. Opacité 0, un `Card` est totalement transparent (totalement noir). Toutefois, certaines parties de notre carte n’est pas devenus plus sombre lors de l’opacité a été remplacée par `127`. Pire encore, certaines parties de notre carte réellement est devenu plus claires lorsqu’ils sont devenus plus transparentes. Examinons les parties de notre carte qui ont été noir *avant* qu’ils ont été transparents – en particulier le texte de détail et les contours noir autour du graphique monster. Si nous placer côte à côte, nous pouvons voir l’impact de l’application de la transparence :

![](ccrendertexture-images/image4.png "Si placées côte à côte, l’impact de l’application de la transparence peut être observé")

Comme mentionné ci-dessus, toutes les parties de la carte doivent devenir plus sombre lors de devenir plus transparents, mais dans un nombre de zones cela n’est pas le cas :

- Structure de du robot devient plus clair (allant du noir au gris, puis)
- Le texte de description devient plus clair (allant du noir au gris, puis)
- La composante verte de l’automate devienne moins saturée, mais ne devienne plus sombre

Pour mieux visualiser pourquoi cela se produit, nous devons n’oubliez pas que chaque composant visuel est dessiné indépendamment, chaque partiellement transparent. Le premier composant visual dessiné est en arrière-plan de la carte. Les éléments transparents suivants seront dessinés en haut de la carte et seront affectés par l’arrière-plan de la carte. Si nous supprimer du texte à partir de notre carte et déplacer le graphique de robot vers le bas, nous pouvons voir l’impact de l’arrière-plan de carte sur le robot. Notez que la ligne orange à partir de la zone supérieure peut être consultée sur le robot et que la zone de l’automate qui chevauche la rayure bleue dans le centre de la carte est dessinée plus sombre :

![](ccrendertexture-images/image5.png "Notez que la ligne orange à partir de la zone supérieure peut être consultée sur le robot et que la zone de l’automate qui chevauche la rayure bleue dans le centre de la carte est dessinée plus sombre")

À l’aide un `CCRenderTexture` permet de rendre la carte entière transparente sans affecter le rendu des composants individuels au sein de la carte, comme nous le verrons plus loin dans ce guide.


## <a name="using-ccrendertexture"></a>À l’aide de CCRenderTexture

Maintenant que nous avons identifié les problèmes de rendu de chaque composant individuellement, nous allons activer le rendu à un `CCRenderTexture` et comparer le comportement.

Pour activer le rendu à un `CCRenderTexture`, modifiez le `userRenderTextures` à la variable `true` dans `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>Appels de dessin de carte

Si nous exécutons le jeu maintenant, nous allons voir les appels de dessin réduits de dix-neuf à quatre (chaque carte réduit de six à une) :

![](ccrendertexture-images/image6.png "Si le jeu est exécuté maintenant, les appels de dessin réduit de dix-neuf à quatre chaque carte réduit de six à un")

Comme mentionné précédemment, ce type de la réduction peut avoir un impact significatif sur les jeux avec des entités visual supplémentaires sur l’écran.


### <a name="card-transparency"></a>Transparence de la carte

Une fois le `useRenderTextures` a la valeur `true`, cartes transparents aura un autre :

![](ccrendertexture-images/image7.png "Une fois que l’useRenderTextures est définie sur true, transparents cartes aura un autre")

Nous allons comparer la carte de robot transparent à l’aide de textures de rendu (à gauche) et sans (à droite) :

![](ccrendertexture-images/image8.png "Comparer la carte de robot transparent à l’aide de vs (gauche) de textures rendu sans (droite)")

Différences les plus évidentes sont dans le texte de détails (noir au lieu du gris clair) et le sprite de robot (foncé au lieu de la lumière et désaturée).


## <a name="ccrendertexture-details"></a>Détails de CCRenderTexture

Maintenant que nous avons vu les avantages de l’utilisation de `CCRenderTexture`, jetons un œil à la façon dont il est utilisé dans le `Card` entité.

Le `CCRenderTexture` est une zone qui peut être la cible de rendu. Il possède deux principales différences par rapport à l’écran de jeu :

1. Le `CCRenderTexture` persiste cadres intermédiaires. Cela signifie qu’un `CCRenderTexture` doit uniquement être affiché lorsque des modifications se produisent. Dans notre cas, le `Card` entité ne change jamais, donc il est rendu uniquement une seule fois. Le cas échéant `Card` composants modifié, puis la carte doit se redessiner à son `CCRenderTexture`. Par exemple, si la valeur de HP (points de contrôle d’intégrité) modifiée lorsqu’il est attaqué, la carte doit être rendue pour refléter la nouvelle valeur de HP.
1. Le `CCRenderTexture` dimensions en pixels ne sont pas liées à l’écran. Un `CCRenderTexture` peut être supérieure ou inférieure à la résolution de l’appareil. Le `Card` code crée un `CCRenderTexture` à l’aide de la taille du sprite de son en arrière-plan. La carte contient une référence à un `CCRenderTexture` appelée `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

Le `renderTexture` instance reste `null` jusqu'à ce que le `UseRenderTexture` propriété est affectée à true, qui appelle `SwitchToRenderTexture`:


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

Les sections suivantes examinent le `SwitchToRenderTexture` (méthode). 


### <a name="ccrendertexture-size"></a>Taille de CCRenderTexture

Le constructeur CCRenderTexture requiert deux ensembles de dimensions. Le premier contrôle la taille de la `CCRenderTexture` quand il est dessiné et que le deuxième spécifie la largeur en pixels et la hauteur de son contenu. Le `Card` entité instancie son `CCRenderTexture` à l’aide de l’arrière-plan [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Notre jeu a un `DesignResolution` de 512 par 384, comme indiqué dans `ViewController.LoadGame` sur iOS et `MainActivity.LoadGame` sur Android :


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

Le `CCRenderTexture` constructeur est appelé avec le `background.ContentSize` comme premier paramètre, ce qui indique que le `CCRenderTexture` ne doit pas être simplement comme arrière-plan `CCSprite`. Depuis l’arrière-plan de carte `CCSprite` est 200 pixels de hauteur, la carte occupera environ la moitié de la hauteur verticale de l’écran.

Le deuxième paramètre passé à la `CCRenderTexture` constructeur spécifie la résolution de pixel de la `CCRenderTexture`. Comme indiqué dans le [guide de résolution de CocosSharp](~/graphics-games/cocossharp/resolutions.md), la largeur et la hauteur de la zone visible en unités de jeu est souvent pas le même que la résolution en pixels de l’écran. De même, un CCRenderTexture peut utiliser une résolution supérieure à sa taille visuels apparaissent plus nettes sur les appareils haute résolution.

La résolution de pixel est deux fois la taille de la CCRenderTexture pour empêcher le texte de qualité sans aspect pixélisé :


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Pour comparer, nous pouvons modifier la valeur pixelResolution à faire correspondre le `background.ContentSize` (sans en cours a doublé rien) et comparer le résultat : 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Pour comparer, peut modifier la valeur de pixelResolution pour correspondre à l’arrière-plan. ContentSize sans être doublé et comparer le résultat")


### <a name="rendering-to-a-ccrendertexture"></a>Rendu à un CCRenderTexture

En règle générale, les objets visuels dans CocosSharp ne sont pas rendus explicitement. Au lieu de cela, les objets visuels sont ajoutés à un `CCLayer` qui fait partie d’un `CCScene`. CocosSharp affiche automatiquement le `CCScene` et sa hiérarchie d’objets visuel dans chaque frame sans aucun code de rendu qui est appelée. 

En revanche, la `CCRenderTexture` doit être dessiné explicitement à. Ce rendu peut être divisé en trois étapes :

1. `CCRenderTexture.BeginWithClear` est appelée, indiquant que tout le rendu suivantes cibleront l’appel `CCRenderTexture`.
1. Objets héritant de `CCNode` (comme le `Card` entité) sont affichées sur le `CCRenderTexture` en appelant `Visit`.
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

Le `renderTexture` ne doivent pas faire partie de la carte lors du rendu, donc elle est supprimée :


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Maintenant le `Card` instance peut se rendre à la `CCRenderTexture` instance :


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

Ce guide a couvert la `CCRenderTexture` classe en utilisant un `Card` entité qui pouvait être utilisée dans un jeu de cartes pouvant être collecté. Il vous a montré comment utiliser le `CCRenderTexture` classe pour améliorer la fréquence d’images et de mise en œuvre la transparence à l’échelle de l’entité.

Bien que ce guide utilisé un `CCRenderTexture` contenues dans une entité, cette classe peut être utilisé pour restituer plusieurs entités, ou même toute `CCLayer` instances pour les effets de l’écran à l’échelle et de performances.

## <a name="related-links"></a>Liens associés

- [Référence de l’API CCRenderTexture](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Projet complet (exemple)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
