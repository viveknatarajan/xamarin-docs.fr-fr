---
title: Pourquoi n’est pas Jenkins prend-il en charge Xamarin ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.openlocfilehash: 37fc134f7e97af74f5bb019f3262972273f0c4cf
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Pourquoi n’est pas Jenkins prend-il en charge Xamarin ?

## <a name="jenkins-support-explanation"></a>Explication de la prise en charge de Jenkins

Jenkins est une suite de l’élément de configuration open source ; en raison de problèmes de ce nombre qui sont directement générés par le Jenkins *lui-même* devra être classé comme problèmes contre où vous avez obtenu le code ; comme le [principal Jenkins référentiel](https://github.com/jenkinsci/jenkins), ou le référentiel pour [ Jenkins.app](https://github.com/stisti/jenkins-app).

L’exception à cette règle concerne les problèmes qui peuvent être isolées des bogues particulier dans les outils de Xamarin ; Si vous pensez que c’est le cas, vous pouvez vérifier votre [prennent en charge des options](~/cross-platform/troubleshooting/support-options.md), bien qu’il est à nouveau, le problème peut être quelque chose en dehors de quelles la prise en charge de Xamarin équipe pouvez *directement* aide.

## <a name="setup-jenkins-with-xamarin"></a>Le programme d’installation Jenkins avec Xamarin

Comme indiqué ci-dessus Jenkins problèmes ne sont pas pris en charge directement par notre équipe ; le [à l’aide de Jenkins avec Xamarin](~/tools/ci/jenkins-walkthrough.md) guide peut être utilisé pour configurer un serveur Jenkins CI intégré avec Xamarin. 

## <a name="fixes-for-common-issues"></a>Résout les problèmes courants
### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins est impossible de trouver le SDK Android

Le message d’erreur pour résoudre ce problème est quelque chose comme ceci :

> erreur XA5205 : Impossible de trouver le répertoire du SDK Android. Définissez via /p:AndroidSdkDirectory

Vos options pour la définition de l’emplacement du Kit de développement logiciel peuvent varier selon le plug-in Jenkins Android exact que vous utilisez ; Il est intéressant de rechercher pour savoir comment définir dans le guide de plug-in. Par exemple : le [plug-in d’émulateur Android](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) recherche automatiquement le Kit de développement, mais si elle ne peut pas trouver ; l’emplacement peut également être défini via la page Configuration du système Jenkins pour ce plug-in. 


## <a name="deprecated-errors"></a>Erreurs déconseillées

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins signale une licence Xamarin non valide
Les messages d’erreur pour résoudre ce problème sont généralement quelque chose comme

> Erreur de XA9008 : génération à partir de la ligne de commande requiert une licence d’entreprise

ou

> Erreur : L’Édition Starter de Xamarin.iOS ne prend pas en charge la génération en dehors de Xamarin Studio 

La cause la plus courante de ce scénario est l’utilisation de Jenkins en se connectant à un compte d’utilisateur non associé à votre licence Xamarin. La façon la plus simple de résoudre ce problème, consiste à installer de Jenkins en tant qu’application directement via le compte d’utilisateur. Ce processus et certaines considérations supplémentaires sont décrits ici : [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
