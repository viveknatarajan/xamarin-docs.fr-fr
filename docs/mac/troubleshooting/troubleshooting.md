---
title: Conseils de dépannage Xamarin.Mac
description: Ce document décrit les approches permettant de résoudre les problèmes rencontrés lors du développement d’applications de Xamarin.Mac. Elle explique également comment obtenir un support technique.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5e6cd5b338034cfa9956244015d4585b4f005793
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792965"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Conseils de dépannage Xamarin.Mac

## <a name="overview"></a>Vue d'ensemble

Nous avons tous utilisée comme parfois bloquées tout en travaillant sur un projet sur l’incapacité à obtenir une API pour fonctionner comme nous voulons ou en essayant de contourner un bogue. Notre objectif à Xamarin consiste à réussir lors de l’écriture de vos applications de bureau et mobiles, et nous vous avons fourni des ressources à l’aide.

Chacune de ces ressources, il existe certaines étapes de préparation, que vous pouvez prendre pour les aider à résoudre votre problème rapidement :

- Déterminer la cause du problème en tant que meilleures que possible afin de signaler les blocages :
 
     - « Mon application se bloque » est difficile à diagnostiquer. « Mon application tombe en panne lorsque vous retournez un tableau vide pour cet appel » est beaucoup plus facile à réparer.

     - « Impossible d’obtenir un NSTable fonctionne » est moins utile à « Aucune des méthodes sur mon NSTableDelegate semble être appelée dans ce cas ».

- Si possible, fournissez un programme de petit exemple montrant le problème. Plonger via les pages de code source à la recherche pour le problème prend beaucoup plus de temps et d’efforts.

- Si vous savez que les modifications apportées à votre application pour entraîner un problème apparaissent peuvent rapidement réduire la source du problème. Noter si vous avez récemment mis à niveau les versions de Xamarin.Mac, la suppression des sections de votre application pour trouver la partie de l’origine du problème, ou de test précédent pour rechercher le changement a introduit le problème peut être très utile.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Que faire quand votre application tombe en panne sans aucune sortie

Dans la plupart des cas, le débogueur dans Visual Studio pour Mac intercepte les exceptions et se bloque dans votre application et vous aider à identifier la cause racine. Toutefois il existe certains cas où votre application sera rebondit sur la station d’accueil et se termine avec peu ou pas de sortie. Celles-ci peuvent inclure :

- Problèmes de signature de code.
- Certain runtime mono tombe en panne.
- Certaines exceptions de Objective-c et les blocages.
- Certains se bloque très tôt la durée de vie de processus.
- Certains dépassements de capacité de la pile.
- La version de macOS répertoriée dans votre **Info.plist** est plus récente que votre version de macOS actuellement installé ou il n’est pas valide.

Ces programmes de débogage peut être frustrant, en tant que recherche les informations nécessaires peuvent être difficiles. Voici quelques approches qui peuvent aider à :

- Vérifiez que la version de macOS répertoriée dans le **Info.plist** est identique à celui de la version de macOS actuellement installé sur l’ordinateur.
- Vérifiez le Visual Studio pour la sortie de l’Application Mac (**vue** -> **remplit** -> **sortie de l’Application**) pour empiler les traces ou en rouge à partir de sortie / Cocoa qui peut décrire la sortie.
- Exécutez votre application à partir de la ligne de commande et examinez la sortie (dans le **Terminal** application) à l’aide de : 

     `MyApp.app/Contents/MacOS/MyApp` (où `MyApp` est le nom de votre application)
- Vous pouvez augmenter la sortie en ajoutant « MONO_LOG_LEVEL » à votre commande sur la ligne de commande, par exemple : 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Vous pouvez joindre un débogueur natif (`lldb`) à votre processus pour voir si cela fournit plus d’informations (cela requiert une licence payante). Par exemple, procédez comme suit :

    1. Entrez `lldb MyApp.app/Contents/MacOS/MyApp` dans le Terminal.
    2. Entrez `run` dans le Terminal.
    3. Entrez `c` dans le Terminal.
    4. Sortie lors de la fin du débogage.
- En dernier recours, avant d’appeler `NSApplication.Init` dans votre `Main` (méthode) (ou dans d’autres emplacements en fonction des besoins), vous pouvez écrire le texte dans un fichier dans un emplacement connu pour assurer le suivi à quelle étape de démarrage que vous êtes confronté à des problèmes.

## <a name="known-issues"></a>Problèmes connus

Les sections suivantes abordent les problèmes connus et leurs solutions.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Impossible de se connecter au débogueur dans les applications de bac à sable

Le débogueur se connecte à des applications de Xamarin.Mac via TCP, ce qui signifie que par défaut lorsque vous activez le sandboxing, il est impossible de se connecter à l’application, donc si vous essayez d’exécuter l’application sans les autorisations appropriées activées, vous obtenez une erreur *« Impossible de se connecter à le débogueur »*. 

