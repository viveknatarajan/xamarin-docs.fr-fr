---
title: L’authentification d’un Service Web RESTful
description: L’authentification de base permet d’accéder aux ressources pour seulement les clients ayant les informations d’identification correctes. Cet article explique comment utiliser l’authentification de base pour protéger l’accès aux ressources du service web RESTful.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 42680ed8b79560f6f4f9f12892f7da5637a7af16
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240959"
---
# <a name="authenticating-a-restful-web-service"></a>L’authentification d’un Service Web RESTful

_HTTP prend en charge l’utilisation de plusieurs mécanismes d’authentification pour contrôler l’accès aux ressources. L’authentification de base permet d’accéder aux ressources pour seulement les clients ayant les informations d’identification correctes. Cet article explique comment utiliser l’authentification de base pour protéger l’accès aux ressources du service web RESTful._

L’exemple d’application Xamarin.Forms accompagnement consomme un service hébergé de Xamarin REST qui fournit l’accès en lecture seule au service web. Par conséquent, les opérations créent, mettre à jour et supprimer des données ne modifiera pas les données utilisées dans l’application. Toutefois, une version principale du service REST est disponible dans le *TodoRESTService* dossier dans l’exemple d’application et obtenir des instructions sur la configuration du service peut se trouver il. Cette version principale du service REST fournit intégral créer, supprimer, lecture et mise à jour d’accéder aux données.

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>L’authentification des utilisateurs via HTTP

L’authentification de base est le mécanisme d’authentification la plus simple pris en charge par le protocole HTTP et implique le client qui envoie le nom d’utilisateur et un mot de passe sous forme de texte non chiffré en base64 encodé. Il fonctionne comme suit :

- Si un service web reçoit une demande pour une ressource protégée, elle rejette la demande avec un code d’état HTTP 401 (accès refusé) et définit l’en-tête de réponse WWW-Authenticate, tel qu’indiqué dans le diagramme suivant :

![](rest-images/basic-authentication-fail.png "Échec de l’authentification de base")

- Si un service web reçoit une demande pour une ressource protégée, avec la `Authorization` en-tête correctement défini, le web service doit répondre avec un code d’état HTTP 200, ce qui indique que la demande a réussi et que les informations demandées figurent dans la réponse. Ce scénario est illustré dans le diagramme suivant :

![](rest-images/basic-authentication-success.png "Réussite de l’authentification de base")

> [!NOTE]
> L’authentification de base doit être utilisée uniquement sur une connexion HTTPS. Lorsqu’il est utilisé sur une connexion HTTP, le <code>Authorization</code> en-tête peut facilement être décodé si le trafic HTTP est capturé par une personne malveillante.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Spécification de l’authentification de base dans une requête Web

Utilisation de l’authentification de base est spécifiée comme suit :

1. La chaîne « Base » est ajouté à la `Authorization` en-tête de la demande.
1. Le nom d’utilisateur et un mot de passe sont combinées en une chaîne au format « nom », qui est ensuite base64 codé et ajouté à la `Authorization` en-tête de la demande.

Par conséquent, avec un nom d’utilisateur de 'XamarinUser' et un mot de passe de 'XamarinPassword', l’en-tête devient :

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

Le `HttpClient` classe peut définir le `Authorization` valeur d’en-tête sur la `HttpClient.DefaultRequestHeaders.Authorization` propriété. Étant donné que la `HttpClient` instance existe dans plusieurs requêtes, la `Authorization` en-tête doit uniquement être définie qu’une seule fois, plutôt que quand effectuer chaque demande, comme indiqué dans l’exemple de code suivant :

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Ensuite, lorsqu’une demande est faite pour une opération de service web la demande est signée avec la `Authorization` en-tête, qui indique si l’utilisateur a l’autorisation d’appeler l’opération.

> [!NOTE]
> Tandis que l’exemple de service REST stocke les informations d’identification en tant que constantes, ils ne doivent pas être stockées dans un format non sécurisé dans une application publiée. Le [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet fournit les fonctionnalités de stockage sécurisé des informations d’identification. Pour plus d’informations, consultez [le stockage et la récupération des informations de compte sur les appareils](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="processing-the-authorization-header-server-side"></a>Traitement côté serveur en-tête d’autorisation

L’exemple de service REST qui l’accompagne décore chaque action avec le `[BasicAuthentication]` attribut. Cet attribut est implémenté par le `BasicAuthenticationAttribute` classe dans la solution et est utilisé pour analyser les `Authorization` en-tête et détermine si les informations d’identification codé en base64 valides en les comparant aux valeurs stockées dans *Web.config*. Cette approche est appropriée pour l’exemple de service, il implique d’étendre pour un service web de publics.

Dans le module d’authentification de base utilisé par IIS, les utilisateurs sont authentifiés par rapport à leurs informations d’identification Windows. Par conséquent, les utilisateurs doivent disposer de comptes sur le domaine du serveur. Toutefois, le modèle de l’authentification de base peut être configuré pour autoriser l’authentification personnalisée, où les comptes d’utilisateurs sont authentifiés par rapport à une source externe, par exemple une base de données. Pour plus d’informations, consultez [l’authentification de base dans l’API Web ASP.NET](http://www.asp.net/web-api/overview/security/basic-authentication) sur le site Web ASP.NET.

> [!NOTE]
> L’authentification de base n’est pas conçue pour gérer la déconnexion. Par conséquent, l’approche de l’authentification de base standard pour la déconnexion est à la fin de la session.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment ajouter l’authentification de base pour les demandes web effectuées par une application de Xamarin.Forms à l’aide de la `HttpClient` classe. L’authentification de base permet d’accéder aux ressources pour seulement les clients ayant les informations d’identification correctes. Pour plus d’informations sur l’utilisation de [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) pour gérer le processus d’authentification dans une application Xamarin.Forms afin que les utilisateurs puissent partager un serveur principal tout en uniquement ayant accès à leurs données, consultez [authentification des utilisateurs avec un fournisseur d’identité](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="related-links"></a>Liens associés

- [TodoREST (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [Utilisation d’un service web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
