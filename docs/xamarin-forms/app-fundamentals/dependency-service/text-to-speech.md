---
title: Implémentation de synthèse vocale
description: Cet article explique comment utiliser la classe Xamarin.Forms DependencyService rappelle API synthèse vocale natif de chaque plateforme.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: d39902f2269d3eb241b48831b8eb1b181ff11e7a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997541"
---
# <a name="implementing-text-to-speech"></a>Implémentation de synthèse vocale

Cet article vous guide lors de la création d’une application multiplateforme qui utilise [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) pour accéder aux API synthèse vocale natifs :

- **[Création de l’Interface](#Creating_the_Interface)**  &ndash; comprendre comment l’interface est créée dans le code partagé.
- **[iOS implémentation](#iOS_Implementation)**  &ndash; Apprenez à implémenter l’interface en code natif pour iOS.
- **[Implémentation Android](#Android_Implementation)**  &ndash; Apprenez à implémenter l’interface en code natif pour Android.
- **[Implémentation de UWP](#WindowsImplementation)**  &ndash; Apprenez à implémenter l’interface en code natif pour la plateforme Windows universelle (UWP).
- **[Mise en œuvre dans le Code partagé](#Implementing_in_Shared_Code)**  &ndash; Apprenez à utiliser `DependencyService` pour appeler l’implémentation native à partir de code partagé.

L’application à l’aide `DependencyService` aura la structure suivante :

![](text-to-speech-images/tts-diagram.png "Structure de l’Application DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Création de l'interface utilisateur

Tout d’abord, créez une interface dans le code partagé qui exprime la fonctionnalité que vous envisagez d’implémenter. Pour cet exemple, l’interface contient une méthode unique, `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Codage par rapport à cette interface dans le code partagé permettent à l’application de Xamarin.Forms accéder à l’API reconnaissance vocale sur chaque plateforme.

> [!NOTE]
> Classes implémentant l’interface doivent avoir un constructeur sans paramètre pour travailler avec le `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implémentation d’iOS

L’interface doit être implémentée dans chaque projet d’application de plateforme spécifique. Notez que la classe a un constructeur sans paramètre afin que le `DependencyService` peut créer des instances.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

Le `[assembly]` attribut inscrit la classe en tant qu’implémentation de la `ITextToSpeech` interface, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implémentation Android

Le code Android est plus complexe que la version iOS : il requiert la classe d’implémentation d’hériter de spécifique à Android `Java.Lang.Object` et pour implémenter le `IOnInitListener` interface également. Il requiert également l’accès au contexte Android actuel, qui est exposé par le `MainActivity.Instance` propriété.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

Le `[assembly]` attribut inscrit la classe en tant qu’implémentation de la `ITextToSpeech` interface, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implémentation de plateforme Windows universelle

La plateforme Windows universelle a une API reconnaissance vocale dans le `Windows.Media.SpeechSynthesis` espace de noms. Le seul inconvénient consiste à penser à cochez le **Microphone** fonctionnalité dans le manifeste, sinon accéder à la reconnaissance vocale API sont bloqués.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

Le `[assembly]` attribut inscrit la classe en tant qu’implémentation de la `ITextToSpeech` interface, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Mise en œuvre dans le Code partagé

Nous pouvons maintenant écrire et tester le code partagé qui accède à l’interface de synthèse vocale. Cette page simple inclut un bouton qui déclenche la fonctionnalité de reconnaissance vocale. Il utilise le `DependencyService` pour obtenir une instance de la `ITextToSpeech` interface &ndash; lors de l’exécution, cette instance sera l’implémentation spécifique à la plateforme qui a un accès complet pour le Kit de développement natif.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

Exécutez cette application sur iOS, Android ou UWP et en appuyant sur le bouton entraîne l’application en parlant pour vous, en utilisant la Kit de développement logiciel de reconnaissance vocale native sur chaque plateforme.

 ![iOS et Android bouton synthèse vocale](text-to-speech-images/running.png "exemple de texte par synthèse vocale")


## <a name="related-links"></a>Liens associés

- [À l’aide de DependencyService (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Classeur de conversion de texte par synthèse vocale](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
