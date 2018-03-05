---
title: CardView
description: "Le widget Cardview est un composant de l’interface utilisateur qui présente le contenu de texte et image dans les affichages qui ressemblent à des cartes. Ce guide explique comment utiliser et personnaliser CardView dans les applications de Xamarin.Android, tout en conservant une compatibilité descendante avec les versions antérieures d’Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: b8f643c8158c5a3a849a3d8ee3dd8d0e7e30addf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="cardview"></a>CardView

_Le widget Cardview est un composant de l’interface utilisateur qui présente le contenu de texte et image dans les affichages qui ressemblent à des cartes. Ce guide explique comment utiliser et personnaliser CardView dans les applications de Xamarin.Android, tout en conservant une compatibilité descendante avec les versions antérieures d’Android._

<a name="overview" />

## <a name="overview"></a>Vue d'ensemble

Le `Cardview` widget, introduite dans Android 5.0 (Lollipop), est un composant de l’interface utilisateur qui présente le contenu de texte et image dans les affichages qui ressemblent à des cartes. `CardView` est implémenté comme un `FrameLayout` widget avec des angles arrondis et une ombre. En règle générale, un `CardView` est utilisé pour présenter un élément de ligne unique dans un `ListView` ou `GridView` afficher le groupe. Par exemple, la capture d’écran suivante est un exemple d’une application de réservation de voyages qui implémente `CardView`-en fonction de déplacement des cartes de destination dans un défilement `ListView`:

![Exemple d’application à l’aide d’un objet CardView pour chaque destination de déplacement](card-view-images/01-cardview-example.png)

Ce guide explique comment ajouter la `CardView` projet de package pour votre Xamarin.Android, comment ajouter `CardView` à votre disposition et comment personnaliser l’apparence de `CardView` dans votre application. En outre, ce guide fournit une liste détaillée des `CardView` les attributs que vous pouvez modifier, y compris les attributs pour vous aider à utiliser `CardView` sur les versions antérieures à Android 5.0 Lollipop Android.

<a name="requirements" />

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les nouvelles Android 5.0 et les fonctionnalités de versions ultérieures (y compris `CardView`) dans les applications Xamarin :

-  **

-  **

