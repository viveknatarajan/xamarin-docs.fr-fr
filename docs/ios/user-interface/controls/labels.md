---
title: Étiquettes dans Xamarin.iOS
description: Ce document explique comment utiliser des étiquettes dans Xamarin.iOS. Il décrit comment créer des étiquettes par programmation et avec le concepteur iOS.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: cca74ac74e5077822193f6dd97a69f8d9b823561
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233287"
---
# <a name="labels-in-xamarinios"></a>Étiquettes dans Xamarin.iOS

Le `UILabel` contrôle est utilisé pour l’affichage unique et plusieurs lignes, texte en lecture seule. 

## <a name="implementing-a-label"></a>Implémentation d’une étiquette

Une nouvelle étiquette est créée en instanciant un [ `UILabel` ](xref:UIKit.UILabel):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Étiquettes et les tables de montage séquentiel

Vous pouvez également ajouter une étiquette à votre interface utilisateur lors de l’utilisation du concepteur iOS. Recherchez **étiquette** dans le **boîte à outils** et faites-le glisser vers votre vue :

![L’étiquette dans la boîte à outils](labels-images/image3.png)

Les propriétés suivantes peuvent être ajustées pour le panneau Propriétés :

![Panneau Propriétés de l’étiquette](labels-images/image2.png)

- **Contexte du texte** – brut ou avec attributs. Texte brut vous permet de définir le [mise en forme des attributs](#Formatting_Text_and_Label) sur la chaîne entière. Textes avec attributs vous permet de définir la mise en forme des caractères différents ou des mots dans la chaîne.
- **Alignement de la couleur, la police,** – mise en forme des attributs qui peuvent être appliquées à l’étiquette.
- **Lignes** : définit le nombre de lignes de l’étiquette peut s’étendre sur. Définissez ce paramètre sur 0 pour permettre à l’étiquette à utiliser autant de lignes en fonction des besoins.
- **Comportement** – peut être définie sur activée ou en surbrillance. Activé est définie par défaut, le texte désactivé apparaît dans une couleur grise plus claire. Mise en surbrillance est désactivée par défaut et permet l’étiquette à être redessiné avec un état mis en surbrillance lorsqu’il est sélectionné par un utilisateur.
- **Baselane et saut de ligne** : 
    - La ligne de base détermine la façon dont le texte sera positionné si les tailles de police est différente de celle spécifiée.
    - Sauts de ligne déterminent la façon dont une chaîne sera encapsulée ou tronquée si elle est supérieure à une seule ligne.
- **Autoshrink** : détermine la façon dont la taille de police sera réduite au sein d’une étiquette, si nécessaire.
- **Mise en surbrillance, ombre, décalage** : vous permet de définir la couleur en surbrillance et les clichés instantanés, et le décalage de l’ombre.

## <a name="truncating-and-wrapping"></a>Troncation et d’habillage

Pour plus d’informations sur l’utilisation de la ligne s’arrête dans iOS, reportez-vous à la [tronquer et habiller le texte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) Recipe (Recette).

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Mise en forme de texte et étiquette

Pour mettre en forme la chaîne que vous utilisez dans une étiquette, vous pouvez soit définir les attributs sur l’ensemble de la chaîne de mise en forme ou vous pouvez utiliser des chaînes avec attributs. Les exemples suivants montrent comment implémenter ces :

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Pour plus d’informations sur l’utilisation de texte de style `NSAttributedString` reportez-vous à la [Style texte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) Recipe (Recette).

Par défaut, les étiquettes ont le `Enabled` défini sur true, mais il est possible de définir sur désactivé pour donner à l’utilisateur une indication qu’un contrôle est désactivé :

```csharp
label.Enabled = false;
```

Cela définit l’étiquette sur une couleur grise claire, comme illustré dans l’exemple d’image de l’écran de Restrictions dans iOS :

![Bouton désactivé dans iOS](labels-images/image1.png)

Vous pouvez également définir les couleurs de texte mise en surbrillance et les clichés instantanés pour votre texte d’étiquette pour des effets supplémentaires :

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Qui affiche le texte comme suit :

![Mise en surbrillance et les clichés instantanés de la définir sur du texte](labels-images/image4.png)

Pour plus d’informations sur la modification de la police d’un UILabel, reportez-vous à la [modifier la police le](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) Recipe (Recette).





