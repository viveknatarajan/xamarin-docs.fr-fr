---
title: Travaillez avec des boutons de tvOS dans Xamarin
description: Ce document décrit comment travailler avec des boutons dans une application tvOS générée avec Xamarin. Il explique comment travailler avec des boutons dans le code et dans les tables de montage séquentiel, et il examine la manière d’un bouton de style.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/07/2017
ms.openlocfilehash: 6d8fc1daaced24dccead78c4f9d0e5d0959b3755
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199009"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Travaillez avec des boutons de tvOS dans Xamarin

Utiliser une instance de la `UIButton` classe pour créer un bouton peut recevoir le focus et sélectionnable dans une fenêtre de tvOS. Lorsque l’utilisateur sélectionne un bouton, il envoie un message d’Action à l’objet cible autoriser votre réponse d’application Xamarin.tvOS à l’utilisateur d’entrée.

[![](buttons-images/buttons01.png "Boutons de l’exemple")](buttons-images/buttons01.png#lightbox)

Pour plus d’informations sur l’utilisation de Focus et en accédant à la distance Siri, veuillez consulter notre [fonctionne avec la Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) et [Siri Remote et Bluetooth contrôleurs](~/ios/tvos/platform/remote-bluetooth.md) documentation.

<a name="About-Buttons" />

## <a name="about-buttons"></a>À propos des boutons

Dans tvOS, les boutons sont utilisés pour les actions spécifiques de l’application et peuvent contenir un titre, une icône ou les deux. Lorsque l’utilisateur navigue à l’aide de l’Interface utilisateur de l’application le [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), deviennent la préoccupation principale du bouton concerné, changer les couleurs de texte et d’arrière-plan. Une ombre est également appliquée au bouton de l’ajout d’un effet 3D faire paraître à envahir le reste de l’Interface utilisateur.

[![](buttons-images/buttons01.png "Boutons de l’exemple")](buttons-images/buttons01.png#lightbox)

Apple a les suggestions suivantes pour travailler avec des boutons :

- **Utilisez un titre ou une icône** , tout en les deux une icône et un titre peut être inclus dans un bouton, l’espace est limité donc essayer de ne pas combiner les deux.
- **Marquer clairement les boutons Destructive** : si le bouton effectue une action (par exemple, la suppression d’un fichier), marquer clairement en tant que tel, à l’aide de texte et/ou l’icône. Actions de destruction doivent toujours présenter un [alerte](~/ios/tvos/user-interface/alerts.md) demandant à l’utilisateur pour limiter l’action.
- **Ne pas utiliser les boutons Précédent** -bouton du Menu à un emplacement distant Siri sert à retourner à l’écran précédent. La seule exception à cette règle concerne les achats dans l’application ou les actions de destruction où un **Annuler** bouton doit s’afficher.

Pour plus d’informations sur l’utilisation avec le Focus et la Navigation, consultez notre [fonctionne avec la Navigation et le Focus](~/ios/tvos/app-fundamentals/navigation-focus.md) documentation.

<a name="Button-Icons" />

### <a name="button-icons"></a>Icônes de bouton

Apple suggère que vous utilisez des images simples et hautement reconnaissables pour vos icônes de bouton. Les icônes trop complexes sont difficiles à reconnaître sur un écran de télévision à travers la pièce sur un canapé, par conséquent, essayez d’utiliser la représentation la plus simple possible pour faire passer l’idée. Si possible, utilisez standard, connu des images pour les icônes (par exemple, une loupe pour la recherche).

<a name="Button-Titles" />

### <a name="button-titles"></a>Titres de bouton

Apple a les suggestions suivantes lors de la création de titres pour vos boutons :

- **Afficher le texte descriptif ci-dessous icônes boutons** : si possible, placez votre texte clair et descriptif ci-dessous icône uniquement des boutons à get plus l’objectif du bouton entre.
- **Utiliser des verbes ou des Phrases de verbe pour le titre** -clairement l’action qui effectuera placer lorsque l’utilisateur clique sur le bouton.
- **Utiliser la mise en majuscules de Style de titre** : à l’exception des articles, conjonctions ou prépositions (quatre lettres ou moins), tous les mots du titre du bouton doivent être en majuscules.
- **Utiliser un court, au Point titre** -utilisez la formulation de possible le plus court pour décrire l’action du bouton.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Boutons et des tables de montage séquentiel

Pour travailler avec des boutons dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application à l’aide du Concepteur de Xamarin pour iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)


1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Faites glisser un **bouton** à partir de la **bibliothèque** et déposez-la sur la vue : 

    [![](buttons-images/storyboard01.png "Un bouton")](buttons-images/storyboard01.png#lightbox)
1. Dans le **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du bouton telles que son **titre** et **couleur du texte**: 

    [![](buttons-images/storyboard02.png "Propriétés d’un bouton")](buttons-images/storyboard02.png#lightbox)
1. Ensuite, basculez vers le **onglet événements** et AutoEventWireup un **événement** à partir de la **bouton** et appelez-le `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "L’onglet d’événements")](buttons-images/storyboard03.png#lightbox)
1. Vous bascule automatiquement vers le `ViewController.cs` vue où vous pouvez placer la nouvelle Action dans votre code en utilisant le **des** et **vers le bas** touches de direction : 

    [![](buttons-images/storyboard04.png "Placer une nouvelle Action dans le code")](buttons-images/storyboard04.png#lightbox)
1. Appuyez sur la **entrée** pour sélectionner l’emplacement : 

    [![](buttons-images/storyboard05.png "L’éditeur de code")](buttons-images/storyboard05.png#lightbox)
1. Enregistrez les modifications à tous les fichiers.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvrir pour modification.
1. Faites glisser un **bouton** à partir de la **bibliothèque** et déposez-la sur la vue : 

    [![](buttons-images/storyboard01vs.png "Un bouton")](buttons-images/storyboard01vs.png#lightbox)
1. Dans le **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés du bouton telles que son **titre** et **couleur du texte**: 

    [![](buttons-images/storyboard02vs.png "L’Explorateur de propriétés")](buttons-images/storyboard02vs.png#lightbox)
1. Ensuite, basculez vers le **onglet événements** et AutoEventWireup un **événement** à partir de la **bouton** et appelez-le `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "L’onglet d’événements")](buttons-images/storyboard03vs.png#lightbox)
1. Enregistrez les modifications à tous les fichiers.



Modifier votre contrôleur d’affichage (exemple `ViewController.cs`) et ajoutez le code suivant pour gérer le bouton sélectionné :


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

Aussi longtemps que d’un bouton `Enabled` propriété est `true` et il n’est pas couvert par un autre contrôle ou une vue, elle peut être rendue de l’élément actif à l’aide de l’instance distante de Siri. Si l’utilisateur sélectionne le bouton et clique sur l’aire de toucher le `ButtonPressed` action définie ci-dessus serait exécutée.

> [!IMPORTANT]
> Bien qu’il soit possible d’affecter des actions telles que `TouchUpInside` à un `UIButton` dans le concepteur lors de la création iOS un **Gestionnaire d’événements**, elle ne sera jamais appelée car Apple TV n’a pas une pression tactile écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser la valeur par défaut **Type d’Action** lors de la création **Actions** tvOS éléments d’interface utilisateur.




Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Boutons et Code

Si vous le souhaitez, une `UIButton` peuvent être créés dans C# de code et ajoutées à la vue de l’application tvOS. Exemple :

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

Lorsque vous créez un nouveau `UIButton` dans le code, vous spécifiez son `UIButtonType` comme l’une des opérations suivantes :

- **Système** -Ceci est le type de bouton présenté par tvOS standard et est le type que vous utiliserez le plus souvent.
- **DetailDisclosure** -présente un type de bouton utilisé pour masquer ou afficher des informations détaillées de « désactiver ».
- **InfoDark** -bouton affiché un « i » dans un cercle des informations détaillées sur sombre.
- **InfoLight** -bouton affiché un « i » dans un cercle des informations détaillées sur une lumière.
- **AddContact** -afficher le bouton comme bouton Ajouter un Contact.
- **Custom** -vous permet de personnaliser plusieurs caractéristiques du bouton.

Ensuite, vous définissez la taille à l’écran et l’emplacement du bouton. Exemple :

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Ensuite, définissez le titre du bouton. `UIButtons` diffèrent de celles de la plupart `UIKit` contrôles dans la mesure où ils ont un état, donc vous ne pouvez pas simplement changent le titre, vous devez le modifier pour une donnée `UIControlState`. Exemple :

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Ensuite, utilisez le `AllEvents` événement pour afficher de lorsque l’utilisateur a cliqué sur le bouton. Exemple :

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
> Bien qu’il soit possible d’affecter des actions telles que `TouchUpInside` à un `UIButton`, elle ne sera jamais appelée car Apple TV n’a pas une pression tactile écran ou de prendre en charge les événements tactiles. Vous devez toujours utiliser des événements tels que **AllEvents** ou **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Un style à un bouton

tvOS fournit plusieurs propriétés d’un `UIButton` qui peut être utilisé pour fournir son titre et appliquer un style avec des éléments tels que la couleur d’arrière-plan et des images.

<a name="Button-Titles" />

### <a name="button-titles"></a>Titres de bouton

Comme nous l’avons vu ci-dessus, `UIButtons` diffèrent de celles de la plupart `UIKit` contrôles dans la mesure où ils ont un état, donc vous ne pouvez pas simplement changent le titre, vous devez le modifier pour une donnée `UIControlState`. Exemple :

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Vous pouvez définir la couleur du titre pour le bouton à l’aide de la `SetTitleColor` (méthode). Exemple :

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Vous pouvez ajuster le titre d’ombre à l’aide de la `SetTitleShadowColor`. Exemple :

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Vous pouvez définir l’ombre du titre pour modifier à partir de *Engraved* à *relief* lorsque le bouton est mis en surbrillance en utilisant le code suivant :

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

En outre, vous pouvez utiliser avec attributs de texte comme le titre du bouton. Exemple :

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Images de bouton

Un `UIButton` peut avoir une image jointe à ce dernier et pouvez utiliser une image en tant que son arrière-plan.

Pour définir l’image d’arrière-plan d’un bouton pour une donnée `UIControlState`, utilisez le code suivant :

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Définir le `AdjustsImageWhenHiglighted` propriété `true` pour dessiner l’image plus claire lorsque le bouton est mis en surbrillance (il s’agit de la valeur par défaut). Définir le `AdjustsImageWhenDisabled` propriété `true` pour dessiner l’image plus sombre lorsque le bouton est désactivé (là encore, c’est la valeur par défaut).

Pour définir l’image affichée sur le bouton, utilisez le code suivant :

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Utilisez le `TintColor` propriété à définir une teinte de couleur qui est appliquée pour le titre et l’image du bouton. Pour les boutons de la `Custom` type, cette propriété n’a aucun effet, vous devez implémenter le `TintColor` comportement vous-même.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation des boutons à l’intérieur d’une application Xamarin.tvOS. Il vous a montré comment travailler avec des boutons dans le concepteur iOS et comment créer des boutons dans C# code. Enfin, il vous a montré comment modifier le titre d’un bouton et modifier son style et l’apparence.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
