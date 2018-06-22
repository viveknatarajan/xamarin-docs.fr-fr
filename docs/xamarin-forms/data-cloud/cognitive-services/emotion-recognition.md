---
title: Reconnaissance émotion à l’aide de l’API d’image
description: L’API Face prend une expression de visage dans une image en tant qu’entrée et retourne les données qui inclut des niveaux de confiance sur un ensemble d’émotions pour chaque police dans l’image. Cet article explique comment utiliser l’API de Face pour reconnaître émotion, d’évaluer une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049764"
---
# <a name="emotion-recognition-using-the-face-api"></a>Reconnaissance émotion à l’aide de l’API d’image

_L’API Face prend une expression de visage dans une image en tant qu’entrée et retourne les données qui inclut des niveaux de confiance sur un ensemble d’émotions pour chaque police dans l’image. Cet article explique comment utiliser l’API de Face pour reconnaître émotion, d’évaluer une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

L’API de la Face peut effectuer la détection pour détecter anger, contempt, répugnantes, peur, bonheur, neutre émotion, leur et surprise, dans une expression de visage. Ces émotions sont globalement, dans toutes les cultures communiquées via les mêmes expressions de visages de base. Ainsi que de retourner un résultat d’émotion pour une expression faciale, l’API de la Face peut également retourne un rectangle englobant pour des faces détectés. Notez qu’une clé d’API doit être obtenue pour utiliser l’API de Face. Cela peut être obtenue au [essayer les Services cognitifs](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

La reconnaissance émotion peut être effectuée via une bibliothèque cliente et une API REST. Cet article se concentre sur la réalisation de reconnaissance émotion via l’API REST. Pour plus d’informations sur l’API REST, consultez [API REST de Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

L’API de la Face peut également servir à reconnaître les expressions de visages de personnes de vidéo et peut retourner un résumé de leurs émotions. Pour plus d’informations, consultez [comment analyser des vidéos en temps réel](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Pour plus d’informations sur l’API de Face, consultez [Face API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Authentification

Chaque demande adressée à l’API Face nécessite une clé d’API qui doit être spécifiée comme valeur de la `Ocp-Apim-Subscription-Key` en-tête. L’exemple de code suivant montre comment ajouter la clé API à le `Ocp-Apim-Subscription-Key` en-tête d’une requête :

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Pour transmettre une clé API valide à l’API Face entraînera une erreur de 401 réponse.

## <a name="performing-emotion-recognition"></a>Exécution de la reconnaissance émotion

La reconnaissance d’émotion est effectuée en faisant une demande POST contenant une image à la `detect` API à `https://[location].api.cognitive.microsoft.com/face/v1.0`, où `[location]]` correspond à la région que vous avez utilisé pour obtenir votre clé API. Les paramètres facultatifs de la demande sont :

- `returnFaceId` – s’il faut retourner faceIds des faces détectés. La valeur par défaut est `true`.
- `returnFaceLandmarks` – s’il faut retourner des repères de face des faces détectés. La valeur par défaut est `false`.
- `returnFaceAttributes` – s’il faut analyser et retourner un ou plusieurs spécifiés sont confrontés les attributs. Les attributs de type pris en charge incluent `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, et `noise`. Notez que face attribut analyse coût de calcul et le temps supplémentaire.

Contenu de l’image doit être placé dans le corps de la demande POST en tant qu’une URL ou les données binaires.

> [!NOTE]
> Formats de fichier pris en charge sont BMP, GIF, JPEG et PNG, et la taille de fichier autorisée est de 1 Ko à 4 Mo.

Dans l’exemple d’application, le processus de reconnaissance émotion est appelé en invoquant la `DetectAsync` méthode :

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Cet appel de méthode spécifie un flux contenant les données d’image, qui doit être retourné faceIds que face repères ne doit pas être retournés et que l’émotion de l’image doit être analysée. Il spécifie également que les résultats sont retournés sous forme de tableau de `Face` objets. À son tour, le `DetectAsync` méthode appelle la `detect` API REST qui effectue la reconnaissance d’émotion :

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Cette méthode génère un URI de demande et envoie ensuite la demande à la `detect` API via le `SendRequestAsync` (méthode).

> [!NOTE]
> Vous devez utiliser la même région dans vos appels d’API de Face comme vous permet d’obtenir les clés de votre abonnement. Par exemple, si vous avez obtenu vos clés d’abonnement à partir de la `westus` région, le point de terminaison de détection de visage sera `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>Envoi de la demande

Le `SendRequestAsync` méthode rend la requête POST à l’API de Face et retourne le résultat comme un `Face` tableau :

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Si l’image est fournie via un flux de données, la méthode crée la demande POST en encapsulant le flux d’image dans un `StreamContent` instance, ce qui fournit du contenu HTTP basé sur un flux. Vous pouvez également, si l’image est fournie via une URL, la méthode génère la demande POST en encapsulant l’URL dans un `StringContent` instance, ce qui fournit du contenu HTTP basé sur une chaîne.

La requête POST est ensuite envoyée au `detect` API. La réponse est en lecture, désérialisée et retournée à la méthode appelante.

Le `detect` API enverra un code d’état HTTP 200 (OK) dans la réponse, fournie est que la demande est valide, ce qui signifie que la demande a réussi et que les informations demandées dans la réponse. Pour obtenir la liste des réponses d’erreur possibles, consultez [API REST de Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Traitement de la réponse

La réponse de l’API est renvoyée au format JSON. Les données JSON suivantes affiche un message de réponse correcte classique qui fournit les données demandées par l’exemple d’application :

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Un message de réponse correcte se compose d’un tableau d’entrées face classés par la taille du rectangle face par ordre décroissant, tandis qu’une réponse vide n’indique aucun faces détectés. Chaque reconnu face inclut une série d’attributs de police facultatif, qui sont spécifiées par le `returnFaceAttributes` l’argument de la `DetectAsync` (méthode).

Dans l’exemple d’application, la réponse JSON est désérialisée dans un tableau de `Face` objets. Lors de l’interprétation des résultats à partir de l’API de Face, l’émotion détectée doit être interprétée comme l’émotion avec le score le plus élevé, comme les scores sont normalisées à la somme est égale à un. Par conséquent, l’exemple d’application affiche l’émotion reconnue avec le score le plus élevé pour la plus grande face détecté dans l’image. Cela est possible avec le code suivant :

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

La capture d’écran suivante montre le résultat du processus de reconnaissance émotion dans l’exemple d’application :

![](emotion-recognition-images/emotion-recognition.png "Reconnaissance émotion")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API de Face pour reconnaître émotion, d’évaluer une application de Xamarin.Forms. L’API Face prend une expression de visage dans une image en tant qu’entrée et retourne les données qui inclut la confiance dans un ensemble d’émotions pour chaque police dans l’image.

## <a name="related-links"></a>Liens associés

- [Sont confrontés à un API](/azure/cognitive-services/face/overview/).
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Visage dans une API REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
