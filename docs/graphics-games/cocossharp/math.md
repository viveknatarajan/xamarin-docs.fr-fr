---
title: Mathématiques 2D avec CocosSharp
description: Ce guide couvre 2D mathématiques pour le développement de jeux. Il utilise CocosSharp pour montrer comment effectuer des tâches courantes de développement de jeux et explique les mathématiques qui sous-tendent ces tâches.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: ae1300936a24ac1381496eaaf78aefb875bd5ed6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="2d-math-with-cocossharp"></a>Mathématiques 2D avec CocosSharp

_Ce guide couvre 2D mathématiques pour le développement de jeux. Il utilise CocosSharp pour montrer comment effectuer des tâches courantes de développement de jeux et explique les mathématiques qui sous-tendent ces tâches._

Pour positionner et déplacer des objets avec le code est un élément essentiel de développement de jeux de toutes tailles. Positionnement et le déplacement de nécessitent l’utilisation de mathématiques, si un jeu requiert le déplacement d’un objet le long d’une ligne droite, ou l’utilisation de trigonométrie de rotation. Ce document couvre les rubriques suivantes :

 - Rapidité
 - Accélération
 - Rotation des objets CocosSharp
 - À l’aide de la rotation avec rapidité

Les développeurs n’ont pas un arrière-plan mathématiques fort, ou qui ont oublié long ces rubriques à partir de l’école, n’avez pas besoin de vous inquiétez pas : ce document sera décomposer concepts en assez courtes fragments et décrit les explications théoriques avec des exemples pratiques. En résumé, cet article sera de question mathématiques vieux étudiant : « Lorsque réellement dois-je utiliser Ceci ? »


## <a name="requirements"></a>Spécifications

