---
title: Prise en main d’e/s
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 34afdd9e91ebfbe7ad57c7eec6ba7f05fff1a2aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-ios"></a>Prise en main d’e/s


## <a name="requirements"></a>Spécifications

Outre les exigences de nos [prise en main de Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide, vous devez également :

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/) ou version ultérieure

## <a name="hello-world"></a>Hello World

Première Commençons par créer un exemple de world hello simple en c#.

### <a name="create-c-sample"></a>Créer l’exemple c#

Ouvrez Visual Studio pour Mac, créez un nouveau projet de bibliothèque de classes d’e/s, nommez-le `hello-from-csharp`et enregistrez-le à `~/Projects/hello-from-csharp`.

Remplacez le code dans le `MyClass.cs` fichier avec l’extrait de code suivant :

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Générez le projet, l’assembly résultant sera enregistré sous `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`.

### <a name="bind-the-managed-assembly"></a>Lier l’assembly managé

Exécutez l’embeddinator pour créer une infrastructure native pour l’assembly managé :

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Le framework sera placé dans `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Utilisez la sortie générée dans un projet Xcode

Ouvrez Xcode et créer un nouveau iOS Application vue unique, nommez-le `hello-from-csharp` et sélectionnez le **Objective-C** language.

Ouvrir le `~/Projects/hello-from-csharp/output` répertoire dans le Finder, sélectionnez `hello-from-csharp.framework`, faites-le glisser vers le projet Xcode et déposez-le juste au-dessus du `hello-from-csharp` dossier dans le projet.

! (Glisser- déposer framework) Images/Hello-from-CSharp-IOS-Drag-Drop-Framework.png)

Assurez-vous que `Copy items if needed` est activée dans la boîte de dialogue qui s’affiche, puis cliquez sur `Finish`.

![Copier des éléments si nécessaire](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Sélectionnez le `hello-from-csharp` de projet et accédez à la `hello-from-csharp` la cible **onglet Général**. Dans le **binaires incorporées** section, ajoutez `hello-from-csharp.framework`.

![Fichiers binaires incorporées](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Ouvrez ViewController.m et remplacez le contenu avec :

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Enfin exécuter le projet Xcode et quelque chose comme ceci s’afficheront :

![Bonjour à partir de l’exemple de code c# en cours d’exécution dans le simulateur](ios-images/hello-from-csharp-ios.png)
