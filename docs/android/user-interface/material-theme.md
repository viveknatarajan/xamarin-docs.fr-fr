---
title: Thème matériau
description: Comment le thème de votre application Xamarin.Android avec le thème matériau
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 4b9c39a0ced9a264f501d78142c3bdfd556593ed
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "30771318"
---
# <a name="material-theme"></a>Thème matériau

*Thème matériau* est un style d’interface utilisateur qui détermine l’apparence des vues et des activités en commençant par Android 5.0 (Lollipop). Thème matériau est intégré à Android 5.0, il est utilisé par l’interface utilisateur du système ainsi que par les applications. Thème matériau n’est pas un « thème » dans le sens d’une option de l’apparence de l’échelle du système qu’un utilisateur peut choisir dynamiquement à partir d’un menu de paramètres. Au lieu de cela, le thème matériau peut être considéré comme un ensemble de styles de base intégrés connexes que vous pouvez utiliser pour personnaliser l’apparence de votre application.

Android fournit trois styles de thème de matériel :

-  `Theme.Material` &ndash; Version sombre du thème de matériel ; Il s’agit de la version par défaut dans Android 5.0.

-  `Theme.Material.Light` &ndash; Version légère du matériel de thème.

-  `Theme.Material.Light.DarkActionBar` &ndash; Version légère de thème de matériau, mais avec une barre d’action foncé.

Exemples de ces versions de thème de matériel sont affichés ici :

