---
title: Reconnaissance vocale à l’aide de l’API de Microsoft Speech
description: L’API Microsoft Speech est une API basée sur le cloud qui fournit des algorithmes pour traiter la langue parlée. Cet article explique comment utiliser l’API de REST de reconnaissance vocale Microsoft pour convertir les données audio à du texte dans une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: c8eb991f67d54f9bacbb776b350cc5649a04ab2b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846852"
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Reconnaissance vocale à l’aide de l’API de Microsoft Speech

_L’API Microsoft Speech est une API basée sur le cloud qui fournit des algorithmes pour traiter la langue parlée. Cet article explique comment utiliser l’API de REST de reconnaissance vocale Microsoft pour convertir les données audio à du texte dans une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

L’API Microsoft Speech comprend deux composants :

- Une reconnaissance vocale API de conversion des mots en texte. La reconnaissance vocale peut être effectuée via une API REST, la bibliothèque cliente ou la bibliothèque de service.
- Un texte en parole API pour la conversion de texte en mots. Conversion de texte par synthèse vocale est effectuée via une API REST.

Cet article se concentre sur l’exécution de la reconnaissance vocale via l’API REST. Les bibliothèques de client et le service prend en charge le renvoi des résultats partiels, l’API REST ne peut retourner un résultat de reconnaissance unique, sans les résultats partiels.

Une clé d’API doit être obtenue pour utiliser l’API Microsoft Speech. Cela peut être obtenue au [essayer les Services cognitifs](https://azure.microsoft.com/try/cognitive-services/).

Pour plus d’informations sur l’API Microsoft Speech, consultez [Documentation de l’API Microsoft Speech](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Authentification

Chaque demande adressée à l’API REST de reconnaissance vocale Microsoft requiert un jeton d’accès JSON Web Token (JWT), qui peut être obtenu à partir du service de jeton de services cognitifs à `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Un jeton peut être obtenu en effectuant une requête POST pour le service de jeton, en spécifiant un `Ocp-Apim-Subscription-Key` en-tête qui contient la clé API en tant que sa valeur.

L’exemple de code suivant montre comment demander un accès jeton à partir du service d’émission de jeton :

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Le jeton d’accès retourné, ce qui est le texte Base64, a un délai d’expiration de 10 minutes. Par conséquent, l’exemple d’application renouvelle le jeton d’accès de toutes les minutes 9.

Le jeton d’accès doit être spécifié dans chaque API REST de Microsoft Speech appeler comme un `Authorization` en-tête préfixé avec la chaîne `Bearer`, comme illustré dans l’exemple de code suivant :

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Pour passer un jeton d’accès valide à l’API REST de reconnaissance vocale Microsoft entraînera une erreur de 403 réponse.

## <a name="performing-speech-recognition"></a>Exécution de la reconnaissance vocale

La reconnaissance vocale est obtenue en effectuant une requête POST pour la `recognition` API à `https://speech.platform.bing.com/speech/recognition/`. Une demande unique ne peut pas contenir plus de 10 secondes de lecture et la durée totale de demande ne peut pas dépasser 14 secondes.

Le contenu audio doit être placé dans le corps de la publication de la demande au format wav.

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
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

Audio est enregistré dans chaque projet spécifique à la plateforme en tant que données de format wav PCM et le `RecognizeSpeechAsync` utilise le `PCLStorage` package NuGet pour ouvrir le fichier audio sous forme de flux. La demande de reconnaissance vocale URI est généré et un jeton d’accès est récupérée à partir du service d’émission de jeton. La demande de reconnaissance vocale est publiée dans le `recognition` API, qui retourne une réponse JSON qui contient le résultat. La réponse JSON est désérialisée, le résultat à la méthode d’appel pour l’affichage.

### <a name="configuring-speech-recognition"></a>Configuration de la reconnaissance vocale

Le processus de reconnaissance vocale peuvent être configuré en spécifiant les paramètres de la requête HTTP :

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

La principale configuration effectuée par le `GenerateRequestUri` méthode consiste à définir les paramètres régionaux du contenu audio. Pour obtenir la liste des paramètres régionaux pris en charge, consultez [langues prises en charge ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>Envoi de la demande

Le `SendRequestAsync` méthode rend la requête POST à l’API REST de reconnaissance vocale Microsoft et retourne la réponse :

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

Cette méthode crée la demande POST :

- Contenant le flux audio dans un `StreamContent` instance, ce qui fournit du contenu HTTP basé sur un flux.
- Définition de la `Content-Type` en-tête de la demande à `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Ajout du jeton d’accès à la `Authorization` en-tête, précédé de la chaîne `Bearer`.

La requête POST est ensuite envoyée au `recognition` API. La réponse est ensuite lu et retournée à la méthode d’appel.

Le `recognition` API enverra un code d’état HTTP 200 (OK) dans la réponse, fournie est que la demande est valide, ce qui signifie que la demande a réussi et que les informations demandées dans la réponse. Pour obtenir la liste des réponses d’erreur possibles, consultez [dépannage](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Traitement de la réponse

La réponse de l’API est retournée au format JSON, avec le texte reconnu qui est contenu dans le `name` balise. Les données JSON suivantes affiche un message de réponse correcte classique :

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

Dans l’exemple d’application, la réponse JSON est désérialisée dans un `SpeechResult` instance, avec le résultat retourné à la méthode d’appel pour l’affichage, comme indiqué dans les captures d’écran suivants :

![](speech-recognition-images/speech-recognition.png "Reconnaissance vocale")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API REST de reconnaissance vocale Microsoft pour convertir les données audio à du texte dans une application de Xamarin.Forms. En plus d’effectuer la reconnaissance vocale, l’API Microsoft Speech peut également convertir texte en mots.

## <a name="related-links"></a>Liens associés

- [Documentation de Microsoft Speech API](/azure/cognitive-services/speech/home/).
- [Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
