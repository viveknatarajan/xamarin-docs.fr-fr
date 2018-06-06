---
title: Utilisation de watchOS d’entrée de texte dans Xamarin
description: Ce document décrit une entrée texte watchOS dans Xamarin. Il traite des PresentTextInputController (méthode), libres, texte brut, emojis et dictée.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: da668333b3549c92264af7d4da4941ac6b5bf865
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791382"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Utilisation de watchOS d’entrée de texte dans Xamarin

L’Apple Watch ne fournit pas d’un clavier pour les utilisateurs d’entrer du texte, mais il ne prend pas en charge les autres possibilités de nom convivial de la surveillance :

- Sélection d’une liste prédéfinie des options de texte,
- Siri dictée,
- En choisissant un emoji,
- Scribble reconnaissance de l’écriture manuscrite lettre par lettre (introduite dans watchOS 3).

Le simulateur ne prend pas en charge dictée, mais vous pouvez toujours tester l’entrée de texte autres options du contrôleur, telles que Scribble, comme illustré ici :

![](text-input-images/textinput-sml.png "Test de l’option de dessin à main levée")

Pour accepter l’entrée de texte dans une application de surveillance :

1. Créer un tableau de chaînes des options prédéfinies.
2. Appelez `PresentTextInputController` avec le tableau, s’il faut autoriser emoji ou non et un `Action` qui est appelée lorsque l’utilisateur est terminé.
3. Dans l’action d’achèvement, de test pour le résultat d’entrée et prenez les mesures appropriées dans l’application (éventuellement définissant la valeur de texte d’une étiquette).

L’extrait de code suivant présente trois options prédéfinies à l’utilisateur :

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

Le `WKTextInputMode` énumération possède trois valeurs :

- Brut
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Brut

Lorsque le mode brut est défini, l’utilisateur peut choisir :

- Dictée,
- Scribble, ou
- à partir d’une liste prédéfinie qui fournit de l’application.

[![](text-input-images/plain-scribble-sml.png "Dictée, Scribble, ou à partir d’une liste prédéfinie qui fournit de l’application")](text-input-images/plain-scribble.png#lightbox)

Le résultat retourné est toujours un `NSObject` qui peut être casté en un `string`.

## <a name="emoji"></a>Emoji

Il existe deux types d’emoji :

- Régulières Unicode emoji
- Images animées

Lorsque l’utilisateur choisit un emoji Unicode, il est retourné sous forme de chaîne.

Si un emoji image animée est sélectionné le `result` dans la réalisation Gestionnaire contiendra un `NSData` objet qui contient l’emoji `UIImage`.

## <a name="accepting-dictation-only"></a>Acceptation dictée uniquement

Pour prendre l’utilisateur directement à l’écran de dictée sans affichage des suggestions (ou l’option de dessin à main levée) :

- passer un tableau vide pour obtenir la liste des suggestions, et
- Set `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Lorsque l’utilisateur est élevé, l’écran de surveillance affiche l’écran suivant, qui inclut le texte qu’il est entendu (par exemple « Ceci est un test ») :

![](text-input-images/dictation.png "Lorsque l’utilisateur est élevé, l’écran de surveillance affiche le texte comme il est entendu")

Une fois qu’il clique sur le **fait** bouton, le texte sera retourné.



## <a name="related-links"></a>Liens associés

- [Apple doc de texte et les étiquettes](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Introduction à watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
