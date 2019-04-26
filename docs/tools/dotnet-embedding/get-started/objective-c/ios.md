---
title: Bien démarrer avec iOS
description: Ce document décrit la prise en main à l’aide de l’incorporation de .NET avec iOS. Il aborde la configuration requise et présente un exemple d’application pour illustrer comment lier un assembly managé et utiliser la sortie dans un projet Xcode.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 009772ac88ad57bab53fb71c9705b71f0f8acc8b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61318704"
---
# <a name="getting-started-with-ios"></a>Bien démarrer avec iOS

## <a name="requirements"></a>Configuration requise

Outre les exigences de notre [mise en route avec Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) guide, vous devez également :

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure

## <a name="hello-world"></a>Hello World

Tout d’abord, créez un exemple de world hello simple en c#.

### <a name="create-c-sample"></a>Créer l’exemple c#

Ouvrez Visual Studio pour Mac, créez un projet de bibliothèque de classes iOS, nommez-le **hello de csharp**et enregistrez-le à **~/Projects/hello-from-csharp**.

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

Générer le projet et l’assembly résultant sera enregistré sous **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Lier l’assembly managé

Une fois que vous avez un assembly managé, la liaison en appelant l’incorporation de .NET.

Comme décrit dans la [installation](~/tools/dotnet-embedding/get-started/install/install.md) guide, cela est possible en tant qu’étape post-build dans votre projet, avec une cible MSBuild personnalisée, ou manuellement :

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Le framework sera placé dans **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Utiliser la sortie générée dans un projet Xcode

Ouvrez Xcode, créez une nouvelle Application avec affichage unique d’iOS, nommez-le **hello de csharp**, puis sélectionnez le **Objective-C** language.

Ouvrez le **~/Projects/hello-from-csharp/output** répertoire dans le Finder, sélectionnez **hello de csharp.framework**, faites-le glisser vers le projet Xcode et déposez-le juste au-dessus le **hello-de-csharp**  dossier dans le projet.

![Faites glisser et déposez le framework](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Assurez-vous que **copier les éléments si besoin** est activée dans la boîte de dialogue qui s’affiche, puis cliquez sur **Terminer**.

![Copier des éléments si nécessaire](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Sélectionnez le **hello de csharp** de projet et accédez à la **hello de csharp** la cible **onglet Général**. Dans le **Embedded binaire** , ajoutez **hello de csharp.framework**.

![Binaires incorporés](ios-images/hello-from-csharp-ios-embedded-binaries.png)

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

Incorporation .NET ne prend pas en charge bitcode sur iOS, qui est activée pour certains modèles de projet Xcode. 

Désactiver dans les paramètres de votre projet :

![Option de Bitcode](../../images/ios-bitcode-option.png)

Enfin, exécutez le projet Xcode et quelque chose comme ceci s’affiche :

![Bonjour de l’exemple c# en cours d’exécution dans le simulateur](ios-images/hello-from-csharp-ios.png)
