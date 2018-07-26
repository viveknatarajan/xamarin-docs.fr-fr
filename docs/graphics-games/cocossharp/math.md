---
title: Mathématiques 2D avec CocosSharp
description: Ce guide couvre les mathématiques 2D pour le développement de jeux. Il utilise CocosSharp de montrer comment effectuer des tâches courantes de développement de jeux et explique les calculs derrière ces tâches.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 63c722b74c7dc7e034475e539f38204aca87763e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242051"
---
# <a name="2d-math-with-cocossharp"></a>Mathématiques 2D avec CocosSharp

_Ce guide couvre les mathématiques 2D pour le développement de jeux. Il utilise CocosSharp de montrer comment effectuer des tâches courantes de développement de jeux et explique les calculs derrière ces tâches._

Pour positionner et déplacer les objets avec du code est un élément fondamental du développement de jeux de toutes tailles. Positionnement et le déplacement nécessitent l’utilisation de mathématiques, si un jeu requiert le déplacement d’un objet le long d’une ligne droite, ou l’utilisation de trigonométrie pour la rotation. Ce document couvre les rubriques suivantes :

 - Rapidité
 - Accélération
 - Faire pivoter des objets de CocosSharp
 - À l’aide de la rotation avec rapidité

Les développeurs n’ont pas un arrière-plan mathématiques fort, ou qui ont oublié long ces rubriques à partir de l’école, n’êtes pas obligé de vous inquiétez pas : ce document cesse de fonctionner concepts en éléments addictive et accompagne les explications théoriques avec des exemples concrets. En bref, cet article répondront à la question de student mathématiques vieux : « Lorsque j’ai réellement devrez utiliser ce sujet ? »


## <a name="requirements"></a>Configuration requise

