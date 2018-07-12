---
title: 'Xamarin.Essentials : préférences'
description: Ce document décrit la classe Préférences dans Xamarin.Essentials, qui enregistre les préférences de l’application dans un magasin de clés/valeurs. Elle explique comment utiliser la classe et les types de données qui peuvent être stockées.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca6d4f1ec60a80b483c79dd75267144e67d80c0b
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831762"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials : préférences

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **préférences** classe permet de stocker les préférences de l’application dans un magasin de clé/valeur.

## <a name="using-preferences"></a>À l’aide des préférences

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour enregistrer une valeur pour une donnée _clé_ dans Préférences :

```csharp
Preferences.Set("my_key", "my_value");
```

Pour récupérer une valeur de préférences ou une valeur par défaut n’est pas défini :

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Pour supprimer le _clé_ des préférences :

```csharp
Preferences.Remove("my_key");
```

Pour supprimer toutes les préférences :

```csharp
Preferences.Clear();
```

Outre ces méthodes, chacune accepter dans facultatif `sharedName` qui peut être utilisé pour créer des conteneurs supplémentaires de préférence. Lire les détails d’implémentation de plateforme ci-dessous.

## <a name="supported-data-types"></a>Types de données pris en charge

Les types de données suivants sont pris en charge dans **préférences**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>Détails d’implémentation

Valeurs de `DateTime` sont stockées dans un format de fichier binaire de 64 bits (entier long) à l’aide de deux méthodes définies par le `DateTime` classe : le [ `ToBinary` ](xref:System.DateTime.ToBinary) méthode est utilisée pour encoder le `DateTime` valeur et le [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) méthode décode la valeur. Consultez la documentation de ces méthodes pour les ajustements apportés à décodé valeurs lorsque un `DateTime` est stocké qui est pas une valeur de temps universel coordonné (UTC).

## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Toutes les données sont stockées dans [préférences partagées](https://developer.android.com/training/data-storage/shared-preferences.html). Si aucun `sharedName` est spécifié pour les préférences partagées par défaut sont utilisées, sinon le nom est utilisé pour obtenir un **privé** partagée préférences avec le nom spécifié.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Valeurs NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) est utilisé pour stocker des valeurs sur les appareils iOS. Si aucun `sharedName` est spécifié le `StandardUserDefaults` sont utilisé, sinon le nom est utilisé pour créer un nouveau `NSUserDefaults` portant le nom spécifié est utilisé pour le `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) est utilisé pour stocker les valeurs sur l’appareil. Si aucun `sharedName` porte la `LocalSettings` sont utilisées, sinon le nom est utilisé pour créer un nouveau conteneur à l’intérieur de `LocalSettings`.

--------------

## <a name="limitations"></a>Limitations

Lorsque vous stockez une chaîne, cette API est destinée à stocker de petites quantités de texte.  Performances peuvent être été si vous essayez d’utiliser pour stocker de grandes quantités de texte.

## <a name="api"></a>API

- [Préférences de code source](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Documentation sur les préférences d’API](xref:Xamarin.Essentials.Preferences)
