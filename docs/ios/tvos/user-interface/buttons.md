---
title: Utilisation des boutons de tvOS dans Xamarin
description: Ce document décrit l’utilisation des boutons dans une application de tvOS générée avec Xamarin. Il explique comment travailler avec les boutons dans du code et dans les plans conceptuels, et il examine comment un bouton de style.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 3de732e9eee696ce21ffc5526afd44f29695a313
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789383"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Utilisation des boutons de tvOS dans Xamarin

Utiliser une instance de la `UIButton` classe pour créer un bouton peut être actif, sélectionnable dans une fenêtre de tvOS. Lorsque l’utilisateur sélectionne un bouton, il envoie un message d’Action à l’objet cible autoriser votre réponse d’application Xamarin.tvOS à l’utilisateur d’entrée.

[![](buttons-images/buttons01.png "Boutons de l’exemple")](buttons-images/buttons01.png#lightbox)

Pour plus d’informations sur l’utilisation avec le Focus et en accédant à la distance Siri, veuillez consulter notre [utilisation de Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) et [Siri distants et les contrôleurs Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) documentation.

<a name="About-Buttons" />

## <a name="about-buttons"></a>À propos des boutons

Dans tvOS, les boutons sont utilisés pour des actions spécifiques de l’application et peuvent contenir un titre, une icône ou les deux. Comme l’utilisateur accède à l’aide de l’Interface utilisateur de l’application le [Siri distant](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), la sélection se déplace sur le bouton donné rendant modifier les couleurs de texte et d’arrière-plan. Un cliché instantané est également appliquée sur le bouton d’ajout d’un effet 3D, ce qui semble dépassent le reste de l’Interface utilisateur.

[![](buttons-images/buttons01.png "Boutons de l’exemple")](buttons-images/buttons01.png#lightbox)

Apple a les suggestions suivantes pour l’utilisation des boutons :

- **Utilisez un titre ou une icône** , si les deux une icône et un titre peut être inclus dans un bouton, l’espace est limité, essayez de ne pas combiner les deux.
- **Marquer clairement les boutons Destructive** : si le bouton exécute une action (par exemple, la suppression d’un fichier), marquer clairement en tant que tel, à l’aide de texte et/ou l’icône. Actions de destruction doivent toujours présenter un [alerte](~/ios/tvos/user-interface/alerts.md) demandant à l’utilisateur pour limiter l’action.
- **Ne pas utiliser les boutons Précédent** -bouton du Menu sur l’instance distante de Siri est utilisé pour retourner à l’écran précédent. La seule exception à cette règle concerne les achats dans l’application ou les actions de destruction où un **Annuler** bouton doit être affiché.

Pour plus d’informations sur l’utilisation de Focus et de Navigation, consultez notre [utilisation de Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) documentation.

<a name="Button-Icons" />

### <a name="button-icons"></a>Icônes de bouton

Apple recommande d’utiliser des images reconnaissables très simples pour les icônes de votre bouton. Icônes trop complexes sont difficiles à reconnaître sur un écran dans l’espace sur un canapé, ainsi que vous essayez d’utiliser la représentation la plus simple possible pour faire passer l’idée. Dès que possible, utilisez standard, et en connaître les images pour les icônes (par exemple, la loupe pour la recherche).

<a name="Button-Titles" />

### <a name="button-titles"></a>Titres de bouton

Apple a les suggestions suivantes lors de la création de titres pour les boutons :

- **Afficher le texte descriptif ci-dessous icônes boutons** : si possible, sur place un texte clair et descriptif sous icône boutons uniquement pour obtenir plu l’objectif du bouton entre.
- **Utilisez des verbes ou verbaux pour le titre** -clairement l’état l’action que doit prendre place lorsque l’utilisateur clique sur le bouton.
- **Utiliser des majuscules de titre-Style** - à l’exception des articles, conjonctions ou prépositions (quatre lettres ou moins), chaque mot du titre du bouton doit être en majuscule.
- **Utiliser un court, au Point titre** -utilisez la formulation de possible le plus court pour décrire l’action du bouton.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Boutons et des animations

Pour travailler avec les boutons dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application à l’aide du Concepteur de Xamarin pour iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)


1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **bouton** à partir de la **bibliothèque** et déposez-la sur la vue : 

    [![](buttons-images/storyboard01.png "Un bouton")](buttons-images/storyboard01.png#lightbox)
1. Dans le **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du bouton telles que sa **titre** et **couleur du texte**: 

    [![](buttons-images/storyboard02.png "Propriétés d’un bouton")](buttons-images/storyboard02.png#lightbox)
1. Ensuite, basculez vers le **onglet événements** et rattachement un **événement** à partir de la **bouton** et appelez-le `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "L’onglet événements")](buttons-images/storyboard03.png#lightbox)
1. Vous va passer automatiquement à la `ViewController.cs` mode dans lequel vous pouvez placer la nouvelle Action dans votre code à l’aide du **des** et **vers le bas** touches de direction : 

    [![](buttons-images/storyboard04.png "Placer une Action dans le code")](buttons-images/storyboard04.png#lightbox)
1. Appuyez sur la **entrée** pour sélectionner l’emplacement : 

    [![](buttons-images/storyboard05.png "L’éditeur de code")](buttons-images/storyboard05.png#lightbox)
1. Enregistrez les modifications à tous les fichiers.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **bouton** à partir de la **bibliothèque** et déposez-la sur la vue : 

    [![](buttons-images/storyboard01vs.png "Un bouton")](buttons-images/storyboard01vs.png#lightbox)
1. Dans le **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du bouton telles que sa **titre** et **couleur du texte**: 

    [![](buttons-images/storyboard02vs.png "L’Explorateur de propriétés")](buttons-images/storyboard02vs.png#lightbox)
1. Ensuite, basculez vers le **onglet événements** et rattachement un **événement** à partir de la **bouton** et appelez-le `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "L’onglet événements")](buttons-images/storyboard03vs.png#lightbox)
1. Enregistrez les modifications à tous les fichiers.



Modifier votre vue contrôleur (exemple `ViewController.cs`) et ajoutez le code suivant pour gérer le bouton de sélection :


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

Tant qu’un bouton de `Enabled` propriété est `true` et il n’est pas couverte par un autre contrôle ou une vue, elle peut être rendue de l’élément actif à l’aide de l’élément distant Siri. Si l’utilisateur sélectionne le bouton et clique sur la Surface tactile, la `ButtonPressed` action définie ci-dessus est exécutée.

> [!IMPORTANT]
> Bien qu’il soit possible d’affecter des actions telles que `TouchUpInside` à un `UIButton` dans le concepteur lors de la création d’iOS un **Gestionnaire d’événements**, il ne sera jamais appelée car Apple TV n’a pas une fonction tactile d’écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser la valeur par défaut **Type d’Action** lors de la création **Actions** tvOS éléments d’interface utilisateur.




Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Boutons et Code

Si vous le souhaitez, un `UIButton` peut être créée dans le code c# et ajoutées à la vue de l’application tvOS. Exemple :

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

Lorsque vous créez un nouveau `UIButton` dans le code, vous spécifiez sa `UIButtonType` comme l’une des opérations suivantes :

- **Système** -Ceci est le type de bouton présenté par tvOS standard et est le type que vous utiliserez le plus souvent.
- **DetailDisclosure** -présente un type « baisser » de bouton permet de masquer ou afficher des informations détaillées.
- **InfoDark** -sombre détaillées info bouton affiche un « i » dans un cercle.
- **InfoLight** -un éclairage détaillé info bouton affiche un « i » dans un cercle.
- **AddContact** -afficher le bouton sous forme de bouton Ajouter un Contact.
- **Personnalisé** -vous permet de personnaliser plusieurs caractéristiques du bouton.

Ensuite, vous définissez la taille à l’écran et l’emplacement du bouton. Exemple :

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Ensuite, définissez le titre du bouton. `UIButtons` est différent de celui de la plupart `UIKit` contrôles dans la mesure où ils ont un état, donc vous ne pouvez pas simplement modifier le titre, vous devez le modifier pour une donnée `UIControlState`. Exemple :

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Ensuite, utilisez le `AllEvents` événements pour voir de lorsque l’utilisateur a cliqué sur le bouton. Exemple :

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Enfin, vous ajoutez le bouton à la vue pour l’afficher :

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> Bien qu’il soit possible d’affecter des actions telles que `TouchUpInside` à un `UIButton`, il ne sera jamais appelée car Apple TV n’a pas une fonction tactile d’écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser des événements tels que **AllEvents** ou **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Un style à un bouton

tvOS fournit plusieurs propriétés d’un `UIButton` qui peut être utilisé pour fournir son titre et leur appliquer un style avec des éléments tels que la couleur d’arrière-plan et des images.

<a name="Button-Titles" />

### <a name="button-titles"></a>Titres de bouton

Comme nous l’avons vu, `UIButtons` diffèrent de la plupart `UIKit` contrôles dans la mesure où ils ont un état, donc vous ne pouvez pas simplement modifier le titre, vous devez la changer pour une donnée `UIControlState`. Exemple :

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Vous pouvez définir la couleur du titre du bouton à l’aide de la `SetTitleColor` (méthode). Exemple :

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Et vous pouvez ajuster le titre de l’ombre à l’aide de la `SetTitleShadowColor`. Exemple :

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Vous pouvez définir l’ombre du titre pour changer de *Engraved* à *relief* lorsque le bouton est mis en surbrillance à l’aide du code suivant :

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

En outre, vous pouvez utiliser avec attributs de texte en tant que titre du bouton. Exemple :

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Images des boutons

Un `UIButton` peut avoir une image attachée et pouvez utiliser une image comme arrière-plan.

Pour définir l’image d’arrière-plan d’un bouton pour une donnée `UIControlState`, utilisez le code suivant :

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Définir le `AdjustsImageWhenHiglighted` propriété `true` pour dessiner l’image plus claire lorsque le bouton est mis en surbrillance (il s’agit de la valeur par défaut). Définir le `AdjustsImageWhenDisabled` propriété `true` pour dessiner l’image la plus sombre lorsque le bouton est désactivé (là encore, c’est la valeur par défaut).

Pour définir l’image affichée sur le bouton, utilisez le code suivant :

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Utilisez le `TintColor` propriété à définir une teinte de couleur qui est appliquée au titre et l’image du bouton. Pour les boutons de la `Custom` type, cette propriété n’a aucun effet, vous devez implémenter la `TintColor` comportement vous-même.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation des boutons à l’intérieur d’une application Xamarin.tvOS. Il a montré comment travailler avec les boutons dans le concepteur iOS et comment créer des boutons en code c#. Enfin, elle a montré comment modifier le titre d’un bouton et modifier son style et l’apparence.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
