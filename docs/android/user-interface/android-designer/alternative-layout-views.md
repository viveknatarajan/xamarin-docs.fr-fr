---
title: Autres vues de disposition
description: Cette rubrique explique comment les dispositions peuvent être gérées à l’aide de qualificateurs de ressources. Par exemple, peut renfermer une version d’une disposition qui est utilisée uniquement lorsque l’appareil est en mode paysage et une version de mise en page concerne uniquement le mode portrait.
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 03b80d3fb1ed7c8db108f86b3b3923c20e1d908f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109976"
---
# <a name="alternative-layout-views"></a>Autres vues de disposition

_Cette rubrique explique comment vous pouvez les dispositions de version à l’aide de qualificateurs de ressources. Par exemple, créez une version d’une disposition qui est utilisée uniquement lorsque l’appareil est en mode paysage et une version de mise en page concerne uniquement le mode portrait._

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="creating-alternative-layouts"></a>Création d’autres dispositions

Lorsque vous cliquez sur le **autre mode** icône (à gauche de **appareil**), un volet de visualisation s’ouvre pour répertorier les autres dispositions disponibles dans votre projet. S’il en existe aucun autres dispositions, le **par défaut** vue est présentée : 

[![Volet de la vue disposition autre](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "volet disposition alternative")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

Lorsque vous cliquez sur le signe plus vert ensuite **nouvelle Version**, le **créer une Variation de disposition** boîte de dialogue s’ouvre afin que vous puissiez sélectionner les qualificateurs de ressources de cette variation de disposition : 

[![Créer une variante de disposition](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "créer variation de disposition")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

Dans l’exemple suivant, l’identificateur de la ressource **orientation de l’écran** a la valeur **paysage**et le **taille de l’écran** est remplacée par **grande**. Cette opération crée une nouvelle version de mise en page nommée **grand land**:

[![Land de grande variation](alternative-layout-views-images/vs/03-large-land-sml.png "variante de terrain de grande taille")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

Notez que le volet de visualisation sur la gauche affiche les effets des sélections d’un qualificateur de nom de ressource. En cliquant sur **ajouter** crée la disposition alternative à cette disposition et le concepteur. Le **Alternative en mode** volet de visualisation indique la disposition est chargée dans le concepteur via un pointeur approprié comme indiqué dans la capture d’écran suivante : 

[![Indicateur de configuration chargé](alternative-layout-views-images/vs/04-new-layout-sml.png "indicateur de disposition chargé")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)


## <a name="editing-alternative-layouts"></a>Modification d’autres dispositions

Lorsque vous créez d’autres dispositions, il est souvent souhaitable d’effectuer une modification unique qui s’applique à toutes les versions dupliquées d’une disposition. Par exemple, vous souhaiterez modifier le texte du bouton par du jaune dans toutes les dispositions. Si vous avez un grand nombre de mises en page et vous avez besoin propager une seule modification à toutes les versions, la maintenance peut rapidement devenir fastidieux et sujet aux erreurs.

Pour simplifier la maintenance de plusieurs versions de mise en page, le concepteur fournit une **edit multi** mode qui propage les modifications sur une ou plusieurs dispositions. Lorsque plusieurs dispositions est présente, le **edit multi** icône s’affiche : 

[![Icône de modification multi](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "icône de modification multiples")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

Lorsque vous cliquez sur le **edit multi** icône, afficher des lignes qui indiquent que les dispositions sont liées (comme indiqué ci-dessous) ; autrement dit, lorsque vous apportez une modification à une disposition, cette modification est propagée vers les dispositions liées. Vous pouvez dissocier toutes les dispositions en cliquant sur l’icône de cercle indiqué dans la capture d’écran suivante : 

[![Dissocier toutes les dispositions](alternative-layout-views-images/vs/06-multi-linked-sml.png "dissocier toutes les dispositions")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

Si vous avez plus de deux dispositions, vous pouvez sélectivement activer/désactiver le bouton Modifier à gauche de chaque mode d’aperçu pour déterminer quelles mises en page sont liés entre eux. Par exemple, si vous souhaitez apporter une modification unique qui se propage aux premier et dernier de trois dispositions, vous serez dissociez d’abord la mise en page intermédiaire comme indiqué ici : 

[![Dissocier la mise en page du milieu](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "disposition intermédiaire dissocier")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)

Dans cet exemple, une modification apportée à le le **par défaut** ou **long** disposition sera propagée à la mise en page, mais pas à la **grand land** mise en page.

### <a name="multi-edit-example"></a>Exemple de modification multiples 

En règle générale, lorsque vous apportez une modification à une disposition, cette même modification est propagée à tous les autres dispositions liées. Par exemple, ajout d’une nouvelle `TextView` widget à la **par défaut** mise en page et en modifiant son texte de chaîne pour `Portrait` entraîne la même modification à apporter aux dispositions de toutes les liaisons. Voici à quoi elle ressemble le **par défaut** disposition : 

[![Ajouter TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "ajouter TextView")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
Le `TextView` est également ajouté à la **grand land** disposition afficher, car il est lié à la **par défaut** disposition : 

[![Paysage TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "paysage TextView")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
Mais que se passe-t-il si vous souhaitez apporter une modification est locale pour qu’une seule disposition (autrement dit, vous voulez la modification d’être propagées à un des autres les dispositions) ? Pour ce faire, vous devez dissocier la disposition que vous souhaitez modifier avant de le modifier, comme expliqué ci-après. 

### <a name="making-local-changes"></a>Apporter des modifications locales 

Supposons que nous voulons que les deux dispositions d’avoir ajouté `TextView`, mais nous souhaitons également modifier la chaîne de texte dans le **grand land** disposition à `Landscape` plutôt que `Portrait`. Si nous apporter cette modification à **grand land** tandis que les deux dispositions sont liées, la modification sera propagée vers le **par défaut** mise en page. Par conséquent, nous devons tout d’abord dissocier les deux dispositions d’avant de nous apporter la modification. Lorsque nous modifions le texte dans **grand land** à `Landscape`, le Concepteur de marque cette modification avec un cadre rouge pour indiquer que la modification est locale à la **grand land** mise en page et est *pas* propagées vers le **par défaut** disposition : 

[![Modification locale](alternative-layout-views-images/vs/10-local-change-sml.png "modification locale")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
Lorsque vous cliquez sur le **par défaut** mise en page pour l’afficher, le `TextView` chaîne de texte est toujours définie sur `Portrait`. 

## <a name="handling-conflicts"></a>Gestion des conflits 

Si vous décidez de modifier la couleur du texte dans le **par défaut** disposition au vert, vous verrez une icône d’avertissement s’affichent sur la disposition liée. Cliquez sur cette disposition pour ouvrir la disposition pour révéler le conflit. Le widget qui a provoqué le conflit est mis en surbrillance avec un cadre rouge et le message suivant s’affiche : *modifications récentes ont provoqué des conflits dans cette disposition alternative*. 

[![Les modifications en conflit](alternative-layout-views-images/vs/11-conflicting-change-sml.png "les modifications en conflit")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
Un *boîte conflit* s’affiche à droite du widget pour expliquer le conflit : 

[![Avertissement de conflit](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

La zone de conflit affiche la liste des propriétés qui ont été modifiés et répertorie leurs valeurs. En cliquant sur **ignorer le conflit** applique la modification de propriété uniquement à ce widget. En cliquant sur **appliquer** s’applique la modification de propriété pour ce widget, ainsi que pour le widget équivalent dans lié **par défaut** mise en page. Si toutes les modifications de propriété sont appliquées, le conflit est automatiquement supprimé. 

### <a name="view-group-conflicts"></a>Afficher les conflits de groupe 

Modifications apportées aux propriétés ne sont pas la seule source de conflits. Conflits peuvent être détectés lors de l’insertion ou la suppression des widgets. Par exemple, lorsque le **grand land** disposition est dissociée de la **par défaut** mise en page et le `TextView` dans le **grand land** disposition est glisser -déplacer au-dessus de la `Button`, le Concepteur de marque le widget déplacé pour indiquer le conflit :

[![Afficher les conflits de groupe](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "afficher les conflits de groupe")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
Toutefois, il n’existe aucun marqueur sur le `Button`. Bien que la position de la `Button` a changé, le `Button` n’indique aucune modification appliquée qui sont spécifiques à la **grand land** configuration. 

Si un `CheckBox` est ajouté à la **par défaut** mise en page, un autre conflit est généré et une icône d’avertissement s’affiche sur le **grand land** disposition : 

[![Conflit de case à cocher](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "conflit de case à cocher")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
En cliquant sur le **grand land** disposition révèle le conflit. Le message suivant s’affiche : *modifications récentes ont provoqué des conflits dans cette disposition alternative*: 

[![Conflit de mise en page ALT](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "conflit de mise en page Alt")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)

En outre, la zone de conflit affiche le message suivant :

[![Message de conflit](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Ajout de la `CheckBox` provoque un conflit, car le **grand land** disposition comporte des modifications le `LinearLayout` qui le contient. Toutefois, dans ce cas la zone de conflit affiche le widget qui vient d’être inséré dans le **par défaut** mise en page (le `CheckBox`).

Si vous cliquez sur **ignorer le conflit**, le concepteur résout le conflit, ce qui permet le widget affiché dans la zone de conflit pour glisser -déplacer dans la mise en page dans laquelle le widget est manquant (dans ce cas, le **grand land**  disposition) : 

[![Groupe conflit résolu](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "groupe conflit résolu")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)

Comme indiqué dans l’exemple précédent avec le `Button`, le `CheckBox` n’a pas d’une marque de modification rouge car uniquement le `LinearLayout` comporte des modifications qui ont été appliquées dans le **grand land** mise en page.



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="creating-alternative-layouts"></a>Création d’autres dispositions

Lorsque vous cliquez sur le **autre mode** icône (à gauche de **appareil**), un volet de visualisation s’ouvre pour répertorier les autres dispositions disponibles dans votre projet. S’il en existe aucun autres dispositions, le **par défaut** vue est présentée : 

[![Volet de la vue disposition alternative](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

Lorsque vous cliquez sur le signe plus vert ensuite **nouvelle Version**, le **créer une Variation de disposition** boîte de dialogue s’ouvre afin que vous puissiez sélectionner les qualificateurs de ressources de cette variation de disposition : 

[![Créer une variante de disposition](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

Dans l’exemple suivant, l’identificateur de la ressource **orientation de l’écran** a la valeur **paysage**et le **taille de l’écran** est remplacée par **grande**. Cette opération crée une nouvelle version de mise en page nommée **grand land**:

[![Variante de terrain de grande taille](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

Notez que le volet de visualisation sur la gauche affiche les effets des sélections d’un qualificateur de nom de ressource. En cliquant sur **ajouter** crée la disposition alternative à cette disposition et le concepteur. Le **Alternative en mode** volet de visualisation indique la disposition est chargée dans le concepteur via un pointeur approprié comme indiqué dans la capture d’écran suivante : 

[![Indicateur de configuration chargé](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>Modification d’autres dispositions

Lorsque vous créez d’autres dispositions, il est souvent souhaitable d’effectuer une modification unique qui s’applique à toutes les versions dupliquées d’une disposition. Par exemple, vous souhaiterez modifier le texte du bouton par du jaune dans toutes les dispositions. Si vous avez un grand nombre de mises en page et vous avez besoin propager une seule modification à toutes les versions, la maintenance peut rapidement devenir fastidieux et sujet aux erreurs.

Pour simplifier la maintenance de plusieurs versions de mise en page, le concepteur fournit une **edit multi** mode qui propage les modifications sur une ou plusieurs dispositions. Lorsque plusieurs dispositions est présente, le **edit multi** icône s’affiche : 

[![Icône de modification multiples](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

Lorsque vous cliquez sur le **edit multi** icône, afficher des lignes qui indiquent que les dispositions sont liées (comme indiqué ci-dessous) ; autrement dit, lorsque vous apportez une modification à une disposition, cette modification est propagée vers les dispositions liées. Vous pouvez dissocier toutes les dispositions en cliquant sur l’icône de cercle indiqué dans la capture d’écran suivante : 

[![Dissocier toutes les dispositions](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

Si vous avez plus de deux dispositions, vous pouvez sélectivement activer/désactiver le bouton Modifier à gauche de chaque mode d’aperçu pour déterminer quelles mises en page sont liés entre eux. Par exemple, si vous souhaitez apporter une modification unique qui se propage aux premier et dernier de trois dispositions, vous serez dissociez d’abord la mise en page intermédiaire comme indiqué ici : 

[![Dissocier la disposition du milieu](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)

Dans cet exemple, une modification apportée à le le **par défaut** ou **long** disposition est propagée aux autres disposition mais pas à la **grand land** mise en page. 

### <a name="multi-edit-example"></a>Exemple de modification multiples 

En règle générale, lorsque vous apportez une modification à une disposition, cette même modification est propagée à tous les autres dispositions liées. Par exemple, ajout d’une nouvelle `TextView` widget à la **par défaut** mise en page et en modifiant son texte de chaîne pour `Portrait` entraîne la même modification à apporter aux dispositions de toutes les liaisons. Voici à quoi elle ressemble le **par défaut** disposition : 

[![Ajouter TextView](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)

Le `TextView` est également ajouté à la **grand land** disposition afficher, car il est lié à la **par défaut** disposition : 

[![Paysage TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
Mais que se passe-t-il si vous souhaitez apporter une modification est locale pour qu’une seule disposition (autrement dit, vous voulez la modification d’être propagées à un des autres les dispositions) ? Pour ce faire, vous devez dissocier la disposition que vous souhaitez modifier avant de le modifier, comme expliqué ci-après. 

### <a name="making-local-changes"></a>Apporter des modifications locales 

Supposons que nous voulons que les deux dispositions d’avoir ajouté `TextView`, mais nous souhaitons également modifier la chaîne de texte dans le **grand land** disposition à `Landscape` plutôt que `Portrait`. Si nous apporter cette modification à **grand land** tandis que les deux dispositions sont liées, la modification sera propagée vers le **par défaut** mise en page. Par conséquent, nous devons tout d’abord dissocier les deux dispositions d’avant de nous apporter la modification. Lorsque nous modifions le texte dans **grand land** à `Landscape`, le Concepteur de marque cette modification avec un cadre rouge pour indiquer que la modification est locale à la **grand land** mise en page et est *pas* propagées vers le **par défaut** disposition : 

[![Modification locale](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)

Lorsque vous cliquez sur le **par défaut** mise en page pour l’afficher, le `TextView` chaîne de texte est toujours définie sur `Portrait`. 

## <a name="handling-conflicts"></a>Gestion des conflits 

Si vous décidez de modifier la couleur du texte dans le **par défaut** disposition au vert, vous verrez une icône d’avertissement s’affichent sur la disposition liée. Cliquez sur cette disposition pour ouvrir la disposition pour révéler le conflit. Le widget qui a provoqué le conflit est mis en surbrillance avec un cadre rouge et le message suivant s’affiche : *modifications récentes ont provoqué des conflits dans cette disposition alternative*. 

[![Modification en conflit](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)

Un *boîte conflit* s’affiche à droite du widget pour expliquer le conflit : 

[![Avertissement de conflit](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

La zone de conflit affiche la liste des propriétés qui ont été modifiés et répertorie leurs valeurs. En cliquant sur **ignorer le conflit** applique la modification de propriété uniquement à ce widget. En cliquant sur **appliquer** s’applique la modification de propriété pour ce widget, ainsi que pour le widget équivalent dans lié **par défaut** mise en page. Si toutes les modifications de propriété sont appliquées, le conflit est automatiquement supprimé. 

### <a name="view-group-conflicts"></a>Afficher les conflits de groupe 

Modifications apportées aux propriétés ne sont pas la seule source de conflits. Conflits peuvent être détectés lors de l’insertion ou la suppression des widgets. Par exemple, lorsque le **grand land** disposition est dissociée de la **par défaut** mise en page et le `TextView` dans le **grand land** disposition est glisser -déplacer au-dessus de la `Button`, le Concepteur de marque le widget déplacé pour indiquer le conflit :

[![Conflit de vue de groupe](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
Toutefois, il n’existe aucun marqueur sur le `Button`. Bien que la position de la `Button` a changé, le `Button` n’indique aucune modification appliquée qui sont spécifiques à la **grand land** configuration. 

Si un `CheckBox` est ajouté à la **par défaut** mise en page, un autre conflit est généré et une icône d’avertissement s’affiche sur le **grand land** disposition : 

[![Conflit de case à cocher](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
En cliquant sur le **grand land** disposition révèle le conflit. Le message suivant s’affiche : *modifications récentes ont provoqué des conflits dans cette disposition alternative*. 

[![Conflit de mise en page ALT](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
En outre, la zone de conflit affiche le message suivant :

[![Message de conflit](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Ajout de la `CheckBox` provoque un conflit, car le **grand land** disposition comporte des modifications le `LinearLayout` qui le contient. Toutefois, dans ce cas la zone de conflit affiche le widget qui vient d’être inséré dans le **par défaut** mise en page (le `CheckBox`).

Si vous cliquez sur **ignorer le conflit**, le concepteur résout le conflit, ce qui permet le widget affiché dans la zone de conflit pour glisser -déplacer dans la mise en page dans laquelle le widget est manquant (dans ce cas, le **grand land**  disposition) : 

[![Groupe conflit résolu](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
Comme indiqué dans l’exemple précédent avec le `Button`, le `CheckBox` n’a pas d’une marque de modification rouge car uniquement le `LinearLayout` comporte des modifications qui ont été appliquées dans le **grand land** mise en page.


-----


### <a name="conflict-persistence"></a>Persistance de conflit

Les conflits sont conservés dans le fichier de disposition sous forme de commentaires XML, comme illustré ici :

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Par conséquent, lorsqu’un projet est fermé et rouvert, tous les conflits sont toujours être il &ndash; même ceux qui ont été ignorés.



