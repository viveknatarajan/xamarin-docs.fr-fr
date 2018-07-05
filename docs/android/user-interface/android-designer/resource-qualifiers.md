---
title: Qualificateurs de ressources et les Options de visualisation
description: Cette rubrique explique comment définir des ressources qui seront utilisées uniquement lorsque certaines valeurs de qualificateur sont mises en correspondance. Un exemple simple est une ressource de chaîne qualifié de langage. Une ressource de chaîne peut être définie comme la valeur par défaut, avec les autres ressources alternatives définis pour être utilisé dans des langues supplémentaires. Tous les types de ressources peuvent être qualifiés, y compris la mise en page lui-même.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 7bc8c1066e557085c1bf34f77765edbb2259ba7a
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403297"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Qualificateurs de ressources et les Options de visualisation

_Cette rubrique explique comment définir des ressources qui seront utilisées uniquement lorsque certaines valeurs de qualificateur sont mises en correspondance. Un exemple simple est une ressource de chaîne qualifié de langage. Une ressource de chaîne peut être définie comme la valeur par défaut, avec les autres ressources alternatives définis pour être utilisé dans des langues supplémentaires. Tous les types de ressources peuvent être qualifiés, y compris la mise en page lui-même._


## <a name="custom-device-configurations"></a>Configurations d’appareils personnalisés

Android est disponible sur une multitude d’appareils et des résolutions d’écran.
Pour aider à concevoir des interfaces utilisateur qui ciblent les nombreux appareils, le concepteur est fourni avec un large éventail de configurations d’appareils intégrés. Il prend également en charge l’ajout de configurations de périphériques supplémentaires ; Ces configurations sont basées sur *qualificateurs* que vous spécifiez pour distinguer une configuration de l’appareil à partir d’un autre. Il existe de nombreux types de qualificateurs. Pour plus d’informations sur ces types de ressources, consultez [ressources Android](~/android/app-fundamentals/resources-in-android/index.md).

