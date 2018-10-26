---
title: SiriKit dans Xamarin.iOS
description: Cet article explique comment utiliser SiriKit dans une application Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri sur un appareil iOS.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f7cbb3f7d9e448947ec8163a8660616910e750f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117523"
---
# <a name="sirikit-in-xamarinios"></a>SiriKit dans Xamarin.iOS

_Cet article explique comment utiliser SiriKit dans une application Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri sur un appareil iOS._

Nouveau à iOS 10, SiriKit permet à une application iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application Maps sur un appareil iOS à l’aide des Extensions d’application et le nouveau **intentions** et **Intents UI** frameworks.

Siri fonctionne avec le concept de **domaines**, groupes de connaître les actions pour les tâches associées. Chaque interaction d’une application avec Siri doit appartenir à l’un de ses domaines de service connues comme suit :

- Audio ou vidéo.
- Un tour de réservation.
- La gestion de sommeil.
- La messagerie.
- Recherche des photos.
- Envoyer ou recevoir des paiements.

Lorsque l’utilisateur effectue une requête de Siri impliquant un des services d’une Extension application, SiriKit envoie l’extension un **intention** objet qui décrit la demande de l’utilisateur, ainsi que les données pertinentes. l’Extension d’application génère ensuite approprié **réponse** de l’objet pour la donnée **intention**, détaillant la façon dont l’extension puisse traiter la demande.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[Présentation des concepts de SiriKit](~/ios/platform/sirikit/understanding-sirikit.md)

Cet article aborde les concepts fondamentaux qui seront nécessaires pour l’utilisation de SiriKit dans une application Xamarin.iOS. Il couvre la nouvelle intentions et Points d’Extension Intents UI et leur fonctionnement avec l’application et le vocabulaire de l’utilisateur pour ouvrir une application à Siri.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[Implémentation de SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

Cet article décrit les étapes requises pour implémenter la prise en charge de SiriKit dans les applications Xamarin.iOS. Le développeur doit lire le guide de présentation des Concepts de SiriKit ci-dessus avant d’ajouter la prise en charge de SiriKit à une application, en tant que clé concepts couvertes qui subordonnée nécessaire pour une implémentation réussie.





## <a name="related-links"></a>Liens associés

- [Exemple de ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Guide de programmation de SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Référence de Framework intentions](https://developer.apple.com/reference/intents)
- [Référence de Framework d’interface utilisateur Intents](https://developer.apple.com/reference/intentsui)
