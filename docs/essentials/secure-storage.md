---
title: 'Xamarin.Essentials : Le stockage sécurisé'
description: Ce document décrit la classe SecureStorage dans Xamarin.Essentials, ce qui permet de stocker en toute sécurité des paires clé/valeur simple. Elle explique comment utiliser la classe, les caractéristiques d’implémentation de plate-forme et les limitations.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: df2aa1fd23976e8db34d7c466317a8630408af7a
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080350"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials : Le stockage sécurisé

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **SecureStorage** classe permet de stocker en toute sécurité des paires clé/valeur simple.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **SecureStorage** fonctionnalités, le programme d’installation spécifique à la plateforme suivante est requise :

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requise.

# <a name="iostabios"></a>[iOS](#tab/ios)

Lorsque vous développez sur le simulateur iOS, activer la **trousseau** droit et ajouter un groupe trousseau d’accès pour l’identificateur de lot de l’application.

Ouvrez le **Entitlements.plist** dans le projet iOS et rechercher la **trousseau** droit et activez-le. L’identificateur de l’application sont automatiquement ajoutés en tant que groupe.

Dans les propriétés du projet, sous **signature d’offre groupée iOS** définir le **les droits personnalisés** à **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-secure-storage"></a>À l’aide du stockage sécurisé

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour enregistrer une valeur pour une donnée _clé_ dans le stockage sécurisé :

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Pour récupérer une valeur à partir du stockage sécurisé :

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

Pour supprimer une clé spécifique, appelez :

```csharp
SecureStorage.Remove("oauth_token");
```

Pour supprimer toutes les clés, appelez :

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>Caractéristiques d’implémentation de plate-forme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le [KeyStore Android](https://developer.android.com/training/articles/keystore.html) est utilisé pour stocker la clé de chiffrement utilisée pour chiffrer la valeur avant d’être enregistré dans un [partagé des préférences](https://developer.android.com/training/data-storage/shared-preferences.html) avec un nom de fichier de **[votre-APP-PACKAGE-ID] .xamarinessentials** .  La clé utilisée dans le fichier de préférences partagées est un _hachage MD5_ de la clé passée dans le `SecureStorage` l’API.

## <a name="api-level-23-and-higher"></a>Niveau de l’API 23 et versions ultérieures

Sur les niveaux d’API plus récente, une **AES** clé est obtenue à partir du magasin de clés Android et utilisée avec un **AES/GCM/NoPadding** chiffrement pour chiffrer la valeur avant d’être stockée dans le fichier de préférences partagées.

## <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieure

Sur les niveaux d’API plus anciennes, le magasin de clés Android prend uniquement en charge le stockage **RSA** clés, qui est utilisé avec un **RSA/ECB/PKCS1Padding** chiffrement pour chiffrer un **AES** clé (au hasard généré lors de l’exécution) et stockés dans le fichier de préférences partagées sous la clé _SecureStorageKey_, s’il n’a pas déjà été généré.

Toutes les valeurs chiffrées seront supprimés lors de l’application est désinstallée de l’appareil.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Trousseau](https://developer.xamarin.com/api/type/Security.SecKeyChain/) est utilisé pour stocker les valeurs en toute sécurité sur les appareils iOS.  Le `SecRecord` utilisé pour stocker la valeur a un `Service` la valeur **[votre---ID d’offre groupée] .xamarinessentials**.

Dans certains cas, les données de trousseau sont synchronisées avec iCloud, et la désinstallation de l’application ne pouvez pas supprimer les valeurs sécurisés à partir d’iCloud et autres périphériques de l’utilisateur.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) est utilisé pour les valeurs d’encryped en toute sécurité sur les appareils de la plateforme Windows universelle.

Les valeurs encryped sont stockées dans `ApplicationData.Current.LocalSettings`, à l’intérieur d’un conteneur avec un nom de **[votre-APP-ID] .xamarinessentials**.

Désinstallation de l’application entraîne le _LocalSettings_et des valeurs chiffrées tout à supprimer également.

-----

## <a name="limitations"></a>Limitations

Cette API est destinée à stocker de petites quantités de texte.  Performances peuvent être lent si vous essayez d’utiliser pour stocker de grandes quantités de texte.

## <a name="api"></a>API

- [Code source de SecureStorage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Documentation de l’API de SecureStorage](xref:Xamarin.Essentials.SecureStorage)
