---
title: "Fonctionnalités de l’interface Lollipop"
description: "Cet article fournit une vue d’ensemble générale des nouvelles fonctionnalités introduites dans Android 5.0 (Lollipop). Ces fonctionnalités incluent un nouveau style d’interface utilisateur appelé thème de documents, ainsi que les nouvelles fonctionnalités de prise en charge tels que les animations, vue ombres et teintes drawable. Android 5.0 inclut également des notifications améliorées, deux nouveaux widgets d’interface utilisateur, un nouveau Planificateur de travaux et un certain nombre de nouvelles API pour améliorer le stockage, de mise en réseau, de connectivité et de fonctionnalités multimédias."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 24d85d7be580f8db8621d91ebbb27c0b7881b4eb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="lollipop-features"></a>Fonctionnalités de l’interface Lollipop

_Cet article fournit une vue d’ensemble générale des nouvelles fonctionnalités introduites dans Android 5.0 (Lollipop). Ces fonctionnalités incluent un nouveau style d’interface utilisateur appelé thème de documents, ainsi que les nouvelles fonctionnalités de prise en charge tels que les animations, vue ombres et teintes drawable. Android 5.0 inclut également des notifications améliorées, deux nouveaux widgets d’interface utilisateur, un nouveau Planificateur de travaux et un certain nombre de nouvelles API pour améliorer le stockage, de mise en réseau, de connectivité et de fonctionnalités multimédias._

## <a name="lollipop-overview"></a>Vue d’ensemble de l’interface Lollipop

Android 5.0 (Lollipop) introduit un nouveau langage de conception, *documents de conception*, et avec lui un prenant en charge effectué de nouvelles fonctionnalités pour créer des applications plus facile et intuitif à utiliser. Avec les documents de conception, Android 5.0 fournit non seulement les téléphones Android actualisation ; Il prévoit également un nouvel ensemble de règles de conception ou de tablettes Android, ordinateurs de bureau, surveille et télévisions intelligentes. Ces règles de création de mettre en évidence la simplicité et minimalism tout en rendant utilisation familiers attributs tactiles (par exemple, les signaux surface et bord réalistes) pour aider les utilisateurs et de comprendre intuitivement l’interface.

*Thème matière* est l’incarnation de ces principes de conception de l’interface utilisateur dans Android. Cet article commence par traitant des fonctionnalités de prise en charge du thème du matériel :

-   **Animations** &ndash; *Touch commentaires* animations, *transition de l’activité* animations, *afficher la transition d’état* des animations et un *révéler effet*.

-   **Afficher des ombres et élévation** &ndash; vues ont maintenant un `elevation` propriété ; les vues avec supérieur `elevation` converties ombres supérieure sur l’arrière-plan.

-   **Fonctionnalités de couleur** &ndash; *teintes Drawable* permet de réutiliser des composants de l’image en modifiant leur couleur, et *extraction de la couleur principale* vous permet de manière dynamique thème de votre application en fonction de couleurs dans une image.

Le thème matériel de nombreuses fonctionnalités sont déjà intégrées à l’interface utilisateur d’Android 5.0 expérience, tandis que d’autres doivent être ajoutés explicitement aux applications. Par exemple, certaines vues standard (tels que les boutons) inclure tactile commentaires animations, tandis que les applications doivent activer la plupart des ombres de vue.

Outre les améliorations de l’interface utilisateur liées via le thème de matériel, Android 5.0 inclut également plusieurs autres nouvelles fonctionnalités qui sont traitées dans cet article :

-   **Amélioré des notifications** &ndash; des Notifications dans Android 5.0 ont été considérablement mis à jour avec le nouvel aspect de la prise en charge pour les notifications de l’écran de verrouillage et un nouveau *frontal* format de présentation de notification.

-   **Nouveaux widgets d’interface utilisateur** &ndash; nouveau `RecyclerView` widget rend plus facile pour les applications avec grands jeux de données et des informations complexes et la nouvelle `CardView` widget fournit un format de présentation de type carte simplifiée pour afficher du texte et images.

-   **Nouvelles API** &ndash; Android 5.0 ajoute les nouvelles API pour la charge de plusieurs réseaux, amélioration connectivité Bluetooth, la gestion de stockage plus facile et un contrôle plus souple de lecteurs multimédias et les appareils photo. Un nouveau travail de fonctionnalité de planification est disponible pour exécuter des tâches de façon asynchrone à des heures planifiées. Cette fonctionnalité contribue à améliorer l’autonomie par, par exemple, planification des tâches d’avoir lieu lorsque l’appareil est connecté et chargement.


## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités Android 5.0 dans les applications Xamarin :

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac. 

-   **Kit de développement logiciel Android** &ndash; Android 5.0 (API 21) ou version ultérieure doit être installé via le Gestionnaire de kit de développement logiciel Android.

-   **Kit de développement Java** &ndash; Xamarin.Android requiert [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) ou une version ultérieure si vous développez pour le niveau de l’API 24 ou supérieure (JDK 1.8 prend également en charge les niveaux d’API antérieures à 24, y compris l’interface Lollipop). La version 64 bits de JDK 1.8 est requise si vous utilisez des contrôles personnalisés ou l’aperçu des formulaires.