Bien que ce document se concentre essentiellement sur le côté mathématique de CocosSharp, exemples de code partent du principe utilisation des objets qui héritent de formulaire `CCNode`. En outre, depuis `CCNode` n’inclut pas de valeurs pour la vitesse et l’accélération, le code part du principe de l’utilisation des entités qui fournissent des valeurs telles que VelocityX, VelocityY, AccelerationX et AccelerationY. Pour plus d’informations sur les entités, consultez notre procédure pas à pas sur [entités dans CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Rapidité

Les développeurs de jeux utilisent le terme *velocity* pour décrire la façon dont un objet se déplace – en particulier, la rapidité, quelque chose se déplace et la direction qu’elle se déplace. 

Rapidité est définie à l’aide de deux types d’unités : une unité de position et une unité de temps. Par exemple, la vitesse d’une voiture est définie comme kilomètres / heure ou en kilomètres par heure. Développeurs de jeux déplace souvent utiliser pixels par seconde à définir la rapidité d’objet. Par exemple, une liste à puces peuvent se déplacer à la vitesse de 300 pixels par seconde. Autrement dit, si une puce se déplace à 300 pixels par seconde, il sera ont déplacé 600 unités dans les deux secondes et les 900 unités dans trois secondes et ainsi de suite. En règle générale, la valeur de la vitesse *ajoute* à la position d’un objet (comme nous allons le voir ci-dessous).

Bien que nous avons utilisé la vitesse d’expliquer les unités de vitesse, la vitesse de terme désigne généralement à la valeur est indépendante de la direction, tandis que la vitesse de terme fait référence à la vitesse et la direction. Par conséquent, l’affectation de la vitesse d’une puce (en supposant que puce est une classe qui inclut les propriétés nécessaires) peut ressembler à ceci :


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Implémentation de rapidité

CocosSharp n’implémente pas de rapidité, sorte que les objets nécessitant le déplacement devront implémenter leur propre logique de déplacement. Les développeurs de jeux nouvelle implémentation velocity souvent commettent l’erreur consistant à leur rapidité dépend de la fréquence d’images. Autrement dit, ce qui suit *implémentation incorrecte* paraîtra pour fournir des résultats corrects, mais doit reposer sur la fréquence d’images du jeu :

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Si le jeu s’exécute une fréquence plus élevée (par exemple, 60 images par seconde au lieu de 30 images par seconde), l’objet s’affiche pour déplacer plus rapidement que si en cours d’exécution à une fréquence d’images plus lente. De même, si le jeu est incapable de traiter les trames en aussi élevé d’une fréquence d’images (ce qui peut être dû à des processus en arrière-plan à l’aide des ressources du périphérique), le jeu semble ralentir.

Pour éviter cela, rapidité est souvent implémentée à l’aide d’une valeur d’heure. Par exemple, si le `seconds` variable représente le nombre (ou fraction) de secondes depuis la dernière rapidité de temps a été appliquée, puis le code suivant entraînerait l’objet d’un déplacement des cohérente, quel que soit la fréquence d’images :

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Considérez qu’un jeu qui s’exécute une fréquence inférieure met à jour la position de ses objets moins fréquemment. Par conséquent, chaque mise à jour entraîne les déplacement plus qu’il le ferait si le jeu a été mise à jour plus fréquemment les objets. Le `seconds` valeur comptes pour ce faire, en signalant combien de temps écoulé depuis la dernière mise à jour.

Pour obtenir un exemple montrant comment ajouter des temps de déplacement, consultez [cette recette couvrant le temps en fonction de déplacement](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement).


### <a name="calculating-positions-using-velocity"></a>Positions à l’aide de la vitesse de calcul

Rapidité peut servir à effectuer des prévisions sur où un objet sera après une certaine quantité de délai s’écoule, ou pour aider à ajuster le comportement des objets sans avoir à exécuter le jeu. Par exemple, un développeur qui implémente le déplacement d’une puce déclenchée doit définir la rapidité de la puce après son instanciation. La taille d’écran peut être utilisée pour fournir une base pour la définition de rapidité. Autrement dit, si le développeur sait que la puce doit déplacer la hauteur de l’écran en 2 secondes, la vitesse doit être définie sur la hauteur de l’écran divisé par 2. Si l’écran est de 800 pixels de hauteur, puis la vitesse de la puce est définie à 400 (c'est-à-dire 800/2).

De même, dans le jeu logique peut-être calculer la durée pendant laquelle un objet prendra d’atteindre une destination étant donnée sa rapidité. Cela peut être calculée en divisant la distance à parcourir par vitesse de déplacement de l’objet. Par exemple, le code suivant montre comment assigner à une étiquette qui affiche la durée pendant laquelle le texte jusqu'à ce qu’un missile atteigne sa cible :


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Accélération

*Accélération* est un concept commun dans le développement de jeux et de nombreuses similitudes avec rapidité. Accélération quantifie si un objet est accélérer ou ralentir (comment la valeur de la vitesse change au fil du temps). Accélération *ajoute* à la rapidité, tout comme la vélocité ajoute à la position. Les applications courantes de l’accélération incluent gravité, une voiture en accélérant l’et un navire espace déclenche ses systèmes de poussée. 

Semblable à la vélocité, l’accélération est définie dans une position et une unité de temps ; Toutefois, unité de temps d’accélération est appelée un *au carré* unité, ce qui reflète l’accélération de définition de point de vue mathématique. Autrement dit, l’accélération de jeu est souvent mesurée en *pixels par seconde carré*.

Si un objet possède une accélération X 10 unités par seconde carré, cela signifie que sa rapidité augmenteront de 10 chaque seconde. Si le démarrage à partir d’un arrêt, après qu’une seconde il doit être mis au 10 unités par seconde, après deux secondes 20 unités par seconde, et ainsi de suite.

L’accélération dans deux dimensions requiert un composant X et Y, donc il peut être affecté comme suit :


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Accélération et la décélération

Bien que l’accélération et la décélération sont parfois différenciés dans chaque jour vocale, il n’existe aucune différence technique entre les deux. La gravité est une force de ce qui entraîne l’accélération. Si un objet est levé vers le haut puis gravité ralentit il (décélération), mais une fois que l’objet a cessé d’escalade et est compris dans la même direction que gravité puis gravité accélère l’il (accélérer). Comme illustré ci-dessous, l’application d’une accélération est si elle est appliquée dans la même direction ou opposé direction du mouvement. 


### <a name="implementing-acceleration"></a>Implémentation d’accélération

L’accélération est semblable à la vélocité lors de l’implémentation : il n’est pas automatiquement implémentée par CocosSharp et accélération temporelle est l’implémentation de votre choix (par opposition à accélération frame). Par conséquent, une implémentation simple d’accélération (ainsi que la vitesse) peut ressembler à :

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Le code ci-dessus est ce qui est appelé un *approximation linéaire* pour l’implémentation de l’accélération. En effet, il implémente l’accélération avec un degré de fermer assez de précision, mais il n’est pas un modèle parfaitement précis de l’accélération. Il est inclus ci-dessus afin d’expliquer le concept de la façon dont l’accélération est implémentée.

L’implémentation suivante est une application mathématiquement exacte d’accélération et de vitesse :


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

La différence la plus évidente pour le code ci-dessus est le `halfSecondsSquared` variable et son utilisation à appliquer pour positionner l’accélération. Les mathématiques fait dépasse le cadre de ce didacticiel, mais les développeurs intéressés par les calculs derrière cette trouverez plus d’informations dans [cette discussion sur l’intégration de l’accélération.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

L’impact pratique de `halfSecondSquare` est que l’accélération se comportera mathématiquement avec précision et de manière prévisible, quel que soit la fréquence d’images. L’approximation linéaire de l’accélération est soumise à la fréquence d’images – faible la fréquence d’image descend devient de moins précise l’approximation. À l’aide de `halfSecondsSquared` garanties que code se comporte les mêmes, quel que soit la fréquence d’images.


## <a name="angles-and-rotation"></a>Angles et rotation

Objets tels que des visuels `CCSprite` prennent en charge la rotation via un `Rotation` variable. Cela peut être assignée à une valeur pour définir sa rotation en degrés. Par exemple, le code suivant montre comment faire pivoter un `CCSprite` instance :


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

Voici le résultat :

![](math-images/image1.png "Il en résulte dans cette capture d’écran")

Notez que la rotation est de 45 degrés dans le sens horaire (qui, pour des raisons historiques, est l’opposé de la rotation est appliquée mathématiquement) :

![](math-images/image2.png "Notez que la rotation de 45 degrés dans le sens horaire qui pour des raisons historiques est l’opposé de la rotation est appliquée mathématiquement")

En général la rotation de CocosSharp et mathématiques standard peut être visualisée comme suit :

![](math-images/image3.png "En général la rotation de CocosSharp et en mathématiques standard peut être visualisée comme ceci")

![](math-images/image4.png "En général la rotation de CocosSharp et en mathématiques standard peut être visualisée comme ceci")

Cette distinction est importante, car le `System.Math` classe utilise une rotation, donc les développeurs familiarisés avec cette classe inverser les angles lorsque vous travaillez avec `CCNode` instances.

Il nous faut remarquer que les diagrammes ci-dessus affichent la rotation en degrés ; Toutefois, certaines fonctions mathématiques (telles que les fonctions dans le `System.Math` espace de noms) attendent et retourner des valeurs dans *radians* au lieu de degrés. Nous allons examiner comment effectuer une conversion entre les types de deux unités un peu plus loin dans ce guide.


### <a name="rotating-to-face-a-direction"></a>Rotation pour faire face à une direction

Comme indiqué ci-dessus, `CCSprite` peut être pivoté à l’aide de la `Rotation` propriété. Le `Rotation` propriété est fournie par `CCNode` (classe de base pour `CCSprite`), ce qui signifie que la rotation peut être appliquée aux entités qui hérite de `CCNode` également. 

Certains jeux nécessite des objets à faire pivoter afin qu’ils courent une cible. Exemples incluent un ennemi contrôlée par l’ordinateur à une cible de lecteur, ou d’un navire espace battant vers le point où l’utilisateur touche l’écran. Toutefois, une valeur de rotation doit tout d’abord être dépend de l’emplacement de l’entité en rotation et l’emplacement de la cible à relever.

Ce processus requiert plusieurs étapes :

 - Identifiant la *décalage* de la cible. Offset fait référence à la distance de X et Y entre l’entité de la rotation et de l’entité cible.
 - Calcul de l’angle à partir de l’offset en utilisant la fonction de trigonométrie arc tangente (expliquée en détail ci-dessous).
 - Ajustement d’une différence entre 0 degré pointant vers la droite et la direction dans laquelle l’entité de la rotation des points lorsque non pivoté.
 - Ajustement de la différence entre la rotation mathématique (dans le sens inverse) et de rotation de CocosSharp (dans le sens horaire).

La fonction suivante (supposée être écrit dans une entité) fait pivoter l’entité pour faire face à une cible :


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

Le code ci-dessus peut être utilisé pour faire pivoter une entité, donc il est confronté le point où l’utilisateur est toucher l’écran, comme suit :


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

Ce code génère le comportement suivant :

![](math-images/image5.gif "Ce code génère ce comportement")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>À l’aide de Atan2 convertir aux offsets angles

`System.Math.Atan2` peut être utilisé pour convertir un décalage d’un angle. Le nom de fonction `Atan2` provient de l’arc tangente de fonctions trigonométriques. Le suffixe « 2 » différencie cette fonction à partir de la norme `Atan` (fonction), ce qui est strictement correspond au comportement mathématique de l’arc tangente. Arc tangente est une fonction qui retourne une valeur comprise entre -90 et + 90 degrés (ou l’équivalent en radians). De nombreuses applications, y compris les jeux de l’ordinateur, nécessitent souvent 360 degrés des valeurs, de sorte que la `Math` classe inclut `Atan2` pour répondre à ce besoin.

Notez que le code ci-dessus passe le paramètre Y tout d’abord, puis le paramètre X, lorsque vous appelez le `Atan2` (méthode). Il s’agit en arrière à partir de X, Y classement des coordonnées de position habituelles. Pour plus d’informations [, consultez la Atan2](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx).

Il est également important de noter que la valeur de retour à partir de `Atan2` est exprimé en radians, qui est une autre unité utilisée pour mesurer les angles. Ce guide ne couvre les détails de radians, mais n’oubliez pas que toutes les fonctions trigonométriques dans le `System.Math` espace de noms utiliser radians, toutes les valeurs doivent être converti en degrés avant d’être utilisées sur les objets de CocosSharp. Vous pouvez trouver plus d’informations sur radians [dans la page de Wikipedia radian](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>Angle vers l’avant

Une fois le `FacePoint` méthode convertit l’angle en radians, il définit un `forwardAngle` valeur. Cette valeur représente l’angle dans lequel l’entité est face lorsque sa valeur de Rotation est égal à 0. Dans cet exemple, nous partons du principe que l’entité est face vers le haut, ce qui est de 90 degrés lors de l’utilisation d’une rotation mathématique (par opposition à la rotation de CocosSharp). Nous utilisons la permutation mathématique ici dans la mesure où nous n’avons pas encore inversé la rotation de CocosSharp.

L’exemple suivant montre quelles une entité avec un `forwardAngle` de 90 degrés peut ressembler à :

![](math-images/image6.png "Voici à quoi peut ressembler une entité avec un forwardAngle de 90 degrés")


### <a name="angled-velocity"></a>Rapidité d’angle

Jusqu'à présent, nous avons examiné comment convertir un décalage en un angle. Cette section passe l’inverse : prend un angle et le convertit en X et les valeurs Y. Voici quelques exemples : une voiture déplacement dans la direction dans laquelle elle est face, ou un navire espace filmer une puce qui se déplace dans la direction dans laquelle le destinataire est confronté. 

Conceptuellement, rapidité peut être calculée en premier définissant la vélocité souhaitée lorsque non pivoté, puis faire pivoter cette rapidité de l’angle de face d’une entité. Pour expliquer ce concept, rapidité (et l’accélération) peuvent être visualisées en tant que dimensions 2 *vecteur* (qui est généralement dessiné comme une flèche). Un vecteur pour une valeur de la vitesse avec X = 100 et Y = 0 peut être visualisée comme suit :

![](math-images/image7.png "Un vecteur pour une valeur de la vitesse avec X = 100 et Y = 0 peut être visualisée comme suit")

Ce vecteur peut être pivoté pour parvenir à une vitesse de nouveau. Par exemple, faire pivoter le vecteur de 45 degrés (à l’aide d’une rotation) génère les éléments suivants :

![](math-images/image8.png "Faire pivoter le vecteur de 45 degrés à l’aide des résultats d’une rotation dans ce")

Heureusement, rapidité, accélération et la même position peuvent tous pivoter avec le même code, quel que soit la façon dont les valeurs sont appliquées. La fonction suivante à usage général peut être utilisée pour faire pivoter un vecteur par une valeur de Rotation de CocosSharp :


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

Une compréhension complète de la `RotateVector` méthode exige d’être familiarisé avec les fonctions trigonométriques cosinus et sinus, ainsi que certains d’algèbre linéaire, ce qui dépasse le cadre de cet article. Toutefois, une fois implémenté le `RotateVector` méthode peut être utilisée pour faire pivoter des vecteurs, y compris un vecteur de rapidité. Par exemple, le code suivant peut déclencher une puce dans une direction spécifiée par la `rotation` valeur :


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

Ce code peut produire quelque chose comme :

![](math-images/image9.png "Ce code peut produire quelque chose comme cette capture d’écran")


## <a name="summary"></a>Récapitulatif

Ce guide explique les concepts mathématiques courants dans le développement de jeux 2D. Il montre comment assigner et mettre en œuvre de la vitesse et l’accélération et explique comment faire pivoter des objets et des vecteurs de mouvement dans n’importe quelle direction.
