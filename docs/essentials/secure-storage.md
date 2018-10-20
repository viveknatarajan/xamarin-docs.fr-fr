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

La classe **SecureStorage** permet de stocker en sécurité des paires clé/valeur simples.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **SecureStorage** quelques étapes de configurations spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Aucune configuration supplémentaire n’est requise.

# <a name="iostabios"></a>[iOS](#tab/ios)

Lorsque vous déployez sur le simulateur iOS, activez les permissions d'accès au **trousseau** et ajoutez un groupe d’accès au trousseau pour l’identificateur de bundle de l’application.

Dans Visual Studio, ouvrez le fichier **Entitlements.plist** dans le projet iOS et trouvez la permission d'accès au **trousseau** et activez-la. Cela ajoutera automatiquement l’identificateur de l’application en tant que groupe.

Dans les propriétés du projet, sous **signature du Bundle iOS** définir le champ **droits personnalisés** à **Entitlements.plist**.

Alternativement vous pouvez dupliquer le fichier **Entitlements.plist** existant en **Entitlements.Simulator.plist** et ajouter ces lignes dans le nœud `dict` du fichier **Entitlements.Simulator.plist** :

 ```xml
  <key>keychain-access-groups</key>
  <array>
    <string>com.[company].[app]</string>
  </array>
```

Vous pouvez ensuite ajouter le PropertyGroup suivant dans le fichier .csproj de votre application iOS :

```xml
  <PropertyGroup Condition=" '$(Platform)' == 'iPhoneSimulator' ">
    <CodesignEntitlements>Entitlements.Simulator.plist</CodesignEntitlements>
  </PropertyGroup>
```

Ceci activera automatiquement la configuration de l'accès au trousseau lors de l'execution de l'application sur un simulateur.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-secure-storage"></a>Utilisation de **SecureStorage**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Pour enregistrer une valeur sous une certaine _clé_ dans le stockage sécurisé :

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

Pour récupérer une valeur sous une certaine _clé_ à partir du stockage sécurisé :

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

Pour supprimer une valeur sous une certaine _clé_ :

```csharp
SecureStorage.Remove("oauth_token");
```

Pour supprimer tous les ensembles clé-valeur du stockage sécurisé :

```csharp
SecureStorage.RemoveAll();
```

## <a name="platform-implementation-specifics"></a>Implémentations spécifiques par plateforme

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le [KeyStore Android](https://developer.android.com/training/articles/keystore.html) est utilisé pour stocker la clé de chiffrement utilisée pour chiffrer la valeur avant de l’enregistrer dans les [préférences partagées](https://developer.android.com/training/data-storage/shared-preferences.html) avec comme nom de fichier "**[YOUR APP-PACKAGE-ID].xamarinessentials**".  La clé utilisée dans le fichier de préférences partagées est un _hachage MD5_ de la clé passée à l'API `SecureStorage`.

## <a name="api-level-23-and-higher"></a>Niveau d’API 23 et versions ultérieures

Sur les niveaux d’API plus récents, une clé de type **AES** est obtenue à partir du magasin de clés Android et est utilisée avec un chiffrement **AES/GCM/NoPadding** pour chiffrer la valeur avant d’être stockée dans le fichier de préférences partagées.

## <a name="api-level-22-and-lower"></a>API de niveau 22 et inférieur

Sur les niveaux d’API plus anciens, le magasin de clés Android prend uniquement en charge les clés de type **RSA**, qui sont ensuite utilisés avec un chiffrement **ECB/RSA/PKCS1Padding** pour chiffrer une clé **AES** (généré au hasard lors de l’exécution) et stockées dans le fichier de préférences partagées sous la clé _SecureStorageKey_, si elle n’a pas déjà été générée.

Si un appareil est mis à niveau à partir du niveau d’API 22 ou inférieur vers un niveau d’API 23 ou supérieur, le type de chiffrement continuera à être utilisée, sauf si l’application est désinstallée ou que la fonction **RemoveAll** est appelée.

# <a name="iostabios"></a>[iOS](#tab/ios)

Le [Trousseau](https://developer.xamarin.com/api/type/Security.SecKeyChain/) est utilisé pour stocker les valeurs en toute sécurité sur les appareils iOS.

Dans certains cas, les données de trousseau sont synchronisées avec iCloud et la désinstallation de l’application ne suffit pas pour supprimer les valeurs sécurisés sur iCloud et sur les autres périphériques de l’utilisateur. Il faudra que l'utilisateur les supprime manuellement ou que l'application les écrase.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) est utilisé pour sécuriser les valeurs sur les appareils UWP.

Les valeurs chiffrées sont stockées dans les `ApplicationData.Current.LocalSettings`, à l’intérieur d’un conteneur avec comme nom **[votre-application-ID] .xamarinessentials**.

-----

## <a name="persistence"></a>Persistance

**SecureStorage** utilise l'API [préférences](preferences.md) et elle suit les mêmes spécificités de persistance des données décrites dans la documentation des [préférences](preferences.md#persistence).

## <a name="limitations"></a>Limitations

Lorsque vous stockez une chaîne de caractères, cette API est destinée à stocker de petites quantités de texte. Les performances peuvent être réduites si vous essayez d’utiliser cette API pour stocker de grandes quantités de texte.

## <a name="api"></a>API

- [Code source de SecureStorage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [Documentation de l’API de SecureStorage](xref:Xamarin.Essentials.SecureStorage)
