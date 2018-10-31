---
title: 'Xamarin.Essentials : Synthèse vocale'
description: La classe TextToSpeech permet d’utiliser l'API de sythèse vocale intégrée à la plateforme. Elle vous permet d'énoncer à l'oral un texte.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ba822870edafce44140caa66b01f4da242fb7779
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353611"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials : Synthèse vocale

![Version préliminaire NuGet](~/media/shared/pre-release.png)

La classe TextToSpeech permet à une application d'utiliser les moteurs de synthèse vocale intégrés pour énoncer du texte à partir de l'appareil, mais aussi pour interroger les langues disponibles que le moteur peut prendre en charge.

## <a name="using-text-to-speech"></a>Utilisation de **TextToSpeech**

Ajoutez une référence à Xamarin.Essentials dans votre classe :

```csharp
using Xamarin.Essentials;
```

La fonctionnalité de synthèse vocale fonctionne en appelant la méthode `SpeakAsync` avec en paramètre le texte à énoncer et les paramètres facultatifs.

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

Cette méthode prend un `CancellationToken` facultatif pour interrompre l’énoncé après son démarrage.

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

Pour mettre en file d’attente les demandes provenant du même thread :

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

### <a name="speech-settings"></a>Paramètres de synthèse vocale

Pour mieux contrôler la façon dont le texte est prononcé, `SpeakSettings` vous permet de définir le Volume, le Pitch et les paramètres régionaux.

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

### <a name="speech-locales"></a>Paramètres régionaux de synthèse vocale

Chaque plateforme propose des paramètres régionaux pour vocaliser le texte en fonction des langues et des accents. Chaque plateforme a différents codes et méthodes de spécification, c’est pourquoi Xamarin.Essentials propose un multiplateforme classe `Locale`  et un moyen de les interroger avec `GetLocalesAsync`.

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

- La mise en file d’attente de l’énoncé n’est pas garantie si elle est appelée depuis plusieurs threads.
- La lecture audio en arrière-plan n’est pas officiellement prise en charge.

## <a name="api"></a>API

- [Code source de texttospeech avec le](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [Documentation de l’API de texttospeech avec le](xref:Xamarin.Essentials.TextToSpeech)
