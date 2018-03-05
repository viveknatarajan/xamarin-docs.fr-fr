---
title: "Autres modes d’affichage"
description: "Cette rubrique explique comment les dispositions peuvent être créées à l’aide de qualificateurs de ressources. Par exemple, il peut y être une version d’une disposition qui est utilisée uniquement quand l’appareil est en mode paysage et une version de mise en page qui est uniquement en mode portrait."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: f6884260315f8846720370c558f7435d2c5a9d91
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="alternative-layout-views"></a>Autres modes d’affichage

_Cette rubrique explique comment les dispositions peuvent être créées à l’aide de qualificateurs de ressources. Par exemple, il peut y être une version d’une disposition qui est utilisée uniquement quand l’appareil est en mode paysage et une version de mise en page qui est uniquement en mode portrait._

<a name="creating_alternative_layouts" />

## <a name="creating-alternative-layouts"></a>Création d’autres dispositions

Lorsque vous cliquez sur le **autre mode** icône (à gauche de **périphérique**), un volet de visualisation s’ouvre pour répertorier les autres dispositions disponibles dans votre projet. S’il n’y a aucune autre mise en page, le **par défaut** vue est présentée : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Volet d’affichage autre disposition](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "volet disposition différente")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Volet de la vue disposition différente](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png)

-----

Lorsque vous cliquez sur le signe plus ensuite **nouvelle Version**, le **créer une Variation de disposition** afin que vous puissiez sélectionner les qualificateurs de ressources pour cette variante de la mise en page de boîte de dialogue : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Créer une variante de disposition](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "créer une variante de la mise en page")](alternative-layout-views-images/vs/02-create-layout-variation.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Créer une variante de la mise en page](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png)

-----


Dans l’exemple suivant, l’identificateur de la ressource **orientation de l’écran** a la valeur **paysage**et le **taille de l’écran** est remplacée par **grande**. Cette opération crée une nouvelle version de mise en page nommée **grand-terrestres**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Terres de grande variation](alternative-layout-views-images/vs/03-large-land-sml.png "variante de terrains de grande")](alternative-layout-views-images/vs/03-large-land.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Variation de la terre grande](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png)

-----


Notez que le volet de visualisation sur la gauche affiche les effets des sélections de qualificateur de ressource. En cliquant sur **ajouter** crée la disposition d’autres à cette mise en page et le concepteur. Le **mode Alternative** volet de visualisation indique la disposition est chargée dans le concepteur via un pointeur à droite, comme indiqué dans la capture d’écran suivante : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Indicateur de mise en page chargées](alternative-layout-views-images/vs/04-new-layout-sml.png "indicateur de mise en page chargées")](alternative-layout-views-images/vs/04-new-layout.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Indicateur de mise en page chargées](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png)

-----


<a name="editing_alternative_layouts" />

## <a name="editing-alternative-layouts"></a>Modification d’autres dispositions

Lorsque vous créez d’autres dispositions, il est souvent souhaitable d’apporter une modification qui s’applique à toutes les versions comprenant des branches d’une disposition. Par exemple, vous souhaiterez modifier le texte du bouton jaune dans toutes les dispositions. Si vous avez un grand nombre de mises en page, la maintenance peut rapidement devenir fastidieuse et sujette à erreurs si vous avez besoin propager une modification à toutes les versions. 

Pour simplifier la maintenance de plusieurs versions de mise en page, le concepteur fournit une **modifier plusieurs** mode qui propage les modifications sur une ou plusieurs dispositions. Lorsque plusieurs dispositions est présente, le **modifier plusieurs** icône s’affiche : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Icône de modification de plusieurs](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "icône Modifier multiples")](alternative-layout-views-images/vs/05-multi-layout-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Icône de modification multiples](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png)

-----