Bien que ce document se concentre essentiellement sur le côté mathématique de CocosSharp, les exemples de code supposent utilisation des objets héritant du formulaire `CCNode`. En outre, depuis `CCNode` n’inclut pas de valeurs pour la vitesse et l’accélération, le code suppose que le travail avec les entités qui fournissent des valeurs telles que VelocityX, VelocityY, AccelerationX et AccelerationY. Pour plus d’informations sur les entités, consultez notre procédure pas à pas sur [entités dans CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Rapidité

Chez les développeurs utilisent le terme *rapidité* pour décrire la façon dont un objet se déplacent – plus précisément, la rapidité de quelque chose se déplacent et la direction qu’elle se déplace. 

Rapidité est définie à l’aide de deux types d’unités : une unité de position et une unité de temps. Par exemple, la vitesse d’une voiture est définie comme miles par heure ou kilomètres par heure. Aux développeurs de jeux souvent utiliser pixels par seconde à définir la rapidité d’objet se déplace. Par exemple, une puce peut déplacer à une vitesse de 300 pixels par seconde. Autrement dit, si une puce se déplace à 300 pixels par seconde, il sera ont déplacé 600 unités dans les deux secondes et les 900 unités dans trois secondes et ainsi de suite. En règle générale, la valeur de la vitesse *ajoute* à la position d’un objet (comme nous allons le voir ci-dessous).

Bien que nous avons utilisé la vitesse d’expliquer les unités de la vitesse, la vitesse de terme désigne généralement à une valeur indépendante de la direction, alors que la vitesse du terme fait référence à la vitesse et la direction. Par conséquent, l’affectation de la vitesse d’une puce (en supposant la puce est une classe qui inclut les propriétés nécessaires) peut ressembler à ceci :


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Implémentation de rapidité

CocosSharp n’implémente pas rapidité, les objets nécessitant une mouvements devez implémenter leur propre logique de déplacement. Nouveaux développeurs jeu implémentation rapidité souvent commettent l’erreur de rendre leur vitesse dépend de la fréquence d’images. Autrement dit, ce qui suit *implémentation incorrecte* semblent pour fournir des résultats corrects, mais doit reposer sur la fréquence d’images du jeu :

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Si le jeu exécute une fréquence plus élevée (par exemple, 60 images par seconde, au lieu de 30 images par seconde), l’objet s’affiche pour déplacer plus rapidement que si en cours d’exécution à une fréquence d’images plus lente. De même, si le jeu est incapable de traiter des images au plus haut d’une fréquence d’images (qui peut être dû à des processus en arrière-plan à l’aide des ressources du périphérique), le jeu semble ralentir.

Pour prendre en compte, rapidité est souvent implémentée à l’aide d’une valeur d’heure. Par exemple, si le `seconds` variable représente le nombre ou les fraction de secondes depuis la dernière rapidité de temps a été appliquée, le code suivant génère dans l’objet de mouvement cohérent, quelle que soit la fréquence d’images :

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Considérez qu’un jeu qui s’exécute à une cadence met à jour la position de ses objets moins fréquemment. Par conséquent, chaque mise à jour génère les objets déplacement plus qu’ils le feraient si le jeu a été mise à jour plus fréquemment. Le `seconds` des comptes de valeur pour ce faire, par rapport à la durée écoulée depuis la dernière mise à jour.

Pour obtenir un exemple montrant comment ajouter des temps de déplacement, consultez [cette recette qui couvrent le temps en fonction de déplacement](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/).


### <a name="calculating-positions-using-velocity"></a>Calcul des positions à l’aide de la vélocité

Rapidité peut servir à effectuer des prévisions sur où un objet sera après une certaine quantité de temps, ou pour faciliter l’ajustement du comportement des objets sans avoir à exécuter le jeu. Par exemple, un développeur qui implémente le déplacement d’une puce déclenché doit définir la rapidité de la puce après que qu’il est instancié. La taille d’écran peut servir à fournir une base pour la définition de rapidité. Autrement dit, si le développeur sait que la puce doit déplacer la hauteur de l’écran en 2 secondes, puis la rapidité doit être définie sur la hauteur de l’écran divisé par 2. Si l’écran est 800 pixels de hauteur, la vitesse de la puce est définie à 400 (qui est le 800/2).

De même, la logique du jeu, peut-être calculer la durée pendant laquelle un objet prendra pour parvenir à une destination donnée sa rapidité. Cela peut être calculée en divisant la distance à parcourir par la rapidité de déplacement de l’objet. Par exemple, le code suivant montre comment assigner à une étiquette qui affiche la durée pendant laquelle le texte jusqu'à ce qu’un missile atteint sa cible :


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Accélération

*Accélération* est un concept courants dans le développement de jeux et de nombreuses similitudes avec rapidité. Accélération quantifie si un objet est accélérer ou ralentir (comment la valeur de la vitesse change au fil du temps). Accélération *ajoute* à la rapidité, tout comme la rapidité ajoute afin de positionner. Applications courantes de l’accélération incluent la gravité, une voiture en accélérant l’et un espace livraison déclenchement ses systèmes de poussée. 

Semblable à la rapidité, l’accélération est définie dans une position et une unité de temps ; Toutefois, unité de temps d’accélération est appelée un *quadratique* unité, qui reflète la manière dont l’accélération est définie mathématiquement. Autrement dit, l’accélération de jeu est souvent mesurée en *pixels par seconde au carré*.

Si un objet a une accélération X de 10 unités par seconde au carré, cela signifie que sa rapidité augmentera de 10 fois par seconde. Si le démarrage à partir d’un arrêt, d’après qu’une seconde il doit être mis à 10 unités par seconde, après deux secondes 20 unités par seconde, et ainsi de suite.

Accélération en deux dimensions nécessite un composant X et Y, donc elle peut être affectée comme suit :


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Accélération et décélération

Bien que l’accélération et décélération se distinguent parfois dans vos tâches quotidiennes vocale, il n’existe aucune différence technique entre les deux. La gravité est une force de ce qui entraîne l’accélération. Si un objet est levé vers le haut puis gravité ralentit il (décélération), mais une fois que l’objet a cessé d’escalade et est compris dans la même direction que gravité puis gravité est accélérant il (accélération). Comme illustré ci-dessous, l’application une accélération est le même si elle est appliquée dans la même direction inverse ou la direction du déplacement. 


### <a name="implementing-acceleration"></a>Implémentation de l’accélération

Accélération est semblable à la vélocité lors de l’implémentation : il n’est pas automatiquement implémentée par CocosSharp, et l’accélération de temps est l’implémentation de votre choix (par opposition à accélération frame). Par conséquent, une implémentation simple d’accélération (ainsi que la rapidité) peut ressembler à :

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Le code ci-dessus est ce qui est appelé un *approximation linéaire* pour l’implémentation de l’accélération. En effet, il implémente l’accélération avec un degré de fermer assez de précision, mais il n’est pas un modèle parfaitement précis de l’accélération. Il est inclus ci-dessus pour vous aider à comprendre le concept de l’implémentation de l’accélération.

L’implémentation suivante est une application mathématiquement exacte d’accélération et de vélocité :


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

La différence la plus évidente pour le code ci-dessus est le `halfSecondsSquared` variable et son utilisation pour appliquer l’accélération pour positionner. La raison mathématique est dépasse le cadre de ce didacticiel, mais les développeurs intéressés par la formule mathématique cela peuvent trouver plus d’informations dans [cette discussion sur l’intégration d’accélération.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

L’impact pratique de `halfSecondSquare` est que l’accélération se comporte mathématiquement précis et plus prévisible, quelle que soit la fréquence d’images. L’approximation linéaire d’accélération est soumis à fréquence d’images : plus le taux de trames supprime devient de moins précise l’approximation. À l’aide de `halfSecondsSquared` garantit que code se comporte le même quelle que soit la fréquence d’images.


## <a name="angles-and-rotation"></a>Angles et la rotation

Objets tels que des visuels `CCSprite` prennent en charge de la rotation via un `Rotation` variable. Cela peut être assignée à une valeur pour définir sa rotation en degrés. Par exemple, le code suivant montre comment faire pivoter un `CCSprite` instance :


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

Notez que la rotation de 45 degrés dans le sens horaire (qui, pour des raisons historiques, est l’inverse de la rotation est appliquée mathématiquement) :

![](math-images/image2.png "Notez que la rotation est de 45 degrés dans le sens horaire qui pour des raisons historiques est l’inverse de la rotation est appliquée mathématiquement")

En général la rotation de CocosSharp et mathématiques régulière peut être visualisée comme suit :

![](math-images/image3.png "En général la rotation de CocosSharp et mathématiques régulière peut être visualisée comme suit")

![](math-images/image4.png "En général la rotation de CocosSharp et mathématiques régulière peut être visualisée comme suit")

Cette distinction est importante, car le `System.Math` classe utilise une rotation, donc les développeurs familiarisés avec cette classe inverser les angles lorsque vous travaillez avec `CCNode` instances.

Nous devons Notez que les diagrammes ci-dessus affichent la rotation en degrés ; Toutefois, certaines fonctions mathématiques (telles que les fonctions dans le `System.Math` espace de noms) attendre et retourner des valeurs dans *radians* au lieu de degrés. Nous allons examiner comment effectuer une conversion entre les types de deux unité un peu plus loin dans ce guide.


### <a name="rotating-to-face-a-direction"></a>Rotation pour un sens.

Comme indiqué ci-dessus, `CCSprite` peuvent être pivotées à l’aide de la `Rotation` propriété. Le `Rotation` propriété est fournie par `CCNode` (la classe de base pour `CCSprite`), ce qui signifie que la rotation peut être appliquée aux entités qui héritent de `CCNode` également. 

Certains jeux nécessite les objets à faire pivoter afin qu’ils sont confrontés à une cible. Exemples incluent un menaces ordinateur contrôlé à une cible de lecteur ou un destinataire espace vol vers le point où l’utilisateur touche l’écran. Toutefois, une valeur de rotation doit tout d’abord être dépend de l’emplacement de l’entité en rotation et l’emplacement de la cible pour faire face à.

Ce processus nécessite plusieurs étapes :

 - Identifier les *offset* de la cible. Offset fait référence à la distance entre l’entité de la rotation et de l’entité cible de X et Y.
 - Calcul de l’angle à partir du décalage à l’aide de la fonction de trigonométrie arc tangente (expliquée en détail ci-dessous).
 - Réglage d’une différence entre 0 degrés pointant vers la droite et la direction de l’entité de faire pivoter des points de lorsque non pivoté.
 - Ajustement de la différence entre la rotation mathématique (gauche) et de rotation CocosSharp (dans le sens horaire).

La fonction suivante (censée être écrites dans une entité) fait pivoter l’entité pour sont confrontés à une cible :


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

Le code ci-dessus peut être utilisé pour faire pivoter une entité afin qu’il rencontre le point où l’utilisateur est toucher l’écran, comme suit :


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

Ce code entraîne le comportement suivant :

![](math-images/image5.gif "Ce code génère ce comportement")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>À l’aide de Atan2 pour convertir des décalages à angles

`System.Math.Atan2` peut être utilisé pour convertir un décalage d’un angle. Le nom de fonction `Atan2` provient de l’arc tangente de fonctions trigonométriques. Le suffixe « 2 » différencie cette fonction à partir de la norme `Atan` fonction, ce qui est strictement correspond au comportement mathématique d’arc tangente. Arc tangente est une fonction qui retourne une valeur entre -90 et + 90 degrés (ou l’équivalent en radians). De nombreuses applications, y compris les jeux de l’ordinateur, nécessitent souvent 360 degrés de valeurs, afin que la `Math` classe inclut `Atan2` pour répondre à ce besoin.

Notez que le code ci-dessus passe le paramètre Y tout d’abord, puis le paramètre X, lorsque vous appelez le `Atan2` (méthode). Il s’agit en arrière à partir de X, Y classement des coordonnées de position classiques. Pour plus d’informations [consultez les documents Atan2](https://msdn.microsoft.com/en-us/library/system.math.atan2(v=vs.110).aspx).

Il est également important de noter que la valeur de retour de `Atan2` est exprimé en radians, c'est-à-dire une autre unité utilisée pour mesurer les angles. Ce guide ne couvrent les détails de radians, mais n’oubliez pas que toutes les fonctions trigonométriques dans le `System.Math` espace de noms utilisation de radians, toutes les valeurs doivent être converti en degrés avant d’être utilisés sur des objets CocosSharp. Trouverez plus d’informations sur radians [dans la page de Wikipedia radian](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>Angle vers l’avant

Une fois la `FacePoint` méthode convertit l’angle en radians, elle définit un `forwardAngle` valeur. Cette valeur représente l’angle dans lequel l’entité est face lorsque sa valeur de Rotation est égal à 0. Dans cet exemple, nous supposons que l’entité est face vers le haut, ce qui est de 90 degrés lors de l’utilisation d’une rotation mathématique (par opposition à CocosSharp rotation). Nous utilisons la rotation mathématique ici, car nous n’avons pas inversé encore la rotation pour CocosSharp.

L’exemple suivant montre une entité qui avec un `forwardAngle` de 90 degrés peut ressembler à :

![](math-images/image6.png "Cela montre à quoi peut ressembler une entité avec une forwardAngle de 90 degrés")


### <a name="angled-velocity"></a>Vélocité de l’angle

Jusqu'à présent, nous avons examiné comment convertir un offset dans un angle. Cette section est mis à l’autre manière – prend un angle et le convertit en X et Y. Voici quelques exemples : une voiture déplacement dans la direction dans laquelle il fait l’objet ou un destinataire espace résoudre une puce qui se déplace dans le sens rencontrés par le destinataire. 

Point de vue conceptuel, vitesse peut être calculée par définir la rapidité souhaitée lorsque non pivoté, puis faire pivoter cette rapidité de l’angle qui fait l’objet d’une entité. Pour expliquer ce concept, la vitesse (et accélération) peuvent être visualisés en tant que dimensions 2 *vecteur* (qui est généralement dessiné par une flèche). Un vecteur pour une valeur de la rapidité x = 100 et Y = 0 peut être visualisé comme suit :

![](math-images/image7.png "Un vecteur pour une valeur de la rapidité x = 100 et Y = 0 peut être visualisé comme suit")

Ce vecteur peut pivoter pour parvenir à une vitesse de nouveau. Par exemple, faire pivoter le vecteur de 45 degrés (à l’aide d’une rotation) renvoie les éléments suivants :

![](math-images/image8.png "Faire pivoter le vecteur de 45 degrés à l’aide des résultats d’une rotation dans cette")

Heureusement, rapidité, accélération et la même position peuvent tous pivoter avec le même code, quelle que soit la façon dont les valeurs sont appliquées. La fonction à usage général suivante peut être utilisée pour faire pivoter un vecteur par une valeur de Rotation de CocosSharp :


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

Comprenez la `RotateVector` méthode exige d’être familiarisé avec les fonctions trigonométriques cosinus et sinus, ainsi que certains algèbre linéaire, ce qui dépasse le cadre de cet article. Toutefois, une fois implémenté le `RotateVector` méthode peut être utilisée pour faire pivoter les vecteur, y compris un vecteur de vitesse. Par exemple, le code suivant peut déclencher une puce dans une direction spécifiée par la `rotation` valeur :


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

![](math-images/image9.png "Ce code peut produire un élément à cette capture d’écran")


## <a name="summary"></a>Récapitulatif

Ce guide aborde les concepts mathématiques courantes dans le développement de jeux 2D. Il montre comment assigner et mettre en œuvre de la vitesse et l’accélération et explique comment faire pivoter les objets et les vecteurs de mouvement dans n’importe quelle direction.
