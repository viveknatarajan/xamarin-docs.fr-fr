---
title: tvOS ressources et stockage des données dans Xamarin
description: Cet article décrit l’utilisation des ressources et de stockage des données persistantes dans une application tvOS générée avec Xamarin. Il traite des ressources de stockage et à la demande de données iCloud.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 8f8ecc115738fb97f71b4ea6b2cdcc5c2714372d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108182"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>tvOS ressources et stockage des données dans Xamarin

_Cet article aborde l’utilisation des ressources et de stockage des données persistantes dans une application Xamarin.tvOS._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>Limitations des ressources de tvOS

Contrairement aux appareils iOS, la TV Apple fournit un stockage persistant, local très limité pour les applications de tvOS ou de données. Pour les éléments de très petite taille (telles que les préférences de l’utilisateur), votre application tvOS a toujours accès à `NSUserDefaults` avec un [limite de 500 Ko de données](https://forums.developer.apple.com/message/50696#50696). Toutefois, si votre application Xamarin.tvOS doit conserver de grandes quantités d’informations, elle doit stocker et récupérer ces données à partir de [iCloud](#iCloud-Data-Storage).

En outre, tvOS limite la taille d’une application Apple TV à 200 Mo. Si votre application nécessite des ressources au-delà de cette taille, ils doivent être empaquetées et chargées à l’aide [On-Demand ressources](#On-Demand-Resources) (jusqu'à 2 Go). Étant donné ces limitations, il est essentiel que vous correctement fois le téléchargement de ressources supplémentaires pour fournir la meilleure expérience pour les utilisateurs de votre application. Pour plus d’informations, consultez le site d’Apple [Guide de ressources à la demande](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Téléchargements non persistant

Chaque application tvOS est fournie à un répertoire de cache temporaire ses ressources supplémentaires et les composants sont téléchargés vers. Ce répertoire est conservé tant que l’application est en cours d’exécution. Toutefois, comme l’Apple TV doit libérer de l’espace pour d’autres applications ou l’utilisation du système, ce cache peut être supprimé.

Par conséquent, votre application ne peut pas s’appuient sur le contenu précédemment téléchargé soient disponibles la prochaine fois qu’elle est lancée. Votre application Xamarin.tvOS doit toujours vérifier l’existence des ressources requises et les télécharger en fonction des besoins.

> [!IMPORTANT]
> Pendant que vous avez la possibilité de télécharger d’autres composants et des ressources en fonction des besoins, Apple vous avertit par rapport à utiliser l’intégralité de l’espace dans le cache de votre application, car elle peut entraîner des résultats imprévisibles.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Gestion des ressources

Comme indiqué ci-dessus, en raison du nombre limité, le stockage non persistant des informations disponibles pour les applications tvOS, ces restrictions nécessitent une planification précise créer une expérience utilisateur satisfaisante pour votre application Xamarin.tvOS.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>Stockage de données iCloud

Stockage sur Apple TV étant limité, est non seulement il très limité stockage persistant, local, votre application ne dispose d’aucune garantie que toutes les informations qu’il a précédemment téléchargé sera disponibles lors de la prochaine fois qu’elle est exécutée.

Par conséquent, votre application Xamarin.tvOS doit stocker toutes les données utilisateur dans un Store de données iCloud. Apple fournit deux options de stockage de données sur iCloud pour vos applications tvOS :

- **iCloud stockage clé-valeur (KVS)** : pour les petites parties d’informations (inférieur à 1 Mo) que votre application peut nécessiter (tels que les préférences de l’utilisateur), vous pouvez utiliser KVS stockage iCloud. iCloud KVS les données sont automatiquement synchronisées avec le cloud et tous les appareils de l’utilisateur exécutant la même application. Pour plus d’informations, consultez le [stockage clé-valeur](~/ios/data-cloud/introduction-to-icloud.md) section de notre [présentation d’iCloud](~/ios/data-cloud/introduction-to-icloud.md) document ou Apple [conception pour les données de clé-valeur dans iCloud](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) documentation.
- **CloudKit** : pour le stockage de pièces plus grandes d’informations (supérieures à 1 Mo), utilisez le Framework de CloudKit d’Apple. Contrairement à iCloud KVS stockage, les données de CloudKit peuvent être partagées entre tous les utilisateurs de votre application (comme étant privé à un seul utilisateur). Plus d’informations, reportez-vous à notre [présentation de CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) documentation ou Apple [démarrage rapide de CloudKit](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

> [!IMPORTANT]
> Apple [fournit des outils](https://developer.apple.com/support/allowing-users-to-manage-data/) pour aider les développeurs à gérer correctement le Règlement général sur la protection des données (RGPD) de l’Union européenne.

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>Ressources à la demande

Ressources de la demande fournissent contenu de l’application et les ressources (distincts à partir de l’offre groupée d’applications), qui sont hébergées sur l’App Store et téléchargés comme requis par votre application. Jusqu'à 2 Go de données supplémentaires peuvent être pris en charge l’utilisation des ressources de la demande. Elles permettent le téléchargement de l’application initiale inférieure (tvOS applications sont limitées à un maximum de 200 Mo), tout en fournissant un riches actifs en fonction des besoins.

Lorsqu’une application tvOS demande les ressources à la demande, le système gère automatiquement le téléchargement et le stockage de ce contenu au répertoire de cache de l’application. Votre application doit attendre pour ce contenu à être téléchargé et mis à disposition avant de pouvoir continuer.

Ces ressources peuvent continuer à être mis en cache sur l’Apple TV tout au long de plusieurs lancements de votre application, par conséquent, en accélérant cycle de lancement. Toutefois, votre application ne peut pas compter sur n’importe quel contenu précédemment téléchargé soient disponibles la prochaine fois qu’elle est lancée. Consultez le [télécharge Non persistant](#Non-Persistent-Downloads) section ci-dessus pour plus d’informations.

Vous utilisez Xcode pour créer des ensembles de contenu associé (par exemple, toutes les ressources pour le niveau de jeu 2) associés à une balise de ressource donné. Votre application demande plus tard les ressources à la demande en spécifiant cette balise de ressource. Votre application doit présenter une interface utilisateur à l’utilisateur indiquant que le contenu est en cours de téléchargement. Pour plus d’informations, consultez le site d’Apple [Guide de ressources à la demande](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Être vigilant pour trouver le bon équilibre entre le nombre de fois où que l’application a pour télécharger les ressources à la demande et la taille des téléchargements individuels. Utilisateur peut devenir frustré avec votre application si le jeu est interrompu en permanence pour télécharger du contenu nouveau ou si un seul téléchargement prend trop de temps.




<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté les limitations de stockage taille, les ressources et les données placées sur une application Xamarin.tvOS par le système de tvOS. Il a présenté les options pour contourner ces limitations et les suggestions pour créer une expérience utilisateur satisfaisante pour votre application.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
