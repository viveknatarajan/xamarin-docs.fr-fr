---
title: Principes de base de concepteur
description: Cette rubrique présente les fonctionnalités du concepteur, explique comment lancer le concepteur, décrit l’aire de conception et explique comment utiliser le volet Propriétés pour modifier les propriétés de widget.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a8201301fc0437ecb79a81f40e865f14dc6af020
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="designer-basics"></a>Principes de base de concepteur

_Cette rubrique présente les fonctionnalités du concepteur, explique comment lancer le concepteur, décrit l’aire de conception et explique comment utiliser le volet Propriétés pour modifier les propriétés de widget._


## <a name="launching-the-designer"></a>Lancer le Concepteur

Le concepteur est lancé automatiquement lorsqu’une disposition est créée, ou il peut être lancé en double-cliquant sur un fichier .axml existant. Par exemple, en double-cliquant sur **Main.axml** dans les **ressources > disposition** dossier chargera le concepteur, comme indiqué ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Écran de définition dans Visual Studio](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Écran de définition dans Visual Studio pour Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

De même, vous pouvez ajouter une nouvelle disposition en cliquant sur le **disposition** dossier dans le **l’Explorateur de solutions** et en sélectionnant **Ajouter > nouvel élément... > disposition Android**:

[![Ajouter un nouvel élément de boîte de dialogue](designer-basics-images/vs/02-add-new-layout-sml.w157.png)](designer-basics-images/vs/02-add-new-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

De même, vous pouvez ajouter une nouvelle disposition en cliquant sur le **disposition** dossier dans le **Solution remplissage** et en sélectionnant **Ajouter > nouveau fichier > Android > disposition**:

[![Boîte de dialogue Nouveau fichier ajouter](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

-----

Cela crée un fichier .axml et les charge vers l’aire de conception.



## <a name="designer-features"></a>Fonctionnalités du Concepteur

Le concepteur est composé de plusieurs sections qui prennent en charge ses diverses fonctionnalités, comme illustré dans la capture d’écran suivante :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagramme des volets du Concepteur](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Diagramme des volets du Concepteur](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

-----

Lorsque vous modifiez une mise en page dans le concepteur, vous utilisez les fonctionnalités suivantes pour créer et mettre en forme votre conception :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Aire de conception** &ndash; facilite la construction visuelle de l’interface utilisateur en ce qui vous donne une représentation modifiable de la façon dont la disposition s’affiche sur l’appareil.

-   **Barre d’outils** &ndash; affiche la liste des sélecteurs : **périphérique**, **Version**, **thème**, configuration de la mise en page et les paramètres de la barre d’Action. La barre d’outils inclut également des icônes pour le lancement de l’éditeur de thème et de l’activation de la grille de création de documents.

-   **Boîte à outils** &ndash; fournit une liste de configurations que vous pouvez faire glisser et déposer sur l’aire de conception et de widgets.

-   **Fenêtre Propriétés** &ndash; répertorie les propriétés du widget sélectionné pour afficher et modifier.

-   **Structure du document** &ndash; affiche l’arborescence de widgets qui composent la mise en page. Vous pouvez cliquer sur un élément dans l’arborescence pour qu’elle est activée dans le concepteur. En outre, en cliquant sur un élément dans l’arborescence de la charge les propriétés de l’élément dans la fenêtre Propriétés.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

-   **Aire de conception** &ndash; facilite la construction visuelle de l’interface utilisateur en ce qui vous donne une représentation modifiable de la façon dont la disposition s’affiche sur l’appareil.

-   **Barre d’outils** &ndash; affiche la liste des sélecteurs : **périphérique**, **Version**, **thème**, configuration de la mise en page et les paramètres de la barre d’Action. La barre d’outils inclut également des icônes pour le lancement de l’éditeur de thème et de l’activation de la grille de création de documents.

-   **Boîte à outils** &ndash; fournit une liste de configurations que vous pouvez faire glisser et déposer sur l’aire de conception et de widgets.

-   **Remplissage de la propriété** &ndash; répertorie les propriétés du widget sélectionné pour afficher et modifier.

-   **Structure du document** &ndash; affiche l’arborescence de widgets qui composent la mise en page. Vous pouvez cliquer sur un élément dans l’arborescence pour qu’elle est activée dans le concepteur. En outre, en cliquant sur un élément dans l’arborescence de la charge des propriétés de l’élément dans le remplissage de la propriété.

-----



## <a name="toolbar"></a>ToolBar

La barre d’outils (au-dessus de l’aire de conception) présente des sélecteurs de configuration et les menus de l’outil :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagramme de la barre d’outils Concepteur](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Diagramme de la barre d’outils Concepteur](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

-----


La barre d’outils permet d’accéder aux fonctionnalités suivantes :

-   **Remplacement de sélecteur de disposition** &ndash; vous permet de sélectionner à partir de versions de mise en page différente.

-   **Sélecteur de périphérique** &ndash; définit un ensemble de qualificateurs associé à un périphérique particulier, telles que la taille de l’écran, la résolution et la disponibilité du clavier. Vous pouvez également ajouter et supprimer les nouveaux périphériques.

-   **Android Version sélecteur** &ndash; Android la version ciblée par la mise en page. Le concepteur affichera la disposition en fonction de la version Android sélectionnée.

-   **Sélecteur de thème** &ndash; sélectionne le thème de l’interface utilisateur pour la mise en page.

-   **Sélecteur de configuration** &ndash; sélectionne la configuration de l’appareil, tel que *portrait* ou *paysage*.

-   **Options du qualificateur de ressource** &ndash; ouvre une boîte de dialogue qui présente les menus déroulants pour sélectionner *langage*, *Mode d’interface utilisateur*, *Mode nuit*, et *Écran round* options.

-   **Paramètres de barre d’action** &ndash; configure les paramètres de la barre d’Action pour la mise en page.

-   **Éditeur de thème** &ndash; ouvre le *éditeur de thème*, ce qui rend possible pour vous permettre de personnaliser les éléments du thème sélectionné.

-   **Grille de création de matériel** &ndash; Active ou désactive le *grille de création de documents*. L’élément de menu de la liste déroulante adjacente à la grille de création de matériel s’ouvre une boîte de dialogue qui vous permet de personnaliser la grille.

Chacune de ces fonctionnalités est expliquée plus en détail dans les rubriques suivantes :

[Qualificateurs de ressources et les Options de visualisation](~/android/user-interface/android-designer/resource-qualifiers.md) fournit des informations détaillées sur la **appareil sélecteur**, **Android Version sélecteur**, **sélecteur de thème**, **Configuration sélecteur**, **ressource Qualifications Options**, et **paramètres de la barre d’Action**.

[Autres modes d’affichage](~/android/user-interface/android-designer/alternative-layout-views.md) explique comment utiliser le **sélecteur de disposition Alternative**.

[Les fonctionnalités de conception matérielles](~/android/user-interface/android-designer/material-design-features.md) fournit une vue d’ensemble complète de la **éditeur de thème** et le **grille de création de documents**.



## <a name="design-surface"></a>Aire de conception

Le concepteur vous permet de vous faire glisser et déposer des widgets à partir de la boîte à outils vers l’aire de conception. Lorsque vous interagissez avec les widgets dans le concepteur (par l’ajout de nouveaux widgets ou repositionnement existants), les lignes horizontales et verticales sont affichés pour marquer les points d’insertion disponibles. Dans l’exemple suivant, un nouveau `Button` widget est déplacé vers l’aire de conception :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Exemples de lignes d’insertion sur l’aire de conception](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Exemples de lignes d’insertion sur l’aire de conception](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

-----

En outre, les widgets peuvent être copiés : vous pouvez utiliser les copier et coller pour copier un widget, ou vous pouvez glisser- déposer un widget existant tout en maintenant la <kbd>Ctrl</kbd> clé.


### <a name="context-menu-commands"></a>Commandes de Menu contextuel

Un menu contextuel est disponible dans l’aire de conception et dans la structure du Document. Ce menu affiche les commandes qui sont disponibles pour le widget sélectionné et son conteneur, facilitant ainsi la vous permet d’effectuer des opérations sur les conteneurs (qui ne sont pas toujours faciles de sélectionner sur l’aire de conception). Voici un exemple d’un menu contextuel :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menu de contexte exemple droit de l’aire de conception](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

Dans cet exemple, droit une `TextView` ouvre un menu contextuel qui fournit plusieurs options :

-   **LinearLayout** &ndash; ouvre un sous-menu pour la modification de la `LinearLayout` parent de la `TextView`.

-   **Supprimer**, **copie**, et **couper** &ndash; les opérations qui s’appliquent à la méthode `TextView`.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Menu de contexte exemple droit de l’aire de conception](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

Dans cet exemple, droit une `TextView` ouvre un menu contextuel qui fournit plusieurs options :

-   **RelativeLayout** &ndash; ouvre un sous-menu pour la modification de la `RelativeLayout` parent de la `TextView`.

-   **LinearLayout** &ndash; ouvre un sous-menu pour la modification de la `LinearLayout` parent de la `TextView`.

-   **Supprimer**, **copie**, et **couper** &ndash; les opérations qui s’appliquent à la méthode `TextView`.

-----

Dans cet exemple, droit une `TextView` ouvre un menu contextuel qui fournit plusieurs options :

-   **LinearLayout** &ndash; ouvre un sous-menu pour la modification de la `LinearLayout` parent de la `TextView`.

-   **Supprimer**, **copie**, et **couper** &ndash; les opérations qui s’appliquent à la méthode `TextView`.



### <a name="zoom-controls"></a>Contrôles de zoom

L’aire de conception prend en charge le zoom via plusieurs contrôles, comme indiqué :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Diagramme de l’aire de conception les contrôles de zoom](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Diagramme de l’aire de conception les contrôles de zoom](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

-----

Ces contrôles rendent plus faciles à voir certaines zones de l’interface utilisateur dans le concepteur :

-   **Mettez en surbrillance les conteneurs** &ndash; met en évidence les conteneurs sur l’aire de conception afin qu’ils soient plus faciles de localiser lors d’un zoom avant et arrière.

-   **Taille normale** &ndash; restitue la disposition de pixels afin que vous pouvez voir l’aspect de la mise en page à la résolution du périphérique sélectionné.

-   **Ajuster à la fenêtre** &ndash; définit le niveau de zoom afin que toute la disposition est visible sur l’aire de conception.

-   **Effectuer un zoom avant dans** &ndash; effectue un zoom incrémentielle à chaque clic, agrandissement de la mise en page.

-   **Zoom arrière** &ndash; effectue de façon incrémentielle à chaque clic, qui effectue la mise en page apparaissent plus petits sur l’aire de conception.

Notez que le paramètre du zoom choisi n’affecte pas l’interface utilisateur de l’application lors de l’exécution.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="property-pad"></a>Remplissage de la propriété

Le concepteur prend en charge la modification des propriétés de widget via la **remplissage de la propriété**. Les propriétés répertoriées dans la modification de remplissage de la propriété selon laquelle le widget est sélectionné dans l’aire du concepteur. Lorsque le `Button` dans l’exemple précédent est sélectionné, les propriétés qui `Button` widget sont affichés :

[![Capture d’écran de la zone de propriété](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>Propriétés (fenêtre)

Le concepteur prend en charge la modification des propriétés de widget via la **fenêtre Propriétés**. Les propriétés répertoriées dans le changement de fenêtre de propriétés selon le widget est sélectionné sur l’aire de conception.
Lorsque le `Button` dans l’exemple précédent est sélectionné, les propriétés qui `Button` widget sont affichés :

![Capture d’écran de la fenêtre Propriétés](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="property-pad-sections"></a>Sections de remplissage de propriété

Le remplissage de la propriété est divisé en plusieurs sections qui regroupent des propriétés similaires &ndash; cela le rend plus facile de localiser les propriétés qui vous intéressent :

-   **Widget** &ndash; propriétés principal du widget, tel que `id`, `visibility`, `text`, etc. Propriétés de gestion de contenu son sont généralement placées ici.

-   **Style** &ndash; propriétés qui modifient l’apparence visuelle du widget, tel que `font`, `text color`, `background`, etc.

-   **Disposition** &ndash; propriétés de définir l’emplacement et la taille du widget.

-   **Défilement** &ndash; propriétés de défilement.

-   **Comportement** &ndash; indicateurs que définir le comportement du widget.

-----



### <a name="default-values"></a>Valeurs par défaut

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Les propriétés de la plupart des widgets seront vides dans le **propriétés** fenêtre, car leurs valeurs héritent du thème Android sélectionné.
Le **propriétés** fenêtre affichera uniquement les valeurs qui sont définies explicitement pour le widget sélectionné ; il n’affichera pas les valeurs qui sont héritées du thème.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Les propriétés de la plupart des widgets seront vides dans le **remplissage de la propriété** , car leurs valeurs héritent du thème Android sélectionné. Le **remplissage de la propriété** affichera uniquement les valeurs qui sont explicitement définies pour le widget sélectionné ; il n’affichera pas les valeurs qui sont héritées du thème.

-----


### <a name="referencing-resources"></a>Référence à des ressources

Certaines propriétés peuvent référencer des ressources qui sont définies dans les fichiers de la mise en page **.axml** fichier. Les scénarios les plus courants de ce type sont `string` et `drawable` ressources. Toutefois, les références peuvent également servir pour les autres ressources, telles que `Boolean` valeurs et les dimensions.
Lorsqu’une propriété prend en charge les références de ressources, une icône de navigation (un bouton de sélection, &hellip;) est indiqué en regard de l’entrée de texte pour la propriété.
Ce bouton ouvre un sélecteur de ressources quand vous cliquez dessus.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Par exemple, la capture d’écran suivante montre les ressources disponibles lorsque vous cliquez sur les points de suspension à droite du champ de texte pour un `Button` widget dans le **propriétés** fenêtre :

[![Capture d’écran de ressources exemple avec deux ressources répertoriées](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Par exemple, la capture d’écran suivante montre les ressources disponibles lorsque vous cliquez sur les points de suspension à droite du champ de texte pour un `Button` widget dans le **remplissage de la propriété**:

[![Capture d’écran de ressources exemple avec deux ressources répertoriées](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

-----

L’exemple suivant illustre le sélecteur de ressource pour le `Src` propriété d’un `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sélecteur de ressources qui répertorie la ressource icône pour un ImageView](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sélecteur de ressources qui répertorie la ressource icône pour un ImageView](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

-----



### <a name="boolean-property-references"></a>Références de propriété booléenne

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*Boolean* propriétés sont normalement choisies à partir d’un menu déroulant dans la fenêtre Propriétés. Vous pouvez sélectionner un `true` ou `false` valeur, ou vous pouvez sélectionner une référence de propriété en cliquant sur **sélectionner une ressource en cours...** . Vous pouvez également directement entrer une valeur comme `true` ou `false`.

![Exemple de définition de propriétés booléennes](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

*Boolean* propriétés figurent normalement en tant qu’une case à cocher dans la zone de propriété. Lorsqu’un `Boolean` propriété prend en charge les références de ressources, une petite case à cocher s’affiche en regard de la propriété. Une case à cocher activé signifie `true` et une zone vide signifie `false`. Vous pouvez également directement entrer une valeur comme `true` ou `false`. Vous pointez votre souris sur l’entrée affiche une icône de champ de texte de petite taille. Si vous souhaitez entrer la valeur manuellement, vous pouvez cliquer dessus.

[![Exemple de définition de propriétés booléennes](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>Propriétés groupées

Certains widgets ont des propriétés à valeurs multiples sont regroupées ensemble (tel que `Padding`, par exemple). Ces valeurs de propriété sont répertoriées dans le **remplissage de la propriété** dans une ligne unique et peut être développée. Certaines de ces propriétés peuvent être modifiées directement dans la ligne groupée, telles que le `Padding` propriété indiquée ci-dessous :

[![Exemples de paramètres pour la propriété de remplissage](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

-----


## <a name="editing-properties-inline"></a>Modification des propriétés Inline

Le concepteur Android prend en charge la modification directe de certaines propriétés sur l’aire de conception (de sorte que vous n’êtes pas obligé de ces propriétés dans la liste de propriétés de recherche). Les propriétés qui peuvent être modifiées directement incluent le texte, marge et la taille.

### <a name="text"></a>Texte

Les propriétés du texte de certains widgets (tel que `Button` et `TextView`), peut être modifiée directement sur l’aire de conception. Double-clic sur un widget placera en mode édition, comme indiqué ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ressource de texte pour la chaîne hello](designer-basics-images/vs/12-text-resource.png "ressource de texte")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Ressource de texte pour la chaîne hello](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

-----

Vous pouvez entrer une nouvelle valeur de texte, ou vous pouvez entrer une nouvelle chaîne de ressource. Dans l’exemple suivant, la `@string/hello` ressource est remplacée par le texte, `CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![MAJ + ENTRÉE pour le texte se lient automatiquement à une nouvelle ressource](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![MAJ + ENTRÉE pour le texte se lient automatiquement à une nouvelle ressource](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

-----

Cette modification est stockée dans son `text` propriété ; il ne modifie pas la valeur affectée à la `@string/hello` ressource.
Lorsque vous la clé dans une nouvelle chaîne de texte, vous pouvez appuyer sur <kbd>MAJ</kbd> +
<kbd>entrée</kbd> pour le texte entré se lient automatiquement à une nouvelle ressource.


### <a name="margin"></a>Marge

Lorsque vous sélectionnez un widget, le concepteur affiche des poignées qui vous permettent de modifier la taille ou la marge du widget en mode interactif. En cliquant sur le widget lorsqu’il est sélectionné bascule entre le mode d’édition de marge et le mode de modification de taille.

Lorsque vous cliquez sur un widget pour la première fois, les poignées de marge sont affichées. Si vous déplacez la souris vers une des poignées, le concepteur affiche la propriété change le descripteur (comme indiqué ci-dessous pour le `layout_marginLeft` propriété) :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Capture d’écran de marge gère dans le Concepteur](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Capture d’écran de marge gère dans le Concepteur](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

-----

Si une marge a déjà été définie, les traits en pointillés s’affichent, qui indique l’espace occupé par la marge par :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Exemple de lignes en pointillés marquage espace autour d’un bouton](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Exemple de lignes en pointillés marquage espace autour d’un bouton](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

-----



### <a name="size"></a>Size

Comme mentionné précédemment, vous pouvez basculer en mode d’édition-taille en cliquant sur un widget lorsqu’il est déjà sélectionné. Cliquez sur la poignée triangulaire pour définir la taille de la dimension indiquée à `wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Poignées de contenu de type Wrap et de redimensionnement](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Poignées de contenu de type Wrap et de redimensionnement](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

-----

En cliquant sur le **encapsuler le contenu** handle réduit le widget dans cette dimension de sorte qu’il ne dépasse pas nécessaire pour encapsuler le contenu entre parenthèses. Dans cet exemple, le texte du bouton se réduit horizontalement, comme indiqué dans la capture d’écran suivante.

Lorsque la valeur de taille a la valeur **encapsuler le contenu**, le concepteur affiche une poignée triangulaire pointant dans la direction opposée pour modifier la taille à `match_parent`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Handle de parent de correspondance](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Handle de parent de correspondance](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

-----

En cliquant sur le **correspondance Parent** handle restaure la taille de la dimension afin qu’il soit le même que le widget de parent.

En outre, vous pouvez faire glisser la poignée de redimensionnement circulaire (comme indiqué dans les captures d’écran ci-dessus) pour redimensionner le widget à arbitraire `dp` valeur. Lorsque vous procédez ainsi, les deux **encapsuler le contenu** et **correspondance Parent** poignées sont présentées dans cette dimension :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Poignées de redimensionnement circulaire](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Poignées de redimensionnement circulaire](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

-----

Pas tous les conteneurs permettent de modifier le `Size` d’un widget. Par exemple, notez que la capture d’écran ci-dessous avec la `LinearLayout` sélectionné, les poignées de redimensionnement n’apparaissent pas :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sans poignées de redimensionnement](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sans poignées de redimensionnement](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

-----



## <a name="document-outline"></a>Structure du document

Le **structure du Document** affiche la hiérarchie de widget de la disposition.
Dans l’exemple suivant, le contenant `LinearLayout` widget est sélectionnée :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Structure du document](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Structure du document](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

-----

Le plan du widget sélectionné (dans ce cas, un `LinearLayout`) est également mis en surbrillance dans l’aire de conception. Le widget sélectionné dans la structure du Document reste synchronisé avec son équivalent sur l’aire de conception. Cela est utile pour sélectionner des groupes de la vue, qui ne sont pas toujours faciles à sélectionner sur l’aire de conception.

La structure du Document prend en charge le copier- coller, ou vous pouvez utiliser la fonction glisser-déplacer. Glisser -déplacer est prise en charge à partir de la structure du Document à l’aire de conception et l’aire de conception pour la structure du Document. En outre, le clic droit sur un élément dans la structure du Document affiche le menu contextuel pour cet élément (le même menu contextuel qui s’affiche lorsque vous cliquez sur ce même widget sur l’aire de conception).
