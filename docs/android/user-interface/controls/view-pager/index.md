---
title: ViewPager
description: ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation de geste. Navigation geste permet à l’utilisateur balayez gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter la navigation geste avec ViewPager, avec et sans les Fragments. Il explique également comment ajouter des indicateurs de page à l’aide de PagerTitleStrip et PagerTabStrip.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: bb9795eb1e77a48b01556c553ae19613d6ab6de6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115937"
---
# <a name="viewpager"></a>ViewPager

_ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation de geste. Navigation geste permet à l’utilisateur balayez gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter la navigation geste avec ViewPager, avec et sans les Fragments. Il explique également comment ajouter des indicateurs de page à l’aide de PagerTitleStrip et PagerTabStrip._

 
## <a name="overview"></a>Vue d'ensemble

Un scénario courant dans le développement d’applications est la nécessité de fournir aux utilisateurs geste navigation entre les vues de frère. Dans cette approche, l’utilisateur fait glisser sa de gauche ou droite pour accéder aux pages de contenu (par exemple, dans un Assistant d’installation ou un diaporama). Vous pouvez créer ces vues de passage à l’aide de la `ViewPager` widget, disponible dans [v4 de la bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Le `ViewPager` est un widget de disposition constitué de plusieurs vues enfants où chaque vue enfant constitue une page dans la disposition : 

[![Application des captures d’écran de TreePager avec exemple de balayage horizontal](images/01-intro-sml.png)](images/01-intro.png#lightbox)

En règle générale, `ViewPager` est utilisé conjointement avec [Fragments](https://developer.xamarin.com/guides/android/platform_features/fragments/); Toutefois, il existe des situations où vous souhaiterez utiliser `ViewPager` sans la complexité ajoutée des `Fragment`s.

`ViewPager` utilise un modèle de l’adaptateur lui pour fournir les vues à afficher. L’adaptateur utilisé ici est conceptuellement semblable à celle utilisée par [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; vous fournissez une implémentation de `PagerAdapter` pour générer les pages qui le `ViewPager` affiche à l’utilisateur. Les pages affichées par `ViewPager` peut être `View`s ou `Fragment`s. Lorsque `View`s sont affichés, de l’adaptateur sous-classes Android `PagerAdapter` classe de base. Si `Fragment`s sont affichés, de l’adaptateur sous-classes Android `FragmentPagerAdapter`. La bibliothèque de prise en charge Android inclut également `FragmentPagerAdapter` (une sous-classe de `PagerAdapter`) pour faciliter les informations de connexion `Fragment`s aux données. 

Ce guide illustre les deux approches : 

-   Dans [Viewpager avec des vues](~/android/user-interface/controls/view-pager/viewpager-and-views.md), un [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) application est développée pour illustrer comment utiliser `ViewPager` pour afficher les vues d’un catalogue de l’arborescence (une galerie d’images d’arbres à feuilles caduques et persistants). 
    `PagerTabStrip`  et `PagerTitleStrip` servent à afficher les titres de faciliter la navigation entre les pages.

-   Dans [Viewpager avec des Fragments](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), un peu plus complexe [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) application est développée pour illustrer comment utiliser `ViewPager` avec `Fragment`s pour générer une application qui présente des problèmes mathématiques en tant que cartes flash et répond aux entrées d’utilisateur. 


## <a name="requirements"></a>Configuration requise

Pour utiliser `ViewPager` dans votre projet d’application, vous devez installer le [v4 de la bibliothèque de prise en charge Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) package. Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Architecture

Trois composants sont utilisés pour implémenter la navigation du geste avec `ViewPager`:

-   ViewPager
-   Adaptateur
-   Indicateur de radiomessagerie

Chacun de ces composants est résumée ci-dessous.



### <a name="viewpager"></a>ViewPager

`ViewPager` est un gestionnaire de mise en page qui affiche une collection de `View`s une à la fois. Son travail consiste à détecter le mouvement de balayage de l’utilisateur et accédez à la vue suivante ou précédente comme il convient. Par exemple, la capture d’écran ci-dessous montre un `ViewPager` la transition à partir d’une image à l’autre en réponse à un mouvement utilisateur : 

[![Application de gros plan de TreePager affichant une transition entre les vues](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Adaptateur

`ViewPager` extrait ses données à partir d’un *adaptateur*. Travail de l’adaptateur consiste à créer le `View`s affiché par le `ViewPager`, en leur fournissant les en fonction des besoins. Le diagramme ci-dessous illustre ce concept &ndash; l’adaptateur crée et remplit `View`s et les fournit à le `ViewPager`. Comme le `ViewPager` détecte les mouvements de balayage de l’utilisateur, il demande à l’adaptateur pour fournir le texte approprié `View` à afficher : 

[![Diagramme illustrant comment l’adaptateur connecte les noms et les images à le ViewPager](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

Dans cet exemple particulier, chaque `View` est construite à partir d’une image de l’arborescence et un nom de l’arborescence avant d’être passé à la `ViewPager`. 



### <a name="pager-indicator"></a>Indicateur de radiomessagerie

`ViewPager` peut être utilisé pour afficher un jeu de données volumineux (par exemple, une galerie d’images peut contenir des centaines d’images). Pour aider à l’utilisateur de naviguer dans les jeux de données volumineux, `ViewPager` est souvent accompagnée d’un *indicateur de radiomessagerie* qui affiche une chaîne. La chaîne peut être le titre de l’image, une légende ou simplement de la vue actuelle position dans le jeu de données. 

Il existe deux vues qui peuvent produire ces informations de navigation pour vous : `PagerTabStrip` et `PagerTitleStrip.` chacun affiche une chaîne en haut d’un `ViewPager`, et chaque extrait ses données à partir de la `ViewPager`d’adaptateur afin qu’il reste toujours synchronisé avec le affichée `View`. La différence entre eux est que `PagerTabStrip` inclut un indicateur visuel pour la chaîne « actuel » lors de la `PagerTitleStrip` est ne pas (comme indiqué dans ces captures d’écran) : 

[![Captures d’écran de l’application TreePager avec PagerTitleStrip et PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

Ce guide explique comment immplement `ViewPager`, la carte et les composants d’application indicateur et les intégrer pour prendre en charge la navigation de geste. 



## <a name="related-links"></a>Liens associés

- [TreePager (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
