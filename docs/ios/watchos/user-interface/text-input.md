---
title: Utilisation de watchOS Text Input dans Xamarin
description: Ce document décrit l’entrée de texte watchOS dans Xamarin. Il traite des PresentTextInputController (méthode), de brouillon, texte brut, emojis et la dictée.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 2092b12254008936f2c5b6a7d9dd610ff751e802
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122359"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Utilisation de watchOS Text Input dans Xamarin

L’Apple Watch ne fournit pas d’un clavier pour les utilisateurs d’entrer du texte, mais il ne prend pas en charge des alternatives espion conviviale :

- Sélection dans une liste prédéfinie d’options de texte,
- Dictée Siri,
- En choisissant un emoji,
- Scribble reconnaissance de l’écriture manuscrite lettre par lettre (introduite dans watchOS 3).

Le simulateur ne prend pas en charge la dictée, mais vous pouvez toujours tester l’entrée de texte d’autres options du contrôleur, par exemple Scribble, comme illustré ici :

![](text-input-images/textinput-sml.png "Test de l’option de dessin à main levée")

Pour accepter l’entrée de texte dans une application watch :

1. Créer un tableau de chaînes des options prédéfinies.
2. Appelez `PresentTextInputController` avec le tableau, s’il faut autoriser emoji ou non et un `Action` qui est appelée lorsque l’utilisateur est terminé.
3. Dans l’action d’achèvement, test pour le résultat d’entrée et prendre les mesures appropriées dans l’application (éventuellement définissant la valeur de texte d’une étiquette).

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

Le `WKTextInputMode` énumération a trois valeurs :

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

Lorsque l’utilisateur choisit un emoji Unicode, elle est retournée sous forme de chaîne.

Si un emoji image animée est sélectionné le `result` dans la réalisation Gestionnaire contiendra un `NSData` objet qui contient l’emoji `UIImage`.

## <a name="accepting-dictation-only"></a>Accepter la dictée uniquement

Dirige l’utilisateur directement à l’écran de dictée sans afficher des suggestions (ou l’option de dessin à main levée) :

- passer un tableau vide pour obtenir la liste des suggestions, et
- Définir `WatchKit.WKTextInputMode.Plain`.

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

Lorsque l’utilisateur est en parlant, l’écran de surveillance affiche l’écran suivant, qui inclut le texte comme il est entendu (par exemple « Ceci est un test ») :

![](text-input-images/dictation.png "Lorsque l’utilisateur est en parlant, l’écran de surveillance affiche le texte comme il est entendu")

Une fois qu’ils appuient sur la **fait** s’affichera le texte de bouton.



## <a name="related-links"></a>Liens associés

- [Apple texte et étiquettes doc](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Introduction à watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
