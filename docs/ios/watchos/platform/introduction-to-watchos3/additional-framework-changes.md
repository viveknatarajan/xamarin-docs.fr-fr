---
title: Modifications des infrastructures supplémentaires watchOS 3
description: Ce document décrit les différentes modifications framework introduites avec watchOS 3 et comment les utiliser dans Xamarin. Données de base, Core Motion, Foundation, HealthKit, HomeKit, PassKit et UIKit sont présentés.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 745c39dab1f73870ce036791434ed9a0b05d681b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122619"
---
# <a name="additional-watchos-3-frameworks-changes"></a>Modifications des infrastructures supplémentaires watchOS 3

_Cet article traite des modifications mineures supplémentaires ou des améliorations apportées aux infrastructures existantes pour watchOS 3._

En plus des modifications majeures à iOS, Apple a apporté des modifications et améliorations apportées à plusieurs infrastructures existantes dans watchOS 3.


## <a name="core-data"></a>Données de base

Les améliorations suivantes ont apportées à l’infrastructure de données de base pour la surveillance du système d’exploitation 3 :

- Racine [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) prend en charge les objets simultanés de provoquer une erreur et de récupération sans sérialisation.
- Le [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) classe gère un pool de magasins de données SQLite.
- Le [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objets avec des banques de données SQLite dans la prise en charge du Mode du Journal WAL la nouvelle génération de requête de fonctionnalités où contextes gérés de l’objet (MOC) peuvent être épinglés aux versions de base de données spécifique pour l’extraction des futures et transactions défaillante.
- À l’aide de la vue `NSPersistenceContainer` pour référencer le `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) et d’autres ressources de configuration des données de base.
- Plusieurs nouvelles méthodes pratiques ont été ajoutés à `NSManagedObject` rend plus facile à effectuer des extractions et créer les sous-classes.

Pour plus d’informations, consultez le site d’Apple [Core données Framework référence](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Mouvement de Core

Les améliorations suivantes ont apportées à l’infrastructure Core mouvement pour la surveillance du système d’exploitation 3 :

- Le nouvel événement de mouvement de l’appareil utilise l’accéléromètre et le gyroscope pour fournir des mises à jour de mouvement et l’orientation. l’application peut s’inscrire pour cette mise à jour (aux tarifs de jusqu'à 100Hz).
- Le nouvel événement PODOMÈTRE permet rapides et des notifications en temps réel lorsque l’utilisateur s’arrête et reprend l’exécution. Utilisez le [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) pour enregistrer les événements de PODOMÈTRE au premier plan ou d’arrière-plan.


## <a name="foundation"></a>Foundation

Les améliorations suivantes ont apportées à l’infrastructure Foundation pour espion 3 du système d’exploitation :

- Utilisez la nouvelle [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) classe pour effectuer les calculs d’intervalle de date et heure telles que des durées, pour comparer les intervalles et de test pour les intersections d’intervalle.
- Plusieurs nouvelles propriétés ont été ajoutées à la [NSLocal](https://developer.apple.com/reference/foundation/nslocale) classe pour acquérir les informations locales et les formats d’affichage disponible.
- Utilisez la nouvelle [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) classe pour convertir entre différentes unités de mesure (UDM) ou effectuer des calculs sur les valeurs d’unités différentes.
- Utilisez la nouvelle [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) classe à mettre en forme des mesures localisées pour l’affichage à l’utilisateur final.
- Utilisez la nouvelle [NSUnit](https://developer.apple.com/reference/foundation/nsunit) et [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) classes pour représenter des unités spécifiques.


## <a name="healthkit"></a>HealthKit

Les améliorations suivantes ont apportées au framework HealthKit pour espion 3 du système d’exploitation :

- Utilisez la nouvelle [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) classe pour spécifier le `ActivityType` et `LocationType` d’un exercice.
- La nouvelle [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) et `WheelchairUse` méthode de la [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) classe ont été ajoutées pour travailler avec fauteuil liés des données d’intégrité.
- Nouvelles clés de métadonnées ont été ajoutées pour les types de météo (tel que `HKWeatherConditionClear` et `HKWeatherConditionCloudy`) et les types d’entraînement (tel que `HKWorkoutActivityTypeFlexibility` et `HKWorkoutActivityTypeWheelchairRunPace`) ont été ajoutés.


## <a name="homekit"></a>HomeKit

Les améliorations suivantes ont apportées au framework HomeKit pour espion 3 du système d’exploitation :

- Ajouté la possibilité d’afficher et interagir avec HomeKit connecté IP caméras.
- Ajout de plusieurs nouveaux services et caractéristiques.
- Ajouter plus de contexte et de configuration des accessoires de services principaux et de services de liaison.


## <a name="passkit"></a>PassKit

Les améliorations suivantes ont apportées au framework PassKit pour espion 3 du système d’exploitation :

- Développe l’infrastructure pour prendre en charge les paiements sécurisés, dans l’application sur l’Apple Watch de biens matériels et services.
- Les classes suivantes sont désormais disponibles : [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) et [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Les améliorations suivantes ont apportées au framework UIKit pour espion 3 du système d’exploitation :

- Pour prendre en charge de Type dynamique dans les étiquettes, utilisent les nouveaux champs de texte et des zones de texte `PreferredFontForTextStyle` méthode de la `UIFont` classe.
- Le `ColorWithDisplayP3` méthode a été ajoutée pour prendre en charge la large couleur.


## <a name="related-links"></a>Liens associés

- [Mise en route (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [Quelles sont les nouveautés dans watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
