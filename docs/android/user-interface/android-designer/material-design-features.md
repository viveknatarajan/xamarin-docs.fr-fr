---
title: Fonctionnalités de conception de matériel
description: Cette rubrique décrit les fonctionnalités du concepteur qui la rendent plus facile pour les développeurs à créer des dispositions matérielles conception conforme. Cette section présente et explique comment utiliser la grille de matériel, la Palette de couleurs de matériel, l’échelle typographiques et l’éditeur de thème.
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a764efe7f2cadd8c777f8427c0220e45eec662c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773629"
---
# <a name="material-design-features"></a>Fonctionnalités de conception de matériel

_Cette rubrique décrit les fonctionnalités du concepteur qui la rendent plus facile pour les développeurs à créer des dispositions matérielles conception conforme. Cette section présente et explique comment utiliser la grille de matériel, la Palette de couleurs de matériel, l’échelle typographiques et l’éditeur de thème._


> [!Video https://youtube.com/embed/E3_ZjIOzVzY]

**Faire évoluer 2016 : Tout le monde peut créer des applications et attrayantes avec la conception de matériel**

## <a name="overview"></a>Vue d'ensemble

Le Concepteur de Xamarin.Android inclut des fonctionnalités qui la rendent plus facile de créer des dispositions matériau conception conforme. Si vous n’êtes pas familiarisé avec les documents de conception, consultez le [documents de conception Introduction](https://www.google.com/design/spec/material-design/introduction.html).

Dans ce guide, nous aurons examiner les fonctionnalités de concepteur suivantes :

-   *Grille matériau* &ndash; un segment de recouvrement sur l’aire de conception qui affiche une grille, l’espacement et objets vectoriels pour vous aider à placer les widgets de disposition en fonction des règles de conception de matériel.

-   *Palette de couleurs matériau* &ndash; boîte de dialogue remplissage de propriété A qui vous aide à choisir une couleur de la palette de conception de la documentation officielle.

-   *Échelle typographique* &ndash; boîte de dialogue remplissage de propriété A qui vous offre un choix de paramètres matériel conception conforme pour le `textAppearance` propriété des champs de texte.

-   *Éditeur de thème* &ndash; un éditeur de ressources de couleur small qui vous permet de définie les informations de couleur pour un sous-ensemble d’un thème. Par exemple, vous pouvez afficher un aperçu et modifier les couleurs de matériel tel que `colorPrimary`, `colorPrimaryDark`, et `colorAccent`.

Nous coup de œil à chacune de ces fonctionnalités et fournissent des exemples de leur utilisation.



## <a name="material-design-grid"></a>Grille de création de matériel

Le menu de la grille de création de documents est disponible à partir de la barre d’outils en haut du concepteur :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Grille de création de matériel](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Grille de création de matériel](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png#lightbox)

-----

Lorsque vous cliquez sur l’icône de la grille de création de documents, le concepteur affiche un segment de recouvrement sur l’aire de conception qui inclut les éléments suivants :

-   Objets vectoriels (lignes oranges)

-   Espacement (zones verts)

-   Une grille (lignes bleues)

Ces éléments peuvent être consultés dans la capture d’écran suivant :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Représente l’espacement et la grille](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Représente l’espacement et la grille](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chacun de ces éléments de la superposition est configurable. Lorsque vous cliquez sur le bouton de sélection en regard du menu de la grille de création de documents, un popover de la boîte de dialogue qui vous permet d’activer/désactiver la grille, configurez le placement des objets vectoriels et définir l’espacement s’ouvre. Notez que toutes les valeurs sont exprimées en `dp` (indépendant de la densité de pixels) :

![Grille représente et la configuration de l’espacement](material-design-features-images/vs/03-grid-configuration.png)

Pour ajouter un nouveau représente, entrez une nouvelle valeur de décalage dans le **Offset** , sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou  **bas**) et cliquez sur l’icône + pour ajouter la nouvelle représente.

De même, pour ajouter un nouvel espacement, entrez la taille et le décalage (dans le point de distribution) dans le **taille** et **Offset** zones, respectivement. Sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou **bas**) et cliquez sur l’icône + pour ajouter le nouvel espacement.

