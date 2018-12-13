---
title: Implémentation de la synthèse vocale
description: Cet article explique comment utiliser la classe DependencyService de Xamarin.Forms pour appeler l’API de synthèse vocale native de chaque plateforme.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6d1948214b97a1b536b07b6420c32e4d27124518
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38997541"
---
# <a name="implementing-text-to-speech"></a>Implémentation de la synthèse vocale

Cet article vous montre comment créer une application multiplateforme qui utilise [`DependencyService`](xref:Xamarin.Forms.DependencyService) pour accéder aux API de synthèse vocale natives :

- **[Création de l’interface](#Creating_the_Interface)** &ndash; Découvrez comment créer l’interface dans le code partagé.
- **[Implémentation iOS ](#iOS_Implementation)** &ndash; Apprenez à implémenter l’interface dans le code natif pour iOS.
- **[Implémentation Android ](#Android_Implementation)** &ndash; Apprenez à implémenter l’interface dans le code natif pour Android.
- **[Implémentation UWP](#WindowsImplementation)** &ndash; Apprenez à implémenter l’interface dans le code natif pour la plateforme Windows universelle (UWP).
- **[Implémentation dans le code partagé](#Implementing_in_Shared_Code)** &ndash; Apprenez à utiliser `DependencyService` pour appeler l’implémentation native à partir du code partagé.

L’application qui utilise `DependencyService` aura la structure suivante :

![](text-to-speech-images/tts-diagram.png "Structure de l’application avec DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Création de l'interface utilisateur

Tout d’abord, créez une interface dans le code partagé qui expose la fonctionnalité que vous souhaitez implémenter. Dans cet exemple, l’interface contient une seule méthode, `Speak` :

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Le codage par rapport à cette interface dans le code partagé permettra à l’application Xamarin.Forms d’accéder aux API de synthèse vocale sur chaque plateforme.

> [!NOTE]
> Les classes implémentant l’interface doivent avoir un constructeur sans paramètre pour fonctionner avec `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implémentation iOS

L’interface doit être implémentée dans le projet d’application de chaque plateforme. Notez que la classe a un constructeur sans paramètre pour permettre à `DependencyService` de créer des instances.

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

L’attribut `[assembly]` inscrit la classe en tant qu’implémentation de l’interface `ITextToSpeech`, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celle-ci.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implémentation Android

Le code Android est plus complexe que la version iOS, car il requiert que la classe d’implémentation hérite de l’objet `Java.Lang.Object` pour Android et qu’elle implémente également l’interface `IOnInitListener`. De plus, il doit accéder au contexte Android actuel, qui est exposé par la propriété `MainActivity.Instance`.

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

L’attribut `[assembly]` inscrit la classe en tant qu’implémentation de l’interface `ITextToSpeech`, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celle-ci.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implémentation sur la plateforme Windows universelle

La plateforme Windows universelle fournit une API de synthèse vocale dans l’espace de noms `Windows.Media.SpeechSynthesis`. La seule mise en garde est de penser à activer la fonctionnalité **Microphone** dans le manifeste, car sinon l’accès aux API de synthèse vocale sera bloqué.

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

L’attribut `[assembly]` inscrit la classe en tant qu’implémentation de l’interface `ITextToSpeech`, ce qui signifie que `DependencyService.Get<ITextToSpeech>()` peut être utilisé dans le code partagé pour créer une instance de celle-ci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implémentation dans le code partagé

Nous pouvons maintenant écrire et tester le code partagé qui accède à l’interface de synthèse vocale. Cette page simple contient un bouton qui déclenche la fonctionnalité de synthèse vocale. `DependencyService` est utilisé pour obtenir une instance de l’interface `ITextToSpeech` &ndash; au moment de l’exécution, cette instance constitue l’implémentation spécifique de la plateforme qui a un accès total au SDK natif.

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

Quand un clic sur ce bouton est enregistré dans cette application exécutée sur iOS, Android ou UWP, l’application vous parle, en utilisant le SDK de synthèse vocale natif sur chaque plateforme.

 ![Bouton de synthèse vocale sur iOS et Android](text-to-speech-images/running.png "Exemple de synthèse vocale")


## <a name="related-links"></a>Liens associés

- [Utilisation de DependencyService (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyService (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)

