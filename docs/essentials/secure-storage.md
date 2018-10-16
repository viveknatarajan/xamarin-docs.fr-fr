---
title: 'Xamarin.Essentials : Stockage sécurisé'
description: Ce document décrit la classe SecureStorage dans Xamarin.Essentials, ce qui permet de stocker en toute sécurité des paires clé/valeur simple. Elle explique comment utiliser la classe, les Implémentations spécifiques par plateforme et les limitations.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 2dfdb7051b269e73c68290a557849b9ae606c165
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353293"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials : Stockage sécurisé

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe **SecureStorage** permet de stocker en toute sécurité des paires clé/valeur simple.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **SecureStorage** quelques étapes de configurations spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requis.

# <a name="iostabios"></a>[iOS](#tab/ios)

Lorsque vous développez sur le simulateur iOS, activer la **trousseau** droit et ajouter un groupe d’accès au trousseau pour l’identificateur de bundle de l’application.

Ouvrez le **Entitlements.plist** dans le projet iOS et trouver le **trousseau** droit et activez-la. Cela ajoutera automatiquement l’identificateur de l’application en tant que groupe.

Dans les propriétés du projet, sous **signature du Bundle iOS** définir le **droits personnalisés** à **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requis.

-----

## <a name="using-secure-storage"></a>Utilisation de **SecureStorage**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour enregistrer une valeur pour une donnée _clé_ dans le stockage sécurisé :

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

Pour récupérer une valeur à partir du stockage sécurisé :

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> Si aucune valeur n’est associé à la clé demandée, `GetAsync` retournera `null`.

Pour supprimer une clé spécifique, appelez :

```csharp
SecureStorage.Remove("oauth_token");
```

Pour supprimer toutes les clés, appelez :

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le [Android KeyStore](https://developer.android.com/training/articles/keystore.html) est utilisé pour stocker la clé de chiffrement utilisée pour chiffrer la valeur avant de l’enregistrer dans un [préférences partagées](https://developer.android.com/training/data-storage/shared-preferences.html) avec un nom de fichier de **[votre-application-PACKAGE-ID] .xamarinessentials** .  La clé utilisée dans le fichier de préférences partagées est un _hachage MD5_ de la clé passée dans le `SecureStorage` API.

## <a name="api-level-23-and-higher"></a>Niveau d’API 23 et versions ultérieures

Sur les niveaux d’API plus récente, une **AES** clé est obtenue à partir du magasin de clés Android et utilisée avec un **AES/GCM/NoPadding** chiffrement pour chiffrer la valeur avant d’être stockée dans le fichier de préférences partagées.

## <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieur

Sur les niveaux d’API plus anciennes, le magasin de clés Android prend uniquement en charge stockage **RSA** clés, qui est utilisé avec un **ECB/RSA/PKCS1Padding** chiffrement pour chiffrer un **AES** clé (au hasard généré lors de l’exécution) et stockées dans le fichier de préférences partagées sous la clé _SecureStorageKey_, s’il n’a pas déjà été généré.

**SecureStorage** utilise le [préférences](preferences.md) API et elle suit la persistance des données même décrites dans le [préférences](preferences.md#persistence) documentation. Si un appareil est mis à niveau à partir du niveau d’API 22 inférieur ou égal au niveau d’API 23 et versions ultérieures, ce type de chiffrement continuera à être utilisée, sauf si l’application est désinstallée ou **RemoveAll** est appelée.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Trousseau](https://developer.xamarin.com/api/type/Security.SecKeyChain/) est utilisé pour stocker les valeurs en toute sécurité sur les appareils iOS.  Le `SecRecord` utilisé pour stocker la valeur a un `Service` la valeur **[YOUR APP-BUNDLE-ID] .xamarinessentials**.

Dans certains cas, les données de trousseau sont synchronisées avec iCloud et désinstallation de l’application ne peut retirer les valeurs sécurisés d’iCloud et autres périphériques de l’utilisateur.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) est utilisé pour les valeurs chiffrées en toute sécurité sur les appareils UWP.

Valeurs chiffrées sont stockées dans `ApplicationData.Current.LocalSettings`, à l’intérieur d’un conteneur avec un nom de **[votre-application-ID] .xamarinessentials**.

**SecureStorage** utilise le [préférences](preferences.md) API et elle suit la persistance des données même décrites dans le [préférences](preferences.md#persistence) documentation.

-----

## <a name="limitations"></a>Limitations

Cette API est destinée à stocker de petites quantités de texte.  Performances peuvent être lentes si vous essayez d’utiliser pour stocker de grandes quantités de texte.

## <a name="api"></a>API

- [Code source de SecureStorage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Documentation de l’API de SecureStorage](xref:Xamarin.Essentials.SecureStorage)
