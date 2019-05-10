---
title: Les outils de Sharpie objectif et les commandes
description: Ce document fournit une vue d’ensemble des outils inclus avec Sharpie d’objectif et les arguments de ligne de commande à utiliser avec eux.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 51a0b81204b743824e24cfed83bd73308fa8d506
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977662"
---
# <a name="objective-sharpie-tools--commands"></a>Les outils de Sharpie objectif et les commandes

_Vue d’ensemble des outils inclus avec l’objectif Sharpie et les arguments de ligne de commande de les utiliser._

<style type="text/css"> .terminal-blue { color: rgb(10,96,254); } .terminal-green { color: rgb(12,156,26); } .terminal-magenta { color: rgb(152,12,103); } </style>


Une fois objectif Sharpie avec succès [installé](~/cross-platform/macios/binding/objective-sharpie/get-started.md), ouvrez un terminal et vous familiariser avec la <em>commandes</em> objectif Sharpie a à offrir :

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Objectif Sharpie fournit les outils suivants :

|Tool|Description|
|--- |--- |
|**xcode**|Fournit des informations sur l’installation de Xcode actuelle et les versions d’iOS et les kits de développement logiciel Mac qui sont disponibles. Nous utiliserons ces informations ultérieurement lorsque nous générons notre liaisons.|
|**pod**|Recherche, configure, installe (dans un répertoire local) et lie Objective-C [CocoaPod](https://cocoapods.org/) bibliothèques disponibles à partir du référentiel Spec maître. Cet outil évalue la CocoaPod installé pour déduire automatiquement l’entrée correcte à passer à la `bind` outil ci-dessous. Nouveautés de 3.0 !|
|**bind**|Analyse les fichiers d’en-tête (`*.h`) dans la bibliothèque Objective-C dans initial [ApiDefinition.cs et StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) fichiers.|
|**update**|Vérifie les versions plus récentes d’objectif Sharpie et télécharge et lance le programme d’installation si celle-ci est disponible.|
|**verify-docs**|Affiche des informations détaillées sur `[Verify]` attributs.|
|**docs**|Accède à ce document dans votre navigateur par défaut.|

Pour obtenir une aide sur un outil Sharpie d’objectif spécifique, entrez le nom de l’outil et le `-help` option. Par exemple, `sharpie xcode -help` renvoie le résultat suivant :

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Pour pouvoir commencer le processus de liaison, nous devons obtenir des informations sur nos kits SDK installés actuelle en entrant la commande suivante dans le Terminal `sharpie xcode -sdks`. Votre sortie peut varier en fonction de quelles sont les versions de Xcode que vous avez installé. Objectif Sharpie recherche les kits de développement logiciel installés dans un `Xcode*.app` sous le `/Applications` directory :

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Dans l’exemple ci-dessus, nous constatons que nous avons le `iphoneos9.1` SDK installé sur votre ordinateur et il a `arm64` prise en charge de l’architecture. Nous allons utiliser cette valeur pour tous les exemples dans cette section. Ces informations en place, nous sommes prêts à analyser les fichiers d’en-tête une bibliothèque Objective-C dans initial `ApiDefinition.cs` et `StructsAndEnums.cs` pour le projet de liaison.
