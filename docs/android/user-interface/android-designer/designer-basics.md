---
title: Principes fondamentaux du Concepteur de Xamarin.Android
description: Cette rubrique présente les fonctionnalités du Concepteur de Xamarin.Android, explique comment lancer le concepteur, décrit l’aire de conception et explique comment utiliser le volet Propriétés pour modifier les propriétés du widget.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/05/2018
ms.openlocfilehash: fe909d72f3c6d6733318b5dcbd1858a1a9e28b37
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61165055"
---
# <a name="xamarinandroid-designer-basics"></a>Principes fondamentaux du Concepteur de Xamarin.Android

_Cette rubrique présente les fonctionnalités du Concepteur de Xamarin.Android, explique comment lancer le concepteur, décrit l’aire de conception et explique comment utiliser le volet Propriétés pour modifier les propriétés du widget._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="launching-the-designer"></a>Lancer le Concepteur

Le concepteur est lancé automatiquement lorsqu’une disposition est créée, ou il peut être lancé en double-cliquant sur un fichier de disposition existant. Par exemple, en double-cliquant sur **activity_main.axml** dans le **ressources > disposition** dossier chargera le concepteur, comme indiqué dans cette capture d’écran :

[![Écran Concepteur dans Visual Studio](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

De même, vous pouvez ajouter une nouvelle disposition en double-cliquant sur le **disposition** dossier dans le **l’Explorateur de solutions** et en sélectionnant **Ajouter > nouvel élément... > disposition Android**:

[![Ajouter un nouvel élément de boîte de dialogue](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

Cette action crée une **.axml** fichier de disposition et les charge dans le concepteur.

## <a name="designer-features"></a>Fonctionnalités du Concepteur

Le concepteur est composé de plusieurs sections qui prennent en charge ses différentes fonctionnalités, comme illustré dans la capture d’écran suivante :

[![Diagramme des volets du Concepteur](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

Lorsque vous modifiez une mise en page dans le concepteur, vous utilisez les fonctionnalités suivantes pour créer et mettre en forme votre conception :

-   **Aire de conception** &ndash; facilite la construction visuelle de l’interface utilisateur en vous offrant une représentation modifiable de la façon dont la disposition s’affiche sur l’appareil. Le **aire de conception** s’affiche à l’intérieur de la **volet de conception** (avec le **barre d’outils Concepteur** placés au-dessus de lui).

-   **Volet de source** &ndash; fournit une vue de la source XML sous-jacente qui correspond à la conception présentée sur le **aire de conception**.

-   **Barre d’outils Concepteur** &ndash; affiche une liste de sélecteurs : **APPAREIL**, **Version**, **thème**, configuration de mise en page et les paramètres de la barre d’Action. Le **barre d’outils Concepteur** comprend également des icônes pour le lancement de l’éditeur de thème et pour l’activation de la grille de création de documents.

-   **Boîte à outils** &ndash; fournit une liste de widgets et dispositions que vous pouvez faire glisser et déposer sur le **aire de conception**.

-   **Fenêtre Propriétés** &ndash; répertorie les propriétés du widget sélectionné pour afficher et modifier.

-   **Structure du document** &ndash; affiche l’arborescence de widgets qui composent la disposition. Vous pouvez cliquer sur un élément dans l’arborescence pour qu’elle être sélectionnés dans le **aire de conception**. En outre, cliquer sur un élément dans l’arborescence de la charge des propriétés de l’élément dans le **propriétés** fenêtre.

## <a name="design-surface"></a>Aire de conception

Le concepteur vous permet de vous faire glisser et déposer des widgets à partir de la boîte à outils vers le **aire de conception**. Lorsque vous interagissez avec les widgets dans le concepteur (soit en ajoutant de nouveaux widgets soit repositionnement existants), les lignes horizontales et verticales sont affichés pour marquer les points d’insertion disponibles. Dans l’exemple suivant, un nouveau `Button` widget est glissé vers le **aire de conception**:

[![Exemples de lignes d’insertion sur l’aire de conception](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

En outre, les widgets peuvent être copiés : vous pouvez utiliser copier et coller pour copier un widget, ou vous pouvez glisser -déplacer un widget existant tout en maintenant la <kbd>CTRL</kbd> clé.

### <a name="designer-toolbar"></a>Barre d’outils Concepteur

Le **barre d’outils Concepteur** (positionnées au-dessus de la **aire de conception**) présente les menus d’outils et les sélecteurs de configuration :

[![Diagramme de la barre d’outils Concepteur](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

Le **barre d’outils Concepteur** fournit l’accès aux fonctionnalités suivantes :

-   **Autre sélecteur de disposition** &ndash; vous permet de sélectionner à partir de versions de mise en page différente.

-   **Sélecteur d’appareils** &ndash; définit un jeu de qualificateurs (par exemple, la taille de l’écran, la résolution et la disponibilité de clavier) associé à un périphérique particulier. Vous pouvez également ajouter et supprimer des nouveaux appareils.

-   **Sélecteur de Version Android** &ndash; The Android version que la cible de la mise en page. Le concepteur affichera la disposition en fonction de la version Android sélectionnée.

-   **Sélecteur de thème** &ndash; sélectionne le thème de l’interface utilisateur pour la mise en page.

-   **Sélecteur de configuration** &ndash; sélectionne la configuration de l’appareil, tel que *portrait* ou *paysage*.

-   **Options du qualificateur de ressource** &ndash; ouvre une boîte de dialogue qui présente les menus déroulants pour sélectionner *langage*, *Mode d’interface utilisateur*, *Mode nuit*, et *Écran arrondi* options.

-   **Paramètres de barre d’action** &ndash; configure les paramètres de la barre d’Action pour la mise en page.

-   **Éditeur de thème** &ndash; ouvre le *éditeur de thème*, ce qui rend possible pour vous permettent de personnaliser les éléments du thème sélectionné.

-   **Grille de création de matériau** &ndash; Active ou désactive le *grille de conception de matériau*. L’élément de menu de liste déroulante adjacente à la grille de conception de matériau ouvre une boîte de dialogue qui vous permet de personnaliser la grille.

Chacune de ces fonctionnalités est expliquée plus en détail dans les rubriques suivantes :

-   [Qualificateurs de ressources et les Options de visualisation](~/android/user-interface/android-designer/resource-qualifiers.md) fournit des informations détaillées sur la **sélecteur d’appareils**, **sélecteur de Version Android**, **sélecteur de thème**, **Configuration sélecteur**, **ressource Qualifications Options**, et **les paramètres de barre d’Action**.

-   [Des vues de disposition](~/android/user-interface/android-designer/alternative-layout-views.md) explique comment utiliser le **sélecteur de disposition Alternative**.

-   [Fonctionnalités de conception de matériau concepteur Xamarin.Android](~/android/user-interface/android-designer/material-design-features.md) fournit une vue d’ensemble complète de la **éditeur de thème** et le **grille de conception de matériau**.

### <a name="context-menu-commands"></a>Commandes de menu contextuel

Un menu contextuel est disponible à la fois dans le **aire de conception** et dans le **structure du Document**. Ce menu affiche les commandes qui sont disponibles pour le widget sélectionné et son conteneur, ce qui facilite pour effectuer des opérations sur les conteneurs (qui ne sont pas toujours facile à sélectionner sur le **aire de conception**). Voici un exemple d’un menu contextuel :

[![Menu de contexte exemple lors du clic droit sur l’aire de conception](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

Dans cet exemple, clic droit sur un `TextView` ouvre un menu contextuel qui fournit plusieurs options :

-   **LinearLayout** &ndash; ouvre un sous-menu pour la modification la `LinearLayout` parent de la `TextView`.

-   **Supprimer**, **copie**, et **couper** &ndash; opérations qui s’appliquent à l’utilisateur clique `TextView`.


### <a name="zoom-controls"></a>Contrôles de zoom

Le **aire de conception** prend en charge le zoom par le biais de plusieurs contrôles, comme indiqué :

[![Diagramme des contrôles de zoom d’aire de conception](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

Ces contrôles rendent plus facile à voir certaines zones de l’interface utilisateur dans le concepteur :

-   **Mettez en surbrillance les conteneurs** &ndash; met en évidence des conteneurs sur le **aire de conception** afin qu’ils soient plus faciles de localiser tout en zoom avant et arrière.

-   **Taille normale** &ndash; restitue la disposition de pixels afin que vous pouvez voir l’aspect de la mise en page à la résolution de l’appareil sélectionné.

-   **Ajuster à la fenêtre** &ndash; définit le niveau de zoom afin que la disposition entière est visible sur l’aire de conception.

-   **Effectuer un zoom avant dans** &ndash; effectue un zoom avant façon incrémentielle avec chaque clic, l’agrandissement de la mise en page.

-   **Zoom arrière** &ndash; effectue un zoom arrière incrémentielle avec chaque clic, rendre la disposition s’affichent plus petits sur l’aire de conception.

Notez que le paramètre du zoom choisi n’affecte pas l’interface utilisateur de l’application lors de l’exécution.

## <a name="switching-between-design-and-source-panes"></a>Basculer entre les volets de conception et de la Source

Dans la bande center entre le **conception** et **Source** volets, plusieurs boutons qui sont utilisés pour modifier la **conception** et **Source**volets sont affichent :

[![Emplacement du bouton de volet Affichage](designer-basics-images/vs/25-pane-buttons-sml.png)](designer-basics-images/vs/25-pane-buttons.png#lightbox)

Ces boutons procédez comme suit :

-   **Conception** &ndash; ce bouton plus haut, **conception**, sélectionne le **conception** volet. Lorsque l’utilisateur clique sur ce bouton, le **boîte à outils** et **propriétés** volets sont activés et la **barre d’outils Éditeur de texte** n’est pas affiché. Lorsque le **réduire** bouton (voir ci-dessous), le **conception** volet est présenté seul sans le **Source** volet.

-   **Permuter les volets** &ndash; ce bouton (qui ressemble à deux flèches contradictoires) permute le **conception** et **Source** volets afin que le **Source** volet se trouve sur la gauche et le **conception** volet se trouve à droite. Ces volets en cliquant permutations retour à leurs emplacements d’origine.

-   **Source** &ndash; sélectionne ce bouton (qui ressemble à deux crochets pointus contradictoires) le **Source** volet. Lorsque l’utilisateur clique sur ce bouton, le **boîte à outils** et **propriétés** volets sont désactivées et le **barre d’outils Éditeur de texte** est rendu visible dans la partie supérieure de Visual Studio. Lorsque le **réduire** bouton est activé (voir ci-dessous), en cliquant sur le **Source** bouton affiche le **Source** volet au lieu du **conception** volet.

-   **Division verticale** &ndash; ce bouton (qui ressemble à une barre verticale), affiche le **conception** et **Source** volets côte à côte. Il s’agit de la disposition par défaut.

-   **Fractionnement horizontal** &ndash; ce bouton (qui ressemble à une barre horizontale), affiche le **conception** volet ci-dessus le **Source** volet. **Permuter les volets** peut être sélectionné pour placer le **Source** volet ci-dessus le **conception** volet.

-   **Réduire le volet** &ndash; l’affichage de deux volets de ce bouton (qui ressemble à deux pointant vers la droite chevrons) « réduit » **conception** et **Source** dans une vue unique d’un des Ces volets.
    Ce bouton devient le **développer le volet** bouton (qui ressemble à deux crochets d’angle pointant vers la gauche), qui peut être activé pour retourner la vue à deux volets (**conception** et **Source**) en mode d’affichage.

Lorsque **réduire le volet** est activé, seuls les **conception** volet s’affiche. Toutefois, vous pouvez cliquer sur le **Source** bouton pour afficher uniquement les **Source** volet. Cliquez sur le **conception** bouton pour revenir à la **conception** volet.

## <a name="source-pane"></a>Volet source

Le **Source** volet affiche la source XML sous-jacent dans la conception présentée sur le **aire de conception**. Étant donné que les deux vues sont disponibles en même temps, il est possible de créer une conception d’interface utilisateur en effectuant des allers-retours entre une représentation visuelle de la conception et de la source XML sous-jacente pour la conception :

[![Exemple source XML dans le volet Source](designer-basics-images/vs/22-source-pane-w158-sml.png)](designer-basics-images/vs/22-source-pane-w158.png#lightbox)

Modifications apportées à la source XML sont immédiatement rendues sur le **aire de conception**; modifications apportées sur le **aire de conception** provoquer la source XML affichée dans le **Source** volet pour mettre à jour en conséquence. Lorsque vous apportez des modifications au format XML dans le **Source** volet, la saisie semi-automatique et les fonctionnalités IntelliSense sont disponibles pour la vitesse de développement d’interface utilisateur basée sur le XML comme expliqué ci-après.

Pour une plus grande facilité de navigation lorsque vous travaillez avec des fichiers XML long, le **Source** volet prend en charge la barre de défilement Visual Studio (comme indiqué à droite dans la capture d’écran précédente). Pour plus d’informations sur la barre de défilement, consultez [comment suivre votre Code en personnalisant la barre de défilement](https://msdn.microsoft.com/library/dn237345.aspx).


### <a name="autocompletion"></a>Saisie semi-automatique

Lorsque vous commencez à taper le nom d’un attribut pour un widget, vous pouvez appuyer sur <kbd>CTRL + espace</kbd> pour afficher la liste des saisies semi-automatiques possibles. Par exemple, après avoir entré `android:lay` dans l’exemple suivant (suivi en tapant <kbd>CTRL + espace</kbd>), présentation de la liste suivante :

[![Saisie semi-automatique de l’attribut de présentation](designer-basics-images/vs/23-autocompletion-w158-sml.png)](designer-basics-images/vs/23-autocompletion-w158.png#lightbox)

Appuyez sur <kbd>entrée</kbd> pour accepter l’achèvement répertorié premier ou utilisez les touches de direction pour faire défiler à la saisie semi-automatique souhaité et appuyez sur <kbd>entrée</kbd>. Ou bien, vous pouvez utiliser la souris pour faire défiler à et cliquez sur la saisie semi-automatique souhaité.

### <a name="intellisense"></a>IntelliSense

Une fois que vous entrez un nouvel attribut pour un widget et commencez à lui attribuer une valeur, IntelliSense s’affiche après qu’un caractère de déclencheur est de type et fournit une liste de valeurs valides à utiliser pour cet attribut. Par exemple, après le premier guillemet double est entré pour `android:layout_width` dans l’exemple suivant, un sélecteur de saisie semi-automatique s’affiche pour fournir la liste de choix valides pour cette largeur :

[![Exemple d’IntelliSense pour la largeur de la disposition](designer-basics-images/vs/24-intellisense-w158-sml.png)](designer-basics-images/vs/24-intellisense-w158.png#lightbox)

En bas de cette fenêtre contextuelle sont deux boutons (comme souligné en rouge dans la capture d’écran ci-dessus). En cliquant sur le **ressources Project** bouton sur la gauche limite la liste aux ressources qui font partie du projet d’application, tout en cliquant sur le **ressources Framework** bouton à droite limite la liste à afficher les ressources disponibles à partir de l’infrastructure.
Ces boutons Activer / désactiver : vous pouvez cliquer à nouveau afin de désactiver l’action de filtrage que chacune fournit.



## <a name="properties-pane"></a>Volet Propriétés

Le concepteur prend en charge la modification des propriétés du widget via la **propriétés** volet : 

![Capture d’écran de la fenêtre Propriétés](designer-basics-images/vs/08-property-pad.png)

Les propriétés répertoriées dans le **propriétés** modification volet selon le widget est sélectionné sur la **aire de conception**.

### <a name="default-values"></a>Valeurs par défaut

Les propriétés de la plupart des widgets seront vides dans le **propriétés** fenêtre étant donné que leurs valeurs héritent du thème sélectionné Android.
Le **propriétés** fenêtre affiche uniquement les valeurs qui sont explicitement définies pour le widget sélectionné ; il n’affiche pas les valeurs qui sont héritées du thème.

### <a name="referencing-resources"></a>Référence à des ressources

Certaines propriétés peuvent référencer des ressources qui sont définies dans les fichiers autres que la mise en page **.axml** fichier. Les scénarios les plus courants de ce type sont `string` et `drawable` ressources. Toutefois, les références peuvent également servir pour d’autres ressources, telles que `Boolean` valeurs et des dimensions. Lorsqu’une propriété prend en charge les références de ressources, une icône de navigation (un carré) s’affiche en regard de l’entrée de texte pour la propriété. Ce bouton ouvre un sélecteur de ressources d’un clic.

Par exemple, la capture d’écran suivante montre les options disponibles lorsque vous cliquez sur le carré sombre à droite du champ de texte pour un `Text` widget dans le **propriétés** fenêtre :

[![Exemple de liste des options de texte](designer-basics-images/vs/09-text-options-sml.png)](designer-basics-images/vs/09-text-options.png#lightbox)

Lorsque **ressource...**  est activé, le **sélectionner une ressource** boîte de dialogue est présentée :

[![Capture d’écran d’exemple ressources avec plusieurs ressources répertoriées](designer-basics-images/vs/09b-resources-w158-sml.png)](designer-basics-images/vs/09b-resources-w158.png#lightbox)

Dans cette liste, vous pouvez sélectionner une ressource de texte à utiliser pour ce widget au lieu de coder en dur le texte dans le **propriétés** volet. L’exemple suivant illustre le sélecteur de ressource pour le `Src` propriété d’un `ImageView`:

[![Sélecteur de ressources répertoriant les ressources de l’icône pour un ImageView](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

En cliquant sur le carré vide à droite de la `Src` propriété ouvre le **sélectionner une ressource** boîte de dialogue avec une liste de ressources allant de couleurs (comme indiqué ci-dessus) à drawables.


### <a name="boolean-property-references"></a>Références de propriété booléenne

*Boolean* propriétés sont normalement sélectionnées en tant que les coches en regard d’une propriété dans la fenêtre Propriétés. Vous pouvez désigner un `true` ou `false` valeur en cochant ou décochant cette case à cocher, ou vous pouvez sélectionner une référence de propriété en cliquant sur le carré foncé remplie à droite de la propriété. Dans l’exemple suivant, le texte est modifié à tout en majuscules en cliquant sur le **texte tout en majuscules** référence de propriété booléenne associée `TextView`:

![Exemple de définition de propriétés booléennes](designer-basics-images/vs/11-boolean.png)

## <a name="editing-properties-inline"></a>Modification des propriétés inline

Le concepteur Android prend en charge la modification directe de certaines propriétés sur le **aire de conception** (de sorte que vous n’êtes pas obligé de rechercher ces propriétés dans la liste de propriétés). Les propriétés qui peuvent être modifiées directement incluent le texte, marge et la taille.

### <a name="text"></a>Texte

Les propriétés de texte de certains widgets (tel que `Button` et `TextView`), peuvent être modifiées directement sur le **aire de conception**. En double-cliquant sur un widget est placez-le en mode édition, comme indiqué ci-dessous :

![Ressource de texte pour la chaîne hello](designer-basics-images/vs/12-text-resource.png "ressource de texte")

Vous pouvez entrer une nouvelle valeur de texte, ou vous pouvez entrer une nouvelle chaîne de ressource. Dans l’exemple suivant, le `@string/hello` ressource est remplacée par le texte, `CLICK THIS BUTTON`:

![MAJ + ENTRÉE pour lier automatiquement le texte à une nouvelle ressource](designer-basics-images/vs/13-shift-enter-resource.png)

Cette modification est stockée dans le widget `text` propriété ; elle ne modifie pas la valeur affectée à la `@string/hello` ressource.
Lorsque vous la clé dans une nouvelle chaîne de texte, vous pouvez appuyer sur <kbd>MAJ</kbd> +
<kbd>entrée</kbd> pour lier automatiquement le texte entré à une nouvelle ressource.

### <a name="margin"></a>Marge

Lorsque vous sélectionnez un widget, le concepteur affiche les poignées qui vous permettent de modifier la taille ou la marge du widget de manière interactive. En cliquant sur le widget lorsqu’il est sélectionné bascule entre le mode d’édition de marge et le mode de modification de taille.

Lorsque vous cliquez sur un widget pour la première fois, les poignées de marge sont affichées. Si vous déplacez la souris vers une des poignées, le concepteur affiche la propriété qui change le descripteur (comme indiqué ci-dessous pour le `layout_marginLeft` propriété) :

![Capture d’écran montrant la marge gère dans le Concepteur](designer-basics-images/vs/15-margin-handles.png)

Si une marge a déjà été définie, les lignes en pointillés sont affichés, indiquant l’espace occupé par la marge :

![Exemple de lignes en pointillés marquage espace autour d’un bouton](designer-basics-images/vs/16-margins-set.png)

### <a name="size"></a>Size

Comme mentionné précédemment, vous pouvez basculer en mode d’édition à la taille en cliquant sur un widget lorsqu’il est déjà sélectionné. Cliquez sur la poignée triangulaire pour définir la taille pour la dimension indiquée à `wrap_content`:

![Poignées de redimensionnement et de contenu de type Wrap](designer-basics-images/vs/17-wrap-content.png)

En cliquant sur le **encapsuler le contenu** handle réduit le widget de la dimension afin qu’il ne dépasse pas nécessaire pour encapsuler le contenu joint. Dans cet exemple, le texte du bouton est réduite horizontalement, comme illustré dans la capture d’écran suivante.

Lorsque la valeur de taille est définie sur **encapsuler le contenu**, le concepteur affiche un handle triangulaire qui pointent dans la direction opposée de modification de la taille à `match_parent`:

![Handle parent de correspondance](designer-basics-images/vs/18-match-parent.png)

En cliquant sur le **correspondance Parent** handle restaure la taille de la dimension afin qu’il soit le même que le widget de parent.

En outre, vous pouvez faire glisser la poignée de redimensionnement circulaire (comme indiqué dans les captures d’écran ci-dessus) pour redimensionner le widget à un élément arbitraire `dp` valeur. Lorsque vous procédez ainsi, les deux **encapsuler le contenu** et **correspondance Parent** handles sont proposées pour cette dimension :

![Poignées de redimensionnement circulaire](designer-basics-images/vs/19-resize-dp.png)

Pas tous les conteneurs autorisent la modification du `Size` d’un widget. Par exemple, notez que dans la capture d’écran ci-dessous avec le `LinearLayout` sélectionnée, les poignées de redimensionnement n’apparaissent pas :

![Aucune des poignées de redimensionnement](designer-basics-images/vs/20-no-resize-handles.png)


## <a name="document-outline"></a>Structure du document

Le **structure du Document** affiche la hiérarchie de widget de la disposition.
Dans l’exemple suivant, le contenant `LinearLayout` widget est sélectionné :

![Exemple de structure du document](designer-basics-images/vs/21-document-outline.png)

Le contour du widget sélectionné (dans ce cas, un `LinearLayout`) est également mis en surbrillance sur la **aire de conception**. Le widget sélectionné dans la structure du Document reste synchronisé avec son homologue sur le **aire de conception**. Cela est utile pour la sélection d’afficher les groupes, qui ne sont pas toujours faciles à sélectionner sur le **aire de conception**.

Le **structure du Document** prend en charge la copie et collage ou vous pouvez utiliser la fonction glisser-déplacer. Glisser -déplacer est prise en charge à partir de la **structure du Document** à la **aire de conception** ainsi qu’à partir de la **aire de conception** à la **structure du Document**. En outre, clic droit sur un élément dans le **structure du Document** affiche le menu contextuel pour cet élément (le même menu contextuel qui s’affiche lorsque vous cliquez sur ce widget même sur le **aire de conception**).




# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="launching-the-designer"></a>Lancer le Concepteur

Le concepteur est lancé automatiquement lorsqu’une mise en page est créée, ou il peut être lancé en double-cliquant sur un fichier .axml existant. Par exemple, en double-cliquant sur **Main.axml** dans le **ressources > disposition** dossier chargera le concepteur, comme indiqué ci-dessous :

[![Écran Concepteur dans Visual Studio pour Mac](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

De même, vous pouvez ajouter une nouvelle disposition en double-cliquant sur le **disposition** dossier dans le **panneau solutions** et en sélectionnant **Ajouter > nouveau fichier > Android > disposition**:

[![Boîte de dialogue Nouveau fichier ajouter](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

Cela crée un nouveau fichier .axml et les charge sur l’aire de conception.

## <a name="designer-features"></a>Fonctionnalités du Concepteur

Le concepteur est composé de plusieurs sections qui prennent en charge ses différentes fonctionnalités, comme illustré dans la capture d’écran suivante :

[![Diagramme des volets du Concepteur](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

Lorsque vous modifiez une mise en page dans le concepteur, vous utilisez les fonctionnalités suivantes pour créer et mettre en forme votre conception :

-   **Aire de conception** &ndash; facilite la construction visuelle de l’interface utilisateur en vous offrant une représentation modifiable de la façon dont la disposition s’affiche sur l’appareil.

-   **Barre d’outils** &ndash; affiche une liste de sélecteurs : **APPAREIL**, **Version**, **thème**, configuration de mise en page et les paramètres de la barre d’Action. La barre d’outils comprend également des icônes pour le lancement de l’éditeur de thème et pour l’activation de la grille de création de documents.

-   **Boîte à outils** &ndash; fournit une liste de configurations que vous pouvez faire glisser et déposer sur l’aire de conception et de widgets.

-   **Remplissage de la propriété** &ndash; répertorie les propriétés du widget sélectionné pour afficher et modifier.

-   **Structure du document** &ndash; affiche l’arborescence de widgets qui composent la disposition. Vous pouvez cliquer sur un élément dans l’arborescence pour qu’elle être sélectionné dans le concepteur. En outre, cliquer sur un élément dans l’arborescence de la charge les propriétés de l’élément dans le panneau de la propriété.

## <a name="toolbar"></a>ToolBar

La barre d’outils (positionnée au-dessus de l’aire de conception) présente les sélecteurs de configuration et les menus d’outils :

[![Diagramme de la barre d’outils Concepteur](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

La barre d’outils permet d’accéder aux fonctionnalités suivantes :

-   **Autre sélecteur de disposition** &ndash; vous permet de sélectionner à partir de versions de mise en page différente.

-   **Sélecteur d’appareils** &ndash; définit un jeu de qualificateurs associé à un périphérique particulier, telles que la taille de l’écran, résolution et la disponibilité de clavier. Vous pouvez également ajouter et supprimer des nouveaux appareils.

-   **Sélecteur de Version Android** &ndash; The Android version que la cible de la mise en page. Le concepteur affichera la disposition en fonction de la version Android sélectionnée.

-   **Sélecteur de thème** &ndash; sélectionne le thème de l’interface utilisateur pour la mise en page.

-   **Sélecteur de configuration** &ndash; sélectionne la configuration de l’appareil, tel que *portrait* ou *paysage*.

-   **Options du qualificateur de ressource** &ndash; ouvre une boîte de dialogue qui présente les menus déroulants pour sélectionner *langage*, *Mode d’interface utilisateur*, *Mode nuit*, et *Écran arrondi* options.

-   **Paramètres de barre d’action** &ndash; configure les paramètres de la barre d’Action pour la mise en page.

-   **Éditeur de thème** &ndash; ouvre le *éditeur de thème*, ce qui rend possible pour vous permettent de personnaliser les éléments du thème sélectionné.

-   **Grille de création de matériau** &ndash; Active ou désactive le *grille de conception de matériau*. L’élément de menu de liste déroulante adjacente à la grille de conception de matériau ouvre une boîte de dialogue qui vous permet de personnaliser la grille.

Chacune de ces fonctionnalités est expliquée plus en détail dans les rubriques suivantes :

[Qualificateurs de ressources et les Options de visualisation](~/android/user-interface/android-designer/resource-qualifiers.md) fournit des informations détaillées sur la **sélecteur d’appareils**, **sélecteur de Version Android**, **sélecteur de thème**, **Configuration sélecteur**, **ressource Qualifications Options**, et **les paramètres de barre d’Action**.

[Des vues de disposition](~/android/user-interface/android-designer/alternative-layout-views.md) explique comment utiliser le **sélecteur de disposition Alternative**.

[Fonctionnalités de conception de matériau](~/android/user-interface/android-designer/material-design-features.md) fournit une vue d’ensemble complète de la **éditeur de thème** et le **grille de conception de matériau**.

## <a name="design-surface"></a>Aire de conception

Le concepteur rend possible pour vous faire glisser et déposer des widgets à partir de la boîte à outils vers l’aire de conception. Lorsque vous interagissez avec les widgets dans le concepteur (soit en ajoutant de nouveaux widgets soit repositionnement existants), les lignes horizontales et verticales sont affichés pour marquer les points d’insertion disponibles. Dans l’exemple suivant, un nouveau `Button` widget est glissé vers l’aire de conception :

[![Exemples de lignes d’insertion sur l’aire de conception](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

En outre, les widgets peuvent être copiés : vous pouvez utiliser copier et coller pour copier un widget, ou vous pouvez glisser -déplacer un widget existant tout en maintenant la <kbd>Ctrl</kbd> clé.

### <a name="context-menu-commands"></a>Commandes de menu contextuel

Un menu contextuel est disponible dans l’aire de conception et dans la structure du Document. Ce menu affiche les commandes qui sont disponibles pour le widget sélectionné et son conteneur, ce qui facilite pour effectuer des opérations sur les conteneurs (qui ne sont pas toujours faciles à sélectionner sur l’aire de conception). Voici un exemple d’un menu contextuel :

[![Menu de contexte exemple lors du clic droit sur l’aire de conception](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

Dans cet exemple, clic droit sur un `Button` ouvre un menu contextuel qui fournit plusieurs options :

-   **LinearLayout** &ndash; ouvre un sous-menu pour la modification la `LinearLayout` parent de la `Button`.

-   **Couper**, **copie**, et **supprimer** &ndash; opérations qui s’appliquent à l’utilisateur clique `Button`.

### <a name="zoom-controls"></a>Contrôles de zoom

L’aire de conception prend en charge le zoom par le biais de plusieurs contrôles, comme indiqué :

[![Diagramme des contrôles de zoom d’aire de conception](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

Ces contrôles rendent plus facile à voir certaines zones de l’interface utilisateur dans le concepteur :

-   **Mettez en surbrillance les conteneurs** &ndash; met en évidence les conteneurs sur l’aire de conception afin qu’ils soient plus faciles de localiser tout en zoom avant et arrière.

-   **Taille normale** &ndash; restitue la disposition de pixels afin que vous pouvez voir l’aspect de la mise en page à la résolution de l’appareil sélectionné.

-   **Ajuster à la fenêtre** &ndash; définit le niveau de zoom afin que la disposition entière est visible sur l’aire de conception.

-   **Effectuer un zoom avant dans** &ndash; effectue un zoom avant façon incrémentielle avec chaque clic, l’agrandissement de la mise en page.

-   **Zoom arrière** &ndash; effectue un zoom arrière incrémentielle avec chaque clic, rendre la disposition s’affichent plus petits sur l’aire de conception.

Notez que le paramètre du zoom choisi n’affecte pas l’interface utilisateur de l’application lors de l’exécution.

## <a name="property-pad"></a>Remplissage de la propriété

Le concepteur prend en charge la modification des propriétés du widget via la **remplissage de la propriété**. Les propriétés répertoriées dans la modification de remplissage de la propriété selon laquelle le widget est sélectionné dans l’aire du concepteur. Lorsque le `Button` dans l’exemple précédent est sélectionné, les propriétés pour ce `Button` widget sont affichés :

[![Capture d’écran de la zone de propriété](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

## <a name="property-pad-sections"></a>Sections de panneau Propriétés

Le panneau de la propriété est divisé en plusieurs sections qui regroupent des propriétés similaires &ndash; cela le rend plus facile de localiser les propriétés qui vous intéressent :

-   **Widget** &ndash; propriétés principales du widget, tel que `id`, `visibility`, `text`, etc. Propriétés de gestion du contenu du widget sont généralement placées ici.

-   **Style** &ndash; propriétés qui modifient l’apparence visuelle du widget, tel que `font`, `text color`, `background`, etc.

-   **Disposition** &ndash; propriétés que définir l’emplacement et la taille du widget.

-   **Défilement** &ndash; propriétés de défilement.

-   **Comportement** &ndash; indicateurs définis comment se comporte le widget.

### <a name="default-values"></a>Valeurs par défaut

Les propriétés de la plupart des widgets seront vides dans le **remplissage de la propriété** étant donné que leurs valeurs héritent du thème sélectionné Android. Le **remplissage de la propriété** affichera uniquement les valeurs qui sont définies explicitement pour le widget sélectionné ; il n’affiche pas les valeurs qui sont héritées du thème.

### <a name="referencing-resources"></a>Référence à des ressources

Certaines propriétés peuvent référencer des ressources qui sont définies dans les fichiers autres que la mise en page **.axml** fichier. Les scénarios les plus courants de ce type sont `string` et `drawable` ressources. Toutefois, les références peuvent également servir pour d’autres ressources, telles que `Boolean` valeurs et des dimensions.
Lorsqu’une propriété prend en charge les références de ressources, une icône de navigation (un points de suspension, &hellip;) s’affiche en regard de l’entrée de texte pour la propriété.
Lorsque vous cliquez dessus, ce bouton ouvre un sélecteur de ressources.

Par exemple, la capture d’écran suivante montre les ressources disponibles lorsque vous cliquez sur les points de suspension à droite du champ de texte pour un `Button` widget dans le **remplissage de la propriété**:

[![Capture d’écran de ressources exemple avec deux ressources répertoriées](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

L’exemple suivant illustre le sélecteur de ressource pour le `Src` propriété d’un `ImageView`:

[![Sélecteur de ressources répertoriant les ressources de l’icône pour un ImageView](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

### <a name="boolean-property-references"></a>Références de propriété booléenne

*Boolean* propriétés s’affichent normalement en tant qu’une case à cocher dans le panneau de la propriété. Quand un `Boolean` propriété prend en charge les références de ressources, une petite case à cocher s’affiche en regard de la propriété. Une case cochée signifie `true` et une case à cocher vide signifie `false`. Vous pouvez également directement entrer une valeur comme `true` ou `false`. Pointez votre souris sur l’entrée affiche une icône de champ de texte de petite taille. Si vous souhaitez entrer la valeur manuellement, vous pouvez cliquer dessus.

[![Exemple de définition de propriétés booléennes](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>Propriétés groupées

Certains widgets ont des propriétés à valeurs multiples qui sont regroupées ensemble (tel que `Padding`, par exemple). Ces valeurs de propriété sont répertoriées dans le **remplissage de la propriété** dans une ligne unique, pouvant être développée. Certaines de ces propriétés peuvent être modifiées directement dans la ligne groupée, telles que le `Padding` propriété indiquée ci-dessous :

[![Exemples de paramètres pour la propriété de remplissage](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

## <a name="editing-properties-inline"></a>Modification des propriétés inline

Le concepteur Android prend en charge la modification directe de certaines propriétés sur l’aire de conception (de sorte que vous n’êtes pas obligé de rechercher ces propriétés dans la liste de propriétés). Les propriétés qui peuvent être modifiées directement incluent le texte, marge et la taille.

### <a name="text"></a>Texte

Les propriétés de texte de certains widgets (tel que `Button` et `TextView`), peuvent être modifiées directement sur l’aire de conception. En double-cliquant sur un widget est placez-le en mode édition, comme indiqué ci-dessous :

[![Ressource de texte pour la chaîne hello](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

Vous pouvez entrer une nouvelle valeur de texte, ou vous pouvez entrer une nouvelle chaîne de ressource. Dans l’exemple suivant, le `@string/hello` ressource est remplacée par le texte, `CLICK THIS BUTTON`:

[![MAJ + ENTRÉE pour lier automatiquement le texte à une nouvelle ressource](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

Cette modification est stockée dans le widget `text` propriété ; elle ne modifie pas la valeur affectée à la `@string/hello` ressource.
Lorsque vous la clé dans une nouvelle chaîne de texte, vous pouvez appuyer sur <kbd>MAJ</kbd> +
<kbd>entrée</kbd> pour lier automatiquement le texte entré à une nouvelle ressource.

### <a name="margin"></a>Marge

Lorsque vous sélectionnez un widget, le concepteur affiche les poignées qui vous permettent de modifier la taille ou la marge du widget de manière interactive. En cliquant sur le widget lorsqu’il est sélectionné bascule entre le mode d’édition de marge et le mode de modification de taille.

Lorsque vous cliquez sur un widget pour la première fois, les poignées de marge sont affichées. Si vous déplacez la souris vers une des poignées, le concepteur affiche la propriété qui change le descripteur (comme indiqué ci-dessous pour le `layout_marginLeft` propriété) :

[![Capture d’écran montrant la marge gère dans le Concepteur](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

Si une marge a déjà été définie, les lignes en pointillés sont affichés, indiquant l’espace occupé par la marge :

[![Exemple de lignes en pointillés marquage espace autour d’un bouton](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

### <a name="size"></a>Size

Comme mentionné précédemment, vous pouvez basculer en mode d’édition à la taille en cliquant sur un widget lorsqu’il est déjà sélectionné. Cliquez sur la poignée triangulaire pour définir la taille pour la dimension indiquée à `wrap_content`:

[![Poignées de redimensionnement et de contenu de type Wrap](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

En cliquant sur le **encapsuler le contenu** handle réduit le widget dans cette dimension de sorte qu’il ne dépassant pas nécessaire pour encapsuler le contenu joint. Dans cet exemple, le texte du bouton est réduite horizontalement, comme illustré dans la capture d’écran suivante.

Lorsque la valeur de taille est définie sur **encapsuler le contenu**, le concepteur affiche un handle triangulaire qui pointent dans la direction opposée de modification de la taille à `match_parent`:

[![Handle parent de correspondance](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

En cliquant sur le **correspondance Parent** handle restaure la taille de la dimension afin qu’il soit le même que le widget de parent.

En outre, vous pouvez faire glisser la poignée de redimensionnement circulaire (comme indiqué dans les captures d’écran ci-dessus) pour redimensionner le widget à un élément arbitraire `dp` valeur. Lorsque vous procédez ainsi, les deux **encapsuler le contenu** et **correspondance Parent** handles sont proposées pour cette dimension :

[![Poignées de redimensionnement circulaire](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

Pas tous les conteneurs autorisent la modification du `Size` d’un widget. Par exemple, notez que dans la capture d’écran ci-dessous avec le `LinearLayout` sélectionnée, les poignées de redimensionnement n’apparaissent pas :

[![Aucune des poignées de redimensionnement](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

## <a name="document-outline"></a>Structure du document

Le **structure du Document** affiche la hiérarchie de widget de la disposition.
Dans l’exemple suivant, le contenant `LinearLayout` widget est sélectionné :

[![Structure du document](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

Le contour du widget sélectionné (dans ce cas, un `LinearLayout`) est également mis en surbrillance sur l’aire de conception. Le widget sélectionné dans la structure du Document reste synchronisé avec son homologue sur l’aire de conception. Cela est utile pour la sélection d’afficher les groupes, qui ne sont pas toujours faciles à sélectionner sur l’aire de conception.

La structure du Document prend en charge le copier et coller, ou vous pouvez utiliser la fonction glisser-déplacer. Glisser -déplacer est prise en charge à partir de la structure du Document à l’aire de conception, ainsi qu’à partir de l’aire de conception pour la structure du Document. En outre, le clic droit sur un élément dans la structure du Document affiche le menu contextuel pour cet élément (le même menu contextuel qui s’affiche lorsque vous cliquez sur ce même widget sur l’aire de conception).

-----
