---
title: "L’authentification des utilisateurs avec un Service de SimpleDB Amazon"
description: "Amazon SimpleDB n’offre pas de son propre système d’autorisations basées sur les ressources. Au lieu de cela, l’authentification auprès d’un fournisseur d’identité peut être utilisée pour vous assurer que les utilisateurs ont accès qu’à leurs propres données dans le domaine SimpleDB. Cet article explique comment restreindre l’accès des utilisateurs à leurs propres données SimpleDB."
ms.topic: article
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 85413cf223e794ad2fda093601f9221d0261af39
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>L’authentification des utilisateurs avec un Service de SimpleDB Amazon

_Amazon SimpleDB n’offre pas de son propre système d’autorisations basées sur les ressources. Au lieu de cela, l’authentification auprès d’un fournisseur d’identité peut être utilisée pour vous assurer que les utilisateurs ont accès qu’à leurs propres données dans le domaine SimpleDB. Cet article explique comment restreindre l’accès des utilisateurs à leurs propres données SimpleDB._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) est utilisé dans l’exemple d’application pour gérer le processus d’authentification et les stocker en toute sécurité les détails du compte d’utilisateur sur l’appareil. Pour plus d’informations, consultez [authentification des utilisateurs avec un fournisseur d’identité](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>Un utilisateur authentifié l’accès aux données du domaine de SimpleDB

L’exemple d’application utilise le `TodoItem` classe aux données du modèle. Pour stocker un `TodoItem` instance dans un service SimpleDB il doit tout d’abord être converti en un `List` de `ReplaceableAttribute` objets. Pour plus d’informations, consultez [SimpleDB de création d’objets](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> Dans iOS 9 et supérieur, sécurité de Transport d’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et l’application, ce qui évite la divulgation accidentelle d’informations sensibles. Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, toutes les connexions sont soumis aux exigences de sécurité de couches d’application. Si les connexions ne répondent pas à ces exigences, il échoue avec une exception.
> ATS peuvent être ignorées si elle n’est pas possible d’utiliser le `HTTPS` de protocole et de sécuriser les communications pour les ressources internet. Cela peut être obtenue en mettant à jour de l’application **Info.plist** fichier. Pour plus d’informations, consultez [sécurité du Transport application](~/ios/app-fundamentals/ats.md).

Pour vous assurer que les utilisateurs ont accès uniquement leurs propres données dans le domaine SimpleDB, le `ToSimpleDBReplaceableAttributes` méthode stocke un attribut supplémentaire pour un `TodoItem` de l’instance, comme indiqué dans l’exemple de code suivant :

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

Cet attribut permet de s’assurer que chaque élément stocké dans le domaine SimpleDB a une adresse de messagerie associée pour un utilisateur, ce qui est utilisé pour identifier de façon unique l’utilisateur qu'appartiennent les données. Lorsque le contenu d’un domaine est récupéré en appelant le `AmazonSimpleDBClient.SelectAsync` méthode, l’expression de requête permet de s’assurer que seuls les éléments de l’utilisateur authentifié sont récupérées, comme indiqué dans l’exemple de code suivant :

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

Le `SelectAsync` méthode retourne une réponse contenant une collection d’éléments et attributs qui correspondent à l’expression de requête. L’expression de requête garantit que seuls les éléments qui correspondent aux adresse de messagerie de l’utilisateur sont récupérées. Pour plus d’informations sur les expressions de requête, consultez [de choisir de créer des requêtes Amazon SimpleDB](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) sur le site Web d’Amazon.

> [!NOTE]
> **Remarque**: veillez à respecter les règles d’indentation lors de la construction de l’expression de requête. Pour plus d’informations, consultez [sélectionner les règles de devis](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) sur le site Web d’Amazon.

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment restreindre l’accès des utilisateurs à leurs propres données SimpleDB. Amazon SimpleDB n’offre pas de son propre système d’autorisations basées sur les ressources. Au lieu de cela, l’authentification auprès d’un fournisseur d’identité peut être utilisée pour vous assurer que les utilisateurs ont accès uniquement leurs propres données dans le domaine SimpleDB.


## <a name="related-links"></a>Liens associés

- [TodoAWSAuth (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Utilisation d’un Service de SimpleDB Amazon](~/xamarin-forms/data-cloud/consuming/aws.md)
- [L’authentification des utilisateurs avec un fournisseur d’identité](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Identité de Cognito Amazon](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Documentation du développeur de SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK pour .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
