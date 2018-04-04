---
title: Thème matière
description: Comment le thème de votre application de Xamarin.Android avec le matériel de thème
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a3b5f908330833a38aad9e329835a4a437fc29f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="material-theme"></a>Thème matière

*Thème matière* est un style d’interface utilisateur qui détermine l’apparence des vues et des activités en commençant par Android 5.0 (Lollipop). Thème matière est intégré dans Android 5.0, afin qu’il est utilisé par le système de l’interface utilisateur, ainsi que par des applications. Thème matière n’est pas un « thème » dans le sens d’une option de l’apparence de l’échelle du système qu’un utilisateur peut choisir dynamiquement à partir d’un menu Paramètres. Au lieu de cela, le thème de matériel peut être considéré comme un ensemble de styles de base intégrés connexes que vous pouvez utiliser pour personnaliser l’apparence de votre application.

Android fournit trois styles de thème du matériel :

-  `Theme.Material` &ndash; Version foncée de thème du matériel ; Il s’agit de la version par défaut dans Android 5.0.

-  `Theme.Material.Light` &ndash; Version légère de matériel de thème.

-  `Theme.Material.Light.DarkActionBar` &ndash; Version légère de thème de matériel, mais avec une barre d’action sombre.

Exemples de ces versions de thème du matériel sont affichés ici :

