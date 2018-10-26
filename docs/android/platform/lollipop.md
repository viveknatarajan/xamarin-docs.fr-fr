---
title: Fonctionnalités d’interface (lollipop)
description: Cet article fournit une vue d’ensemble détaillée des nouvelles fonctionnalités introduites dans Android 5.0 (Lollipop). Ces fonctionnalités incluent un nouveau style d’interface utilisateur appelé thème de matériau, ainsi que les nouvelles fonctionnalités de prise en charge telles que des animations, des ombres de vue et teintes drawable. Android 5.0 inclut également des notifications améliorées, deux nouveaux widgets d’interface utilisateur, un nouveau Planificateur de travail et un certain nombre de nouvelles API afin d’améliorer le stockage, la mise en réseau, la connectivité et les fonctionnalités multimédia.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d79c0563d1dc9a2cfe75b702300982bb4d38553b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117861"
---
# <a name="lollipop-features"></a>Fonctionnalités d’interface (lollipop)

_Cet article fournit une vue d’ensemble détaillée des nouvelles fonctionnalités introduites dans Android 5.0 (Lollipop). Ces fonctionnalités incluent un nouveau style d’interface utilisateur appelé thème de matériau, ainsi que les nouvelles fonctionnalités de prise en charge telles que des animations, des ombres de vue et teintes drawable. Android 5.0 inclut également des notifications améliorées, deux nouveaux widgets d’interface utilisateur, un nouveau Planificateur de travail et un certain nombre de nouvelles API afin d’améliorer le stockage, la mise en réseau, la connectivité et les fonctionnalités multimédia._

## <a name="lollipop-overview"></a>Vue d’ensemble de l’interface (lollipop)

Android 5.0 (Lollipop) introduit un nouveau langage de conception, *Material Design*, et avec lui un prenant en charge effectuer un cast de nouvelles fonctionnalités pour créer des applications plus facile et plus intuitif à utiliser. Avec Material Design, Android 5.0 offre non seulement les téléphones Android une restructuration ; Il fournit également un nouvel ensemble de règles de conception pour les tablettes Android, les ordinateurs de bureau, les espions et les télévisions intelligentes. Ces règles de création de mettre en évidence la simplicité et minimalism tout en rendant utiliser des attributs de tactiles familiers (tels que les signaux aire et périmètre réalistes) pour aider les utilisateurs rapidement et intuitivement comprendre l’interface.

*Thème matériau* est l’incarnation de ces principes de conception de l’interface utilisateur dans Android. Cet article commence par traitant des fonctionnalités de prise en charge du thème matériau :

-   **Animations** &ndash; *Touch commentaires* animations, *transition de l’activité* animations, *afficher la transition d’état* animations et un *révéler effet*.

-   **Afficher les ombres et l’élévation** &ndash; les vues ont maintenant un `elevation` propriété ; vues avec supérieur `elevation` valeurs converties des ombres supérieure sur l’arrière-plan.

-   **Fonctionnalités de couleur** &ndash; *teintes Drawable* rend possible pour vous permet de réutiliser des ressources d’image en modifiant leur couleur, et *extraction de la couleur éminents* vous permet de manière dynamique thème de votre application en fonction de couleurs dans une image.

Thème de matériel de nombreuses fonctionnalités sont déjà intégrées à l’Android 5.0 expérience utilisateur, tandis que d’autres doivent être ajoutés explicitement aux applications. Par exemple, certaines vues standard (tels que des boutons) incluent déjà des animations de commentaires tactile, tandis que les applications doivent activer la plupart des ombres de vue.

Outre les améliorations de l’interface utilisateur apportées via le thème de matériau, Android 5.0 inclut également plusieurs autres nouvelles fonctionnalités qui sont couvertes dans cet article :

-   **Amélioré notifications** &ndash; Notifications dans Android 5.0 ont été considérablement mises à jour avec un nouveau look, prise en charge pour les notifications de l’écran de verrouillage et un nouveau *tête haute* format de présentation de notification.

-   **Nouveaux widgets d’interface utilisateur** &ndash; le nouveau `RecyclerView` widget simplifie l’utilisation des applications transmettre des jeux de données volumineux et des informations complexes et le nouveau `CardView` widget fournit un format de présentation de type carte simplifiée pour afficher du texte et images.

-   **Nouvelles API** &ndash; Android 5.0 ajoute de nouvelles API pour la prise en charge de réseau multiples, amélioration de connectivité Bluetooth, la gestion de stockage plus facile et un contrôle plus souple de lecteurs multimédias et les appareils photo. Un nouveau travail de la fonctionnalité de planification est disponible pour exécuter des tâches de façon asynchrone à des heures planifiées. Cette fonctionnalité contribue à améliorer l’autonomie par, par exemple, planification des tâches d’avoir lieu quand l’appareil est branché et la facturation.


## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités de Android 5.0 dans les applications basées sur Xamarin :

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac. 

