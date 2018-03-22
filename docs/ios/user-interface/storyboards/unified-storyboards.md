---
title: "Storyboards unifiées"
description: "Storyboards unifiées autoriser iOS développeur de créer l’interface utilisateur avec un seul storyboard, plutôt que plusieurs tables de montage séquentiel, pour couvrir la plage d’expansion de tailles d’écran de périphérique. Cet article est conçu pour donner une vue d’ensemble plus approfondie du fonctionnement de la table de montage séquentiel unifiée dans Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 60b2e6fa65226631fe2d2c847a56852ac9ae63d2
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="unified-storyboards"></a>Storyboards unifiées

_Storyboards unifiées autoriser iOS développeur de créer l’interface utilisateur avec un seul storyboard, plutôt que plusieurs tables de montage séquentiel, pour couvrir la plage d’expansion de tailles d’écran de périphérique. Cet article est conçu pour donner une vue d’ensemble plus approfondie du fonctionnement de la table de montage séquentiel unifiée dans Xamarin.iOS._

iOS 8 inclut un mécanisme de nouvelle et plus simple à utiliser pour la création de l’interface utilisateur, le plan conceptuel unifié. Avec un seul plan conceptuel pour couvrir toutes les tailles d’écran de matériel différents, rapides et réactives vues peuvent être créés dans un « conception-une fois, utilisez plusieurs « style.

Comme le développeur n’a plus besoin créer une table de montage séquentiel distinct et spécifique pour les appareils iPhone et iPad, ils ont la possibilité de concevoir des applications avec une interface commune, puis personnalisez cette interface pour les classes de taille différente. De cette façon, une application peut être adaptée à la force de chaque facteur de forme, et chaque interface utilisateur peut être paramétrée pour offrir la meilleure expérience.

<a name="size-classes" />

## <a name="size-classes"></a>Classes de taille

Avant d’iOS 8, le développeur a utilisée `UIInterfaceOrientation` et `UIInterfaceIdiom` à faire la différence entre les modes portrait et paysage et entre les appareils iPhone et iPad. Dans iOS8, l’orientation et appareils est déterminée est à l’aide de *taille Classes*.

Les appareils sont définis par les Classes de taille, dans les axes horizontales et verticales, et il existe deux types de classes de taille dans iOS 8 :

