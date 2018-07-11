---
title: 'Xamarin.Essentials : Vibration'
description: Ce document décrit la classe de Vibration dans Xamarin.Essentials, ce qui vous permet de démarrer et arrêter la fonctionnalité vibrer pour une quantité de temps souhaitée.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 530273543c6cb71038613c22fa4a6bfbde4928d7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947255"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials : Vibration

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **Vibration** classe vous permet de démarrer et arrêter la fonctionnalité vibrer pour une quantité de temps souhaitée.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **Vibration** fonctionnalité de la configuration spécifique de plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

L’autorisation vibrer est obligatoire et doit être configurée dans le projet Android. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

OU mettre à jour le manifeste Android :

Ouvrez le **AndroidManifest.xml** fichier sous le **propriétés** dossier et ajoutez le code suivant à l’intérieur de la **manifeste** nœud.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Ou cliquez avec le bouton droit sur le projet Anroid et ouvrez les propriétés du projet. Sous **manifeste Android** trouver la **autorisations requises :** zone et vérifiez la **vibrer** autorisation. Cela met automatiquement à jour le **AndroidManifest.xml** fichier.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requis.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune différence de la plateforme.

-----

## <a name="using-vibration"></a>À l’aide de Vibration

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

## <a name="platform-differences"></a>Différences de plateforme

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