Vous pouvez continuer à utiliser [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) si vous êtes le développement en particulier pour les API de niveau 23 ou une version antérieure.


<a name="settingup" />

## <a name="setting-up-an-android-50-project"></a>Configuration d’un projet 5.0 Android

Pour créer un projet Android 5.0, vous devez installer les outils les plus récents et les packages de kit de développement logiciel. Utilisez les étapes suivantes pour configurer un projet Xamarin.Android ciblant Android 5.0 :

1. Installez les outils de Xamarin.Android et activer votre licence Xamarin. Consultez [le programme d’installation et Installation](~/android/get-started/installation/index.md) pour plus d’informations sur l’installation de Xamarin.Android.

2. Si vous utilisez Visual Studio pour Mac, installez les dernières mises à jour Android 5.0.

3. Démarrez le Gestionnaire de SDK Android (dans Visual Studio pour Mac, utilisez **outils &gt; Open Android SDK Manager&hellip;**) et installer les outils de kit de développement logiciel Android 23.0.5 ou version ultérieure :

    [![Sélection d’outils du SDK Android dans le Gestionnaire de kit de développement logiciel Android.](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png)

   En outre, installez les derniers packages Android 5.0 SDK (API 21 ou version ultérieure) :

    [![Kit de développement logiciel Android 5.0 lors de l’installation des packages dans le Gestionnaire de kit de développement logiciel Android.](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png)

   Pour plus d’informations sur l’utilisation du Gestionnaire de SDK Android, consultez [Gestionnaire du SDK](http://developer.android.com/tools/help/sdk-manager.html).

4. Créer un nouveau projet Xamarin.Android. Si vous ne connaissez pas le développement Android avec Xamarin, consultez [Hello, Android](~/android/get-started/hello-android/index.md) pour en savoir plus sur la création de projets Android. Lorsque vous créez un projet Android, veillez à configurer les paramètres de version pour Android 5.0.
   Dans Visual Studio pour Mac, accédez à **Options du projet &gt; générer &gt; général** et **framework cible** à **Android 5.0 (Lollipop)** ou plus tard :

    ![Définition de la cible Framwework sur Android Lollipop 5.0](lollipop-images/target-framework.png)

   Sous **Options du projet &gt; générer &gt; Application Android**, valeur minimale et cibler la version Android à **automatique - version du framework cible utilisation**:

    ![Définir les versions minimale et cible Android sur automatique](lollipop-images/minimum-android-version.png)

5. Configurer un émulateur ou sur un appareil Android pour tester votre application. Si vous utilisez un émulateur, consultez [le programme d’installation d’émulateur Android](~/android/get-started/installation/android-emulator/index.md) pour savoir comment configurer un émulateur Android pour une utilisation avec Xamarin Studio ou Visual Studio. Si vous utilisez un appareil Android, consultez [paramètre le SDK d’aperçu](https://developer.android.com/preview/setup-sdk.html) pour savoir comment mettre à jour votre appareil pour Android 5.0. Pour configurer votre appareil Android pour exécuter et déboguer des applications de Xamarin.Android, consultez [définir appareil pour le développement](~/android/get-started/installation/set-up-device-for-development.md).

Remarque : Si vous mettez à jour un projet Android existant qui a été ciblant la version d’évaluation L Android, vous devez mettre à jour le **Framework cible** et **Android version** aux valeurs décrites ci-dessus.


<a name="changes" />

## <a name="important-changes"></a>Modifications importantes

Précédemment publié des applications Android pourrait être affectées par les modifications Android 5.0. Android 5.0 utilise en particulier, une nouvelle exécution et un format de notification de modification de manière significative.

<a name="runtime" />

### <a name="android-runtime"></a>Runtime Android

Android 5.0 utilise le Runtime Android nouvelle (image) en tant que le runtime par défaut au lieu de Dalvik. ART implémente plusieurs nouvelles fonctionnalités principales :

-   **Compilation de transfert de temps (AOA)** &ndash; AOA peut améliorer les performances de l’application à compiler le code de l’application avant le premier lancement de l’application. Quand une application est installée, ART génère une application compilée exécutable pour le périphérique cible.

-   **Améliorer le garbage collection (GC)** &ndash; améliorations dans le catalogue global dans les graphiques peuvent également améliorer les performances de l’application. Le garbage collection maintenant utilise une pause de catalogue global au lieu de deux, et effectuer des opérations simultanées de catalogue global en temps voulu plus.

-   **Amélioration du débogage de l’application** &ndash; ART fournit plus de détails diagnostic à l’aide de l’analyse des exceptions et de rapports.

Les applications existantes doivent fonctionner sans modification sous ART &ndash; à l’exception des applications qui exploitent les techniques propres à l’exécution de Dalvik précédente, ce qui peut ne pas fonctionner sous ART. Pour plus d’informations sur ces modifications, consultez [vérification du comportement des applications sur le Runtime Android (ART)](http://developer.android.com/guide/practices/verifying-apps-art.html).

<a name="notifchanges" />

### <a name="notification-changes"></a>Modifications de notification

Les notifications ont considérablement évolué dans Android 5.0 :

-   **Sons et vibration sont gérées différemment** &ndash; des sons de Notification et vibrations sont désormais gérées par `Notification.Builder` au lieu de `Ringtone`, `MediaPlayer`, et `Vibrator`.

-   **Nouveau jeu de couleurs** &ndash; conformément aux documents de thème, les notifications sont rendues avec texte foncé dans un arrière-plan blanc ou très légère. En outre, les canaux alpha dans les icônes de notification peuvent être modifiées par Android pour la coordination avec les jeux de couleurs système. 

-   **Les notifications de l’écran de verrouillage** &ndash; les Notifications peuvent s’affichent désormais sur le périphérique l’écran de verrouillage.

-   **Frontal** &ndash; des notifications de haute priorité apparaissent désormais dans une petite fenêtre flottante (notification frontal) lorsque l’appareil est déverrouillé et que l’écran est activée.

Dans la plupart des cas, le portage de fonctionnalité de notification d’application existant pour Android 5.0 implique les étapes suivantes :

1.  Convertir votre code d’utiliser `Notification.Builder` (ou `NotificationsCompat.Builder`) pour créer des notifications. 

2.  Vérifiez que vos ressources de notification existant sont visibles dans le nouveau jeu de couleurs de thème du matériel.

3.  Décidez quels visibilité vos notifications doivent avoir lorsqu’ils sont présentés sur l’écran de verrouillage. Si une notification n’est pas publique, contenu qui doit s’afficher sur l’écran de verrouillage ?

4.  Définir la catégorie des notifications de sorte qu’ils sont gérés correctement dans le nouveau 5.0 Android *ne pas perturber* mode.

Si vos notifications présentent les contrôles de transport, affichage état de lecture, utilisez `RemoteControlClient`, ou appelez `ActivityManager.GetRecentTasks`, consultez [importantes modifications de comportement](http://developer.android.com/preview/api-overview.html#Behaviors) pour plus d’informations sur la mise à jour de vos notifications pour Android 5.0.

Pour plus d’informations sur la création de notifications dans Android, consultez [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md). Le [compatibilité](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) section de cet article explique comment créer des notifications vers le bas compatibles avec les versions antérieures d’Android.

<a name="materialtheme" />

## <a name="material-theme"></a>Thème matière

Le nouveau thème de matériel Android 5.0 apporte des modifications à l’apparence de l’interface utilisateur Android. Éléments visuels à présent utilisent des surfaces tactiles prennent sur le graphique en gras et la typographie couleurs vives de structure d’impression. Exemples de documents de thème sont représentées dans les captures d’écran suivants :

[![Captures d’écran de l’écran d’accueil du thème matériel, l’écran d’applications et écran de paramètre](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png)

Android 5.0 vous accueille l’écran d’accueil qui apparaît sur la gauche. La capture d’écran du centre est le premier écran de la liste des applications, et la capture d’écran de droite est le **paramètres** écran. Google [documents de conception](https://material.io/guidelines/material-design/introduction.html) spécification explique les règles de conception sous-jacent derrière le nouveau concept de thème du matériel.

Thème matière inclut trois types intégrés que vous pouvez utiliser dans votre application : le `Theme.Material` (la valeur par défaut), le thème sombre le `Theme.Material.Light` thème et le `Theme.Material.Light.DarkActionBar` thème : 

[![Thèmes des captures d’écran de foncé, clair et DarkActionBar](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png)

Pour plus d’informations sur l’utilisation des fonctionnalités de thème du matériel dans les applications de Xamarin.Android, consultez [matériel thème](~/android/user-interface/material-theme.md).

<a name="animations" />

## <a name="animations"></a>Animations

Android 5.0 fournit tactile commentaires animations, les animations de transition activité et animations de transition d’état vue pour rendre les interfaces d’application plus intuitive à utiliser. En outre, les applications Android 5.0 peuvent utiliser *révéler effet* animations pour masquer ou afficher des vues. Vous pouvez utiliser *courbé mouvement* pour configurer la vitesse à laquelle les paramètres ou les animations à variation lente sont rendues.

<a name="touchanim" />

### <a name="touch-feedback-animations"></a>Animations de commentaires tactile

Animations de commentaires tactile fournissent aux utilisateurs une rétroaction visuelle lorsqu’une vue a été modifiée. Par exemple, boutons affichent désormais un effet papillon quand ils sont touchées &ndash; Voici l’animation de commentaires par défaut tactile dans Android 5.0. Animation d’ondulation est implémentée par la nouvelle `RippleDrawable` classe. Cet effet peut être configuré pour se terminer dans les limites de la vue ou étendre au-delà des limites de la vue. Par exemple, la séquence suivante de captures d’écran illustre l’effet d’ondulation dans un bouton au cours de l’animation tactile :

![Frame par des captures d’écran de frame d’animation ripple sur un bouton](lollipop-images/touch-animation.png)

Contact contact initial avec le bouton se produit dans la première image sur la gauche, tandis que la séquence restante (de gauche à droite) illustre la façon dont les répercussions s’étend vers le bord du bouton. Lors de l’animation ripple se termine, la vue retourne à son apparition d’origine. L’animation d’ondulation par défaut a lieu dans une fraction de seconde, mais la longueur de l’animation peut être personnalisée pour les longueurs longues ou plus courtes durée.

Pour plus d’informations sur touchent les animations de commentaires dans Android 5.0, consultez [personnaliser les commentaires Touch](http://developer.android.com/training/material/animations.html#Touch).

<a name="activityanim" />

### <a name="activity-transition-animations"></a>Animations de Transition d’activité

Animations de transition d’activité permettent aux utilisateurs d’une idée de la continuité des activités visual lorsqu’une activité passe à un autre. Les applications peuvent spécifier trois types d’animations de transition :

-   **Entrez la transition** &ndash; pour laquelle une activité entre dans la scène.

-   **Quitter la transition** &ndash; pour lorsqu’une activité termine la scène.

-   **Une transition de l’élément partagée** &ndash; pour lorsqu’une vue qui est commune aux deux activités change à mesure que des première transitions d’activité à la suivante.

Par exemple, la séquence de captures d’écran suivante illustre une transition de l’élément partagé :

[![Frame par frame les captures d’écran des animations de transition d’un élément partagé](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png)

Un élément partagé (il s’agit d’une photo d’un caterpillar) est un des plusieurs vues dans la première activité ; Il s’agrandit pour devenir le seul affichage dans la deuxième activité en tant que les première transitions d’activité à la seconde.

#### <a name="enter-transition-animation-types"></a>Entrez les Types d’animations de Transition

Pour les transitions de l’entrée, Android 5.0 fournit trois types d’animations :

-   **Éclater animation** &ndash; agrandit une vue à partir du centre de la scène.

-   **Faites glisser l’animation** &ndash; déplace une vue d’un des bords de la scène.

-   **Animation de fondu** &ndash; en fondu une vue dans la scène.

#### <a name="exit-transition-animation-types"></a>Types d’animations de Transition de sortie

Pour quitter les transitions, Android 5.0 fournit trois types d’animations :

-   **Éclater animation** &ndash; réduit d’une vue au centre de la scène.

-   **Faites glisser l’animation** &ndash; déplace une vue vers l’un des bords de la scène.

-   **Animation de fondu** &ndash; en fondu une vue de la scène.

#### <a name="shared-element-transition-animation-types"></a>Élément Transition Animation Types partagés

Transitions de l’élément partagé prennent en charge plusieurs types d’animations, telles que :

-   Modifier les limites de mise en page ou l’image d’une vue.

-   Modification de la mise à l’échelle et la rotation d’une vue.

-   Modification du type de taille et l’échelle pour une vue.

Pour plus d’informations sur les animations de transition d’activité dans Android 5.0, consultez [personnaliser les Transitions activité](http://developer.android.com/training/material/animations.html#Transitions).

<a name="viewstate" />

### <a name="view-state-transition-animations"></a>Animations de Transition d’état d’affichage

Android 5.0 rend possible pour les animations à exécuter lorsque l’état d’une vue change. Vous pouvez animer les transitions d’état de vue en utilisant l’une des techniques suivantes :

-   Créer drawables qui animer les changements d’état associés à une vue. La nouvelle `AnimatedStateListDrawable` classe vous permet de créer des drawables qui affichent des animations entre les changements d’état de vue.

-   Définir les fonctionnalités d’animation qui s’exécute lorsque l’état d’une vue est modifiée. La nouvelle `StateListAnimator` classe vous permet de définir une animation qui s’exécute lorsque l’état d’une vue est modifiée.

Pour plus d’informations sur les animations de transition d’état view dans Android 5.0, consultez [animer les modifications d’affichage état](http://developer.android.com/training/material/animations.html#ViewState).

<a name="reveal" />

### <a name="reveal-effect"></a>Révéler effet

Le *révéler effet* est un cercle de découpage que radius de modifications pour afficher ou masquer une vue. Vous pouvez contrôler cet effet en définissant l’initial et final rayon du cercle de découpage. La séquence de captures d’écran suivante illustre une animation d’effet révéler à partir du centre de l’écran :

[![Frame par frame les captures d’écran de révéler animation](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png)

La séquence suivante illustre une animation d’effet révéler qui a lieu à partir de l’angle inférieur gauche de l’écran :

[![Frame par des captures d’écran de frame d’animation de découpage](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png)

Afficher les animations peuvent être inversées ; Autrement dit, le cercle de découpage peut réduire pour masquer la vue au lieu agrandir pour afficher la vue.

Pour plus d’informations sur l’effet de révéler Android 5.0 dans, consultez [utilisation de l’effet de révéler](http://developer.android.com/training/material/animations.html#Reveal).

<a name="curvedmotion" />

### <a name="curved-motion"></a>Courbe de mouvement

En plus de ces fonctionnalités d’animation, Android 5.0 fournit également les nouvelles API qui vous permettent de spécifier les courbes de temps et des mouvements d’animations. Android 5.0 utilise ces courbes pour interpoler temporel et spatial déplacement au cours des animations. Trois courbes sont définies dans Android 5.0 :

-   **Fast\_hors\_linéaire\_dans** &ndash; accélère rapidement et se poursuit accélérer jusqu'à la fin de l’animation.

-   **Fast\_hors\_lente\_dans** &ndash; accélère rapidement et lentement ralentit vers la fin de l’animation.

-   **Linéaire\_hors\_lente\_dans** &ndash; commence avec une rapidité maximale et lentement ralentit à la fin de l’animation.

Vous pouvez utiliser la nouvelle `PathInterpolator` classe pour spécifier comment une interpolation de mouvement a lieu. `PathInterpolator` est un interpolateur qui traverse les chemins d’accès de l’animation en fonction des points de contrôle spécifiés et les courbes de mouvement. Pour plus d’informations sur la façon de spécifier les paramètres de la courbe de mouvement dans Android 5.0, consultez [mouvement de courbe utilisation](http://developer.android.com/training/material/animations.html#CurvedMotion).

<a name="viewshadows" />

## <a name="view-shadows--elevation"></a>Vue ombres & élévation

Dans Android 5.0, vous pouvez spécifier le *élévation* d’une vue en définissant un nouveau `Z` propriété. Une meilleure `Z` valeur provoque l’affichage à une ombre plus volumineux sur l’arrière-plan, qui effectue la vue s’affichent en float supérieure au-dessus de l’arrière-plan. Vous pouvez définir l’élévation initiale d’une vue en configurant ses `elevation` attribut dans la disposition.

L’exemple suivant illustre les ombres par vide `TextView` contrôler lorsque son attribut élévation a la valeur 2dp, 4dp et 6dp, respectivement :

[![Afficher les captures d’écran de progessively supérieure ombres](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png)

Afficher les paramètres de cliché instantané peuvent être statiques (comme indiqué ci-dessus) ou être utilisés dans les animations pour afficher une vue d’augmenter temporairement au-dessus de l’arrière-plan. Vous pouvez utiliser la `ViewPropertyAnimator` classe pour animer l’élévation d’une vue. L’élévation d’une vue est la somme de sa disposition `elevation` paramètre plus une `translationZ` propriété que vous pouvez définir via un `ViewPropertyAnimator` appel de méthode.

Pour plus d’informations sur les ombres de vue dans Android 5.0, consultez [définissant les ombres et les vues de découpage](http://developer.android.com/training/material/shadows-clipping.html).

<a name="colorfeatures" />

## <a name="color-features"></a>Fonctionnalités de couleur

Android 5.0 fournit deux nouvelles fonctionnalités de gestion des couleurs dans les applications :

-   *Teintes drawable* vous permet de modifier les couleurs des composants de l’image en modifiant un attribut de disposition.

-   *Extraction de la couleur principale* permet de personnaliser de manière dynamique le thème de couleur de votre application pour se coordonner avec la palette de couleurs d’une image affichée.

<a name="tinting" />

### <a name="drawable-tinting"></a>Teintes drawable

5.0 dispositions Android reconnaissent un nouveau `tint` attribut que vous pouvez utiliser pour définir la couleur de drawables sans avoir à créer plusieurs versions de ces ressources pour afficher des couleurs différentes. Pour utiliser cette fonctionnalité, vous définissez une image bitmap en tant que masque alpha et utilisez le `tint` attribut pour définir la couleur de l’élément multimédia. Cela rend possible pour vous permettre de créer des composants d’une seule fois et les couleurs dans votre disposition pour correspondre à votre thème.

Dans l’exemple suivant, une seule ressource &ndash; un logo blanc avec un arrière-plan transparent &ndash; est utilisé pour créer des variations de teinte :

![Logo blanc Xamarin avec un arrière-plan transparent](lollipop-images/xamarin-logo-white.png)

Ce logo s’affiche au-dessus d’un cercle bleu comme indiqué dans les exemples suivants. L’image de gauche est comment le logo apparaît sans une `tint` paramètre. Dans le centre, le logo de l’image `tint` attribut est défini sur un gris foncé. Dans l’image sur la droite, `tint` est définie sur un gris clair :

![Exemples du logo ci-dessus avec les paramètres de teinte différents](lollipop-images/drawable-tinting.png)

Pour plus d’informations sur les teintes drawable dans Android 5.0, consultez [teintes Drawable](http://developer.android.com/training/material/drawables.html#DrawableTint).

<a name="colorextract" />

### <a name="prominent-color-extraction"></a>Extraction de la couleur principale

Le nouveau 5.0 Android `Palette` classe vous permet d’extraire des couleurs à partir d’une image afin que vous puissiez les appliquer dynamiquement à une palette de couleurs personnalisées. La `Palette` classe extrait les six couleurs d’une image et des étiquettes de ces couleurs en fonction de leurs niveaux relatifs de saturation de la couleur et la luminosité :

-   Vibrant

-   Dark dynamique

-   Lumière dynamique

-   Mode Muet

-   Dark muet

-   Lumière muet

Par exemple, dans les captures d’écran ci-dessous, une application d’affichage de photos extrait les couleurs visibles à partir de l’image sur l’affichage et utilise ces couleurs pour adapter le jeu de couleurs de l’application correspond à l’image :

[![Captures d’écran de l’extraction de couleur de thème bleu, vert et rose](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png)

Dans les captures d’écran ci-dessus, la barre d’action est définie pour l’extrait « light vives » couleur et l’arrière-plan est défini sur extrait « vives foncé » couleur. Dans chaque exemple ci-dessus, une ligne des carrés de couleur small est incluse pour illustrer la palette de couleurs qui ont été extraites de l’image.

Pour plus d’informations sur l’extraction de la couleur dans Android 5.0, consultez [extraction des couleurs visible à partir d’une Image](http://developer.android.com/training/material/drawables.html#ColorExtract).

<a name="newuiwidgets" />

## <a name="new-ui-widgets"></a>Nouveaux Widgets d’interface utilisateur

Android 5.0 introduit deux nouveaux widgets d’interface utilisateur :

-   `RecyclerView` &ndash; Un groupe de vue qui affiche une liste d’éléments de défilement.

-   `CardView` &ndash; Disposition de base avec des angles arrondis.

Les deux widgets cuit dans prennent en charge les fonctionnalités de thème du matériel ; par exemple, `RecyclerView` utilise des animations pour ajouter et supprimer des vues, et `CardView` utilise afficher shadows pour afficher chaque carte flotter au-dessus de l’arrière-plan. Exemples de ces nouveaux widgets sont présentés dans les captures d’écran suivants :

[![Captures d’écran des applications générées avec RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png)

La capture d’écran de gauche est un exemple de `RecyclerView` qu’utilisée dans une application de messagerie et la capture d’écran sur la droite est un exemple de `CardView` comme utilisé dans une application de réservation de voyages.

<a name="recyclerview" />

### <a name="recyclerview"></a>RecyclerView

`RecyclerView` est semblable à `ListView,` , mais il est plus adapté pour les grands ensembles de vues ou les listes comportant des éléments qui changent de manière dynamique. Comme `ListView,` vous spécifiez un adaptateur pour accéder à l’ensemble de données sous-jacente. Cependant, contrairement à `ListView,` vous utilisez un *Gestionnaire de disposition* pour placer des éléments dans `RecyclerView`. Le Gestionnaire de disposition prend également en charge de recyclage de la vue ; Il gère la réutilisation des vues des éléments qui ne sont plus visibles par l’utilisateur.

Lorsque vous utilisez un `RecyclerView` widget, vous devez spécifier un `LayoutManager` et une carte. Comme indiqué dans cette figure, `LayoutManager` est intermédiaire entre l’adaptateur et le `RecyclerView`:

![Diagramme de RecyclerView avec prise en charge LayoutManager, l’adaptateur et Dataset](lollipop-images/recyclerview-diagram.png)

Les captures d’écran suivantes illustrent un `RecyclerView` qui contient 100 éléments (chaque élément se compose d’un `ImageView` et `TextView`) :

[![Captures d’écran d’une application RecyclerView défilement d’images](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png)

`RecyclerView` gère ce jeu de données volumineux en toute simplicité &ndash; le défilement à partir du début de la liste à la fin de la liste dans cet exemple, application prend quelques secondes seulement. `RecyclerView` prend également en charge les animations ; en fait, les animations pour ajouter et supprimer des éléments sont activées par défaut. Lorsqu’un élément est ajouté à un `RecyclerView`, il en fondu dans comme illustré dans cette séquence de captures d’écran :

[![Frame par frame capture d’écran d’une transition d’élément photo dans](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png)

Pour plus d’informations `RecyclerView`, consultez [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).

<a name="cardview" />

### <a name="cardview"></a>CardView

`CardView` est une vue simple qui simule une carte flottante avec des angles arrondis. Étant donné que `CardView` a ombres d’affichage intégré, il fournit un moyen facile d’ajouter la profondeur visuelle à votre application. Les captures d’écran suivantes illustrent trois orienté texte `CardView`:

[![Captures d’écran des applications à l’aide de RecyclerView avec CardView des éléments](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png)

Chacune des cartes dans l’exemple ci-dessus contient un `TextView`; la couleur d’arrière-plan est définie la `cardBackgroundColor` attribut.

Pour plus d’informations `CardView`, consultez [CardView](~/android/user-interface/controls/card-view.md).

<a name="enhanced" />

## <a name="enhanced-notifications"></a>Notifications améliorées

Le système de notification dans Android 5.0 a été considérablement mis à jour avec un nouveau format visuel et de nouvelles fonctionnalités. Les notifications ont une nouvelle apparence dans Android 5.0. Par exemple, les notifications dans Android 5.0 a maintenant utilisent texte foncé sur un arrière-plan clair :

![Exemple d’une notification d’Android 5.0 non développée](lollipop-images/expanded-notification-contracted.png)

Lorsqu’une grande icône est affichée dans une notification (comme indiqué dans l’exemple ci-dessus), Android 5.0 présente la petite icône sous la forme d’un badge sur l’icône de grande taille. 

Dans Android 5.0, les notifications peuvent également apparaître sur le périphérique l’écran de verrouillage.
Par exemple, voici une capture d’écran de l’exemple d’une écran de verrouillage avec une seule notification :

[![Capture d’écran de notification qui apparaissent sur l’écran de verrouillage](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png)

Les utilisateurs peuvent double-clic une notification sur l’écran de verrouillage pour déverrouiller l’appareil et accéder à l’application qui a créé cette notification, ou effectuez un balayage à ignorer les notifications. Les notifications ont un nouveau *visibilité* qui détermine la quantité de contenu peut être affiché sur l’écran de verrouillage. Les utilisateurs peuvent choisir s’il faut autoriser le contenu à afficher dans l’écran de verrouillage notifications sensibles.

Android 5.0 introduit un nouveau format de présentation de notification de haute priorité appelé *frontal*. Les notifications de tête haute glissement vers le bas à partir du haut de l’écran pendant quelques secondes et puis reformatage à la nuance de notification en haut de l’écran. Notifications de tête haute rendent possible pour le système de l’interface utilisateur pour placer des informations importantes devant l’utilisateur sans interrompre l’activité en cours d’exécution. L’exemple suivant illustre une notification frontal simple qui affiche sur une application :

[![Exemple d’une notification profondes](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png)

Notifications de tête haute sont généralement utilisées pour les événements suivants :

-   Un nouveau message suivant

-   Un appel téléphonique entrant

-   Indication de batterie faible

-   Une alarme

Android 5.0 affiche une notification au format de tête haute uniquement lorsqu’il a un paramètre de priorité élevée ou max.

Dans Android 5.0, vous pouvez fournir des métadonnées de notification pour aider à trier et afficher les notifications de manière plus intelligente Android. Android 5.0 organise les notifications en fonction de la priorité, visibilité et la catégorie.
Catégories de notification sont utilisés pour filtrer les notifications peuvent être présentées lors de l’appareil est en *ne pas perturber* mode.

Pour plus d’informations sur la création et le lancement des notifications avec les dernières fonctionnalités Android 5.0, consultez [Notifications Local](~/android/app-fundamentals/notifications/local-notifications.md).

<a name="newapis" />

## <a name="new-apis"></a>Nouvelles API

Outre les nouvelles fonctionnalités de l’apparence et convivialité décrites ci-dessus, Android 5.0 ajoute les nouvelles API qui étendent les fonctionnalités des éléments multimédias existants, de stockage et les fonctionnalités de connectivité sans fil. En outre, Android 5.0 inclut les nouvelles API qui prendre en charge d’une nouvelle fonctionnalité de planificateur de travaux.

### <a name="camera"></a>Appareil photo

Android 5.0 fournit plusieurs nouvelles API pour les fonctionnalités améliorées de la caméra. La nouvelle `Android.Hardware.Camera2` espace de noms inclut la fonctionnalité de l’accès aux appareils de caméra connectée à un appareil Android. En outre, `Android.Hardware.Camera2` modèles chaque appareil photo comme un pipeline : accepte une demande de capture, capture l’image et génère ensuite le résultat. Cette approche rend possible pour les applications en file d’attente de plusieurs demandes de capture sur un appareil photo.

Les API suivantes permettent ces nouvelles fonctionnalités :

-   `CameraManager.GetCameraIdList` &ndash; Vous permet d’accéder par programmation à des périphériques de webcam ; vous utilisez `CameraManager.OpenCamera` pour se connecter à un périphérique spécifique à la caméra.

-   `CameraCaptureSession` &ndash; Capture ou le flux d’images à partir de la caméra. Vous implémentez un `CameraCaptureSession.CaptureListener` permettant de gérer les nouveaux événements de capture d’image.

-   `CaptureRequest` &ndash; Définit les paramètres de capture.

-   `CaptureResult` &ndash; Présente les résultats d’une opération de capture d’image.

Pour plus d’informations sur l’appareil photo de nouvelles API dans Android 5.0, consultez [Media](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Lecture audio

Les mises à 5.0 jour Android la `AudioTrack` classe pour une meilleure lecture audio :

-   `ENCODING_PCM_FLOAT` &ndash; Configure `AudioTrack` pour accepter des données audio dans un format à virgule flottante pour une meilleure plage dynamique, une marge supérieure et une meilleure qualité (grâce à une plus grande précision). En outre, format à virgule flottante permet d’éviter l’extrait de l’audio.

-   `ByteBuffer` &ndash; Vous pouvez maintenant fournir des données audio à `AudioTrack` en tant que tableau d’octets.

-   `WRITE_NON_BLOCKING` &ndash; Cette option simplifie la mise en mémoire tampon et le multithreading pour certaines applications.

Pour plus d’informations `AudioTrack` améliorations dans Android 5.0, consultez [Media](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Contrôle de lecture de médias

Android 5.0 introduit la nouvelle `Android.Media.MediaController` classe, qui remplace `RemoteControlClient`. `Android.Media.MediaController` Fournit des API de contrôle de transport simplifiée et offre un contrôle de thread-safe de lecture en dehors du contexte de l’interface utilisateur. Les nouvelles API suivantes gérer le contrôle de transport :

-   `Android.Media.Session.MediaSession` &ndash; Une session de contrôle de support qui gère plusieurs contrôleurs. Vous appelez `MediaSession.GetSessionToken` pour demander un jeton que votre application utilise pour interagir avec la session.

-   `MediaController.TransportControls` &ndash; Commandes telles que le transport **lire**, **arrêter**, et **ignorer**.

En outre, vous pouvez utiliser la nouvelle `Android.App.Notification.MediaStyle` classe à associer une session de support avec un contenu riche notification (par exemple, l’extraction et l’affichage de pochette).

Pour plus d’informations sur les nouvelles fonctionnalités de contrôle de lecture multimédia dans Android 5.0, consultez [Media](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Stockage

Android 5.0 met à jour l’infrastructure d’accès de stockage pour permettre plus facilement aux applications de travailler avec des répertoires et des documents :

-   Pour sélectionner une sous-arborescence de répertoires, vous pouvez générer et envoyer un `Android.Intent.Action.OPEN_DOCUMENT_TREE` intention. Ce mode provoque le système afficher toutes les instances de fournisseur qui prend en charge la sélection de la sous-arborescence ; l’utilisateur parcourt ensuite et sélectionne un répertoire.

-   Pour créer et gérer des documents ou des répertoires n’importe où dans une sous-arborescence, vous utilisez la nouvelle `CreateDocument`, `RenameDocument`, et `DeleteDocument` méthodes de `DocumentsContract`.

-   Pour obtenir les chemins d’accès aux répertoires de support sur tous les périphériques de stockage partagé, vous appelez la nouvelle `Android.Content.Context.GetExternalMediaDirs` (méthode).

Pour plus d’informations sur les nouvelles API dans Android 5.0 de stockage, consultez [stockage](http://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Connectivité et sans fil

Android 5.0 ajoute les améliorations d’API suivantes pour la connectivité et sans fil :

-   Nouvelle *réseau multiples* API qui permettre à des applications rechercher et sélectionner des réseaux avec des capacités spécifiques avant d’établir une connexion.

-   Fonctionnalité de diffusion Bluetooth qui permet à un appareil Android 5.0 en tant que périphérique Bluetooth faible énergie.

-   Améliorations NFC qui le rendent plus facile d’utiliser les communications en champ proche des fonctionnalités pour le partage des données avec d’autres périphériques.

Pour plus d’informations sur la nouvelle sans fil et la connectivité API Android 5.0, consultez [sans fil et la connectivité](http://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Planification des tâches

Android 5.0 introduit un nouveau `JobScheduler` API qui peut aider les utilisateurs à réduire la décharge de la batterie en planifiant certaines tâches s’exécutent uniquement quand l’appareil est connecté et de chargement. Cette fonctionnalité du Planificateur de tâches peut également servir pour planifier une tâche à exécuter lorsque les conditions sont plus appropriées à cette tâche, telles que le téléchargement d’un fichier volumineux lorsque l’appareil est connecté via un réseau Wi-Fi au lieu d’un connexion réseau limitée.

Pour plus d’informations sur la nouvelle tâche de planification de l’API Android 5.0, consultez [planification des tâches](http://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Récapitulatif

Cet article a fourni une vue d’ensemble des nouvelles fonctionnalités importantes dans Android 5.0 pour les développeurs d’applications Xamarin.Android :

-   Thème matière

-   Animations

-   Affichage des ombres et l’élévation

-   Fonctionnalités de couleur, telles que des teintes drawable et mise en évidence de couleur d’extraction

-   La nouvelle `RecyclerView` et `CardView` widgets

-   Améliorations de la notification

-   Nouvelles API pour l’appareil photo, lecture audio, MCI, de stockage, de connectivité sans fil et planification des tâches

Si vous ne connaissez pas le développement Xamarin Android, lisez [le programme d’installation et Installation](~/android/get-started/installation/index.md) pour vous aider à prendre en main Xamarin.Android.
[Hello, Android](~/android/get-started/hello-android/index.md) est une excellente introduction pour apprendre à créer des projets Android.



## <a name="related-links"></a>Liens associés

- [L Android Developer Preview](http://developer.android.com/preview/index.html)
- [Obtenir le Kit de développement logiciel Android](https://developer.android.com/sdk/index.html#Other)
- [Conception de matériel](http://developer.android.com/preview/material/index.html)
- [Principes de conception de matériel](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