Lorsque vous cliquez sur le **modifier plusieurs** icône, lignes apparaissent pour indiquer que les mises en page sont liés (comme indiqué ci-dessous) ; autrement dit, lorsque vous apportez une modification à une disposition, cette modification est propagée à toutes les dispositions liées. Vous pouvez rompre la liaison de toutes les dispositions en cliquant sur l’icône CERCLÉE indiqué dans la capture d’écran suivante : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Supprimer la liaison de toutes les dispositions](alternative-layout-views-images/vs/06-multi-linked-sml.png "supprimer la liaison de toutes les dispositions")](alternative-layout-views-images/vs/06-multi-linked.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Supprimer la liaison de toutes les dispositions](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png)

-----


Si vous avez plus de deux dispositions, vous pouvez basculer sélectivement le bouton Modifier à gauche de chaque mode d’aperçu pour déterminer quelles mises en page sont liés entre eux. Par exemple, si vous souhaitez apporter une modification unique qui se propage vers le premier et le dernier trois dispositions, vous serez tout d’abord supprimer la liaison de la disposition du milieu comme indiqué ici : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Supprimer la mise en page du milieu](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "dissocier la mise en page intermédiaire")](alternative-layout-views-images/vs/07-unlink-middle-layout.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Supprimer la liaison de mise en page intermédiaire](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png)
 
-----
 

Dans cet exemple, une modification apportée à la **par défaut** ou **long** disposition est propagée aux autres disposition, mais pas à la **grand-terrestres** mise en page. 


<a name="multi_edit_example" />

### <a name="multi-edit-example"></a>Exemple de modification de plusieurs 

En règle générale, lorsque vous apportez une modification à une disposition, ce même modification est propagée à toutes les autres dispositions liées. Par exemple, ajoutez un nouveau `TextView` widget à la **par défaut** mise en page et en modifiant son texte de chaîne pour `Portrait` entraîne la même modification à apporter aux dispositions toutes les liaisons. Voici comment il apparaît le **par défaut** disposition : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ajouter TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "ajouter TextView")](alternative-layout-views-images/vs/08-add-textview.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Ajouter TextView](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png)
 
-----
 

Le `TextView` est également ajouté à la **grand-terrestres** mode car elle est liée à la **par défaut** disposition : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Paysage TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "paysage TextView")](alternative-layout-views-images/vs/09-landscape-textview.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Paysage TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png)
 
-----
 

Mais que se passe-t-il si vous souhaitez apporter une modification est locale à disposition qu’un seul (autrement dit, vous souhaitez éviter la modification d’être propagées à un des autres configurations) ? Pour ce faire, vous devez supprimer le lien de la mise en page que vous souhaitez modifier avant de le modifier, comme expliqué ci-dessous. 


<a name="making_local_changes" />

### <a name="making-local-changes"></a>Apporter des modifications locales 

Supposons que nous voulons que les deux dispositions d’avoir ajouté `TextView`, mais nous souhaitons également modifier la chaîne de texte dans le **grand-terrestres** mise en page pour `Landscape` plutôt que `Portrait`. Si nous apporter cette modification à **grand-terrestres** tandis que les deux dispositions sont liées, la modification sera propagée à la **par défaut** mise en page. Par conséquent, nous devons tout d’abord supprimer la liaison les deux dispositions avant la modification. Lorsque nous modifions le texte dans **grand-terrestres** à `Landscape`, le concepteur marque cette modification avec un cadre rouge pour indiquer que la modification est locale pour le **terrestres grand** mise en page et est *pas* propagées vers le **par défaut** disposition : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Modification locale](alternative-layout-views-images/vs/10-local-change-sml.png "modification locale")](alternative-layout-views-images/vs/10-local-change.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Modification locale](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png)
 
-----
 

Lorsque vous cliquez sur le **par défaut** mise en page pour l’afficher, le `TextView` chaîne de texte est toujours définie `Portrait`. 


<a name="handling_conflicts" />

## <a name="handling-conflicts"></a>Gestion des conflits 

