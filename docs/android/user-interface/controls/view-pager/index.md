---
title: ViewPager
description: ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation geste. Navigation geste permet à l’utilisateur pour le passage de gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter la navigation geste avec ViewPager, avec et sans Fragments. Il décrit également comment ajouter des indicateurs de page à l’aide de PagerTitleStrip et PagerTabStrip.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bd687175048bb6a19dde21e66619667511a76796
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769001"
---
# <a name="viewpager"></a>ViewPager

_ViewPager est un gestionnaire de disposition qui vous permet d’implémenter la navigation geste. Navigation geste permet à l’utilisateur pour le passage de gauche et droite pour parcourir les pages de données. Ce guide explique comment implémenter la navigation geste avec ViewPager, avec et sans Fragments. Il décrit également comment ajouter des indicateurs de page à l’aide de PagerTitleStrip et PagerTabStrip._

 
## <a name="overview"></a>Vue d'ensemble

Un scénario courant dans le développement d’applications est nécessaire pour fournir aux utilisateurs geste navigation entre les vues de frère. Dans cette approche, l’utilisateur fait glisser sa de gauche ou droite pour accéder aux pages de contenu (par exemple, dans un Assistant d’installation ou un diaporama). Vous pouvez créer ces vues de passage à l’aide de la `ViewPager` widget, disponible dans [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Le `ViewPager` est un widget de disposition se compose de plusieurs vues enfants où chaque vue enfant constitue une page dans la disposition : 

[![Application des captures d’écran de TreePager avec l’exemple de balayage horizontal](images/01-intro-sml.png)](images/01-intro.png#lightbox)

En règle générale, `ViewPager` est utilisé conjointement avec [Fragments](https://developer.xamarin.com/guides/android/platform_features/fragments/); Toutefois, il existe des situations où vous pouvez souhaiter utiliser `ViewPager` sans la complexité ajouté des `Fragment`s.

`ViewPager` utilise un modèle de carte à lui fournir les affichages à afficher. L’adaptateur utilisé ici est conceptuellement semblable à celle utilisée par [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; vous fournissez une implémentation de `PagerAdapter` pour générer les pages qui le `ViewPager` affiche à l’utilisateur. Les pages affichées par `ViewPager` peut être `View`s ou `Fragment`s. Lorsque `View`s sont affichés, de l’adaptateur sous-classes Android `PagerAdapter` classe de base. Si `Fragment`s sont affichés, de l’adaptateur sous-classes Android `FragmentPagerAdapter`. La bibliothèque de prise en charge Android inclut également `FragmentPagerAdapter` (une sous-classe de `PagerAdapter`) pour aider les détails de la connexion `Fragment`s pour les données. 

Ce guide illustre les deux approches : 

-   Dans [Viewpager avec les vues](~/android/user-interface/controls/view-pager/viewpager-and-views.md), un [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) application est développée pour illustrer l’utilisation de `ViewPager` pour afficher des vues d’un catalogue de l’arborescence (une galerie d’images d’arbres à feuilles caduques et persistants). 
    `PagerTabStrip`  et `PagerTitleStrip` sont utilisés pour afficher les titres qui aident à avec une navigation de page.

-   Dans [Viewpager avec des Fragments](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), légèrement plus complexe [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) application est développée pour illustrer l’utilisation de `ViewPager` avec `Fragment`s pour générer une application qui présente des problèmes mathématiques en tant que cartes flash et répond aux entrées d’utilisateur. 


## <a name="requirements"></a>Spécifications

Pour utiliser `ViewPager` dans votre projet d’application, vous devez installer le [bibliothèque de prise en charge Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) package. Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Architecture

Trois composants sont utilisés pour l’implémentation de navigation geste avec `ViewPager`:

-   ViewPager
-   Adaptateur
-   Indicateur de récepteur de radiomessagerie

Chacun de ces composants est résumée ci-dessous.



### <a name="viewpager"></a>ViewPager

`ViewPager` est un gestionnaire de mise en page qui affiche une collection de `View`s une à la fois. Son rôle consiste à détecter le mouvement de balayage de l’utilisateur et accédez à l’affichage suivant ou précédent. Par exemple, la capture d’écran ci-dessous illustre un `ViewPager` la transition à partir d’une image à l’autre en réponse à un mouvement utilisateur : 

[![Application de gros plan de TreePager afficher une transition entre les vues](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Adaptateur

`ViewPager` tire ses données d’une *carte*. Travail de l’adaptateur consiste à créer le `View`s affiché par le `ViewPager`, leur fournir en fonction des besoins. Le diagramme ci-dessous illustre ce concept &ndash; l’adaptateur crée et remplit `View`s et les fournit à le `ViewPager`. Comme le `ViewPager` détecte les mouvements de balayage de l’utilisateur, le cas échéant, l’adaptateur pour fournir les informations appropriées `View` à afficher : 

[![Diagramme illustrant comment l’adaptateur connecte les noms et les images à le ViewPager](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

Dans cet exemple, chaque `View` est construite à partir d’une image de l’arborescence et d’un nom de l’arborescence avant d’être transmis à la `ViewPager`. 



### <a name="pager-indicator"></a>Indicateur de récepteur de radiomessagerie

`ViewPager` peut être utilisé pour afficher un jeu de données volumineux (par exemple, une galerie d’images peut contenir des centaines d’images). Pour aider l’utilisateur de navigation de grands jeux de données, `ViewPager` est souvent accompagnée un *indicateur de radiomessagerie* qui affiche une chaîne. La chaîne peut être simplement position de la vue actuelle dans le jeu de données, une légende ou le titre de l’image. 

Il existe deux vues qui peuvent produire ces informations de navigation pour vous : `PagerTabStrip` et `PagerTitleStrip.` chacun une chaîne s’affiche en haut d’un `ViewPager`, et chaque extrait ses données à partir de la `ViewPager`d’adaptateur afin qu’il reste toujours synchronisé avec le affichée `View`. La différence est que `PagerTabStrip` inclut un indicateur visuel pour la chaîne « en cours » lors de la `PagerTitleStrip` est ne pas (comme indiqué dans les captures d’écran) : 

[![Captures d’écran de l’application TreePager avec PagerTitleStrip et PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

Ce guide montre comment immplement `ViewPager`, carte et les composants d’application indicateur et les intégrer pour prendre en charge la navigation geste. 



## <a name="related-links"></a>Liens associés

- [TreePager (exemple)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
