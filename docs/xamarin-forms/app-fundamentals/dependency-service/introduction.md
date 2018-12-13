---
title: Introduction à la classe DependencyService
description: Cet article explique comment utiliser la classe DependencyService de Xamarin.Forms pour accéder aux fonctionnalités natives de chaque plateforme.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 3c8cc31c21f354b60001cefb919b51bf4d42da9f
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675014"
---
# <a name="introduction-to-dependencyservice"></a>Introduction à la classe DependencyService

## <a name="overview"></a>Vue d'ensemble

[`DependencyService`](xref:Xamarin.Forms.DependencyService) permet aux applications d’appeler les fonctionnalités spécifiques de la plateforme à partir du code partagé. Les applications Xamarin.Forms peuvent ainsi faire les mêmes choses qu’une application native.

`DependencyService` est un localisateur de services. Dans la pratique, une interface est définie, et `DependencyService` recherche l’implémentation appropriée de cette interface dans les différents projets de plateforme.

> [!NOTE]
> Par défaut, [`DependencyService`](xref:Xamarin.Forms.DependencyService) résout uniquement les implémentations de plateforme qui ont des constructeurs sans paramètre. Toutefois, il est possible d’injecter une méthode de résolution de dépendance dans Xamarin.Forms qui utilise un conteneur d’injection de dépendance ou des méthodes de fabrique pour résoudre les implémentations de plateforme. Cette approche permet de résoudre les implémentations de plateforme qui ont des constructeurs avec des paramètres. Pour plus d’informations, consultez [Résolution des dépendances dans Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

## <a name="how-dependencyservice-works"></a>Fonctionnement de DependencyService

Les applications Xamarin.Forms ont besoin de quatre composants pour utiliser `DependencyService` :

- **Interface** &ndash; La fonctionnalité requise est définie par une interface dans le code partagé.
- **Implémentation par plateforme** &ndash; Les classes qui implémentent l’interface doivent être ajoutées à chaque projet de plateforme.
- **Inscription** &ndash; Chaque classe d’implémentation doit être inscrite avec `DependencyService` à l’aide d’un attribut de métadonnées. L’inscription permet à `DependencyService` de trouver la classe d’implémentation et de la fournir à la place de l’interface au moment de l’exécution.
- **Appel de DependencyService** &ndash; Le code partagé doit appeler explicitement `DependencyService` pour demander les implémentations de l’interface.

Notez que les implémentations doivent être fournies pour chaque projet de plateforme dans votre solution. Les projets de plateforme qui n’ont pas les implémentations nécessaires ne s’exécuteront pas.

Le diagramme suivant montre la structure de l’application :

![](introduction-images/overview-diagram.png "Structure de l’application avec DependencyService")

### <a name="interface"></a>Interface

L’interface que vous concevez définit les interactions avec les fonctionnalités propres à la plateforme. Apportez une attention particulière à la conception si vous développez un composant qui sera partagé comme composant ou package NuGet. La conception de l’API est décisive pour le fonctionnement d’un package. L’exemple ci-dessous définit une interface simple de synthèse vocale qui offre suffisamment de flexibilité pour les mots à prononcer tout en autorisant la personnalisation de l’implémentation pour chaque plateforme :

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Implémentation par plateforme

Une fois que vous avez terminé la conception de l’interface appropriée, vous devez l’implémenter dans le projet de chaque plateforme que vous ciblez. Par exemple, la classe suivante implémente l’interface `ITextToSpeech` sur iOS :

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>Inscription

Chaque implémentation de l’interface doit être inscrite avec `DependencyService` à l’aide d’un attribut de métadonnées. Le code suivant inscrit l’implémentation pour iOS :

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Au final, l’implémentation par plateforme ressemble à ceci :

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

Notez que l’inscription est effectuée au niveau de l’espace de noms, pas au niveau de la classe.

#### <a name="universal-windows-platform-net-native-compilation"></a>Compilation .NET Native pour la plateforme Windows universelle (UWP)

Les projets UWP qui utilisent l’option de compilation .NET Native nécessitent une [configuration légèrement différente](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) lors de l’initialisation de Xamarin.Forms. La compilation .NET Native nécessite également une inscription légèrement différente pour les services de dépendance.

Dans le fichier **App.xaml.cs**, inscrivez manuellement chaque service de dépendance défini dans le projet UWP à l’aide de la méthode `Register<T>`, comme indiqué ci-dessous :

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Remarque : l’inscription manuelle avec la méthode `Register<T>` est possible uniquement dans les versions de mise en production générées par la compilation .NET Native. Si vous omettez cette ligne, les versions de débogage continueront de fonctionner, mais les versions de mise en production ne pourront pas charger le service de dépendance.

### <a name="call-to-dependencyservice"></a>Appel de DependencyService

Une fois que vous avez configuré le projet avec une interface commune et des implémentations pour chaque plateforme, utilisez `DependencyService` pour obtenir l’implémentation correcte au moment de l’exécution :

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` trouve l’implémentation correcte de l’interface `T`.

### <a name="solution-structure"></a>Structure de la solution

L’[exemple de solution UsingDependencyService](https://developer.xamarin.com/samples/UsingDependencyService/) est illustré ci-dessous pour iOS et Android. Il montre les modifications de code dont nous avons parlé plus haut.

 [![Solution iOS et Android](introduction-images/solution-sml.png "Structure de l’exemple de solution DependencyService")](introduction-images/solution.png#lightbox "Structure de l’exemple de solution DependencyService")

> [!NOTE]
> Vous **devez** fournir une implémentation dans chaque projet de plateforme. Si aucune implémentation d’interface n’est inscrite, `DependencyService` ne pourra pas résoudre la méthode `Get<T>()` au moment de l’exécution.

## <a name="related-links"></a>Liens associés

- [DependencyService (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
