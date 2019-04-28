---
title: Rotation de la gestion
description: Cette rubrique décrit comment gérer les changements d’orientation de périphérique dans Xamarin.Android. Il explique comment travailler avec le système de ressources Android pour charger automatiquement les ressources pour une orientation de périphérique particulier, ainsi que la manière dont pour gérer par programme l’orientation des modifications.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 62e64be89e26e5a8412cd34221da581e99fc5e6a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61017332"
---
# <a name="handling-rotation"></a>Rotation de la gestion

_Cette rubrique décrit comment gérer les changements d’orientation de périphérique dans Xamarin.Android. Il explique comment travailler avec le système de ressources Android pour charger automatiquement les ressources pour une orientation de périphérique particulier, ainsi que la manière dont pour gérer par programme l’orientation des modifications._


## <a name="overview"></a>Vue d'ensemble

Étant donné que les appareils mobiles pivotent facilement, rotation intégrée est une fonctionnalité standard dans les systèmes d’exploitation mobiles. Android fournit une infrastructure sophistiquée pour la gestion avec une rotation au sein d’applications, si l’interface utilisateur est créé de façon déclarative dans XML, ou par programmation dans le code. Lors de la gestion automatiquement les changements de disposition déclarative sur un appareil pivoté, une application peut bénéficier de l’intégration étroite avec le système de ressources Android. Pour la mise en page par programmation, les modifications doivent être traitées manuellement. Cela permet un contrôle plus précis lors de l’exécution, mais au détriment de plus de travail pour le développeur. Une application peut également choisir de refuser le redémarrage de l’activité et de prendre le contrôle manuel de changements d’orientation.

Ce guide examine les rubriques d’orientation suivantes :

-   **Rotation de disposition déclarative** &ndash; comment utiliser le système de ressources Android pour générer des applications prenant en charge l’orientation, y compris comment charger des dispositions et drawables pour les orientations particuliers.

-   **Rotation de disposition par programmation** &ndash; comment ajouter des contrôles par programmation, ainsi que comment gérer manuellement les changements d’orientation.


## <a name="handling-rotation-declaratively-with-layouts"></a>Rotation de façon déclarative avec les dispositions de la gestion

En incluant les fichiers dans les dossiers qui suivent les conventions d’affectation de noms, Android charge automatiquement les fichiers appropriés lors de l’orientation est modifiée.
Cela inclut la prise en charge pour :

-   *Ressources de mise en page* &ndash; spécifiant les fichiers de disposition sont compressées pour chaque orientation.

-   *Ressources drawable* &ndash; spécifiant quel drawables sont chargés pour chaque orientation.


### <a name="layout-resources"></a>Ressources de mise en page

Par défaut, les fichiers XML Android (AXML) inclus dans le **ressources/disposition** dossier sont utilisés pour le rendu des vues pour une activité. Les ressources de ce dossier sont utilisées pour l’orientation portrait et paysage si aucune ressource de dispositions supplémentaires n’est fournies spécifiquement pour paysage. Considérez la structure de projet créée par le modèle de projet par défaut :

