---
title: "Traduction de texte à l’aide de l’API du traducteur"
description: "L’API du traducteur Microsoft peut être utilisé pour convertir les vocale et texte via une API REST. Cet article explique comment utiliser l’API de traduction de texte de Microsoft pour traduire le texte d’une langue à l’autre dans une application de Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: f403ebaffdf742c22e61b73aee7a42648fe597dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="text-translation-using-the-translator-api"></a>Traduction de texte à l’aide de l’API du traducteur

_L’API du traducteur Microsoft peut être utilisé pour convertir les vocale et texte via une API REST. Cet article explique comment utiliser l’API de traduction de texte de Microsoft pour traduire le texte d’une langue à l’autre dans une application de Xamarin.Forms._

## <a name="overview"></a>Vue d'ensemble

L’API du traducteur comprend deux composants :

- Une traduction du texte API REST pour traduire le texte d’une langue dans le texte d’une autre langue. L’API détecte automatiquement la langue du texte qui a été envoyé avant la translation.
- Une traduction vocale API REST pour la transcription de reconnaissance vocale à partir d’une langue dans le texte d’une autre langue. L’API intègre également des fonctionnalités de synthèse vocale pour énoncer le texte traduit précédent.

Cet article se concentre sur la traduction de texte d’une langue à une autre à l’aide de l’API de traduction du texte.

Une clé d’API doit être obtenue pour utiliser l’API de traduction du texte. Cela peut être obtenu en suivant les instructions à [mise en route](http://docs.microsofttranslator.com/text-translate.html) sur [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

Pour plus d’informations sur l’API du traducteur de Microsoft, consultez [Microsoft Translator Documentation](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview) sur microsoft.com.

## <a name="authentication"></a>Authentification

Chaque demande adressée à l’API de traduction de texte requiert un jeton d’accès JSON Web Token (JWT), qui peut être obtenu à partir du service de jeton de services cognitifs à `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Un jeton peut être obtenu en effectuant une requête POST pour le service de jeton, en spécifiant un `Ocp-Apim-Subscription-Key` en-tête qui contient la clé API en tant que sa valeur.

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

Le jeton d’accès doit être spécifié dans chaque API de traduction du texte appeler comme un `Authorization` en-tête préfixé avec la chaîne `Bearer`, comme illustré dans l’exemple de code suivant :

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Pour plus d’informations sur le service de jeton cognitifs services, consultez [API de jeton d’authentification](http://docs.microsofttranslator.com/oauth-token.html) sur [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

## <a name="performing-text-translation"></a>Effectuer la traduction de texte

Traduction de texte peut être obtenue en faisant une demande GET pour le `Translate` API à `https://api.microsofttranslator.com/v2/http.svc/Translate`. Dans l’exemple d’application, le `TranslateTextAsync` méthode appelle le processus de conversion de texte :

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

Le `TranslateTextAsync` méthode génère un URI de demande et récupère un jeton d’accès à partir du service d’émission de jeton. La demande de conversion de texte est ensuite envoyée à la `Translate` API, qui retourne une réponse XML qui contient le résultat. La réponse XML est analysée, et le résultat de la traduction est retourné à la méthode d’appel pour l’affichage.

Pour plus d’informations sur les API REST de traduction de texte, consultez [exemple de Code](http://docs.microsofttranslator.com/text-translate.html#/default) sur [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="configuring-text-translation"></a>Configuration de la traduction de texte

Le processus de conversion de texte peut être configuré en spécifiant des paramètres de requête HTTP. Il existe des paramètres obligatoires et facultatifs, avec la méthode suivante indique les paramètres obligatoires qui doivent être définies :

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Cette méthode définit le texte à traduire et la langue à traduire le texte. Pour obtenir la liste des langues prises en charge par Microsoft Translator, consultez [langues](https://www.microsoft.com/translator/languages.aspx) sur microsoft.com.

> [!NOTE]
> Si une application a besoin de savoir quelle langue le texte se trouve dans, le `Detect` API peut être appelée pour détecter la langue de la chaîne de texte.

Pour plus d’informations sur les paramètres obligatoires et facultatifs, consultez [API de traduction de texte](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) sur [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="sending-the-request"></a>Envoi de la demande

Le `SendRequestAsync` méthode rend la requête GET à l’API REST de traduction de texte et retourne la réponse :

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Cette méthode crée la demande GET en ajoutant le jeton d’accès à la `Authorization` en-tête, précédé de la chaîne `Bearer`. La demande GET est ensuite envoyée à la `Translate` API, avec l’URL de requête qui spécifie le texte à traduire et la langue à traduire le texte. La réponse est ensuite lu et retournée à la méthode d’appel.

Le `Translate` API enverra un code d’état HTTP 200 (OK) dans la réponse, fournie est que la demande est valide, ce qui signifie que la demande a réussi et que les informations demandées dans la réponse. Pour obtenir la liste des réponses d’erreur possibles, consultez les Messages de réponse à [obtenir traduire](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) sur [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="processing-the-response"></a>Traitement de la réponse

La réponse de l’API est renvoyée au format XML. Les données XML suivantes affiche un message de réponse correcte classique :

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

Dans l’exemple d’application, la réponse XML est analysée dans un `XDocument` instance, avec la valeur de racine XML retournée à la méthode d’appel pour l’affichage comme indiqué dans les captures d’écran suivants :

![](text-translation-images/text-translation.png "Traduction de texte en allemand")

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment utiliser l’API de traduction de texte de Microsoft pour traduire le texte d’une langue dans le texte d’une autre langue dans une application de Xamarin.Forms. En plus de la traduction de texte, l’API du traducteur Microsoft peut également transcription de reconnaissance vocale à partir d’une langue dans le texte d’une autre langue.



## <a name="related-links"></a>Liens associés

- [Documentation de Microsoft Translator](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)
- [Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Services de troubles cognitifs TODO (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [API de traduction de texte](http://docs.microsofttranslator.com/text-translate.html)
