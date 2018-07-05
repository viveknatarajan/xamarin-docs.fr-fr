---
title: 'Xamarin.Essentials : Stockage sécurisé'
description: Ce document décrit la classe SecureStorage dans Xamarin.Essentials, ce qui permet de stocker en toute sécurité des paires clé/valeur simple. Elle explique comment utiliser la classe, les caractéristiques de mise en œuvre la plateforme et les limitations.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: fae5f5f0f15d80e2f3bdce26b8beb5f6fae2f81f
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403440"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials : Stockage sécurisé

![Version préliminaire NuGet](~/media/shared/pre-release.png)

Le **SecureStorage** classe permet de stocker en toute sécurité des paires clé/valeur simple.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **SecureStorage** fonctionnalité, la configuration spécifique à la plateforme suivante est requise :

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requis.

# <a name="iostabios"></a>[iOS](#tab/ios)

Lorsque vous développez sur le simulateur iOS, activer la **trousseau** droit et ajouter un groupe d’accès au trousseau pour l’identificateur de bundle de l’application.

Ouvrez le **Entitlements.plist** dans le projet iOS et trouver le **trousseau** droit et activez-la. Cela ajoutera automatiquement l’identificateur de l’application en tant que groupe.

Dans les propriétés du projet, sous **signature du Bundle iOS** définir le **droits personnalisés** à **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requis.

-----

## <a name="using-secure-storage"></a>L’utilisation du stockage sécurisé

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


## <a name="platform-implementation-specifics"></a>Caractéristiques de mise en œuvre la plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le [Android KeyStore](https://developer.android.com/training/articles/keystore.html) est utilisé pour stocker la clé de chiffrement utilisée pour chiffrer la valeur avant de l’enregistrer dans un [préférences partagées](https://developer.android.com/training/data-storage/shared-preferences.html) avec un nom de fichier de **[votre-application-PACKAGE-ID] .xamarinessentials** .  La clé utilisée dans le fichier de préférences partagées est un _hachage MD5_ de la clé passée dans le `SecureStorage` l’API.

## <a name="api-level-23-and-higher"></a>Niveau d’API 23 et versions ultérieures

Sur les niveaux d’API plus récente, une **AES** clé est obtenue à partir du magasin de clés Android et utilisée avec un **AES/GCM/NoPadding** chiffrement pour chiffrer la valeur avant d’être stockée dans le fichier de préférences partagées.

## <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieur

Sur les niveaux d’API plus anciennes, le magasin de clés Android prend uniquement en charge stockage **RSA** clés, qui est utilisé avec un **ECB/RSA/PKCS1Padding** chiffrement pour chiffrer un **AES** clé (au hasard généré lors de l’exécution) et stockées dans le fichier de préférences partagées sous la clé _SecureStorageKey_, s’il n’a pas déjà été généré.

Toutes les valeurs chiffrées seront supprimés lors de l’application est désinstallée de l’appareil.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Trousseau](https://developer.xamarin.com/api/type/Security.SecKeyChain/) est utilisé pour stocker les valeurs en toute sécurité sur les appareils iOS.  Le `SecRecord` utilisé pour stocker la valeur a un `Service` la valeur **[YOUR APP-BUNDLE-ID] .xamarinessentials**.

Dans certains cas, les données de trousseau sont synchronisées avec iCloud et désinstallation de l’application ne peut retirer les valeurs sécurisés d’iCloud et autres périphériques de l’utilisateur.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) est utilisé pour les valeurs chiffré en toute sécurité sur les appareils UWP.

Les valeurs chiffré sont stockées dans `ApplicationData.Current.LocalSettings`, à l’intérieur d’un conteneur avec un nom de **[votre-application-ID] .xamarinessentials**.

Désinstallation de l’application entraîne la _LocalSettings_et des valeurs chiffrées tout à supprimer également.

-----

## <a name="limitations"></a>Limitations

Cette API est destinée à stocker de petites quantités de texte.  Performances peuvent être lentes si vous essayez d’utiliser pour stocker de grandes quantités de texte.

## <a name="api"></a>API

- [Code source de SecureStorage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Documentation de l’API de SecureStorage](xref:Xamarin.Essentials.SecureStorage)
