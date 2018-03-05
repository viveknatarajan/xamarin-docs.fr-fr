---
title: SiriKit
description: "Cet article explique comment utiliser SiriKit dans une application Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri sur un appareil iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c4fdf61b35ca28af82e3890242d54a75e50d2f82
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="sirikit"></a>SiriKit

_Cet article explique comment utiliser SiriKit dans une application Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri sur un appareil iOS._

Nouveau pour iOS 10, SiriKit permet à une application iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application de cartes sur un appareil iOS à l’aide des Extensions d’application et les nouveaux **intentions** et **intentions UI** infrastructures.

Siri fonctionne avec le concept de **domaines**, groupes de connaître les actions pour les tâches associées. Chaque interaction avec Siri d’une application doit appartenir à l’un de ses domaines de service connu comme suit :

- Audio ou vidéo.
- Un cas de réservation.
- La gestion de sommeil.
- Gestion des messages.
- Recherche des photos.
- Envoyer ou recevoir des paiements.

Lorsque l’utilisateur effectue une demande de Siri impliquant un des services d’une Extension application, SiriKit envoie l’extension un **intention** objet qui décrit la demande de l’utilisateur, ainsi que toutes les données de prise en charge. l’Extension d’application, puis génère les **réponse** de l’objet pour la donnée **intention**, détaillant la façon dont l’extension peut gérer la demande.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[Présentation des concepts de SiriKit](~/ios/platform/sirikit/understanding-sirikit.md)

Cet article aborde les concepts clés qui seront nécessaires pour travailler avec SiriKit dans une application Xamarin.iOS. Elle couvre les nouveaux modes et Points d’Extension intentions de l’interface utilisateur et leur fonctionnement avec App et le vocabulaire de l’utilisateur pour ouvrir une application à Siri.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[Implémentation de SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

Cet article décrit les étapes requises pour implémenter la prise en charge SiriKit dans un Xamarin.iOS les applications. Le développeur doit lire le guide de présentation des Concepts de SiriKit ci-dessus avant d’ajouter la prise en charge SiriKit à une application, en tant que clé concepts sont traités et qui subordonnée nécessaire pour la réussite de l’implémentation.





## <a name="related-links"></a>Liens associés

- [Exemple de ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Guide de programmation SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Référence de Framework intentions](https://developer.apple.com/reference/intents)
- [Référence de Framework intentions l’interface utilisateur](https://developer.apple.com/reference/intentsui)
