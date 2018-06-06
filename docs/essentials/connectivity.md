---
title: 'Xamarin.Essentials : connectivité'
description: La classe de connectivité dans Xamarin.Essentials vous permet de surveiller les modifications apportées dans les conditions de réseau du périphérique, vérifiez l’accès réseau en cours, et comment il est actuellement connecté.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 54c165e15e725caaecb1573b74cfe295170db141
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782864"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials : connectivité

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **connectivité** classe vous permet de contrôler les modifications apportées à des conditions réseau du périphérique, vérifiez l’accès réseau en cours et comment il est actuellement connecté.

## <a name="getting-started"></a>Prise en main

Pour accéder à la **connectivité** fonctionnalité de la configuration spécifique plate-forme suivante est requise.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Le `AccessNetworkState` autorisation est requise et doit être configurée dans le projet Android. Il peut être ajouté comme suit :

Ouvrez le **AssemblyInfo.cs** de fichiers sous le **propriétés** dossier et ajouter :

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

OU mettre à jour le manifeste Android :

Ouvrez le **AndroidManifest.xml** fichier sous le **propriétés** dossier et ajoutez le code suivant à l’intérieur de la **manifeste** nœud.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Ou cliquez avec le bouton droit sur le projet Anroid et ouvrez les propriétés du projet. Sous **manifeste Android** de trouver la **les autorisations requises :** zone et vérifiez la **état d’accès réseau** autorisation. Met automatiquement à jour la **AndroidManifest.xml** fichier.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aucune configuration supplémentaire n’est requise.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Aucune configuration supplémentaire n’est requise.

-----

## <a name="using-connectivity"></a>À l’aide de la connectivité

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

Vérifier l’accès réseau actuelle :

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Accès au réseau](xref:Xamarin.Essentials.NetworkAccess) se situe dans les catégories suivantes :

* **Internet** – accès Local et internet.
* **ConstrainedInternet** – limité l’accès à internet. Indique la connectivité au portail captif, où l’accès local à un portail web est fourni, mais l’accès à Internet nécessite que les informations d’identification spécifiques sont fournies via un portail.
* **Local** : Local uniquement les accès réseau.
* **Aucun** – aucune connectivité n’est disponible.
* **Inconnu** : Impossible de déterminer la connectivité internet.

Vous pouvez vérifier que le type de [profil de connexion](xref:Xamarin.Essentials.ConnectionProfile) l’appareil est activement à l’aide de :

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Chaque fois que le profil de connexion ou le réseau, accéder aux modifications, vous pouvez recevoir un événement de déclenchement :

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

Il est important de noter qu’il est possible que `Internet` est signalée par `NetworkAccess` mais un accès complet sur le web n’est pas disponible. En raison de fonctionne de la connectivité sur chaque plateforme, il peut uniquement garantir qu’une connexion est disponible. Par exemple, le périphérique peut être connecté à un réseau Wi-Fi, mais le routeur est déconnecté à partir d’internet. Dans cette instance d’Internet peut-être être signalé, mais une connexion active n’est pas disponible.

## <a name="api"></a>API

* [Code source de connectivité](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Documentation de l’API de connectivité](xref:Xamarin.Essentials.Connectivity)
