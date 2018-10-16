---
title: 'Xamarin.Essentials : Connectivité'
description: La classe de connectivité dans Xamarin.Essentials vous permet de surveiller les modifications des conditions de réseau de l’appareil, vérifiez l’accès réseau actuelle, et comment il est actuellement connecté.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 96b4ee0487034c651bec1dfb168fed7567b63c96
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353696"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials : Connectivité

![Version préliminaire NuGet](~/media/shared/pre-release.png)
La classe **Connectivity** vous permet d'obtenir des informations sur la connexion de l'appareil au réseau.

## <a name="getting-started"></a>Prise en main

Pour accéder aux fonctionnalités de l'API **connectivité** quelques étapes de configurations spécifiques aux plateformes sont nécessaires.

# <a name="androidtabandroid"></a>[Android](#tab/android)

L'autorisation `AccessNetworkState` est obligatoire et doit être configurée dans le projet Android. Elle peut être ajoutée comme suit :

Ouvrez le fichier **AssemblyInfo.cs** sous le dossier **propriétés** et ajoutez :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

OU mettez à jour le manifeste Android :

Ouvrez le fichier **AndroidManifest.xml** sous le dossier **propriétés** et ajoutez le code suivant à l’intérieur du nœud **manifest**.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Ou cliquez avec le bouton droit sur le projet Android et ouvrez les propriétés du projet. Sous **manifeste Android** trouvez la zone "**autorisations requises :**" et cochez l'autorisation **état d’accès réseau**. Cela met automatiquement à jour le fichier **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requis.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requis.

-----

## <a name="using-connectivity"></a>Utilisation de **Connectivity**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Vérifier si l'appareil peut accèder au réseau :

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Accès réseau](xref:Xamarin.Essentials.NetworkAccess) possède les valeurs possibles suivantes :

* **Internet** : accès local et internet.
* **ConstrainedInternet** : l’accès à internet limité. Indique la connectivité via un portail captif, l’accès à Internet nécessite que les informations d’identification spécifiques soient fournies via un portail.
* **Local** : accès local uniquement.
* **None** : aucune connectivité n’est disponible.
* **Unknown** : impossible de déterminer la connectivité internet.

Vous pouvez vérifier quel [profil de connexion](xref:Xamarin.Essentials.ConnectionProfile) actuellement actif à l’aide de :

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

L'événement `ConnectivityChanged` est mis à disposition pour surveiller l'évolution du profil de connexion :

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

Il est important de noter que ce n'est pas parceque `NetworkAccess` indique `Internet` que l'appareil peut accèder a tout internet. En raison des différences entre les plateformes concernant la connectivité impossible de garantir l'accès à une certaine URL à 100%. Par exemple, l'appareil peut être connecté à un routeur qui n'est pas connecté à internet. Dans ce cas `NetworkAccess` indiquera `Internet` alors qu'une connexion active ne sera pas disponible.

## <a name="api"></a>API

* [Code source de connectivité](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Documentation de l’API de connectivité](xref:Xamarin.Essentials.Connectivity)
