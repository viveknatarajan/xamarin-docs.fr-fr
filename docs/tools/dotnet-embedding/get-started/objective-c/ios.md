---
title: Prise en main d’e/s
description: Ce document décrit comment démarrer à l’aide de l’incorporation de .NET avec iOS. Il décrit la configuration requise et présente un exemple d’application pour illustrer comment lier un assembly managé et utiliser la sortie dans un projet Xcode.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 683f448c3e0cdba1bbe3ba68f0db3225675f5830
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066895"
---
# <a name="getting-started-with-ios"></a>Prise en main d’e/s

## <a name="requirements"></a>Configuration requise

Outre les exigences de nos [prise en main de Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide, vous devez également :

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure

## <a name="hello-world"></a>Hello World

Tout d’abord, créer un exemple de world hello simple en c#.

### <a name="create-c-sample"></a>Créer l’exemple c#

Ouvrez Visual Studio pour Mac, créez un nouveau projet de bibliothèque de classes d’e/s, nommez-le **hello-de-csharp**et enregistrez-le **~/Projects/hello-from-csharp**.

Remplacez le code dans le **MyClass.cs** fichier avec l’extrait de code suivant :

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

Générez le projet et l’assembly résultant sera enregistré sous **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Lier l’assembly managé

Une fois que vous avez un assembly managé, la lier en appelant l’incorporation de .NET.

Comme décrit dans la [installation](~/tools/dotnet-embedding/get-started/install/install.md) guide, cela est possible en tant qu’étape post-build dans votre projet, avec une cible MSBuild personnalisée, ou manuellement :

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Le framework sera placé dans **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Utilisez la sortie générée dans un projet Xcode

Ouvrez Xcode, créer un nouveau iOS Application vue unique, nommez-le **hello-de-csharp**, puis sélectionnez le **Objective-C** language.

Ouvrir le **~/Projects/hello-from-csharp/output** répertoire dans le Finder, sélectionnez **hello-de-csharp.framework**, faites-le glisser vers le projet Xcode et déposez-le juste au-dessus du **hello-de-csharp**  dossier dans le projet.

! (Glisser- déposer framework) Images/Hello-from-CSharp-IOS-Drag-Drop-Framework.png)

Assurez-vous que **copier des éléments si nécessaire** est activée dans la boîte de dialogue qui s’affiche, puis cliquez sur **Terminer**.

![Copier des éléments si nécessaire](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Sélectionnez le **hello-de-csharp** de projet et accédez à la **hello-de-csharp** la cible **onglet Général**. Dans le **binaires incorporées** section, ajoutez **hello-de-csharp.framework**.

![Fichiers binaires incorporées](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Ouvrez **ViewController.m**et remplacez le contenu avec :

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

Incorporation de .NET ne prend pas en charge bitcode sur iOS, qui est activée pour certains modèles de projet Xcode. 

Désactiver dans les paramètres de votre projet :

![Option de Bitcode](../../images/ios-bitcode-option.png)

Enfin, exécutez le projet Xcode et quelque chose comme ceci s’afficheront :

![Bonjour à partir de l’exemple de code c# en cours d’exécution dans le simulateur](ios-images/hello-from-csharp-ios.png)
