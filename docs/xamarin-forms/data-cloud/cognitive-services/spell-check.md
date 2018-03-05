---
title: "À l’aide de la vérification d’orthographe Bing API de vérification orthographique"
description: "Vérifiez l’orthographe Bing effectue contextuelles de correction orthographique pour le texte, de suggestions d’inline pour les mots mal orthographiés. Cet article explique comment utiliser l’API REST de vérification de l’orthographe Bing pour corriger des fautes d’orthographe dans une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: ad2bdf27323fd7d7e108a25387cd6aea6d442098
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>À l’aide de la vérification d’orthographe Bing API de vérification orthographique

_Vérifiez l’orthographe Bing effectue contextuelles de correction orthographique pour le texte, de suggestions d’inline pour les mots mal orthographiés. Cet article explique comment utiliser l’API REST de vérification de l’orthographe Bing pour corriger des fautes d’orthographe dans une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

L’API REST de vérification de l’orthographe Bing a deux modes de fonctionnement, et un mode doit être spécifié lors d’une demande à l’API :

- `Spell` corrige le texte court (mots jusqu'à 9) sans aucune modification de la casse.
- `Proof` corrige le texte long, fournit des corrections de la casse et ponctuation de base et supprime les corrections agressives.

Une clé d’API doit être obtenue pour utiliser l’API de vérification orthographique Bing. Cela peut être obtenue au [mise en route gratuitement](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) sur microsoft.com.

Pour obtenir la liste des langues prises en charge par l’API de vérification orthographique Bing, consultez [prise en charge linguistique](https://www.microsoft.com/cognitive-services/Bing-Spell-check-API/documentation#language-support) sur microsoft.com. Pour plus d’informations sur l’API de vérification orthographique Bing, consultez [API de vérification orthographique Bing](https://www.microsoft.com/cognitive-services/bing-spell-check-api/documentation) sur microsoft.com.

## <a name="authentication"></a>Authentification

Chaque demande adressée à l’API de vérification orthographique Bing nécessite une clé d’API qui doit être spécifiée comme valeur de la `Ocp-Apim-Subscription-Key` en-tête. L’exemple de code suivant montre comment ajouter la clé API à le `Ocp-Apim-Subscription-Key` en-tête d’une requête :

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
  ...
}
```

Pour passer d’une clé d’API valide à l’API de vérification orthographique Bing entraînera une erreur de 401 réponse.

## <a name="performing-spell-checking"></a>Exécution de la vérification orthographique

La correction orthographique peut être obtenue en faisant une demande GET ou POST pour la `SpellCheck` API à `https://api.cognitive.microsoft.com/bing/v5.0/SpellCheck`. Lorsque vous élaborez une demande GET, le texte à vérifier l’orthographe est envoyé en tant que paramètre de requête. Lorsque vous élaborez une demande POST, vérifié l’orthographe du texte est envoyé dans le corps de la demande. Les requêtes GET sont limités à 1 500 caractères en raison de la limite de longueur de chaîne de paramètre requête de correction orthographique. Par conséquent, les requêtes POST généralement seront, sauf si les chaînes courtes sont en cours de vérifier l’orthographe.

Dans l’exemple d’application, le `SpellCheckTextAsync` méthode appelle le processus de correction orthographique :

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
  string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
  var response = await SendRequestAsync(requestUri, Constants.BingSpellCheckApiKey);
  var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
  return spellCheckResults;
}
```

Le `SpellCheckTextAsync` méthode génère un URI de demande et envoie ensuite la demande à la `SpellCheck` API, qui retourne une réponse JSON qui contient le résultat. La réponse JSON est désérialisée, le résultat à la méthode d’appel pour l’affichage.

Pour plus d’informations sur l’API REST de vérification de l’orthographe Bing, consultez [API de vérification orthographique](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) sur microsoft.com.

### <a name="configuring-spell-checking"></a>Configuration de la vérification orthographique

Le processus de correction orthographique peut être configuré en spécifiant des paramètres de requête HTTP. Il existe des paramètres obligatoires et facultatifs, avec la méthode suivante indique les paramètres obligatoires qui doivent être définies pour une demande GET :

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Cette méthode définit le texte à vérifier l’orthographe et le mode de vérification orthographique.

Pour plus d’informations sur les paramètres obligatoires et facultatifs, consultez [API de vérification orthographique](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) sur microsoft.com.

### <a name="sending-the-request"></a>Envoi de la demande

Le `SendRequestAsync` méthode rend la requête GET à l’API REST de vérification de l’orthographe Bing et retourne la réponse :

```csharp
async Task<string> SendRequestAsync(string url, string apiKey)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Cette méthode crée la demande GET en ajoutant la clé d’API comme valeur de la `Ocp-Apim-Subscription-Key` en-tête. La demande GET est ensuite envoyée à la `SpellCheck` API, avec l’URL de requête qui spécifie le texte à traduire et le mode de vérification orthographique. La réponse est ensuite lu et retournée à la méthode d’appel.