[![Captures d’écran du thème foncé, le thème clair et thème sombre Action barre](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Vous pouvez dériver de thème de matériel pour créer votre propre thème, substituant certains ou tous les attributs de couleur. Par exemple, vous pouvez créer un thème qui dérive de `Theme.Material.Light`, mais se substitue à la couleur de barre d’application pour faire correspondre la couleur de votre image de marque. Vous pouvez également appliquer un style des vues ; par exemple, vous pouvez créer un style pour [CardView](~/android/user-interface/controls/card-view.md) qui a des angles arrondis plus et qui utilise une couleur d’arrière-plan plus sombre.

Vous pouvez utiliser un thème unique pour une application entière, ou vous pouvez utiliser différents thèmes pour différents écrans (activités) dans une application. Dans les captures d’écran ci-dessus, par exemple, une seule application utilise un thème différent pour chaque activité pour illustrer les jeux de couleurs intégrée. Cases d’option Basculer de l’application pour différentes activités et, par conséquent, affichent différents thèmes.

Thème du matériel est pris en charge uniquement sur Android 5.0 et versions ultérieures, vous ne pouvez pas utiliser (ou un thème personnalisé dérivé de thème du matériel) au thème votre application pour l’exécution sur des versions antérieures d’Android. Toutefois, vous pouvez configurer votre application pour utiliser le thème de la documentation sur les appareils Android 5.0 et en douceur revient à un thème antérieur lorsqu’il s’exécute sur des versions antérieures d’Android (consultez la [compatibilité](#compatibility) section de cet article pour plus d’informations).


## <a name="requirements"></a>Spécifications

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités de thème de matériel Android 5.0 dans les applications Xamarin :

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac. 

-  **Kit de développement logiciel Android** &ndash; Android 5.0 (API 21) ou version ultérieure doit être installé via le Gestionnaire de kit de développement logiciel Android.

-  **Java JDK 1.8** &ndash; JDK 1.7 peut être utilisé si vous n’êtes plus précisément au niveau du ciblage API 23 et les versions antérieur. JDK 1.8 est disponible à partir de [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Pour savoir comment configurer un projet d’application Android 5.0, consultez [paramètre configurer un projet Android 5.0](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>À l’aide des thèmes intégrés

À utiliser le thème de matériel, le plus simple consiste à configurer votre application pour utiliser un thème intégré sans personnalisation. Si vous ne souhaitez pas configurer explicitement un thème, votre application par défaut est `Theme.Material` (thème foncé). Si votre application n'a qu’une seule activité, vous pouvez configurer un thème au niveau de l’application. Si votre application a plusieurs activités, vous pouvez configurer un thème au niveau de l’application afin qu’il utilise le même thème sur toutes les activités, ou vous pouvez attribuer différents thèmes pour différentes activités. Les sections suivantes expliquent comment configurer des thèmes au niveau de l’application et au niveau de l’activité.


### <a name="theming-an-application"></a>Une Application de thèmes

Pour configurer un ensemble de l’application pour utiliser une version de thème de matériel, définissez la `android:theme` attribut du nœud dans l’application **AndroidManifest.xml** à une des opérations suivantes :

-  `@android:style/Theme.Material` &ndash; Thème sombre.

-  `@android:style/Theme.Material.Light` &ndash; Thème clair.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Thème clair avec barre d’action sombre.

L’exemple suivant configure l’application *MyApp* pour utiliser le thème clair :

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Ou bien, vous pouvez définir l’application `Theme` attribut **AssemblyInfo.cs** (ou **Properties.cs**). Par exemple :

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Lorsque le thème de l’application a la valeur `@android:style/Theme.Material.Light`, chaque activité dans *MyApp* s’affichera à l’aide de `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Une activité de thèmes

Thème une activité, vous ajoutez un `Theme` à la `[Activity]` attribut au-dessus de la déclaration de votre activité et affecter `Theme` à la version de thème de matériel que vous souhaitez utiliser. Les thèmes suivants exemple une activité avec `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Autres activités dans cette application utilisera la valeur par défaut `Theme.Material` couleurs sombres (ou, si configurés, le paramètre de thème d’application).

<a name="customtheme" />

## <a name="using-custom-themes"></a>À l’aide des thèmes personnalisés

Vous pouvez améliorer votre marque en créant un thème personnalisé que les styles de votre application avec votre marque&rsquo;couleurs de s. Pour créer un thème personnalisé, vous définissez un style qui dérive d’une version de thème du matériel intégrée, en substituant les attributs de couleur que vous souhaitez modifier. Par exemple, vous pouvez définir un thème personnalisé qui dérive de `Theme.Material.Light.DarkActionBar` et modifie la couleur d’arrière-plan à beige blanc à la place.

Thème matière expose les attributs de disposition suivants pour la personnalisation :

-  `colorPrimary` &ndash; Couleur de la barre des applications.

-  `colorPrimaryDark` &ndash; La couleur de la barre d’état et les barres de l’application contextuelles ; Il s’agit normalement une version foncée de `colorPrimary`.

-  `colorAccent` &ndash; La couleur des contrôles d’interface utilisateur tels que les cases à cocher, cases d’option et modifier des zones de texte.

-  `windowBackground` &ndash; La couleur de l’arrière-plan de l’écran.

-  `textColorPrimary` &ndash; La couleur du texte de l’interface utilisateur dans la barre des applications.

-  `statusBarColor` &ndash; Couleur de la barre d’état.

-  `navigationBarColor` &ndash; Couleur de la barre de navigation.

Ces zones de l’écran sont étiquetées dans le diagramme suivant :

[![Diagramme des attributs et leurs zones d’écran associé](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Par défaut, `statusBarColor` est définie à la valeur de `colorPrimaryDark`. Vous pouvez définir `statusBarColor` une couleur unie, ou vous pouvez le définir sur `@android:color/transparent` pour rendre la barre d’état transparente. La barre de navigation permettre également être transparentes en définissant `navigationBarColor` à `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Création d’un thème d’application personnalisée

Vous pouvez créer un thème d’application personnalisée en créant et en modifiant les fichiers dans le **ressources** dossier de votre projet d’application. Pour appliquer un style votre application avec un thème personnalisé, procédez comme suit :

-   Créer un **colors.xml** fichier **ressources/valeurs** &mdash; ce fichier vous permet de définir les couleurs de thème personnalisé. Par exemple, vous pouvez coller le code suivant dans **colors.xml** pour vous aider à démarrer :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Modifiez cet exemple de fichier pour définir les noms et les codes de couleur pour les ressources de couleur que vous utiliserez dans votre thème personnalisé.

-   Créer un **valeurs/ressources-v21** dossier. Dans ce dossier, créez un **styles.xml** fichier :

    [![Emplacement de styles.xml dans le dossier de ressources/valeurs-21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Notez que **valeurs/ressources-v21** est spécifique à Android 5.0 &ndash; des versions antérieures d’Android ne lira pas les fichiers dans ce dossier.

-   Ajouter un `resources` nœud **styles.xml** et définir un `style` nœud portant le nom de votre thème personnalisé. Par exemple, voici une **styles.xml** fichier qui définit *MyCustomTheme* (dérivée de la fonction intégrée `Theme.Material.Light` style de thème) :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   À ce stade, une application qui utilise *MyCustomTheme* affichera le stock `Theme.Material.Light` thème sans personnalisation :

    [![Apparence de thème personnalisé avant les personnalisations](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Ajouter des personnalisations de couleur pour **styles.xml** en définissant les couleurs des attributs de mise en page que vous souhaitez modifier. Par exemple, pour modifier la couleur de barre d’application pour `my_blue` et modifiez la couleur des contrôles d’interface utilisateur à `my_purple`, ajouter la couleur des remplacements afin de **styles.xml** qui font référence aux ressources de couleur configurés dans **colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

Avec ces modifications en place, une application qui utilise *MyCustomTheme* affichera une couleur de barre d’application dans `my_blue` et contrôles d’interface utilisateur dans `my_purple`, mais utilisez le `Theme.Material.Light` couleurs partout ailleurs :

[![Apparence de thème personnalisé après les personnalisations](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

Dans cet exemple, *MyCustomTheme* emprunte les couleurs de `Theme.Material.Light` pour l’arrière-plan de couleur, barre d’état et les couleurs de texte, mais il modifie la couleur de la barre d’application pour `my_blue` et définit la couleur du bouton radio `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Création d’un Style d’affichage personnalisé

Android 5.0 rend également possible pour vous permettre de style d’une vue individuelle. Après avoir créé **colors.xml** et **styles.xml** (comme décrit dans la section précédente), vous pouvez ajouter un style d’affichage pour **styles.xml**.
Pour appliquer un style une vue individuelle, procédez comme suit :

-   Modifier **Resources/values-v21/styles.xml** et ajoutez un `style` nœud portant le nom de votre style de la vue personnalisée. Définir les attributs de couleur personnalisée pour l’affichage dans cette `style` nœud. Par exemple, pour créer un personnalisé [CardView](~/android/user-interface/controls/card-view.md) style qui a plus des angles arrondis et utilise `my_blue` comme couleur d’arrière-plan de carte, ajouter un `style` nœud **styles.xml** (à l’intérieur de la `resources`nœud) et configurer le rayon de coin et la couleur d’arrière-plan :

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   Dans la mise en page, définissez la `style` attribut pour cette vue pour correspondre au nom de style personnalisé que vous avez choisi à l’étape précédente. Par exemple :

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

La capture d’écran suivante fournit un exemple de la valeur par défaut `CardView` (indiqué sur la gauche) par rapport à un `CardView` qui a été mis en forme avec personnalisé `CardView.MyBlue` thème (affiché sur la droite) :

[![Exemples de valeur par défaut CardView et personnalisé CardView](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

Dans cet exemple, personnalisé `CardView` s’affiche avec la couleur d’arrière-plan `my_blue` et un rayon d’angle 18dp.


## <a name="compatibility"></a>Compatibilité

Pour appliquer un style votre application afin qu’il utilise le thème du matériel sur Android 5.0 mais revient automatiquement à un style vers le bas compatible sur les anciennes versions d’Android, procédez comme suit :

-   Définir un thème personnalisé dans **Resources/values-v21/styles.xml** qui dérive d’un style de thème du matériel. Par exemple :

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Définir un thème personnalisé dans **Resources/values/styles.xml** qui dérive d’un thème plus anciens, mais utilise le même nom de thème comme indiqué ci-dessus. Par exemple :

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Dans **AndroidManifest.xml**, configurez votre application avec le nom du thème personnalisé. 
    Par exemple :

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Ou bien, vous pouvez appliquer un style une activité spécifique à l’aide de votre thème personnalisé :

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Si votre thème utilise des couleurs sont définies dans un **colors.xml** de fichiers, veillez à placer ce fichier dans **ressources/valeurs** (au lieu de **valeurs/ressources-v21**) afin que les deux versions de votre thème personnalisé peut accéder à vos définitions de couleur.

Lorsque votre application s’exécute sur un appareil Android 5.0, il utilise la définition de thème spécifiée dans **Resources/values-v21/styles.xml**. Lorsque cette application s’exécute sur les anciens appareils Android, il automatiquement revient à la définition de thème spécifiée dans **Resources/values/styles.xml**.

Pour plus d’informations sur la compatibilité de thème avec les anciennes versions d’Android, consultez [autres ressources](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Récapitulatif

Cet article a introduit le nouveau style d’interface utilisateur matériau thème inclus dans Android 5.0 (Lollipop). Il décrit les trois versions matériau thème intégrées que vous pouvez utiliser pour définir le style de votre application, il explique comment créer un thème personnalisé pour la personnalisation de votre application et il fourni un exemple de thème une vue individuelle. Enfin, cet article a expliqué comment utiliser le thème de documents dans votre application tout en conservant une compatibilité descendante avec les versions antérieures d’Android.



## <a name="related-links"></a>Liens associés

- [ThemeSwitcher (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Introduction à l’interface Lollipop](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [Autres ressources](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [L Android Developer Preview](http://developer.android.com/preview/index.html)
- [Conception de matériel](http://developer.android.com/preview/material/index.html)
- [Principes de conception de matériel](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [Maintenir la compatibilité](http://developer.android.com/preview/material/compatibility.html)