[![Captures d’écran du thème foncé, le thème clair et thème de la barre d’Action foncé](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Vous pouvez dériver de thème matériau pour créer votre propre thème, substituant certains ou tous les attributs de couleur. Par exemple, vous pouvez créer un thème qui dérive de `Theme.Material.Light`, mais se substitue à la couleur de barre d’application pour faire correspondre la couleur de votre marque. Vous pouvez également appliquer un style vues individuelles ; par exemple, vous pouvez créer un style pour [CardView](~/android/user-interface/controls/card-view.md) qui a des angles arrondis plus et qui utilise une couleur d’arrière-plan plus sombre.

Vous pouvez utiliser un thème unique pour une application entière, ou vous pouvez utiliser différents thèmes pour différents écrans (activités) dans une application. Dans les captures d’écran ci-dessus, par exemple, une seule application utilise un autre thème pour chaque activité pour illustrer les jeux de couleurs intégrée. Cases d’option Basculer l’application vers différentes activités et, par conséquent, affichent les différents thèmes.

Thème de matériel est pris en charge uniquement sur Android 5.0 et versions ultérieures, vous ne pouvez pas utiliser (ou un thème personnalisé dérivé de matériel de thème) au thème de votre application pour l’exécution sur des versions antérieures d’Android. Toutefois, vous pouvez configurer votre application pour utiliser le thème de matériel sur les appareils Android 5.0 et normalement revenir à un thème antérieures lorsqu’elle s’exécute sur les versions antérieures d’Android (consultez le [compatibilité](#compatibility) section de cet article pour plus d’informations).


## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour utiliser les nouvelles fonctionnalités de thème de matériel Android 5.0 dans les applications basées sur Xamarin :

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 ou version ultérieure doit être installé et configuré avec Visual Studio ou Visual Studio pour Mac. 

-  **Kit de développement logiciel Android** &ndash; Android 5.0 (API 21) ou version ultérieure doit être installé via le gestionnaire Android SDK.

-  **Java JDK 1.8** &ndash; JDK 1.7 peut être utilisé si vous êtes spécifiquement niveau d’API ciblé 23 et versions antérieur. JDK 1.8 est disponible à partir de [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Pour savoir comment configurer un projet d’application Android 5.0, consultez [paramètre configurer un projet Android 5.0](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>À l’aide de ces thèmes

Pour utiliser le thème de matériel le plus simple consiste à configurer votre application pour utiliser un thème intégré sans personnalisation. Si vous ne souhaitez pas configurer explicitement un thème, votre application par défaut `Theme.Material` (le thème sombre). Si votre application n'a qu’une seule activité, vous pouvez configurer un thème au niveau de l’application. Si votre application comporte plusieurs activités, vous pouvez configurer un thème au niveau de l’application afin qu’elle utilise le même thème entre toutes les activités, ou vous pouvez affecter différents thèmes à différentes activités. Les sections suivantes expliquent comment configurer des thèmes au niveau de l’application et au niveau des activités.


### <a name="theming-an-application"></a>Une Application de thèmes

Pour configurer une application entière à utiliser une version de thème de matériau, définissez le `android:theme` attribut du nœud d’application dans **AndroidManifest.xml** à une des opérations suivantes :

-  `@android:style/Theme.Material` &ndash; Thème sombre.

-  `@android:style/Theme.Material.Light` &ndash; Thème clair.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Thème clair avec la barre d’action foncé.

L’exemple suivant configure l’application *MyApp* pour utiliser le thème clair :

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternativement, vous pouvez définir l’application `Theme` attribut **AssemblyInfo.cs** (ou **Properties.cs**). Exemple :

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Quand le thème de l’application est défini sur `@android:style/Theme.Material.Light`, chaque activité dans *MyApp* s’affichera à l’aide de `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Une activité de thèmes

À thème une activité, vous ajoutez un `Theme` affectant le `[Activity]` attribut au-dessus de la déclaration de votre activité et affecter `Theme` à la version de thème de matériel que vous souhaitez utiliser. Les thèmes d’exemple suivant une activité avec `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Autres activités dans cette application utilisera la valeur par défaut `Theme.Material` modèle de couleurs sombres (ou, si configuré, le paramètre de thème d’application).

<a name="customtheme" />

## <a name="using-custom-themes"></a>À l’aide des thèmes personnalisés

Vous pouvez améliorer votre marque en créant un thème personnalisé qui définit le style de votre application avec votre marque&rsquo;couleurs de s. Pour créer un thème personnalisé, vous définissez un nouveau style qui dérive d’une version de thème de matériel intégrée, en substituant les attributs de couleur que vous souhaitez modifier. Par exemple, vous pouvez définir un thème personnalisé qui dérive de `Theme.Material.Light.DarkActionBar` et modifie la couleur d’arrière-plan de l’écran à beige au lieu de blanc.

Thème matériau expose les attributs de disposition suivants pour la personnalisation :

-  `colorPrimary` &ndash; Couleur de la barre des applications.

-  `colorPrimaryDark` &ndash; La couleur de la barre d’état et les barres de l’application contextuelles ; Il s’agit généralement d’une version sombre de `colorPrimary`.

-  `colorAccent` &ndash; Couleur de contrôles d’interface utilisateur tels que les cases à cocher, cases d’option et zones de texte d’édition.

-  `windowBackground` &ndash; La couleur de l’arrière-plan de l’écran.

-  `textColorPrimary` &ndash; La couleur du texte de l’interface utilisateur dans la barre des applications.

-  `statusBarColor` &ndash; Couleur de la barre d’état.

-  `navigationBarColor` &ndash; Couleur de la barre de navigation.

Ces zones d’écran sont étiquetées dans le diagramme suivant :

[![Diagramme des attributs et leurs zones d’écran associé](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Par défaut, `statusBarColor` est défini sur la valeur de `colorPrimaryDark`. Vous pouvez définir `statusBarColor` à une couleur unie, ou vous pouvez le définir sur `@android:color/transparent` pour rendre la barre d’état transparent. La barre de navigation peut également être rendue transparente en définissant `navigationBarColor` à `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Création d’un thème d’application personnalisée

Vous pouvez créer un thème de l’application personnalisée en créant et modifiant des fichiers dans le **ressources** dossier de votre projet d’application. Pour appliquer un style votre application avec un thème personnalisé, utilisez les étapes suivantes :

-   Créer un **colors.xml** fichier **ressources/valeurs** &mdash; ce fichier vous permet de définir les couleurs de thème personnalisé. Par exemple, vous pouvez coller le code suivant dans **colors.xml** pour vous aider à bien démarrer :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Modifier cet exemple de fichier pour définir les noms et les codes de couleur pour les ressources de couleur que vous utiliserez dans votre thème personnalisé.

-   Créer un **ressources/valeurs-v21** dossier. Dans ce dossier, créez un **styles.xml** fichier :

    [![Emplacement de styles.xml dans le dossier de ressources/valeurs-21.xml](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Notez que **ressources/valeurs-v21** est spécifique à Android 5.0 &ndash; des versions antérieures d’Android ne lira pas les fichiers dans ce dossier.

-   Ajouter un `resources` nœud **styles.xml** et définir un `style` nœud portant le nom de votre thème personnalisé. Par exemple, voici une **styles.xml** fichier qui définit *MyCustomTheme* (dérivée d’intégrés `Theme.Material.Light` style de thème) :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   À ce stade, une application qui utilise *MyCustomTheme* affichera le stock `Theme.Material.Light` thème sans personnalisations :

    [![Apparence de thème personnalisé avant les personnalisations](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Ajouter des personnalisations de couleur à **styles.xml** en définissant les couleurs des attributs de mise en page que vous souhaitez modifier. Par exemple, pour modifier la couleur de barre d’application à `my_blue` et modifier la couleur des contrôles d’IU à `my_purple`, ajouter la couleur des remplacements afin de **styles.xml** qui font référence aux ressources de couleur configurés dans **colors.xml**:

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

Avec ces modifications en place, une application qui utilise *MyCustomTheme* affichera une couleur de barre d’application dans `my_blue` et contrôles d’interface utilisateur dans `my_purple`, mais utiliser le `Theme.Material.Light` couleurs partout ailleurs :

[![Apparence de thème personnalisé après les personnalisations](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

Dans cet exemple, *MyCustomTheme* emprunte des couleurs à partir de `Theme.Material.Light` pour l’arrière-plan de couleur, barre d’état et les couleurs de texte, mais il modifie la couleur de la barre d’application pour `my_blue` et définit la couleur du bouton radio `my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Création d’un Style d’affichage personnalisé

Android 5.0 rend également possible pour vous permettre d’appliquer un style une vue spécifique. Une fois que vous créez **colors.xml** et **styles.xml** (comme décrit dans la section précédente), vous pouvez ajouter un style d’affichage à **styles.xml**.
Pour appliquer un style une vue spécifique, utilisez les étapes suivantes :

-   Modifier **Resources/values-v21/styles.xml** et ajoutez un `style` nœud portant le nom de votre style de la vue personnalisée. Définir les attributs de couleur personnalisée pour votre vue dans cette `style` nœud. Par exemple, pour créer un personnalisé [CardView](~/android/user-interface/controls/card-view.md) style qui a plus des angles arrondis et utilise `my_blue` en tant que la couleur d’arrière-plan de la carte, ajouter un `style` nœud **styles.xml** (à l’intérieur de la `resources`nœud) et configurer le rayon du coin et la couleur d’arrière-plan :

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   Dans votre disposition, définissez la `style` attribut pour cette vue doit correspondre au nom de style personnalisé que vous avez choisi à l’étape précédente. Exemple :

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

La capture d’écran suivante fournit un exemple de la valeur par défaut `CardView` (indiqué sur la gauche) par rapport à un `CardView` qui a été mis en forme avec personnalisé `CardView.MyBlue` thème (illustré à droite) :

[![Exemples de défaut CardView et widgets CardView personnalisé](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

Dans cet exemple, personnalisé `CardView` s’affiche avec la couleur d’arrière-plan `my_blue` et un rayon de l’angle 18dp.


## <a name="compatibility"></a>Compatibilité

Pour appliquer un style votre application afin qu’il utilise le thème matériau sur Android 5.0 mais revient automatiquement à un style compatible vers le bas sur les versions antérieures d’Android, utilisez les étapes suivantes :

-   Définir un thème personnalisé dans **Resources/values-v21/styles.xml** qui dérive d’un style de thème du matériau. Exemple :

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Définir un thème personnalisé dans **Resources/values/styles.xml** qui dérive d’un thème plus anciens, mais utilise le même nom de thème comme indiqué ci-dessus. Exemple :

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Dans **AndroidManifest.xml**, configurez votre application avec le nom de thème personnalisé. 
    Exemple :

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternativement, vous pouvez appliquer un style une activité spécifique à l’aide de votre thème personnalisé :

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Si votre thème utilise des couleurs définies dans un **colors.xml** de fichier, veillez à placer ce fichier dans **ressources/valeurs** (au lieu de **ressources/valeurs-v21**) afin que les deux versions de votre thème personnalisé peut accéder à vos définitions de couleur.

Lorsque votre application s’exécute sur un appareil Android 5.0, il utilisera la définition de thème spécifiée dans **Resources/values-v21/styles.xml**. Lorsque cette application s’exécute sur les anciens appareils Android, il sera automatiquement revenir à la définition de thème spécifiée dans **Resources/values/styles.xml**.

Pour plus d’informations sur la compatibilité de thème avec les versions antérieures d’Android, consultez [autres ressources](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Récapitulatif

Cet article a présenté le nouveau style d’interface utilisateur matériau thème inclus dans Android 5.0 (Lollipop). Il décrit les trois versions matériau thème intégrées que vous pouvez utiliser pour votre application de style, vous avez appris comment créer un thème personnalisé pour la personnalisation de votre application, et il a fourni un exemple de procédure à thème une vue spécifique. Enfin, cet article a expliqué comment utiliser le thème de documents dans votre application tout en conservant une compatibilité descendante avec les versions antérieures d’Android.



## <a name="related-links"></a>Liens associés

- [ThemeSwitcher (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Présentation d’interface (lollipop)](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [Autres ressources](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android Lollipop](https://developer.android.com/about/versions/lollipop)
- [Développeurs Android à secteurs](https://developer.android.com/about/versions/pie/)
- [Conception matérielle](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [Principes de conception matérielle](https://material.io/design/)
- [Conservation de la compatibilité](https://developer.android.com/training/backward-compatible-ui/)
