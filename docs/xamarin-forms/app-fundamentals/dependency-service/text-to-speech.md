---
title: "Mise en œuvre de la synthèse vocale"
description: "Utiliser DependencyService pour appeler l’API de chaque plateforme de synthèse vocale native"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6ac9ca7bae517602c33729134eb0bd48359afbc7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-text-to-speech"></a>Mise en œuvre de la synthèse vocale

Cet article vous aidera à mesure que vous créez une application multiplateforme qui utilise [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) pour accéder aux API synthèse vocale natifs :

- **[Création de l’Interface](#Creating_the_Interface)**  &ndash; comprendre la façon dont l’interface est créée dans le code partagé.
- **[iOS implémentation](#iOS_Implementation)**  &ndash; apprendre à implémenter l’interface en code natif pour iOS.
- **[Implémentation Android](#Android_Implementation)**  &ndash; apprendre à implémenter l’interface en code natif pour Android.
- **[Implémentation Windows](#WindowsImplementation)**  &ndash; apprendre à implémenter l’interface en code natif pour Windows Phone et la plateforme Windows universelle (UWP).
- **[Mise en œuvre dans le Code partagé](#Implementing_in_Shared_Code)**  &ndash; apprendre à utiliser `DependencyService` pour appeler l’implémentation native à partir de code partagé.

L’application à l’aide `DependencyService` aura la structure suivante :

![](text-to-speech-images/tts-diagram.png "Structure de l’Application DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Création de l'interface utilisateur

Tout d’abord, créez une interface dans le code partagé qui exprime la fonctionnalité que vous souhaitez mettre en œuvre. Pour cet exemple, l’interface contient une méthode unique, `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Codage de cette interface dans le code partagé permettent à l’application de Xamarin.Forms accéder aux API vocal sur chaque plateforme.

> [!NOTE]
> Classes implémentant l’interface doivent avoir un constructeur sans paramètre pour travailler avec les `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implémentation d’iOS

L’interface doit être implémentée dans chaque projet d’application de plateforme spécifique. Notez que la classe a un constructeur sans paramètre afin que la `DependencyService` peut créer des instances.

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

Le `[assembly]` attribut enregistre la classe en tant qu’implémentation de la `ITextToSpeech` interface, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implémentation Android

Le code Android est plus complexe que la version iOS : il requiert la classe d’implémentation d’hériter Android spécifiques `Java.Lang.Object` et pour implémenter le `IOnInitListener` interface également. Elle requiert également l’accès au contexte Android actuel, qui est exposé par le `MainActivity.Instance` propriété.

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

Le `[assembly]` attribut enregistre la classe en tant qu’implémentation de la `ITextToSpeech` interface, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone et l’implémentation de plateforme Windows universelle

Windows Phone et la plateforme Windows universelle ont une API vocale dans le `Windows.Media.SpeechSynthesis` espace de noms. Le seul inconvénient consiste à mémoriser cycle le **Microphone** capacité dans le manifeste, sinon accéder à la reconnaissance vocale API est bloqués.

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

Le `[assembly]` attribut enregistre la classe en tant qu’implémentation de la `ITextToSpeech` interface, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Mise en œuvre dans le Code partagé

Maintenant, nous pouvons écrire et tester le code partagé qui accède à l’interface de synthèse vocale. Cette page simple inclut un bouton qui déclenche la fonctionnalité de reconnaissance vocale. Elle utilise le `DependencyService` pour obtenir une instance de la `ITextToSpeech` interface &ndash; lors de l’exécution, cette instance correspond à l’implémentation spécifique à la plateforme qui a un accès complet pour le Kit de développement natif.

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

Exécution de cette application sur iOS, Android, ou les plateformes Windows et en appuyant sur que le bouton entraînera l’application est élevé, pour vous, à l’aide de la voix native Kit de développement logiciel sur chaque plateforme.

 ![iOS et Android bouton synthèse vocale](text-to-speech-images/running.png "exemple de texte par synthèse vocale")


## <a name="related-links"></a>Liens associés

- [À l’aide de DependencyService (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Classeur de texte par synthèse vocale](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