-  **Java JDK 1.8** &ndash; JDK 1.7 peut être utilisé si vous n’êtes plus précisément au niveau du ciblage API 23 et les versions antérieur. JDK 1.8 est disponible à partir de [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Votre application doit également inclure le `Xamarin.Android.Support.v7.CardView` package. Pour ajouter le `Xamarin.Android.Support.v7.CardView` package dans Visual Studio pour Mac :

1. Ouvrez votre projet, cliquez sur **Packages** et sélectionnez **ajouter des Packages**.

2. Dans le **ajouter des Packages** boîte de dialogue, recherchez **CardView**.

3. Sélectionnez **bibliothèque de prise en charge de Xamarin v7 CardView**, puis cliquez sur **ajouter un Package**.

Pour ajouter le `Xamarin.Android.Support.v7.CardView` package dans Visual Studio :

1. Ouvrez votre projet, cliquez sur le **références** nœud (dans le **l’Explorateur de solutions** volet) et sélectionnez **gérer les Packages NuGet...** .

2. Lorsque le **gérer les Packages NuGet** boîte de dialogue s’affiche, entrez **CardView** dans la zone de recherche.

3. Lorsque **bibliothèque de prise en charge de Xamarin v7 CardView** s’affiche, cliquez sur **installer**.

Pour savoir comment configurer un projet d’application Android 5.0, consultez [paramètre configurer un projet Android 5.0](~/android/platform/lollipop.md).
Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

<a name="basic" />

## <a name="introducing-cardview"></a>Présentation de CardView

La valeur par défaut `CardView` ressemble à une carte blanche avec des angles arrondis au minimum et une ombre légère. L’exemple suivant **Main.axml** disposition affiche une seule `CardView` widget contenant un `TextView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Si vous utilisez ce code XML pour remplacer le contenu existant de **Main.axml**, veillez à placer en commentaire une partie du code **MainActivity.cs** qui fait référence à des ressources dans le document XML précédent.

Cet exemple de disposition crée une valeur par défaut `CardView` avec une seule ligne de texte, comme illustré dans la capture d’écran suivante :

[![Capture d’écran de CardView avec arrière-plan blanc et de la ligne de texte](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png)

Dans cet exemple, le style de l’application est défini pour le matériel de thème clair (`Theme.Material.Light`) afin que le `CardView` les bords et les ombres sont plus faciles à voir. Pour plus d’informations sur les applications de thèmes Android 5.0, consultez [matériel thème](~/android/user-interface/material-theme.md). Dans la section suivante, vous allez apprendre comment personnaliser `CardView` pour une application.

<a name="customizing" />

## <a name="customizing-cardview"></a>Personnalisation CardView

Vous pouvez modifier le basic `CardView` attributs pour personnaliser l’apparence de la `CardView` dans votre application. Par exemple, d’élever le `CardView` peut être augmenté pour une ombre plus volumineux (ce qui rend la carte semble float supérieure au-dessus de l’arrière-plan). En outre, le rayon d’angle peut être augmenté pour rendre les angles de la carte plus arrondis.

Dans l’exemple suivant de disposition, personnalisé `CardView` permet de créer une simulation d’une photo impression (un « instantané »). Un `ImageView` est ajouté à la `CardView` pour l’affichage de l’image et un `TextView` se trouve sous le `ImageView` pour afficher le titre de l’image.
Dans cet exemple de disposition, la `CardView` a les personnalisations suivantes :

-  Le `cardElevation` est augmentée à 4dp à une ombre plus volumineux.
-  Le `cardCornerRadius` est augmentée à 5dp pour afficher les angles arrondis plus.

Étant donné que `CardView` est fournie par la bibliothèque de prise en charge v7 Android, ses attributs ne sont pas disponibles à partir de la `android:` espace de noms. Par conséquent, vous devez définir votre propre espace de noms XML et utiliser cet espace de noms comme le `CardView` préfixe de l’attribut. Dans l’exemple de disposition ci-dessous, nous allons utiliser cette ligne pour définir un espace de noms appelé `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Vous pouvez appeler cet espace de noms `card_view` ou même `myapp` si vous choisissez (il est accessible uniquement dans la portée de ce fichier). Tout ce que vous choisissez d’appeler cet espace de noms, vous devez l’utiliser comme préfixe pour le `CardView` attribut que vous souhaitez modifier. Dans cet exemple de disposition, la `cardview` espace de noms est le préfixe pour `cardElevation` et `cardCornerRadius`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Lorsque cet exemple de disposition est utilisé pour afficher une image dans une application de visualisation photo, la `CardView` a l’apparence d’un instantané de la photo, comme illustré dans la capture d’écran suivante :

[![CardView avec une image et la légende sous l’image](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png)

Cette capture d’écran provient de la [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) exemple d’application qui utilise un `RecyclerView` widget pour présenter une liste déroulante de `CardView` images pour l’affichage des photos. Pour plus d’informations sur `RecyclerView`, consultez la [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) guide.

Notez qu’un `CardView` peut afficher plusieurs enfants dans sa zone de contenu. Par exemple, dans l’exemple d’application d’affichage de photos ci-dessus, la zone de contenu se compose d’un `ListView` qui contient un `ImageView` et un `TextView`. Bien que `CardView` instances sont souvent verticalement, vous pouvez également organiser horizontalement (consultez [création d’un Style d’affichage personnalisée](~/android/user-interface/material-theme.md#customview) pour une capture d’écran).

<a name="layout" />

### <a name="cardview-layout-options"></a>Options de mise en page CardView

`CardView` mises en page peuvent être personnalisés en définissant un ou plusieurs attributs qui affectent son remplissage, élévation, rayon d’angle et la couleur d’arrière-plan :

[![Diagramme d’attributs de CardView](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png)

Chaque attribut peut également être modifié de manière dynamique en appelant un équivalent `CardView` (méthode) (pour plus d’informations sur `CardView` méthodes, consultez le [référence de classe CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Notez que ces attributs (à l’exception de la couleur d’arrière-plan) acceptent une valeur de la dimension, qui est un nombre décimal suivi de l’unité. Par exemple, `11.5dp` spécifie 11,5 indépendant de la densité de pixels.

<a name="padding" />

#### <a name="padding"></a>Padding
`
CardView` propose cinq attributs de remplissage pour positionner le contenu dans la carte. Vous pouvez les définir dans votre mise en forme XML, ou vous pouvez appeler des méthodes analogues dans votre code :

[![Diagramme de CardView attributs de marge intérieure](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png)

Les attributs de marge intérieure sont présentent comme suit :

-  `contentPadding` &ndash; Interne remplissage entre les vues enfants de la `CardView` et tous les bords de la carte.

-  `contentPaddingBottom` &ndash; Interne remplissage entre les vues enfants de le `CardView` et le bord inférieur de la carte.

-  `contentPaddingLeft` &ndash; Interne remplissage entre les vues enfants de le `CardView` et le bord gauche de la carte.

-  `contentPaddingRight` &ndash; Interne remplissage entre les vues enfants de le `CardView` et le bord droit de la carte.

-  `contentPaddingTop` &ndash; Interne remplissage entre les vues enfants de le `CardView` et le bord supérieur de la carte.

Attributs de marge intérieure de contenu sont par rapport à la limite de la zone de contenu plutôt qu’à toute donnée widget que qui se trouve dans la zone de contenu.
Par exemple, si `contentPadding` suffisamment une augmentation dans l’application de visualisation photo, le `CardView` serait rogner l’image et le texte affiché sur la carte.


<a name="elevation" />

#### <a name="elevation"></a>Élévation

`CardView` offre deux attributs d’élévation pour contrôler son élévation et, par conséquent, la taille de son ombre :

[![Diagramme des attributs d’élévation CardView](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png)

Les attributs de l’élévation sont présentent comme suit :

-  `cardElevation` &ndash; D’élever le `CardView` (représente son axe Z).

-  `cardMaxElevation` &ndash; La valeur maximale de la `CardView`d’élévation.

Des valeurs plus élevées de `cardElevation` augmenter la taille de l’ombre pour rendre `CardView` semble float supérieure au-dessus de l’arrière-plan. Le `cardElevation` attribut détermine également l’ordre de dessin de chevauchement des vues ; autrement dit, la `CardView` seront dessinés sous une autre vue qui se chevauche avec une valeur plus élevée de l’élévation et au-dessus de toutes les vues qui se chevauchent avec une faible valeur d’élévation.
Le `cardMaxElevation` paramètre est utile pour lorsque votre application modifie l’élévation dynamiquement &ndash; il empêche l’ombre de l’extension au-delà de la limite que vous définissez ce paramètre.

<a name="radius" />

#### <a name="corner-radius-and-background-color"></a>Rayon de l’angle et la couleur d’arrière-plan

`CardView` offre d’attributs que vous pouvez utiliser pour contrôler son rayon d’angle et sa couleur d’arrière-plan. Ces deux propriétés vous permettent de modifier le style de la `CardView`:

[![Diagramme de l’angle CardView radious et les attributs de couleur d’arrière-plan](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png)

Ces attributs sont présentent comme suit :

-  `cardCornerRadius` &ndash; Rayon de l’angle de tous les angles de la `CardView`.

-  `cardBackgroundColor` &ndash; La couleur d’arrière-plan de la `CardView`.

Dans ce diagramme, `cardCornerRadius` est définie sur une 10dp plus arrondi et `cardBackgroundColor` a la valeur `"#FFFFCC"` (jaune clair).


<a name="compatibility" />

## <a name="compatibility"></a>Compatibilité

Vous pouvez utiliser `CardView` sur les versions antérieures à Android 5.0 Lollipop Android. Étant donné que `CardView` fait partie de la bibliothèque de prise en charge v7 Android, vous pouvez utiliser `CardView` avec Android 2.1 (API niveau 7) et versions ultérieures.
Toutefois, vous devez installer le `Xamarin.Android.Support.v7.CardView` package comme décrit dans [exigences](#requirements), ci-dessus.

`CardView` a un comportement légèrement différent sur les appareils avant de l’interface Lollipop (niveau 21 d’API) :

-  `CardView` utilise une implémentation de la programmation de clichés instantanés qui ajoute le remplissage supplémentaire.

-  `CardView` ne détoure pas les vues enfants qui croisent la `CardView`de l’arrondi des angles.

Pour aider à gérer ces différences de compatibilité `CardView` fournit plusieurs attributs supplémentaires que vous pouvez configurer dans votre disposition :

-   `cardPreventCornerOverlap` &ndash; Définissez cet attribut sur `true` pour ajouter une marge intérieure lorsque votre application s’exécute sur les versions antérieures Android (API niveau 20 et versions antérieur). Ce paramètre empêche `CardView` contenu en intersection avec le `CardView`de l’arrondi des angles.

-   `cardUseCompatPadding` &ndash; Définissez cet attribut sur `true` pour ajouter une marge intérieure lorsque votre application est en cours d’exécution dans les versions d’Android à ou supérieur au niveau 21 d’API. Si vous souhaitez utiliser `CardView` sur les appareils Lollipop préliminaire et qu’il l’identiques sur l’interface Lollipop (ou version ultérieure), définissez cet attribut sur `true`. Lorsque cet attribut est activé, `CardView` ajoute le remplissage supplémentaire pour dessiner des ombres lorsqu’il s’exécute sur les appareils avant l’interface Lollipop. Cela permet de résoudre les différences de remplissage qui sont introduits lors de l’ombre de programmation Lollipop préliminaire implémentations est en vigueur.

Pour plus d’informations sur la compatibilité avec les versions antérieures d’Android de maintenance, consultez [en conservant une compatibilité](https://developer.android.com/training/material/compatibility.html).

<a name="summary" />

## <a name="summary"></a>Récapitulatif

Ce guide a introduit la nouvelle `CardView` widget inclus dans Android 5.0 (Lollipop). Il démontré la valeur par défaut `CardView` apparence et expliqué comment personnaliser `CardView` en modifiant son élévation, un arrondi, remplissage de contenu et la couleur d’arrière-plan. Il apparaît la `CardView` les attributs de mise en page (avec les diagrammes de référence) et explique comment utiliser `CardView` sur les appareils Android antérieures à Android 5.0 Lollipop. Pour plus d’informations sur `CardView`, consultez la [référence de classe CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html).


## <a name="related-links"></a>Liens associés

- [RecyclerView (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Introduction à l’interface Lollipop](~/android/platform/lollipop.md)
- [Référence de classe CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
