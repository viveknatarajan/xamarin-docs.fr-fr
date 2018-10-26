---
title: ListView et le cycle de vie d’activité
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b2328759b3158920bc8683ec14c2aebefd7a04ae
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117718"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView et le cycle de vie d’activité

Les activités passent par certains États en tant que l’exécution de votre application, telles que le démarrage, en cours d’exécution, en pause et en cours d’arrêt. Pour plus d’informations et des recommandations spécifiques sur la gestion des transitions d’état, consultez le [didacticiel du cycle de vie de l’activité](~/android/app-fundamentals/activity-lifecycle/index.md).
Il est important de comprendre le cycle de vie des activités et place votre `ListView` code dans les emplacements corrects.

Tous les exemples dans ce document effectuer « les tâches d’installation » dans l’activité `OnCreate` (méthode) et (si nécessaire) effectuer « désactivation » dans `OnDestroy`. Les exemples utilisent généralement petits jeux de données qui ne changent pas, donc il est inutile de nouveau le chargement des données plus fréquemment.

Toutefois, si vos données changent fréquemment ou utilise une grande quantité de mémoire il peut être approprié d’utiliser les méthodes de cycle de vie différent pour remplir et actualiser votre `ListView`. Par exemple, si les données sous-jacentes changent constamment (ou peuvent être affectées par les mises à jour sur les autres activités) puis de la création de l’adaptateur dans `OnStart` ou `OnResume` garantit les données les plus récentes s’affiche chaque fois que l’activité s’affiche.

Si l’adaptateur utilise des ressources telles que la mémoire ou un curseur managé, n’oubliez pas de libérer ces ressources dans la méthode complémentaire à où elles ont été instanciées (par ex. les objets créés dans `OnStart` peuvent être supprimées dans `OnStop`).


## <a name="configuration-changes"></a>Modifications de configuration

Il est important de se rappeler que la configuration change &ndash; particulièrement écran visibilité de rotation et de clavier &ndash; peut provoquer l’activité actuelle à être détruit et recréé (sauf si vous spécifiez dans le cas contraire à l’aide de la `ConfigurationChanges` attribut). Cela signifie que dans des conditions normales, un dispositif rotatif entraînera un `ListView` et `Adapter` être recréée et (sauf si vous avez écrit du code `OnPause` et `OnResume`) la sélection de ligne et de position de défilement États seront perdues.

L’attribut suivant empêcherait une activité d’être détruit et recréé à la suite de modifications de configuration :

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

L’activité doit remplacer puis `OnConfigurationChanged` pour répondre à ces modifications en conséquence. Pour plus d’informations sur la façon de gérer les modifications de configuration, consultez la documentation.

