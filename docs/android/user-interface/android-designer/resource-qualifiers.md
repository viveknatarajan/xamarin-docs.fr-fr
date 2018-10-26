---
title: Qualificateurs de ressources et les Options de visualisation
description: Cette rubrique explique comment définir des ressources qui seront utilisées uniquement lorsque certaines valeurs de qualificateur sont mises en correspondance. Un exemple simple est une ressource de chaîne qualifié de langage. Une ressource de chaîne peut être définie comme la valeur par défaut, avec les autres ressources alternatives définis pour être utilisé dans des langues supplémentaires. Tous les types de ressources peuvent être qualifiés, y compris la mise en page lui-même.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 9d99e6a59b57b59d585b32befdadc0890d41448c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115627"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Qualificateurs de ressources et les options de visualisation

_Cette rubrique explique comment définir des ressources qui seront utilisées uniquement lorsque certaines valeurs de qualificateur sont mises en correspondance. Un exemple simple est une ressource de chaîne qualifié de langage. Une ressource de chaîne peut être définie comme la valeur par défaut, avec les autres ressources alternatives définis pour être utilisé dans des langues supplémentaires. Tous les types de ressources peuvent être qualifiés, y compris la mise en page lui-même._


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="resource-qualifier-options"></a>Options du qualificateur de ressource

**Options du qualificateur de ressource** est accessible en cliquant sur l’icône de points de suspension à droite de la **paysage** bouton mode :

