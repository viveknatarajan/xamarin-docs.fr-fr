---
title: Règles d’analyse Xamarin.iOS
description: Ce document décrit un ensemble de règles d’analyse qui vérifient les paramètres du projet Xamarin.iOS pour vous aider à déterminer si les paramètres plus/better-optimized sont disponibles.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: 8a4990ce7b2bcacbd4b97b214458531b3d94122e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121033"
---
# <a name="xamarinios-analysis-rules"></a>Règles d’analyse Xamarin.iOS

Analyse de Xamarin.iOS est un ensemble de règles qui vérifient les paramètres de votre projet pour vous aider à déterminer si des paramètres optimisés meilleure/plus élevé sont disponibles.

Exécuter les règles d’analyse aussi souvent que possible pour rechercher les améliorations possibles au début et de gagner du temps de développement.

Pour exécuter les règles, dans Visual Studio pour le menu du Mac, sélectionnez **projet > exécuter l’analyse du Code**.

> [!NOTE]
> Analyse de Xamarin.iOS s’exécute uniquement sur votre configuration actuellement sélectionnée. Nous vous recommandons vivement de l’outil pour le débogage en cours d’exécution **et** configurations release.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001 : DisabledLinkerRule

- **Problème :** l’éditeur de liens est désactivé sur l’appareil pour le mode de débogage.
- **Correctif :** vous essayez d’exécuter votre code avec l’éditeur de liens afin d’éviter les surprises.
Pour configurer, accéder au projet > Build iOS > comportement de l’éditeur de liens.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002 : TestCloudAgentReleaseRule

- **Problème :** builds d’applications qui initialisent l’agent de Test Cloud sont rejetées par Apple lorsque envoyé, car elles utilisent les API privée.
- **Correctif :** ajouter ou corriger les #if nécessaires et définit dans le code.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003 : IPADebugBuildsRule

- **Problème :** configuration de débogage qui utilise des clés de signature de développeur ne doit pas générer un fichier IPA qu’il est nécessaire uniquement pour la distribution, qui utilise désormais l’Assistant Publication.
- **Correctif :** désactiver la génération de fichier IPA dans Options du projet pour la configuration Debug.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004 : Missing64BitSupportRule

- **Problème :** l’architecture prise en charge pour « mise en production | l’appareil » n’est pas compatible, manquant ARM64 64 bits. Il s’agit d’un problème comme Apple n’accepte pas les applications iOS uniquement 32 bits dans l’App Store.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS, générer et modifier les architectures prises en charge, par conséquent, il a ARM64.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005 : Float32Rule

- **Problème :** ne pas à l’aide de l’option float32 (--aot-options-O = = float32) entraîne des performances importantes de coût, en particulier sur mobile, où les mathématiques à double précision est sensiblement plus lente. Notez que .NET utilise double précision en interne, même pour float, par conséquent, l’activation de cette option affecte la précision et, éventuellement, de compatibilité.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS générer et décochez le « Effectuer toutes les opérations de virgule flottante 32 bits en tant que valeur float 64 bits ».

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006 : HttpClientAvoidManaged

- **Problème :** nous recommandons d’utiliser le gestionnaire HttpClient natif au lieu de celle pour de meilleures performances, plus petite taille de l’exécutable, géré et facilement prendre en charge les standards plus récents.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS, générer et modifier l’implémentation de HttpClient NSUrlSession (iOS 7 +) ou CFNetwork pour prendre en charge de la version précédente d’iOS 7.