Le `SpellCheck` API enverra un code d’état HTTP 200 (OK) dans la réponse, fournie est que la demande est valide, ce qui signifie que la demande a réussi et que les informations demandées dans la réponse. Pour obtenir la liste des réponses d’erreur possibles, consultez les réponses au [API de vérification orthographique](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) sur microsoft.com.

### <a name="processing-the-response"></a>Traitement de la réponse

La réponse de l’API est renvoyée au format JSON. Les données JSON suivantes illustre le message de réponse pour le texte mal orthographié `Go shappin tommorow`:

```csharp
{
  "_type": "SpellCheck",
  "flaggedTokens": [
    {
      "offset": 3,
      "token": "shappin",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "shopping",
          "score": 1
        }
      ]
    },
    {
      "offset": 11,
      "token": "tommorow",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "tomorrow",
          "score": 1
        }
      ]
    }
  ]
}
```

Le `flaggedTokens` tableau contient un tableau de mots dans le texte qui ont été marqués comme ne pas correctement orthographiés ou sont incorrectes. grammaire. Le tableau sera vide si aucun orthographe ou les erreurs de grammaire sont trouvés. Les balises figurant dans le tableau sont :

- `offset` – un offset de base zéro à partir du début de la chaîne de texte pour le mot qui a été signalé.
- `token` – le mot dans la chaîne de texte qui n’est pas correctement orthographié ou est incorrect de grammaire.
- `type` : le type de l’erreur qui a provoqué le mot marquage. Il existe deux valeurs possibles : `RepeatedToken` et `UnknownToken`.
- `suggestions` – un tableau de mots qui corrige l’erreur d’orthographe ou de grammaire. Le tableau est composé d’un `suggestion` et un `score`, qui indique le niveau de confiance que la correction suggérée est correcte.

Dans l’exemple d’application, la réponse JSON est désérialisée dans un `SpellCheckResult` instance, avec le résultat à la méthode d’appel pour l’affichage. Le code suivant exemple illustre comment la `SpellCheckResult` traitement pour l’affichage de l’instance :

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Ce code effectue une itération dans la `FlaggedTokens` collection et remplace tout mot mal orthographié ou mots grammaire incorrects dans le texte source avec la première suggestion. Les captures d’écran suivantes illustrent avant et après la vérification orthographique :

![](spell-check-images/before-spell-check.png "Avant de vérifier l’orthographe")

![](spell-check-images/after-spell-check.png "Après vérification orthographique")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API REST de vérification de l’orthographe Bing pour corriger des fautes d’orthographe dans une application de Xamarin.Forms. Vérifiez l’orthographe Bing effectue contextuelles de correction orthographique pour le texte, de suggestions d’inline pour les mots mal orthographiés.



## <a name="related-links"></a>Liens associés

- [Documentation de vérification orthographique Bing](https://www.microsoft.com/cognitive-services/bing-spell-check-api/documentation)
- [Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Vérifier l’orthographe Bing API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358)
