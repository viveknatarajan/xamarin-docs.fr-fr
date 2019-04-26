---
title: Animation avec CCAction
description: La classe CCAction simplifie l’ajout d’animations aux jeux de CocosSharp. Ces animations peuvent servir pour implémenter la fonctionnalité ou à peaufiner l’aspect.
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: c486bb2e78579360e0f935219cd82958fedee34b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357701"
---
# <a name="animating-with-ccaction"></a>Animation avec CCAction

_La classe CCAction simplifie l’ajout d’animations aux jeux de CocosSharp. Ces animations peuvent servir pour implémenter la fonctionnalité ou à peaufiner l’aspect._

`CCAction` est une classe de base qui peut être utilisée pour animer des objets de CocosSharp. Ce guide couvre intégré `CCAction` implémentations pour les tâches courantes telles que le positionnement, la mise à l’échelle et la rotation. Il examine également comment créer des implémentations personnalisées en héritant de `CCAction`.

Ce guide utilise un projet appelé **ActionProject** qui [peut être téléchargé ici](https://developer.xamarin.com/samples/mobile/CCAction). Ce guide utilise les `CCDrawNode` (classe), qui est abordé dans le [dessin une géométrie avec CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md) guide.


## <a name="running-the-actionproject"></a>Le ActionProject en cours d’exécution

**ActionProject** est une solution de CocosSharp qui peut être générée pour iOS et Android. Il sert à la fois comme un exemple de code pour savoir comment utiliser le `CCAction` classe et en guise de démonstration en temps réel de commun `CCAction` implémentations.

Lors de l’exécution, ActionProject affiche trois `CCLabel` instances sur la gauche de l’écran et un objet visuel est dessiné par deux `CCDrawNode` instances pour afficher les différentes actions :

![](ccaction-images/image1.png "ActionProject affiche trois instances CCLabel sur la gauche de l’écran et un objet visuel est dessiné par deux instances CCDrawNode pour afficher les différentes actions")

Les étiquettes sur la gauche indiquent le type de `CCAction` sera créé lors de l’appui dans l’écran. Par défaut, le **Position** valeur est sélectionnée, ce qui entraîne un `CCMove` action qui est créée et appliquée pour le cercle rouge :

![](ccaction-images/image2.gif "La valeur de Position est sélectionnée, ce qui entraîne une action CCMove qui est créé et appliqué pour le cercle rouge")

En cliquant sur les étiquettes sur la gauche change le type de `CCAction` est effectuée sur le cercle. Par exemple, en cliquant sur le **Position** étiquette passera en revue les différentes valeurs qui peuvent être modifiés :

![](ccaction-images/image3.gif "En cliquant sur l’étiquette de Position de passer en revue les différentes valeurs qui peuvent être modifiés")

## <a name="common-variable-changing-ccaction-classes"></a>Classes de CCAction de modification de variable communes

Le **ActionProject** utilise les éléments suivants `CCAction`-héritant des classes qui font partie de CocosSharp :

 - `CCMoveTo` : Modifie un `CCNode` l’instance `Position` propriété
 - `CCScaleTo` : Modifie un `CCNode` l’instance `Scale` propriété
 - `CCRotateTo` : Modifie un `CCNode` l’instance `Rotation` propriété

Ce guide fait référence à ces actions en tant que *modification de variable*, ce qui signifie qu’ils un impact direct sur la variable de la `CCNode` qui ils sont ajoutés à. Autres types d’actions sont appelées *d’accélération* actions qui sont traitées plus loin dans ce guide.

Le **ActionProject** montre que l’objectif de ces actions consiste à modifier une variable au fil du temps. Plus précisément, ces `CCActions` constructeurs prennent deux arguments : durée d’exécution à prendre et la valeur à assigner. Le fragment de code suivant montre comment les trois types d’actions sont créés :


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

Une fois que l’action est créée, il est ajouté à un CCNode comme suit :


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` démarre le `CCAction` comportement de l’instance et il effectuera automatiquement sa logique après-image jusqu'à la fin.

Chacun des types répertoriés ci-dessus se termine par le mot *à* ce qui signifie que le `CCAction` modifiera le `CCNode` afin que la valeur d’argument représente l’état final lorsque l’action est terminée. Par exemple, créez un `CCMoveTo` avec une position x = 100 et Y = 200 génère le `CCNode` l’instance `Position` définie sur X = 100, Y = 200 à la fin de l’heure spécifiée, quelle que soit la `CCNode` emplacement du début de l’instance.

Chaque classe « To » a également une version « Par », qui ajoute la valeur d’argument à la valeur actuelle sur le `CCNode`. Par exemple, créez un `CCMoveBy` avec une position x = 100 et Y = 200 entraîne la `CCNode` instance déplacée aux unités de droite 100 et 200 unités de la position qu’il était au démarrage de l’action.


## <a name="easing-actions"></a>Accélération des actions

Par défaut, les actions de modification de variable effectuera *une interpolation linéaire* – l’action se déplace vers la valeur souhaitée à une vitesse constante. Si l’interpolation *position* linéairement, l’objet animé immédiatement démarre et arrête le déplacement au début et à la fin de l’action, et sa vitesse reste constante pendant l’exécution de l’action. 

Interpolation non linéaire est moins souffrent et ajoute un élément de polonais, afin de CocosSharp offre une variété de l’accélération des actions qui peuvent être utilisées pour modifier les actions de modification de variable.

Dans le **ActionProject** exemple, nous pouvons basculer entre ces types d’accélération d’actions en cliquant sur le deuxième contrôle label (qui utilise par défaut **<None>**) :

![](ccaction-images/image4.gif "L’utilisateur peut basculer entre ces types d’accélération d’actions en cliquant sur le deuxième contrôle label")

Accélération des actions sont particulièrement puissantes, car ils ne sont pas liés à une action de paramètre de variable particulier. Cela signifie que la même action accélération peut être utilisée pour affecter la position, de rotation, de mise à l’échelle ou d’actions personnalisées (tel qu’il apparaîtra plus loin dans ce guide).

Actions accélération encapsulent les actions de paramètre de variable (tant que l’action du paramètre de la variable hérite `CCFiniteTimeAction`) en acceptant une action de paramètre de la variable en tant qu’argument dans leurs constructeurs.

Par exemple, si les étiquettes sont définies sur **Position**, **CCEaseElastic**, puis le code suivant s’exécute lorsqu’une pression tactile est détectée (Notez que le code a été omis pour mettre en surbrillance les lignes correspondantes) :


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

Comme indiqué par l’application, l’accélération même exacte peut être appliqué à autres actions de paramètre de variable comme `CCRotateTo`:

![](ccaction-images/image5.gif "L’accélération même exacte peut être appliqué aux autres actions de paramètre de variable comme CCRotateTo")


## <a name="easing-in-out-and-inout"></a>In, Out et InOut d’accélération

Toutes les actions d’accélération avoir `In`, `Out`, ou `InOut` ajouté pour le type d’accélération. Ces termes font référence aux lorsque l’accélération est appliquée : `In` signifie d’accélération s’appliqueront au début, `Out` signifie à la fin, et `InOut` signifie à la fois au début et fin.

Un `In` accélération action aura un impact sur la façon d’une variable est appliquée tout au long de l’interpolation entier (à la fois au début et à la fin), mais généralement les caractéristiques plus identifiables de l’action d’accélération aura lieu au début. De même, `Out` actions accélération sont caractérisent par leur comportement à la fin de l’interpolation. Par exemple, `CCEaseBounceOut` aboutit à un objet rebondissent pas à la fin de l’action.


### <a name="out"></a>Out

`Out` en général d’accélération s’applique les modifications majeures à la fin de l’interpolation. Par exemple, `CCEaseExponentialOut` ralentira le taux de variation de la variable variation lorsqu’elle approche de la valeur cible :

![](ccaction-images/image6.gif "CCEaseExponentialOut ralentira le taux de variation de la variable variation lorsqu’elle approche de la valeur cible")


### <a name="in"></a>Vers l'avant

`In` en général d’accélération s’applique la modification la plus remarquable au début de l’interpolation. Par exemple, `CCEaseExponentialIn` déplacera plus lentement au début de l’action :

![](ccaction-images/image7.gif "CCEaseExponentialIn déplacera plus lentement au début de l’action")


### <a name="inout"></a>InOut

`InOut` s’applique généralement les modifications majeures à la fois au début et fin. `InOut` l’accélération est habituellement symétrique. Par exemple, `CCEaseExponentialInOut` déplacera lentement au début et à la fin de l’action :

![](ccaction-images/image8.gif "CCEaseExponentialInOut déplacera lentement au début et à la fin de l’action")


## <a name="implementing-a-custom-ccaction"></a>Implémentation d’une CCAction personnalisée

Toutes les classes que nous avons abordés jusqu'à présent sont inclus dans CocosSharp pour fournir des fonctionnalités communes. Custom `CCAction` implémentations peuvent fournir davantage de flexibilité. Par exemple, un `CCAction` qui contrôle le ratio rempli d’une barre de l’expérience peut être utilisé afin que la barre de l’expérience s’agrandit normalement chaque fois que l’utilisateur reçoit l’expérience.

`CCAction` implémentations nécessitent généralement deux classes :

 - `CCFiniteTimeAction` implémentation, la classe d’action de temps finie est responsable du démarrage de l’action. C’est la classe qui est instanciée et soit ajouté directement à un `CCNode` ou à une action d’accélération. Il doit instancier et retourner un `CCFiniteTimeActionState`, qui effectue les mises à jour.
 - `CCFiniteTimeActionState` implémentation, la classe d’état de temps finie action est responsable de la mise à jour les variables impliquées dans l’action. Elle doit implémenter une fonction de mise à jour, ce qui attribue la valeur sur la cible en fonction de la valeur d’heure. Cette classe n’est pas explicitement référencée en dehors de la `CCFiniteTimeAction` qui le crée. Il fonctionne simplement « les coulisses ».

**ActionProject** fournit un personnalisé `CCFiniteTimeAction` implémentation appelée `LineWidthAction,` qui est utilisé pour ajuster la largeur de la ligne jaune dessinée sur le cercle rouge. Notez que sa seule tâche consiste à instancier et retourner un `LineWidthState` instance :


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

Comme mentionné ci-dessus, le `LineWidthState` effectue le travail de l’affectation de la ligne `Width` propriété en fonction de la quantité `time` a passé :


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

La LineWidthAction peut être combinée avec toute action accélération pour modifier la largeur de ligne de différentes manières, comme indiqué dans l’animation suivante :

![](ccaction-images/image9.gif "La LineWidthAction peut être combinée avec toute action accélération pour modifier la largeur de ligne de différentes manières, comme indiqué dans cette animation")


### <a name="interpolation-and-the-update-method"></a>Interpolation et la méthode de mise à jour

La logique uniquement, à part le stockage de valeurs dans les classes qui précède, se trouve dans le `LineWidthState.Update` (méthode). Le `startWidth` variable stocke la largeur de la cible `LineNode` au début de l’action et le `deltaWidth` variable stocke la valeur de la quantité est modifiée au cours de l’action.

En remplaçant le `time` variable avec une valeur égale à 0, nous pouvons voir que la cible `LineNode` sera à sa position de départ :


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

De même, nous pouvons voir que la cible `LineNode` sera en remplaçant la variable d’heure avec une valeur de 1 à sa destination :


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

Le `time` valeur sera généralement comprise entre 0 et 1 - mais pas toujours - et `Update` implémentations ne doivent pas présumer ces limites. Certaines méthodes d’accélération (tel que `CCEaseBackIn` et `CCEaseBackOut`) fournit une valeur d’heure en dehors de la plage de 0 à 1.


## <a name="conclusion"></a>Conclusion

Interpolation et en facilitant la sont un élément essentiel de la création d’un jeu très soigné, en particulier lors de la création d’interfaces utilisateur. Ce guide explique comment utiliser `CCActions` pour interpoler les valeurs standard telles que la rotation et de position, ainsi que des valeurs personnalisées. Le `LineWidthState` et `LineWidthAction` classes montrent comment implémenter une action personnalisée.

## <a name="related-links"></a>Liens connexes

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [Exemple complet](https://developer.xamarin.com/samples/mobile/CCAction/)
