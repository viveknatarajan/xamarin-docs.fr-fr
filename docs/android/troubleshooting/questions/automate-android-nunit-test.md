---
title: Comment pour automatiser un projet de Test de NUnit Android ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: f63a25f36682038b7fcd85d711d980b9e3ec869d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763769"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Comment pour automatiser un projet de Test de NUnit Android ?

> [!NOTE]
> Ce guide explique comment automatiser un projet de test NUnit Android, pas dans un projet Xamarin.UITest. Repères de Xamarin.UITest se trouve [ici](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Lorsque vous créez un **application de Test unitaire (Android)** projet dans Visual Studio (ou **Android Test unitaire** projet dans Visual Studio pour Mac), ce projet le seront pas automatiquement d’exécuter vos tests par défaut.
Pour exécuter les tests NUnit sur un appareil cible, vous pouvez créer un [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) sous-classe est démarrée à l’aide de la commande suivante : 

```shell
adb shell am instrument 
```

Les étapes suivantes expliquent ce processus :

1.  Créer un nouveau fichier appelé **TestInstrumentation.cs**: 

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
    Dans ce fichier, [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (à partir de **Xamarin.Android.NUnitLite.dll**) est une sous-classe pour créer `TestInstrumentation`.

2.  Implémentez la [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) constructeur et la [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) (méthode). Le `AddTests` méthode contrôle les tests sont réellement exécutées.

3.  Modifier la `.csproj` fichier à ajouter **TestInstrumentation.cs**. Par exemple :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  Utilisez la commande suivante pour exécuter les tests unitaires. Remplacez `PACKAGE_NAME` avec le nom du package de l’application (le nom du package sont accessibles dans l’application `/manifest/@package` attribut situé dans **AndroidManifest.xml**) :

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  Si vous le souhaitez, vous pouvez modifier le `.csproj` fichier à ajouter le `RunTests` cibles de MSBuild. Il est ainsi possible d’appeler les tests unitaires avec une commande comme suit :

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (Notez qu’à l’aide de cette nouvelle cible n’est pas obligatoire ; la plus antérieure `adb` commande peut être utilisée à la place de `msbuild`.)

Pour plus d’informations sur l’utilisation de la `adb shell am instrument` commande pour exécuter des tests unitaires, consultez le développeur Android [exécution de tests avec ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) rubrique.


> [!NOTE]
> Avec la [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) de version, les noms de package par défaut pour les Wrappers RCW Android se basera sur le MD5SUM du nom qualifié d’assembly du type en cours d’exportation. Ainsi, le même nom qualifié complet doit être obtenue dans deux assemblys différents, et pas obtenir une erreur d’empaquetage. Par conséquent, assurez-vous que vous utilisez le `Name` propriété sur le `Instrumentation` attribut pour générer un nom ACW/classe accessible en lecture.

_Le nom ACW doit être utilisé dans le `adb` la commande ci-dessus_.
Changement de nom/refactorisation de la classe c# sera nécessitent donc pas de modification de la `RunTests` commande utilise le nom ACW correct.