Lorsque vous modifiez ces valeurs de configuration, elles sont enregistrées dans le fichier XML de mise en page et réutilisés lorsque vous ouvrez de nouveau la mise en page.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Chacun de ces éléments de la superposition est configurable. Lorsque vous cliquez sur le triangle vers le bas en regard du menu de la grille de création de documents, un popover de la boîte de dialogue qui vous permet d’activer/désactiver la grille, configurez le placement des objets vectoriels et définir l’espacement s’ouvre. Notez que toutes les valeurs sont exprimées en `dp` (indépendant de la densité de pixels) :

[![Grille représente et la configuration de l’espacement](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png#lightbox)

Pour ajouter un nouveau représente, entrez une nouvelle valeur de décalage dans le **Offset** , sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou  **bas**) et cliquez sur l’icône + pour ajouter la nouvelle représente.

De même, pour ajouter un nouvel espacement, entrez la taille et le décalage (dans le point de distribution) dans le **taille** et **Offset** zones, respectivement. Sélectionnez un emplacement (**gauche**, **haut**, **droit**, ou **bas**) et cliquez sur l’icône + pour ajouter le nouvel espacement.

Lorsque vous modifiez ces valeurs de configuration, elles sont enregistrées dans le fichier XML de mise en page et réutilisés lorsque vous ouvrez de nouveau la mise en page.


## <a name="material-design-color-palette"></a>Palette de couleurs de matière de conception

Chaque élément de panneau de configuration de propriété qui accepte une couleur maintenant a une icône supplémentaire que vous pouvez utiliser pour ouvrir la Palette de couleurs de conception de matériel, comme indiqué dans cette capture d’écran :

[![Icône de couleur](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png#lightbox)

Lorsque vous cliquez sur cette icône, une popover de la boîte de dialogue qui permet de définir la couleur de cette propriété à partir de la palette de couleurs de conception de matériel s’ouvre :

[![Palette de couleurs de conception matériel](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png#lightbox)

La partie supérieure de la palette de couleurs affiche les couleurs de conception de matériel principales tandis que la partie inférieure de la palette affiche une plage de teintes pour la couleur principale sélectionnée. Par exemple, lorsque vous sélectionnez **Indigo**, une collection de **Indigo** teintes s’affiche au bas de la boîte de dialogue.
Lorsque vous sélectionnez une teinte, la couleur de la propriété est modifiée pour la teinte sélectionnée. Dans l’exemple suivant, la `Background Tint` du bouton est remplacée par *500 Indigo*:

[![Choisissez Indigo 500](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png#lightbox)

`Background Tint` le code de couleur a la valeur *500 Indigo* (`#ff3f51b5`), et le concepteur met à jour la couleur d’arrière-plan du bouton afin de refléter cette modification :

[![Modifications de teinte en arrière-plan](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png#lightbox)

Pour plus d’informations sur la palette de couleurs de conception de matériel, consultez la conception de matériel [Guide Palette des couleurs](http://www.google.com/design/spec/style/color.html#color-color-palette).

## <a name="typographic-scale"></a>Échelle typographique

Le **apparence de texte** section de la **propriété** remplissage **Style** onglet possède une icône qui vous permet de sélectionner à partir d’un `TextAppearance` style qui est conforme à la conception de matériel spécification :

[![Onglet de style](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png#lightbox)

Lorsque vous cliquez sur cette icône, il ouvre le **échelle typographiques** popover boîte de dialogue, qui présente la liste des styles de texte préconfiguré que vous pouvez choisir de :

[![Sélecteur de style de texte](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png#lightbox)

Dans l’exemple suivant, en cliquant sur **affichage 1** modifie le texte du bouton de la plus grande police de **affichage 1**:

[![Style d’affichage 1](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png#lightbox)

Le style de texte dans le **échelle typographiques** suit de la boîte de dialogue le **thème** paramètre. Par exemple, si le **Light** thème est choisi dans le concepteur, la liste des miroirs des styles de texte disponible le **Light** thème :

[![Thème clair](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png#lightbox)

-----



## <a name="theme-editor"></a>Éditeur de thème

Le **éditeur de thème** vous permet de personnaliser les informations de couleur pour un sous-ensemble d’attributs de thème. Pour ouvrir la **éditeur de thème**, cliquez sur l’icône représentant un pinceau de la barre d’outils :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Icône de l’éditeur de thème](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Icône de l’éditeur de thème](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png#lightbox)

-----

Bien que le **éditeur de thème** est accessible à partir de la barre d’outils pour toutes les ciblent Android versions et les niveaux d’API, seul un sous-ensemble des fonctionnalités décrites ci-dessous sont disponibles si le niveau de l’API cible est antérieur à 21 d’API (Android 5.0 Interface lollipop).

Le panneau gauche de la **éditeur de thème** affiche la liste des couleurs qui composent le thème actuellement sélectionné (dans cet exemple, nous utilisons le `Default Theme`) :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Éditeur de thème](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Éditeur de thème](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png#lightbox)

-----

Lorsque vous sélectionnez une couleur sur la gauche, le volet de droit fournit les onglets suivants pour vous aider à modifier cette couleur :

-   **Hériter** &ndash; affiche un diagramme de l’héritage de style pour la couleur sélectionnée et répertorie les couleurs résolu et le code de couleur assignée à cette couleur de thème.

-   **Sélecteur de couleurs** &ndash; vous permet de modifier le texte sélectionné de couleur à une valeur arbitraire.

-   **Palette de matériel** &ndash; vous permet de modifier le texte sélectionné de couleur à une valeur qui est conforme aux documents de conception.

-   **Ressources** &ndash; vous permet de modifier de couleurs sélectionné à un des autres existants des ressources dans le thème de couleur.

Examinons chacune de ces onglets en détail.



### <a name="inherit-tab"></a>Hériter d’onglet

Comme indiqué dans l’exemple suivant, la **hériter** onglet répertorie l’héritage de style pour le **arrière-plan** couleur de la **thème par défaut**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Hériter d’onglet](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Hériter d’onglet](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png#lightbox)

-----

Dans cet exemple, le **thème par défaut** hérite d’un style qui utilise `@color/background_material_dark` , mais se substitue à elle avec `color/material_grey_850`, qui a une valeur de code de couleur `#ff303030`.
Pour plus d’informations sur l’héritage de style, consultez [Styles et thèmes](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).



### <a name="color-picker"></a>Sélecteur de couleurs

La capture d’écran suivante illustre le **sélecteur de couleurs**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sélecteur de couleurs](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sélecteur de couleurs](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png#lightbox)

-----

Dans cet exemple, le **arrière-plan** couleur peut être remplacée par n’importe quelle valeur par différents moyens :

-   Cliquez sur une couleur directement.
-   Saisie de valeurs de teinte, saturation et de luminosité.
-   Entrez les valeurs RVB (rouges, verts, bleus) au format décimal.
-   Définition de la valeur alpha (opacité) pour la couleur sélectionnée.
-   Entrer le code de couleur hexadécimale directement.

La couleur que vous choisissez dans le sélecteur de couleurs est *pas* restreint les règles de conception de matériel ou à l’ensemble de ressources de couleur disponibles.


### <a name="resources"></a>Ressources

Le **ressources** onglet propose une liste de ressources sont déjà présents dans le thème de couleur :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Resources](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Resources](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png#lightbox)

-----

À l’aide de la **ressources** onglet contraint de votre choix à la liste de couleurs. N’oubliez pas que si vous choisissez une ressource de couleur qui est déjà affectée à une autre partie du thème, deux éléments adjacents de l’interface utilisateur peuvent « exécuter ensemble » (car ils ont la même couleur) et s’avérer difficile pour l’utilisateur de faire la distinction.



### <a name="material-palette"></a>Palette de matériel

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le **matériel Palette** onglet s’ouvre le **Palette de couleurs de documents de conception**. Choix d’une valeur de couleur de cette palette de limite votre choix de couleurs afin qu’il soit cohérent avec la conception de matériel.

[![Palette de matériel](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png#lightbox)

La partie supérieure de la palette de couleurs affiche les couleurs de conception de matériel principales tandis que la partie inférieure de la palette affiche une plage de teintes pour la couleur principale sélectionnée. Par exemple, lorsque vous sélectionnez **Indigo**, une collection de **Indigo** teintes s’affiche au bas de la boîte de dialogue.
Lorsque vous sélectionnez une teinte, la couleur de la propriété est modifiée pour la teinte sélectionnée. Dans l’exemple suivant, la `Background Tint` du bouton est remplacée par *500 Indigo*:

![Sélectionnez Indigo 500](material-design-features-images/vs/10-indigo.png)

`Background Tint` le code de couleur a la valeur *500 Indigo* (`#ff3f51b5`), et le concepteur met à jour la couleur d’arrière-plan pour refléter cette modification :

[![Teinte en arrière-plan modifié](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png#lightbox)

Pour plus d’informations sur la palette de couleurs de conception de matériel, consultez la conception de matériel [Guide Palette des couleurs](http://www.google.com/design/spec/style/color.html#color-color-palette).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le **matériel Palette** onglet s’ouvre le **Palette de couleurs de documents de conception** décrit [antérieures](#material_palette). Choix d’une valeur de couleur de cette palette de limite votre choix de couleurs afin qu’il soit cohérent avec la conception de matériel.

[![Palette de matériel](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png#lightbox)

-----



### <a name="creating-a-new-theme"></a>Création d’un nouveau thème.

Dans l’exemple suivant, nous allons utiliser la Palette de matériel pour créer un thème personnalisé. Tout d’abord, nous allons modifier le **arrière-plan** couleur *900 bleu*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Modifier l’arrière-plan bleu 900](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Modifier l’arrière-plan bleu 900](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png#lightbox)

-----


Lorsqu’une ressource de couleur est modifiée, un message s’affiche avec le message, *le thème actuel a des modifications non enregistrées*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Avertissement de modifications non enregistrées](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Avertissement de modifications non enregistrées](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png#lightbox)

-----


Le **arrière-plan** couleur dans le concepteur est passée à la nouvelle sélection de couleur, mais cette modification n’a pas encore été enregistrée. Deux solutions sont proposées pour savoir comment gérer les modifications :

-   **Ignorer les modifications** &ndash; ignore la nouvelle couleur choisie (ou le choix), puis rétablit le thème à son état d’origine.

-   **Créer le nouveau thème** &ndash; crée un nouveau thème qui est dérivé le thème sélectionné et utilise les substitutions de couleur dans le **éditeur de thème**.

Lorsque vous cliquez sur **créer un nouveau thème**, une des valeurs suivantes peut se produire, selon le thème sélectionné :

-   Si le thème actuellement sélectionné dans le concepteur n’est pas un thème de projet, vous sont présentées avec l’option permettant de créer un fichier de ressources avec le thème personnalisé (en utilisant le thème sélectionné en tant que le parent du thème personnalisé). Le concepteur bascule sur le thème personnalisé après sa création.

-   Si le thème sélectionné est un thème de projet qui est défini dans un emplacement, vous sont présentées avec le **thème de la mise à jour** option ; cette option met à jour le thème sélectionné au lieu de créer un nouveau.

-   Si le thème sélectionné est un thème de projet qui est défini dans plusieurs emplacements (par exemple, dans différemment-suivi du suffixe **valeurs** dossiers comme **v21 de valeurs**), une boîte de dialogue qui vous demande s’offrent à vous pour un nouvel emplacement pour enregistrer le thème personnalisé.

Poursuivre l’exemple précédent, en cliquant sur **créer un nouveau thème** entraîne la création d’un nouveau thème appelé **personnalisée**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Thème personnalisé ajouté](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Thème personnalisé ajouté](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png#lightbox)

-----


Étant donné que le thème sélectionné n’est pas un thème de projet, il n’existe aucune boîte de dialogue pour mettre à jour le thème sélectionné ou pour spécifier un nouvel emplacement.


## <a name="summary"></a>Récapitulatif

Cette rubrique décrit les fonctionnalités de création de documents disponibles dans le Concepteur de Xamarin.Android. Il explique comment activer et configurer la grille de création de documents, l’utilisation de la Palette de couleurs de documents de conception pour modifier les propriétés de couleur et comment utiliser le sélecteur de montée en puissance typographiques pour configurer les propriétés de texte. Il a également montré comment utiliser l’éditeur de thème pour créer de nouveaux thèmes personnalisés qui sont conformes aux règles de conception de matériel. Pour plus d’informations sur la prise en charge de Xamarin.Android des documents de conception, consultez [matériel thème](~/android/user-interface/material-theme.md).



## <a name="related-links"></a>Liens associés

- [Thème matériau](~/android/user-interface/material-theme.md)
- [Présentation de la conception matérielle](https://www.google.com/design/spec/material-design/introduction.html)
