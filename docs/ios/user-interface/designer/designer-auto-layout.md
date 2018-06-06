---
title: Disposition automatique avec le Concepteur de Xamarin pour iOS
description: Ce guide présente les e/s mise en forme automatique et explique comment utiliser le Concepteur de Xamarin pour iOS pour créer et modifier la disposition à l’aide des contraintes. Il traite également des contraintes de modification dans le code, l’animation et les modifications de la contrainte.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 876bf3de19d2bcce7d951facc92d5b05a928cd38
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790199"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Disposition automatique avec le Concepteur de Xamarin pour iOS

Mise en page automatique (également appelé « disposition adaptive ») est une approche de conception réactive. Contrairement au système de disposition transitoire, où les emplacement de chaque élément sont codé en dur à un point sur l’écran, la mise en page automatique est sur *relations* -la position des éléments par rapport aux autres éléments sur l’aire de conception. Au cœur de la mise en page automatique est l’idée des contraintes ou des règles qui définissent le placement d’un élément ou un ensemble d’éléments dans le contexte des autres éléments sur l’écran. Étant donné que les éléments ne sont pas liés à un emplacement spécifique sur l’écran, les contraintes permettent de créer une disposition adaptive convenable sur différentes tailles d’écran et les orientations de l’appareil.

Dans ce guide, nous introduisons les contraintes et comment les utiliser dans le Concepteur de Xamarin iOS. Ce guide ne couvre pas l’utilisation de contraintes par programme. Pour plus d’informations sur l’utilisation par programme de mise en page automatique, reportez-vous à la la [documentation Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Configuration requise

Le Concepteur de Xamarin pour iOS est disponible dans Visual Studio pour Mac dans Visual Studio 2015 et 2017 sur Windows.

Ce guide suppose la connaissance des composants du concepteur à partir de la [présentation dans le concepteur iOS](~/ios/user-interface/designer/introduction.md) guide.

## <a name="introduction-to-constraints"></a>Introduction aux contraintes

Une contrainte est une représentation mathématique de la relation entre deux éléments sur l’écran. Qui représente une interface utilisateur position de l’élément comme une relation mathématique résout plusieurs problèmes avec le codage en dur emplacement de l’élément d’une interface utilisateur. Par exemple, si nous devions pour placer un 20px bouton au bas de l’écran en mode portrait, la position du bouton serait désactivé l’écran en mode paysage. Pour éviter cela, nous avons défini une contrainte qui place le bord inférieur de la 20px bouton au bas de la vue. La position du bord de bouton puis sera calculée comme *button.bottom = view.bottom - 20px*, qui serait placer le bouton 20px au bas de la vue en mode portrait et paysage. La possibilité de calculer la sélection élective basée sur une relation mathématique est ce qui rend les contraintes donc utile dans la conception de l’interface utilisateur.

Quand nous définissons une contrainte, nous créons un `NSLayoutConstraint` objet qui prend comme arguments pour les objets à être contraint et les propriétés, ou *attributs*, qui agit sur la contrainte. Dans le concepteur iOS, les attributs incluent des bords, tels que les *gauche*, *droit*, *haut*, et *bas* d’un élément. Ils comprennent également des attributs de taille comme *hauteur* et *largeur*et le centre de l’emplacement, du point de *centerX* et *centerY*. Par exemple, lorsque nous ajoutons une contrainte sur la position de la limite gauche de deux boutons, le concepteur génère le code suivant en arrière-plan :

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

La section suivante couvre l’utilisation des contraintes à l’aide du concepteur, y compris l’activation et désactivation de disposition automatique et à l’aide de la barre d’outils de contraintes d’iOS.

## <a name="enable-auto-layout"></a>Activer la mise en page automatique

La configuration de concepteur par défaut iOS contrainte mode est activée. Toutefois, si vous avez besoin activer ou désactiver manuellement, vous pouvez le faire en deux étapes :

1.  Cliquez sur un espace vide sur l’aire de conception. Cela désactive tous les éléments et affiche les propriétés pour le document de plan conceptuel.
1.  Activez ou désactivez le **utilisez Autolayout** case à cocher dans le panneau de configuration de propriété :

    ![](designer-auto-layout-images/image01.png "La case à cocher Utiliser mise en forme automatique dans le panneau de configuration de propriété")


Par défaut, aucune contrainte n’est créés ou visible sur l’aire. Au lieu de cela, ils sont déduits automatiquement les informations de frame au moment de la compilation. Pour ajouter des contraintes, nous devons sélectionner un élément sur l’aire de conception et de lui ajouter des contraintes. Nous pouvons faire que l’utilisation du **barre d’outils de la contrainte**.

## <a name="constraints-toolbar"></a>Barre d’outils de contraintes

 [![](designer-auto-layout-images/toolbarnew.png "Les commandes de Menu contextuel")](designer-auto-layout-images/toolbarnew.png#lightbox)

La barre d’outils de contraintes a été mis à jour et maintenant se compose de deux parties principales :

- **Un élément de bascule du bouton mode contraintes**: auparavant, vous avez entré le mode de contraintes en cliquant de nouveau sur une vue sélectionnée sur l’aire de conception. Vous devez désormais utiliser ce bouton bascule dans la barre de contraintes :

  ![Activer/désactiver des modes de contraintes](designer-auto-layout-images/constraints.png)

- **Un bouton « Contraintes de mise à jour » :** qu’il est important de noter que les modifications selon que vous êtes en mode de modification de contraintes.
  - En mode de modification de contrainte, ce bouton ajuste les contraintes pour faire correspondre l’image de l’élément.
  - Dans le cadre de mode d’édition, ce bouton ajuste le cadre de l’élément pour correspondre à la position, que définissant les contraintes.


## <a name="surface-based-constraint-editing"></a>Modification de la restriction basés sur la surface

Dans la section précédente, nous avons appris à ajouter des contraintes par défaut et supprimer des contraintes à l’aide de la barre d’outils de contraintes. Pour plus de modification de contrainte ajustée, pouvoir interagir avec des contraintes directement sur l’aire de conception. Cette section présente les notions de base de la modification de la restriction basés sur la surface, y compris les contrôles espacement de code confidentiel, zones de dépôt et comment utiliser les différents types de contraintes.

### <a name="creating-constraints"></a>Création de contraintes

L’outil de concepteur iOS offre deux types de contrôles pour manipuler des éléments sur l’aire de conception. *En faisant glisser des contrôles* et *des contrôles de code confidentiel-espacement*, comme illustré dans l’image suivante :

![contrôles d’affichage](designer-auto-layout-images/controls.png)

Ceux-ci sont activés en sélectionnant le bouton de mode de contraintes dans la barre de contraintes.

Les poignées en forme de T 4 sur chaque côté de l’élément de définissent la *haut*, *droit*, *bas*, et *gauche* bords de l’élément pour un contrainte. Les deux poignées I en forme à droite et en bas de l’élément définissent *hauteur* et *largeur* contrainte respectivement. Le carré central gère les deux *centerX* et *centerY* contraintes.

Pour créer une contrainte, choisir une poignée et faites-la glisser quelque part sur l’aire de conception. Lorsque vous démarrez l’opération glisser, une série de lignes/zones vertes apparaît sur l’aire de vous indiquant que vous pouvez limiter les. Par exemple, la capture d’écran ci-dessous, nous sommes en limitant le côté supérieur du bouton central :

 [![](designer-auto-layout-images/image07.png "En limitant le côté supérieur du bouton central")](designer-auto-layout-images/image07.png#lightbox)

Notez les trois lignes vertes en pointillés entre les deux autres boutons. Les lignes vertes indiquent *zones de dépôt*, ou les attributs des autres éléments à laquelle nous pouvons limiter. Dans la capture d’écran ci-dessus, les deux autres boutons offrent les zones de dépôt vertical 3 ( *bas*, *centerY*, *haut*) pour contraindre notre bouton. La ligne en pointillés verte en haut de la vue signifie que le contrôleur de la vue offre une contrainte en haut de la vue, et la zone verte unie signifie que le contrôleur de la vue offre une contrainte ci-dessous le guide de présentation en haut.

> [!IMPORTANT]
> Guides de mise en page sont des types spéciaux de cibles de contrainte qui nous permettent de créer en haut et les contraintes de bas qui tiennent compte de la présence des barres du système, telles que les barres d’état ou les barres d’outils. Une des principales utilisations consiste à avoir une application compatible entre iOS 6 et iOS 7 étant donné que la version la plus récente a la vue du conteneur qui s’étend au-dessous de la barre d’état. Pour plus d’informations sur le guide de présentation en haut, reportez-vous à la [documentation Apple](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



Les trois sections suivantes présentent travailler avec différents types de contraintes.

### <a name="size-constraints"></a>Contraintes de taille

Avec les contraintes de taille - *hauteur* et *largeur* -vous avez deux options. La première option consiste à faire glisser la poignée pour limiter à une taille d’élément voisin, comme illustré dans l’exemple ci-dessus. L’autre option est de double-cliquer sur la poignée pour créer une contrainte de libre-service. Cela nous permet de spécifier une valeur de taille constante, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/sizec.png "Faites glisser la poignée pour limiter à une taille d’élément voisin, comme illustré ici")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Centre des contraintes

La poignée carrée créera un *centerX* ou *centerY* contrainte, en fonction du contexte. En faisant glisser la poignée carrée s’allument les autres éléments pour offrir les deux zones de dépôt de verticales et horizontales, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/centerc.png "Centre des contraintes")](designer-auto-layout-images/centerc.png#lightbox)

Si vous choisissez une zone de liste verticale, un *centerY* contrainte sera créée. Si vous choisissez une zone de dépôt horizontale, la contrainte doit reposer sur *centerX*.

### <a name="combinational-constraints"></a>Contraintes COMBINATOIRES

Pour créer l’alignement et contraintes de l’égalité de taille entre deux éléments, vous pouvez sélectionner des éléments à partir d’une barre d’outils supérieure spécifier - que - alignement horizontal, alignement vertical et égalités de taille, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/image06.png "Contraintes COMBINATOIRES")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Visualisation et la modification des contraintes

Lorsque vous ajoutez une contrainte, il s’affichera sur l’aire de conception en tant qu’une ligne bleue lorsque vous sélectionnez un élément :

 [![](designer-auto-layout-images/image09.png "Visualisation des contraintes")](designer-auto-layout-images/image09.png#lightbox)

Vous pouvez sélectionner une contrainte en cliquant sur une ligne bleue et de modifier les valeurs de contrainte directement dans le panneau de configuration de propriété. Vous pouvez également double-cliquer sur une ligne bleue affiche un popover qui vous permet de modifier les valeurs directement sur l’aire de conception :

 [![](designer-auto-layout-images/image08.png "Modification des contraintes")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Problèmes de contrainte

Plusieurs types de problèmes peuvent survenir lors de l’utilisation de contraintes :

-  **Des contraintes en conflit** : cela se produit lorsque plusieurs contraintes de forcer l’élément peut avoir des valeurs en conflit pour un attribut et le moteur de contrainte ne peut pas rapprocher les.
-  **Sous-restreint éléments** : les propriétés d’un élément (emplacement + taille) doivent être couvert entièrement par son jeu de contraintes et des tailles intrinsèques pour les contraintes doivent être valides. Si ces valeurs sont ambiguës, l’élément est dit sous-restreint.
-  **Erreur de trame** : cela se produit lorsque les frames d’un élément et son jeu de contraintes définissent deux rectangles qui en résulte différentes.


Cette section présente les trois problèmes répertoriés ci-dessus et fournit des détails sur la façon de les gérer.

### <a name="conflicting-constraints"></a>Contraintes en conflit

Des contraintes en conflit sont marqués en rouge et un symbole d’avertissement. Vous pointez sur les symboles d’avertissement affiche une popover avec des informations sur le conflit :

 [![](designer-auto-layout-images/image11.png "Des contraintes en conflit avertissement")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Chargement des éléments

Chargement des éléments s’affichent en orange et déclenchent l’apparence d’une icône orange de marqueur dans la barre de l’objet de vue contrôleur :

 [![](designer-auto-layout-images/image02.png "Chargement des éléments s’affichent en orange")](designer-auto-layout-images/image02.png#lightbox)

Si vous cliquez sur cette icône du marqueur, vous pouvez obtenir des informations sur les éléments de chargement de la scène et résoudre les problèmes par soit entièrement les contraindre ou en supprimant des contraintes, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/image10.png "Correction de chargement des éléments")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Erreur de trame

Erreur de trame utilise le même code de couleur en tant qu’éléments de chargement. L’élément sera toujours rendu sur la surface à l’aide de son frame natif, mais dans le cas d’une erreur de trame un rectangle rouge marquera où l’élément se retrouve lorsque l’application s’exécute, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/image05.png "Exemple de vue Frame dues")](designer-auto-layout-images/image05.png#lightbox)

Pour résoudre les erreurs de trame dues, sélectionnez le **mise à jour de Frames en fonction de contraintes** bouton à partir de la barre d’outils de contraintes (bouton à droite) :

 [![](designer-auto-layout-images/image03.png "Mettre à jour des Frames basés sur le bouton de barre d’outils de contraintes")](designer-auto-layout-images/image03.png#lightbox)

Il ajuste automatiquement le cadre de l’élément pour correspondent aux positions définies par les contrôles.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Modification des contraintes dans le Code

Selon les besoins de votre application, il peut arriver que vous deviez modifier une contrainte dans le code. Par exemple, pour l’affichage de déplacer ou redimensionner une contrainte est rattachée, pour modifier la priorité d’une contrainte ou de désactiver une contrainte de complètement.

Pour accéder à une contrainte dans le code, vous devez d’abord l’exposer dans le concepteur iOS en procédant comme suit :

1. Créer la contrainte normalement (à l’aide d’une des méthodes répertoriées ci-dessus).
2. Dans le **Explorer de structure du Document**, trouver la contrainte et sélectionnez-la :

    [![](designer-auto-layout-images/modify01.png "L’Explorateur de structure du Document")](designer-auto-layout-images/modify01.png#lightbox)
3. Ensuite, affectez un **nom** à la contrainte dans le **Widget** onglet de la **l’Explorateur de propriétés**:

    [![](designer-auto-layout-images/modify02.png "L’onglet du Widget")](designer-auto-layout-images/modify02.png#lightbox)
4. Enregistrez les modifications apportées.

Avec les modifications ci-dessus en place, vous pouvez accéder à la contrainte dans le code et modifier ses propriétés. Par exemple, vous pouvez utiliser les éléments suivants pour définir la hauteur de la vue attachée à zéro :

```csharp
ViewInfoHeight.Constant = 0;
```

Étant donné le paramètre suivant pour la contrainte dans le concepteur iOS :

[![](designer-auto-layout-images/modify03.png "Modification d’une contrainte dans l’Explorateur de propriétés")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>La passe de disposition différée

Au lieu d’instantanément la mise à jour de la vue attachée en réponse aux modifications de la contrainte, le moteur de disposition automatique planifie un _passe de disposition différé_ dans le futur proche. Pendant cette étape différée, est non seulement de contrainte de la vue donnée de mise à jour, les contraintes pour chaque vue dans la hiérarchie sont recalculées et mis à jour pour l’adapter à la nouvelle disposition.

À tout moment, vous pouvez planifier votre propre passe de disposition différé en appelant le `SetNeedsLayout` ou `SetNeedsUpdateConstraints` méthodes de la vue parent. 

La passe de disposition différé se compose de deux passes uniques dans la hiérarchie de la vue :

- **Le test de mise à jour** -dans cette étape, le moteur de disposition automatique parcourt la hiérarchie de la vue et appelle le `UpdateViewConstraints` méthode sur tous les contrôleurs de la vue et le `UpdateConstraints` méthode sur toutes les vues.
- **La passe de disposition** - là encore, le moteur de disposition automatique parcourt la hiérarchie de la vue, mais cette fois appelle la `ViewWillLayoutSubviews` méthode sur tous les contrôleurs de la vue et le `LayoutSubviews` méthode sur toutes les vues. Le `LayoutSubviews` méthode met à jour le `Frame` propriété de chaque sous-affichage avec le rectangle calculé par le moteur de disposition automatique.

### <a name="animating-constraint-changes"></a>Animer des modifications de contrainte

Outre la modification des propriétés de contrainte, vous pouvez utiliser Core Animation pour animer les modifications apportées à des contraintes d’une vue. Exemple :

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

L’appel de la clé ici le `LayoutIfNeeded` méthode de la vue parent à l’intérieur du bloc de l’animation. Cela indique à la vue pour dessiner chaque « image » de l’emplacement animée ou de la modification de la taille. Sans cette ligne, la vue est simplement aligner sur la version finale sans animation.

## <a name="summary"></a>Récapitulatif

Ce guide introduite automatique de l’e/s (ou « adaptive ») mise en page et le concept des contraintes en tant que représentations mathématiques des relations entre les éléments sur l’aire de conception. Il décrit comment activer la mise en page automatique dans le concepteur iOS, utilisation de la **barre d’outils de contraintes**et la modification des contraintes individuellement sur l’aire de conception. Ensuite, il a expliqué comment résoudre les problèmes de trois problèmes courants de contraintes. Enfin, elle a montré comment modifier des contraintes dans le code.

## <a name="related-links"></a>Liens associés

- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
- [procédure pas à pas de contrôles possible d’iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Vue d’ensemble du concepteur Android](~/android/user-interface/android-designer/index.md)
- [Contraintes par programmation](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple - Guide de mise en page automatique](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
