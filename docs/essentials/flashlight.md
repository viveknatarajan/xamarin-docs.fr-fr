---
title: 'Xamarin.Essentials : Torche'
description: La classe Flashlight a la possibilité d’activer ou de désactiver le flash de l’appareil photo afin de l'utiliser comme lampe torche.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353358"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials : Torche

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Flashlight** a la possibilité d’activer ou de désactiver le flash de l’appareil photo afin de l'utiliser comme lampe torche.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **Flashlight**, quelques étapes de configuration spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Les autorisations `Flashlight` et `Camera` sont obligatoires et doivent être configurées dans le projet Android. Elles peuvent être ajoutées comme suit :

Ouvrez le fichier **AssemblyInfo.cs** sous le dossier **propriétés** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

Ou mettez à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** dans le dossier **Properties** et ajoutez le code suivant dans le nœud **manifest**.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

OU cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **Android manifest**, repérez la zone "**Require permissions:**" et cochez les autorisations **Flashlight** et **Camera**. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

En ajoutant ces autorisations [Google Play filtrent automatiquement les périphériques](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) sans matériel spécifique. Vous pouvez obtenir contourner ce problème en ajoutant le code suivant à votre fichier AssemblyInfo.cs dans votre projet Android :

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-flashlight"></a>Utilisation de **Flashlight**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La torche peut être activée et désactivée via les méthodes `TurnOnAsync` et `TurnOffAsync`:

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

## <a name="platform-implementation-specifics"></a>Caractéristiques d'implémentation de la plateforme

### <a name="androidtabandroid"></a>[Android](#tab/android)

La classe Flashlight a été optimisée en fonction du système d’exploitation de l’appareil.

#### <a name="api-level-23-and-higher"></a>Niveau d’API 23 et versions ultérieures

Sur les niveaux d’API plus récents, [Torch Mode](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) sera utilisé pour activer ou désactiver le flash de l’appareil.

#### <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieur

Une texture de surface de caméra est créée afin d'activer et de désactiver le flash de l'appareil.

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/)  sera utilisé pour activer ou désactiver le flash de l’appareil.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp)  sera utilisé pour détecter la première lampe à l’arrière de l’appareil et pour l'activer ou la désactiver.

-----

## <a name="api"></a>API

- [Code source de torche](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Documentation de torche API](xref:Xamarin.Essentials.Flashlight)
