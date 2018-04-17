---
title: Stockage des données et ressources
description: Cet article décrit l’utilisation des ressources et du stockage des données persistantes dans une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8103dcc1c2e1e8c2375a86d51580b1ea3fcce432
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="resources-and-data-storage"></a>Stockage des données et ressources

_Cet article décrit l’utilisation des ressources et du stockage des données persistantes dans une application Xamarin.tvOS._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS Limitations des ressources

Contrairement aux appareils iOS, le nouveau Apple TV fournit très limitée un stockage local pour les applications de tvOS ou de données. Pour les éléments de très faible (par exemple, les préférences de l’utilisateur), votre application tvOS a toujours accès au `NSUserDefaults` avec un [limite de 500 Ko de données](https://forums.developer.apple.com/message/50696#50696). Toutefois, si votre application Xamarin.tvOS doit conserver de grandes quantités d’informations, il doit stocker et récupérer des données à partir de [iCloud](#iCloud-Data-Storage).

En outre, tvOS limite la taille d’une application Apple TV à 200 Mo. Si votre application nécessite des ressources au-delà de cette taille, ils doivent être empaquetés et chargés à l’aide [à toutes les demandes](#On-Demand-Resources) (jusqu'à 2 Go). Étant donné ces limitations, il est essentiel que vous correctement fois le téléchargement de ressources supplémentaires pour fournir une meilleure expérience pour les utilisateurs de votre application. Pour plus d’informations, consultez le site d’Apple [Guide de ressources à la demande](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Téléchargements non persistant

Chaque application tvOS est fournie à un répertoire de cache temporaire des ses ressources supplémentaires sont téléchargés vers. Ce répertoire est conservé tant que l’application est en cours d’exécution. Toutefois, comme l’Apple TV doit libérer de l’espace pour d’autres applications ou l’utilisation du système, ce cache peut être supprimé.

Par conséquent, votre application ne peut pas s’appuient sur le contenu précédemment téléchargé soient disponibles la prochaine fois qu’elle est lancée. Votre application Xamarin.tvOS doit toujours vérifier l’existence des ressources requises et les télécharger en fonction des besoins.

> [!IMPORTANT]
> Pendant que vous avez la possibilité de télécharger d’autres ressources et les ressources en fonction des besoins, Apple vous avertit par rapport à l’utilisation de tout l’espace dans le cache de votre application, car elle peut entraîner des résultats imprévisibles.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Gestion des ressources

Comme indiqué précédemment, en raison du nombre limité, le stockage non persistant d’informations disponibles pour les applications de tvOS, ces restrictions nécessitent une planification rigoureuse afin de créer une expérience utilisateur approprié pour votre application Xamarin.tvOS.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud de stockage de données

Stockage sur l’Apple TV étant limité, non seulement est il le stockage persistant, local très limité, votre application n’a aucune garantie que toutes les informations il précédemment téléchargée seront disponibles lors de la prochaine exécution.

Par conséquent, votre application Xamarin.tvOS doit stocker des données utilisateur dans un magasin de données iCloud. Apple offre deux options de stockage de données basées sur iCloud pour vos applications tvOS :

- **iCloud stockage clé-valeur (KVS)** - pour les petits éléments d’informations (inférieur à 1 Mo) que votre application peut nécessiter (tels que les préférences de l’utilisateur), vous pouvez utiliser iCloud KVS stockage. iCloud KVS données sont automatiquement synchronisées avec le cloud et tous les périphériques de l’utilisateur exécutant la même application. Pour plus d’informations, veuillez consulter le [clé-valeur stockage](~/ios/data-cloud/introduction-to-icloud.md) section de notre [Introduction sur iCloud](~/ios/data-cloud/introduction-to-icloud.md) document ou Apple [conception pour les données de la valeur de clé dans iCloud](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) documentation.
- **CloudKit** - pour le stockage de plus d’éléments d’informations (supérieures à 1 Mo), utilisez CloudKit Framework d’Apple. Contrairement aux iCloud KVS stockage, les données de CloudKit peuvent être partagées entre tous les utilisateurs de votre application (comme étant privé à un seul utilisateur). Plus d’informations, consultez notre [présentation CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) documentation ou Apple [CloudKit Quick Start](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

> [!IMPORTANT]
> Apple [fournit des outils](https://developer.apple.com/support/allowing-users-to-manage-data/) pour aider les développeurs de gérer correctement général données Protection règlement (PIBR l’Union européenne).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>Ressources à la demande

Les ressources à la demande fournissent le contenu de l’application et les ressources (distincts à partir de l’offre groupée), qui sont hébergés sur l’App Store et téléchargés sous la forme requise par votre application. Jusqu'à 2 Go de données peut être pris en charge l’utilisation des ressources de la demande. Ils permettent le téléchargement de l’application initiale plus petites (tvOS applications sont limitées à un maximum de 200 Mo), tout en fournissant un riches actifs en fonction des besoins.

Lorsqu’une application tvOS demande les ressources à la demande, le système gère automatiquement le téléchargement et le stockage de ce contenu au répertoire de cache de l’application. Votre application doit attendre pour ce contenu à télécharger et mises à disposition avant de pouvoir continuer.

Ces ressources peuvent continuer à être mis en cache sur l’Apple TV tout au long de plusieurs lance de votre application, par conséquent, en accélérant cycle de lancement. Toutefois, votre application ne peut pas compter sur n’importe quel contenu précédemment téléchargé soient disponibles la prochaine fois qu’elle est lancée. Consultez le [télécharge Non persistant](#Non-Persistent-Downloads) section ci-dessus pour plus d’informations.

Xcode vous permet de créer des lots de contenu connexe (par exemple, tous les composants de niveau de jeu 2) associés à une balise de ressource donné. Votre application demande plus tard les ressources à la demande en spécifiant cette balise de ressource. Votre application doit présenter une interface utilisateur à l’utilisateur indiquant que le contenu est en cours de téléchargement. Pour plus d’informations, consultez le site d’Apple [Guide de ressources à la demande](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Soyez vigilant pour parvenir au juste équilibre entre le nombre de fois où que l’application doit télécharger les ressources à la demande et de la taille des téléchargements individuels. Utilisateur peut devenir déçu avec votre application si le jeu est interrompu en permanence pour télécharger le contenu nouveau ou si un seul téléchargement prend trop de temps.




<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert les limitations de stockage de taille, les ressources et les données placées sur une application Xamarin.tvOS par le système tvOS. Il a présenté les options permettant de contourner ces limitations et les suggestions pour créer une expérience utilisateur satisfaisante pour votre application.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
