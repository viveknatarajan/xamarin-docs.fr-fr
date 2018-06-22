---
title: Rotation de la gestion
description: Cette rubrique décrit comment gérer les modifications de l’orientation de périphérique dans Xamarin.Android. Elle couvre l’utilisation avec le système de ressource Android pour charger automatiquement les ressources pour une orientation de périphérique particulier, ainsi que la manière de gérer par programme l’orientation change.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1cdc7928c45b99cdd8c8149b3ae9b06e790deeca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30768894"
---
# <a name="handling-rotation"></a>Rotation de la gestion

_Cette rubrique décrit comment gérer les modifications de l’orientation de périphérique dans Xamarin.Android. Elle couvre l’utilisation avec le système de ressource Android pour charger automatiquement les ressources pour une orientation de périphérique particulier, ainsi que la manière de gérer par programme l’orientation change._


## <a name="overview"></a>Vue d'ensemble

Étant donné que les appareils mobiles sont facilement pivotés, rotation intégrée est une fonctionnalité standard dans les systèmes d’exploitation mobile. Android fournit une infrastructure sophistiquée pour traiter de rotation dans les applications, si l’interface utilisateur est créé de façon déclarative dans XML, ou par programmation dans le code. Lors de la gestion automatiquement les modifications de disposition déclarative sur un appareil avec une rotation, une application peut bénéficier de l’intégration étroite avec le système de ressources Android. Pour la mise en page par programme, les modifications doivent être gérées manuellement. Cela permet un contrôle plus précis lors de l’exécution, mais au détriment de plus de travail pour le développeur. Une application peut également choisir de désactiver le redémarrage de l’activité et prendre le contrôle manuel des modifications d’orientation.

Ce guide présente les rubriques d’orientation suivantes :

-   **Rotation de disposition déclarative** &ndash; comment utiliser le système de ressource Android pour générer des applications prenant en charge l’orientation, y compris comment charger des dispositions et drawables pour les orientations particuliers.

-   **Rotation de disposition par programmation** &ndash; comment ajouter des contrôles par programme, ainsi que comment gérer manuellement les modifications de l’orientation.


## <a name="handling-rotation-declaratively-with-layouts"></a>Rotation de façon déclarative avec les dispositions de gestion

En incluant des fichiers dans les dossiers qui suivent les conventions d’affectation de noms, Android charge automatiquement les fichiers appropriés lorsque l’orientation est modifiée.
Cela inclut la prise en charge pour :

-   *Ressources de mise en page* &ndash; en spécifiant les fichiers de mise en page sont agrandies pour chaque orientation.

-   *Ressources drawable* &ndash; spécifiant les drawables sont chargés pour chaque orientation.


### <a name="layout-resources"></a>Ressources de mise en page

Par défaut, les fichiers XML Android (AXML) inclus dans le **ressources/disposition** dossier sont utilisés pour le rendu des vues pour une activité. Les ressources de ce dossier sont utilisés pour l’orientation portrait et paysage si aucune ressource de mise en forme supplémentaires n’est fournies spécifiquement pour paysage. Considérez la structure de projet créée par le modèle de projet par défaut :

