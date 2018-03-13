---
title: "Comment pour automatiser un projet de Test de NUnit Android ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 11b693193b36a80b55a61308d98b76f4f6984e8a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Comment pour automatiser un projet de Test de NUnit Android ?

> [!NOTE]
> Ce guide couvre les étapes de configuration d’un projet de test NUnit Android, pas dans un projet Xamarin.UITest. Repères de Xamarin.UITest se trouve [ici](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Lorsque vous créez un projet de Test unitaire Android (Visual Studio pour Mac) ou d’une application de Test unitaire (Android) (Visual Studio), par défaut il ne sera pas exécutée automatiquement vos tests.
Pour automatiser le Test unitaire android : pour exécuter des tests NUnit sur un appareil cible, nous utilisons un `Android.App.Instrumentation` sous-classe, ce qui peut être créé et exécuté à l’aide de la `adb shell am instrument` commande.

Tout d’abord, nous créons le **TestInstrumentation.cs** fichier, qui crée une sous-classe de `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (déclaré dans `Xamarin.Android.NUnitLite.dll`). Le `TestInstrumentation(IntPtr, JniHandleOwnership)` constructeur _doit_ soient fournies et le serveur virtuel `AddTests()` méthode doit être substituée.
`AddTests()` les contrôles les tests sont réellement exécutées. Ce fichier est en grande partie réutilisable.

Ensuite, le `.csproj` doit être modifié pour ajouter `TestInstrumentation.cs`.

Si vous le souhaitez, le `.csproj` peuvent être modifiés pour ajouter le `RunTests` cible MSBuild, ce qui permettrait à appeler les tests unitaires en tant que :

```shell
msbuild /t:RunTests Project.csproj
```

À l’aide d’une nouvelle cible n’est pas obligatoire ; la commande adb correspondante peut être utilisée à la place :

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Remplacez `@PACKAGE\_NAME@` selon le cas ; il s’agit de la valeur présente dans le **AndroidManifest.xml** `/manifest/@package` attribut.

*Remarque importante*: avec la [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) de version, les noms de package par défaut pour les Wrappers RCW Android se basera sur le MD5SUM du nom qualifié d’assembly du type en cours d’exportation. Ainsi, le même nom qualifié complet doit être obtenue dans deux assemblys différents, et pas obtenir une erreur d’empaquetage. Par conséquent, assurez-vous que vous utilisez le \`nom\` propriété sur le \`Instrumentation\` l’attribut pour générer un nom ACW/classe accessible en lecture.

_Le nom ACW doit être utilisé dans le `adb` commande_. Changement de nom/refactorisation de la classe c# sera nécessitent donc pas de modification de la `RunTests` commande utilise le nom ACW correct.

Ajouts au fichier .csproj :

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

```cs 
using System;
using System.Reflection;
 
using Android.App;
using Android.Content;
using Android.Runtime;
 
using Xamarin.Android.NUnitLite;
 
namespace App.Tests {
 
    [Instrumentation(Name="app.tests.TestInstrumentation")]
    public class TestInstrumentation : TestSuiteInstrumentation {
 
        public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
        {
        }
 
        protected override void AddTests ()
        {
            AddTest (Assembly.GetExecutingAssembly ());
        }
    }
}
```

