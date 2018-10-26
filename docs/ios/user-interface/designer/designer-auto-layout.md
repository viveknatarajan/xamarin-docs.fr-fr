---
title: Disposition automatique avec le Concepteur de Xamarin pour iOS
description: Ce guide présente iOS disposition automatique et explique comment utiliser le Concepteur de Xamarin pour iOS pour créer et modifier des mises en page à l’aide de contraintes. Il aborde également les contraintes de modification dans le code, animer des modifications de la contrainte et bien plus encore.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cfb684df89bf800a0d32607b119d1bd3fc297c58
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123737"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Disposition automatique avec le Concepteur de Xamarin pour iOS

Disposition automatique (également appelée « disposition adaptatif ») est une approche de conception réactive. Contrairement au système de disposition transitoire, où les emplacement de chaque élément sont codé en dur à un point sur l’écran, la disposition automatique est sur *relations* -les positions d’éléments par rapport aux autres éléments sur l’aire de conception. Au cœur de la disposition automatique est l’idée des contraintes ou des règles qui définissent le placement d’un élément ou un ensemble d’éléments dans le contexte d’autres éléments à l’écran. Étant donné que les éléments ne sont pas liés à une position particulière sur l’écran, les contraintes permettent de créer une disposition adaptative qui vous semble correcte sur différentes tailles d’écran et les orientations de l’appareil.