-   **Kit de développement logiciel Android** &ndash; Android 5.0 (API 21) ou version ultérieure doit être installé via le gestionnaire Android SDK.

-   **Kit de développement Java** &ndash; Xamarin.Android nécessite [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou version ultérieure si vous développez pour le niveau d’API 24 ou supérieure (JDK 1.8 prend également en charge les niveaux d’API antérieures à 24, y compris Lollipop). La version 64 bits du JDK 1.8 est requise si vous utilisez des contrôles personnalisés ou le Générateur d’aperçu de formulaires.

Vous pouvez continuer à utiliser [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) si vous êtes développement spécifiquement pour le niveau d’API 23 ou une version antérieure.


## <a name="setting-up-an-android-50-project"></a>Configuration d’un projet Android 5.0

Pour créer un projet Android 5.0, vous devez installer les derniers outils et les packages de kit de développement logiciel. Utilisez les étapes suivantes pour configurer un projet Xamarin.Android qui cible Android 5.0 :

1. Installer les outils Xamarin.Android et activer votre licence Xamarin. Consultez [le programme d’installation et Installation](~/android/get-started/installation/index.md) pour plus d’informations sur l’installation de Xamarin.Android.

2. Si vous utilisez Visual Studio pour Mac, installez les dernières mises à jour Android 5.0.

3. Démarrer le gestionnaire Android SDK (dans Visual Studio pour Mac, utilisez **outils &gt; Open Android SDK Manager&hellip;**) et installer Android SDK Tools 23.0.5 ou version ultérieure :

    [![En sélectionnant Outils Android SDK dans le Gestionnaire de kit de développement logiciel Android.](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   En outre, installez les derniers packages de kit de développement logiciel Android 5.0 (API 21 ou version ultérieure) :

    [![Installation des packages de kit de développement logiciel Android 5.0 dans le Gestionnaire de kit de développement logiciel Android.](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Pour plus d’informations sur l’utilisation du gestionnaire Android SDK, consultez [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).

4. Créer un nouveau projet Xamarin.Android. Si vous êtes novice en développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Android. Lorsque vous créez un projet Android, veillez à configurer les paramètres de version pour Android 5.0.
   Dans Visual Studio pour Mac, accédez à **Options du projet &gt; Build &gt; général** et définissez **framework cible** à **Android 5.0 (Lollipop)** ou plus tard :

    ![Définition de la cible Framwework sur Android 5.0 Lollipop](lollipop-images/target-framework.png)

   Sous **Options du projet &gt; Build &gt; Application Android**, définissez minimale et la cible Android version à **automatique - Utilisez la version target framework**:

    ![Définir les versions minimale et cible Android sur automatique](lollipop-images/minimum-android-version.png)

5. Configurer un émulateur ou un appareil Android pour tester votre application. Si vous utilisez un émulateur, consultez [configuration de l’émulateur Android](~/android/get-started/installation/android-emulator/index.md) pour savoir comment configurer un émulateur Android pour une utilisation avec Xamarin Studio ou Visual Studio. Si vous utilisez un appareil Android, consultez [paramètre le SDK de visualisation](https://developer.android.com/preview/setup-sdk.html) pour savoir comment mettre à jour votre appareil pour Android 5.0. Pour configurer votre appareil Android pour exécuter et déboguer des applications Xamarin.Android, consultez [configurer un appareil pour le développement](~/android/get-started/installation/set-up-device-for-development.md).

Remarque : Si vous mettez à jour un projet Android existant qui ciblait la version préliminaire de L Android, vous devez mettre à jour le **Framework cible** et **Android version** aux valeurs décrites ci-dessus.

## <a name="important-changes"></a>Modifications importantes

Précédemment, les applications Android publiées pourrait être affectées par des modifications dans Android 5.0. En particulier, Android 5.0 utilise une nouvelle exécution et un format de notification de modification de manière significative.

### <a name="android-runtime"></a>Runtime Android

Android 5.0 utilise le Runtime Android nouveau (ART) en tant que le runtime par défaut au lieu de Dalvik. ART implémente plusieurs nouvelles fonctionnalités majeures :

-   **Compilation de Ahead-of-time (AOT)** &ndash; AOT peut améliorer les performances de l’application en compilant le code d’application avant le premier lancement de l’application. Quand une application est installée, ART génère une application compilée exécutable pour l’appareil cible.

-   **Amélioré le garbage collection (GC)** &ndash; améliorations GC dans la bibliothèque peuvent également améliorer les performances de l’application. Le garbage collection maintenant utilise une pause de GC au lieu de deux, et effectuer des opérations GC simultanées à plus de temps.

-   **Amélioration du débogage de l’application** &ndash; ART fournit des détails de diagnostic pour aider à analyser les exceptions et les rapports d’incident.

Les applications existantes doivent fonctionner sans modification dans ART &ndash; à l’exception des applications qui exploitent des techniques uniques à l’exécution de Dalvik précédente, ce qui peut ne pas fonctionner sous ART. Pour plus d’informations sur ces modifications, consultez [vérification de comportement de l’application sur le Runtime Android (ART)](http://developer.android.com/guide/practices/verifying-apps-art.html).


### <a name="notification-changes"></a>Modifications de notification

Notifications ont sensiblement évolué dans Android 5.0 :

-   **Sons et vibration sont gérées différemment** &ndash; des sons de Notification et vibrations sont désormais gérées par `Notification.Builder` au lieu de `Ringtone`, `MediaPlayer`, et `Vibrator`.

-   **Nouveau modèle de couleurs** &ndash; conformément à thème matériau, les notifications sont rendues avec texte foncé dans un arrière-plan blanc ou très clair. En outre, les canaux alpha dans les icônes de notification peuvent être modifiés par Android pour coordonner avec sa palette de couleurs système. 

-   **Notifications de l’écran de verrouillage** &ndash; Notifications peuvent maintenant apparaître sur l’appareil l’écran de verrouillage.

-   **Tête haute** &ndash; notifications de haute priorité apparaissent désormais dans une petite fenêtre flottante (notification de tête haute) lorsque l’appareil est déverrouillé et que l’écran est activé.

Dans la plupart des cas, le portage des fonctionnalités de notification d’application existante pour Android 5.0 nécessite les étapes suivantes :

1.  Convertir votre code pour utiliser `Notification.Builder` (ou `NotificationsCompat.Builder`) pour la création de notifications. 

2.  Vérifiez que vos ressources existantes de la notification sont visibles dans le nouveau modèle de couleurs de thème du matériau.

3.  Décidez quels visibilité vos notifications doivent avoir lorsqu’ils sont présentés sur l’écran de verrouillage. Si une notification n’est pas publique, le contenu qui doit s’afficher sur l’écran de verrouillage ?

4.  Définir la catégorie de vos notifications afin qu’ils sont gérés correctement dans le nouveau Android 5.0 *ne pas déranger* mode.

Si vos notifications présentent des contrôles de transport, support d’affichage état de la lecture, utilisez `RemoteControlClient`, ou appelez `ActivityManager.GetRecentTasks`, consultez [importantes modifications de comportement](http://developer.android.com/preview/api-overview.html#Behaviors) pour plus d’informations sur la mise à jour de vos notifications pour Android 5.0.

Pour plus d’informations sur la création de notifications dans Android, consultez [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md). Le [compatibilité](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) section de cet article explique comment créer des notifications qui présentent une compatibilité descendante avec les versions antérieures d’Android.


## <a name="material-theme"></a>Thème matériau

Le nouveau thème de matériel Android 5.0 apporte des modifications importantes à l’apparence de l’interface utilisateur Android. Éléments visuels maintenant utilisent des surfaces tactiles prennent sur le graphique en gras et la typographie couleurs vives de structure d’impression. Exemples de documents de thème sont représentées dans les captures d’écran suivante :

[![Captures d’écran de l’écran d’accueil de thème matériau, écran des applications et écran de paramètre](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 vous accueille avec l’écran d’accueil qui apparaît sur la gauche. La capture d’écran du centre est le premier écran de la liste des applications, et la capture d’écran de droite est le **paramètres** écran. Google [Material Design](https://material.io/guidelines/material-design/introduction.html) spécification explique les règles de conception sous-jacente derrière le nouveau concept de thème du matériau.

Thème matériau inclut trois versions intégrées que vous pouvez utiliser dans votre application : le `Theme.Material` (la valeur par défaut), le thème sombre le `Theme.Material.Light` thème et le `Theme.Material.Light.DarkActionBar` thème : 

[![Thèmes des captures d’écran de foncé, clair et DarkActionBar](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Pour plus d’informations sur l’utilisation des fonctionnalités de thème de documents dans les applications Xamarin.Android, consultez [matériau thème](~/android/user-interface/material-theme.md).


## <a name="animations"></a>Animations

Android 5.0 fournit des animations de commentaires tactile, animations de transition d’activité et animations de transition d’état vue pour rendre les interfaces d’application plus intuitive à utiliser. En outre, les applications Android 5.0 peuvent utiliser *révéler effet* animations pour masquer ou afficher des vues. Vous pouvez utiliser *courbé mouvement* paramètres pour configurer comment rapidement ou lentement les animations sont rendues.


### <a name="touch-feedback-animations"></a>Animations de commentaires tactile

Animations de commentaires tactiles fournissent aux utilisateurs des commentaires visuels lorsqu’une vue a été touchée. Par exemple, boutons affichent désormais un effet papillon quand ils sont touchées &ndash; Voici l’animation de commentaires par défaut tactiles dans Android 5.0. Animation de Ripple est implémentée par la nouvelle `RippleDrawable` classe. Cet effet peut être configuré pour se terminer dans les limites de la vue ou l’étendre au-delà des limites de la vue. Par exemple, la séquence de captures d’écran suivante illustre l’effet de ripple dans un bouton pendant une animation de tactile :

![Frame par frame des captures d’écran de l’animation de ripple sur un bouton](lollipop-images/touch-animation.png)

Contact contact initial avec le bouton se produit dans la première image sur la gauche, tandis que la séquence restante (de gauche à droite) illustre la façon dont les répercussions répartit sur le bord du bouton. Lorsque l’animation ripple se termine, la vue retourne à son apparence d’origine. L’animation de ripple par défaut s’effectue en une fraction de seconde, mais la longueur de l’animation peut être personnalisée pour des longueurs de temps plus ou moins longtemps.

Pour plus d’informations sur touch animations de commentaires dans Android 5.0, consultez [personnaliser les commentaires Touch](http://developer.android.com/training/material/animations.html#Touch).


### <a name="activity-transition-animations"></a>Animations de Transition d’activité

Animations de transition d’activité donnent aux utilisateurs d’une idée de la continuité des activités visual lorsqu’une activité passe à un autre. Les applications peuvent spécifier trois types d’animations de transition :

-   **Entrez la transition** &ndash; pour laquelle une activité entre dans la scène.

-   **Quitter la transition** &ndash; pour lorsqu’une activité termine la scène.

-   **Partagé de transition de l’élément** &ndash; pour quand une vue qui est commune à deux activités change à mesure que des première transitions d’activité à l’autre.

Par exemple, la séquence de captures d’écran suivante illustre une transition de l’élément partagé :

[![Frame par frame des captures d’écran d’une animation de transition élément partagé](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Un élément partagé (il s’agit d’une photo d’un caterpillar) est un des plusieurs vues dans la première activité ; Il s’agrandit pour devenir la vue uniquement dans la deuxième activité en tant que les transitions d’activité premier au second.

#### <a name="enter-transition-animation-types"></a>Entrez les Types d’animations de Transition

Pour les transitions de l’entrée, Android 5.0 fournit trois types d’animations :

-   **Explode animation** &ndash; agrandit une vue à partir du centre de la scène.

-   **Animation de diapositives** &ndash; déplace une vue d’un des bords de la scène.

-   **Animation de fondu** &ndash; fondu d’une vue dans la scène.

#### <a name="exit-transition-animation-types"></a>Types d’animations de Transition de sortie

Pour quitter les transitions, Android 5.0 fournit trois types d’animations :

-   **Explode animation** &ndash; réduit une vue au centre de la scène.

-   **Animation de diapositives** &ndash; déplace une vue à un des bords de la scène.

-   **Animation de fondu** &ndash; fondu d’une vue de la scène.

#### <a name="shared-element-transition-animation-types"></a>Élément Transition Animation Types partagés

Transitions de l’élément partagé prennent en charge plusieurs types d’animations, telles que :

-   Modifier les limites de mise en page ou l’image d’une vue.

-   Modification de la mise à l’échelle et la rotation d’une vue.

-   Modification du type de taille et l’échelle pour une vue.

Pour plus d’informations sur les animations de transition d’activité dans Android 5.0, consultez [personnaliser les Transitions activité](http://developer.android.com/training/material/animations.html#Transitions).


### <a name="view-state-transition-animations"></a>Animations de Transition d’état d’affichage

Android 5.0 rend possible pour les animations à exécuter lorsque l’état d’une vue change. Vous pouvez animer des transitions d’état de vue en utilisant l’une des techniques suivantes :

-   Créer drawables qui animer des modifications d’état associées à une vue. La nouvelle `AnimatedStateListDrawable` classe vous permet de créer de drawables qui affichent des animations entre les changements d’état de vue.

-   Définir les fonctionnalités d’animation qui s’exécute lorsque l’état d’une vue change. La nouvelle `StateListAnimator` classe vous permet de définir une animation qui s’exécute lorsque l’état d’une vue change.

Pour plus d’informations sur les animations de transition d’état vue dans Android 5.0, consultez [animer des modifications d’état vue](http://developer.android.com/training/material/animations.html#ViewState).


### <a name="reveal-effect"></a>Révéler effet

Le *révéler effet* est un cercle de découpage ce rayon de modifications pour afficher ou masquer une vue. Vous pouvez contrôler cet effet en définissant le rayon initial et final du cercle de découpage. La séquence de captures d’écran suivante illustre une animation d’effet révéler à partir du centre de l’écran :

[![Frame par frame des captures d’écran de divulgation animation](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

La séquence suivante illustre une animation d’effet de divulgation qui a lieu dans le coin inférieur gauche de l’écran :

[![Frame par frame des captures d’écran de l’animation de découpage](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Révéler les animations peuvent être inversées ; le cercle de découpage peut réduire pour masquer la vue au lieu d’agrandir pour afficher la vue.

Pour plus d’informations sur l’effet de divulgation Android 5.0 dans, consultez [utilisation de l’effet de révéler](http://developer.android.com/training/material/animations.html#Reveal).


### <a name="curved-motion"></a>Mouvement en arc

En plus de ces fonctionnalités d’animation, Android 5.0 fournit également de nouvelles API qui vous permettent de spécifier les courbes de temps et de mouvement d’animations. Android 5.0 utilise ces courbes pour interpoler le mouvement temporel et spatial pendant les animations. Trois courbes sont définies dans Android 5.0 :

-   **Fast\_out\_linéaire\_dans** &ndash; accélère rapidement et continue à accélérer jusqu'à la fin de l’animation.

-   **Fast\_out\_lente\_dans** &ndash; accélère rapidement et lentement ralentit vers la fin de l’animation.

-   **Linéaire\_out\_lente\_dans** &ndash; commence avec une rapidité maximale et lentement ralentit à la fin de l’animation.

Vous pouvez utiliser la nouvelle `PathInterpolator` classe pour spécifier comment interpolation déplacement a lieu. `PathInterpolator` est un interpolateur qui traverse les chemins d’accès de l’animation en fonction des points de contrôle spécifié et les courbes de mouvement. Pour plus d’informations sur la façon de spécifier les paramètres de la courbe de mouvement dans Android 5.0, consultez [Motion courbé utilisation](http://developer.android.com/training/material/animations.html#CurvedMotion).


## <a name="view-shadows--elevation"></a>Vue Shadows & élévation

Dans Android 5.0, vous pouvez spécifier le *élévation* d’une vue en définissant un nouveau `Z` propriété. Un signe supérieur `Z` valeur provoque l’affichage peut convertir une ombre plus grande sur l’arrière-plan, ce qui la vue s’affichent en float supérieur au-dessus de l’arrière-plan. Vous pouvez définir l’élévation initiale d’une vue en configurant ses `elevation` attribut dans la disposition.

L’exemple suivant illustre les ombres un cast à vide `TextView` contrôler quand son élévation attribut a la valeur 2dp, 4dp et 6dp, respectivement :

[![Captures d’écran des ombres de vue supérieure progessively](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Afficher les paramètres de clichés instantanés peuvent être statiques (comme indiqué ci-dessus), ou elles peuvent servir dans des animations pour afficher une vue d’augmenter temporairement ci-dessus en arrière-plan de la vue. Vous pouvez utiliser la `ViewPropertyAnimator` classe pour animer l’élévation d’une vue. L’élévation d’une vue est la somme de sa disposition `elevation` paramètre plus une `translationZ` propriété que vous pouvez définir via un `ViewPropertyAnimator` appel de méthode.

Pour plus d’informations sur les ombres de vue dans Android 5.0, consultez [définissant les ombres et les vues de découpage](http://developer.android.com/training/material/shadows-clipping.html).


## <a name="color-features"></a>Fonctionnalités de couleur

Android 5.0 fournit deux nouvelles fonctionnalités de gestion des couleurs dans les applications :

-   *Teintes drawable* vous permet de modifier les couleurs des ressources d’image en modifiant un attribut de disposition.

-   *Extraction de la couleur éminents* rend possible pour vous permettre de personnaliser le thème de couleur de votre application pour se coordonner avec la palette de couleurs d’une image affichée de manière dynamique.


### <a name="drawable-tinting"></a>Teintes drawable

5.0 dispositions Android reconnaissent un nouveau `tint` attribut que vous pouvez utiliser pour définir la couleur de drawables sans avoir à créer plusieurs versions de ces ressources pour afficher des couleurs différentes. Pour utiliser cette fonctionnalité, vous définissez une image bitmap en tant que masque alpha et utilisez le `tint` attribut permettant de définir la couleur de l’élément multimédia. Cela rend possible pour créer des ressources qu’une seule fois et les couleurs dans votre présentation en fonction de votre thème.

Dans l’exemple suivant, une seule ressource &ndash; un logo blanc avec un arrière-plan transparent &ndash; est utilisé pour créer des variations de teinte :

![Logo blanc Xamarin avec arrière-plan transparent](lollipop-images/xamarin-logo-white.png)

Ce logo est affiché au-dessus un arrière-plan bleu circulaire, comme indiqué dans les exemples suivants. L’image sur la gauche est la façon dont le logo s’affiche sans un `tint` paramètre. Dans le centre, le logo de l’image `tint` attribut est défini sur un gris foncé. Dans l’image sur la droite, `tint` est définie sur un gris clair :

![Exemples du logo ci-dessus avec les paramètres de teinte différents](lollipop-images/drawable-tinting.png)

Pour plus d’informations sur les teintes drawable dans Android 5.0, consultez [teintes Drawable](http://developer.android.com/training/material/drawables.html#DrawableTint).


### <a name="prominent-color-extraction"></a>Extraction de la couleur principales

Le nouveau Android 5.0 `Palette` classe vous permet d’extraire des couleurs à partir d’une image afin que vous puissiez les appliquer dynamiquement à une palette de couleurs personnalisée. Le `Palette` classe extrait les six couleurs d’une image et des étiquettes de ces couleurs en fonction de leurs niveaux relatifs de saturation de la couleur et de luminosité :

-   Dynamique

-   Dynamique foncé

-   Lumière dynamique

-   Muet

-   Sombre muet

-   Lumière muet

Par exemple, dans les captures d’écran suivante, une application d’affichage de photos extrait les couleurs principales à partir de l’image sur l’affichage et utilise ces couleurs pour adapter le modèle de couleurs de l’application pour correspondre à l’image :

[![Captures d’écran des extractions de couleur de thème vert, bleu et rose](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

Dans les captures d’écran ci-dessus, la barre d’action a la valeur extraite « vives light » couleur et l’arrière-plan est définie sur extrait « vives sombre » couleur. Dans chaque exemple ci-dessus, une ligne des carrés de couleur small est incluse pour illustrer la palette de couleurs qui ont été extraites de l’image.

Pour plus d’informations sur l’extraction de la couleur dans Android 5.0, consultez [extraction principales couleurs à partir d’une Image](http://developer.android.com/training/material/drawables.html#ColorExtract).


## <a name="new-ui-widgets"></a>Nouveaux Widgets d’interface utilisateur

Android 5.0 introduit deux nouveaux widgets d’interface utilisateur :

-   `RecyclerView` &ndash; Un groupe de vue qui affiche une liste d’éléments à défilement.

-   `CardView` &ndash; Disposition de base avec des angles arrondis.

Les deux widgets sont intégrées dans de charge des fonctionnalités de thème de matériel ; par exemple, `RecyclerView` utilise des animations pour ajouter et supprimer des vues, et `CardView` utilise afficher shadows pour afficher chaque carte pour les faire flotter au-dessus de l’arrière-plan. Exemples de ces nouveaux widgets sont présentés dans les captures d’écran suivante :

[![Captures d’écran des applications créées avec RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

La capture d’écran de gauche est un exemple de `RecyclerView` tel qu’utilisé dans une application de messagerie et la capture d’écran sur la droite est un exemple de `CardView` comme utilisé dans une application de réservation de voyages.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` est similaire à `ListView,` , mais il est mieux adapté aux grands ensembles de vues ou les listes avec des éléments qui changent de manière dynamique. Comme `ListView,` vous spécifiez un adaptateur pour accéder au jeu de données sous-jacent. Cependant, contrairement à `ListView,` vous utilisez un *Gestionnaire de disposition* pour positionner des éléments dans `RecyclerView`. Le Gestionnaire de disposition s’occupe également de recyclage de la vue ; Il gère la réutilisation des vues des éléments qui ne sont plus visibles par l’utilisateur.

Lorsque vous utilisez un `RecyclerView` widget, vous devez spécifier un `LayoutManager` et une carte graphique. Comme indiqué dans cette figure, `LayoutManager` est l’intermédiaire entre l’adaptateur et le `RecyclerView`:

![Diagramme de RecyclerView avec prise en charge LayoutManager, adaptateur et jeu de données](lollipop-images/recyclerview-diagram.png)

Les captures d’écran suivantes illustrent un `RecyclerView` qui contient 100 éléments (chaque élément se compose d’un `ImageView` et un `TextView`) :

[![Captures d’écran d’une application de RecyclerView défilement d’images](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` gère ce jeu de données volumineux en toute simplicité &ndash; le défilement à partir du début de la liste à la fin de la liste dans cet exemple, application prend quelques secondes seulement. `RecyclerView` prend également en charge les animations ; en fait, les animations pour ajouter et supprimer des éléments sont activées par défaut. Lorsqu’un élément est ajouté à un `RecyclerView`, il disparaît dans comme indiqué dans cette séquence de captures d’écran :

[![Frame par frame capture d’écran d’un fondu d’élément photos dans](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Pour plus d’informations `RecyclerView`, consultez [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


### <a name="cardview"></a>CardView

`CardView` est une vue simple qui simule une carte flottante avec des angles arrondis. Étant donné que `CardView` a ombres d’affichage intégré, il fournit un moyen facile d’ajouter de profondeur visuelle à votre application. Les captures d’écran suivantes illustrent trois orienté texte `CardView`:

[![Captures d’écran de l’exemple d’applications à l’aide de RecyclerView avec CardView des éléments](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Les cartes dans l’exemple ci-dessus contient un `TextView`; la couleur d’arrière-plan est définie via la `cardBackgroundColor` attribut.

Pour plus d’informations `CardView`, consultez [CardView](~/android/user-interface/controls/card-view.md).


## <a name="enhanced-notifications"></a>Notifications améliorées

Le système de notification dans Android 5.0 a été considérablement mis à jour avec un nouveau format visuel et de nouvelles fonctionnalités. Notifications ont un nouveau look dans Android 5.0. Par exemple, les notifications dans Android 5.0 utilisent maintenant texte foncé sur un arrière-plan clair :

![Exemple d’une notification d’Android 5.0 non développée](lollipop-images/expanded-notification-contracted.png)

Lorsqu’une grande icône est affichée dans une notification (comme indiqué dans l’exemple ci-dessus), Android 5.0 présente la petite icône comme un badge sur l’icône de grande taille. 

Dans Android 5.0, les notifications peuvent également apparaître sur l’appareil l’écran de verrouillage.
Par exemple, voici une capture d’écran de l’exemple d’une écran de verrouillage avec une seule notification :

[![Capture d’écran de notification qui apparaît sur l’écran de verrouillage](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Les utilisateurs peuvent double-cliquer une notification sur l’écran de verrouillage pour déverrouiller l’appareil et accéder à l’application à l’origine de cette notification, ou faites défiler pour faire disparaître la notification. Notifications ont un nouveau *visibilité* paramètre qui détermine la quantité de contenu peut être affiché sur l’écran de verrouillage. Les utilisateurs peuvent choisir s’il faut autoriser le contenu sensible à afficher dans les notifications de l’écran de verrouillage.

Android 5.0 introduit un nouveau format de présentation de notification de haute priorité appelé *tête haute*. Les notifications de tête haute faites glisser vers le bas à partir du haut de l’écran pendant quelques secondes et puis reformatage vers la nuance de notification en haut de l’écran. Notifications de tête haute rendent possible pour le système de l’interface utilisateur pour placer des informations importantes devant l’utilisateur sans interrompre l’activité en cours d’exécution. L’exemple suivant illustre une simple notification tête haute qui s’affiche sur une application :

[![Exemple d’une notification profondes](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Notifications de tête haute sont généralement utilisées pour les événements suivants :

-   Un nouveau message suivant

-   Un appel téléphonique entrant

-   Indication de batterie faible

-   Une alarme

Android 5.0 affiche une notification au format de tête haute uniquement lorsqu’il a un paramètre de priorité élevée ou max.

Dans Android 5.0, vous pouvez fournir des métadonnées de notification pour aider à Android trier et afficher les notifications plus intelligemment. Android 5.0 organise les notifications en fonction de la priorité, de visibilité et de catégorie.
Catégories de notification sont utilisés pour filtrer les notifications peuvent être présentées lorsque celui-ci se trouve dans *ne pas déranger* mode.

Pour plus d’informations sur la création et le lancement des notifications avec les dernières fonctionnalités d’Android 5.0, consultez [Notifications locales](~/android/app-fundamentals/notifications/local-notifications.md).


## <a name="new-apis"></a>Nouvelles API

Outre les nouvelles fonctionnalités de l’apparence et convivialité décrites ci-dessus, Android 5.0 ajoute de nouvelles API qui étendent les fonctionnalités de multimédia existant, fonctionnalités et de stockage/connectivité sans fil. En outre, Android 5.0 inclut de nouvelles API qui prennent en charge une nouvelle fonctionnalité de planificateur de travail.

### <a name="camera"></a>Appareil photo

Android 5.0 fournit plusieurs nouvelles API pour les fonctionnalités de l’appareil photo améliorée. La nouvelle `Android.Hardware.Camera2` espace de noms inclut des fonctionnalités de l’accès aux appareils de la caméra connectée à un appareil Android. En outre, `Android.Hardware.Camera2` modélise chaque appareil photo en tant que pipeline : elle accepte une demande de capture, capture l’image, puis renvoie le résultat. Cette approche rend possible pour les applications en file d’attente de plusieurs demandes de capture sur un appareil photo.

Les API suivantes rendent ces nouvelles fonctionnalités possible :

-   `CameraManager.GetCameraIdList` &ndash; Vous permet d’accéder par programmation aux appareils de l’appareil photo ; vous utilisez `CameraManager.OpenCamera` pour se connecter à un appareil photo spécifique.

-   `CameraCaptureSession` &ndash; Capture ou de diffusion des images à partir de l’appareil photo. Vous implémentez un `CameraCaptureSession.CaptureListener` permettant de gérer les nouveaux événements de capture d’image.

-   `CaptureRequest` &ndash; Définit les paramètres de capture.

-   `CaptureResult` &ndash; Présente les résultats d’une opération de capture d’image.

Pour plus d’informations sur l’appareil photo la nouvelle API dans Android 5.0, consultez [Media](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Lecture audio

Les mises à 5.0 jour Android la `AudioTrack` classe pour une meilleure lecture audio :

-   `ENCODING_PCM_FLOAT` &ndash; Configure `AudioTrack` pour accepter des données audio dans un format à virgule flottante pour une meilleure plage dynamique, une marge supérieure et une qualité supérieure (grâce à une plus grande précision). Format à virgule flottante permet également, pour éviter le découpage audio.

-   `ByteBuffer` &ndash; Vous pouvez désormais fournir des données audio à `AudioTrack` en tant que tableau d’octets.

-   `WRITE_NON_BLOCKING` &ndash; Cette option simplifie la mise en mémoire tampon et le multithreading pour certaines applications.

Pour plus d’informations `AudioTrack` améliorations dans Android 5.0, consultez [Media](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Contrôle Media Playback

Android 5.0 introduit la nouvelle `Android.Media.MediaController` classe, qui remplace `RemoteControlClient`. `Android.Media.MediaController` Fournit des API de contrôle de transport simplifiée et offre un contrôle de la lecture en dehors du contexte de l’interface utilisateur thread-safe. Les nouvelles API suivantes gèrent le contrôle de transport :

-   `Android.Media.Session.MediaSession` &ndash; Une session de contrôle de média qui gère plusieurs contrôleurs. Vous appelez `MediaSession.GetSessionToken` pour demander un jeton que votre application utilise pour interagir avec la session.

-   `MediaController.TransportControls` &ndash; Commandes telles que le transport **lire**, **arrêter**, et **Skip**.

En outre, vous pouvez utiliser la nouvelle `Android.App.Notification.MediaStyle` classe à associer une session de support avec le contenu de notification riche (par exemple, l’extraction et affichage d’une pochette d’album).

Pour plus d’informations sur les nouvelles fonctionnalités de contrôle de la lecture multimédia dans Android 5.0, consultez [Media](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Stockage

Android 5.0 met à jour l’infrastructure d’accès de stockage pour faciliter aux applications d’utiliser des répertoires et des documents :

-   Pour sélectionner une sous-arborescence de répertoires, vous pouvez générer et envoyer un `Android.Intent.Action.OPEN_DOCUMENT_TREE` intention. Ce mode, le système afficher toutes les instances de fournisseur qui prennent en charge la sélection de la sous-arborescence ; l’utilisateur accède, puis sélectionne un répertoire.

-   Pour créer et gérer des documents ou des répertoires n’importe où sous une sous-arborescence, vous utilisez la nouvelle `CreateDocument`, `RenameDocument`, et `DeleteDocument` méthodes de `DocumentsContract`.

-   Pour obtenir les chemins d’accès aux répertoires de média sur tous les appareils de stockage partagé, vous appelez la nouvelle `Android.Content.Context.GetExternalMediaDirs` (méthode).

Pour plus d’informations sur les nouvelles API dans Android 5.0 de stockage, consultez [stockage](http://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Connectivité et sans fil

Android 5.0 ajoute les améliorations d’API suivantes pour la connectivité et sans fil :

-   Nouvelle *multi-réseau* API qui permettent à des applications rechercher et sélectionner des réseaux avec des capacités spécifiques avant d’établir une connexion.

-   Fonctionnalité de diffusion de Bluetooth qui permet à un appareil Android 5.0 à agir en tant que périphérique Bluetooth faible énergie.

-   Améliorations de NFC qui le rendent plus facile à utiliser les fonctionnalités de communications de champ proche pour partager des données avec d’autres périphériques.

Pour plus d’informations sur le nouveau sans fil et la connectivité API dans Android 5.0, consultez [sans fil et la connectivité](http://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Planification des travaux

Android 5.0 introduit un nouveau `JobScheduler` API permettant aux utilisateurs pour réduire une batterie se décharge, certaines tâches à exécuter uniquement quand l’appareil est branché de planification et de charge. Cette fonctionnalité de planificateur de travail peut également servir pour planifier une tâche à exécuter lorsque les conditions sont plus adaptées à cette tâche, telles que le téléchargement d’un fichier volumineux lorsque l’appareil est connecté via un réseau Wi-Fi au lieu d’un réseau limitées.

Pour plus d’informations sur la nouvelle tâche de planification des API dans Android 5.0, consultez [planification des tâches](http://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Récapitulatif

Cet article a fourni une vue d’ensemble des nouvelles fonctionnalités importantes dans Android 5.0 pour les développeurs d’applications Xamarin.Android :

-   Thème matériau

-   Animations

-   Les ombres de vue et l’élévation

-   Fonctionnalités de couleur, telles que teintes drawable et mise en évidence de couleur d’extraction

-   La nouvelle `RecyclerView` et `CardView` widgets

-   Améliorations de la notification

-   Nouvelles API de caméra, lecture audio, contrôle media, stockage, / connectivité sans fil et planification des travaux

Si vous êtes novice en développement de Xamarin Android, consultez [le programme d’installation et Installation](~/android/get-started/installation/index.md) pour vous aider à bien démarrer avec Xamarin.Android.
[Hello, Android](~/android/get-started/hello-android/index.md) est une excellente introduction pour apprendre à créer des projets Android.



## <a name="related-links"></a>Liens associés

- [L Android Developer Preview](http://developer.android.com/preview/index.html)
- [Obtenir le kit SDK Android](https://developer.android.com/sdk/index.html#Other)
- [Conception matérielle](http://developer.android.com/preview/material/index.html)
- [Principes de conception matérielle](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
