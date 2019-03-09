---
title: Référence de GamePad MonoGame
description: Ce document décrit GamePad, une classe inter-plateformes pour accéder à des périphériques d’entrée dans MonoGame. Il explique comment lire l’entrée à partir du boîtier de commande et fournit un exemple de code.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: c1c03e0ec17ade57536b4ed121469e3ae2274e75
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668970"
---
# <a name="monogame-gamepad-reference"></a>Référence de GamePad MonoGame

_GamePad est une classe standard permettant d’accéder aux périphériques d’entrée dans MonoGame multiplateforme._

`GamePad` peut être utilisé pour lire l’entrée à partir de périphériques d’entrée sur plusieurs plateformes MonoGame. Ce guide montre comment utiliser la classe GamePad. Étant donné que chaque périphérique d’entrée est différent dans la disposition et le nombre de boutons, qu'il fournit, ce guide inclut des diagrammes qui illustrent les différents mappages d’appareil.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>GamePad en remplacement de Xbox360GamePad

L’API XNA d’origine fournie le `Xbox360GamePad` classe pour la lecture de l’entrée à partir d’un contrôleur de jeu sur le PC ou Xbox 360. MonoGame a remplacé avec un `GamePad` classe dans la mesure où les contrôleurs de Xbox 360 ne peut pas être utilisés sur la plupart des plateformes de MonoGame (par exemple, iOS ou votre Xbox One). En dépit de la modification du nom, l’utilisation de la `GamePad` classe est semblable à la `Xbox360GamePad` classe.

## <a name="reading-input-from-gamepad"></a>Lit l’entrée de GamePad

Le `GamePad` classe fournit un moyen normalisé de lecture de l’entrée sur n’importe quelle plateforme MonoGame. Il fournit des informations via deux méthodes :

- `GetState` – Retourne l’état actuel des boutons du contrôleur, bâtons analogiques et pavé directionnel.
- `GetCapabilities` – Retourne des informations sur les fonctionnalités du matériel, telles que si le contrôleur a certains boutons ou prend en charge de vibration.

### <a name="example-moving-a-character"></a>Exemple : Déplacement d’un caractère

Le code suivant illustre l’utilisation de la clé de curseur gauche pour déplacer un caractère en définissant son `XVelocity` et `YVelocity` propriétés. Ce code part du principe que `characterInstance` est une instance d’un objet qui a `XVelocity` et `YVelocity` propriétés :

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>Exemple : Détection de notifications Push

`GamePadState` Fournit des informations sur l’état actuel du contrôleur, par exemple si un certain bouton est enfoncé. Certaines actions, notamment à rendre un caractère de saut, exiger la vérification si le bouton a été envoyé (n’était pas la dernière image, mais est arrêté ce frame) ou libéré (était la dernière image, mais pas vers le bas de ce frame).

Pour effectuer ce type de logique, les variables locales qui stockent le frame précédent `GamePadState` et le frame actuel `GamePadState` doit être créé. L’exemple suivant montre comment stocker et utiliser le frame précédent `GamePadState` pour implémenter le moment du saut :

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

### <a name="example-checking-for-buttons"></a>Exemple : Vérification des boutons

`GetCapabilities` peut être utilisé pour vérifier si un contrôleur dispose d’un matériel spécifique, tel qu’un bouton particulier ou le stick analogique. Le code suivant montre comment vérifier pour les boutons de B et Y sur un contrôleur dans un jeu qui nécessite la présence des deux boutons :

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

les applications iOS prennent en charge l’entrée de contrôleur de jeu sans fil.

> [!IMPORTANT]
> Les packages NuGet pour MonoGame 3.5 n’incluent pas de prise en charge des contrôleurs de jeu sans fil. À l’aide de la classe GamePad sur iOS nécessite la génération 3.5 MonoGame à partir de la source ou à l’aide de fichiers binaires MonoGame 3.6 NuGet.

### <a name="ios-game-controller"></a>Contrôleur de jeu iOS

Le `GamePad` classe retourne les propriétés lues à partir des contrôleurs sans fil. Les propriétés dans le `GamePad` fournir des indications relativement complètes pour iOS standard matériel de contrôleur, comme indiqué dans le diagramme suivant :

![](input-images/image1.png "Les propriétés de la GamePad fournissent des indications relativement complètes pour iOS standard matériel de contrôleur, comme illustré dans ce diagramme")

## <a name="apple-tv"></a>Apple TV

Jeux de Apple TV peuvent utiliser le Siri Remote ou les contrôleurs de jeu sans fil pour l’entrée.

### <a name="siri-remote"></a>Siri Remote

*Siri Remote* est le périphérique d’entrée natif pour Apple TV. Bien que les valeurs à partir de l’instance distante de Siri peuvent être lues via des événements (comme indiqué dans le [Siri Remote et Bluetooth contrôleurs guide](~/ios/tvos/platform/remote-bluetooth.md)), la `GamePad` classe peut retourner des valeurs à partir de l’instance distante de Siri.

Notez que `GamePad` peut uniquement lire l’entrée à partir du bouton play et touchez surface :

![](input-images/image2.png "Notez que GamePad peut uniquement lire l’entrée à partir du bouton play et touchez surface")

Depuis la pression tactile déplacement de l’aire de conception est en lecture le `DPad` propriété, le déplacement des valeurs sont indiquées à l’aide de la `ButtonState` classe. En d’autres termes, les valeurs sont uniquement disponibles en tant que `ButtonState.Pressed` ou `ButtonState.Released`, par opposition à des valeurs numériques ou des mouvements.

### <a name="apple-tv-game-controller"></a>Contrôleur de jeu de TV Apple

Contrôleurs de jeu pour Apple TV se comportent comme des contrôleurs de jeu pour les applications iOS. Pour plus d’informations, consultez le [iOS section de contrôleur de jeu](#iOS-game-controller). 

## <a name="xbox-one"></a>Xbox One

La console Xbox One prend en charge la lecture de l’entrée à partir d’un contrôleur de jeu Xbox One.

### <a name="xbox-one-game-controller"></a>Contrôleur de jeu Xbox One

Le contrôleur de jeu Xbox One est le périphérique d’entrée plus courantes pour Xbox One. Le `GamePad` classe fournit des valeurs d’entrée du matériel de contrôleur de jeu.

![](input-images/image3.png "La classe GamePad fournit des valeurs d’entrée du matériel de contrôleur de jeu")

## <a name="summary"></a>Récapitulatif

Ce guide a présenté de MonoGame `GamePad` classe, comment implémenter la logique de lecture de l’entrée et des diagrammes commun `GamePad` implémentations.

## <a name="related-links"></a>Liens associés

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
