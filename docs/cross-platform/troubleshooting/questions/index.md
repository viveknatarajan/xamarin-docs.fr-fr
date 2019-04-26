---
title: Forum aux Questions général
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61157191"
---
# <a name="general-frequently-asked-questions"></a>Forum aux Questions général

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Comment afficher les bibliothèques prises en charge dans une bibliothèque de classes portable ?](pcl-support-libraries.md)
Ce guide répertorie des ressources et des méthodes pour déterminer si votre bibliothèque existante est pris en charge par les différentes plateformes cibles de bibliothèque de classes portable, ou peut être converti en un profil de bibliothèque de classes portable.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[API de réflexion de bibliothèque de classes portable](pcl-reflection.md)
Microsoft a développé une nouvelle API de réflexion pour une utilisation dans les bibliothèques de classes portables. Si vous avez un code de réflexion existant que vous souhaitez déplacer vers une bibliothèque de classes portable, il peut ne pas fonctionne.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Étude de cas de bibliothèque de classes portable : Comment puis-je résoudre des problèmes liés à System.Diagnostics.Tracing pour le package NuGet Microsoft TPL Dataflow ?](pcl-case-study.md)
Xamarin.iOS et Xamarin.Android n’implémentent pas 100 % de chaque profil PCL acceptant en tant que références. Pour plus de commodité pratique dans Visual Studio pour Mac, Visual Studio et le Gestionnaire de package NuGet, projets Xamarin autorisent l’utilisation de plusieurs profils qui ont uniquement des implémentations incomplètes. Par exemple, ni Xamarin.iOS ni Xamarin.Android actuellement inclut une implémentation complète des types dans le `System.Diagnostics.Tracing` espace de noms de bibliothèque de classes portable. Vous pouvez contourner ce en basculant le projet d’application pour référencer le portable-net45 win8 + wp8 + wpa81 version de la bibliothèque de flux de données TPL.

## <a name="nuget-packages--xamarin-components"></a>Composants Xamarin et les packages NuGet
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Comment mettre à jour NuGet ?](nuget-update.md)
Vous trouverez les mises à jour de NuGet, extensions et des compléments sous la **mises à jour** onglet dans le **Gestionnaire de Package NuGet**. Navigation détaillée pour rechercher les mises à jour dans Visual Studio pour Mac et Visual Studio est dans ce guide.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Comment mettre à niveau un package NuGet ?](nuget-package-downgrade.md)
Visual Studio pour Mac et Visual Studio ont des fonctionnalités pour sélectionner des versions antérieures des packages et les installer automatiquement ; semblable à la façon dont la mise à jour des packages fonctionne.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Erreur de packages manquants après la mise à jour de packages NuGet](nuget-packages-missing.md)
Ce problème a principalement été signalé sur les solutions d’application exemple Xamarin.Forms, mais le risque de ce problème peut se produire sur n’importe quel projet qui utilise les packages NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Unification des composants des services Google Play et de NuGet](gps-components-nuget.md)
Il utilisée pour être plusieurs composants des Services Google Play et les packages NuGet, mais pour simplifier les choses pour les développeurs, nous avons maintenant unifiée nos composants et NuGet packages en deux. Dans presque tous les cas, Google Play Services doit être utilisés. La seule raison pour laquelle utiliser le package (Froyo) est si vous ciblez activement Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Où sont stockés les composants sur ma machine ?](component-storage.md)
Chaque fois que vous installez un composant de Xamarin dans un projet d’application, il est placé dans les deux emplacements répertoriés dans ce guide.


## <a name="troubleshooting"></a>Résolution des problèmes
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Où trouver les informations sur ma version et les journaux ?](version-logs.md)
Ce guide explique où trouver la plupart des informations de diagnostic qui peuvent être utilisées pour résoudre les problèmes de Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Quand et comment dois-je classer un rapport de bogues ?](howto-file-bug.md)
Ce guide fournit des conseils permettant de soumettre des rapports de bogues de haute qualité, afin que nos ingénieurs sont en mesure de déterminer la cause (et toutes les corrections éventuelles) pour un problème plus efficacement.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Pourquoi Jenkins n’est-il pas pris en charge par Xamarin ?](xamarin-jenkins.md)
Jenkins est une suite d’intégration continue open source ; en raison de problèmes de ce nombre sont directement générés par le Jenkins *lui-même* devra être archivés sous forme de problèmes contre où vous avez obtenu le code, telles que la [principal Jenkins référentiel](https://github.com/jenkinsci/jenkins), ou le référentiel pour [ Jenkins.app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Quels sont les paramètres de projet requis pour le débogueur ?](debugger-settings.md)
Dans l’ordre pour que le débogueur fonctionne comme prévu (positionnement des points d’arrêt, afficher les journaux de débogage, etc.), affichage des informations developer instrumentation et de débogage doit être activée. Ce guide décrit en détail comment trouver et d’activer ces paramètres.

