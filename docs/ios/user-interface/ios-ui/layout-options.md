---
title: Option de disposition
ms.topic: article
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1e3139eb4c94264c91307f6f8a69b183f3bf7fa6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="layout-options"></a>Option de disposition

Il existe deux mécanismes différents pour contrôler la disposition lorsqu’une vue est redimensionnée ou pivotée :

-  **Redimensionnement automatique** – inspecteur le redimensionnement automatique dans le concepteur permet de définir la `AutoresizingMask` propriétés. Cela permet un contrôle ancrées sur les bords de leur conteneur et/ou corriger leur taille. Redimensionnement automatique fonctionne dans toutes les versions d’iOS. Cet exemple est décrit plus en détail ci-dessous
-  **Mise en forme automatique** – une fonctionnalité introduite dans iOS6 qui permet un contrôle affiné sur les relations entre les contrôles d’interface utilisateur. Cela permet de contrôle de la position des éléments par rapport aux autres éléments sur l’aire de conception. Cette rubrique est traitée plus en détail dans les [disposition automatique avec Xamarin iOS concepteur](~/ios/user-interface/designer/designer-auto-layout.md) guide.


## <a name="autosizing"></a>Redimensionnement automatique

Lorsqu’un utilisateur redimensionne une fenêtre, telles que la rotation quand l’appareil ainsi que les modifications de l’orientation, le système va se redimensionner automatiquement les vues à l’intérieur de cette fenêtre selon leurs règles de redimensionnement automatique. Ces règles peuvent être définies dans c# à l’aide de la `AutoresizingMask` propriété de la `UIView` ou dans le **propriétés remplissage** du concepteur, comme illustré ci-dessous iOS :

 [ ![](layout-options-images/image41.png "Visual Studio pour Mac Concepteur")](layout-options-images/image41.png)

Lorsqu’un contrôle est sélectionné, cela vous permet de spécifier manuellement l’emplacement et les dimensions du contrôle, ainsi que les choix **redimensionnement automatique** comportement. Comme illustré dans la capture d’écran ci-dessous, nous pouvons utiliser les connections springs dans le contrôle de redimensionnement automatique pour définir la relation de la vue sélectionnée à son parent :

 [ ![](layout-options-images/image42.png "Visual Studio pour Mac Concepteur")](layout-options-images/image42.png)

Ajuster une *ressort* entraîne l’affichage redimensionner en fonction de la largeur ou hauteur de la vue parent. Ajuster une *principale de train* permettront à la vue de maintenir une distance constante entre lui-même et sa vue parent, sur ce bord particulier.

Ces paramètres peuvent également être définies dans le code :

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Pour tester les paramètres de redimensionnement automatique, activer différents **Orientations de périphérique pris en charge** dans les options du projet :

 [ ![](layout-options-images/image43a.png "Paramètres de redimensionnement automatique")](layout-options-images/image43a.png)

Dans le code-behind, nous pouvons utiliser le code suivant, ce qui provoque les contrôles de deux texte redimensionner horizontalement :

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Nous pouvons également ajuster les contrôles à l’aide du concepteur. En sélectionnant les connections comme présenté ci-dessous entraîne l’image à rester aligné à droite sans être détourée la partie inférieure de la vue :

 [ ![](layout-options-images/autoresize.png "AutoRotation")](layout-options-images/autoresize.png)

Ces captures d’écran montrent comment les contrôles de déplacer ou redimensionnement eux-mêmes lors de la rotation de l’écran :

 [ ![](layout-options-images/image44a.png "AutoRotation")](layout-options-images/image44a.png)

Notez que l’affichage de texte et un champ de texte à la fois s’étire pour conserver la même gauche et droite des marges, en raison du `FlexibleWidth` paramètre. L’image a la supérieure et gauche marge flexible, ce qui signifie qu’il conserve les marges inférieure et droite, en conservant l’image dans la vue lors de la rotation de l’écran. Des dispositions complexes requièrent généralement une combinaison de ces paramètres sur tous les contrôles visibles pour assurer la cohérence de l’interface utilisateur et pour empêcher que les contrôles qui se chevauchent lorsque les limites de la vue modifiée (en raison de rotation ou de tout autre événement de redimensionnement).





## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
