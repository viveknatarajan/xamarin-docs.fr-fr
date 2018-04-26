---
title: Introduction à l’intégration continue avec Xamarin
description: Intégration continue est une pratique de l’ingénierie logicielle dans laquelle une génération automatique compile et teste éventuellement une application lorsque le code est ajouté ou modifié par les développeurs dans le référentiel de contrôle de version du projet. Cet article présente les concepts généraux de l’intégration continue et certaines des options disponibles pour l’intégration continue avec les projets de Xamarin.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 5e4720ac44876620783b32f731c44afe5a623231
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Introduction à l’intégration continue avec Xamarin

_Intégration continue est une pratique de l’ingénierie logicielle dans laquelle une génération automatique compile et teste éventuellement une application lorsque le code est ajouté ou modifié par les développeurs dans le référentiel de contrôle de version du projet. Cet article présente les concepts généraux de l’intégration continue et certaines des options disponibles pour l’intégration continue avec les projets de Xamarin._

Il est courant dans les projets de logiciels pour les développeurs de travailler en parallèle. À un moment donné, il n’est nécessaire intégrer tous ces parallèles flux de travail en une seule base de code qui composent le produit final. Dans les premières versions de développement de logiciels, cette intégration a été effectuée à la fin d’un projet, ce qui était un processus difficile et présente des risques.

Intégration continue (CI) éviter ces difficultés en fusionnant les modifications de chaque développeur dans la base de code commune en continu, généralement chaque fois que les développeurs vérifie les modifications du projet partagé référentiel de code. Chaque archivage déclenche une génération automatique et exécute des tests automatisés pour vérifier que le code qui vient d’être introduit n’a pas interrompre tout code existant.  De cette façon, l’élément de configuration met en évidence les problèmes et erreurs immédiatement et garantit que tous les membres de l’équipe restent à jour avec chacun des autres travaux. Cela entraîne un code de base stable et cohérent.

Systèmes d’intégration continue ont deux parties principales :

-  **Contrôle de version** – Version contrôle (VC), également appelé contrôle de code source ou la gestion du code source, tout le code d’un projet de consolide dans un référentiel partagé unique et conserve un historique complet de chaque modification apportée à chaque fichier. Ce référentiel, souvent appelée le *ligne principale* ou *master* créer une branche, contient le code source qui est finalement être utilisé pour générer la production ou de version de l’application. Il existe plusieurs open source et des produits commerciaux pour cette tâche, qui permettent généralement plusieurs personnes pour répliquer une copie du code en branches secondaire où elles peuvent apporter des modifications importantes ou réaliser des expériences sans risque à la branche principale ou équipes. Une fois les modifications apportées à une branche secondaire sont validées, ils peuvent ensuite être ensemble de nouveau fusionnées dans la branche principale.
-  **Serveur d’intégration continue** : le serveur d’intégration continue est responsable de la collecte de tous les artefacts d’un projet (code source, images, vidéos, bases de données, des tests automatisés, etc.) et la compilation de l’application en cours d’exécution des tests automatisés. Là encore, il existe plusieurs open source et les outils de serveur de l’élément de configuration professionnelle.

Les développeurs ont généralement une copie de travail d’une ou plusieurs branches sur leurs stations de travail, où le travail est effectué au départ. Une fois qu’un ensemble approprié de travail est terminé, les modifications sont « archivées » ou « validées » à la branche appropriée, qui les propage sur les copies de travail d’autres développeurs. Voici comment une équipe garantit qu’ils travaillent bien sur le même code.

Là encore, avec l’intégration continue, le fait d’effectuer des modifications, le serveur de l’élément de configuration générer le projet et exécuter des tests automatisés pour vérifier l’exactitude du code source. Si des erreurs de build ou d’échecs de test, un serveur de l’élément de configuration avertit le développeur responsable (par courrier électronique, messagerie instantanée, Twitter, Growl, etc.) afin qu’il ou elle peut corriger le problème. (Les serveurs de l’élément de configuration peuvent refuser même de la validation s’il existe des erreurs, qui est appelé un « archivage contrôlé ».)

