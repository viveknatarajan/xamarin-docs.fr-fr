---
title: Outils et commandes
description: "Vue d’ensemble des outils inclus avec l’objectif Sharpie et les arguments de ligne de commande pour pouvoir les utiliser."
ms.topic: article
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 0d6e953a6a45b78d470c7ff73e1d6faa7444a683
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="tools--commands"></a>Outils et commandes

_Vue d’ensemble des outils inclus avec l’objectif Sharpie et les arguments de ligne de commande pour pouvoir les utiliser._

<style type="text/css"> .Terminal-bleu {couleur : rgb(10,96,254) ;} .terminal-vert {couleur : rgb(12,156,26) ;} .terminal à magenta {couleur : rgb(152,12,103) ;} </style>


Une fois l’objectif Sharpie correctement [installé](~/cross-platform/macios/binding/objective-sharpie/get-started.md), ouvrez un terminal et vous familiariser avec les <em>commandes</em> objectif Sharpie a à offrir :

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

<table>
  <thead>
    <tr><td>Outil</td><td>Description</td>
  </thead>
  <tbody>
    <tr><td><b>xcode</b></td><td>Fournit des informations sur l’installation actuelle de Xcode et les versions d’iOS et les kits de développement logiciel Mac qui sont disponibles. Nous utiliserons ces informations ultérieurement quand nous générons notre liaisons.</td></tr>
    <tr><td><b>pod</b></td><td>Recherche, configure, installe (dans un répertoire local) et lie Objective-C <a href="https://cocoapods.org">CocoaPod</a> bibliothèques disponibles à partir du référentiel Spec maître. Cet outil évalue la CocoaPod installée pour déduire automatiquement de l’entrée appropriée à passer à la <code>bind</code> outil ci-dessous. <em><strong>De 3.0 !</strong></em></td></tr>
    <tr><td><b>bind</b></td><td>Analyse les fichiers d’en-tête (<code>*.h</code>) dans la bibliothèque Objective-C en les <a href="~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md">initiale <i>ApiDefinition.cs</i> et <i>StructsAndEnums.cs</i> fichiers</a>.</td></tr>
    <tr><td><b>update</b></td><td>Recherche les versions plus récentes d’objectif Sharpie et télécharge et lance le programme d’installation si celle-ci est disponible.</td></tr>
    <tr><td><b>verify-docs</b></td><td>Affiche des informations détaillées sur <code>[Verify]</code> attributs.</td></tr>
    <tr><td><b>docs</b></td><td>Accède à ce document dans votre navigateur par défaut.</td></tr>
  </tbody>
</table>

Pour obtenir de l’aide sur un outil objectif Sharpie spécifique, entrez le nom de l’outil et la `-help` option. Par exemple, `sharpie xcode -help` retourne la sortie suivante :

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Nous pouvons commencer le processus de liaison, nous devons obtenir des informations sur nos kits de développement logiciel installés en cours en entrant la commande suivante dans le Terminal `sharpie xcode -sdks`. La sortie peut différer selon les versions de Xcode que vous avez installé. Objectif Sharpie recherche les kits de développement logiciel installés dans aucun `Xcode*.app` sous le `/Applications` active :

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

À partir des éléments ci-dessus, nous constatons que nous avons le `iphoneos9.1` SDK est installé sur votre ordinateur et qu’il comporte `arm64` prise en charge de l’architecture. Nous allons utiliser cette valeur pour tous les exemples de cette section. Ces informations en place, nous sommes prêts analyser les fichiers d’un en-tête de bibliothèque Objective-C dans la première `ApiDefinition.cs` et `StructsAndEnums.cs` pour le projet de liaison.

