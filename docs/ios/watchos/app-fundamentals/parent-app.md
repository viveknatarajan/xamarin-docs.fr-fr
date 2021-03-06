---
title: Utilisation de l’Application parente dans Xamarin watchOS
description: Ce document décrit comment travailler avec une application de parent watchOS dans Xamarin. Il aborde les extensions d’application WatchKit, des applications iOS, un stockage partagé et bien plus encore.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 74b9243b1b985cba6d815a673d1abed054eda234
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61409863"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Utilisation de l’Application parente dans Xamarin watchOS

> [!IMPORTANT]
> Accéder à l’application parent à l’aide uniquement des exemples ci-dessous fonctionne sur les applications de cadran watchOS 1.


Il existe différentes façons de communiquer entre l’application de surveillance et de l’application iOS qui il est fourni avec :

- Espion extensions peut [appeler une méthode](#code) par rapport à l’application parente qui s’exécute en arrière-plan sur l’iPhone.

- Espion extensions peut [partagent un emplacement de stockage](#storage) avec l’application iPhone parent.

- À l’aide de remise pour passer des données à partir d’un coup de œil ou une Notification à l’application Watch, envoi de l’utilisateur à un contrôleur d’interface spécifique dans l’application.

L’application parente est également parfois appelée l’application de conteneur.


<a name="code" />

## <a name="run-code"></a>Exécuter du Code

Communication entre une extension watch et l’application iPhone parent est illustrée dans le [GpsWatch exemple](https://developer.xamarin.com/samples/GpsWatch).
Votre extension watch peut demander l’application iOS de parent à procéder à un traitement sur son nom à l’aide de la `OpenParentApplication` (méthode).

Cela est particulièrement utile pour longues tâches (y compris les demandes réseau) - uniquement le parent application iOS peuvent tirer parti du traitement en arrière-plan pour effectuer ces tâches et enregistrez les données récupérées dans un emplacement accessible à l’extension watch.



### <a name="watch-kit-app-extension"></a>Regardez le Kit d’Extension d’application

Appelez le `WKInterfaceController.OpenParentApplication` dans votre extension d’application watch. Elle retourne un `bool` qui indique si la demande de méthode a été envoyée avec succès ou non. Vérifier le `error` paramètre dans la réponse `Action` pour déterminer si tout s’est produite lors de la méthode en cours d’exécution dans l’application iPhone.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>Application iOS

Tous les appels à partir d’une extension d’application espion sont routées via l’application iPhone `HandleWatchKitExtensionRequest` (méthode).
Si que vous apportez aux différentes demandes de l’application Apple watch, cette méthode devez interroger le `userInfo` dictionnaire pour déterminer comment traiter la demande.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>Stockage partagé

Si vous configurez un [groupe app](~/ios/watchos/app-fundamentals/app-groups.md) puis extensions iOS 8 (y compris les extensions d’espion) peuvent partager des données avec l’application parente.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>Valeurs NSUserDefaults

Le code suivant peut être écrit dans l’extension d’application watch et l’application iPhone parent afin qu’ils peuvent référencer un ensemble commun de `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Fichiers

L’extension d’application et regardez iOS peut également partager des fichiers à l’aide d’un chemin d’accès de fichier commun.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Remarque : si le chemin d’accès est `null` puis vérifiez le [configuration du groupe application](~/ios/watchos/app-fundamentals/app-groups.md) afin de garantir les profils de provisionnement ont été configurés correctement et ont été téléchargés/installé sur l’ordinateur de développement.

Pour plus d’informations, consultez le [application groupe fonctionnalités](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.

## <a name="wormholesharp"></a>WormHoleSharp

Un mécanisme d’open source populaires pour watchOS 1 (selon [MMWormHole](https://github.com/mutualmobile/MMWormhole)) pour transmettre des données ou les commandes entre l’application parente et l’application watch.

Vous pouvez configurer WormHole à l’aide d’un groupe de l’application comme suit dans votre application iOS et regardez l’extension :

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Téléchargez le C# version [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Liens associés

- [GpsWatch (exemple)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WormHoleSharp (exemple)](https://github.com/Clancey/WormHoleSharp)
- [Référence de WKInterfaceController d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple partage de données avec votre application de conteneur](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
