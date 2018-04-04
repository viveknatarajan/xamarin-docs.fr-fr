---
title: Reconnaissance émotion à l’aide de l’API d’image
description: L’API Face prend une expression de visage dans une image en tant qu’entrée et retourne les données qui inclut des niveaux de confiance sur un ensemble d’émotions pour chaque police dans l’image. Cet article explique comment utiliser l’API de Face pour reconnaître émotion, d’évaluer une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 49e53425dbaf3aadd74d02ab030929e3311c7c8c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Reconnaissance émotion à l’aide de l’API d’image

_L’API Face prend une expression de visage dans une image en tant qu’entrée et retourne les données qui inclut des niveaux de confiance sur un ensemble d’émotions pour chaque police dans l’image. Cet article explique comment utiliser l’API de Face pour reconnaître émotion, d’évaluer une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

L’API de la Face peut effectuer la détection pour détecter anger, contempt, répugnantes, peur, bonheur, neutre émotion, leur et surprise, dans une expression de visage. Ces émotions sont globalement, dans toutes les cultures communiquées via les mêmes expressions de visages de base. Ainsi que de retourner un résultat d’émotion pour une expression faciale, l’API de la Face peut également retourne un rectangle englobant pour des faces détectés. Notez qu’une clé d’API doit être obtenue pour utiliser l’API de Face. Cela peut être obtenue au [essayer les Services cognitifs](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

La reconnaissance émotion peut être effectuée via une bibliothèque cliente et une API REST. Cet article se concentre sur la réalisation de reconnaissance émotion via la [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) bibliothèque cliente, qui peut être téléchargé à partir de NuGet.

L’API de la Face peut également servir à reconnaître les expressions de visages de personnes de vidéo et peut retourner un résumé de leurs émotions. Pour plus d’informations, consultez [comment analyser des vidéos en temps réel](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Pour plus d’informations sur l’API de Face, consultez [Face API](/azure/cognitive-services/face/overview/).

## <a name="performing-emotion-recognition"></a>Exécution de la reconnaissance émotion

La reconnaissance émotion est obtenue en téléchargeant des flux d’images à l’API de Face. La taille du fichier image ne doit pas être supérieure à 4 Mo, et les formats de fichier pris en charge sont BMP, GIF, JPEG et PNG.

L’exemple de code suivant illustre le processus de reconnaissance émotion :

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

Un `FaceServiceClient` instance doit être créée pour effectuer une reconnaissance émotion, avec la clé d’API de Face et passé en tant qu’arguments à un point de terminaison le `FaceServiceClient` constructeur.

> [!NOTE]
> Vous devez utiliser la même région dans vos appels d’API de Face comme vous permet d’obtenir les clés de votre abonnement. Par exemple, si vous avez obtenu vos clés d’abonnement à partir de la `westus` région, le point de terminaison de détection de visage sera `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

Le `DetectAsync` (méthode), qui est appelé sur le `FaceServiceClient` d’une instance, il télécharge une image à l’API Face, sous un `Stream`. La clé d’API sera soumise à l’API Face lorsque cette opération est appelée. Pour envoyer une clé d’API valide provoque l’affichage dans un `Microsoft.ProjectOxford.Face.FaceAPIException` levée avec le message d’exception indiquant qu’une clé API non valide a été envoyée.

Le `DetectAsync` méthode retournera un `Face` de tableau, si un type a été reconnu. Chaque retournée face contient un rectangle pour indiquer son emplacement, combinée avec une série d’attributs de police facultatif, qui sont spécifiées par le `faceAttributes` l’argument de la `DetectAsync` (méthode). Si aucune image n’est détectée, vide `Face` tableau sera renvoyé.

Lors de l’interprétation des résultats à partir de l’API de Face, l’émotion détectée doit être interprétée comme l’émotion avec le score le plus élevé, comme les scores sont normalisées à la somme est égale à un. Par conséquent, l’exemple d’application affiche l’émotion reconnue avec le score le plus élevé pour la plus grande face détecté dans l’image, comme indiqué dans les captures d’écran suivants :

![](emotion-recognition-images/emotion-recognition.png "Reconnaissance émotion")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API de Face pour reconnaître émotion, d’évaluer une application de Xamarin.Forms. L’API Face prend une expression de visage dans une image en tant qu’entrée et retourne les données qui inclut la confiance dans un ensemble d’émotions pour chaque police dans l’image.

## <a name="related-links"></a>Liens associés

- [Sont confrontés à un API](/azure/cognitive-services/face/overview/).
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
