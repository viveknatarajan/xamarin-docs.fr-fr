---
title: Prise en charge UrhoSharp Android
description: "Le programme d’installation spécifique Android et fonctionnalités UrhoSharp."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: f99b8d2d9f779bc0cf14d76c110d9769ec49ad53
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-android-support"></a>Prise en charge UrhoSharp Android

_Fonctionnalités et le programme d’installation spécifique android_

Bien que Urho est une bibliothèque de classes portable et permet la même API à utiliser sur la plateforme différents pour la logique du jeu, vous devez initialiser Urho dans votre pilote plateforme et dans certains cas, vous souhaitez tirer parti des fonctionnalités spécifiques de plateforme .

Dans les pages suivantes, supposons que `MyGame` est une sous-classe de la `Application` classe.

## <a name="architectures"></a>Architectures

**Architectures prises en charge**: x86, armeabi, armeabi-v7a

## <a name="create-a-project"></a>Créer un projet

Créer un projet Android et ajoutez le package NuGet de UrhoSharp.

Ajouter des données qui contient vos éléments multimédias à la **actifs** active et assurez-vous que tous les fichiers ont **AndroidAsset** comme le **Action de génération**.

![Le programme d’installation de projet](android-images/image-3.png "ajouter des données qui contient les éléments multimédias dans le répertoire de ressources")

## <a name="configure-and-launching-urho"></a>Configurer et de lancer Urho

Ajouter à l’aide des instructions pour la `Urho` et `Urho.Android` espaces de noms, puis ajoutez ce code pour l’initialisation Urho, ainsi que le lancement de votre application.

Pour exécuter un jeu, tel qu’implémenté dans la classe MyGame le plus simple consiste à appeler

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Une activité en plein écran s’ouvre avec le jeu comme un contenu.

## <a name="custom-embedding-of-urho"></a>Incorporation personnalisé de Urho

Vous peut également avoir Urho prendre sur l’écran de l’ensemble de l’application, et pour l’utiliser en tant que composant de votre application, vous pouvez créer un `SurfaceView` via :

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

Vous devrez également transmettre quelques événements de votre activité à UrhoSharp, par exemple :

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Vous devez faire de même : `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` et `OnWindowFocusChanged`.

Cet exemple montre une activité classique qui lance le jeu :

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

