---
title: Référence de boîtier MonoGame
description: Ce document décrit le boîtier de commande, une classe inter-plateformes pour accéder à des périphériques d’entrée dans MonoGame. Il explique comment lire l’entrée à partir du boîtier de commande et fournit un exemple de code.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: f8a833dbab2b8a69f328cd26cb69db84f3083222
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783322"
---
# <a name="monogame-gamepad-reference"></a>Référence de boîtier MonoGame

_Boîtier de commande est une classe standard pour accéder à des périphériques d’entrée dans MonoGame inter-plateformes._

`GamePad` peut être utilisé pour lire l’entrée à partir de périphériques d’entrée sur plusieurs plateformes MonoGame. Ce guide montre comment utiliser la classe du boîtier de commande. Étant donné que chaque périphérique d’entrée est différent dans la disposition et le nombre de boutons, qu'il fournit, ce guide comprend des diagrammes qui montrent les différents mappages d’appareil.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Boîtier de commande en remplacement de Xbox360GamePad

L’API XNA d’origine fournie le `Xbox360GamePad` classe pour l’entrée de la lecture à partir d’un contrôleur de jeu sur le PC ou Xbox 360. MonoGame a remplacé avec un `GamePad` classe étant donné que les contrôleurs de Xbox 360 ne peut pas être utilisés sur la plupart des plateformes de MonoGame (par exemple, iOS ou Xbox One). En dépit de la modification du nom, l’utilisation de la `GamePad` classe est semblable à la `Xbox360GamePad` classe.

## <a name="reading-input-from-gamepad"></a>Lit l’entrée de boîtier

La `GamePad` classe fournit une méthode standardisée de lecture de l’entrée sur n’importe quelle plateforme MonoGame. Il fournit des informations via deux méthodes :

- `GetState` – Retourne l’état actuel des boutons, les clés analogiques et boîtier du contrôleur.
- `GetCapabilities` – Retourne des informations sur les fonctionnalités du matériel, telles que si le contrôleur a certains boutons ou prend en charge de vibration.

### <a name="example-moving-a-character"></a>Exemple : Déplacement d’un caractère

Le code suivant montre comment le stick gauche du curseur de défilement peut servir pour vous déplacer d’un caractère en définissant son `XVelocity` et `YVelocity` propriétés. Ce code suppose que `characterInstance` est une instance d’un objet qui a `XVelocity` et `YVelocity` propriétés :

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>Exemple : Détection des notifications Push

`GamePadState` Fournit des informations sur l’état actuel du contrôleur, par exemple si un certain bouton est enfoncé. Certaines actions, par exemple, un caractère de saut, nécessitent le contrôle si le bouton a été enfoncé (n’était pas la dernière image, mais est arrêté ce frame) ou libéré (était la dernière image, mais pas vers le bas de cette image). 

Pour effectuer ce type de logique, les variables locales qui stockent l’image précédente `GamePadState` et le frame actuel `GamePadState` doit être créé. L’exemple suivant montre comment stocker et utiliser l’image précédente `GamePadState` pour implémenter le moment du saut :

```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed = 
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```

### <a name="example-checking-for-buttons"></a>Exemple : La vérification des boutons

`GetCapabilities` peut être utilisé pour vérifier si un contrôleur a configuration matérielle spécifique, tel qu’un bouton particulier ou le stick analogique. Le code suivant montre comment vérifier les boutons B et Y sur un contrôleur dans un jeu qui nécessite la présence des deux boutons :

```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```

## <a name="ios"></a>iOS

des applications iOS prennent en charge l’entrée d’un contrôleur de jeu sans fil.

> [!IMPORTANT]
> Les packages NuGet pour MonoGame 3.5 n’incluent pas de prise en charge des contrôleurs de jeu sans fil. À l’aide de la classe de boîtier sur iOS requiert la génération 3.5 MonoGame à partir de la source ou à l’aide des fichiers binaires MonoGame 3.6 NuGet. 

### <a name="ios-game-controller"></a>e/s de contrôleur de jeu

La `GamePad` classe retourne les propriétés lues à partir des contrôleurs sans fil. Les propriétés de la `GamePad` fournissent une bonne couverture pour les e/s standard matériel de contrôleur, comme indiqué dans le diagramme suivant :

![](input-images/image1.png "Les propriétés du boîtier de commande fournissent une bonne couverture pour les e/s standard matériel de contrôleur, comme illustré dans ce diagramme")

## <a name="apple-tv"></a>Apple TV

Jeux d’Apple TV permet le Siri distant ou sans fil contrôleurs de jeu pour l’entrée.

### <a name="siri-remote"></a>Siri distant

*Siri distant* est le périphérique d’entrée natif pour Apple TV. Bien que les valeurs à partir de l’instance distante de Siri peuvent être lues via les événements (comme indiqué dans le [Siri distants et les contrôleurs Bluetooth guide](~/ios/tvos/platform/remote-bluetooth.md)), la `GamePad` classe peut retourner des valeurs à partir de l’instance distante Siri.

Notez que `GamePad` peut uniquement lire l’entrée à partir du bouton de lecture et touchez surface : 

![](input-images/image2.png "Notez que le boîtier de commande peut uniquement lire l’entrée à partir du bouton de lecture et touchez surface")

Depuis la fonction tactile, déplacement de l’aire de conception est en lecture le `DPad` propriété, le déplacement des valeurs sont indiquées à l’aide de la `ButtonState` classe. En d’autres termes, les valeurs sont disponibles seulement comme `ButtonState.Pressed` ou `ButtonState.Released`, par opposition à des valeurs numériques ou de mouvements.

### <a name="apple-tv-game-controller"></a>Apple TV jeu contrôleur

Contrôleurs de jeu pour Apple TV se comportent comme des contrôleurs de jeu pour les applications iOS. Pour plus d’informations, consultez la [iOS section du contrôleur de jeu](#iOS_Game_Controller). 

## <a name="xbox-one"></a>Xbox One

La console Xbox One prend en charge la lecture de l’entrée à partir d’un contrôleur de jeu Xbox One.

### <a name="xbox-one-game-controller"></a>Contrôleur de jeu Xbox un

Le contrôleur de jeu Xbox One est le périphérique d’entrée plus courant pour la Xbox One. La `GamePad` classe fournit des valeurs d’entrée du matériel de contrôleur de jeu.

![](input-images/image3.png "La classe de boîtier de commande fournit des valeurs d’entrée du matériel de contrôleur de jeu")

## <a name="summary"></a>Récapitulatif

Ce guide fourni une vue d’ensemble de MonoGame `GamePad` classe, comment implémenter la logique de la lecture de l’entrée et les diagrammes commun `GamePad` implémentations.

## <a name="related-links"></a>Liens associés

- [Boîtier de commande MonoGame](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