Dans ce guide, nous présentons les contraintes et comment les utiliser dans le Concepteur de Xamarin iOS. Ce guide ne couvre pas l’utilisation des contraintes par programmation. Pour plus d’informations sur l’utilisation par programmation de la disposition automatique, reportez-vous à la [documentation Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Configuration requise

Le Concepteur de Xamarin pour iOS est disponible dans Visual Studio pour Mac dans Visual Studio 2015 et 2017 sur Windows.

Ce guide suppose la connaissance des composants du concepteur à partir de la [présentation du concepteur iOS](~/ios/user-interface/designer/introduction.md) guide.

## <a name="introduction-to-constraints"></a>Introduction aux contraintes

Une contrainte est une représentation mathématique de la relation entre deux éléments sur l’écran. Qui représente une interface utilisateur position de l’élément comme une relation mathématique résout plusieurs problèmes associés de coder en dur les emplacement d’un élément d’interface utilisateur. Par exemple, si vous deviez placer un 20px de bouton à partir du bas de l’écran en mode portrait, la position du bouton serait hors de l’écran en mode paysage. Pour éviter ce problème, nous avons défini une contrainte qui place le bord inférieur de la 20px de bouton à partir du bas de la vue. La position du bord de bouton puis sera calculée comme *button.bottom = view.bottom - 20px*, lequel placerait le 20px de bouton à partir du bas de la vue en mode portrait et paysage. L’aptitude à calculer la sélection élective selon une relation mathématique est ce qui rend les contraintes si utiles dans la conception d’interface utilisateur.

Lorsque nous avons une contrainte, nous créons un `NSLayoutConstraint` objet qui prend comme arguments les objets à limiter et les propriétés, ou *attributs*, qui agit sur la contrainte. Dans le concepteur iOS, les attributs incluent des bords comme le *gauche*, *droit*, *haut*, et *bas* d’un élément. Ils incluent également des attributs de la taille comme *hauteur* et *largeur*et l’emplacement, de point central *centerX* et *centerY*. Par exemple, lorsque nous ajoutons une contrainte sur la position de la limite gauche de deux boutons, le concepteur génère le code suivant en arrière-plan :

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

La section suivante aborde l’utilisation des contraintes à l’aide du concepteur, y compris l’activation et désactivation de disposition automatique et à l’aide de la barre d’outils de contraintes iOS.

## <a name="enable-auto-layout"></a>Activer la disposition automatique

La configuration de concepteur iOS par défaut a le mode de contrainte est activé. Toutefois, si vous avez besoin activer ou désactiver manuellement, vous pouvez le faire en deux étapes :

1.  Cliquez sur un espace vide sur l’aire de conception. Cela désélectionne tous les éléments et permet d’afficher les propriétés pour le document de la table de montage séquentiel.
1.  Activez ou désactivez le **mise en forme automatique Utilisez** case à cocher dans le panneau de propriétés :

    ![](designer-auto-layout-images/image01.png "La case à cocher Utiliser mise en forme automatique dans le panneau de propriétés")


Par défaut, aucune contrainte n’est créées ou visible sur l’aire. Au lieu de cela, ils sont automatiquement déduits à partir des informations de frame au moment de la compilation. Pour ajouter des contraintes, nous devons sélectionner un élément sur l’aire de conception et de lui ajouter des contraintes. Nous pouvons le faire en utilisant le **barre d’outils de la contrainte**.

## <a name="constraints-toolbar"></a>Barre d’outils de contraintes

 [![](designer-auto-layout-images/toolbarnew.png "Les commandes de Menu contextuel")](designer-auto-layout-images/toolbarnew.png#lightbox)

La barre d’outils de contraintes a été mis à jour et maintenant se compose de deux parties principales :

- **Un élément de bascule du bouton mode contraintes**: auparavant, vous avez entré le mode de contraintes en cliquant à nouveau sur une vue sélectionnée dans l’aire de conception. Vous devez désormais utiliser ce bouton bascule dans la barre de contraintes :

  ![Activer/désactiver des modes de contraintes](designer-auto-layout-images/constraints.png)

- **Un bouton « Contraintes de mise à jour » :** il est important de noter que les modifications selon que vous êtes en mode de modification de contraintes.
  - En mode de modification de contrainte, ce bouton s’ajuste les contraintes pour faire correspondre l’image de l’élément.
  - En mode d’édition de Frame ce bouton ajuste le cadre de l’élément pour correspondre à la position, que définissant les contraintes.


## <a name="surface-based-constraint-editing"></a>Contrainte basée sur la surface de modification

Dans la section précédente, nous avons appris à ajouter des contraintes par défaut et supprimer des contraintes à l’aide de la barre d’outils de contraintes. Pour la modification de contrainte enrichi plus, nous pouvons interagir avec contraintes directement sur l’aire de conception. Cette section présente les principes de base de contrainte basée sur la surface d’édition, notamment l’espacement du code confidentiel contrôles, zones de dépôt, de travailler avec différents types de contraintes.

### <a name="creating-constraints"></a>Création de contraintes

L’outil de concepteur iOS propose deux types de contrôles pour manipuler des éléments sur l’aire de conception. *En faisant glisser des contrôles* et *des contrôles de code confidentiel-espacement*, comme illustré dans l’image suivante :

![contrôles d’affichage](designer-auto-layout-images/controls.png)

Ceux-ci sont activés en sélectionnant le bouton de mode de contraintes dans la barre de contraintes.

Définissent les handles en forme de T 4 sur chaque côté de l’élément le *haut*, *droit*, *bas*, et *gauche* bords de l’élément pour un contrainte. Définissent les deux poignées j’en forme à droite et en bas de l’élément *hauteur* et *largeur* contrainte respectivement. Le carré central gère les deux *centerX* et *centerY* contraintes.

Pour créer une contrainte, choisir un handle et faites glisser quelque part sur l’aire de conception. Lorsque vous démarrez l’opération glisser, une série de lignes/zones vertes s’affiche sur l’aire de vous indiquer les éléments que vous pouvez contraindre. Par exemple, dans la capture d’écran ci-dessous, nous allons contraindre le côté supérieur du bouton du milieu :

 [![](designer-auto-layout-images/image07.png "Limiter le côté supérieur du bouton du milieu")](designer-auto-layout-images/image07.png#lightbox)

Notez les trois lignes vertes en pointillés entre les deux autres boutons. Les lignes vertes indiquent *zones de dépôt*, ou les attributs des autres éléments à laquelle nous pouvons limiter. Dans la capture d’écran ci-dessus, les deux autres boutons offrent 3 zones de dépôt vertical ( *bas*, *centerY*, *haut*) pour contraindre notre bouton. La ligne en pointillés verte en haut de la vue signifie que le contrôleur d’affichage offre une contrainte en haut de la vue et l’encadré vert solide signifie que le contrôleur d’affichage offre une contrainte ci-dessous le repère de disposition supérieur.

> [!IMPORTANT]
> Repères de disposition sont des types spéciaux de cibles de contrainte qui nous permettent de créer de haut et de contraintes bas qui prennent en compte la présence des barres du système, telles que les barres d’état ou les barres d’outils. Une des principales utilisations consiste à disposer d’une application compatible entre iOS 6 et iOS 7 dans la mesure où la version la plus récente a la vue du conteneur qui s’étend au-dessous de la barre d’état. Pour plus d’informations sur le guide de présentation en haut, reportez-vous à la [documentation Apple](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



Les trois sections suivantes présentent de travailler avec différents types de contraintes.

### <a name="size-constraints"></a>Contraintes de taille

Avec les contraintes de taille - *hauteur* et *largeur* -vous avez deux options. La première option consiste à faire glisser la poignée pour limiter à une taille d’élément voisin, comme illustré dans l’exemple ci-dessus. L’autre option consiste à double-cliquer sur la poignée pour créer une contrainte de self. Cela nous permet de spécifier une valeur de taille constante, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/sizec.png "Faites glisser la poignée pour limiter à une taille d’élément voisin, comme illustré ici")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Contraintes de centre

La poignée carrée créera un *centerX* ou *centerY* contrainte, en fonction du contexte. En faisant glisser la poignée carrée égayer les autres éléments pour offrir les deux zones de dépôt de vertical et horizontal, comme l’illustre la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/centerc.png "Contraintes de centre")](designer-auto-layout-images/centerc.png#lightbox)

Si vous choisissez une zone de dépôt vertical, un *centerY* contrainte sera créée. Si vous choisissez une zone de dépôt horizontale, la contrainte doit reposer sur *centerX*.

### <a name="combinational-constraints"></a>Contraintes COMBINATOIRES

Pour créer l’alignement et contraintes de l’égalité de taille entre deux éléments, vous pouvez sélectionner des éléments à partir d’une barre d’outils supérieure pour spécifier - dans l’ordre : l’alignement horizontal, alignement vertical et égalités de taille, comme l’illustre la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/image06.png "Contraintes COMBINATOIRES")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Visualisation et la modification des contraintes

Lorsque vous ajoutez une contrainte, il s’affichera sur l’aire de conception en tant qu’une ligne bleue lorsque vous sélectionnez un élément :

 [![](designer-auto-layout-images/image09.png "Visualisation des contraintes")](designer-auto-layout-images/image09.png#lightbox)

Vous pouvez sélectionner une contrainte en cliquant sur une ligne bleue et en modifiant les valeurs de contrainte directement dans le panneau de la propriété. Vous pouvez également double-cliquer sur une ligne bleue s’affiche un menu qui vous permet de modifier les valeurs directement sur l’aire de conception :

 [![](designer-auto-layout-images/image08.png "Modification des contraintes")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Problèmes de contrainte

Plusieurs types de problèmes peuvent survenir lors de l’utilisation de contraintes :

-  **Des contraintes en conflit** : cela se produit lorsque plusieurs contraintes forcer l’élément peut avoir des valeurs en conflit pour un attribut et le moteur de contrainte ne parvient pas à les rapprocher.
-  **Sous-restreint éléments** : les propriétés d’un élément (emplacement + taille) doivent être couvert entièrement par son jeu de contraintes et des tailles intrinsèques pour les contraintes être valide. Si ces valeurs sont ambiguës, l’élément est dit sous-restreint.
-  **Frame dues** : cela se produit lorsque le frame d’un élément et son ensemble de contraintes définissent deux rectangles qui en résulte différentes.


Cette section décrit plus en détail les trois problèmes répertoriés ci-dessus et fournit des détails sur la façon de les gérer.

### <a name="conflicting-constraints"></a>Contraintes en conflit

Des contraintes en conflit sont marqués en rouge et un symbole d’avertissement. Vous placez le curseur sur les symboles d’avertissement affiche un menu avec des informations sur le conflit :

 [![](designer-auto-layout-images/image11.png "Des contraintes en conflit avertissement")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Chargement des éléments

Chargement des éléments s’affichent en orange et déclenchent l’apparence d’une icône orange de marqueur dans la barre de l’objet de contrôleur de vue :

 [![](designer-auto-layout-images/image02.png "Chargement des éléments s’affichent en orange")](designer-auto-layout-images/image02.png#lightbox)

Si vous cliquez sur cette icône de marqueur, vous pouvez obtenir des informations sur les éléments de chargement dans la scène et résoudre les problèmes par soit entièrement leur contrainte ou en supprimant leurs contraintes, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/image10.png "Correction de chargement des éléments")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Frame dues

Erreur de trame utilise le même code de couleur en tant qu’éléments de chargement. L’élément sera toujours affiché sur la surface à l’aide de son frame natif, mais dans le cas d’une erreur de trame un rectangle rouge marquera où l’élément se retrouve lorsque l’application s’exécute, comme illustré par la capture d’écran ci-dessous :

 [![](designer-auto-layout-images/image05.png "Exemple de vue Frame dues")](designer-auto-layout-images/image05.png#lightbox)

Pour résoudre les erreurs de trame dues, sélectionnez le **mise à jour les Frames basée sur les contraintes** bouton à partir de la barre d’outils de contraintes (bouton à droite) :

 [![](designer-auto-layout-images/image03.png "Mettre à jour des images basées sur le bouton de barre d’outils de contraintes")](designer-auto-layout-images/image03.png#lightbox)

Cela s’ajuste automatiquement le cadre de l’élément pour correspondre à des positions définies par les contrôles.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Modification des contraintes dans le Code

En fonction des exigences de votre application, il peut arriver que vous deviez modifier une contrainte dans le code. Par exemple, pour la vue de déplacer ou redimensionner une contrainte est rattachée, pour modifier la priorité d’une contrainte ou de désactiver une contrainte complètement.

Pour accéder à une contrainte dans le code, vous devez d’abord exposer dans le concepteur iOS en procédant comme suit :

1. Créer la contrainte comme d’habitude (en utilisant l’une des méthodes répertoriées ci-dessus).
2. Dans le **Explorer de structure du Document**, trouver la contrainte et sélectionnez-la :

    [![](designer-auto-layout-images/modify01.png "L’Explorateur de structure du Document")](designer-auto-layout-images/modify01.png#lightbox)
3. Ensuite, affectez un **nom** à la contrainte dans le **Widget** onglet de la **l’Explorateur de propriétés**:

    [![](designer-auto-layout-images/modify02.png "L’onglet de Widget")](designer-auto-layout-images/modify02.png#lightbox)
4. Enregistrez les modifications apportées.

Avec les modifications ci-dessus en place, vous pouvez accéder à la contrainte dans le code et modifier ses propriétés. Par exemple, vous pouvez utiliser les éléments suivants pour définir la hauteur de la vue attachée à zéro :

```csharp
ViewInfoHeight.Constant = 0;
```

Étant donné le paramètre suivant pour la contrainte dans le concepteur iOS :

[![](designer-auto-layout-images/modify03.png "Modification d’une contrainte dans l’Explorateur de propriétés")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>La phase de disposition différée

Au lieu d’instantanément la mise à jour de la vue attachée en réponse aux modifications de la contrainte, le moteur de disposition automatique planifie un _passe de disposition différé_ dans le futur proche. Pendant ce passage différé, est non seulement de contrainte de la vue donnée de mise à jour, les contraintes pour chaque vue dans la hiérarchie sont recalculés et mis à jour pour ajuster la nouvelle disposition.

À tout moment, vous pouvez planifier votre propre passe de disposition différé en appelant le `SetNeedsLayout` ou `SetNeedsUpdateConstraints` méthodes de la vue parent. 

La phase de disposition différés consistant en deux passes uniques via la hiérarchie d’affichage :

- **Le test de mise à jour** -dans cette étape, le moteur de disposition automatique parcourt la hiérarchie d’affichage et appelle le `UpdateViewConstraints` (méthode) sur tous les contrôleurs d’affichage et le `UpdateConstraints` (méthode) sur toutes les vues.
- **La phase de disposition** - là encore, le moteur de disposition automatique parcourt la hiérarchie d’affichage, mais cette fois appelle le `ViewWillLayoutSubviews` (méthode) sur tous les contrôleurs d’affichage et le `LayoutSubviews` (méthode) sur toutes les vues. Le `LayoutSubviews` méthode met à jour le `Frame` propriété de chaque sous-affichage avec le rectangle calculé par le moteur de disposition automatique.

### <a name="animating-constraint-changes"></a>Animer des modifications de contrainte

En plus de modifier les propriétés de contrainte, vous pouvez utiliser Core Animation pour animer des modifications de l’affichage de contraintes. Exemple :

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

Ce guide a introduit automatique de l’iOS (ou « adaptive ») mise en page et le concept de contraintes en tant que représentations mathématiques des relations entre les éléments sur l’aire de conception. Il décrit comment activer la disposition automatique dans le concepteur iOS, utilisation de la **barre d’outils de contraintes**et la modification des contraintes individuellement sur l’aire de conception. Ensuite, vous avez appris comment résoudre les trois problèmes courants de contraintes. Enfin, il vous a montré comment modifier des contraintes dans le code.

## <a name="related-links"></a>Liens associés

- [Introduction aux storyboards](~/ios/user-interface/storyboards/index.md)
- [procédure pas à pas de contrôles concevable d’iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Vue d’ensemble du concepteur Android](~/android/user-interface/android-designer/index.md)
- [Contraintes par programmation](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple - Guide de mise en page automatique](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
