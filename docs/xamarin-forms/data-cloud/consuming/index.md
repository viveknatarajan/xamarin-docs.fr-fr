---
title: Consommation de services web
description: Ce guide montre comment communiquer avec les services web différents pour fournir créer, lire, mettre à jour, fonctionnalités et de suppression (CRUD) pour une application Xamarin.Forms. Les sujets abordés incluent la communication avec les services ASMX, WCF services, services REST et Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a4c842ea7fd37ade9be0a9cb3e3ff7e50a6d1491
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61328188"
---
# <a name="consuming-web-services"></a>Consommation de services web

Ce_t guide montre comment communiquer avec les services web différents pour fournir créer, lire, mettre à jour, fonctionnalités et de suppression (CRUD) pour une application Xamarin.Forms. Les sujets abordés incluent la communication avec les services ASMX, WCF services, services REST et Azure Mobile Apps.

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Utilisation d’un Service Web ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

Services Web ASP.NET (ASMX) offrent la possibilité de créer des services web qui envoient des messages via HTTP à l’aide de SOAP Simple Object Access Protocol (). SOAP est un protocole indépendant de la plateforme et indépendant du langage pour la création et l’accès aux services web. Consommateurs d’un service ASMX n’avez pas besoin de savoir quoi que ce soit sur la plateforme, le modèle d’objet ou le langage de programmation utilisé pour implémenter le service. Ils doivent uniquement comprendre comment envoyer et recevoir des messages SOAP. Cet article montre comment consommer un service web ASMX à partir d’une application Xamarin.Forms.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Utilisation d’un Service Web de Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF est un cadre unifié de Microsoft pour la création d’applications orientées service. Il permet aux développeurs de créer des applications distribuées traitées, fiables, sécurisées et interopérables. Il existe des différences entre ASP.NET Web Services (ASMX) et WCF, mais il est important de comprendre que WCF prend en charge les mêmes fonctionnalités que fournit ASMX, les messages SOAP sur HTTP. Cet article illustre l’utilisation d’un service WCF SOAP à partir d’une application Xamarin.Forms.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) est un style d’architecture pour la création de services web. Demandes REST sont établies via HTTP en utilisant les mêmes verbes HTTP qui utilisent de navigateurs web pour récupérer les pages web et d’envoyer des données aux serveurs. Cet article montre comment utiliser un service web RESTful à partir d’une application Xamarin.Forms.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps vous permet de vous permet de développer des applications avec les serveurs principaux évolutives hébergées dans Azure App Service, avec prise en charge pour l’authentification d’appareils mobiles, de synchronisation hors connexion et de notifications push. Cet article, qui s’applique uniquement aux Azure Mobile Apps qui utilisent un serveur principal Node.js, explique comment interroger, insérer, mettre à jour et supprimer les données stockées dans une table dans une instance d’Azure Mobile Apps.

## <a name="related-links"></a>Liens associés

- [Introduction aux services web](~/cross-platform/data-cloud/web-services/index.md)
- [Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)
