---
title: Fonctionnalités de conception matérielle de Concepteur de Xamarin.Android
description: Cette rubrique décrit les fonctionnalités de concepteur qui le rendent plus facile pour les développeurs créer des dispositions conformes Material Design. Cette section présente et explique comment utiliser la grille de matériau, la Palette de couleurs de matériau, l’échelle typographiques et l’éditeur de thème.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: eb636c3b7a41adbab9162e192ead65def377a1a0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118979"
---
# <a name="xamarinandroid-designer-material-design-features"></a>Fonctionnalités de concepteur Material Design Xamarin.Android

_Cette rubrique décrit les fonctionnalités de concepteur qui le rendent plus facile pour les développeurs créer des dispositions conformes Material Design. Cette section présente et explique comment utiliser la grille de matériau, la Palette de couleurs de matériau, l’échelle typographiques et l’éditeur de thème._

> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Evolve 2016 : Tout le monde peut créer de superbes applications conception matérielle**

## <a name="overview"></a>Vue d'ensemble

Le Concepteur de Xamarin.Android inclut des fonctionnalités qui le rendent plus facile de créer des dispositions de matériau conception compatible. Si vous n’êtes pas familiarisé avec la conception de matériau, consultez la [présentation de la conception de matériau](https://material.io/design/introduction).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Dans ce guide, nous aurons un coup de œil sur les fonctionnalités de concepteur suivantes :

-   *Grille Material* &ndash; une superposition sur l’aire de conception qui affiche une grille, les espaces et les objets vectoriels pour vous aider à placer les widgets de mise en forme conformément aux directives de conception de matériau.

-   *Éditeur de thème* &ndash; un éditeur de ressources de couleur small qui vous permet de définie les informations de couleur pour un sous-ensemble d’un thème. Par exemple, vous pouvez afficher un aperçu et modifier les couleurs de matière comme `colorPrimary`, `colorPrimaryDark`, et `colorAccent`.

Nous allons coup de œil à chacune de ces fonctionnalités et fournissent des exemples montrant comment les utiliser.

## <a name="material-design-grid"></a>Grille de conception matérielle

Le menu de la grille de création de documents est disponible à partir de la barre d’outils en haut du concepteur :

[![Grille de création de matériau](material-design-features-images/vs/01-material-design-grid-w158-sml.png)](material-design-features-images/vs/01-material-design-grid-w158.png#lightbox)

Lorsque vous cliquez sur l’icône de grille de conception de matériau, le concepteur affiche une superposition sur l’aire de conception qui inclut les éléments suivants :

-   Objets vectoriels (lignes oranges)

-   Espacement (zones vertes)

-   Une grille (lignes bleues)

Ces éléments peuvent être vus dans la capture d’écran précédente. Chacun de ces éléments de superposition est configurable. Lorsque vous cliquez sur les points de suspension en regard du menu de la grille de création de documents, un menu de la boîte de dialogue s’ouvre et vous permet d’activer/désactiver la grille, configurez le placement des objets vectoriels et définir espacements. Notez que toutes les valeurs sont exprimées dans `dp` (pixels indépendants de densité) :

[![Grille représente et configuration d’espacement](material-design-features-images/vs/03-grid-configuration-w158-sml.png)](material-design-features-images/vs/03-grid-configuration-w158.png#lightbox)

Pour ajouter un nouveau représente, entrez une nouvelle valeur de décalage dans le **décalage** , sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou  **bas**) et cliquez sur l’icône pour ajouter le nouveau représente +. De même, pour ajouter une nouvelle d’espacement, entrez la taille et le décalage (en dp) dans le **taille** et **décalage** boîtes, respectivement. Sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou **bas**) et cliquez sur l’icône pour ajouter le nouvel espacement +.

Lorsque vous modifiez ces valeurs de configuration, elles sont enregistrées dans le fichier XML de disposition et réutilisées, lorsque vous ouvrez la mise en page à nouveau.


## <a name="theme-editor"></a>Éditeur de thème

Le **éditeur de thème** vous permet de personnaliser les informations de couleur pour un sous-ensemble d’attributs de thème. Pour ouvrir le **éditeur de thème**, cliquez sur l’icône représentant un pinceau dans la barre d’outils :

[![Icône de l’éditeur de thème](material-design-features-images/vs/04-theme-editor-icon-w158-sml.png)](material-design-features-images/vs/04-theme-editor-icon-w158.png#lightbox)

Bien que le **éditeur de thème** est accessible à partir de la barre d’outils pour toutes les ciblent des versions Android et niveaux d’API, uniquement un sous-ensemble des fonctionnalités décrites ci-dessous sont disponibles si le niveau d’API cible est antérieur à l’API 21 (Android 5.0 Interface (lollipop)).

Le panneau de gauche de la **éditeur de thème** affiche la liste des couleurs qui composent le thème actuellement sélectionné (dans cet exemple, nous utilisons le `Default Theme`) :

[![Éditeur de thème](material-design-features-images/vs/05-theme-editor-w158-sml.png)](material-design-features-images/vs/05-theme-editor-w158.png#lightbox)

Lorsque vous sélectionnez une couleur sur la gauche, le volet de droite fournit les onglets suivants pour vous aider à modifier cette couleur :

-   **Hériter** &ndash; affiche un diagramme de l’héritage de style pour la couleur sélectionnée et répertorie les couleurs résolu et le code couleur assignée à cette couleur de thème.

-   **Sélecteur de couleurs** &ndash; vous permet de modifier la couleur sélectionnée à n’importe quelle valeur arbitraire.

-   **Palette de matériau** &ndash; vous permet de modifier la couleur sélectionnée à une valeur qui est conforme à la conception de matériau.

-   **Ressources** &ndash; vous permet de modifier la couleur sélectionnée à un des autres ressources existantes couleur dans le thème.

Nous allons étudier chacune de ces onglets en détail.

### <a name="inherit-tab"></a>Hériter d’onglet

Comme indiqué dans l’exemple suivant, le **hériter** onglet répertorie l’héritage de style pour le **arrière-plan** couleur de la **thème par défaut**:

[![Hériter d’onglet](material-design-features-images/vs/06-inherit-tab-w158-sml.png)](material-design-features-images/vs/06-inherit-tab-w158.png#lightbox)

Dans cet exemple, le **thème par défaut** hérite d’un style qui utilise `@color/background_material_light` mais se substitue à elle avec `color/material_grey_50`, qui a une valeur de code de couleur de `#fffafafa`.
Pour plus d’informations sur l’héritage de style, consultez [Styles et thèmes](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).

### <a name="color-picker"></a>Sélecteur de couleurs

La capture d’écran suivante illustre le **sélecteur de couleurs**:

[![Sélecteur de couleurs](material-design-features-images/vs/07-color-picker-w158-sml.png)](material-design-features-images/vs/07-color-picker-w158.png#lightbox)

Dans cet exemple, le **arrière-plan** couleur peut être modifiée à n’importe quelle valeur de plusieurs façons :

-   Cliquez sur une couleur directement.
-   Entrer des valeurs de teinte, saturation et de luminosité.
-   Entrer les valeurs RVB (rouge, verts et bleus) au format décimal.
-   Définition de la valeur alpha (opacité) pour la couleur sélectionnée.
-   Entrer le code de couleur hexadécimal directement.

La couleur que vous choisissez dans le sélecteur de couleurs est *pas* restreint aux règles de conception du matériel ou à l’ensemble des ressources de couleur disponibles.

### <a name="resources"></a>Ressources

Le **ressources** onglet propose une liste de ressources de couleur qui sont déjà présents dans le thème :

[![Ressources](material-design-features-images/vs/08-resources-w158-sml.png)](material-design-features-images/vs/08-resources-w158.png#lightbox)

À l’aide de la **ressources** onglet contraint de votre choix à cette liste de couleurs. N’oubliez pas que si vous choisissez une ressource de couleur qui est déjà affectée à une autre partie du thème, deux éléments adjacents de l’interface utilisateur peuvent « exécuter ensemble » (car ils ont la même couleur) et de devenir plus difficile pour l’utilisateur faire la distinction.

### <a name="material-palette"></a>Palette de matériau

Le **Palette de matériau** onglet s’ouvre le **Palette de couleurs de conception de matériau**. Choix d’une valeur de couleur à partir de cette palette de contraint votre choix de couleurs afin qu’il soit cohérent avec la conception de documents :

[![Palette de matériau](material-design-features-images/vs/09-material-palette-w158-sml.png)](material-design-features-images/vs/09-material-palette-w158.png#lightbox)

En haut de la palette de couleurs affiche les couleurs de Material Design primaires tandis que la partie inférieure de la palette affiche une plage de teintes pour la couleur principale sélectionnée. Par exemple, lorsque vous sélectionnez **Indigo**, une collection de **Indigo** teintes s’affiche en bas de la boîte de dialogue.
Lorsque vous sélectionnez une teinte, la couleur de la propriété est modifiée pour la teinte sélectionnée. Dans l’exemple suivant, le `Background Tint` du bouton devient *500 Indigo*:

![Sélectionnez 500 Indigo](material-design-features-images/vs/10-indigo-w158.png)

`Background Tint` le code de couleur a la valeur *500 Indigo* (`#ff3f51b5`), et le concepteur met à jour la couleur d’arrière-plan pour refléter cette modification :

[![Teinte arrière-plan modifié](material-design-features-images/vs/11-background-tint-w158-sml.png)](material-design-features-images/vs/11-background-tint-w158.png#lightbox)

Pour plus d’informations sur la palette de couleurs Material Design, consultez la conception de matériau [Guide de Palette de couleurs](https://material.io/design/color/).

### <a name="creating-a-new-theme"></a>Création d’un nouveau thème

Dans l’exemple suivant, nous allons utiliser la Palette de matériau pour créer un thème personnalisé. Tout d’abord, nous allons modifier le **arrière-plan** couleur *900 bleu*:

![Modifier l’arrière-plan de 900 bleu](material-design-features-images/vs/12-change-background-to-blue-w158.png)

Lorsqu’une ressource de couleur est modifiée, un message s’affiche avec le message, *le thème actuel comporte des modifications non enregistrées*:

[![Avertissement de modifications non enregistrées](material-design-features-images/vs/13-unsaved-changes-w158-sml.png)](material-design-features-images/vs/13-unsaved-changes-w158.png#lightbox)

Le **arrière-plan** couleur dans le concepteur a changé à la nouvelle sélection de couleur, mais cette modification n’a pas encore été enregistrée. À ce stade, vous pouvez effectuer l’une des opérations suivantes :

-   Cliquez sur **ignorer les modifications** l’abandon de la nouvelle couleur choisie (ou les choix) et de rétablir le thème à son état d’origine. 

-   Appuyez sur <kbd>CTRL + S</kbd> pour enregistrer vos modifications dans le thème actuellement. 

Dans l’exemple suivant, <kbd>CTRL + S</kbd> a été enfoncé afin que les modifications ont été enregistrées dans **AppTheme**:

[![Modifications enregistrées pour AppTheme](material-design-features-images/vs/14-custom-theme-w158-sml.png)](material-design-features-images/vs/14-custom-theme-w158.png#lightbox)

## <a name="summary"></a>Récapitulatif

Cette rubrique décrit les fonctionnalités de conception de documents disponibles dans le Concepteur de Xamarin.Android. Vous avez appris comment activer et configurer la grille de création de documents, et que vous avez appris comment utiliser l’éditeur de thème pour créer de nouveaux thèmes personnalisés qui sont conformes aux règles de conception de matériau.
Pour plus d’informations sur la prise en charge de Xamarin.Android pour la conception de matériau, consultez [matériau thème](~/android/user-interface/material-theme.md).




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Dans ce guide, nous allons un Examinez les fonctionnalités de concepteur suivantes :

-   *Grille de création de matériau* &ndash; une superposition sur l’aire de conception qui affiche une grille, les espaces et les objets vectoriels pour vous aider à placer les widgets de mise en forme conformément aux directives de conception de matériau.

-   *Palette de couleurs Material Design* &ndash; boîte de dialogue une propriété pavé qui vous aide à choisir une couleur de la palette de Material Design officielle.

-   *Mise à l’échelle typographique* &ndash; boîte de dialogue une propriété pavé qui vous offre un choix de paramètres de matériau conception conforme pour la `textAppearance` propriété des champs de texte.

-   *Éditeur de thème* &ndash; un éditeur de ressources de couleur small qui vous permet de définie les informations de couleur pour un sous-ensemble d’un thème. Par exemple, vous pouvez afficher un aperçu et modifier les couleurs de matière comme `colorPrimary`, `colorPrimaryDark`, et `colorAccent`.

Nous allons coup de œil à chacune de ces fonctionnalités et fournissent des exemples montrant comment les utiliser.

## <a name="material-design-grid"></a>Grille de conception matérielle

Le menu de la grille de création de documents est disponible à partir de la barre d’outils en haut du concepteur :

[![Grille de création de matériau](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

Lorsque vous cliquez sur l’icône de grille de conception de matériau, le concepteur affiche une superposition sur l’aire de conception qui inclut les éléments suivants :

-   Objets vectoriels (lignes oranges)

-   Espacement (zones vertes)

-   Une grille (lignes bleues)

Ces éléments peuvent être vus dans la capture d’écran suivante :

[![Représente l’espacement et la grille](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

Chacun de ces éléments de superposition est configurable. Lorsque vous cliquez sur le bouton de sélection (&hellip;) en regard du menu de la grille de création de documents, un menu de la boîte de dialogue s’ouvre et vous permet d’activer/désactiver la grille, configurez le placement des objets vectoriels et définir l’espacement. Notez que toutes les valeurs sont exprimées dans `dp` (pixels indépendants de densité) :

[![Grille représente et configuration d’espacement](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

Pour ajouter un nouveau représente, entrez une nouvelle valeur de décalage dans le **décalage** , sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou  **bas**) et cliquez sur l’icône (qui apparaît à droite lorsque la valeur est entrée) + pour ajouter le nouveau représente. De même, pour ajouter une nouvelle d’espacement, entrez la taille et le décalage (en dp) dans le **taille** et **décalage** boîtes, respectivement. Sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou **bas**) et cliquez sur l’icône pour ajouter le nouvel espacement +.

Lorsque vous modifiez ces valeurs de configuration, elles sont enregistrées dans le fichier XML de disposition et réutilisées, lorsque vous ouvrez la mise en page à nouveau.

## <a name="material-design-color-palette"></a>Palette de couleurs de conception matérielle

Chaque élément de panneau de configuration de propriété qui accepte une couleur maintenant a une icône de palette supplémentaire que vous pouvez utiliser pour ouvrir la Palette de couleurs de conception de matériau, comme indiqué dans cette capture d’écran :

[![Icône de couleur](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

Lorsque vous cliquez sur cette icône, un menu de la boîte de dialogue s’ouvre et rend possible pour vous permettent de configurer la couleur de cette propriété à partir de la palette de couleurs Material Design :

[![Palette de couleurs de conception matérielle](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

En haut de la palette de couleurs affiche les couleurs de Material Design primaires tandis que la partie inférieure de la palette affiche une plage de teintes pour la couleur principale sélectionnée. Par exemple, lorsque vous sélectionnez **Indigo**, une collection de **Indigo** teintes s’affiche en bas de la boîte de dialogue.
Lorsque vous sélectionnez une teinte, la couleur de la propriété est modifiée pour la teinte sélectionnée. Dans l’exemple suivant, le `Background Tint` du bouton devient *500 Indigo*:

[![Choisissez 500 Indigo](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` le code de couleur a la valeur *500 Indigo* (`#ff3f51b5`), et le concepteur met à jour la couleur d’arrière-plan du bouton afin de refléter cette modification :

[![Modifications de teinte en arrière-plan](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

Pour plus d’informations sur la palette de couleurs Material Design, consultez la conception de matériau [Guide de Palette de couleurs](https://material.io/design/color/).

## <a name="typographic-scale"></a>Mise à l’échelle typographique

Le **apparence du texte** section de la **propriété** pavé **Style** onglet possède une icône qui vous permet de select à partir d’un `TextAppearance` style qui est conforme à la conception de matériau spécification :

[![Onglet de style](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

Lorsque vous cliquez sur cette icône, elle s’ouvre le **mise à l’échelle typographiques** dialogue là, affectons qui présente une liste de styles de texte préconfigurées que vous pouvez choisir à partir de :

[![Sélecteur de style de texte](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

Dans l’exemple suivant, en cliquant sur **affichage 1** modifie le texte du bouton à la plus grande police de **affichage 1**:

[![Style d’affichage 1](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

Le style de texte dans le **mise à l’échelle typographiques** suit de la boîte de dialogue le **thème** paramètre. Par exemple, si le **Light** thème est choisi dans le concepteur, la liste des miroirs des styles de texte disponible le **Light** thème :

[![Thème clair](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

## <a name="theme-editor"></a>Éditeur de thème

Le **éditeur de thème** vous permet de personnaliser les informations de couleur pour un sous-ensemble d’attributs de thème. Pour ouvrir le **éditeur de thème**, cliquez sur l’icône représentant un pinceau dans la barre d’outils :

[![Icône de l’éditeur de thème](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

Bien que le **éditeur de thème** est accessible à partir de la barre d’outils pour toutes les ciblent des versions Android et niveaux d’API, uniquement un sous-ensemble des fonctionnalités décrites ci-dessous sont disponibles si le niveau d’API cible est antérieur à l’API 21 (Android 5.0 Interface (lollipop)).

Le panneau de gauche de la **éditeur de thème** affiche la liste des couleurs qui composent le thème actuellement sélectionné (dans cet exemple, nous utilisons le `Default Theme`) :

[![Éditeur de thème](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

Lorsque vous sélectionnez une couleur sur la gauche, le volet de droite fournit les onglets suivants pour vous aider à modifier cette couleur :

-   **Hériter** &ndash; affiche un diagramme de l’héritage de style pour la couleur sélectionnée et répertorie les couleurs résolu et le code couleur assignée à cette couleur de thème.

-   **Sélecteur de couleurs** &ndash; vous permet de modifier la couleur sélectionnée à n’importe quelle valeur arbitraire.

-   **Palette de matériau** &ndash; vous permet de modifier la couleur sélectionnée à une valeur qui est conforme à la conception de matériau.

-   **Ressources** &ndash; vous permet de modifier la couleur sélectionnée à un des autres ressources existantes couleur dans le thème.

Nous allons étudier chacune de ces onglets en détail.

### <a name="inherit-tab"></a>Hériter d’onglet

Comme indiqué dans l’exemple suivant, le **hériter** onglet répertorie l’héritage de style pour le **arrière-plan** couleur de la **thème par défaut**:

[![Hériter d’onglet](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

Dans cet exemple, le **thème par défaut** hérite d’un style qui utilise `@color/background_material_dark` mais se substitue à elle avec `color/material_grey_850`, qui a une valeur de code de couleur de `#ff303030`.
Pour plus d’informations sur l’héritage de style, consultez [Styles et thèmes](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).

### <a name="color-picker"></a>Sélecteur de couleurs

La capture d’écran suivante illustre le **sélecteur de couleurs**:

[![Sélecteur de couleurs](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)


Dans cet exemple, le **arrière-plan** couleur peut être modifiée à n’importe quelle valeur de plusieurs façons :

-   Cliquez sur une couleur directement.
-   Entrer des valeurs de teinte, saturation et de luminosité.
-   Entrer les valeurs RVB (rouge, verts et bleus) au format décimal.
-   Définition de la valeur alpha (opacité) pour la couleur sélectionnée.
-   Entrer le code de couleur hexadécimal directement.

La couleur que vous choisissez dans le sélecteur de couleurs est *pas* restreint aux règles de conception du matériel ou à l’ensemble des ressources de couleur disponibles.

### <a name="resources"></a>Ressources

Le **ressources** onglet propose une liste de ressources de couleur qui sont déjà présents dans le thème :

[![Ressources](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

À l’aide de la **ressources** onglet contraint de votre choix à cette liste de couleurs. N’oubliez pas que si vous choisissez une ressource de couleur qui est déjà affectée à une autre partie du thème, deux éléments adjacents de l’interface utilisateur peuvent « exécuter ensemble » (car ils ont la même couleur) et de devenir plus difficile pour l’utilisateur faire la distinction.

### <a name="material-palette"></a>Palette de matériau

Le **Palette de matériau** onglet s’ouvre le **Palette de couleurs de conception de matériau** décrit [antérieures](#material_palette). Choix d’une valeur de couleur à partir de cette palette de contraint votre choix de couleurs afin qu’il soit cohérent avec la conception de matériau.

[![Palette de matériau](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

### <a name="creating-a-new-theme"></a>Création d’un nouveau thème

Dans l’exemple suivant, nous allons utiliser la Palette de matériau pour créer un thème personnalisé. Tout d’abord, nous allons modifier le **arrière-plan** couleur *900 bleu*:

[![Modifier l’arrière-plan de 900 bleu](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

Lorsqu’une ressource de couleur est modifiée, un message s’affiche avec le message, *le thème actuel comporte des modifications non enregistrées*:

[![Avertissement de modifications non enregistrées](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

La couleur dans le concepteur a été modifié, mais cette modification n’a pas encore été enregistrée. À ce stade, vous pouvez effectuer l’une des opérations suivantes :

-   Cliquez sur **ignorer les modifications** l’abandon de la nouvelle couleur choisie (ou les choix) et de rétablir le thème à son état d’origine. 

-   Appuyez sur  **&#8984; + S** pour enregistrer vos modifications dans un nouveau thème appelé **personnalisé**. 


## <a name="summary"></a>Récapitulatif

Cette rubrique décrit les fonctionnalités de conception de documents disponibles dans le Concepteur de Xamarin.Android. Vous avez appris comment activer et configurer la grille de création de documents, l’utilisation de la Palette de couleurs de conception de matériau pour modifier les propriétés de couleur et comment utiliser le sélecteur de mise à l’échelle typographiques pour configurer les propriétés de texte. Il a également montré comment utiliser l’éditeur de thème pour créer de nouveaux thèmes personnalisés qui sont conformes aux règles de conception de matériau. Pour plus d’informations sur la prise en charge de Xamarin.Android pour la conception de matériau, consultez [matériau thème](~/android/user-interface/material-theme.md).

-----


## <a name="related-links"></a>Liens associés

- [Thème matériau](~/android/user-interface/material-theme.md)
- [Présentation de la conception matérielle](https://material.io/design/introduction)
