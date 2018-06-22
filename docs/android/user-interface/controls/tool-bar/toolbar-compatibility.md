---
title: Compatibilité de la barre d’outils
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: c3418a007ded30175049e83d515f59d5134deee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768699"
---
# <a name="toolbar-compatibility"></a>Compatibilité de la barre d’outils


## <a name="overview"></a>Vue d'ensemble

Cette section explique comment utiliser `Toolbar` sur les versions antérieures à Android 5.0 Lollipop Android. Si votre application ne prend pas en charge les versions d’Android antérieure à Android 5.0, vous pouvez ignorer cette section. 

Étant donné que `Toolbar` fait partie de la bibliothèque de prise en charge v7 Android, il peut être utilisé sur les appareils exécutant Android 2.1 (API niveau 7) et versions ultérieures. Toutefois, le [bibliothèque de prise en charge Android v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet doit être installé et le code modifié afin qu’il utilise le `Toolbar` implémentation fournie dans cette bibliothèque. Cette section explique comment installer cette NuGet et modifier le **ToolbarFun** application à partir de [Ajout d’une seconde barre d’outils](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) afin qu’elle s’exécute sur les versions d’Android antérieures à la version 5.0 d’interface (lollipop).

Pour modifier une application pour utiliser la version AppCompat de barre d’outils : 

1.  Définir les versions minimale et cible Android pour l’application.

2.  Installez le Package NuGet de AppCompat.

3.  Utiliser un thème AppCompat au lieu d’un thème Android intégré.

4.  Modifier `MainActivity` afin qu’il les sous-classes `AppCompatActivity` plutôt que `Activity`. 

Chacune de ces étapes est expliquée en détail dans les sections suivantes.



## <a name="set-the-minimum-and-target-android-version"></a>Définir les valeurs minimale et la Version de la cible Android

Framework cible de l’application doit être définie au niveau d’API 21 ou supérieure, ou l’application se déploiera pas correctement. Si une erreur comme **aucun identificateur de ressource ne trouvée pour l’attribut 'tileModeX' dans le package 'android'** se produit lors du déploiement de l’application, il s’agit, car le Framework cible n’est pas défini sur **Android 5.0 (API niveau 21 - Lollipop)**  ou supérieur. 

Définir le Framework cible au niveau niveau API 21 ou supérieur et définir les paramètres de projet au niveau d’API Android pour la Version minimale Android qui est de l’application pour prendre en charge. Pour plus d’informations sur la définition des niveaux d’API Android, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md). Dans la `ToolbarFun` exemple, la Version Android minimale est définie à KitKat (4.4 au niveau de l’API). 


## <a name="install-the-appcompat-nuget-package"></a>Installez le Package NuGet de AppCompat

Ensuite, ajoutez le [bibliothèque de prise en charge Android v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) package au projet. Dans Visual Studio, cliquez sur **références** et sélectionnez **gérer les Packages NuGet...** . Cliquez sur **Parcourir** et recherchez **bibliothèque de prise en charge Android v7 AppCompat**. Sélectionnez **Xamarin.Android.Support.v7.AppCompat** et cliquez sur **installer**: 