[![Options du qualificateur de ressource](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

Cette boîte de dialogue présente les menus déroulants pour les qualificateurs de ressources suivants :

-   **Langage** &ndash; affiche les ressources de langue disponible et offre une option permettant d’ajouter de nouvelles ressources de langue/région.

-   **Mode d’interface utilisateur** &ndash; modes d’affichage de listes (tel que **Dock de voiture** et **Dock de bureau**), ainsi que le sens de disposition.

Chacun de ces menus déroulants s’ouvre de nouvelles boîtes de dialogue dans laquelle vous pouvez sélectionner et configurer les qualificateurs de ressources (comme expliqué ci-dessous).

### <a name="language"></a>Langue

Le **langage** menu déroulant répertorie uniquement les langues qui utilisent des ressources définies (ou **toutes les langues**, qui est la valeur par défaut). Toutefois, il existe également un **Ajouter langue/région...**  option qui vous permet d’ajouter un nouveau langage à la liste :

[![Ajouter une langue/région](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

Lorsque vous cliquez sur **Ajouter langue/région...** , le **sélectionnez une langue** boîte de dialogue s’ouvre et affiche les listes déroulantes de régions et langues disponibles :

![Liste des langues](resource-qualifiers-images/vs/10-languages.png "la liste des langues")

Dans cet exemple, nous avons choisi **fr (Français)** pour la langue et **BE** (Belgique) pour le dialecte régionaux Français. Notez que le **région** champ est facultatif, car de nombreux langages peuvent être spécifiés sans tenir compte des régions spécifiques. Lorsque le **langage** menu déroulant est ouvert à nouveau, il affiche la ressource de langue/région qui vient d’être ajouté :

![Langue et région choisie](resource-qualifiers-images/vs/11-language-region-added.png "choisi de langue et région")

Notez que si vous ajoutez un nouveau langage, mais vous ne créez pas de nouvelles ressources pour elle, la langue ajoutée n’utilisera plus affiche la prochaine fois que vous ouvrez le projet.

### <a name="ui-mode"></a>Mode d’interface utilisateur

Lorsque vous cliquez sur le **Mode d’interface utilisateur** menu déroulant, une liste des modes est indiquée, telles que **Normal**, **Dock de voiture**, **Dock de bureau**, **Télévision**, **Appliance**, et **espion**:


[![Menu du Mode d’interface utilisateur](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Sous cette liste sont les modes de nuit **nuit pas** et **nuit**, suivi par les instructions de mise en page **de gauche à droite** et **de droite à gauche** (pour plus d’informations sur **de gauche à droite** et **de droite à gauche** options, consultez [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)).
Les éléments de la dernière dans le **Options du qualificateur de ressource** boîte de dialogue sont les **arrondir les écrans** (pour une utilisation avec Android Wear) ou **écrans non arrondi**.
Pour plus d’informations sur les écrans de round et non arrondi, consultez [dispositions](https://developer.android.com/training/wearables/ui/layouts.html).
Pour plus d’informations sur les modes d’interface utilisateur Android, consultez [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

## <a name="action-bar-settings"></a>Paramètres de la barre d’action

Le **paramètres de barre d’Action** icône n’est disponible à gauche de l’icône représentant un pinceau (éditeur de thème) :

![Paramètres de la barre d’action](resource-qualifiers-images/vs/14-action-bar.png "les paramètres de barre d’Action")

Cette icône s’ouvre un menu de la boîte de dialogue qui fournit un moyen de sélectionner un des trois modes de barre d’Action :

-   **Standard** &ndash; se compose d’un logo ou un icône et un texte Titre et un sous-titre facultatif.

-   **Liste** &ndash; mode de navigation de liste. Au lieu de texte du titre statique, ce mode présente un menu de liste pour la navigation au sein de l’activité (autrement dit, il peut être présenté à l’utilisateur comme une liste déroulante).

-   **Onglets** &ndash; mode de navigation d’onglet. Au lieu de texte du titre statique, ce mode présente une série d’onglets de navigation au sein de l’activité.

## <a name="themes"></a>Thèmes

Le **thème** menu de liste déroulante affiche tous les thèmes définis dans le projet. En sélectionnant **plus de thèmes** ouvre une boîte de dialogue avec une liste de tous les thèmes disponibles à partir du SDK Android installés, comme indiqué ci-dessous :

[![Liste de thèmes plus](resource-qualifiers-images/vs/15-theme-menu-sml.png "liste de plus de thèmes")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

Lorsqu’un thème est sélectionné, l’aire de conception est mise à jour pour montrer l’effet du nouveau thème. Notez que cette modification est apportée permanente uniquement si le **OK** clic est effectué dans le **thème** boîte de dialogue. Une fois un thème a été sélectionné, elle sera incluse dans le **thème** déroulante comme indiqué ci-dessous :

![Le thème clair est désormais disponible](resource-qualifiers-images/vs/16-light-theme.png "le thème clair est désormais disponible")

## <a name="android-version"></a>Version d’Android

Android **Version** sélecteur définit la version Android qui est utilisée pour restituer la mise en page dans le concepteur. Le sélecteur affiche toutes les versions qui sont compatibles avec la version du framework cible du projet :

![Liste des versions Android](resource-qualifiers-images/vs/17-android-version.png "liste Android versions")

La version de framework cible peut être définie dans les paramètres du projet sous **Propriétés > applications > Compiler à l’aide de la version d’Android**. Pour plus d’informations sur la version du framework cible, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).

L’ensemble de widgets disponibles dans la boîte à outils est déterminé par la version du framework cible du projet. Cela est également vrai pour les propriétés disponibles dans le **fenêtre Propriétés**. La liste des widgets disponibles est *pas* déterminé par la valeur sélectionnée dans le **Version** sélecteur de la barre d’outils. Par exemple, si vous définissez la version cible du projet sur Android 4.4, vous pouvez toujours sélectionner Android 6.0 dans le sélecteur de version de barre d’outils pour voir à quoi ressemble le projet dans Android 6.0, mais vous ne pourrez pas ajouter des widgets qui sont spécifiques à Android 6.0 &ndash;  vous serez toujours limité pour les widgets qui sont disponibles dans Android 4.4.

Pour plus d’informations sur les types de ressources, consultez [ressources Android](~/android/app-fundamentals/resources-in-android/index.md).



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="resource-qualifier-options"></a>Options du qualificateur de ressource

**Options du qualificateur de ressource** est accessible en cliquant sur l’icône de points de suspension à droite de la **paysage** bouton mode :

[![Options du qualificateur de ressource](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

Cette boîte de dialogue présente les menus déroulants pour les qualificateurs de ressources suivants :

-   **Langage** &ndash; affiche les ressources de langue disponible et offre une option permettant d’ajouter de nouvelles ressources de langue/région.

-   **Mode d’interface utilisateur** &ndash; modes d’affichage de listes (tel que **Dock de voiture** et **Dock de bureau**), ainsi que le sens de disposition.

Chacun de ces menus déroulants s’ouvre de nouvelles boîtes de dialogue dans laquelle vous pouvez sélectionner et configurer les qualificateurs de ressources (comme expliqué ci-dessous).

### <a name="language"></a>Langue

Le **langage** menu déroulant répertorie uniquement les langues qui utilisent des ressources définies (ou **toutes les langues**, qui est la valeur par défaut). Toutefois, il existe également un **Ajouter langue/région...**  option qui vous permet d’ajouter un nouveau langage à la liste :

[![Ajouter une langue/région](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

Lorsque vous cliquez sur **Ajouter langue/région...** , le **sélectionnez une langue** boîte de dialogue s’ouvre et affiche les listes déroulantes de régions et langues disponibles :

[![Liste des langues](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

Dans cet exemple, nous avons choisi **fr (Français)** pour la langue et **BE** (Belgique) pour le dialecte régionaux Français. Notez que le **région** champ est facultatif, car de nombreux langages peuvent être spécifiés sans tenir compte des régions spécifiques. Lorsque le **langage** menu déroulant est ouvert à nouveau, il affiche la ressource de langue/région qui vient d’être ajouté :

[![Langue et région choisie](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

Notez que si vous ajoutez un nouveau langage, mais vous ne créez pas de nouvelles ressources pour elle, la langue ajoutée n’utilisera plus affiche la prochaine fois que vous ouvrez le projet.

### <a name="ui-mode"></a>Mode d’interface utilisateur

Lorsque vous cliquez sur le **Mode d’interface utilisateur** menu déroulant, une liste des modes est indiquée, telles que **Normal**, **Dock de voiture**, **Dock de bureau**, **Télévision**, **Appliance**, et **espion**:

[![Menu du Mode d’interface utilisateur](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

Sous cette liste sont les modes de nuit **nuit pas** et **nuit**, suivi par les instructions de mise en page **de gauche à droite** et **de droite à gauche**. La dernière paire d’options vous permet de sélectionner soit **arrondir écrans** ou **écrans rectangulaires** (utile pour les appareils Android Wear).

Pour plus d’informations sur les modes d’interface utilisateur Android, consultez [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Pour plus d’informations sur **de gauche à droite** et **de droite à gauche** options, consultez [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).


## <a name="action-bar-settings"></a>Paramètres de la barre d’action

Le **paramètres de barre d’Action** icône n’est disponible à gauche de l’icône représentant un pinceau (éditeur de thème) :

[![Paramètres de la barre d’action](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

Cette icône s’ouvre un menu de la boîte de dialogue qui fournit un moyen de sélectionner un des trois modes de barre d’Action :

-   **Standard** &ndash; se compose d’un texte logo ou icône et le titre et un sous-titre facultatif.

-   **Liste** &ndash; mode de navigation de liste. Au lieu de texte du titre statique, ce mode présente un menu de liste pour la navigation au sein de l’activité (autrement dit, il peut être présenté à l’utilisateur comme une liste déroulante).

-   **Onglets** &ndash; mode de navigation d’onglet. Au lieu de texte du titre statique, ce mode présente une série d’onglets de navigation au sein de l’activité.

## <a name="themes"></a>Thèmes

Le **thème** menu de liste déroulante affiche tous les thèmes définis dans le projet. En sélectionnant **plus de thèmes** ouvre une boîte de dialogue avec une liste de tous les thèmes disponibles à partir du SDK Android installés, comme indiqué ci-dessous :

[![Liste de thèmes plus](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

Lorsqu’un thème est sélectionné, l’aire de conception est mise à jour pour montrer l’effet du nouveau thème. Notez que cette modification est apportée permanente uniquement si le **OK** clic est effectué dans le **thème** boîte de dialogue. Une fois un thème a été sélectionné, elle sera incluse dans le **thème** déroulante comme indiqué ci-dessous :

[![Le thème clair est désormais disponible](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## <a name="android-version"></a>Version d’Android

Android **Version** sélecteur définit la version Android qui est utilisée pour restituer la mise en page dans le concepteur. Le sélecteur affiche toutes les versions qui sont compatibles avec la version du framework cible du projet :

[![Liste des versions d’Android](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

La version de framework cible peut être définie dans les paramètres du projet sous le **Options du projet > Build > Général** section. Pour plus d’informations sur la version du framework cible, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).

L’ensemble de widgets disponibles dans la boîte à outils est déterminé par la version du framework cible du projet. Cela est également vrai pour les propriétés disponibles dans le **remplissage de la propriété**. La liste des widgets disponibles est *pas* déterminé par la valeur sélectionnée dans le **Version** sélecteur de la barre d’outils. Par exemple, si vous définissez la version cible du projet sur Android 4.4, vous pouvez toujours sélectionner Android 6.0 dans le sélecteur de version de barre d’outils pour voir à quoi ressemble le projet dans Android 6.0, mais vous ne pourrez pas ajouter des widgets qui sont spécifiques à Android 6.0 &ndash;  vous serez toujours limité pour les widgets qui sont disponibles dans Android 4.4.

Pour plus d’informations sur les types de ressources, consultez [ressources Android](~/android/app-fundamentals/resources-in-android/index.md).

-----