Si vous décidez de modifier la couleur du texte dans le **par défaut** disposition vert, vous verrez une icône d’avertissement s’affichent sur la disposition liée. En cliquant sur cette mise en page, la mise en page pour afficher le conflit s’ouvre. Le widget qui a provoqué le conflit est mis en surbrillance avec un cadre rouge et le message suivant s’affiche : *modifications récentes ont provoqué des conflits dans cette disposition autre*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Les modifications en conflit](alternative-layout-views-images/vs/11-conflicting-change-sml.png "les modifications en conflit")](alternative-layout-views-images/vs/11-conflicting-change.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Modification en conflit](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png)
 
-----
 

A *boîte conflit* est affichée à droite du widget pour expliquer le conflit : 

[ ![Avertissement de conflit](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png)

La boîte de conflit affiche la liste des propriétés qui ont été modifiés et répertorie leurs valeurs. En cliquant sur **ignorer le conflit** applique la modification de propriété uniquement à ce widget. En cliquant sur **appliquer** applique la modification de propriété à ce widget ainsi que pour le widget équivalent dans le texte lié **par défaut** mise en page. Si toutes les modifications de propriété sont appliquées, le conflit est automatiquement supprimé. 

<a name="view_group_conflicts" />

### <a name="view-group-conflicts"></a>Groupe d’affichage des conflits 

Modifications apportées aux propriétés ne sont pas la seule source de conflits. Les conflits peuvent être détectées lors de l’insertion ou la suppression de widgets. Par exemple, lorsque le **terrestres grand** disposition est dissociée de la **par défaut** mise en page et le `TextView` dans le **terrestres grand** disposition est déplacée et déposée au-dessus de la `Button`, le Concepteur de marque le widget déplacé pour indiquer le conflit :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Afficher les conflits de groupe](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "afficher les conflits de groupe")](alternative-layout-views-images/vs/12-view-group-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Conflit de groupe d’affichage](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png)
 
-----
 

Toutefois, il n’existe aucun marqueur sur le `Button`. Bien que la position de la `Button` a changé, le `Button` n’indique aucune modification appliquée qui sont spécifiques à la **grand-terrestres** configuration. 

Si un `CheckBox` est ajouté à la **par défaut** mise en page, un autre conflit est généré et une icône d’avertissement s’affiche sur le **grand-terrestres** disposition : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Conflit de case à cocher](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "conflit de case à cocher")](alternative-layout-views-images/vs/13-checkbox-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Conflit de case à cocher](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png)
 
-----
 

En cliquant sur le **grand-terrestres** disposition révèle le conflit. Le message suivant s’affiche : *modifications récentes ont provoqué des conflits dans cette disposition autre*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Conflit de mise en page ALT](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "conflit de mise en page Alt")](alternative-layout-views-images/vs/14-alt-layout-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Conflit de mise en page ALT](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png)
 
-----
 

En outre, le conflit affiche le message suivant :

[ ![Message de conflit](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png)

Ajout de la `CheckBox` entraîne un conflit, car la **terrestres grand** disposition comporte des modifications le `LinearLayout` qui le contient. Toutefois, dans ce cas le conflit affiche le widget qui vient d’être inséré dans le **par défaut** mise en page (la `CheckBox`).

Si vous cliquez sur **ignorer le conflit**, le concepteur résout le conflit, autorisant le widget affiché dans la zone de conflit pour être déplacé et déposé dans la mise en page dans laquelle le widget est manquant (dans ce cas, le **sur le terrain volumineux**  mise en page) : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Résoudre les conflits de groupe](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "résolu des conflits de groupe")](alternative-layout-views-images/vs/15-resolved-group-conflict.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[ ![Résoudre les conflits de groupe](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png)
 
-----
 

Comme indiqué dans l’exemple précédent avec la `Button`, le `CheckBox` n’a pas d’une marque de modification rouge car uniquement le `LinearLayout` comporte des modifications qui ont été appliquées dans le **terrestres grand** mise en page.


<a name="Conflict_Persistence" />

### <a name="conflict-persistence"></a>Persistance de conflit

Conflits sont conservés dans le fichier de disposition en tant que commentaires XML, comme illustré ici :

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Par conséquent, lorsqu’un projet est fermé et rouvert, tous les conflits sont toujours être &ndash; même ceux qui ont été ignorés.

