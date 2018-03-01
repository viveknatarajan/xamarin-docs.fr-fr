---
title: Prise en main de macOS
ms.topic: article
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 5e2f14e7b29f85e838563914089743f56239d7bb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-macos"></a>Prise en main de macOS


## <a name="what-you-will-need"></a>Vous devez

* Suivez les instructions de notre [prise en main de Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide.

* Un assembly .NET à utiliser avec **Embeddinator-4000**.

* Une Application / Cocoa macOS

Continuer une fois que vous avez suivi les instructions de notre [prise en main de Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide. Si vous disposez déjà d’un assembly .NET, vous pouvez passer directement dans **Embeddinator-4000 à l’aide de** section.

## <a name="creating-a-net-assembly"></a>Création d’un assembly .NET

Pour générer un assembly .NET que vous devrez ouvrir [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) et créer un nouveau **projet de bibliothèque .NET** faisant *fichier > Nouvelle Solution > autres > .NET > bibliothèque*. Cliquez sur Suivant et donner *météo* en tant que le *nom du projet*, puis cliquez sur *créer*.

Suivez les étapes suivantes :

1. Supprimer le **MyClass.cs** fichier et le **propriétés** dossier.

2. Cliquez avec le bouton droit sur *projet de météo > Ajouter > nouveau fichier.*

3. Sélectionnez *classe vide* et utiliser **XAMWeatherFetcher** comme nom, puis cliquez sur Nouveau.

4. Remplacez le contenu de *XAMWeatherFetcher.cs* avec le code suivant :

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

Vous remarquerez que `Using System.Json;` génère une erreur ; pour y remédier, nous devons effectuer les opérations suivantes :

1. Double-cliquez sur le **références** dossier.

2. Cliquez sur le **Packages** onglet.

3. Vérifiez **System.Json**.

4. Click **Ok**.

Une fois la commande ci-dessus fait il nous suffit est build notre assembly .NET en cliquant sur *menu Générer > générer tout* ou ⌘ + b. Build réussie de message doit apparaître dans la barre d’état supérieur.

Avec le bouton droit cliquez maintenant sur *météo* le nœud de projet et sélectionnez *révéler*. Dans le Finder, accédez à la *bin/Debug* à l’intérieur de dossier ; il vous trouverez **Weather.dll.**

## <a name="using-embeddinator-4000"></a>À l’aide de Embeddinator-4000.

Si vous avez installé à l’aide de notre programme d’installation pkg Embeddinator-4000 et démarrer une nouvelle session Terminal Server après l’installation, vous devez être en mesure d’utiliser le **objcgen** commande (dans le cas contraire, vous pouvez utiliser son chemin d’accès absolu : `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`) ; **objcgen** est l’outil que nous avons besoin pour générer une bibliothèque native à partir d’un assembly .NET.

Ouvrez terminal, `cd` dans le dossier contenant Weather.dll et exécutez **objcgen** avec les arguments indiqués ci-dessous :

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

Tout ce dont vous avez besoin seront placés dans le **sortie** répertoire suivant pour *Weather.dll*. Si vous disposez de votre propre assembly .NET, remplacez *Weather.dll* et suivez la même procédure ci-dessus.

## <a name="using-the-generated-output-in-an-xcode-project"></a>À l’aide de la sortie générée dans un projet Xcode

Ouvrez Xcode et créez un nouveau **macOS/Cocoa Application** et nommez-le **MyWeather**. Cliquez avec le bouton droit sur le *MyWeather le nœud de projet*, sélectionnez *Ajout de fichiers à « MyWeather »*, accédez à la **sortie** répertoire créé par *Embeddinator-4000.* et ajouter les fichiers suivants :

* Bindings.h
* embeddinator.h
* glib.h
* mono-support.h
* mono_embeddinator.h
* objc-support.h
* libWeather.dylib
* Weather.dll

Assurez-vous que **copier des éléments si nécessaire** est activée dans le panneau d’options de la boîte de dialogue de fichier.

Maintenant nous avons besoin pour vous assurer que **libWeather.dylib** et **Weather.dll** obtenir dans le lot d’applications :

* Cliquez sur *MyWeather le nœud de projet*.
* Sélectionnez *générer les Phases* onglet.
* Ajouter un nouveau *Phase de copie de fichiers*.
* Sur *Destination* sélectionnez **infrastructures** et ajoutez **libWeather.dylib**.
* Ajouter un nouveau *Phase de copie de fichiers*.
* Sur *Destination* sélectionnez **exécutables**, ajoutez **Weather.dll** et assurez-vous que *signer du Code lors de la copie* est activée.

À présent ouvrir **ViewController.m** et remplacez son contenu avec :

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

Enfin exécuter le projet Xcode et quelque chose comme ceci s’afficheront :

![En cours d’exécution MyWeather exemple](macos-images/weather-from-csharp-macos.png)

Vous trouverez un exemple plus complet et attrayantes [ici](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
