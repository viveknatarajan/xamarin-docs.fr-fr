---
title: ListView et le cycle de vie d’activité
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 6e15fb8796ae6a616c5eae44059caae3d9478aef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764218"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView et le cycle de vie d’activité

Activités passent par certains États en tant que l’exécution de votre application, telles que le démarrage, en cours d’exécution, en pause et en cours d’arrêt. Pour plus d’informations et des recommandations spécifiques sur la gestion des transitions d’état, consultez le [activité du cycle de vie didacticiel](~/android/app-fundamentals/activity-lifecycle/index.md).
Il est important de comprendre le cycle de vie des activités et place votre `ListView` code dans les emplacements corrects.

Tous les exemples dans ce document effectuer « les tâches d’installation » dans l’activité `OnCreate` (méthode) et (si nécessaire) effectuer 'démontage' dans `OnDestroy`. Les exemples utilisent généralement les petits jeux de données qui ne changent pas, donc de nouveau le chargement des données plus fréquemment n’est pas nécessaire.

Toutefois, si vos données changent fréquemment ou utilise une grande quantité de mémoire qu’il peut être approprié d’utiliser les méthodes de cycle de vie différent pour remplir et actualiser votre `ListView`. Par exemple, si les données sous-jacentes constamment (ou peuvent être affectées par les mises à jour sur d’autres activités) puis de la création de l’adaptateur dans `OnStart` ou `OnResume` garantit les données les plus récentes s’affiche chaque fois que l’activité s’affiche.

Si l’adaptateur utilise des ressources telles que la mémoire ou un curseur managé, n’oubliez pas de libérer ces ressources dans la méthode complémentaire à où elles ont été instanciées (par exemple). les objets créés dans `OnStart` peuvent être supprimées dans `OnStop`).


## <a name="configuration-changes"></a>Modifications de configuration

Il est important de se rappeler que la configuration change &ndash; écran en particulier de visibilité de rotation et de clavier &ndash; peut entraîner l’activité actuelle à être détruits et recréés (sauf si vous spécifiez dans le cas contraire à l’aide de la `ConfigurationChanges` (attribut). Cela signifie que dans des conditions normales, faire pivoter un périphérique entraîne une `ListView` et `Adapter` être recréé et (sauf si vous avez écrit le code dans `OnPause` et `OnResume`) la sélection de ligne et de position de défilement États seront perdues.

L’attribut suivant empêche une activité d’être détruit et recréé à la suite de modifications de configuration :

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

L’activité doit remplacer puis `OnConfigurationChanged` répondre à ces modifications de façon appropriée. Pour plus d’informations sur la gestion des modifications de configuration, consultez la documentation.