[![Structure de modèle de projet par défaut](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Ce projet crée un seul **Main.axml** de fichiers dans le **ressources/disposition** dossier. Lors de l’activité `OnCreate` est appelée, elle augmente la vue définie dans **Main.axml,** qui déclare un bouton, comme indiqué dans le code XML ci-dessous :

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

Si l’appareil passe en orientation paysage, l’activité de `OnCreate` méthode est appelée à nouveau et le même **Main.axml** fichier est agrandie, comme indiqué dans la capture d’écran ci-dessous :

[![Même écran mais en orientation paysage](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>Dispositions spécifiques à l’orientation

Outre le dossier de disposition (qui est portrait par défaut et peut également être explicitement nommé *disposition-port* en incluant un dossier nommé `layout-land`), une application peut définir les vues dont il a besoin en paysage sans code modifications.

Supposons que la **Main.axml** fichier contient le code XML suivant :

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

Si un dossier nommé terrestres mise en page qui contient un autre **Main.axml** fichier est ajouté au projet, gonfler la disposition en paysage entraîne Android chargement récemment ajouté **Main.axml.** Prendre en compte la version paysage de le **Main.axml** fichier qui contient le code suivant (par souci de simplicité, ce code XML est similaire à la version de portrait par défaut du code, mais utilise une autre chaîne dans le `TextView`) :

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

Lors de la rotation, Android traite drawable ressources de la même façon aux ressources de mise en page. Dans ce cas, le système Obtient les drawables à partir de la **drawable/ressources** et **ressources/drawable-terrestres** dossiers, respectivement.

Par exemple, le projet inclut une image nommée Monkey.png dans le **drawable/ressources** dossier, où le drawable est référencé à partir une `ImageView` dans le fichier XML comme suit :

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Imaginons qu’une version différente de **Monkey.png** est inclus sous **ressources/drawable-terrestres**. Tout comme avec les fichiers de mise en page, quand l’appareil est pivoté, les modifications drawable pour l’orientation donnée, comme indiqué ci-dessous :

[![Version différente de Monkey.png indiqué dans les modes portrait et paysage](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>Gestion par programme de Rotation

Parfois, nous définissons mises en page dans le code. Cela peut se produire pour diverses raisons, y compris les limitations techniques, de préférence de développeur, etc. Lorsque nous ajoutons des contrôles par programme, une application doit tenir compte manuellement l’orientation du périphérique, qui est géré automatiquement quand vous utilisez les ressources XML.


### <a name="adding-controls-in-code"></a>Ajout de contrôles dans le Code

Pour ajouter des contrôles par programme, une application doit effectuer les étapes suivantes :

-  Créer une disposition.
-  Définissez les paramètres de mise en page.
-  Créer des contrôles.
-  Définissez les paramètres de mise en page de contrôle.
-  Ajouter des contrôles à la disposition.
-  Définir la disposition en tant que l’affichage du contenu.

Par exemple, considérez une interface utilisateur constituée d’une unique `TextView` contrôle ajouté à un `RelativeLayout`, comme illustré dans le code suivant.

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

Ce code crée une instance d’un `RelativeLayout` classe et définit son `LayoutParameters` propriété. La `LayoutParams` classe consiste d’Android encapsulant la façon dont les contrôles sont positionnés de façon réutilisable. Après la création d’une instance d’une disposition, les contrôles peuvent être créés et ajoutés. Les contrôles ont également `LayoutParameters`, telles que le `TextView` dans cet exemple. Après le `TextView` est créé, en ajoutant à la `RelativeLayout` et en définissant le `RelativeLayout` en tant que les résultats de l’affichage du contenu dans l’affichage de l’application le `TextView` comme indiqué :

[![Bouton de compteur d’incrément affiché en mode portrait et paysage](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>Détection de l’Orientation dans le Code

Si une application ne tente de charger une interface utilisateur différente pour chaque orientation lorsque `OnCreate` est appelé (cela se produit chaque fois qu’un appareil rotation), il doit détecter l’orientation et chargez le code d’interface utilisateur de votre choix. Android dispose d’une classe appelée le `WindowManager`, qui peut être utilisé pour déterminer la rotation de l’appareil via le `WindowManager.DefaultDisplay.Rotation` propriété, comme indiqué ci-dessous :

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

Ce code définit le `TextView` à être positionnées à 100 pixels de haut en gauche de l’écran, l’animation automatiquement à la nouvelle disposition, lorsque vous faites pivoter paysage, comme illustré ici :

[![État d’affichage est conservé entre les modes portrait et paysage](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>Empêche le redémarrage de l’activité

En plus de gérer tous les éléments de `OnCreate`, une application peut également empêcher une activité en cours de redémarrage lorsque l’orientation est modifiée en définissant `ConfigurationChanges` dans le `ActivityAttribute` comme suit :

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Maintenant lorsque vous faites pivoter l’appareil, l’activité n’est pas redémarrée. Pour gérer manuellement la modification de l’orientation dans ce cas, une activité peut remplacer le `OnConfigurationChanged` (méthode) et déterminer l’orientation de la `Configuration` objet est passé, comme dans la nouvelle implémentation de l’activité ci-dessous :

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

Ici le `TextView's` les paramètres de mise en page sont initialisés pour portrait et paysage. Variables de classe contiennent les paramètres, ainsi que la `TextView` lui-même, étant donné que l’activité n'est pas recréée lorsque l’orientation est modifiée. Le code utilise toujours le `surfaceOrientartion` dans `OnCreate` pour définir la disposition initiale pour le `TextView`. Après cela, `OnConfigurationChanged` gère toutes les modifications de disposition suivants.

Lorsque nous exécutons l’application, Android charge les modifications de l’interface utilisateur, comme la rotation de l’appareil se produit et ne redémarre pas l’activité.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>Empêche le redémarrage de l’activité pour les dispositions déclaratives

Les redémarrages d’activité dus à la rotation du périphérique peuvent empêcher également si nous définissons la disposition au format XML. Par exemple, nous pouvons utiliser cette approche si vous souhaitez empêcher un redémarrage de l’activité (pour des raisons de performances, par exemple) et nous n’avez pas besoin de charger de nouvelles ressources pour les différentes orientations.

Pour ce faire, nous la même procédure que nous utilisons avec une disposition par programmation. Il suffit de définir `ConfigurationChanges` dans les `ActivityAttribute`, comme nous l’avons fait le `CodeLayoutActivity` précédemment. Tout code qui doit-elle s’exécuter pendant la modification de l’orientation peut à nouveau être implémentée dans le `OnConfigurationChanged` (méthode).


## <a name="maintaining-state-during-orientation-changes"></a>Gestion de l’état lors de la modification de l’Orientation

Si la gestion des rotation de façon déclarative ou par programme, toutes les applications Android doivent implémenter les mêmes techniques pour la gestion d’état lorsque l’orientation de périphérique change. Gestion de l’état est importante, car le système redémarre une activité en cours d’exécution lorsque vous faites pivoter un appareil Android. Android cela pour faciliter le chargement des ressources de remplacement, tels que les dispositions et drawables qui sont conçues spécifiquement pour une orientation spécifique. Lors de son redémarrage, l’activité perd n’importe quel état transitoire, qu'il risque d’avoir stocké dans des variables de classe locale. Par conséquent, si une activité est dépendant d’état, il doit rendre son état persistant au niveau de l’application. Une application doit gérer l’enregistrement et la restauration de n’importe quel état de l’application qu’il souhaite conserver entre les modifications de l’orientation.

Pour plus d’informations sur la persistance de l’état dans Android, reportez-vous à la [cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md) guide.


## <a name="summary"></a>Récapitulatif

Cet article expliqué comment utiliser les fonctionnalités intégrées d’Android avec rotation. Tout d’abord, il a expliqué comment le système Android ressource permet de créer des applications prenant en charge l’orientation. Ensuite, elle présente comment ajouter des contrôles dans le code, ainsi que comment gérer manuellement les modifications de l’orientation.



## <a name="related-links"></a>Liens associés

- [Démonstration de rotation (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md)
- [La gestion des modifications du Runtime](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Modification de l’Orientation d’écran rapide](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