En bas du sélecteur de périphérique de menu est un **personnaliser** option comme indiqué ci-dessous :


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menu de sélecteur de périphérique](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Menu de sélecteur de périphérique](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


En sélectionnant **personnaliser** affiche une boîte de dialogue qui vous permettent de naviguer dans les configurations de périphériques disponibles. Lorsque vous cliquez sur le **Device Definitions** onglet, une liste de toutes les définitions d’appareil connu est présentée :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Gestionnaire AVD](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Gestionnaire AVD](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Appareils préconfigurés dans le concepteur ne peut pas être modifiés. Toutefois, vous pouvez cliquer sur **créer un appareil...**  pour définir une définition d’appareil personnalisée. Alternativement, vous pouvez sélectionner une définition existante et cliquez sur **Clone...**  à utiliser comme point de départ pour une nouvelle définition.
Par exemple, si vous sélectionnez le **Nexus 5** définition et en cliquant sur **Clone...**  présente la boîte de dialogue suivante :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Clone Device](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Clone Device](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


Dans la capture d’écran suivante, le nom est remplacé par **Nexus 5 Custom** et les paramètres de périphérique sont modifiés pour créer une nouvelle définition d’appareil personnalisé. Dans cet exemple, **Portrait** est désactivée afin que la définition de l’appareil est Paysage uniquement :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![APPAREIL personnalisé](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![APPAREIL personnalisé](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Lorsque vous cliquez sur **Clone Device**, une nouvelle définition d’appareil est créée et s’affiche dans la liste **Device Definitions** :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Définitions d’appareil mis à jour](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Définitions d’appareil mis à jour](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Notez que chaque définition d’appareil créées par l’utilisateur s’affiche avec une icône verte, comme indiqué ci-dessus. Lorsque vous revenez à la **appareil** menu Sélecteur, la nouvelle définition d’appareil personnalisé est présentée dans la section plus haut de la liste (si vous ne voyez pas votre configuration d’appareil personnalisé dans cette liste, essayez de redémarrer l’IDE) :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![APPAREIL personnalisé s’affiche dans la liste des appareils](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![APPAREIL personnalisé s’affiche dans la liste des appareils](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


En sélectionnant cette configuration d’appareil modifie la disposition pour respecter les personnalisations créées précédemment (dans ce mode ici, paysage uniquement) :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![APPAREIL personnalisé en cours d’utilisation](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![APPAREIL personnalisé en cours d’utilisation](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Options du qualificateur de ressource

**Options du qualificateur de ressource** est accessible en cliquant sur les points de suspension à droite de la **Configuration de l’appareil** options :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Options du qualificateur de ressource](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Options du qualificateur de ressource](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


Cette boîte de dialogue présente les menus déroulants pour les qualificateurs de ressources suivants :

-   **Langage** &ndash; affiche les ressources de langue disponible et offre une option permettant d’ajouter de nouvelles ressources de langue/région.

-   **Mode d’interface utilisateur** &ndash; modes d’affichage de listes (tel que **Dock de voiture** et **Dock de bureau**), ainsi que le sens de disposition.

Chacun de ces menus déroulants s’ouvre de nouvelles boîtes de dialogue dans laquelle vous pouvez sélectionner et configurer les qualificateurs de ressources (comme expliqué ci-dessous).



### <a name="language"></a>Langue

Le **langage** menu déroulant répertorie uniquement les langues qui utilisent des ressources définies (ou **toutes les langues**, qui est la valeur par défaut). Toutefois, il existe également un **Ajouter langue/région...**  option qui vous permet d’ajouter un nouveau langage à la liste :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ajouter une langue/région](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Ajouter une langue/région](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


Lorsque vous cliquez sur **Ajouter langue/région...** , le **sélectionnez une langue** boîte de dialogue s’ouvre et affiche les listes déroulantes de régions et langues disponibles :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Liste des langues](resource-qualifiers-images/vs/10-languages.png "la liste des langues")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Liste des langues](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


Dans cet exemple, nous avons choisi **fr (Français)** pour la langue et **BE** (Belgique) pour le dialecte régionaux Français. Notez que le **région** champ est facultatif, car de nombreux langages peuvent être spécifiés sans tenir compte des régions spécifiques. Lorsque le **langage** menu déroulant est ouvert à nouveau, il affiche la ressource de langue/région qui vient d’être ajouté :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Langue et région choisie](resource-qualifiers-images/vs/11-language-region-added.png "choisi de langue et région")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Langue et région choisie](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Notez que si vous ajoutez un nouveau langage, mais vous ne créez pas de nouvelles ressources pour elle, la langue ajoutée n’utilisera plus affiche la prochaine fois que vous ouvrez le projet.



### <a name="ui-mode"></a>Mode d’interface utilisateur

Lorsque vous cliquez sur le **Mode d’interface utilisateur** menu déroulant, une liste des modes est indiquée, telles que **Normal**, **Dock de voiture**, **Dock de bureau**, **Télévision**, **Appliance**, et **espion**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Menu du Mode d’interface utilisateur](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Sous cette liste sont les modes de nuit **nuit pas** et **nuit**, suivi par les instructions de mise en page **de gauche à droite** et **de droite à gauche** (pour plus d’informations sur **de gauche à droite** et **de droite à gauche** options, consultez [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Les éléments de la dernière dans le **Options du qualificateur de ressource** boîte de dialogue sont les **arrondir les écrans** (pour une utilisation avec Android Wear) ou **pas arrondir écrans** (pour plus d’informations sur round et écrans non arrondi, consultez [dispositions](https://developer.android.com/training/wearables/ui/layouts.html)).
Pour plus d’informations sur les modes d’interface utilisateur Android, consultez [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Menu du Mode d’interface utilisateur](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Sous cette liste sont les modes de nuit **nuit pas** et **nuit**, suivi par les instructions de mise en page **de gauche à droite** et **de droite à gauche**. Pour plus d’informations sur les modes d’interface utilisateur Android, consultez [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Pour plus d’informations sur **de gauche à droite** et **de droite à gauche** options, consultez [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Écran arrondi

Le dernier élément dans le **Options du qualificateur de ressource** boîte de dialogue est la **Round écran** menu. Ce menu vous permet de sélectionner soit **arrondir écrans** (pour une utilisation avec Android Wear) ou **écrans rectangulaires**:

[![Menu de l’écran arrondi](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Paramètres de la barre d’action

Le **paramètres de barre d’Action** icône n’est disponible à gauche de l’icône représentant un pinceau (éditeur de thème) :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Paramètres de la barre d’action](resource-qualifiers-images/vs/14-action-bar.png "les paramètres de barre d’Action")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Paramètres de la barre d’action](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Cette icône s’ouvre un menu de la boîte de dialogue qui fournit un moyen de sélectionner un des trois modes de barre d’Action :

-   **Standard** &ndash; se compose d’un texte logo ou icône et le titre et un sous-titre facultatif.

-   **Liste** &ndash; mode de navigation de liste. Au lieu de texte du titre statique, ce mode présente un menu de liste pour la navigation au sein de l’activité (autrement dit, il peut être présenté à l’utilisateur comme une liste déroulante).

-   **Onglets** &ndash; mode de navigation d’onglet. Au lieu de texte du titre statique, ce mode présente une série d’onglets de navigation au sein de l’activité.



## <a name="themes"></a>Thèmes

Le **thème** menu de liste déroulante affiche tous les thèmes définis dans le projet. En sélectionnant **plus de thèmes** ouvre une boîte de dialogue avec une liste de tous les thèmes disponibles à partir du SDK Android installés, comme indiqué ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Liste de thèmes plus](resource-qualifiers-images/vs/15-theme-menu-sml.png "liste de plus de thèmes")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Liste de thèmes plus](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Lorsqu’un thème est sélectionné, l’aire de conception est mise à jour pour montrer l’effet du nouveau thème. Notez que cette modification est apportée permanente uniquement si le **OK** clic est effectué dans le **thème** boîte de dialogue. Une fois un thème a été sélectionné, elle sera incluse dans le **thème** déroulante comme indiqué ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Le thème clair est désormais disponible](resource-qualifiers-images/vs/16-light-theme.png "le thème clair est désormais disponible")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Le thème clair est désormais disponible](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Version d’Android

Android **Version** sélecteur définit la version Android qui est utilisée pour restituer la mise en page dans le concepteur. Le sélecteur affiche toutes les versions qui sont compatibles avec la version du framework cible du projet :


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Liste des versions Android](resource-qualifiers-images/vs/17-android-version.png "liste Android versions")

La version de framework cible peut être définie dans les paramètres du projet sous **Propriétés > applications > Compiler à l’aide de la version d’Android**. Pour plus d’informations sur la version du framework cible, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).

L’ensemble de widgets disponibles dans la boîte à outils est déterminé par la version du framework cible du projet. Cela est également vrai pour les propriétés disponibles dans le **fenêtre Propriétés**. La liste des widgets disponibles est *pas* déterminé par la valeur sélectionnée dans le **Version** sélecteur de la barre d’outils. Par exemple, si vous définissez la version cible du projet sur Android 4.4, vous pouvez toujours sélectionner Android 6.0 dans le sélecteur de version de barre d’outils pour voir à quoi ressemble le projet dans Android 6.0, mais vous ne pourrez pas ajouter des widgets qui sont spécifiques à Android 6.0 &ndash;  vous serez toujours limité pour les widgets qui sont disponibles dans Android 4.4.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Liste des versions d’Android](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

La version de framework cible peut être définie dans les paramètres du projet sous le **Options du projet > Build > Général** section. Pour plus d’informations sur la version du framework cible, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).

L’ensemble de widgets disponibles dans la boîte à outils est déterminé par la version du framework cible du projet. Cela est également vrai pour les propriétés disponibles dans le **remplissage de la propriété**. La liste des widgets disponibles est *pas* déterminé par la valeur sélectionnée dans le **Version** sélecteur de la barre d’outils. Par exemple, si vous définissez la version cible du projet sur Android 4.4, vous pouvez toujours sélectionner Android 6.0 dans le sélecteur de version de barre d’outils pour voir à quoi ressemble le projet dans Android 6.0, mais vous ne pourrez pas ajouter des widgets qui sont spécifiques à Android 6.0 &ndash;  vous serez toujours limité pour les widgets qui sont disponibles dans Android 4.4.

-----
