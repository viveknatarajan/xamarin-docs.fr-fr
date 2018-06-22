---
title: Forum aux Questions général
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.openlocfilehash: b81f5c09ea06acf87449448f43b6c0474a6737b0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917471"
---
# <a name="general-frequently-asked-questions"></a>Forum aux Questions général

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Comment afficher les bibliothèques prises en charge dans une bibliothèque de classes portable ?](pcl-support-libraries.md)
Ce guide répertorie des ressources et des méthodes pour déterminer si votre bibliothèque existante est pris en charge par les différentes plateformes cibles de bibliothèque PCL, ou peut être converti en un profil de bibliothèque de classes portables.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[API de réflexion de bibliothèque de classes portable](pcl-reflection.md)
Microsoft a développé une nouvelle API de réflexion pour une utilisation dans les bibliothèques de classes portables. Si vous avez un code de réflexion existant que vous souhaitez déplacer vers une bibliothèque de classes portables, il peut ne pas fonctionne.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Étude de cas de bibliothèque de classes portable : Comment résoudre des problèmes liés à System.Diagnostics.Tracing pour le package NuGet Microsoft TPL Dataflow ?](pcl-case-study.md)
Xamarin.iOS et Xamarin.Android n’implémentent pas 100 % de chaque profil PCL permettant en tant que références. Pour plus de commodité pratique dans Visual Studio pour Mac, Visual Studio et le Gestionnaire de package NuGet, les projets de Xamarin permettent l’utilisation de plusieurs profils qui ont uniquement des implémentations incomplètes. Par exemple, Xamarin.iOS, ni Xamarin.Android actuellement inclut une implémentation complète des types dans les `System.Diagnostics.Tracing` espace de noms de bibliothèque de classes portables. Vous pouvez contourner ce en basculant le projet d’application pour faire référence à l’ordinateur portable-net45 win8 + wp8 + wpa81 version de la bibliothèque de flux de données de la bibliothèque parallèle de tâches.

## <a name="nuget-packages--xamarin-components"></a>Les packages NuGet et des composants Xamarin
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Comment mettre à jour NuGet ?](nuget-update.md)
Vous trouverez les compléments, extensions et mises à jour de NuGet sous la **mises à jour** onglet dans le **Gestionnaire de Package NuGet**. La navigation détaillée pour rechercher les mises à jour dans Visual Studio pour Mac et Visual Studio est dans ce guide.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Comment mettre à niveau un package NuGet ?](nuget-package-downgrade.md)
Visual Studio pour Mac et Visual Studio ont des fonctionnalités pour sélectionner les versions antérieures des packages et de les installer automatiquement ; similaire à la mise à jour des packages fonctionne.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Erreur de packages manquants après la mise à jour de packages NuGet](nuget-packages-missing.md)
Ce problème a principalement été signalé sur les solutions d’application exemple Xamarin.Forms, mais le risque de ce problème peut se produire sur n’importe quel projet qui utilise les packages NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Unification des composants des services Google Play et de NuGet](gps-components-nuget.md)
Il utilisée à plusieurs composants des Services Google Play et les packages NuGet, mais pour simplifier les choses pour les développeurs, nous avons maintenant nos composants et unifiés NuGet packages en deux. Dans presque tous les cas, les Services Google Play doit être utilisés. La seule raison pour laquelle utiliser le package (Froyo) est que si vous ciblez activement Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Où sont stockés les composants sur ma machine ?](component-storage.md)
Chaque fois que vous installez un composant Xamarin dans un projet d’application, il est placé dans les deux emplacements répertoriés dans ce guide.


## <a name="troubleshooting"></a>Résolution des problèmes
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Où trouver les informations sur ma version et les journaux ?](version-logs.md)
Ce guide détaille où se trouvent la plupart des informations de diagnostic qui peuvent être utilisées pour résoudre les problèmes de Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Quand et comment dois-je classer un rapport de bogues ?](howto-file-bug.md)
Ce guide fournit des conseils pour le classement des rapports de bogue de haute qualité, afin que nos ingénieurs sont en mesure de déterminer la cause (et toutes les corrections éventuelles) pour un problème plus efficacement.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Pourquoi Jenkins n’est-il pas pris en charge par Xamarin ?](xamarin-jenkins.md)
Jenkins est une suite de l’élément de configuration open source ; en raison de problèmes de ce nombre qui sont directement générés par le Jenkins *lui-même* devra être classé comme problèmes contre où vous avez obtenu le code ; comme le [principal Jenkins référentiel](https://github.com/jenkinsci/jenkins), ou le référentiel pour [ Jenkins.app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Quels sont les paramètres de projet requis pour le débogueur ?](debugger-settings.md)
Dans l’ordre pour que le débogueur fonctionne comme prévu (d’atteinte de points d’arrêt, afficher les journaux de débogage, etc.), affichage d’informations développeur instrumentation et de débogage doit être activée. Ce guide explique comment rechercher et d’activer ces paramètres.

