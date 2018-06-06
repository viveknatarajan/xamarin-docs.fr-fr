---
title: Prise en main de macOS
description: Ce document décrit comment commencer à utiliser .NET incorporation avec macOS. Il décrit la configuration requise et présente un exemple d’application pour illustrer comment lier l’assembly managé et utiliser la sortie générée dans un projet Xcode.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 38049eae5e420e5f3610341c2682fa92d2ac426e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793687"
---
# <a name="getting-started-with-macos"></a>Prise en main de macOS

## <a name="what-you-will-need"></a>Vous devez

* Suivez les instructions fournies dans le [prise en main de Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide.

## <a name="hello-world"></a>Hello World

Tout d’abord, créer un exemple de world hello simple en c#.

### <a name="create-c-sample"></a>Créer l’exemple c#

Ouvrez Visual Studio pour Mac, créez un nouveau projet de bibliothèque de classes Mac nommé **hello-de-csharp**et enregistrez-le **~/Projects/hello-from-csharp**.

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

Une fois que vous avez un assembly managé, la lier en appelant l’incorporation de .NET.

Comme décrit dans la [installation](~/tools/dotnet-embedding/get-started/install/install.md) guide, cela est possible en tant qu’étape post-build dans votre projet, avec une cible MSBuild personnalisée, ou manuellement :

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Le framework sera placé dans **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Utilisez la sortie générée dans un projet Xcode

Ouvrez Xcode et créez une nouvelle Application/Cocoa. Nommez-le **hello-de-csharp** et sélectionnez le **Objective-C** language.

Ouvrir le **~/Projects/hello-from-csharp/output** répertoire dans le Finder, sélectionnez **hello-de-csharp.framework**, faites-le glisser vers le projet Xcode et déposez-le juste au-dessus du **hello-de-csharp**  dossier dans le projet.

![Faites glisser et déposez le framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Assurez-vous que **copier des éléments si nécessaire** est activée dans la boîte de dialogue qui s’affiche, puis cliquez sur **Terminer**.

![Copier des éléments si nécessaire](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Sélectionnez le **hello-de-csharp** de projet et accédez à la **hello-de-csharp** la cible **général** onglet. Dans le **binaires incorporées** section, ajoutez **hello-de-csharp.framework**.

![Fichiers binaires incorporées](macos-images/hello-from-csharp-mac-embedded-binaries.png)

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

Enfin, exécutez le projet Xcode et quelque chose comme ceci s’afficheront :

![Bonjour à partir de l’exemple de code c# en cours d’exécution dans le simulateur](macos-images/hello-from-csharp-mac.png)

Un exemple plus complet et plus attrayants [sont disponibles ici](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
