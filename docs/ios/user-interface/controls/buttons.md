---
title: Boutons
description: La classe UIButton est utilisée pour représenter différents différents styles de bouton dans les écrans d’iOS. Cette section présente les différentes options pour l’utilisation des boutons dans iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c2c33103c005a5ed567b1c4703846f887d824ac4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="buttons"></a>Boutons

_La classe UIButton est utilisée pour représenter différents différents styles de bouton dans les écrans d’iOS. Cette section présente les différentes options pour l’utilisation des boutons dans iOS._

La `UIButton`classe représente un contrôle de bouton dans iOS. 

Propriétés d’un bouton peuvent être modifiées dans le `Properties Pad` du concepteur iOS :


![](buttons-images/properties.png "La zone de propriétés du concepteur iOS")

## <a name="creating-a-button"></a>Création d’un bouton

Un UIButton peut être créé dans via uniquement quelques lignes de code.

Tout d’abord, instanciez un nouveau bouton et spécifier le type de bouton que vous avez besoin :

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

Le UIButtonType doit être spécifié comme l’une des opérations suivantes :

- **Système** -Ceci est le type de bouton standard utilisé par iOS et est le type que vous utiliserez le plus souvent.
- **DetailDisclosure** -présente un type « baisser » de bouton permet de masquer ou afficher des informations détaillées.
- **InfoDark** -sombre détaillées info bouton affiche un « i » dans un cercle.
- **InfoLight** -un éclairage détaillé info bouton affiche un « i » dans un cercle.
- **AddContact** -afficher le bouton sous forme de bouton Ajouter un Contact.
- **Personnalisé** -vous permet de personnaliser plusieurs caractéristiques du bouton.

Vous trouverez plus d’informations sur les types de boutons dans les [Types de boutons](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) recette.

Ensuite, définissez la taille à l’écran et l’emplacement du bouton. Exemple :

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

Pour modifier le texte dans un bouton, utilisez le `SetTitle` propriété sur le bouton, ce qui vous oblige à définir une chaîne de texte et un `UIControlStyle`. Par exemple :

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

Définir des propriétés différentes pour chaque état vous permet de communiquer plus d’informations à l’utilisateur (par exemple). Vérifiez la couleur du texte gris de l’état désactivé). Vous pouvez basculer entre chaque état à l’aide du concepteur iOS, ou vous pouvez le faire par programme. Pour plus d’informations sur le texte de bouton de paramètre et l’état, reportez-vous à la [texte du bouton Définir](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) recette.

## <a name="dealing-with-user-interactions"></a>Vous traitez des interactions de l’utilisateur


Boutons ne sont pas très utiles, sauf si elles faire en cas de clic sur ! 

Dans iOS sur les boutons, les événements sont presque toujours les événements tactiles, comme l’utilisation interagit avec le bouton sur l’écran en toucher. Vous trouverez une liste de tous les événements de contrôle possibles [ici](https://developer.apple.com/documentation/uikit/uicontrolevents), mais l’événement le plus couramment utilisé sur iOS est `TouchUpInside`. Vous pouvez ensuite créer un gestionnaire d’événements pour faire quelque chose une fois que le bouton a été enfoncé :


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>Ajout d’événements dans le concepteur iOS
 
Utilisez l’onglet événements dans la zone de propriété pour ajouter des événements à des contrôles.

Sélectionnez l’événement, et tapez le nom d’un gestionnaire d’événements ou sélectionnez une dans la liste. Cela créera une nouvelle méthode partielle dans votre classe de contrôleur d’affichage.

![Onglet événements](buttons-images/image1.png)

## <a name="styling-a-button"></a>Un style à un bouton

UIButtons sont différentes de celles UIKit la plupart des contrôles dans la mesure où ils ont un état, vous ne pouvez pas déplacer simplement le titre, vous devez la changer pour chaque `UIControlState`. La définition de la couleur de titre et la couleur de l’ombre est effectuée de façon similaire :

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

En outre, vous pouvez utiliser avec attributs de texte en tant que titre du bouton. Par exemple :

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Types de boutons personnalisés


Lorsque vous définissez la `Custom` type de bouton, l’objet ne possède aucun rendu par défaut. Vous pouvez configurer l’apparence du bouton en définissant une image pour les différents états. Par exemple, le code suivant montre comment ajouter des images pour les `Normal`, `Highlighted` et `Selected` états :


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


Selon si l’utilisateur touche le bouton ou non, elle est restituée comme l’une des images suivantes (`Normal`, `Highlighted` et `Selected` États respectivement) :


![](buttons-images/image22.png "État de UIButton Normal")
![](buttons-images/image23.png "état UIButton mis en surbrillance")
![](buttons-images/image24.png "état UIButton sélectionné")

Pour plus d’informations sur l’utilisation des boutons personnalisés, reportez-vous à la [utiliser une Image pour un bouton](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/).


## <a name="related-links"></a>Liens associés

- [Classeur de UIButton](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