[![Structure de modèle de projet par défaut](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Ce projet crée une valeur unique **Main.axml** de fichiers dans le **ressources/disposition** dossier. Lors de l’activité `OnCreate` est appelée, elle augmente la vue définie dans **Main.axml,** qui déclare un bouton, comme indiqué dans le code XML ci-dessous :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

Si l’appareil est pivoté en orientation paysage, l’activité de `OnCreate` méthode est appelée à nouveau et les mêmes **Main.axml** fichier est agrandie, comme illustré dans la capture d’écran ci-dessous :

[![Même écran, mais en orientation paysage](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>Dispositions spécifiques à l’orientation

En plus du dossier de disposition (qui est par défaut portrait et peuvent également être nommé explicitement *disposition-port* en incluant un dossier nommé `layout-land`), une application peut définir les vues dont il a besoin en cas de paysage sans code modifications.

Supposons que le **Main.axml** fichier contenait le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Si un dossier nommé land mise en page qui contient un autre **Main.axml** fichier est ajouté au projet, compression de la disposition en cas de paysage entraîne Android chargement récemment ajouté **Main.axml.** Prendre en compte la version du paysage de la **Main.axml** fichier qui contient le code suivant (par souci de simplicité, ce code XML est semblable à la version de portrait par défaut du code, mais utilise une autre chaîne dans le `TextView`) :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Ce code en cours d’exécution et de faire pivoter l’appareil de portrait à paysage montrent le nouveau chargement de XML, comme indiqué ci-dessous :

[![Captures d’écran portrait et paysage imprimer le mode portrait](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>Ressources drawable

Lors de la rotation, Android traite les ressources drawable la même manière aux ressources de mise en page. Dans ce cas, le système Obtient le drawables à partir de la **ressources/drawable** et **ressources/drawable-land** dossiers, respectivement.

Par exemple, le projet inclut une image nommée Monkey.png dans le **ressources/drawable** dossier, où le drawable est référencé à partir d’un `ImageView` dans XML comme suit :

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Supposons par ailleurs qu’une version différente de **Monkey.png** est inclus sous **ressources/drawable-land**. Tout comme avec les fichiers de mise en page, quand l’appareil est de faire pivoter, les modifications drawable pour l’orientation donnée, comme indiqué ci-dessous :

[![Version différente de Monkey.png indiqué dans les modes portrait et paysage](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>Rotation de la gestion par programmation

Parfois, nous définissons les dispositions dans le code. Cela peut se produire pour diverses raisons, y compris les limitations techniques, de préférence de développeur, etc. Lorsque nous ajoutons des contrôles par programme, une application doit tenir compte manuellement pour l’orientation de l’appareil, ce qui est géré automatiquement lorsque nous utilisons des ressources XML.


### <a name="adding-controls-in-code"></a>Ajout de contrôles dans le Code

Pour ajouter des contrôles par programme, une application doit effectuer les étapes suivantes :

-  Créer une disposition.
-  Définissez les paramètres de disposition.
-  Créer des contrôles.
-  Définissez les paramètres de mise en page de contrôle.
-  Ajouter des contrôles à la disposition.
-  Définir la disposition en tant que l’affichage du contenu.

Par exemple, considérez une interface utilisateur consistant en un seul `TextView` contrôle ajouté à un `RelativeLayout`, comme illustré dans le code suivant.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Ce code crée une instance d’un `RelativeLayout` classe et définit son `LayoutParameters` propriété. Le `LayoutParams` classe est le moyen de d’Android d’encapsuler la façon dont les contrôles sont positionnés de façon réutilisable. Après la création d’une instance d’une disposition, contrôles peuvent être créés et ajoutés à ce dernier. Les contrôles ont également `LayoutParameters`, telles que le `TextView` dans cet exemple. Après le `TextView` est créé, en ajoutant à la `RelativeLayout` et en définissant le `RelativeLayout` en tant que les résultats de l’affichage du contenu dans l’affichage de l’application le `TextView` comme indiqué :

[![Bouton de compteur d’incrément indiqué dans les modes portrait et paysage](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>Détection de l’Orientation dans le Code

Si une application essaie de charger une interface utilisateur différente pour chaque orientation lorsque `OnCreate` est appelé (cela se produit chaque fois qu’un appareil est pivoté), il doit détecter l’orientation et chargez ensuite le code de l’interface utilisateur de votre choix. Android a une classe appelée le `WindowManager`, ce qui peut être utilisé pour déterminer la rotation de l’appareil via le `WindowManager.DefaultDisplay.Rotation` propriété, comme indiqué ci-dessous :

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Ce code définit le `TextView` être positionnées 100 pixels à partir de la partie supérieure gauche de l’écran, animer automatiquement à la nouvelle disposition, lorsque vous faites pivoter paysage, comme illustré ici :

[![État d’affichage est conservé entre les modes portrait et paysage](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>Empêche le redémarrage de l’activité

En plus de gérer tous les éléments de `OnCreate`, une application peut également empêcher une activité en cours de redémarrage lors de l’orientation est modifiée en définissant `ConfigurationChanges` dans le `ActivityAttribute` comme suit :

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Maintenant lorsque vous faites pivoter l’appareil, l’activité n’est pas redémarrée. Pour gérer manuellement la modification d’orientation dans ce cas, une activité peut remplacer le `OnConfigurationChanged` méthode et déterminer l’orientation de la `Configuration` objet qui est passé, comme dans la nouvelle implémentation de l’activité ci-dessous :

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

Ici le `TextView's` les paramètres de disposition sont initialisés pour portrait et paysage. Les variables de classe contenir les paramètres, ainsi que la `TextView` lui-même, étant donné que l’activité n'est pas recréée lors de l’orientation est modifiée. Le code utilise toujours le `surfaceOrientartion` dans `OnCreate` pour définir la disposition initiale pour le `TextView`. Après cela, `OnConfigurationChanged` gère toutes les modifications de disposition suivantes.

Lorsque nous exécutons l’application, charge les modifications de l’interface utilisateur comme la rotation de l’appareil se produit et ne redémarre pas l’activité.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>Empêche le redémarrage de l’activité pour les dispositions déclaratives

Redémarrages d’activité dus à la rotation de l’appareil peuvent également être évités si nous définissons la disposition dans XML. Par exemple, nous pouvons utiliser cette approche si vous souhaitez empêcher un redémarrage de l’activité (pour des raisons de performances, par exemple) et nous n’avez pas besoin de charger de nouvelles ressources pour différentes orientations.

Pour ce faire, nous suivons la même procédure que nous utilisons avec une disposition par programmation. Il suffit de définir `ConfigurationChanges` dans le `ActivityAttribute`, comme nous l’avons fait le `CodeLayoutActivity` précédemment. Tout code qui n’a besoin d’exécuter pendant la modification de l’orientation peut à nouveau être implémentée dans le `OnConfigurationChanged` (méthode).


## <a name="maintaining-state-during-orientation-changes"></a>Gestion de l’état lors de la modification de l’Orientation

Si la rotation de la gestion de manière déclarative ou par programme, toutes les applications Android doivent implémenter les mêmes techniques pour gérer l’état lors de l’orientation de l’appareil est modifiée. Gestion de l’état est importante, car le système redémarre une activité en cours d’exécution lorsque vous faites pivoter un appareil Android. Android procède ainsi pour faciliter le chargement d’autres ressources, telles que les dispositions et drawables qui sont conçues spécifiquement pour une orientation particulière. Lorsqu’elle redémarre, l’activité perd n’importe quel état transitoire, qu'il risque d’avoir stocké dans des variables de classe locale. Par conséquent, si une activité est dépendante d’état, il doit conserver son état au niveau de l’application. Une application doit gérer l’enregistrement et la restauration de n’importe quel état de l’application qu’il souhaite conserver entre les changements d’orientation.

Pour plus d’informations sur la persistance de l’état dans Android, reportez-vous à la [cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md) guide.


## <a name="summary"></a>Récapitulatif

Cet article a abordé comment utiliser les fonctionnalités intégrées d’Android pour travailler avec une rotation. Tout d’abord, vous avez appris comment utiliser le système de ressources Android pour créer des applications prenant en charge l’orientation. Puis il présenté comment ajouter des contrôles dans le code, ainsi que comment gérer manuellement les changements d’orientation.



## <a name="related-links"></a>Liens associés

- [Démonstration de rotation (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Gestion des modifications du Runtime](https://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Modification de l’Orientation d’écran rapide](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
