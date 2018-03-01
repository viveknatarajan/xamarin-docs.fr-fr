---
title: "Règles d’analyse de Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 7cf627f369b666bb54d0f512dc1361d2a685a057
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-analysis-rules"></a>Règles d’analyse de Xamarin.iOS


## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001 : DisabledLinkerRule

- **Problème :** l’éditeur de liens est désactivé sur l’appareil pour le mode de débogage.
- **Correctif :** vous essayez d’exécuter votre code avec l’éditeur de liens pour éviter tout risque de surprises.
Pour configurer, accédez au projet > Build iOS > comportement de l’éditeur de liens.

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002 : TestCloudAgentReleaseRule

- **Problème :** builds application initialiser l’agent de Test Cloud seront rejetées par Apple lors de l’envoi, car ils utilisent les API privée.
- **Correctif :** ajouter ou corriger #if nécessaires et définit dans le code.

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003 : IPADebugBuildsRule

- **Problème :** configuration de débogage qui utilise des clés de signature de développeur ne doit pas générer une extension IPA qu’il est nécessaire uniquement pour la distribution, qui utilise à présent l’Assistant Publication.
- **Correctif :** désactivez la génération IPA dans les Options de projet pour la configuration Debug.

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004 : Missing64BitSupportRule

- **Problème :** l’architecture prise en charge pour « version | appareil » n’est pas compatible, manquant ARM64 64 bits. Il s’agit d’un problème comme Apple n’accepte pas les applications iOS uniquement 32 bits dans l’App Store.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS générer et modifier les architectures prises en charge, par conséquent, il a ARM64.

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005 : Float32Rule

- **Problème :** ne pas à l’aide de l’option float32 (--AOA-options-O = = float32) conduit à des performances pas de coût, spécialement sur mobile, où double mathématique de précision est sensiblement plus lente. Notez que .NET utilise double précision en interne, même pour float, par conséquent, l’activation de cette option affecte la précision et, éventuellement, de compatibilité.
- **Correctif :** Double cliquez sur votre projet iOS, accédez à la Build > iOS générer et désactivez la « Effectuer toutes les opérations de virgule flottante de 32 bits en tant que valeur float 64 bits ».
