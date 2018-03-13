---
title: "Étiquettes"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 0fdeecc4465aa5709b452ef0b591ec4e5c262e3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="labels"></a>Étiquettes

Le `UILabel` contrôle est utilisé pour l’affichage unique et plusieurs lignes, texte en lecture seule. 

## <a name="implementing-a-label"></a>Implémentation d’une étiquette

Une nouvelle étiquette est créée en instanciant une [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Les étiquettes et les animations

Vous pouvez également ajouter une étiquette à votre interface utilisateur lorsque vous utilisez le concepteur iOS. Recherchez **étiquette** dans les **boîte à outils** et faites-le glisser vers la vue :

![Attribuer une étiquette dans la boîte à outils](labels-images/image3.png)

Les propriétés suivantes peuvent être ajustées pour la zone de propriétés :

![Volet Propriétés de l’étiquette](labels-images/image2.png)

- **Contexte du texte** – format brut ou avec attributs. Texte brut vous permet de définir la [mise en forme des attributs](#Formatting_Text_and_Label) sur la chaîne entière. Textes avec attributs vous permet de définir la mise en forme à des caractères différents ou des mots dans la chaîne.
- **Alignement de la couleur, la police,** – mise en forme les attributs qui peuvent être appliqués à l’étiquette.
- **Lignes** : définit le nombre de lignes qui peut couvrir l’étiquette. Définissez cette propriété sur 0 pour autoriser l’étiquette à utiliser autant de lignes en fonction des besoins.
- **Comportement** – peut être définie sur activée ou en surbrillance. Activé est définie par défaut, texte désactivé s’affichera dans une couleur grise plus claire. Mise en surbrillance est désactivée par défaut et permet à l’étiquette à être redessiné avec un état mis en surbrillance lorsqu’il est sélectionné par un utilisateur.
- **Baselane et saut de ligne** : 
    - La ligne de base détermine la façon dont le texte est positionné si les tailles de police est différente de celle spécifiée.
    - Sauts de ligne déterminent comment une chaîne sera encapsulée ou tronquée si elle est supérieure à une seule ligne.
- **Autoshrink** – détermine la façon dont la taille de police sera réduite au sein d’une étiquette, si nécessaire.
- **Mise en surbrillance, ombre, décalage** – permet de définir une couleur Hightlighted et les clichés instantanés, et le décalage de l’ombre.

## <a name="truncating-and-wrapping"></a>Troncation et de renvoi à la ligne

Pour plus d’informations sur l’utilisation de la ligne s’arrête dans iOS, reportez-vous à la [tronquer et habiller le texte](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) recette.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Mise en forme de texte et étiquette

Pour mettre en forme la chaîne que vous utilisez dans une étiquette vous pouvez soit définir les attributs sur l’ensemble de la chaîne de mise en forme, ou vous pouvez utiliser des chaînes avec attributs. Les exemples suivants montrent comment implémenter ces :

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

Pour plus d’informations sur l’utilisation de texte style `NSAttributedString` reportez-vous à la [du Style du texte](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) recette.

Par défaut, les étiquettes ont le `Enabled` défini sur true, mais il est possible de lui affectez la valeur désactivé pour donner une indication qu’un contrôle est désactivé à l’utilisateur :

```csharp
label.Enabled = false;
```

Cela définit l’étiquette pour une couleur grise claire, comme illustré dans l’image d’exemple suivante de l’écran de Restrictions dans iOS :

![Bouton désactivé dans iOS](labels-images/image1.png)

Vous pouvez également définir les couleurs de texte en surbrillance et les clichés instantanés pour votre texte d’étiquette pour des effets supplémentaires :

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Qui affiche le texte comme suit :

![Mise en surbrillance et les clichés instantanés de la définir sur du texte](labels-images/image4.png)

Pour plus d’informations sur la modification de la police d’un UILabel, reportez-vous à la [modifier la police du](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) recette.





