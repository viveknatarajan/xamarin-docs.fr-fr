---
title: Ajout de AppCompat et conception matérielle
description: Cet article explique comment convertir des applications Android de Xamarin.Forms existantes pour utiliser AppCompat et conception de matériau.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: c2eed44a7c684b91ceed4493a83ff3b4e1578b5f
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209314"
---
# <a name="adding-appcompat-and-material-design"></a>Ajout de AppCompat et conception matérielle

_Suivez ces étapes pour convertir des applications Android de Xamarin.Forms existantes pour utiliser AppCompat et conception de matériau_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Vue d'ensemble

Ces instructions expliquent comment mettre à jour vos applications Android de Xamarin.Forms pour utiliser la bibliothèque de AppCompat et activer les documents de conception dans la version Android de vos applications Xamarin.Forms.

### <a name="1-update-xamarinforms"></a>1. Mettre à jour de Xamarin.Forms

Vérifiez que la solution est à l’aide de Xamarin.Forms 2.0 ou version ultérieure. Mettre à jour le package Xamarin.Forms Nuget vers la version 2.0 si nécessaire.

### <a name="2-check-android-version"></a>2. Vérifier la version Android

Vérifiez le que Framework cible du projet Android est Android 6.0 (Marshmallow). Vérifier le **projet Android > Options > Build > Général** sélectionné de paramètres pour vous assurer de l’infrastructure appropriée, par exemple :

 ![](appcompat-images/target-android-6-sml.png "Configuration de Build de Android général")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Ajouter de nouveaux thèmes pour prendre en charge de la conception de matériau

Créez les trois fichiers suivants dans votre projet Android et collez le contenu ci-dessous. Google fournit un [guide de style](http://www.google.com/design/spec/style/color.html#color-color-palette) et un [Générateur de palette de couleurs](http://www.materialpalette.com/) pour vous aider à choisir un schéma d’une couleur différente à celle spécifiée.

**Resources/Values/Colors.Xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Un style supplémentaire doit être inclus dans le **valeurs-v21** dossier pour appliquer des propriétés spécifiques lors de l’exécution sur Android Lollipop et versions ultérieures.

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Mise à jour AndroidManifest.xml

Pour vous assurer de ce nouveau thème informations sont utilisé, définissez le thème dans le **AndroidManifest** fichier en ajoutant `android:theme="@style/MyTheme"` (laissez le reste du code XML, telle qu’elle était).

**Properties/Androidmanifest.Xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Fournir des dispositions de barre d’outils, onglet

Créer **Tabbar.axml** et **Toolbar.axml** des fichiers dans le **ressources/disposition** directory et les coller dans leur contenu à partir du bas :

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Quelques propriétés pour les onglets ont été définies, y compris la gravité de l’onglet à `fill` et mode à `fixed`.
Si vous avez un grand nombre d’onglets peut vouloir passer cela à défilement - Lisez Android [TabLayout documentation](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) pour en savoir plus.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

Dans ces fichiers, nous créons un thème spécifique pour la barre d’outils qui peut-être varier pour votre application.
Reportez-vous à la [barre d’outils Hello](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) billet de blog pour en savoir plus.


### <a name="6-update-the-mainactivity"></a>6. Mise à jour le `MainActivity`

Dans les applications Xamarin.Forms existantes le **MainActivity.cs** classe héritera de `FormsApplicationActivity`. Il doit être remplacé par `FormsAppCompatActivity` pour activer les nouvelles fonctionnalités.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Enfin, « associer » les nouvelles dispositions de l’étape 5 dans le `OnCreate` (méthode), comme illustré ici :

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