[![Modifier les droits](troubleshooting-images/debug01.png "modifier les droits")](troubleshooting-images/debug01-large.png#lightbox)

Le **autoriser les connexions réseau sortantes (Client)** autorisation est celui requis pour le débogueur, l’activation de celle-ci permet normalement le débogage. Étant donné que vous ne pouvez pas déboguer sans lui, nous avons mis à jour le `CompileEntitlements` cibles pour `msbuild` pour ajouter automatiquement cette autorisation pour les droits pour les versions seulement n’importe quelle application est sable pour le débogage. Versions Release doivent utiliser les droits que ceux spécifiés dans le fichier de droits, non modifié.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException : aucune donnée n’est disponible pour l’encodage 437
 
Lorsque vous incluez des bibliothèques tierces 3e dans votre application Xamarin.Mac, vous pouvez obtenir une erreur sous la forme « System.NotSupportedException : aucune donnée n’est disponible pour l’encodage 437 » lorsque vous tentez de compiler et exécuter l’application. Par exemple, bibliothèques, telles que `Ionic.Zip.ZipFile`, peut lever cette exception au cours de l’opération.

Cela peut être résolu en ouvrant les options pour le projet Xamarin.Mac, en accédant à **Mac générer** > **internationalisation** et en vérifiant la **ouest** internationalisation :

[![Modification des options de build](troubleshooting-images/issue01.png "Modification des options de build")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Échec de compilation (mm5103)

Cette erreur survient généralement lorsqu’une nouvelle version de Xcode est mise en production et que vous avez installé la nouvelle version, mais n’a encore exécutée. Avant d’essayer de compiler avec une nouvelle version de Xcode, vous devez tout d’abord exécuter au moins une fois que la version.

La première fois que vous exécutez une nouvelle version de Xcode, il installe plusieurs outils de ligne de commande qui sont requis par Xamarin.Mac. En outre, vous devez effectuer un nettoyage de build après la mise à jour Xcode ou votre version Xamarin.Mac.

Si vous ne pouvez pas résoudre ce problème, veuillez [entrer un bogue](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Entitlements.plist manquant

La dernière version de Visual Studio pour Mac a supprimé la section des droits à partir de la **Info.plist** éditeur et placé dans distinct **Entitlements.plist** éditeur (pour mieux prendre en charge multiplateforme avec Xamarin.iOS).

Avec le nouveau Visual Studio pour Mac est installé, lorsque vous créez un projet d’application Xamarin.Mac, un **Entitlements.plist** fichier est automatiquement ajouté à l’arborescence du projet :

![Sélection de droits](troubleshooting-images/entitlements01.png "en sélectionnant des droits")

Si vous double-cliquez sur le **Entitlements.plist** , l’éditeur de droits s’affichera :

[![Modifier les droits](troubleshooting-images/entitlements02.png "modifier les droits")](troubleshooting-images/entitlements02-large.png#lightbox)

Pour les projets Xamarin.Mac existants, vous devez créer manuellement le **Entitlements.plist** le fichier en cliquant sur le projet dans le **Solution remplissage** et en sélectionnant **ajouter**  >  **Nouveau fichier...** . Ensuite, sélectionnez **Xamarin.Mac** > **liste de propriétés vide**:

![Ajout d’une nouvelle liste de propriétés](troubleshooting-images/entitlements03.png "Ajout d’une nouvelle liste de propriétés")

Entrez `Entitlements` pour le nom et cliquez sur le **nouveau** bouton. Si votre projet inclut déjà un fichier des droits, vous devrez ajouter le projet au lieu de créer un nouveau fichier :

[![Vérifier le remplacement d’un fichier](troubleshooting-images/entitlements04.png "vérifier le remplacement d’un fichier")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="contacting-support-business-or-enterprise-licenses"></a>Contacter le support (licences d’entreprise ou de l’entreprise)

Si vous disposez d’une entreprise ou une licence d’entreprise, vous pouvez demander de l’aide directement à partir des ingénieurs de Xamarin via des tickets de support. Consultez [xamarin.com/support](http://xamarin.com/support) pour plus d’informations.

## <a name="community-support-on-the-forums"></a>Prise en charge de la Communauté sur les forums

La Communauté de développeurs à l’aide de Xamarin produits est incroyable et nombreux visitez notre [Xamarin.Mac forums](http://forums.xamarin.com/categories/mac) pour partager des expériences et leur expertise. En outre, les ingénieurs de Xamarin visiter régulièrement le forum pour aider à.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Classement d’un bogue

Vos commentaires nous sont précieux. Si vous constatez des problèmes avec Xamarin.Mac :

- Recherchez dans le [dépôt des problèmes](https://github.com/xamarin/xamarin-macios/issues) 
- Avant de devenir des problèmes GitHub, les problèmes Xamarin étaient suivis sur [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Recherchez-y des problèmes correspondants.
- Si vous ne trouvez pas de problème correspondant, ouvrez un nouveau problème dans le [dépôt de problèmes GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

Les problèmes GitHub sont tous publics. Il n’est pas possible de masquer des commentaires ou des pièces jointes. 

Dans la mesure du possible, essayez d’inclure :                                                                                                                                          

- Un exemple simple reproduisant le problème. Si vous pouvez en fournir un, c’est **inestimable**. 
- La trace de pile complète du plantage.
- Le code C# se rapportant au plantage. 
