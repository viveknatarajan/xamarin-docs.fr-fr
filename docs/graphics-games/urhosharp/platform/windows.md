---
title: Prise en charge UrhoSharp Windows
description: "Fonctionnalités et le programme d’installation spécifique de Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: ec4fec01aa27e522aeec6df09041f14b1c3db080
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-windows-support"></a>Prise en charge UrhoSharp Windows

_Fonctionnalités et le programme d’installation spécifique de Windows_

Bien que Urho est une bibliothèque de classes portable et permet la même API à utiliser sur la plateforme différents pour la logique du jeu, vous devez initialiser Urho dans votre pilote plateforme et dans certains cas, vous souhaitez tirer parti des fonctionnalités spécifiques de plateforme .

Dans les pages suivantes, supposons que `MyGame` est une sous-classe de la `Application` classe.

**Architectures prises en charge :** uniquement Windows 64 bits.

Vous pouvez voir des exemples complets illustrant comment utiliser cela dans notre [exemples](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

# <a name="standalone-project"></a>Projet autonome

## <a name="creating-a-project"></a>Création d'un projet

Créer un projet de Console, référence Urho NuGet et assurez-vous que vous pouvez localiser les ressources (les répertoires contenant le répertoire de données).

## <a name="configuring-and-launching-urho"></a>Configuration et lancement Urho

Pour lancer votre application, procédez comme suit :

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```
## <a name="example"></a>Exemple

[Exemple complet](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

# <a name="integrated-with-wpf"></a>Intégré avec WPF

## <a name="creating-a-project"></a>Création d'un projet

Créer un projet WPF, référence Urho NuGet et assurez-vous que vous pouvez localiser les ressources (les répertoires contenant le répertoire de données).

## <a name="configuring-and-launching-urho-from-wpf"></a>Configuration et lancer Urho à partir de WPF

Créer une sous-classe de `Window` et configurer vos éléments multimédias comme suit :

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

## <a name="example"></a>Exemple

[Exemple complet](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

# <a name="integrated-with-uwp"></a>Intégré à la plateforme Windows universelle

## <a name="creating-a-project"></a>Création d'un projet

Créer un projet UWP, référence Urho NuGet et assurez-vous que vous pouvez localiser les ressources (les répertoires contenant le répertoire de données).

## <a name="configuring-and-launching-urho-from-uwp"></a>Configuration et lancer Urho à partir de la plateforme Windows universelle

Créer une sous-classe de `Window` et configurer vos éléments multimédias comme suit :

```csharp
{
            InitializeComponent();
            GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
                .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
                .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
                .ToArray();
            DataContext = this;
            Loaded += (s, e) => RunGame (new MyGame ());
        }

        public void RunGame(TypeInfo value)
        {
            //at this moment, UWP supports assets only in pak files (see PackageTool)
            currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
        }
    }
```

## <a name="example"></a>Exemple

[Exemple complet](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

# <a name="integrated-with-windowsforms"></a>Intégré avec Windows.Forms

## <a name="creating-a-project"></a>Création d'un projet

Créer un projet Windows.Forms, référence Urho NuGet et assurez-vous que vous pouvez localiser les ressources (les répertoires contenant le répertoire de données).

## <a name="configuring-and-launching-urho-from-windowsforms"></a>Configuration et lancement Urho de Windows.Forms

Lancer Urho à partir de votre formulaire, consultez [exemple complet](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)

