---
title: 'Xamarin.Essentials : Préférences'
description: La classe Preferences permet de stocker les préférences de l’application dans un magasin de clé/valeur.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4a45587c79cfbbcd1198f100915e698289f74950
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353748"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials : Préférences

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **Preferences** permet de stocker les préférences de l’application dans un magasin de clé/valeur.

## <a name="using-preferences"></a>Utilisation de **Preferences**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour enregistrer une valeur sous une certaine _clé_ :

```csharp
Preferences.Set("my_key", "my_value");
```

Pour récupérer une valeur sous une certaine _clé_. si aucune valeur n'est définie (NULL), la _valeur par défaut_ sera retournée :

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Pour supprimer une valeur sous une certaine _clé_ :

```csharp
Preferences.Remove("my_key");
```

Pour supprimer tous les ensembles clé-valeur des préférences :

```csharp
Preferences.Clear();
```

Toutes ces méthodes peuvent prendre en paramètre supplémentaire un `sharedName`. Ce `sharedName` peut être utilisé pour segmenter les paramètres en conteneurs distincts. Vous trouverez plus d'informations dans les détails d’implémentations par plateforme ci-dessous.

## <a name="supported-data-types"></a>Types de données prises en charge

Les types de données suivants sont pris en charge dans les **préférences**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>Détails d’implémentation

Les valeurs de type `DateTime` sont stockées dans un format de fichier binaire de 64 bits (entier long) à l’aide de deux méthodes définies par la classe `DateTime` : la méthode [`ToBinary`](xref:System.DateTime.ToBinary) est utilisée pour encoder la valeur `DateTime` et la méthode [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) pour décoder la valeur. Consultez la documentation de ces méthodes pour les ajustements apportés au décodage des valeurs lorsque un `DateTime` qui est pas une valeur de temps universel coordonné (UTC) est stocké.

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Toutes les données sont stockées dans les [préférences partagées](https://developer.android.com/training/data-storage/shared-preferences.html). Si aucun `sharedName` n'est spécifié les préférences partagées par défaut seront utilisées, sinon le `sharedName` sera utilisé pour obtenir un espace de préférences partagées **privé** avec le nom spécifié.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) est utilisé pour stocker des valeurs sur les appareils iOS. Si aucun `sharedName` n'est spécifié le `StandardUserDefaults` sera utilisé, sinon le `sharedName` sera utilisé comme `NSUserDefaultsType.SuiteName` sous lequel l'API créera un nouveau `NSUserDefaults`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) est utilisé pour stocker les valeurs sur l’appareil. Si aucun `sharedName` n'est spécifié les `LocalSettings` seront utilisées, sinon le `sharedName` sera utilisé pour créer un nouveau conteneur à l’intérieur des `LocalSettings`.

--------------

## <a name="persistence"></a>Persistance

La désinstallation de l'application supprimera toutes les _préférences_. À l'exception des applications qui ciblent et s’exécutent sur Android 6.0 (niveau d’API 23) ou version ultérieure, qui utilisent la [__sauvegarde automatique__](https://developer.android.com/guide/topics/data/autobackup). Cette fonctionnalité est activée par défaut et conserve notamment les [préférences partagées](https://developer.android.com/training/data-storage/shared-preferences.html), ce sur quoi l'API se base. Vous pouvez désactiver la sauvegarde automatique en suivant la [documentation Google](https://developer.android.com/guide/topics/data/autobackup).

## <a name="limitations"></a>Limitations

Lorsque vous stockez une chaîne de caractères, cette API est destinée à stocker de petites quantités de texte. Les performances peuvent être réduites si vous essayez d’utiliser cette API pour stocker de grandes quantités de texte.

## <a name="api"></a>API

- [Préférences de code source](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Documentation sur les préférences d’API](xref:Xamarin.Essentials.Preferences)
