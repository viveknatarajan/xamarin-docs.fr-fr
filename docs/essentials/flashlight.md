---
title: 'Xamarin.Essentials : torche'
description: Ce document décrit la classe torche dans Xamarin.Essentials, ce qui permet d’activer ou désactiver photo de l’appareil flash à transformer en une torche.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353358"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials : torche

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **torche** classe a la possibilité d’activer ou désactiver photo de l’appareil flash à transformer en une torche.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **torche** fonctionnalité de la configuration spécifique de plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Les autorisations torche et caméra sont requises et doivent être configurées dans le projet Android. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

OU mettre à jour le manifeste Android :

Ouvrez le **AndroidManifest.xml** fichier sous le **propriétés** dossier et ajoutez le code suivant à l’intérieur de la **manifeste** nœud.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** trouver la **autorisations requises :** zone et vérifiez la **torche** et **caméra** autorisations. Cela met automatiquement à jour le **AndroidManifest.xml** fichier.

En ajoutant ces autorisations [Google Play filtrent automatiquement les périphériques](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) sans matériel spécifique. Vous pouvez obtenir contourner ce problème en ajoutant le code suivant à votre fichier AssemblyInfo.cs dans votre projet Android :

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requis.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requis.

-----

## <a name="using-flashlight"></a>À l’aide de torche

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Le torche peut être activée et désactivée via la `TurnOnAsync` et `TurnOffAsync` méthodes :

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

### <a name="androidtabandroid"></a>[Android](#tab/android)

La classe torche a été optimisée basé sur le système d’exploitation de l’appareil.

#### <a name="api-level-23-and-higher"></a>Niveau d’API 23 et versions ultérieures

Sur les niveaux d’API plus récente, [Torch Mode](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) sera utilisé pour activer ou désactiver l’unité flash de l’appareil.

#### <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieur

Une texture de surface d’exposition de caméra est créée pour activer ou désactiver le `FlashMode` de l’unité de l’appareil photo. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) sert à activer et désactiver le Torch et le mode Flash de l’appareil.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) est utilisé pour détecter la première lamp à l’arrière de l’appareil pour activer ou désactiver.

-----

## <a name="api"></a>API

- [Code source de torche](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Documentation de torche API](xref:Xamarin.Essentials.Flashlight)
