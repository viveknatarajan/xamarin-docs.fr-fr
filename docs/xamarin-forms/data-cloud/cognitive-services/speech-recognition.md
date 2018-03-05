---
title: "Reconnaissance vocale à l’aide de l’API de Speech Bing"
description: "L’API de reconnaissance vocale Bing est une API basée sur le cloud qui fournit des algorithmes pour traiter la langue parlée. Cet article explique comment utiliser l’API REST de Bing vocale reconnaissance pour convertir les données audio à du texte dans une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>Reconnaissance vocale à l’aide de l’API de Speech Bing

_L’API de reconnaissance vocale Bing est une API basée sur le cloud qui fournit des algorithmes pour traiter la langue parlée. Cet article explique comment utiliser l’API REST de Bing vocale reconnaissance pour convertir les données audio à du texte dans une application de Xamarin.Forms._

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!NOTE]
> L’API de reconnaissance vocale Bing est encore en version préliminaire. Il peut être modifications avec rupture à l’API avant la version finale.

## <a name="overview"></a>Vue d'ensemble

L’API de reconnaissance vocale Bing comprend deux composants :

- Une reconnaissance vocale API de conversion des mots en texte. La reconnaissance vocale peut être effectuée via une API REST, la bibliothèque cliente ou la bibliothèque de service.
- Un texte en parole API pour la conversion de texte en mots. Conversion de texte par synthèse vocale est effectuée via une API REST.

Cet article se concentre sur l’exécution de la reconnaissance vocale via l’API REST. Les bibliothèques de client et le service prend en charge le renvoi des résultats partiels, l’API REST ne peut retourner un résultat de reconnaissance unique, sans les résultats partiels.

Une clé d’API doit être obtenue pour utiliser l’API de reconnaissance vocale Bing. Cela peut être obtenue au [mise en route gratuitement](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) sur microsoft.com.

Pour plus d’informations sur l’API de reconnaissance vocale Bing, consultez [Bing vocale Documentation](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview) sur microsoft.com.

## <a name="authentication"></a>Authentification

Chaque demande adressée à l’API REST de Bing vocale reconnaissance requiert un jeton d’accès JSON Web Token (JWT), qui peut être obtenu à partir du service de jeton de services cognitifs à `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Un jeton peut être obtenu en effectuant une requête POST pour le service de jeton, en spécifiant un `Ocp-Apim-Subscription-Key` en-tête qui contient la clé API en tant que sa valeur.

L’exemple de code suivant montre comment demander un accès jeton à partir du service d’émission de jeton :

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Le jeton d’accès retourné, ce qui est le texte Base64, a un délai d’expiration de 10 minutes. Par conséquent, l’exemple d’application renouvelle le jeton d’accès de toutes les minutes 9.

Le jeton d’accès doit être spécifié dans chaque API de REST de reconnaissance vocale Bing appeler comme un `Authorization` en-tête préfixé avec la chaîne `Bearer`, comme illustré dans l’exemple de code suivant :

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Pour passer un jeton d’accès valide à l’API REST de Bing vocale reconnaissance entraînera une erreur de 403 réponse.

## <a name="performing-speech-recognition"></a>Exécution de la reconnaissance vocale

La reconnaissance vocale est obtenue en faisant une demande POST `recognize` API à `https://speech.platform.bing.com/recognize`. Une demande unique ne peut pas contenir plus de 10 secondes de lecture et la durée totale de demande ne peut pas dépasser 14 secondes.

Le contenu audio doit être placé dans le corps de la publication de la demande au format wav. Pour plus d’informations sur les codecs pris en charge, consultez [prise en charge des Codecs](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) sur microsoft.com.

Dans l’exemple d’application, le `RecognizeSpeechAsync` méthode appelle le processus de reconnaissance vocale :

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

Audio est enregistré dans chaque projet spécifique à la plateforme en tant que données de format wav PCM et le `RecognizeSpeechAsync` utilise le `PCLStorage` package NuGet pour ouvrir le fichier audio sous forme de flux. La demande de reconnaissance vocale URI est généré et un jeton d’accès est récupérée à partir du service d’émission de jeton. La demande de reconnaissance vocale est publiée dans le `recognize` API, qui retourne une réponse JSON qui contient le résultat. La réponse JSON est désérialisée, le résultat à la méthode d’appel pour l’affichage.

### <a name="configuring-speech-recognition"></a>Configuration de la reconnaissance vocale

Le processus de reconnaissance vocale peut être configuré en spécifiant des paramètres de requête HTTP. Il existe des paramètres obligatoires et facultatifs, avec la méthode suivante indique les paramètres obligatoires qui doivent être définies :

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

La principale configuration effectuée par le `GenerateRequestUri` méthode consiste à définir les paramètres régionaux du contenu audio. Pour obtenir la liste des paramètres régionaux pris en charge, consultez [prise en charge des paramètres régionaux ](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) sur microsoft.com.

Pour plus d’informations sur les valeurs possibles pour les paramètres obligatoires, consultez [Required Parameters](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) sur microsoft.com. Pour plus d’informations sur les paramètres facultatifs, consultez [paramètres facultatifs](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) sur microsoft.com.

### <a name="sending-the-request"></a>Envoi de la demande

Le `SendRequestAsync` méthode rend la requête POST à l’API REST de Bing vocale reconnaissance et retourne la réponse :

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

Cette méthode crée la demande POST :

- Contenant le flux audio dans un `StreamContent` instance, ce qui fournit du contenu HTTP basé sur un flux.
- Définition de la `Content-Type` en-tête de la demande à `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Ajout du jeton d’accès à la `Authorization` en-tête, précédé de la chaîne `Bearer`.

La requête POST est ensuite envoyée au `recognize` API. La réponse est ensuite lu et retournée à la méthode d’appel.

Le `recognize` API enverra un code d’état HTTP 200 (OK) dans la réponse, fournie est que la demande est valide, ce qui signifie que la demande a réussi et que les informations demandées dans la réponse. Pour obtenir la liste des réponses d’erreur possibles, consultez [des réponses d’erreur](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) sur microsoft.com.

### <a name="processing-the-response"></a>Traitement de la réponse

La réponse de l’API est retournée au format JSON, avec le texte reconnu qui est contenu dans le `name` balise. Les données JSON suivantes affiche un message de réponse correcte classique :

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

Dans l’exemple d’application, la réponse JSON est désérialisée dans un `SpeechResult` instance, avec le résultat retourné à la méthode d’appel pour l’affichage, comme indiqué dans les captures d’écran suivants :

![](speech-recognition-images/speech-recognition.png "Reconnaissance vocale")

Pour plus d’informations sur les valeurs de chaque balise JSON, consultez [réponses de reconnaissance vocale](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) sur microsoft.com.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API REST de Bing vocale reconnaissance pour convertir les données audio à du texte dans une application de Xamarin.Forms. En plus d’effectuer la reconnaissance vocale, l’API de reconnaissance vocale Bing peut également convertir texte en mots.



## <a name="related-links"></a>Liens associés

- [Documentation de reconnaissance vocale Bing](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Reconnaissance vocale Bing API REST](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
