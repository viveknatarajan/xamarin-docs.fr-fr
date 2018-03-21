---
title: "Règles d’analyse Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: a63cc916d3c182baccb4ddd3c9003bdb8e5f30c7
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinios-analysis-rules"></a>Règles d’analyse Xamarin.iOS

Analyse de Xamarin.iOS est un ensemble de règles qui vérifient les paramètres de votre projet pour vous aider à déterminer si des paramètres optimisés favorable/plus sont disponibles.

Exécuter les règles d’analyse aussi souvent que possible pour rechercher des améliorations possibles au début et de gagner du temps de développement.

Pour exécuter les règles, dans Visual Studio pour le menu du Mac, sélectionnez **projet > exécuter l’analyse du Code**.

> [!NOTE]
> Analyse de Xamarin.iOS ne s’exécute sur votre configuration actuellement sélectionnée. Il est vivement recommandé d’exécuter l’outil de débogage **et** configurations release.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001 : DisabledLinkerRule

- **Problème :** l’éditeur de liens est désactivé sur l’appareil pour le mode de débogage.
- **Correctif :** vous essayez d’exécuter votre code avec l’éditeur de liens pour éviter tout risque de surprises.
Pour configurer, accédez au projet > Build iOS > comportement de l’éditeur de liens.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002 : TestCloudAgentReleaseRule

- **Problème :** builds application initialiser l’agent de Test Cloud seront rejetées par Apple lors de l’envoi, car ils utilisent les API privée.
- **Correctif :** ajouter ou corriger #if nécessaires et définit dans le code.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003 : IPADebugBuildsRule

- **Problème :** configuration de débogage qui utilise des clés de signature de développeur ne doit pas générer une extension IPA qu’il est nécessaire uniquement pour la distribution, qui utilise à présent l’Assistant Publication.
- **Correctif :** désactivez la génération IPA dans les Options de projet pour la configuration Debug.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004 : Missing64BitSupportRule

- **Problème :** l’architecture prise en charge pour « version | appareil » n’est pas compatible, manquant ARM64 64 bits. Il s’agit d’un problème comme Apple n’accepte pas les applications iOS uniquement 32 bits dans l’App Store.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS générer et modifier les architectures prises en charge, par conséquent, il a ARM64.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005 : Float32Rule

- **Problème :** ne pas à l’aide de l’option float32 (--AOA-options-O = = float32) conduit à des performances pas de coût, en particulier sur mobile, où double mathématique de précision est sensiblement plus lente. Notez que .NET utilise double précision en interne, même pour float, par conséquent, l’activation de cette option affecte la précision et, éventuellement, de compatibilité.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS générer et désactivez la « Effectuer toutes les opérations de virgule flottante de 32 bits en tant que valeur float 64 bits ».

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006 : HttpClientAvoidManaged

- **Problème :** nous recommandons d’utiliser le Gestionnaire de HttpClient natif au lieu de celui pour de meilleures performances, plus petite taille de l’exécutable, géré et facilement prendre en charge les standards plus récents.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS générer et modifier l’implémentation HttpClient NSUrlSession (iOS 7 +) ou CFNetwork pour prendre en charge la version précédente d’iOS 7.
