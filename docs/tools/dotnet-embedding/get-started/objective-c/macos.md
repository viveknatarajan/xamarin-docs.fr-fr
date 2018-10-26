---
title: Bien démarrer avec Mac OS
description: Ce document décrit comment commencer à utiliser l’incorporation de .NET avec macOS. Il aborde la configuration requise et présente un exemple d’application pour montrer comment lier l’assembly managé et utiliser la sortie générée dans un projet Xcode.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 3de47aa57df29f52f71508977ae017dff009c282
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121566"
---
# <a name="getting-started-with-macos"></a>Bien démarrer avec Mac OS

## <a name="what-you-will-need"></a>Vous aurez besoin

* Suivez les instructions fournies dans le [mise en route avec Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide.

## <a name="hello-world"></a>Hello World

Tout d’abord, créez un exemple de world hello simple en c#.

### <a name="create-c-sample"></a>Créer l’exemple c#

Ouvrez Visual Studio pour Mac, créez un projet de bibliothèque de classes Mac nommé **hello de csharp**et enregistrez-le à **~/Projects/hello-from-csharp**.

Remplacez le code dans le **MyClass.cs** fichier avec l’extrait de code suivant :

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Générez le projet. L’assembly résultant sera enregistré sous **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Lier l’assembly managé

Une fois que vous avez un assembly managé, la liaison en appelant l’incorporation de .NET.

Comme décrit dans la [installation](~/tools/dotnet-embedding/get-started/install/install.md) guide, cela est possible en tant qu’étape post-build dans votre projet, avec une cible MSBuild personnalisée, ou manuellement :

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Le framework sera placé dans **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Utiliser la sortie générée dans un projet Xcode

Ouvrez Xcode et créez une Application Cocoa. Nommez-le **hello de csharp** et sélectionnez le **Objective-C** language.

Ouvrez le **~/Projects/hello-from-csharp/output** répertoire dans le Finder, sélectionnez **hello de csharp.framework**, faites-le glisser vers le projet Xcode et déposez-le juste au-dessus le **hello-de-csharp**  dossier dans le projet.

![Faites glisser et déposez le framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Assurez-vous que **copier les éléments si besoin** est activée dans la boîte de dialogue qui s’affiche, puis cliquez sur **Terminer**.

![Copier des éléments si nécessaire](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Sélectionnez le **hello de csharp** de projet et accédez à la **hello de csharp** la cible **général** onglet. Dans le **Embedded binaire** , ajoutez **hello de csharp.framework**.

![Binaires incorporés](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Ouvrez **ViewController.m**et remplacez le contenu avec :

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Enfin, exécutez le projet Xcode et quelque chose comme ceci s’affiche :

![Bonjour de l’exemple c# en cours d’exécution dans le simulateur](macos-images/hello-from-csharp-mac.png)

Un exemple plus complet et plus attrayante [est disponible ici](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
