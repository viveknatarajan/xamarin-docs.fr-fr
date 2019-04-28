---
title: Puis-je utiliser une version antérieure de Xcode ou Xamarin.iOS
description: Ce guide décrit les problèmes liés à l’aide des versions antérieures de Xamarin.iOS ou Xcode (à la version stable actuelle).
ms.prod: xamarin
ms.assetid: 27CF7EB7-9251-435F-BEA5-F20F8DD7DC17
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 04/16/2019
ms.openlocfilehash: 7cbc14e0a912fe9c55ff672796e839a8dcdfd9b5
ms.sourcegitcommit: 864f47c4f79fa588b65ff7f721367311ff2e8f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "64347115"
---
# <a name="can-i-use-an-older-version-of-xcode-or-xamarinios"></a>Puis-je utiliser une version antérieure de Xcode ou Xamarin.iOS ?

Documentation de Xamarin suppose l’utilisation de la dernière Xamarin.iOS et Xcode, ce qui est recommandé. Toutefois, certains clients préfèrent utiliser les anciennes Xamarin.iOS et/ou Xcode et souhaitez plus d’informations sur les conséquences.

Les notes de publication contiennent l’avertissement suivant :

> [!WARNING]
> **À l’aide d’une ancienne version de Xcode**
>
> À l’aide d’une ancienne version de Xcode (que celle indiquée dans l’exemple ci-dessus [exigences](https://docs.microsoft.com/xamarin/ios/release-notes/12/12.8#requirements)) est souvent possible, mais certaines fonctionnalités ne peuvent pas être disponibles. Également certaines limitations peuvent nécessiter des solutions de contournement, par exemple :
>
> - Le bureau d’enregistrement statique nécessite des fichiers d’en-têtes de Xcode pour créer des applications, ce qui conduit à [ `MT0091` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT0091) ou [ `MT4109` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT4109) erreurs si l’API sont manquants. Dans la plupart des cas l’activation de l’éditeur de liens managé aidera (en supprimant l’API).
> - Builds Bitcode (pour tvOS et watchOS) peuvent échouer soumission à l’App Store, sauf si une chaîne d’outils Xcode 9.0 + est utilisé.

## <a name="further-information"></a>Informations supplémentaires

Microsoft recommande fortement d’utiliser la dernière Xcode et version de Xamarin.iOS la plus récente lors du développement et l’envoi des applications. Apple requiert l’utilisation de la plus récente Xcode lors de l’envoi des applications.

Notez qu’à l’aide de la dernière Xcode n’empêche pas votre application à partir de ciblage des anciennes versions d’iOS. Les versions d’iOS que vous prenez en charge est basé sur votre **Info.plist** entrée et les API de votre application utilise.

Il est possible d’installer plusieurs versions de Xcode côte-à-côte, avec des noms différents, tels que **Xcode101.app** et **Xcode102.app**. Si vous utilisez plusieurs versions, veillez à définir le Xcode active dans [Visual Studio pour les paramètres Mac](~/ios/troubleshooting/questions/ios-sdk.md) et avec le [ `xcode-select` ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_) [outil de ligne de commande](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_).

Toutefois, rares circonstances peuvent nécessiter l’utilisation de composants plus anciens. Cette documentation décrit les difficultés générales, vous risquez de rencontrer quand à l’aide de versions antérieures à la plus récente.

Chaque version d’Apple est unique, cependant, et vous êtes susceptible de rencontrer autres pièges ne pas décrites ici.

Ces défis sont parfois non négligeable à résoudre, ainsi chaque fois que possible se borner à la configuration prise en charge de Xcode plus récente et dernière Xamarin.iOS.

## <a name="use-of-an-old-xamarinios-with-an-old-xcode"></a>Utilisation d’un ancien Xamarin.iOS avec un ancien Xcode

Ne pas la mise à jour Xamarin.iOS et Xcode est possible, au moins d’une certaine quantité de temps. La limite est que, à un moment donné, Apple nécessite une version minimale de Xcode pour soumettre vos applications. À ce stade vous devez mettre à jour tous vos composants (macOS, Xcode et Xamarin.iOS) pour les dernières versions (ou la nouvelle version minimale de Xcode requise par Apple et la version de Xamarin.iOS correspondante).

Il est généralement plus facile mettre à jour progressivement et de suivre les modifications mineures. Pour les grands projets où les mises à jour peuvent être plus difficile à suivre, le jeu de travail connus en gardant peut être un bon compromis.

## <a name="use-of-new-xamarinios-with-older-xcode"></a>Utilisez de nouveau Xamarin.iOS avec Xcode antérieure

Xamarin.iOS en général prend en charge que les versions plus anciennes Xcode chaque fois que possible. Quelques défis potentiels :

- La plus récente Xamarin.iOS peuvent prendre en charge certaines fonctionnalités et API sont absents dans Xcode sélectionné. 
- Le **bureau d’enregistrement statique** nécessite des fichiers d’en-têtes de Xcode pour créer des applications, ce qui conduit à [ `MT0091` ](~/ios/troubleshooting/mtouch-errors.md#MT0091) ou [ `MT4109` ](~/ios/troubleshooting/mtouch-errors.md#MT4109) erreurs si l’API sont manquants.
  - Dans la plupart des cas, l’activation de l’éditeur de liens managé aidera (en supprimant les liaisons gérés pour la nouvelle API) non utilisées.
- Builds Bitcode (pour tvOS et watchOS) peuvent échouer soumission à l’App Store, sauf si une chaîne d’outils Xcode 9.0 + est utilisé.

## <a name="use-of-new-xcode-with-older-xamarinios"></a>Utilisez de nouveau Xcode avec Xamarin.iOS plus anciens

Ce cas d’usage est considérablement plus difficile, que Xamarin.iOS ne peut pas prédire les besoins de nouveau Xcode. Mises à jour de macOS peuvent également introduire des problèmes, et sans les correctifs de compatibilité de nombreux éléments de Xamarin.iOS pourrait être affectées. 

Il existe un nombre de zones potentielles où choses peuvent mal se passer, notamment :

- Incompatibilités avec `mlaunch`:
  - Prise en charge du simulateur peut ne pas fonctionne correctement (ou tout)
  - Prise en charge de l’appareil peut ne pas fonctionne correctement (ou tout)
- Prise en charge inconnu pour `mtouch` 
  - Aucune prise en charge de nouvelles infrastructures
  - Aucune prise en charge de nouveaux droits
  - Aucune prise en charge des outils nouveaux ou mis à jour
    - Cela peut affecter également de signature de code

### <a name="new-appstore-submission-rules"></a>Nouvelles règles de soumission App Store

Apple réserve le droit aux mises à jour les règles de soumissions App Store à tout moment. Ces modifications de règle sont annoncées uniquement parfois à l’avance. Certaines de ces modifications nécessitent des modifications pour prendre en charge, ce qui nécessiteraient un composant de Xamarin.iOS mis à jour des outils.

En plus des modifications de règle, Apple souvent ajoute des validations supplémentaires pour les applications soumises ou renforce existants. Certaines d'entre elles nécessitent des modifications dans nos outils (par exemple une nouvelle sur liste noire des symboles). Beaucoup d'entre elles sont tout d’abord rencontrés par les clients soumis, car il n’existe aucune annonce (ou liste) des règles.

## <a name="summary"></a>Récapitulatif

Si possible, jouez la sécurité en instructions d’Apple suivant et en le développement sur et en soumettant avec la dernière Xcode publié sur l’App Store.

À son tour, utilisez la dernière Xamarin.iOS publié. Il effectue le suivi des derniers correctifs qui peuvent affecter les applications soumises et respecter les dernières modifications de règle.

Ce n’est pas possible, envisagez d’utiliser une mise en correspondance version Xcode antérieure de Xamarin.iOS. Cela peut fonctionner pour une heure, mais à un moment donné, Apple insistent sur des outils plus récent donc de planifier en conséquence.
