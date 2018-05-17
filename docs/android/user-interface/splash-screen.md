---
title: Écran de démarrage
description: Une application Android prend un certain temps à démarrer, en particulier lorsque l’application est lancée tout d’abord sur un appareil. Un écran de démarrage peut afficher début jusqu'à la progression à l’utilisateur ou pour indiquer la marque.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/14/2018
ms.openlocfilehash: 6200a04bb4d82174d36a48beab7c63709ac39187
ms.sourcegitcommit: c5bb1045b2f4607dafe3101ad1ea6ade23e44342
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="splash-screen"></a>Écran de démarrage

_Une application Android prend un certain temps à démarrer, en particulier lorsque l’application est lancée tout d’abord sur un appareil. Un écran de démarrage peut afficher début jusqu'à la progression à l’utilisateur ou pour indiquer la marque._


## <a name="overview"></a>Vue d'ensemble

Une application Android prend un certain temps pour démarrer, en particulier lors de la première fois que l’application est exécutée sur un appareil (il est parfois appelée une _démarrage à froid_). L’écran de démarrage peut afficher la progression à l’utilisateur pour démarrer, ou il peut afficher des informations de personnalisation pour identifier et promouvoir l’application.

Ce guide décrit une technique pour implémenter un écran de démarrage dans une application Android. Elle couvre les étapes suivantes :

1.  Création d’une ressource drawable pour l’écran de démarrage.

2.  La définition d’un nouveau thème qui affichera la ressource drawable.

3.  Ajouter une nouvelle activité à l’application qui sera utilisée en tant que l’écran de démarrage défini par le thème créé à l’étape précédente.

