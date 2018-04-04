---
title: À l’aide de la vérification d’orthographe Bing API de vérification orthographique
description: Vérifiez l’orthographe Bing effectue contextuelles de correction orthographique pour le texte, de suggestions d’inline pour les mots mal orthographiés. Cet article explique comment utiliser l’API REST de vérification de l’orthographe Bing pour corriger des fautes d’orthographe dans une application de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 41bd79b22aa193dd5303847997bc07e8e8d12e58
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>À l’aide de la vérification d’orthographe Bing API de vérification orthographique

_Vérifiez l’orthographe Bing effectue contextuelles de correction orthographique pour le texte, de suggestions d’inline pour les mots mal orthographiés. Cet article explique comment utiliser l’API REST de vérification de l’orthographe Bing pour corriger des fautes d’orthographe dans une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

L’API REST de vérification de l’orthographe Bing a deux modes de fonctionnement, et un mode doit être spécifié lors d’une demande à l’API :

- `Spell` corrige le texte court (mots jusqu'à 9) sans aucune modification de la casse.
- `Proof` corrige le texte long, fournit des corrections de la casse et ponctuation de base et supprime les corrections agressives.

Une clé d’API doit être obtenue pour utiliser l’API de vérification orthographique Bing. Cela peut être obtenue au [essayer les Services cognitifs](https://azure.microsoft.com/try/cognitive-services/)

Pour obtenir la liste des langues prises en charge par l’API de vérification orthographique Bing, consultez [langues prises en charge](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Pour plus d’informations sur l’API de vérification orthographique Bing, consultez [Documentation de vérifier l’orthographe Bing](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Authentification

Chaque demande adressée à l’API de vérification orthographique Bing nécessite une clé d’API qui doit être spécifiée comme valeur de la `Ocp-Apim-Subscription-Key` en-tête. L’exemple de code suivant montre comment ajouter la clé API à le `Ocp-Apim-Subscription-Key` en-tête d’une requête :

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Pour passer d’une clé d’API valide à l’API de vérification orthographique Bing entraînera une erreur de 401 réponse.

## <a name="performing-spell-checking"></a>Exécution de la vérification orthographique

La correction orthographique peut être obtenue en faisant une demande GET ou POST pour la `SpellCheck` API à `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`. Lorsque vous élaborez une demande GET, le texte à vérifier l’orthographe est envoyé en tant que paramètre de requête. Lorsque vous élaborez une demande POST, vérifié l’orthographe du texte est envoyé dans le corps de la demande. Les requêtes GET sont limités à 1 500 caractères en raison de la limite de longueur de chaîne de paramètre requête de correction orthographique. Par conséquent, les requêtes POST généralement doivent être effectuées, sauf si les chaînes courtes sont en cours de vérifier l’orthographe.

Dans l’exemple d’application, le `SpellCheckTextAsync` méthode appelle le processus de correction orthographique :

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

Le `SpellCheckTextAsync` méthode génère un URI de demande et envoie ensuite la demande à la `SpellCheck` API, qui retourne une réponse JSON qui contient le résultat. La réponse JSON est désérialisée, le résultat à la méthode d’appel pour l’affichage.

### <a name="configuring-spell-checking"></a>Configuration de la vérification orthographique

Le processus de correction orthographique peut être configuré en spécifiant les paramètres de la requête HTTP :

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

Pour plus d’informations sur l’API REST de vérification de l’orthographe Bing, consultez [référence de v7 d’API de vérification orthographique](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Envoi de la demande

Le `SendRequestAsync` méthode rend la requête GET à l’API REST de vérification de l’orthographe Bing et retourne la réponse :

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Cette méthode crée la demande GET en ajoutant la clé d’API comme valeur de la `Ocp-Apim-Subscription-Key` en-tête. La demande GET est ensuite envoyée à la `SpellCheck` API, avec l’URL de requête qui spécifie le texte à traduire et le mode de vérification orthographique. La réponse est ensuite lu et retournée à la méthode d’appel.

Le `SpellCheck` API enverra un code d’état HTTP 200 (OK) dans la réponse, fournie est que la demande est valide, ce qui signifie que la demande a réussi et que les informations demandées dans la réponse. Pour obtenir la liste des objets de réponse, consultez [des objets de réponse](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Traitement de la réponse

La réponse de l’API est renvoyée au format JSON. Les données JSON suivantes illustre le message de réponse pour le texte mal orthographié `Go shappin tommorow`:

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
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

- [Documentation de vérification orthographique Bing](/azure/cognitive-services/bing-spell-check/)
- [Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Référence de v7 d’API de vérification de l’orthographe Bing](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