[![Package de capture d’écran de V7 Appcompat sélectionné dans gérer les packages NuGet](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

Lorsque cette NuGet est installé, plusieurs autres packages NuGet sont également installés si ce n’est pas déjà présent (tel que **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, et **Xamarin.Android.Support.Vector.Drawable**). Pour plus d’informations sur l’installation des packages NuGet, consultez [procédure pas à pas :, y compris un NuGet dans votre projet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 


## <a name="use-an-appcompat-theme-and-toolbar"></a>Utiliser un thème de AppCompat et de la barre d’outils

La bibliothèque AppCompat est fourni avec plusieurs `Theme.AppCompat` thèmes qui peuvent être utilisés sur n’importe quelle version d’Android pris en charge par la bibliothèque AppCompat. Le `ToolbarFun` est dérivé de thème d’application exemple `Theme.Material.Light.DarkActionBar`, qui n’est pas disponible sur les versions Android antérieures à l’interface Lollipop. Par conséquent, `ToolbarFun` doivent être adaptés pour utiliser l’équivalent de AppCompat pour ce thème, `Theme.AppCompat.Light.DarkActionBar`. En outre, étant donné que `Toolbar` est non disponible sur les versions d’Android antérieure à l’interface Lollipop, nous devons utiliser la version AppCompat de `Toolbar`. Par conséquent, les dispositions doivent utiliser `android.support.v7.widget.Toolbar` au lieu de `Toolbar`. 


### <a name="update-layouts"></a>Dispositions de mise à jour

Modifier **Resources/layout/Main.axml** et remplacez le `Toolbar` élément avec le code XML suivant : 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Modifier **Resources/layout/toolbar.xml** et remplacez son contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Notez que la `?attr` valeurs ne sont plus portent le préfixe `android:` (n’oubliez pas que le `?` notation fait référence à une ressource dans le thème actuel). Si `?android:attr` toujours utilisés ici, Android devez référence à la valeur d’attribut à partir de la plateforme en cours d’exécution, plutôt qu’à partir de la bibliothèque AppCompat. Étant donné que cet exemple utilise le `actionBarSize` définies par la bibliothèque AppCompat, le `android:` préfixe est supprimé. De même, `@android:style` est remplacée par `@style` afin que le `android:theme` attribut est défini pour un thème dans la bibliothèque AppCompat &ndash; le `ThemeOverlay.AppCompat.Dark.ActionBar` thème est utilisé ici au lieu de `ThemeOverlay.Material.Dark.ActionBar`. 


### <a name="update-the-style"></a>Mettre à jour le Style

Modifier **Resources/values/styles.xml** et remplacez son contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Les noms d’élément et le thème de parent dans cet exemple n’est plus portent le préfixe `android:` , car nous utilisons la bibliothèque AppCompat. En outre, le thème de parent est modifié à la version AppCompat de `Light.DarkActionBar`. 



### <a name="update-menus"></a>Mettre à jour des Menus

Pour prendre en charge les versions antérieures d’Android, la bibliothèque AppCompat utilise des attributs personnalisés qui reflètent les attributs de la `android:` espace de noms. Toutefois, certains attributs (telles que la `showAsAction` attribut utilisé dans le `<menu>` balise) n’existent pas dans le framework Android sur les anciens appareils &ndash; `showAsAction` a été introduit dans 11 d’API Android, mais n’est pas disponible dans les 7 API Android. Pour cette raison, un espace de noms personnalisé doit être utilisé pour préfixer tous les attributs définis par la bibliothèque de prise en charge. Dans les fichiers de ressources de menu, un espace de noms appelé `local` est défini pour préfixer la `showAsAction` attribut. 

Modifier **Resources/menu/top_menus.xml** et remplacez son contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Le `local` espace de noms est ajouté à cette ligne :

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

Le `showAsAction` attribut est précédé `local:` espace de noms plutôt que `android:` 

```csharp
local:showAsAction="ifRoom"
```

De même, modifier **Resources/menu/edit_menus.xml** et remplacez son contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Comment ce commutateur d’espace de noms assure une prise en charge pour le `showAsAction` attribut sur les versions antérieures à 11 de niveau d’API Android ? L’attribut personnalisé `showAsAction` et toutes ses valeurs possibles sont incluses dans l’application lors de l’installation de AppCompat NuGet. 


## <a name="subclass-appcompatactivity"></a>Sous-classe AppCompatActivity

L’étape finale de la conversion consiste à modifier `MainActivity` afin qu’il soit une sous-classe de `AppCompactActivity`. Modifier **MainActivity.cs** et ajoutez le code suivant `using` instructions : 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Cela déclare `Toolbar` à la version AppCompat de `Toolbar`. Ensuite, modifiez la définition de classe `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Pour définir la barre d’action à la version AppCompat de `Toolbar`, remplacez l’appel à `SetActionBar` avec `SetSupportActionBar`. Dans cet exemple, le titre est également modifié pour indiquer que la version AppCompat de `Toolbar` est utilisé :

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Enfin, modifiez le niveau Minimum Android à la valeur de l’interface Lollipop avant doit être pris en charge (par exemple, API 19). 

Générez l’application et l’exécuter sur un périphérique d’interface (lollipop) avant ou l’émulateur Android. La capture d’écran suivante montre la version AppCompat de **ToolbarFun** sur 4 Nexus KitKat (API 19) en cours d’exécution : 

[![Capture d’écran complète de l’application en cours d’exécution sur un appareil KitKat, deux barres d’outils sont affichés.](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

Lorsque la bibliothèque AppCompat est utilisée, thèmes est inutile de passer en fonction de la version Android &ndash; la bibliothèque AppCompat permet de fournir une expérience utilisateur cohérente sur toutes les versions d’Android pris en charge. 




## <a name="related-links"></a>Liens associés

- [La barre d’outils de l’interface Lollipop (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [La barre d’outils de AppCompat (exemple)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
