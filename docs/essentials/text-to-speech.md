---
title: Xamarin.Essentials vocale
description: La classe texttospeech avec le permet à une application utiliser intégrée dans les moteurs de synthèse vocale de parler de texte précédent de l’appareil et également pour les langues disponibles de requête prises en charge par le moteur.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2fcb97eafda15a73e2b7e5441f1d6c94f75af0ce
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials vocale

![Version préliminaire de NuGet](~/media/shared/pre-release.png)

Le **texttospeech avec le** classe permet une application utilisent intégrée dans les moteurs de parler de texte précédent de l’appareil et également pour les langues disponibles de requête prises en charge par le moteur de synthèse vocale.

## <a name="using-text-to-speech"></a>À l’aide de la synthèse vocale

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de synthèse vocale fonctionne en appelant le `SpeakAsync` méthode avec le texte et les paramètres facultatifs et retourne la fin de l’énoncé. 

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) => 
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Cette méthode prend dans un CancellationToken facultatif pour arrêter l’énoncé une qu'au démarrage. 
```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

Synthèse vocale file d’attente automatiquement les demandes de reconnaissance vocale à partir du même thread. 

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Paramètres de reconnaissance vocale

Pour mieux contrôler la façon dont le son est lu sauvegarder avec `SpeakSettings` qui permet de définir le Volume, hauteur et paramètres régionaux.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Les valeurs prises en charge pour ces paramètres sont les suivantes :

| Paramètre | Minimum | Maximum |
| --- | :---: | :---: |
| Espacement | 0 | 2.0 |
| Volume | 0 | 1.0 |

### <a name="speech-locales"></a>Paramètres régionaux de reconnaissance vocale

Chaque plate-forme offre des paramètres régionaux pour parler de texte précédent dans plusieurs langues et les accents. Chaque plateforme possède une autre codes et les méthodes de spécification, c’est pourquoi Essentials fournit un inter-plateformes `Locale` classe et un moyen de les interroger avec `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Limitations

- File d’attente de l’énoncé n’est pas garantie si elle est appelée sur plusieurs threads.
- Arrière-plan de lecture audio n’est pas officiellement pris en charge.

## <a name="api"></a>API

- [Code source de texttospeech avec le](https://github.com/xamarin/Essentials/tree/master/Essentials/TextToSpeech)
- [Documentation de l’API de texttospeech avec le](xref:Xamarin.Essentials.TextToSpeech)
