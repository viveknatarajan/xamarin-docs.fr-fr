---
title: Pourquoi n’est pas Jenkins prises en charge Microsoft ?
description: Ce document décrit un haut niveau, l’interaction de Xamarin avec le système Jenkins CI. Il aborde également quelques problèmes courants qui s’afficher lorsque vous travaillez avec Jenkins.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: c2e409b796d5ef2525079e02aafdd0c6e8db5d81
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113441"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Pourquoi n’est pas Jenkins prises en charge Microsoft ?

## <a name="jenkins-support-explanation"></a>Explication de la prise en charge de Jenkins

Jenkins est une suite d’intégration continue open source ; en raison de problèmes de ce nombre sont directement générés par le Jenkins *lui-même* devra être archivés sous forme de problèmes contre où vous avez obtenu le code, telles que la [principal Jenkins référentiel](https://github.com/jenkinsci/jenkins), ou le référentiel pour [ Jenkins.app](https://github.com/stisti/jenkins-app).

L’exception à cette règle concerne les problèmes qui peuvent être isolées pour des bogues particuliers dans les outils de Xamarin ; Si vous pensez que c’est le cas, vous pouvez vérifier votre [options de support](~/cross-platform/troubleshooting/support-options.md), mais là encore, le problème peut provenir de quelque chose en dehors de quelles la prise en charge de Xamarin équipe pouvez *directement* aide.

## <a name="setup-jenkins-with-xamarin"></a>Le programme d’installation Jenkins avec Xamarin

Comme indiqué ci-dessus Jenkins problèmes ne sont pas pris en charge directement par notre équipe ; le [à l’aide de Jenkins avec Xamarin](~/tools/ci/jenkins-walkthrough.md) guide peut être utilisé pour configurer un serveur Jenkins CI qui est intégré avec Xamarin. 

## <a name="fixes-for-common-issues"></a>Correctifs pour les problèmes courants

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins est impossible de trouver le Kit de développement logiciel Android

Le message d’erreur pour ce problème est quelque chose comme ceci :

> erreur XA5205 : Impossible de trouver le répertoire du SDK Android. Veuillez définir via /p:AndroidSdkDirectory

Vos options permettant de définir l’emplacement du SDK peuvent varier selon le plug-in Jenkins Android exact que vous utilisez ; Il est intéressant à rechercher comment la configurer dans le guide du plug-in. Par exemple ; le [plug-in d’émulateur Android](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) recherche automatiquement le Kit de développement, mais si elle ne peut pas trouver ; l’emplacement peut également être défini via la page de Configuration du système Jenkins pour ce plug-in. 


## <a name="deprecated-errors"></a>Erreurs déconseillées

> [!IMPORTANT]
> Ce problème a été résolu dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins signale une licence Xamarin non valide
Les messages d’erreur pour ce problème sont généralement quelque chose comme

> Erreur de XA9008 : génération à partir de la ligne de commande requiert une licence entreprise

ou

> Erreur : L’Édition Starter de Xamarin.iOS ne prend pas en charge la génération en dehors de Xamarin Studio 

La cause la plus courante de ce scénario est l’utilisation de Jenkins en se connectant avec un compte d’utilisateur non associé à votre licence Xamarin. La façon la plus simple de résoudre ce problème, consiste à installer Jenkins en tant qu’application directement via le compte d’utilisateur. Ce processus et autres considérations sont décrits ici : [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
