---
title: "Reconnaissance émotion à l’aide de l’API émotion"
description: "L’API émotion prend une expression de visage dans une image en tant qu’entrée et retourne les niveaux de confiance sur un ensemble d’émotions pour chaque police dans l’image. Cet article explique comment utiliser l’API émotion de reconnaître émotion, d’évaluer une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>Reconnaissance émotion à l’aide de l’API émotion

_L’API émotion prend une expression de visage dans une image en tant qu’entrée et retourne les niveaux de confiance sur un ensemble d’émotions pour chaque police dans l’image. Cet article explique comment utiliser l’API émotion de reconnaître émotion, d’évaluer une application de Xamarin.Forms._

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!NOTE]
> L’API émotion est encore en version préliminaire. Il peut être modifications avec rupture à l’API avant la version finale.

## <a name="overview"></a>Vue d'ensemble

L’API émotion peut détecter anger, contempt, répugnantes, peur, bonheur, neutre, leur et surprise, dans une expression de visage. Ces émotions sont globalement, dans toutes les cultures communiquées via les mêmes expressions de visages de base. Ainsi que de retourner un résultat d’émotion pour une expression faciale, l’API émotion retourne également un rectangle englobant pour des faces détectés à l’aide de l’API de Face. Si un utilisateur a déjà appelé l’API Face, ils peuvent envoyer le rectangle face comme une entrée facultative. Notez qu’une clé d’API doit être obtenue pour utiliser l’API émotion. Cela peut être obtenue au [mise en route gratuitement](https://www.microsoft.com/cognitive-services/sign-up) sur microsoft.com.

La reconnaissance émotion peut être effectuée via une bibliothèque cliente et une API REST. Cet article se concentre sur la réalisation de reconnaissance émotion via la [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) bibliothèque cliente, qui peut être téléchargé à partir de NuGet.

L’API émotion peut également servir à reconnaître les expressions de visages de personnes de vidéo et renvoie un récapitulatif de leurs émotions. Pour plus d’informations, consultez [émotion vidéo](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) sur microsoft.com.

Pour plus d’informations sur l’API émotion, consultez [Documentation de l’API émotion](https://www.microsoft.com/cognitive-services/emotion-api/documentation) sur microsoft.com.

## <a name="performing-emotion-recognition"></a>Exécution de la reconnaissance émotion

La reconnaissance émotion est obtenue en téléchargeant des flux d’images à l’API émotion. La taille du fichier image ne doit pas être supérieure à 4 Mo, et les formats de fichier pris en charge sont BMP, GIF, JPEG et PNG. Dans l’image, la plage de tailles de police détectables est 36 x 36 à 4096 x 4096 pixels. Les faces en dehors de cette plage ne pas être détectés.

L’exemple de code suivant illustre le processus de reconnaissance émotion :

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

Un `EmotionServiceClient` instance doit être créée pour effectuer une reconnaissance émotion, avec la clé API d’émotion passée comme argument à la `EmotionServiceClient` constructeur.

Le `RecognizeAsync` (méthode), qui est appelé sur le `EmotionServiceClient` d’une instance, il télécharge une image à l’API émotion, comme un `Stream`. La clé d’API sera soumise à l’API émotion lorsque cette opération est appelée. Pour envoyer une clé d’API valide provoque l’affichage dans un `Microsoft.ProjectOxford.Common.ClientException` levée avec le message d’exception indiquant qu’une clé API non valide a été envoyée.

Le `RecognizeAsync` méthode retournera un `Emotion` de tableau, si un type a été reconnu. Pour chaque image, le nombre maximal de faces qui peut être détectée est 64, et les faces sont classés par la taille du rectangle face dans l’ordre décroissant. Si aucune image n’est détectée, vide `Emotion` tableau sera renvoyé.

Lors de l’interprétation des résultats à partir de l’API émotion, l’émotion détectée doit être interprétée comme l’émotion avec le score le plus élevé, comme les scores sont normalisées à la somme est égale à un. Par conséquent, l’exemple d’application affiche l’émotion reconnue avec le score le plus élevé pour la plus grande face détecté dans l’image, comme indiqué dans les captures d’écran suivants :

![](emotion-recognition-images/emotion-recognition.png "Reconnaissance émotion")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API émotion de reconnaître émotion, d’évaluer une application de Xamarin.Forms. L’API émotion prend une expression de visage dans une image en tant qu’entrée et retourne la confiance dans un ensemble d’émotions pour chaque police dans l’image.


## <a name="related-links"></a>Liens associés

- [Documentation d’émotion API](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [API émotion de](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
