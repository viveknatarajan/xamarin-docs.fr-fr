---
title: Authentification de l’accès aux Services Web
description: Ce guide explique comment intégrer des services d’authentification à une application de Xamarin.Forms pour permettre aux utilisateurs de partager un serveur principal lors de l’accès uniquement à leurs propres données. Les rubriques à l’aide de l’authentification de base avec un service REST, à l’aide du composant Xamarin.Auth pour s’authentifier auprès de fournisseurs d’identité OAuth, et en utilisant les mécanismes d’authentification intégré offerts par des fournisseurs différents.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: df0e188efd2791b03a63c31b715ed1da77079230
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-access-to-web-services"></a>Authentification de l’accès aux Services Web

_Ce guide explique comment intégrer des services d’authentification à une application de Xamarin.Forms pour permettre aux utilisateurs de partager un serveur principal lors de l’accès uniquement à leurs propres données. Les rubriques à l’aide de l’authentification de base avec un service REST, à l’aide du composant Xamarin.Auth pour s’authentifier auprès de fournisseurs d’identité OAuth, et en utilisant les mécanismes d’authentification intégré offerts par des fournisseurs différents._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[L’authentification d’un Service Web RESTful](rest.md)

HTTP prend en charge l’utilisation de plusieurs mécanismes d’authentification pour contrôler l’accès aux ressources. L’authentification de base permet d’accéder aux ressources pour seulement les clients ayant les informations d’identification correctes. Cet article explique comment utiliser l’authentification de base pour protéger l’accès aux ressources du service web RESTful.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[L’authentification des utilisateurs avec un fournisseur d’identité](oauth.md)

Xamarin.Auth est un kit de développement multiplateforme pour l’authentification des utilisateurs et le stockage de leurs comptes. Il inclut les authentificateurs OAuth qui fournissent la prise en charge pour l’utilisation de fournisseurs d’identité tels que Google, Microsoft, Facebook et Twitter. Cet article explique comment utiliser Xamarin.Auth pour gérer le processus d’authentification dans une application de Xamarin.Forms.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[L’authentification des utilisateurs avec les applications mobiles Azure](azure.md)

Azure Mobile Apps utiliser divers modules fournisseurs d’identité externe pour prendre en charge l’authentification et l’autorisation des utilisateurs de l’application. Autorisations peuvent ensuite être définies sur les tables pour restreindre l’accès aux utilisateurs authentifiés uniquement. Cet article explique comment utiliser des applications mobiles Azure pour gérer le processus d’authentification dans une application de Xamarin.Forms.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[L’authentification des utilisateurs avec Azure Active Directory B2C](azure-ad-b2c.md)

Azure B2C Active Directory est une solution de gestion d’identité cloud pour les applications pour les consommateurs web et mobiles. Cet article explique comment utiliser la bibliothèque d’authentification Microsoft (MSAL) et Azure Active Directory B2C pour intégrer la gestion des identités dans une application de Xamarin.Forms.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Intégration d’Azure Active Directory B2C avec Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Azure B2C Active Directory peut être utilisé pour gérer le flux de travail de l’authentification pour les applications mobiles Azure. Avec cette approche, l’expérience de gestion d’identité est entièrement définie dans le cloud et peut être modifiée sans modifier votre code d’application mobile. Cet article explique comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance Azure Mobile Apps avec Xamarin.Forms.

## <a name="authenticating-users-with-an-amazon-simpledb-serviceawsmd"></a>[L’authentification des utilisateurs avec un Service de SimpleDB Amazon](aws.md)

Amazon SimpleDB n’offre pas de son propre système d’autorisations basées sur les ressources. Au lieu de cela, l’authentification auprès d’un fournisseur d’identité peut être utilisée pour vous assurer que les utilisateurs ont accès qu’à leurs propres données dans le domaine SimpleDB. Cet article explique comment restreindre l’accès des utilisateurs à leurs propres données SimpleDB.


## <a name="related-links"></a>Liens associés

- [Introduction aux services web](~/cross-platform/data-cloud/web-services/index.md)
- [Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)
