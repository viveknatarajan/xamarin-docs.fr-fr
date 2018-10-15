---
title: 'Xamarin.Essentials : Vibration'
description: Ce document décrit la classe de Vibration dans Xamarin.Essentials, ce qui vous permet de démarrer et arrêter la fonctionnalité vibrer pour une quantité de temps souhaitée.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 622689342dd961a63318a88f098dea4d1a60e277
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353865"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials : Vibration

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Vibration** vous permet de démarrer et arrêter la fonctionnalité vibrer pour une quantité de temps souhaitée.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **Vibration** quelques étapes de configuration spécifiques à la plate-forme sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

L'autorisation `Vibrate` est obligatoire et doit être configurée dans le projet Android. Elle peut être ajoutée comme suit :

Ouvrez le fichier **AssemblyInfo.cs** sous le dossier **propriétés** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

OU mettre à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** sous le dossier **propriétés** et ajoutez le code suivant à l’intérieur de la **manifeste** nœud.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** trouver la **autorisations requises :** zone et vérifiez la **vibrer** autorisation. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requis.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune différence de la plateforme.

-----

## <a name="using-vibration"></a>Utilisation de Vibration

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de Vibration peut être demandée pour un laps de temps défini ou la valeur par défaut de 500 millisecondes.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

L’annulation de vibration d’appareil peut être demandée avec la `Cancel` méthode :

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Différences entre les plateformes

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune différence de la plateforme.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Terme uniquement lorsque le périphérique est défini sur « Vibrer sur anneau ».
* Terme toujours pour 500 millisecondes.
* Pas possible d’annuler la vibration.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune différence de la plateforme.

-----

## <a name="api"></a>API

- [Code source de vibration](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Documentation de l’API de vibration](xref:Xamarin.Essentials.Vibration)
