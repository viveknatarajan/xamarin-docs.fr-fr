---
title: Authentification de l’accès aux services web
description: Ce guide explique comment intégrer des services d’authentification dans une application Xamarin.Forms pour permettre aux utilisateurs de partager un serveur principal tout en ayant uniquement accès à leurs propres données.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: d598a9b3de31ea6823530f911c3544bf3cebb37f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331180"
---
# <a name="authenticating-access-to-web-services"></a>Authentification de l’accès aux services web

_Ce guide explique comment intégrer des services d’authentification dans une application Xamarin.Forms pour permettre aux utilisateurs de partager un serveur principal tout en ayant uniquement accès à leurs propres données. Les sujets abordés incluent l’authentification de base avec un service REST, à l’aide du composant Xamarin.Auth pour s’authentifier auprès des fournisseurs d’identité OAuth, et à l’aide de mécanismes d’authentification intégrés proposés par différents fournisseurs._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[L’authentification d’un Service Web RESTful](rest.md)

HTTP prend en charge l’utilisation de plusieurs mécanismes d’authentification pour contrôler l’accès aux ressources. L’authentification de base fournit l’accès aux ressources aux clients qui ont les informations d’identification correctes. Cet article montre comment utiliser l’authentification de base pour protéger l’accès aux ressources du service web RESTful.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Authentification des utilisateurs avec un fournisseur d’identité](oauth.md)

Xamarin.Auth est un kit de développement logiciel inter-plateformes pour l’authentification des utilisateurs et le stockage de leurs comptes. Il inclut les authentificateurs OAuth qui prennent en charge pour l’utilisation de fournisseurs d’identité comme Google, Microsoft, Facebook et Twitter. Cet article explique comment utiliser Xamarin.Auth pour gérer le processus d’authentification dans une application Xamarin.Forms.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Authentification des utilisateurs avec Azure Mobile Apps](azure.md)

Azure Mobile Apps utiliser divers fournisseurs d’identité externes pour prendre en charge l’authentification et autorisation des utilisateurs de l’application. Autorisations peuvent ensuite être définies sur les tables pour restreindre l’accès aux seuls utilisateurs authentifiés. Cet article explique comment utiliser Azure Mobile Apps pour gérer le processus d’authentification dans une application Xamarin.Forms.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Authentification des utilisateurs avec Azure Active Directory B2C](azure-ad-b2c.md)

Azure B2C Active de répertoire est une solution de gestion des identités de cloud pour applications mobiles et web destinées aux consommateurs. Cet article montre comment utiliser Microsoft Authentication Library (MSAL) et Azure Active Directory B2C pour intégrer la gestion des identités de consommateur dans une application Xamarin.Forms.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Intégration d’Azure Active Directory B2C avec Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Azure B2C Active de répertoire peut être utilisé pour gérer le flux de travail de l’authentification pour Azure Mobile Apps. Avec cette approche, l’expérience de gestion d’identité est entièrement défini dans le cloud et peut être modifié sans modifier votre code d’application mobile. Cet article montre comment utiliser Azure Active Directory B2C pour fournir l’authentification et l’autorisation à une instance d’Azure Mobile Apps avec Xamarin.Forms.

## <a name="related-links"></a>Liens associés

- [Introduction aux services web](~/cross-platform/data-cloud/web-services/index.md)
- [Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)
