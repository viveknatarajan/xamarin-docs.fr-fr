---
title: Boutons dans Xamarin.iOS
description: La classe UIButton est utilisée pour représenter les divers différents styles de bouton dans les écrans d’iOS. Ce guide présente les différentes options pour travailler avec des boutons dans iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2018
ms.openlocfilehash: a98ddc2622682f2c105a6aff32e94bd92a5b11f2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896458"
---
# <a name="buttons-in-xamarinios"></a>Boutons dans Xamarin.iOS

Dans iOS, le `UIButton` classe représente un contrôle bouton.

Les propriétés d’un bouton peuvent être modifiées par programme ou par le **panneau Propriétés** du concepteur iOS :

![Le panneau Propriétés du concepteur iOS](buttons-images/properties.png "le panneau Propriétés du concepteur iOS")

## <a name="creating-a-button-programmatically"></a>Création d’un bouton par programmation

Un `UIButton` peuvent être créés avec seulement quelques lignes de code.

- Instancier un bouton et spécifier son type :

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Type du bouton est spécifié par un `UIButtonType`:

  - `UIButtonType.System` -Un bouton à usage général
  - `UIButtonType.DetailDisclosure` -Indique la disponibilité des informations détaillées, généralement sur un élément spécifique dans une table
  - `UIButtonType.InfoDark` -Indique la disponibilité des informations de configuration ; couleur foncée
  - `UIButtonType.InfoLight` -Indique la disponibilité des informations de configuration ; couleur claire
  - `UIButtonType..AddContact` : Indique qu’un contact peut être ajouté.
  - `UIButtonType.Custom` -Bouton personnalisable

  Pour plus d’informations sur les différents types de boutons, jeter un œil :
  
  - Le [les types de bouton personnalisé](#custom-button-types) section de ce document
  - Le [bouton types](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) Recipe (Recette)
  - Apple [Human Interface Guidelines pour iOS](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/).

- Définir la taille et la position du bouton :

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Définir le texte du bouton. Utilisez le `SetTitle` (méthode), ce qui nécessite le texte et un `UIControlState` valeur :

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Pour plus d’informations sur un style à un bouton et en définissant son texte, consultez :

  - Le [un style à un bouton](#styling-a-button) section de ce document
  - Le [définir le texte du bouton](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) Recipe (Recette).

## <a name="handling-a-button-tap"></a>Gestion d’un appui sur un bouton

Pour répondre à un appui sur un bouton, fournir un gestionnaire pour le bouton `TouchUpInside` événement :

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` n’est pas l’événement de bouton disponible uniquement. `UIButton` est une classe enfant de `UIControl`, qui définit [grand nombre d’événements différents](xref:UIKit.UIControlEvent).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>À l’aide du concepteur iOS pour spécifier des gestionnaires d’événements de bouton

Utilisez le **événements** onglet de la **panneau Propriétés** pour spécifier des gestionnaires d’événements pour un bouton de divers événements.

Pour l’événement approprié, tapez le nom d’un nouveau gestionnaire d’événements ou sélectionnez-en un dans la liste. Cette opération crée un gestionnaire d’événements dans le code de contrôleur d’affichage du bouton.

![Onglet événements du panneau Propriétés](buttons-images/image1.png "onglet événements du panneau Propriétés")

## <a name="styling-a-button"></a>Un style à un bouton

`UIButton` les contrôles peuvent exister dans un nombre d’états différents, spécifié par un `UIControlState` valeur – `Normal`, `Disabled`, `Focused`, `Highlighted`, etc. Chaque état peut être donné à un style unique, spécifié par programmation ou avec le concepteur iOS.

> [!NOTE]
> Pour obtenir la liste complète de tous les `UIControlState` valeurs, examinons le [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> Documentation.

Par exemple, pour définir la couleur de titre et la couleur de l’ombre pour `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Le code suivant définit le titre de bouton à une chaîne (stylisé) avec attributs pour `UIControlState.Normal` et `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Types de bouton personnalisé

Boutons avec une `UIButtonType` de `Custom` n’avoir aucune des styles par défaut. Toutefois, il est possible de configurer l’apparence du bouton en définissant une image de ses différents états :

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

Selon si l’utilisateur touche le bouton ou non, il affichera comme l’une des images suivantes (`UIControlState.Normal`, `UIControlState.Highlighted` et `UIControlState.Selected` les États, respectivement) :

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted")
![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

Pour plus d’informations sur l’utilisation des boutons personnalisés, reportez-vous à la [utiliser une image pour un bouton](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) Recipe (Recette).