-  **Régulière** – il s’agit d’une taille d’écran de grande taille (tels qu’un iPad) ou d’un gadget qui donne l’impression d’une grande taille (tels qu’un `UIScrollView`
-  **Compact** – il s’agit de périphériques de petite taille (par exemple, un iPhone). Cette taille prend en compte l’orientation de l’appareil.


Si les deux concepts sont utilisés ensemble, le résultat est une grille de 2 x 2 qui définit les différentes tailles possibles qui peuvent être utilisés dans les deux les différentes orientations, comme indiqué dans le diagramme suivant :

 [![](unified-storyboards-images/sizeclassgrid.png "Une grille de 2 x 2 qui définit les différentes tailles qui peuvent être utilisés dans les orientations régulières et Compact")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Le développeur peut créer un contrôleur de vue qui utilise un des quatre possibilités qui entraîneraient des dispositions différentes (comme indiqué dans le graphique ci-dessus).

### <a name="ipad-size-classes"></a>Classes de taille d’iPad

IPad, en raison de la taille, a un **régulière** taille pour les deux orientations de classe.

 [![](unified-storyboards-images/image1.png "Classes de taille d’iPad")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone, les Classes de taille

IPhone comporte les classes de taille différente selon l’orientation de l’appareil :

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone, les Classes de taille")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Quand l’appareil est en mode portrait, l’écran a un **compact** classe horizontalement et **régulière** verticalement
-  Quand l’appareil est en mode paysage, les classes de l’écran sont inversés de mode portrait.

### <a name="iphone-6-plus-size-classes"></a>Classes de taille d’iPhone 6 Plus

Les tailles sont les mêmes que les iPhone antérieures en orientation portrait, mais diffère en paysage :

[![](unified-storyboards-images/iphone6sizeclasses.png "Classes de taille d’iPhone 6 Plus")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Étant donné que l’iPhone 6 Plus dispose d’un écran suffisamment grand, il est en mesure d’avoir une classe de taille largeur standard en mode paysage.

### <a name="support-for-a-new-screen-scale"></a>Prise en charge pour une nouvelle échelle de l’écran

IPhone 6 Plus utilise un nouvel affichage rétine HD avec un facteur d’échelle écran de 3.0 (trois fois la résolution d’origine iPhone écran). Pour fournir la meilleure expérience possible sur ces appareils, inclure la nouvelle illustration conçue pour cette échelle de l’écran. Dans Xcode 6 et ultérieur, asset catalogues peuvent inclure des images à 1 x, x 2 et 3 tailles x ; Ajoutez simplement les composants de l’image et iOS choisir les éléments multimédias corrects lors de l’exécution sur un iPhone 6 Plus.

L’image de chargement de comportement dans iOS également reconnaît un `@3x` suffixe dans des fichiers image. Par exemple, si le développeur inclut un composant de l’image (à différentes résolutions) dans l’offre groupée de l’application avec les noms de fichier suivants : `MonkeyIcon.png`, `MonkeyIcon@2x.png`, et `MonkeyIcon@3x.png`. Sur l’iPhone 6 Plus la `MonkeyIcon@3x.png` image sera utilisée automatiquement si le développeur charge une image en utilisant le code suivant :

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Ou si elles assigner l’image à un élément d’interface utilisateur à l’aide de l’e/s concepteur en tant que `MonkeyIcon.png`, le `MonkeyIcon@3x.png` servira, automatiquement, sur l’iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Écrans de démarrage dynamique

Le fichier d’écran de démarrage s’affiche comme écran de démarrage pendant le lance d’une application iOS pour fournir des commentaires à l’utilisateur que l’application est en fait à partir des. Avant d’iOS 8, le développeur devra inclure plusieurs `Default.png` actifs pour chaque résolution de type, l’orientation et écran de périphérique qui s’exécuterait sur l’application de l’image.

Nouveau pour iOS 8, le développeur peut créer un seul, atomique `.xib` fichier dans Xcode qui utilise les Classes de taille et de disposition automatique pour créer un *dynamique d’écran de lancement* qui fonctionne pour chaque périphérique, la résolution et l’orientation. Cela réduit non seulement la quantité de travail du développeur pour créer et gérer toutes les ressources d’image requis, mais elle réduit la taille du groupement de l’application installée.

## <a name="traits"></a>Caractéristiques

Les caractéristiques sont des propriétés qui peuvent être utilisées pour déterminer la façon dont une disposition change à mesure que de ses modifications d’environnement. Il s’agit d’un ensemble de propriétés (la `HorizontalSizeClass` et `VerticalSizeClass` selon `UIUserInterfaceSizeClass`), ainsi que l’idiome interface ( `UIUserInterfaceIdiom`) et l’échelle.

Tous les États ci-dessus sont encapsulés dans un conteneur Apple désigne comme une Collection de caractéristique ( `UITraitCollection`), qui contient non seulement les propriétés, mais leurs valeurs.

## <a name="trait-environment"></a>Environnement de trait

Les environnements de caractéristique sont une nouvelle interface dans iOS 8 et sont en mesure de retourner une Collection de caractéristique pour les objets suivants :

-  Écrans ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Afficher les contrôleurs ( `UIViewController` ).
-  Vues ( `UIView` ).
-  Contrôleur de présentation ( `UIPresentationController` ).


Le développeur utilise le regroupement de caractéristique retourné par un environnement de caractéristique pour déterminer la façon dont une interface utilisateur doit être disposée.

Tous les environnements de caractéristique de créer une hiérarchie comme indiqué dans le diagramme suivant :

 [![](unified-storyboards-images/viewhierarchy.png "Le diagramme de la hiérarchie des environnements de Trait")](unified-storyboards-images/viewhierarchy.png#lightbox)

La Collection de caractéristiques qui ont chacun des environnements caractéristique ci-dessus circule, par défaut, à partir du parent de l’environnement de l’enfant.

En plus de l’obtention de la Collection de Trait en cours, l’environnement caractéristique a un `TraitCollectionDidChange` (méthode), qui peut être substituée dans les sous-classes de la vue ou de vue de contrôleur. Le développeur peut utiliser cette méthode pour modifier un des éléments d’interface utilisateur qui dépendent des caractéristiques lorsque ces caractéristiques ont changé.

## <a name="typical-trait-collections"></a>Collections de caractéristique classique

Cette section décrit les types courants de collections de caractéristique trouvera lorsque vous travaillez avec iOS 8.

Ceci est une Collection de Trait de type que le développeur peut voir sur un iPhone :

|Propriété|Value|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|Normale|
|`UserInterfaceIdom`|Téléphone|
|`DisplayScale`|2.0|

Le jeu ci-dessus représente une Collection de caractéristique pleinement qualifié, car elle a des valeurs pour toutes ses propriétés de trait.

Il est également possible d’avoir une Collection de caractéristique auquel il manque certaines de ses valeurs (c'est-à-dire, Apple en tant que *Unspecified*) :

|Propriété|Value|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|Non spécifié|
|`UserInterfaceIdom`|Non spécifié|
|`DisplayScale`|Non spécifié|

En règle générale, toutefois, lorsque le développeur vous invite à l’environnement de caractéristique pour sa Collection caractéristique, il retourne une collection complète comme indiqué dans l’exemple ci-dessus.

Si un environnement de Trait (par exemple, une vue ou une vue contrôleur) n’est pas à l’intérieur de la hiérarchie en cours, le développeur peut récupérer les valeurs non pour un ou plusieurs des propriétés de trait.

Le développeur obtenez également une Collection de caractéristique partiellement qualifié s’ils utilisent une des méthodes de création fournis par Apple, tels que `UITraitCollection.FromHorizontalSizeClass`, pour créer une nouvelle collection.

Une opération qui peut être effectuée sur plusieurs Collections de caractéristique est en les comparant à d’autres, qui consiste à demander une caractéristique Collection si elle contient un autre. Que signifie *relation contenant-contenu* est que, pour tout paramètre spécifié dans la deuxième collection, la valeur doit correspondre exactement à la valeur de la première collection.

Pour tester l’utilisation de deux traits le `Contains` méthode de le `UITraitCollection` en passant la valeur de la caractéristique à tester.

Le développeur peut effectuer les comparaisons manuellement dans le code pour déterminer comment les différents modes d’affichage ou les contrôleurs de la vue. Toutefois, `UIKit` utilise cette méthode en interne pour fournir certaines de ses fonctionnalités, comme dans le Proxy de l’apparence, par exemple.

## <a name="appearance-proxy"></a>Proxy de l’apparence

Le Proxy de l’apparence a été introduit dans les versions antérieures d’iOS pour permettre aux développeurs de personnaliser les propriétés de leur point de vue. Il a été étendu dans iOS 8 pour prendre en charge les Collections de caractéristique.

Proxys d’apparence incluent désormais une nouvelle méthode `AppearanceForTraitCollection`, qui retourne un nouveau Proxy d’apparence pour la Collection de caractéristique de donnée qui a été passée. Toutes les personnalisations que le développeur exécute sur ce Proxy de l’apparence prendront effet que sur les vues qui sont conformes à la Collection spécifiée de la caractéristique.

En règle générale le développeur passe d’une Collection de caractéristique partiellement spécifié dans le `AppearanceForTraitCollection` (méthode), tel que celui qui vient d’être spécifié une Horizontal taille classe de Compact, afin que leur peut personnaliser une vue de l’application est compacte horizontalement.

## <a name="uiimage"></a>UIImage

Une autre classe Apple a ajouté la Collection caractéristique est `UIImage`. Dans le passé, le développeur devait spécifier un @1X et @2x version de n’importe quel élément de graphique bitmap qui leur destination à inclure dans l’application (par exemple, une icône).

iOS 8 a été étendue pour permettre au développeur d’inclure plusieurs versions d’une image dans un catalogue de l’Image selon une Collection de caractéristique. Par exemple, le développeur peut inclure une image plus petite pour travailler avec une classe de caractéristiques Compact et une image de taille complète de toute autre collection.

Lorsque l’une des images est utilisée à l’intérieur d’un `UIImageView` classe, l’affichage de l’Image s’affiche automatiquement la version correcte de l’image pour sa Collection caractéristique. Si l’environnement caractéristique change (par exemple, le changement d’utilisateur l’appareil du mode portrait en mode paysage), l’affichage de l’Image sélectionne automatiquement la nouvelle taille d’image pour correspondre à la nouvelle Collection caractéristique et modifier sa taille pour correspondre à celui de la version actuelle de l’image en cours affiché.

## <a name="uiimageasset"></a>UIImageAsset

Apple a ajouté une nouvelle classe à iOS 8 appelée `UIImageAsset` afin de donner au développeur un contrôle accru sur la sélection de l’image.

Une ressource Image conclusion de toutes les différentes versions d’une image et permet au développeur de demander une image spécifique qui correspond à une Collection de caractéristiques qui ont été passés. Les images peuvent être ajoutés ou supprimés à partir d’un composant de l’Image, à la volée.

Pour plus d’informations sur les ressources d’Image, consultez Apple [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) documentation.

## <a name="combining-trait-collections"></a>Combiner des Collections de Trait

Une autre fonction qu’un développeur peut effectuer sur des Collections de caractéristique consiste à additionner deux qui va entraîner la collection combinée, où les valeurs non spécifiées d’une collection sont remplacés par les valeurs spécifiées dans une seconde. Cette opération est effectuée à l’aide de la `FromTraitsFromCollections` méthode de la `UITraitCollection` classe.

Comme indiqué précédemment, si une des caractéristiques n’est pas spécifié dans une des Collections caractéristique et est spécifié dans une autre, la valeur est définie pour la version spécifiée. Toutefois, s’il existe plusieurs versions d’une valeur donnée spécifiée, la valeur de la dernière caractéristique Collection sera la valeur qui est utilisée.

## <a name="adaptive-view-controllers"></a>Affichage Adaptive des contrôleurs

Cette section décrit les détails de la façon dont iOS vue et affichage des contrôleurs ont adopté les concepts des caractéristiques et des Classes de taille automatiquement être plus adaptable dans les applications de développeur.

### <a name="split-view-controller"></a>Contrôleur d’affichage fractionné

Une des classes de contrôleur d’affichage qui a été modifié le plus dans iOS 8 est la `UISplitViewController` classe. Dans le passé, le développeur utilise souvent un contrôleur d’affichage fractionné sur la version iPad de l’application, et ils doivent ensuite fournir une version complètement différente de leur hiérarchie de la vue pour la version de l’iPhone de l’application.

Dans iOS 8, la `UISplitViewController` classe n’est disponible sur les deux plateformes (iPad et iPhone), ce qui permet au développeur de créer une hiérarchie de contrôleur d’affichage qui fonctionnera pour iPhone et iPad.

Lorsqu’un iPhone est Paysage, le contrôleur d’affichage fractionné présente ses vues côte-à-côte, comme il le ferait lors de l’affichage sur un iPad.

### <a name="overriding-traits"></a>Caractéristiques de substitution

Environnements de trait en cascade à partir du conteneur parent sur les conteneurs enfants, comme dans le graphique suivant affichant un contrôleur d’affichage fractionné sur un iPad en paysage :

 [![](unified-storyboards-images/cascadingclasses01.png "Un contrôleur de vue de division sur un iPad en paysage")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Étant donné que l’iPad a une classe de taille normale dans les orientations horizontales et verticales, le mode Fractionné affiche affichages principale et de détails.

Sur un iPhone, où la classe de taille est compacte dans les deux orientations, le contrôleur d’affichage fractionné affiche uniquement la vue détail, comme indiqué ci-dessous :

 [![](unified-storyboards-images/cascadingclasses02.png "Le contrôleur d’affichage fractionné affiche uniquement l’affichage des détails")](unified-storyboards-images/cascadingclasses02.png#lightbox)

Dans une application où le développeur souhaite l’affichage maître et détail sur un iPhone l’orientation paysage, le développeur doit insérer un conteneur parent pour le contrôleur d’affichage fractionné et remplacer la Collection de caractéristique. Comme indiqué dans le graphique ci-dessous :

 [![](unified-storyboards-images/cascadingclasses03.png "Le développeur doit insérer un conteneur parent pour le contrôleur d’affichage fractionné et remplacer la Collection de Trait")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A `UIView` est défini comme parent du contrôleur de mode de fractionnement et la `SetOverrideTraitCollection` méthode est appelée sur la vue en passant une nouvelle Collection caractéristique et ciblant le contrôleur d’affichage fractionné. La nouvelle Collection caractéristique remplace le `HorizontalSizeClass`, la valeur `Regular`, de sorte que le contrôleur d’affichage fractionné affiche le masque et le détail des vues sur un iPhone dans l’orientation paysage.

Notez que la `VerticalSizeClass` a été définie sur `unspecified`, ce qui permet à la nouvelle Collection caractéristique à ajouter à la Collection de Trait sur la page parente, ce qui entraîne un `Compact VerticalSizeClass` pour l’enfant Split-View-Controller.

### <a name="trait-changes"></a>Modifications de trait

Cette section est un œil, en détail, comment les Collections de caractéristique de transition lorsque l’environnement de la caractéristique est modifiée. Par exemple, lorsque le périphérique pivote de portrait à paysage.

 [![](unified-storyboards-images/traittransitions01.png "Le mode portrait en vue d’ensemble de modifications de caractéristique de paysage")](unified-storyboards-images/traittransitions01.png#lightbox)

Tout d’abord, iOS 8 effectue certaines le programme d’installation pour préparer la transition. Ensuite, le système s’anime à l’état de transition. Enfin, iOS 8 Nettoie tout état temporaire requis pendant la transition.

iOS 8 fournit plusieurs rappels que le développeur peut utiliser pour participer à la modification de la caractéristique comme indiqué dans le tableau suivant :

|Phase|Rappel|Description|
|--- |--- |--- |
|Installation|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Cette méthode est appelée au début d’une modification de la caractéristique avant une Collection de la caractéristique est définie à sa nouvelle valeur.</li><li>La méthode est appelée lorsque la valeur de la Collection de caractéristiques a été modifiée, mais avant toute animation a lieu.</li></ul>|
|Animation|`WillTransitionToTraitCollection`|Le coordinateur de Transition qui est passé à cette méthode a une `AnimateAlongside` propriété qui permet au développeur d’ajouter des animations qui seront exécutées en même temps que les animations de la valeur par défaut.|
|Nettoyage|`WillTransitionToTraitCollection`|Fournit une méthode permettant aux développeurs d’inclure leur propre code de nettoyage après que la transition a lieu.|

Le `WillTransitionToTraitCollection` méthode est idéale pour animer les contrôleurs de la vue, ainsi que les modifications de la Collection de caractéristique. Le `WillTransitionToTraitCollection` méthode est uniquement disponible sur les contrôleurs de vue ( `UIViewController`) et non sur les autres environnements caractéristique, comme `UIViews`.

Le `TraitCollectionDidChange` est très utile pour travailler avec les `UIView` (classe), où le développeur souhaite mettre à jour l’interface utilisateur qu’une modifiant des caractéristiques.

### <a name="collapsing-the-split-view-controllers"></a>Réduction de l’affichage des contrôleurs de fractionnement

Maintenant nous allons prendre étudier ce qui se produit lorsqu’un contrôleur de vue Split réduit à partir d’une colonne à une vue d’une colonne. Dans le cadre de cette modification, il existe deux processus qui doivent se produire :

-  Par défaut, le contrôleur d’affichage fractionné utilisera le contrôleur d’affichage principal en tant que la vue après que la réduction se produit. Le développeur peut substituer ce comportement en substituant la `GetPrimaryViewControllerForCollapsingSplitViewController` méthode de la `UISplitViewControllerDelegate` et de fournir n’importe quel contrôleur de vue qu’ils souhaitent afficher dans l’état réduit.
-  Le contrôleur secondaire de la vue doit obtenir fusionnées dans le contrôleur principal de la vue. En règle générale le développeur ne devrez pas aucune action pour cette étape. le contrôleur d’affichage fractionné inclut la gestion automatique de cette phase basée sur le périphérique matériel. Toutefois, il peut être certains cas que le développeur d’interagir avec cette modification. Appel de la `CollapseSecondViewController` méthode de la `UISplitViewControllerDelegate` permet au contrôleur de la vue principale à afficher lorsque la réduction se produit, au lieu de l’affichage des détails.


### <a name="expanding-the-split-view-controller"></a>Développer le contrôleur d’affichage fractionné

Maintenant nous allons prendre étudier ce qui se produit lorsqu’un contrôleur de vue de fractionnement est développé à partir d’un état réduit. Une fois encore, il existe des deux étapes qui doivent se produire :

-  Tout d’abord définir le nouveau contrôleur de mode principal. Par défaut, le contrôleur d’affichage fractionné utilisent automatiquement le contrôleur principal de la vue à partir de l’affichage réduit. Là encore, le développeur peut substituer ce comportement en utilisant le `GetPrimaryViewControllerForExpandingSplitViewController` méthode de la `UISplitViewControllerDelegate` .
-  Une fois que le contrôleur principal de la vue a été choisi, le contrôleur secondaire de la vue doivent être recréé. Là encore, le contrôleur d’affichage fractionné inclut la gestion automatique de cette phase basée sur le périphérique matériel. Le développeur peut substituer ce comportement en appelant le `SeparateSecondaryViewController` méthode de la `UISplitViewControllerDelegate` .


Dans un contrôleur de mode fractionné, le contrôleur principal de la vue joue un rôle dans le développement et réduction des vues en implémentant la `CollapseSecondViewController` et `SeparateSecondaryViewController` méthodes de la `UISplitViewControllerDelegate`. `UINavigationController` implémente ces méthodes pour automatiquement push et pop, le contrôleur de la vue secondaire.

### <a name="showing-view-controllers"></a>Affichage montrant des contrôleurs

Un autre changement d’Apple pour iOS 8 est la manière que le développeur montre les contrôleurs de la vue. Dans le passé, si l’application devait un contrôleur de vue de feuille (par exemple, une Table View Controller) et que le développeur a donné lieu à une autre (par exemple, en réponse à l’en appuyant sur utilisateur sur une cellule), susceptibles d’atteindre l’application par le biais de la hiérarchie de contrôleur pour le Navigation-View-Controller et appelez le `PushViewController` méthode par rapport à elle pour afficher la nouvelle vue.

Cela affiche un couplage très étroit entre le contrôleur de Navigation et de l’environnement dans lequel il a été exécuté dans. Dans iOS 8, Apple a dissocié cela en fournissant deux nouvelles méthodes :

-  `ShowViewController` – S’adapte pour afficher le nouveau contrôleur de vue en fonction de son environnement. Par exemple, dans un `UINavigationController` il transmet simplement la nouvelle vue dans la pile. Dans un contrôleur de mode fractionné, le nouveau contrôleur de vue s’affiche sur le côté gauche en tant que le nouveau contrôleur de mode principal. Si aucun contrôleur de vue du conteneur n’est présent, la nouvelle vue s’affichera comme un contrôleur d’affichage Modal.
-  `ShowDetailViewController` – Fonctionne de façon similaire à `ShowViewController`, mais est implémenté sur un contrôleur de vue de fractionnement pour remplacer l’affichage des détails avec le nouveau contrôleur de vue en cours de passage. Si le contrôleur d’affichage fractionné est réduit (comme peut se produire dans un Application iPhone), l’appel sera redirigé vers le `ShowViewController` (méthode) et la nouvelle vue seront affichera en tant que le contrôleur principal de la vue. Là encore, si aucun contrôleur de vue du conteneur n’est présent, la nouvelle vue s’affichera comme un contrôleur d’affichage Modal.


Ces méthodes fonctionnent en démarrant à la feuille View Controller et remonter dans la hiérarchie jusqu'à ce que le contrôleur de vue de conteneur approprié pour gérer l’affichage de la nouvelle vue.

Les développeurs peuvent implémenter `ShowViewController` et `ShowDetailViewController` dans leurs propres contrôleurs d’affichage personnalisé pour obtenir le même automatisée des fonctionnalités qui `UINavigationController` et `UISplitViewController` fournit.

### <a name="how-it-works"></a>Son fonctionnement

Dans cette section, nous allons examiner comment ces méthodes sont réellement implémentées dans iOS 8. Première Examinons le nouveau `GetTargetForAction` méthode :

 [![](unified-storyboards-images/gettargetforaction.png "La nouvelle méthode GetTargetForAction")](unified-storyboards-images/gettargetforaction.png#lightbox)

Cette méthode parcourt la chaîne de la hiérarchie jusqu'à ce que le contrôleur de la vue conteneur correct est trouvé. Exemple :

1.  Si un `ShowViewController` est appelée, le premier contrôleur de vue dans la chaîne qui implémente cette méthode est le contrôleur de Navigation, il est utilisé en tant que le parent de la nouvelle vue.
1.  Si un `ShowDetailViewController` méthode a été appelée à la place, le contrôleur d’affichage fractionné est le premier contrôleur de vue pour l’implémenter, afin qu’il est utilisé en tant que parent.


Le `GetTargetForAction` méthode fonctionne par localisation d’un contrôleur de vue qui implémente une Action donnée et puis demander ce contrôleur de vue s’il souhaite recevoir cette action. Étant donné que cette méthode est publique, les développeurs peuvent créer leurs propres méthodes personnalisées qui fonctionnent comme intégrée dans `ShowViewController` et `ShowDetailViewController` méthodes.

## <a name="adaptive-presentation"></a>Présentation adaptative

IOS 8, Apple a effectué des présentations Popover ( `UIPopoverPresentationController`) adaptive également. Ainsi, un contrôleur d’affichage de présentation Popover automatiquement présente une vue Popover normale dans une classe de taille normale, mais il s’affiche en plein écran dans une classe de taille horizontalement compact (tel que sur un iPhone).

Pour prendre en compte les modifications dans le système d’animation unifiée, un nouvel objet de contrôleur a été créé pour gérer les contrôleurs de vue présenté : `UIPresentationController`. Ce contrôleur est créé à partir du moment que le contrôleur de la vue est présenté jusqu'à ce qu’il a été abandonnée. Comme c’est une classe de gestion, il peut être considéré une superclasse sur le contrôleur de la vue lorsqu’elle répond aux modifications de périphérique qui affectent le contrôleur de vue (par exemple, l’orientation) qui sont ensuite le répercutés dans le contrôleur d’afficher le contrôleur de présentation contrôle.

Lorsque le développeur présente une vue contrôleur à l’aide de la `PresentViewController` (méthode), la gestion du processus de présentation est transmise à `UIKit`. UIKit gère (entre autres), le contrôleur approprié pour le style en cours de création, à la seule exception quand un contrôleur de vue a le style `UIModalPresentationCustom`. Ici, l’application peut fournir qu’il est propre PresentationController plutôt qu’à l’aide de la `UIKit` contrôleur.

### <a name="custom-presentation-styles"></a>Styles de présentation personnalisé

Avec un style de présentation personnalisée, les développeurs ont la possibilité d’utiliser un contrôleur de présentation personnalisé. Ce contrôleur personnalisé peut être utilisé pour modifier l’apparence et le comportement de la vue, il est des connexes à.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Utilisation des Classes de taille

Le projet Xamarin Photos adaptatif qui est inclus dans cet article donne un exemple de l’utilisation de Classes de taille et de Adaptive contrôleurs d’affichage dans une application d’Interface d’unifiée iOS 8.

Pendant que l’application crée complètement son interface utilisateur à partir du code, au lieu d’utiliser le concepteur IOS et création d’un plan conceptuel unifiée, les mêmes techniques s’appliquent. Plus loin dans cet article, nous allons montrer comment utiliser les Classes de taille avec un plan conceptuel unifié et le concepteur dans une Application Xamarin iOS.

Maintenant comprendre comment le projet de Photos adaptatif est mise en œuvre plusieurs des fonctionnalités de la classe de taille dans iOS 8 pour créer une Application Adaptive plus approfondie.

### <a name="adapting-to-trait-environment-changes"></a>Adaptation aux modifications d’environnement de Trait

Lorsque vous exécutez l’application Photos adaptative sur un iPhone, lorsque l’utilisateur fait pivoter l’appareil de portrait à paysage, le contrôleur d’affichage fractionné affiche le masque et les détails de l’affichage :

 [![](unified-storyboards-images/rotation.png "Le contrôleur d’affichage fractionné affiche à la fois le masque et afficher les détails comme illustré ici")](unified-storyboards-images/rotation.png#lightbox)

Cela est effectué en substituant le `UpdateConstraintsForTraitCollection` méthode de contrôleur de la vue et en ajustant les contraintes en se basant sur la valeur de la `VerticalSizeClass`. Exemple :

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Ajout d’Animations de Transition

Lorsque le contrôleur de vue de fractionnement dans les Photos Adaptive application passe à partir de réduit de développé, des animations sont ajoutées aux animations par défaut en remplaçant le `WillTransitionToTraitCollection` méthode du contrôleur d’affichage. Exemple :

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Substitution de l’environnement de Trait

Comme indiqué ci-dessus, l’application de Photos Adaptive force le contrôleur de mode de fractionnement pour afficher les détails et les vues maîtres quand l’appareil de l’iPhone est en mode paysage.

Cela a été accompli en utilisant le code suivant dans le contrôleur d’affichage :

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Développer et réduire le contrôleur d’affichage fractionné

Suivant nous allons examiner comment le comportement en mode de réduction et de l’extension du fractionnement vue contrôleur a été implémenté dans Xamarin. Dans la `AppDelegate`, lorsque le contrôleur de mode de fractionnement est créé, son délégué est assigné à gérer ces modifications :

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

Le `SeparateSecondaryViewController` méthode vérifie si une photo est affichée et prend des mesures en fonction de cet état. Si aucune photo n’est affiché il réduit le contrôleur secondaire de la vue afin que le contrôleur de la vue principale s’affiche.

Le `CollapseSecondViewController` méthode est utilisée lors de l’expansion le contrôleur d’affichage fractionné pour voir si des photos existent sur la pile, si par conséquent, il est réduit à cette vue.

### <a name="moving-between-view-controllers"></a>Déplacement entre les contrôleurs de vue

Ensuite, jetons un œil sur la façon dont l’application Photos ADAPTATIF se déplace entre les contrôleurs de la vue. Dans le `AAPLConversationViewController` classe lorsque l’utilisateur sélectionne une cellule de la table, la `ShowDetailViewController` méthode est appelée pour afficher la vue Détails :

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Affichage des indicateurs de divulgation d’informations

Dans l’application Photo Adaptive, il existe plusieurs endroits où les indicateurs de divulgation d’informations sont affichés ou masqués sur les modifications apportées à l’environnement de caractéristique. Ceci est géré par le code suivant :

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Celles-ci sont implémentées à l’aide de la `GetTargetViewControllerForAction` méthode présentée en détail ci-dessus.

Quand un contrôleur de la vue Table affiche des données, il utilise les méthodes implémentées ci-dessus pour afficher ou non un push va se produire et ou non afficher ou masquer l’indicateur de divulgation d’informations en conséquence :

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Nouvelle `ShowDetailTargetDidChangeNotification` Type

Apple a ajouté un nouveau type de notification pour l’utilisation des Classes de taille et de la caractéristique des environnements à partir d’un contrôleur de vue de fractionnement, `ShowDetailTargetDidChangeNotification`. Cette notification est envoyée à chaque modification de l’affichage des détails pour un contrôleur d’affichage fractionné cible, par exemple lorsque le contrôleur se développe ou réduit.

L’application de Photos adaptative utilise cette notification pour mettre à jour l’état de l’indicateur de divulgation d’informations lorsque le contrôleur de la vue Détails change :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Présentation détaillée de l’application de Photos adaptative pour voir toutes les méthodes qui taille des Classes, caractéristique Collections et contrôleurs adaptative de la vue peuvent utilisées pour créer facilement une Application unifiée dans Xamarin.iOS.

## <a name="unified-storyboards"></a>Storyboards unifiées

Nouveau pour iOS 8, unifiée des animations permettent au développeur de créer un, unifiée qui peut être affiché sur les appareils iPhone et iPad en ciblant plusieurs Classes de taille de fichier d’animation. À l’aide d’unifiée des animations, le développeur écrit moins code spécifique de l’interface utilisateur et a la conception d’une seule interface pour créer et gérer.

Les principaux avantages d’unifiée des animations sont :

-  Utiliser le même fichier de table de montage séquentiel pour iPhone et iPad.
-  Déployer vers l’arrière pour iOS 6 et iOS 7.
-  L’aperçu pour différents appareils, les orientations et les versions de système d’exploitation à partir de dans le Concepteur de Xamarin iOS.

Cette fonctionnalité est entièrement prise en charge dans Visual Studio pour Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>L’activation des Classes de taille

Par défaut, tous les nouveaux projets Xamarin.iOS utilise les classes de taille. Pour utiliser les Classes de taille et est parfait pour adaptative à l’intérieur d’un plan conceptuel à partir d’un projet antérieur, il doit d’abord être converti au format de plan conceptuel Xcode 6 unifiée à partir de dans le concepteur iOS.

Pour faire cela ouvrir le plan conceptuel à convertir dans iOS concepteur et vérifiez la **utiliser les Classes de taille** case à cocher :

 [![](unified-storyboards-images/sizeclass01.png "La case à cocher Utiliser les Classes de taille")](unified-storyboards-images/sizeclass01.png#lightbox)

Le concepteur iOS confirme que le développeur souhaite convertir le format de la table de montage séquentiel pour utiliser les Classes de taille :

 [![](unified-storyboards-images/sizeclass02.png "L’alerte de Classes de taille de l’utilisation")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Mise en page automatique doit également être activée pour les Classes de taille fonctionner correctement.

### <a name="generic-device-types"></a>Types de périphériques génériques

Une fois l’animation a été convertie pour utiliser les Classes de taille, il s’affiche de nouveau dans l’aire de conception et le **en tant que vue** appareil sera générique :

 [![](unified-storyboards-images/sizeclass03.png "Afficher sous la forme d’un type de périphérique générique")](unified-storyboards-images/sizeclass03.png#lightbox)

Lorsque le type de périphérique générique est sélectionné, tous les contrôleurs de la vue doit être redimensionnées carré 600 x 600. Ce carré représente les tailles de n’importe quel largeur et de n’importe quelle hauteur. Lorsque le fichier iOS concepteur est dans ce mode, toutes les modifications s’appliqueront à toutes les Classes de taille.

Le développeur a également l’option d’affichage de l’aire de conception en tant qu’un iPhone :

 [![](unified-storyboards-images/sizeclass04.png "Affichage de l’aire de conception en tant qu’un iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Ou visualiser en tant qu’un iPad :

 [![](unified-storyboards-images/sizeclass05.png "Affichage de l’aire de conception en tant qu’un iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Sélectionnez une classe de taille

Le bouton de sélecteur de classe de taille est au niveau de l’angle supérieur gauche de l’aire de conception (à côté de la liste déroulante en tant que vue). Il permet au développeur de sélectionner les Classes de taille sont en cours de modification :

 [![](unified-storyboards-images/sizeclass06.png "Sélectionnez une classe de taille")](unified-storyboards-images/sizeclass06.png#lightbox)

Le sélecteur présente la sélection de la classe de taille dans une grille de 3 x 3. Chacun des carrés dans la grille représente une combinaison d’une classe de largeur et une hauteur de classe. Le carré center sélectionne la classe de n’importe quelle taille de hauteur Width/Any (qui est la vue par défaut pour un plan conceptuel unifiée). Lorsque ce carré est sélectionnée, le développeur modifie la disposition par défaut, qui est héritée par toutes les autres configurations.

Le carré dans le coin supérieur gauche de la grille représente la classe de taille de hauteur largeur/Compact Compact :

 [![](unified-storyboards-images/sizeclass07.png "La classe de taille de hauteur largeur/Compact Compact")](unified-storyboards-images/sizeclass07.png#lightbox)

Ce mode correspond à un iPhone dans l’orientation paysage. Le carré dans le coin inférieur droit de la grille représente la classe de taille du hauteur de largeur/Regular régulière, qui représente un iPad :

 [![](unified-storyboards-images/sizeclass08.png "La classe de taille de hauteur régulière largeur/normal")](unified-storyboards-images/sizeclass08.png#lightbox)

Pour modifier la disposition d’un iPhone dans l’orientation portrait, sélectionnez le carré dans le coin inférieur gauche. Représente la classe de taille de hauteur largeur/Regular Compact :

 [![](unified-storyboards-images/sizeclass09.png "La classe de taille de hauteur largeur/Regular Compact")](unified-storyboards-images/sizeclass09.png#lightbox)

Cliquez dans le carré pour le sélectionner et l’aire de conception modifie la taille de l’affichage des contrôleurs pour correspondre à la nouvelle sélection :

 [![](unified-storyboards-images/sizeclass10.png "L’aire de conception modifie la taille de l’affichage des contrôleurs pour correspondre à la nouvelle sélection comme indiqué")](unified-storyboards-images/sizeclass10.png#lightbox)

Consultez la section de la classe de taille de cet article pour plus d’informations sur les Classes de taille et comment ils affectent la disposition pour iPhone et iPad.

### <a name="adaptive-segue-types"></a>Types de Segue adaptative

Si le développeur a utilisé des plans conceptuels avant, ils seront familiarisés avec les types de segue existants de **Push**, **modale** et **Popover**. Lorsque les Classes de taille sont activées sur un fichier de table de montage séquentiel unifiée, Adaptive Segue suivants (qui correspondent à la nouvelle API de contrôleur vue décrits ci-dessus) sont accessibles : **afficher** et **afficher les détails** .

> [!IMPORTANT]
> Lorsque les Classes de taille sont activées, existant est parfait pour va être converti pour les nouveaux types.

Prenons l’exemple d’une e/s 8 Application qui utilise un Storyboard unifiée avec un contrôleur de vue de fractionnement qui dispose d’un menu de navigation du jeu simple dans la vue principale. Si l’utilisateur clique sur un bouton de menu, contrôleur de la vue de l’élément sélectionné doit figurer dans la section Détails du fractionnement vue contrôleur lors de l’exécution sur un iPad. Sur un iPhone contrôleur d’affichage de l’élément doit être placé sur la pile de Navigation.

Pour parvenir à cet effet, dans le concepteur iOS contrôle-cliquez sur le bouton, puis faites glisser une ligne pour le contrôleur de la vue à afficher. Lorsque le bouton de la souris est relâché, sélectionnez `Show Detail` dans le menu contextuel de Type Segue :

 [![](unified-storyboards-images/segue01.png "Sélectionnez Afficher les détails dans le menu contextuel de Type Segue")](unified-storyboards-images/segue01.png#lightbox)

La nouvelle segue sera créé entre le bouton et le contrôleur de la vue. Maintenant exécuter l’application dans le simulateur iPhone et le Menu principal s’affichera :

 [![](unified-storyboards-images/segue02.png "Le Menu principal")](unified-storyboards-images/segue02.png#lightbox)

Cliquez sur le **sélectionner le jeu** seront poussée de bouton et le contrôleur de la vue de l’élément dans la pile de Navigation :

 [![](unified-storyboards-images/segue03.png "Les éléments View Controller repoussés de la pile de Navigation comme")](unified-storyboards-images/segue03.png#lightbox)

Arrêter le simulateur iPhone et exécuter l’Application dans le simulateur iPad. Basculez vers l’orientation paysage et principal menu s’affiche à nouveau :

 [![](unified-storyboards-images/segue04.png "Le menu principal s’affiché")](unified-storyboards-images/segue04.png#lightbox)

Là encore, cliquez sur le **sélectionner le jeu** bouton et-View-Controller l’article est indiqué dans la section des détails du contrôleur de mode de fractionnement :

 [![](unified-storyboards-images/segue05.png "Les éléments de vue contrôleur indiqué dans la section des détails du contrôleur de mode fractionné")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>À l’exclusion d’un élément d’une classe de taille

Il est parfois lorsqu’un élément donné (par exemple, une vue, contrôle ou une contrainte) n’est pas nécessaire à l’intérieur d’une classe de taille spécifique. Pour exclure un élément d’une classe de taille, sélectionnez l’élément désiré à exclure de la **aire de conception**. Faites défiler vers le bas de la **Explorateur de propriétés** et cliquez sur le **ENGRENAGE** menu déroulant. Sélectionnez la combinaison de **largeur** et **hauteur** pour exclure l’élément à partir de :

[![](unified-storyboards-images/exclude-a.png "Sélectionnez la combinaison de la largeur et la hauteur")](unified-storyboards-images/exclude-a.png#lightbox)

Un nouveau *cas d’Exclusion* sera ajouté à l’élément dans la partie inférieure de la **Explorateur de propriétés**. Ensuite, désactivez le **installé** case à cocher pour la classe de taille donnée :

[![](unified-storyboards-images/exclude-b.png "Désactivez la case à cocher installé")](unified-storyboards-images/exclude-b.png#lightbox)

Commutateur de l’aire de conception à la largeur et la hauteur de l’élément a été exclu, il a été supprimé de la classe de taille donnée, mais pas la conception de l’interface utilisateur entière :

 [![](unified-storyboards-images/exclude02.png "Basculer l’aire de conception pour la largeur et la hauteur de l’élément a été exclu")](unified-storyboards-images/exclude02.png#lightbox)

Revenir à la classe de taille Any Width/Any hauteur et de l’élément est toujours en place :

 [![](unified-storyboards-images/exclude03.png "Revenir à la classe de taille Any Width/Any hauteur")](unified-storyboards-images/exclude03.png#lightbox)

Lorsque l’application est exécutée dans le simulateur iPad, l’élément s’affiche :

 [![](unified-storyboards-images/exclude04.png "L’élément à afficher lorsque l’application en cours d’exécution dans le simulateur iPad")](unified-storyboards-images/exclude04.png#lightbox)

Et lorsque l’application est exécutée sur le simulateur iPhone, l’élément est manquant :

 [![](unified-storyboards-images/exclude05.png "L’élément manquant quand l’application en cours d’exécution dans le simulateur iPhone")](unified-storyboards-images/exclude05.png#lightbox)

Pour supprimer un cas d’Exclusion d’un élément, il suffit de sélectionner l’élément dans le **aire de conception**, faites défiler vers le bas de la **Explorateur de propriétés** et cliquez sur le  **-** bouton en regard de l’incident à supprimer.

Pour voir une implémentation d’unifiée des animations, examinez le `UnifiedStoryboard` exemple d’application d’iOS 8 Xamarin jointe à ce document.

## <a name="dynamic-launch-screens"></a>Écrans de démarrage dynamique

Le fichier d’écran de démarrage s’affiche comme écran de démarrage pendant le lance d’une application iOS pour fournir des commentaires à l’utilisateur que l’application est en fait à partir des. Avant d’iOS 8, le développeur devra inclure plusieurs `Default.png` actifs pour chaque résolution de type, l’orientation et écran de périphérique qui s’exécuterait sur l’application de l’image. Par exemple, `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`, etc.

En tenant compte de la nouvelle iPhone 6 et iPhone 6 Plus périphériques (et l’Apple Watch à venir) avec tous les appareils iPad et iPhone existant, cela représente un grand tableau de tailles différentes, les orientations et les résolutions de `Default.png` des ressources d’image d’écran démarrage qui doivent Création et la maintenance. En outre, ces fichiers peuvent être assez volumineux et seront » et Active » l’application de l’application livrable, augmenter la quantité de temps nécessaire pour télécharger l’application à partir de l’iTunes App Store (éventuellement maintenant en mesure d’être transmises sur un réseau cellulaire) et en augmentant la quantité de stockage requis sur l’appareil de l’utilisateur final.

Nouveau pour iOS 8, le développeur peut créer un seul, atomique `.xib` fichier dans Xcode qui utilise les Classes de taille et de disposition automatique pour créer un *dynamique d’écran de lancement* qui fonctionne pour chaque périphérique, la résolution et l’orientation. Cela réduit non seulement la quantité de travail du développeur pour créer et gérer toutes les ressources d’image requis, mais elle réduit considérablement la taille du groupement de l’application installée.


Écrans de lancement dynamique présentent les considérations et limitations suivantes :

 - Utilisez uniquement `UIKit` classes.
 - Utiliser un affichage d’une racine unique qui est un `UIView` ou `UIViewController` objet.
 - Ne faites pas toutes les connexions au code de l’application (ne pas ajouter de **Actions** ou **prises**).
 - N’ajoutez pas `UIWebView` objets.
 - N’utilisez pas les classes personnalisées.
 - N’utilisez pas les attributs de runtime.

Avec les instructions ci-dessus à l’esprit, examinons Ajout d’un écran de lancement dynamique à un projet d’iOS 8 Xamarin existant.

Effectuez ce qui suit :

1. Ouvrez **Visual Studio pour Mac** et charger la **Solution** pour ajouter l’écran de lancement dynamique pour.
2. Dans le **l’Explorateur de solutions**, avec le bouton droit le `MainStoryboard.storyboard` fichier et sélectionnez **ouvrir avec** > **Xcode Interface Builder**:

    [![](unified-storyboards-images/dls01.png "Ouvrir avec le générateur Xcode Interface")](unified-storyboards-images/dls01.png#lightbox)
3. Dans Xcode, sélectionnez **fichier** > **nouveau** > **fichier...** :

    [![](unified-storyboards-images/dls02.png "Sélectionnez le fichier / nouveau")](unified-storyboards-images/dls02.png#lightbox)
4. Sélectionnez **iOS** > **Interface utilisateur** > **lancer l’écran** et cliquez sur le **suivant** bouton :

    [![](unified-storyboards-images/dls03.png "Sélectionnez iOS / de l’Interface utilisateur / lancer l’écran")](unified-storyboards-images/dls03.png#lightbox)
5. Nommez le fichier `LaunchScreen.xib` et cliquez sur le **créer** bouton :

    [![](unified-storyboards-images/dls04.png "Nommez le fichier LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. Modifier la conception de l’écran d’installation en ajoutant des éléments graphiques et à l’aide des contraintes de disposition pour les positionner pour les appareils donnés, les orientations et les tailles d’écran :

    [![](unified-storyboards-images/dls05.png "Modification de la conception de l’écran d’installation")](unified-storyboards-images/dls05.png#lightbox)
7. Enregistrer les modifications apportées à `LaunchScreen.xib`.
8. Sélectionnez le **Applications cible** et **général** onglet :

    [![](unified-storyboards-images/dls06.png "Sélectionnez la cible des Applications et l’onglet Général")](unified-storyboards-images/dls06.png#lightbox)
9. Cliquez sur le **choisir le fichier Info.plist** bouton, sélectionnez le `Info.plist` pour l’application Xamarin et cliquez sur le **choisir** bouton :

    [![](unified-storyboards-images/dls07.png "Sélectionnez le fichier Info.plist de l’application Xamarin")](unified-storyboards-images/dls07.png#lightbox)
10. Dans le **icônes de l’application et lancer des Images** section, ouvre le **fichier écran de démarrage** liste déroulante et choisissez le `LaunchScreen.xib` créé ci-dessus :

    [![](unified-storyboards-images/dls08.png "Choisissez le LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. Enregistrez les modifications dans le fichier et revenir à Visual Studio pour Mac.
12. Attendez que Visual Studio pour Mac terminer la synchronisation des modifications avec Xcode.
13. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le **ressource** et sélectionnez **ajouter** > **ajouter des fichiers...** :

    [![](unified-storyboards-images/dls09.png "Sélectionnez Ajouter / ajoute des fichiers...")](unified-storyboards-images/dls09.png#lightbox)
14. Sélectionnez le `LaunchScreen.xib` fichier créé précédemment et cliquez sur le **ouvrir** bouton :

    [![](unified-storyboards-images/dls10.png "Sélectionnez le fichier LaunchScreen.xib")](unified-storyboards-images/dls10.png#lightbox)
15. Générez l'application.

### <a name="testing-the-dynamic-launch-screen"></a>Test de l’écran d’installation dynamique

Dans Visual Studio pour Mac, sélectionnez le simulateur de rétine iPhone 4 et exécuter l’application. L’écran de lancement dynamique s’affichera dans le format correct et l’orientation :

[![](unified-storyboards-images/dls11.png "L’écran de lancement dynamique affiché de l’orientation verticale")](unified-storyboards-images/dls11.png#lightbox)

Arrêter l’application dans Visual Studio pour Mac et sélectionnez un appareil iOS 8 d’iPad. Exécutez l’application et l’écran d’installation sera correctement formaté pour ce périphérique et de l’orientation :

[![](unified-storyboards-images/dls12.png "L’écran de lancement dynamique affiché dans l’orientation horizontale")](unified-storyboards-images/dls12.png#lightbox)

Revenez à Visual Studio pour Mac et arrêter l’application en cours d’exécution.

### <a name="working-with-ios-7"></a>Travailler avec iOS 7

Pour assurer la compatibilité descendante avec iOS 7, choisir d’inclure simplement classiques `Default.png` actifs normalement dans l’application iOS 8 de l’image. iOS retournera le comportement précédent et utiliser ces fichiers en tant que l’écran de démarrage lors de l’exécution sur un appareil iOS 7.

Pour voir une implémentation d’un écran de lancement dynamique dans Xamarin, examinez le [dynamique écrans lancer](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) exemple d’application iOS 8 jointe à ce document.

## <a name="summary"></a>Récapitulatif

Cet article a pris un coup de œil les Classes de taille et leur impact sur la disposition des appareils iPhone et iPad. Il décrit comment caractéristiques, des environnements de caractéristique et des Collections de caractéristique fonctionnent avec les Classes de taille pour créer des Interfaces unifiée. Il a fallu brièvement Adaptive contrôleurs d’affichage et leur fonctionnement avec les Classes de taille dans des Interfaces d’unifiée. Il se présentait à mettre en oeuvre des Classes de taille et les Interfaces d’unifiée complètement à partir de code c# à l’intérieur d’une application iOS 8 de Xamarin.

Enfin, cet article couvert les principes fondamentaux de la création d’animations d’unifiée avec Xamarin iOS concepteur qui fonctionne sur des appareils iOS et la création d’un seul écran de lancement dynamique qui s’affichera en tant que l’écran de démarrage sur chaque appareil iOS 8.

## <a name="related-links"></a>Liens associés

- [Adaptive Photos (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [Exemple de StoryboardIntro](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Écrans de lancement dynamique (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Introduction à iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Les dispositions dynamiques dans iOS8 - évoluer 2014 (vidéo)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
