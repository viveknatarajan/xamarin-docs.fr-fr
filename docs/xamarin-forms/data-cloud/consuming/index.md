---
title: Utilisation de Services Web
description: Ce guide montre comment communiquer avec les services web différents pour fournir créer, lire, mettre à jour et supprimer des fonctionnalités à une application de Xamarin.Forms (CRUD). Rubriques couvertes incluent la communication avec ASMX services WCF services, autres services, les applications mobiles Azure et Amazon Web Services.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 530b57c009a1f76d3756d7315856f74b6cda2f66
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-web-services"></a>Utilisation de Services Web

_Ce guide montre comment communiquer avec les services web différents pour fournir créer, lire, mettre à jour et supprimer des fonctionnalités à une application de Xamarin.Forms (CRUD). Rubriques couvertes incluent la communication avec ASMX services WCF services, autres services, les applications mobiles Azure et Amazon Web Services._

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Utilisation d’un Service Web ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

Les Services Web ASP.NET (ASMX) permettent de créer des services web qui envoient des messages sur HTTP à l’aide de SOAP Simple Object Access Protocol (). SOAP est un protocole indépendant de la plateforme et indépendant du langage pour la création et l’accès aux services web. Les consommateurs d’un service ASMX n’avez pas besoin de connaître quoi que ce soit sur la plateforme, modèle d’objet ou langage de programmation utilisé pour implémenter le service. Il leur suffit de comprendre comment envoyer et recevoir des messages SOAP. Cet article montre comment utiliser un service web ASMX à partir d’une application de Xamarin.Forms.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Utiliser un Service Web de Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF est un cadre unifié de Microsoft pour la création d’applications orientées service. Il permet aux développeurs de créer des applications distribuées traitées, fiables, sécurisées et interopérables. Il existe des différences entre les Services de Web ASP.NET (ASMX) et WCF, mais il est important de comprendre que WCF prend en charge les mêmes fonctionnalités que fournit des fichiers ASMX, les messages SOAP sur HTTP. Cet article montre comment consommer un service WCF SOAP à partir d’une application de Xamarin.Forms.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Utilisation d’un Service Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)

Transfert de REST (Representational State) est un style d’architecture pour la création de services web. Demandes REST sont effectuées via HTTP utilisent les mêmes verbes HTTP qui utilisent de navigateurs web pour récupérer des pages web et envoient des données à des serveurs. Cet article montre comment utiliser un service web RESTful à partir d’une application de Xamarin.Forms.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Utilisation d’une application Mobile Azure](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps permettent de développer des applications avec les serveurs principaux évolutifs est hébergés dans Azure App Service, avec prise en charge pour l’authentification mobile, synchronisation hors connexion et des notifications push. Cet article, qui s’applique uniquement aux applications mobiles Azure qui utilisent un serveur principal Node.js, explique comment interroger, insérer, mettre à jour et supprimer des données stockées dans une table dans une instance Azure Mobile Apps.

## <a name="consuming-an-amazon-simpledb-servicexamarin-formsdata-cloudconsumingawsmd"></a>[Utilisation d’un Service de SimpleDB Amazon](~/xamarin-forms/data-cloud/consuming/aws.md)

Amazon SimpleDB est un service web qui offre la possibilité de stocker et interroger des données dans un cloud d’Amazon. Cet article explique comment utiliser le AWS SDK pour .NET pour interroger, de créer et de remplacer et supprimer des données stockées dans un service SimpleDB.


## <a name="related-links"></a>Liens associés

- [Introduction aux services web](~/cross-platform/data-cloud/web-services/index.md)
- [Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)
