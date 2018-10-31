---
title: 'Xamarin.Essentials : Connectivity
description: La classe Connectivity vous permet de surveiller les modifications des conditions réseau de l'appareil, de vérifier la connexion réseau actuelle et comment l'appareil est connecté.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 96b4ee0487034c651bec1dfb168fed7567b63c96
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353696"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials : Connectivity

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe Connectivity vous permet de surveiller les modifications des conditions réseau de l'appareil, de vérifier la connexion réseau actuelle et comment l'appareil est connecté.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **Connectivity**, quelques étapes de configuration spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

L'autorisation `AccessNetworkState` est obligatoire et doit être configurée dans le projet Android. Elle peut être ajoutée comme suit :

Ouvrez le fichier **AssemblyInfo.cs** dans le dossier **Properties** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

OU mettez à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** dans le dossier **Properties** et ajoutez le code suivant dans le nœud **manifest**.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez-en les propriétés. Sous **Android manifest**, repérez la zone « **Required permissions:** » et cochez l'autorisation **Access Network State**. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-connectivity"></a>Utilisation de Connectivity

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Vérifiez l'accès actuel au réseau :

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Accès réseau](xref:Xamarin.Essentials.NetworkAccess) se situe dans les catégories suivantes :bill

* **Internet** : accès local et à internet.
* **ConstrainedInternet** : accès à Internet limité. Indique une connectivité via un portail captif, où l’accès à un portail Web est assuré, mais l'accès à Internet nécessite la fourniture d'informations d’identification spécifiques via un portail.
* **Local** : accès au réseau local uniquement.
* **None** : aucune connectivité n’est disponible.
* **Unknown** : impossible de déterminer la connectivité internet.

Vous pouvez vérifier quel [profil de connexion](xref:Xamarin.Essentials.ConnectionProfile) l'appareil utilise de manière active à l’aide de :

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Chaque fois que le profil de connexion ou l'accès réseau change, vous pouvez recevoir un événement au déclenchement :

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Limitations

Il faut noter qu'il est possible que l'accès à Internet soit signalé par NetworkAccess sans toutefois qu'un accès web complet soit disponible. En raison du fonctionnement de la connectivité sur chaque plateforme, il n'est possible de garantir que la disponibilité d'une connexion. Par exemple, l'appareil peut être connecté à un réseau Wi-Fi alors que le routeur est déconnecté d'internet. Dans ce cas, internet peut être signalé sans qu'une connexion active soit disponible.

## <a name="api"></a>API

* [Code source de connectivité](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Documentation de l’API de connectivité](xref:Xamarin.Essentials.Connectivity)
