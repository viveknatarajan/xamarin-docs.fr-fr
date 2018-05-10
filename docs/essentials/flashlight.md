---
title: Xamarin.Essentials Flash
description: La classe Flash a la possibilité d’activer ou désactiver photo l’appareil flash à transformer en un Flash.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f187fa404df09e387ed870f524239d3baabfdd3f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials Flash

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **Flash** classe a la possibilité d’activer ou désactiver photo l’appareil pour qu’il devienne un flash flash.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **Flash** fonctionnalité de la configuration spécifique plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Les autorisations photo et Flash sont requises et doivent être configurées dans le projet Android. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajouter :

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

Ou cliquez avec le bouton droit sur le projet Anroid et ouvrez les propriétés du projet. Sous **manifeste Android** de trouver la **les autorisations requises :** zone et vérifiez la **Flash** et **caméra** autorisations. Met automatiquement à jour la **AndroidManifest.xml** fichier.

En ajoutant ces autorisations [Google Play filtrent automatiquement les périphériques](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) sans matériel spécifique. Vous pouvez obtenir contourner ce problème en ajoutant le code suivant à votre fichier AssemblyInfo.cs dans votre projet Android :

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-flashlight"></a>À l’aide de Flash

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Le Flash permettre être activée et désactivées via la `TurnOnAsync` et `TurnOffAsync` méthodes :

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

## <a name="platform-implementation-specifics"></a>Caractéristiques d’implémentation de plate-forme

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

La classe Flash a été optmized basé sur le système d’exploitation de l’appareil.

#### <a name="api-level-23-and-higher"></a>Niveau de l’API 23 et versions ultérieures

Sur les niveaux d’API plus récente, [CHALUMEAU Mode](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) servira à activer ou désactiver l’unité de disque mémoire flash de l’appareil.

#### <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieure

Une texture de surface de la caméra est créée pour activer ou désactiver la `FlashMode` de la caméra. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) est utilisé pour activer et désactiver la torche et mode Flash de l’appareil.

### <a name="uwptabuwp-specifics"></a>[UWP](#tab/uwp-specifics)

[Feu](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) est utilisé pour détecter le premier feu à l’arrière de l’appareil pour activer ou désactiver.

-----

## <a name="api"></a>API

- [Code source de Flash](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [Documentation de l’API de Flash](xref:Xamarin.Essentials.Flashlight)
