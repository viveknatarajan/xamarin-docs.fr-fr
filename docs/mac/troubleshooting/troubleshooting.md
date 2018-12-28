---
title: Conseils de dépannage de Xamarin.Mac
description: Ce document décrit les approches pour résoudre les problèmes rencontrés lors du développement d’applications Xamarin.Mac. Il explique également comment obtenir un support technique.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: f498aab5bfaffc08a22f62a318f8f9f73ab0afca
ms.sourcegitcommit: d294c967a18e6d91f3909c052eeff98ede1a21f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609907"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Conseils de dépannage de Xamarin.Mac

## <a name="overview"></a>Vue d'ensemble

Parfois, nous sommes bloqués pendant que vous travaillez sur un projet, l’impossibilité d’obtenir une API permettant de travailler comme nous voulons soit en tentant de contourner un bogue. Notre objectif chez Xamarin est pour vous permettre de réussir à écrire vos applications mobiles et de bureau, et nous vous avons fourni quelques ressources pour aider.

Avec une de ces ressources, il existe certaines étapes de préparation, que vous pouvez prendre pour aider à résoudre votre problème rapidement :

- Déterminer la cause du problème en tant que meilleure que possible pour les signaler :
 
     - « Mon application se bloque » est difficile à diagnostiquer. « Mon application tombe en panne lorsque vous retournez un tableau vide dans cet appel » est beaucoup plus facile de travailler à la résolution.

     - « Impossible d’obtenir un NSTable fonctionne » est moins utile que « Aucune des méthodes sur mon NSTableDelegate semble pas être appelée dans ce cas. »

- Si possible, fournir un programme de petit exemple montrant le problème. Rentrer dans les pages de code source à la recherche pour le problème prend beaucoup plus de temps et d’efforts.

- Permet de savoir ce que les modifications apportées à votre application pour entraîner un problème apparaissent peuvent circonscrire rapidement la source du problème. Noter si vous avez récemment mis à niveau les versions de Xamarin.Mac, filtrage des sections de votre application pour trouver la partie qui provoque le problème, ou test précédent génère pour trouver quelle modification a introduit le problème peut être très utile.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Que faire en cas de défaillance de votre application sans aucune sortie

Dans la plupart des cas, le débogueur dans Visual Studio pour Mac intercepte des exceptions dans votre application tombe en panne et vous aider à identifier la cause racine. Toutefois il existe certains cas où votre application sera rebondissent sur la station d’accueil, puis quittez avec peu ou pas de sortie. Celles-ci peuvent inclure :

- Problèmes de signature de code.
- Certains runtime mono tombe en panne.
- Certaines exceptions Objective-c et les incidents.
- Certains se bloque très tôt la durée de vie de processus.
- Certains les débordements de pile.
- La version de macOS répertoriée dans votre **Info.plist** est plus récente que votre version de macOS actuellement installé ou il n’est pas valide.

Débogage de ces programmes peut être frustrant, en tant que recherche les informations nécessaires peuvent être difficiles. Voici quelques approches qui peuvent aider à :

- Vérifiez que la version de macOS répertoriée dans le **Info.plist** est identique à celui de la version de macOS actuellement installé sur l’ordinateur.
- Vérifier le Visual Studio pour la sortie de l’Application Mac (**vue** -> **panneaux** -> **sortie de l’Application**) pour empiler les traces ou en rouge à partir de sortie Cocoa qui peut-être décrire la sortie.
- Exécuter votre application à partir de la ligne de commande et examinez la sortie (dans le **Terminal** application) à l’aide de : 

     `MyApp.app/Contents/MacOS/MyApp` (où `MyApp` est le nom de votre application)
- Vous pouvez augmenter la sortie en ajoutant « MONO_LOG_LEVEL » à votre commande sur la ligne de commande, par exemple : 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Vous pouvez joindre un débogueur natif (`lldb`) à votre processus pour voir si cela fournit plus d’informations (cela nécessite une licence payante). Par exemple, procédez comme suit :

    1. Entrez `lldb MyApp.app/Contents/MacOS/MyApp` dans le Terminal.
    2. Entrez `run` dans le Terminal.
    3. Entrez `c` dans le Terminal.
    4. Quitter une fois le débogage.
- En dernier recours, avant d’appeler `NSApplication.Init` dans votre `Main` (méthode) (ou à d’autres emplacements en fonction des besoins), vous pouvez écrire du texte dans un fichier dans un emplacement connu pour dépister à quelle étape de lancement vous rencontrez des difficultés à.

## <a name="known-issues"></a>Problèmes connus

Les sections suivantes abordent les problèmes connus et leurs solutions.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Impossible de se connecter au débogueur dans les applications sandbox

Le débogueur se connecte aux applications Xamarin.Mac via TCP, ce qui signifie que par défaut lorsque vous activez le sandboxing, il est impossible de se connecter à l’application, donc si vous essayez d’exécuter l’application sans les autorisations appropriées activées, vous obtenez une erreur *« Impossible de se connecter à le débogueur »*. 