Le diagramme suivant illustre ce processus :

[![](intro-to-ci-images/intro01-small.png "Ce diagramme illustre ce processus")](intro-to-ci-images/intro01.png#lightbox)

Les applications mobiles présentent des défis uniques pour l’intégration continue. Les applications peuvent nécessiter des capteurs tels que le GPS ou d’un appareil photo qui sont disponibles uniquement sur des périphériques physiques. En outre, simulateurs ou les émulateurs sont uniquement une approximation du matériel et peuvent dissimuler ou masquer des problèmes. Au final, il est nécessaire tester une application mobile sur du vrai matériel pour être certain qu’il s’agit réellement prêtes à l’emploi.

Le [Application Center Test](https://docs.microsoft.com/appcenter/test-cloud) résout ce problème particulier en test d’applications directement sur des centaines de périphériques physiques. Les développeurs écrivent des tests d’acceptation automatisés, qui autorisent les tests d’interface utilisateur puissantes. Une fois que ces tests sont téléchargés au centre de l’application, le serveur de l’élément de configuration permettre les exécuter automatiquement dans le cadre d’un processus de l’élément de configuration comme indiqué dans le diagramme suivant :

[![](intro-to-ci-images/intro02-small.png "Une fois que ces tests sont téléchargés au centre de l’application, le serveur de l’élément de configuration permettre les exécuter automatiquement dans le cadre d’un processus de l’élément de configuration comme indiqué dans ce diagramme")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Composants d’intégration continue

Il existe un écosystème complet d’outils commerciaux et open source conçu pour prendre en charge l’élément de configuration. Cette section décrit quelques-unes des causes plus courantes.

## <a name="version-control"></a>Gestion de version

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services et Team Foundation Server

[Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs) (VSTS) et [Team Foundation Server](http://msdn.microsoft.com/vstudio/ff637362.aspx) (TFS) sont des outils de collaboration de Microsoft pour l’intégration continue build services, suivi de tâches, de planification agile et création de rapports outils et version contrôle. Avec le contrôle de version, VSTS et TFS peut fonctionner avec son propre système (Team Foundation Version Control ou TFVC) ou avec des projets hébergés sur GitHub.

 - Visual Studio Team Services fournit des services via le cloud. Son principal avantage est qu’il ne nécessite pas un matériel dédié ou une infrastructure et est accessible à partir de n’importe où via les navigateurs web et des outils de développement courants tels que Visual Studio, rendant attrayante pour les équipes qui sont géographiquement distribué. Il est disponible pour les équipes de cinq développeurs ou moins, une fois les licences supplémentaires peuvent être achetées pour prendre en charge d’une équipe croissante.
 - TFS est conçu pour les serveurs Windows locaux et accessibles via un réseau local ou une connexion VPN au réseau. L’avantage principal est entièrement contrôler la configuration des serveurs de build et peut installer les logiciels supplémentaires ou des services sont nécessaires. TFS a une édition Express libre pour des petites équipes.

TFS et VSTS sont intégrés à Visual Studio et permettent aux développeurs d’effectuer de contrôle de version et les tâches de l’élément de configuration à partir d’un IDE unique sans quitter. Le plug-in Team Explorer Everywhere pour Eclipse (voir ci-dessous) est également disponible. Visual Studio pour Mac n’offre pas une prise en charge de TFS ou VSTS.

Système de génération du Service Visual Studio Team a une prise en charge directe pour les projets de Xamarin, dans lequel vous créez une définition de build pour chaque plateforme que vous souhaitez cible (Android, iOS et Windows). La licence Xamarin appropriée est nécessaire pour chaque définition de build. Il est également possible de se connecter à une variable locale, prenant en charge de Xamarin TFS build server pour Visual Studio Team Services à cet effet. Avec cette configuration, les builds sont en attente pour VSTS seront déléguées pour le serveur local. Pour plus d’informations, reportez-vous à [déployer et configurer un serveur de builds](https://msdn.microsoft.com/library/ms181712.aspx). Vous pouvez également utiliser un autre outil de génération telle que Jenkins ou une équipe.

Un résumé complet de toutes les fonctionnalités d’Application Lifecycle Management (ALM) de Visual Studio, Visual Studio Team Services et Team Foundation Server, consultez [gestion du cycle de vie des applications avec les applications Xamarin](https://msdn.microsoft.com/library/mt162217(v=vs.140).aspx) sur MSDN.


### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](http://msdn.microsoft.com/library/gg413285.aspx) apporte aux équipes de développement en dehors de Visual Studio la puissance de Team Foundation Server et Visual Studio Team Services. Il permet aux développeurs de se connecter aux projets d’équipe sur site ou dans le cloud à partir d’Eclipse ou le client de ligne de commande multiplateforme pour OS X et Linux. Team Explorer Everywhere fournit complet d’accès au contrôle de version (y compris Git), des éléments de travail et générer des fonctions pour les plateformes non Windows.


### <a name="git"></a>Git

[GIT](http://git-scm.com) est une solution de contrôle de version open source populaires qui a été développée pour gérer le code source pour le noyau Linux. C’est un système très rapide, flexible qui est souvent utilisé avec des projets de logiciels de toutes tailles. Il peut évoluer facilement de développeurs uniques connecté à Internet médiocres à grandes équipes qui couvrent le monde entier. GIT vous permet également de créer une branche très simple, qui à son tour peut encourager les flux parallèles de développement en limitant les risques.

GIT peut fonctionner entièrement via un navigateur web, ou via [les clients de l’interface graphique utilisateur](http://git-scm.com/downloads/guis) qui s’exécutent sur Windows, Linux et Mac OSX. C’est gratuit pour les référentiels publics ; dépôts privés nécessitent un [payé plan](https://github.com/pricing).

Visual Studio 2015 et Visual Studio pour Mac fournissent une prise en charge native pour Git ; pour les versions antérieures, Microsoft fournit un [extension téléchargeable pour Git](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Comme indiqué ci-dessus, Visual Studio Team Services et TFS peut utiliser Git pour le contrôle de version au lieu de TFVC.


### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) est un système de contrôle de version populaires et open source qui a été utilisé depuis 2000. SVN s’exécute sur toutes les versions récentes du système d’exploitation X, Windows, FreeBSD, Linux et Unix. Visual Studio pour Mac propose une prise en charge native pour SVN. Il existe des extensions tierces qui permettent la prise en charge de SVN Visual Studio.


## <a name="continuous-integration-environments"></a>Environnements d’intégration continue

Configuration d’un environnement d’intégration continue signifie combinant un système de contrôle de version avec le service de build.  Dans le second cas, les deux plus courantes sont :

- [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) est le système de génération de Visual Studio Team Services et de TFS. Il est étroitement intégré à Visual Studio, qui permet les builds, il est pratique pour les développeurs déclencher automatiquement exécuter des tests et afficher les résultats.
- Jenkins est un serveur de l’élément de configuration open source avec un écosystème complet de plug-ins pour prendre en charge tous les types de développement de logiciels. Il s’exécute sur Windows et Mac OS X. Jenkins n’est pas intégré à n’importe quel IDE spécifique. Au lieu de cela, il est configuré et géré via une interface web. Jenkins CI est également facile à installer et configurer ce qui le rend attrayants pour les petites équipes.

Vous pouvez utiliser TFS/VSTS par lui-même, ou vous pouvez utiliser Jenkins en combinaison avec TFS/VSTS ou Git, comme décrit dans les sections suivantes.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services et Team Foundation Server

Comme indiqué, Visual Studio Team Services et Team Foundation Server fournit la version la version de contrôle et de créer des services. Services de build requièrent toujours une licence Xamarin entreprise ou de l’entreprise pour chaque plateforme cible.

Avec Visual Studio Team Services, vous créez une définition de build distincts pour chaque plateforme cible et entrez la licence appropriée il. Une fois configuré, VSTS exécuter des builds et les tests dans le cloud. Consultez [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) pour plus d’informations.

Avec Team Foundation Server, vous configurez un ordinateur de build pour les plateformes cibles spécifiques comme suit :

- **Android et Windows :** installer Visual Studio et le Xamarin tools (pour Android et Windows) et configurer vos licences Xamarin. Il est également nécessaire de transférer le SDK Android à un emplacement partagé sur le serveur sur lequel l’agent de build TFS puissent le trouver. Pour plus d’informations, consultez [TFVC de configuration](https://docs.microsoft.com/vsts/tfvc/overview).
- **iOS et Xamarin :** installer Visual Studio et les outils Xamarin sur le serveur Windows avec la licence appropriée. Ensuite, installez Visual Studio pour Mac sur un ordinateur Mac OS X accessible via le réseau, ce qui va servir à un hôte de build et le package d’application finale (IPA pour iOS, l’application pour OS X).

Le diagramme suivant illustre cette topographie :

[![](intro-to-ci-images/intro03-small.png "Ce diagramme illustre cette topographie")](intro-to-ci-images/intro03.png#lightbox)

Il est également possible de lier un serveur TFS local à un projet Visual Studio Team Services afin que les builds VSTS sont déléguées sur le serveur local. Pour plus d’informations, consultez [déployer et configurer un serveur de builds](http://msdn.microsoft.com/library/ms181712.aspx) sur MSDN.

### <a name="visual-studio-team-services-and-jenkins"></a>Jenkins et visual Studio Team Services

Si vous utilisez Jenkins créer des applications, vous pouvez stocker votre code dans Visual Studio Team Services ou Team Foundation Server et continuer à utiliser Jenkins pour vos builds de l’élément de configuration. Vous pouvez déclencher une build Jenkins lorsque vous effectuez un push au référentiel Git de votre projet d’équipe ou lorsque vous archivez code dans TFVC code. Pour plus d’informations, consultez [Jenkins avec Visual Studio Team Services](https://www.visualstudio.com/docs/marketplace/integrate/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Si vous utilisez Jenkins créer des applications, vous pouvez stocker votre code dans Visual Studio Team Services ou Team Foundation Server et continuer à utiliser Jenkins pour vos builds de l’élément de configuration")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>GIT et Jenkins

Un autre environnement commun de l’élément de configuration peut être entièrement OS X en fonction. Ce scénario implique l’utilisation de Git pour le contrôle de code source et Jenkins pour le serveur de builds. Ces deux éléments sont en cours d’exécution sur un seul ordinateur Mac OS X avec Visual Studio pour Mac installé. Cela est très similaire au Visual Studio Team Services + environnement Jenkins présentés dans la section précédente :

[![](intro-to-ci-images/intro05-small.png "Cela est très similaire au Visual Studio Team Services + environnement Jenkins présentés dans la section précédente")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Remarque : Jenkins est [non pris en charge par Xamarin](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**


# <a name="summary"></a>Récapitulatif

Ce document a introduit le concept d’intégration continue et les avantages qu’il permet aux équipes de développement logiciel. L’importance du contrôle de version a été indiqué, ainsi que le rôle et les responsabilités du serveur de build. Le document allés pour discuter de certains des outils qui peuvent être utilisé pour le contrôle de code source et les serveurs de builds. Nous avons introduit également application Centre de Test, ce qui permet aux développeurs de publier des applications excellentes en exécutant des tests automatisés qui seront avère la qualité et les fonctionnalités de leurs applications. Plus de documentation sur la soumission d’applications et les tests au centre de l’application, vous pouvez trouver [ici](https://docs.microsoft.com/appcenter/test-cloud). Enfin, pour aider à comprendre comment ces outils et les composants s’articulent, décrit plusieurs environnements l’élément de configuration qui peuvent établir des organisations pour l’intégration continue. Pour plus d’informations à l’aide de Visual Studio Team Services et Team Foundation Server avec les projets de Xamarin, consultez [TFVC de configuration](https://docs.microsoft.com/vsts/tfvc/overview) et [introduction d’intégration continue](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1). De même, si vous utilisez Jenkins, consultez [à l’aide de Jenkins avec Xamarin](~/tools/ci/jenkins-walkthrough.md) pour plus d’informations sur la configuration d’une intégration continue.