[![Écran de démarrage logo Xamarin exemple suivi d’écran de l’application](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>Spécifications

Ce guide part du principe que l’application cible Android API niveau 15 (Android 4.0.3) ou une version ultérieure. L’application doit également avoir le **Xamarin.Android.Support.v4** et **Xamarin.Android.Support.v7.AppCompat** les packages NuGet ajoutés au projet.

Tous le code et le code XML dans ce guide se trouvent dans le [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen) exemple de projet pour ce guide.


## <a name="implementing-a-splash-screen"></a>Implémentation d’un écran de démarrage

Pour afficher l’écran de démarrage, la plus rapide consiste à créer un thème personnalisé et l’appliquer à une activité qui présente l’écran de démarrage. Lorsque l’activité est rendue, il charge le thème et applique la ressource drawable (référencée par le thème) à l’arrière-plan de l’activité. Cette approche évite la création d’un fichier de disposition.

L’écran de démarrage est implémenté comme une activité qui affiche la marque drawable, effectue les initialisations et toutes les tâches de démarrage. Une fois que l’application a été amorcé, l’écran de démarrage activité démarre l’activité principale et lui-même supprime de la pile back.


### <a name="creating-a-drawable-for-the-splash-screen"></a>Création d’un Drawable pour l’écran de démarrage

L’écran de démarrage affiche un document XML drawable dans l’arrière-plan de l’activité de l’écran de démarrage. Il est nécessaire d’utiliser une image bitmap (par exemple, le format PNG ou JPG) pour l’image à afficher.

Dans ce guide, nous utilisons un [couche liste](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) pour centrer l’image d’écran de démarrage de l’application. L’extrait de code suivant est un exemple d’un `drawable` à l’aide de la ressource un `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

Cela `layer-list` centre l’image d’écran de démarrage **splash.png** sur un arrière-plan spécifié par le `@color/splash_background` ressource.
Placez ce fichier dans le **drawable/ressources** dossier (par exemple, **Resources/drawable/splash_screen.xml**).

Une fois que l’écran de démarrage drawable a été créé, l’étape suivante consiste à créer un thème de l’écran de démarrage.


### <a name="implementing-a-theme"></a>Implémentation d’un thème

Pour créer un thème personnalisé pour l’activité de l’écran de démarrage, modifier (ou ajouter) le fichier **values/styles.xml** et créer un nouveau `style` , élément pour l’écran de démarrage. Un exemple **values/style.xml** fichier est présenté ci-dessous, avec un `style` nommé **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** est très spartan &ndash; il déclare l’arrière-plan de la fenêtre, explicitement supprime la barre de titre de la fenêtre et déclare qu’il est plein écran. Si vous souhaitez créer un écran de démarrage qui émule l’interface utilisateur de votre application avant de l’activité augmente la première mise en page, vous pouvez utiliser `windowContentOverlay` plutôt que `windowBackground` dans votre définition de style. Dans ce cas, vous devez également modifier le **splash_screen.xml** drawable afin qu’il affiche une émulation de l’interface utilisateur.


### <a name="create-a-splash-activity"></a>Créer une activité de démarrage

Nous devons maintenant une nouvelle activité pour Android lancer notre image de démarrage et effectue les tâches de démarrage. Le code suivant est un exemple d’une implémentation d’écran de démarrage terminée :

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` utilise explicitement le thème qui a été créé dans la section précédente, la substitution le thème par défaut de l’application.
Il n’est pas nécessaire pour charger une disposition dans `OnCreate` comme le thème déclare un drawable en arrière-plan.

Il est important de définir la `NoHistory=true` attribut afin que l’activité est supprimée de la pile back. Pour empêcher le bouton précédent de l’annulation du processus de démarrage, vous pouvez également remplacer `OnBackPressed` et qu’il ne rien faire :

```csharp
public override void OnBackPressed() { }
```

La tâche de démarrage est effectuée de façon asynchrone dans `OnResume`. Cela est nécessaire afin que le travail de démarrage ne pas ralentir ou retarder l’apparence de l’écran d’installation. Lorsque le travail est terminé, `SplashActivity` lancera `MainActivity` et l’utilisateur peut commencer à interagir avec l’application.

Cette nouvelle `SplashActivity` est défini comme l’activité de service de lancement de l’application en définissant le `MainLauncher` attribut `true`. Étant donné que `SplashActivity` est maintenant l’activité de service de lancement, vous devez modifier `MainActivity.cs`et supprimer les `MainLauncher` d’attribut de `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>Mode Paysage

L’écran de démarrage implémentée dans les étapes précédentes ne s’affichent correctement en mode portrait et paysage. Toutefois, dans certains cas, il est nécessaire d’avoir des écrans de démarrage distinct pour les modes portrait et paysage (par exemple, si l’image de démarrage est plein écran).

Pour ajouter un écran de démarrage pour le mode paysage, procédez comme suit :

1. Dans le **drawable/ressources** dossier, ajouter la version de paysage de l’image d’écran de démarrage à utiliser. Dans cet exemple, **splash_logo_land.png** est la version paysage du logo qui a été utilisée dans les exemples ci-dessus (il utilise les caractères blancs au lieu de bleu).

2. Dans le **drawable/ressources** dossier, créer une version paysage de le `layer-list` drawable qui a été défini précédemment (par exemple, **splash_screen_land.xml**). Dans ce fichier, définissez le chemin d’accès de l’image bitmap à la version de paysage de l’image d’écran de démarrage. Dans l’exemple suivant, **splash_screen_land.xml** utilise **splash_logo_land.png**:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
        <color android:color="@color/splash_background"/>
      </item>
      <item>
        <bitmap
            android:src="@drawable/splash_logo_land"
            android:tileMode="disabled"
            android:gravity="center"/>
      </item>
    </layer-list>
    ```

3.  Créer le **valeurs/ressources-terrestres** dossier s’il n’existe pas.

4.  Ajouter les fichiers **colors.xml** et **style.xml** à **terrestres de valeurs** (il peuvent être copiés et modifiés existante **values/colors.xml**et **values/style.xml** fichiers).

5.  Modifier **valeurs-terrestres/style.xml** afin qu’il utilise la version de paysage de le drawable pour `windowBackground`. Dans cet exemple, **splash_screen_land.xml** est utilisé :

    ```xml
    <resources>
      <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
      </style>
        <style name="MyTheme" parent="MyTheme.Base">
      </style>
      <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowBackground">@drawable/splash_screen_land</item>
        <item name="android:windowNoTitle">true</item>  
        <item name="android:windowFullscreen">true</item>  
        <item name="android:windowContentOverlay">@null</item>  
        <item name="android:windowActionBar">true</item>  
      </style>
    </resources>
    ```

6.  Modifier **valeurs-terrestres/colors.xml** pour configurer les couleurs que vous souhaitez utiliser pour la version de paysage de l’écran de démarrage. Dans cet exemple, la couleur d’arrière-plan de démarrage est modifiée pour le bleu pour le mode paysage :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#3498DB</color>
    </resources>
    ```

7.  Générez et exécutez de nouveau l’application. Faire pivoter le périphérique pour le mode paysage alors que l’écran de démarrage est toujours affichée. Modifications de l’écran de démarrage à la version paysage :

    [![Rotation d’écran de démarrage en mode paysage](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)


Notez que l’utilisation d’un écran de démarrage en mode paysage ne fournit pas toujours une expérience transparente. Par défaut, Android lance l’application en mode portrait et passe en mode paysage même si l’appareil est déjà en mode paysage. Par conséquent, si l’application est lancée alors que l’appareil est en mode paysage, l’appareil brièvement présente l’écran de démarrage portrait et puis réalise une animation de rotation à partir du portrait à l’écran de démarrage paysage. Malheureusement, cette transition portrait à paysage initiale se produit même lorsque `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` est spécifié dans les indicateurs de l’activité de démarrage. Il est le meilleur moyen de contourner cette limitation permet de créer une capture d’écran de démarrage unique qui s’affiche correctement en mode portrait et paysage.


## <a name="summary"></a>Récapitulatif

Ce guide décrit une façon d’implémenter un écran de démarrage dans une application Xamarin.Android ; à savoir, appliquer un thème personnalisé à l’activité de lancement.


## <a name="related-links"></a>Liens associés

- [Écran de démarrage (exemple)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [liste de couche Drawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Modèles de conception de matériel - écrans de démarrage](https://www.google.com/design/spec/patterns/launch-screens.html)
