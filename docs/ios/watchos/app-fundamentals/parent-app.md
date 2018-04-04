---
title: Utilisation de l’Application parente
description: Partage des données entre l’application iOS et espion dans watchOS 1
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 769847cccb3e21fea4d8f45d8e5d0c0fb59bdd43
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-parent-application"></a>Utilisation de l’Application parente

_Partage des données entre l’application iOS et espion dans watchOS 1_

> [!IMPORTANT]
> L’accès à l’application parente uniquement à l’aide des exemples ci-dessous fonctionne sur les applications de surveillance watchOS 1.


Il existe différentes façons de communiquer entre l’application de surveillance et de l’application iOS qui il est fourni avec :

- Espion extensions peut [appeler une méthode](#code) par rapport à l’application parente qui s’exécute en arrière-plan sur l’iPhone.

- Espion extensions peut [partagent un emplacement de stockage](#storage) avec l’application iPhone parent.

- À l’aide de remise pour passer des données à partir d’un coup de œil ou de Notification à l’application de surveillance, envoi de l’utilisateur à un contrôleur d’interface spécifique dans l’application.

L’application parente est également parfois que l’application conteneur.


<a name="code" />

## <a name="run-code"></a>Exécuter du Code

Communication entre une extension de surveillance et de l’application iPhone parent est illustré dans le [GpsWatch exemple](https://developer.xamarin.com/samples/GpsWatch).
Votre extension de surveillance peut demander l’application iOS parent pour effectuer un traitement sur son nom à l’aide de la `OpenParentApplication` (méthode).

Ceci est particulièrement utile pour les longues tâches (y compris les demandes réseau) - uniquement le parent application iOS peuvent tirer parti du traitement en arrière-plan pour effectuer ces tâches et d’enregistrer les données récupérées dans un emplacement accessible à l’extension de surveillance.



### <a name="watch-kit-app-extension"></a>Regardez le Kit d’Extension d’application

Appelez le `WKInterfaceController.OpenParentApplication` dans votre extension d’application espion. Elle retourne un `bool` qui indique si la requête de méthode a été envoyée avec succès ou non. Vérifiez le `error` paramètre dans la réponse `Action` pour déterminer le cas échéant, s’est produite lors de la méthode en cours d’exécution dans l’application iPhone.

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
Si vous apportez aux différentes demandes de l’application de surveillance, cette méthode devez interroger la `userInfo` dictionnaire pour déterminer comment traiter la demande.


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

Si vous configurez un [groupe app](~/ios/watchos/app-fundamentals/app-groups.md) puis extensions iOS 8 (y compris les extensions espion) peuvent partager des données avec l’application parente.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Le code suivant peut être écrite dans l’extension d’application de surveillance et de l’application iPhone parent afin qu’ils peuvent faire référence à un ensemble commun de `NSUserDefaults`:

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

L’extension d’application et Espion iOS peut également partager des fichiers à l’aide d’un chemin d’accès de fichier courantes.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Remarque : si le chemin d’accès est `null` puis vérifiez le [configuration du groupe application](~/ios/watchos/app-fundamentals/app-groups.md) afin de garantir les profils de configuration ont été configurés correctement et ont été téléchargées/installé sur l’ordinateur de développement.

Pour plus d’informations, consultez la [fonctionnalités des applications du groupe](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) documentation.

## <a name="wormholesharp"></a>WormHoleSharp

Un mécanisme open source populaire pour watchOS 1 (selon [MMWormHole](https://github.com/mutualmobile/MMWormhole)) pour passer des commandes ou des données entre l’application parente et l’application de surveillance.

Vous pouvez configurer à l’aide d’un groupe d’application comme suit dans votre application iOS de repère et regardez extension :

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Télécharger la version c# [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Liens associés

- [GpsWatch (sample)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (sample)](https://github.com/Clancey/WormHoleSharp)
- [Référence de WKInterfaceController d’Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Partage de données avec votre application conteneur Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