[![Modification des droits](troubleshooting-images/debug01.png "modification des droits")](troubleshooting-images/debug01-large.png#lightbox)

Le **autoriser les connexions réseau sortantes (Client)** autorisation est nécessaire pour que le débogueur, l’activation de celle-ci permet le débogage normalement. Étant donné que vous ne pouvez pas déboguer sans lui, nous avons mis à jour le `CompileEntitlements` cibler pour `msbuild` pour ajouter automatiquement cette autorisation pour les droits pour les builds de n’importe quelle application est sable pour le débogage uniquement. Versions Release doivent utiliser les droits spécifiés dans le fichier de droits, tels quels.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException : aucune donnée n’est disponible pour l’encodage 437
 
Lorsque vous incluez des bibliothèques tierces 3e dans votre application Xamarin.Mac, vous pouvez obtenir une erreur sous la forme « System.NotSupportedException : Aucune donnée n’est disponible pour l’encodage 437" lorsque vous tentez de compiler et exécuter l’application. Par exemple, les bibliothèques, telles que `Ionic.Zip.ZipFile`, peut lever cette exception au cours de l’opération.

Cela peut être résolu en ouvrant les options pour le projet Xamarin.Mac, en accédant à **Build Mac** > **internationalisation** et en vérifiant la **ouest** internationalisation :

[![Modification des options de build](troubleshooting-images/issue01.png "Modification des options de build")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Échec de compilation (mm5103)

Cette erreur est généralement dû quand une nouvelle version de Xcode est mise en production et que vous avez installé la nouvelle version, mais non encore l’exécuter. Avant d’essayer de compiler avec une nouvelle version de Xcode, vous devez tout d’abord exécuter au moins une fois que la version.

La première fois que vous exécutez une nouvelle version de Xcode, il installe plusieurs outils de ligne de commande qui sont requis par Xamarin.Mac. En outre, vous devez effectuer un nettoyage de build après la mise à jour Xcode ou votre version de Xamarin.Mac.

Si vous ne pouvez pas résoudre ce problème, veuillez [signaler un bogue](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Entitlements.plist manquant

La dernière version de Visual Studio pour Mac a supprimé la section de droits à partir de la **Info.plist** éditeur et placé dans distinct **Entitlements.plist** éditeur (pour une meilleure prise en charge multiplateforme avec Xamarin.iOS).

Avec le nouveau Visual Studio pour Mac est installé, lorsque vous créez un nouveau projet d’application Xamarin.Mac, un **Entitlements.plist** fichier sera automatiquement ajouté à l’arborescence du projet :

![La sélection de droits](troubleshooting-images/entitlements01.png "la sélection de droits")

Si vous double-cliquez sur le **Entitlements.plist** , l’éditeur de droits s’affichera :

[![Modification des droits](troubleshooting-images/entitlements02.png "modification des droits")](troubleshooting-images/entitlements02-large.png#lightbox)

Pour les projets Xamarin.Mac existants, vous devez créer manuellement le **Entitlements.plist** fichier en cliquant sur le projet dans le **panneau solutions** et en sélectionnant **ajouter**  >  **Nouveau fichier...** . Ensuite, sélectionnez **Xamarin.Mac** > **liste de propriétés vide**:

![Ajout d’une nouvelle liste de propriétés](troubleshooting-images/entitlements03.png "Ajout d’une nouvelle liste de propriétés")

Entrez `Entitlements` pour le nom et cliquez sur le **New** bouton. Si votre projet inclut déjà un fichier de droits, vous devrez l’ajouter au projet au lieu de créer un nouveau fichier :

[![Vérification de l’écrasement d’un fichier](troubleshooting-images/entitlements04.png "vérifier le remplacement d’un fichier")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>Prise en charge de la Communauté sur les forums

La Communauté de développeurs à l’aide des produits Xamarin est merveilleux et nombreux visitez notre [forums Xamarin.Mac](http://forums.xamarin.com/categories/mac) pour partager des expériences et leur expertise. En outre, les ingénieurs de Xamarin consultez régulièrement le forum pour aider à.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Soumettre un bogue

Vos commentaires sont importants pour nous. Si vous rencontrez des problèmes avec Xamarin.Mac :

- Recherchez dans le [dépôt des problèmes](https://github.com/xamarin/xamarin-macios/issues) 
- Avant de devenir des problèmes GitHub, les problèmes Xamarin étaient suivis sur [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Recherchez-y des problèmes correspondants.
- Si vous ne trouvez pas de problème correspondant, ouvrez un nouveau problème dans le [dépôt de problèmes GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

Les problèmes GitHub sont tous publics. Il n’est pas possible de masquer des commentaires ou des pièces jointes. 

Dans la mesure du possible, essayez d’inclure :                                                                                                                                          

- Un exemple simple reproduisant le problème. Si vous pouvez en fournir un, c’est **inestimable**. 
- La trace de pile complète du plantage.
- Le code C# se rapportant au plantage. 
