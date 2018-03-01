---
title: Notions de base des applications
description: "Principaux Concepts d’applications"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: 9cace37a9e762a49a90963591931793d30d05453
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>Notions de base des applications

Cette section fournit un guide sur certaines des tâches de choses ou concepts que les développeurs doivent connaître lors du développement d’applications de Xamarin.iOS (anciennement MonoTouch) plus courant.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Sécurité de Transport de l’application](~/ios/app-fundamentals/ats.md)

Cet article vous présente les modifications de sécurité qui applique la sécurité de Transport d’application sur une application iOS 9 et ce que cela signifie pour vos projets Xamarin.iOS, qui couvre les options de configuration de couches d’application et elle explique comment annuler des couches d’application, si nécessaire. Étant donné que les couches d’application est activé par défaut, toutes les connexions internet non sécurisées lève une exception dans les applications iOS 9 (sauf si vous avez explicitement autorisé).


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)

Arrière-plan de traitement ou backgrounding est le processus de laisser les applications à effectuer des tâches en arrière-plan pendant une autre application s’exécute dans le premier plan. Ce guide constitue une introduction aux e/s de traitement en arrière-plan.


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Les délégués, les protocoles et les événements](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Cet article présente les technologies de clé iOS utilisés pour recevoir des rappels et remplir les contrôles d’interface utilisateur avec des données. Ces technologies sont des événements, des protocoles et des délégués ; Cet article explique ce que chacune d’elles est et comment chacune est utilisée à partir de c#. Il montre comment Xamarin.iOS utilise les contrôles d’iOS pour exposer des événements de .NET familiers, ainsi que la manière dont Xamarin.iOS prend en charge les concepts de Objective-C, tels que les protocoles et des délégués (délégués d’Objective-C ne doivent pas être confondues avec les délégués en c#). Cet article fournit également des exemples qui montrent comment protocoles sont utilisés à la fois comme base pour les délégués de Objective-C et dans les scénarios non délégué.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Thread](~/ios/app-fundamentals/threading.md)

Cet article traite des threads dans une application Xamarin.iOS et parle un peu le pool de threads .NET, la réponse des applications et de garbage collection.&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Utilisation d’images](~/ios/app-fundamentals/images-icons/index.md)

Cet article examine comment utiliser des images dans Xamarin.iOS, les images de prise en charge d’application (tels que des icônes, le chargement des images, etc.) et images dans les applications (tels que des images de contrôles). Il explique également comment utiliser Visual Studio pour Mac pour incorporer des images, ainsi que comment interagir avec les images à partir du code.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Utilisation des listes de propriétés](~/ios/app-fundamentals/index.md)

Ce document présente Visual Studio pour l’éditeur de liste (.plist) de Mac propriété graphique et avancées pour travailler avec Info.plist et Entitlements.plist. Il illustre la définition d’icônes et les images pour application iOS de démarrage et illustre les fonctionnalités des applications en spécifiant (droits) à partir de Visual Studio pour Mac.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Utilisation du système de fichiers](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS pouvez utiliser les mêmes classes System.IO pour travailler avec des fichiers et des répertoires dans iOS que vous utiliseriez dans n’importe quelle application .NET. Toutefois, malgré les classes familiers et les méthodes, iOS implémente certaines restrictions sur les fichiers qui peuvent être créés ou accessibles et fournit également des fonctionnalités spéciales pour certains répertoires. Cet article décrit ces restrictions et les fonctionnalités et illustre le fonctionnement de l’accès aux fichiers dans une application Xamarin.iOS.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Création des Applications iOS dans le Code](~/ios/app-fundamentals/ios-code-only.md)

Cet article explique comment créer des applications iOS entièrement dans le code à l’aide de Visual Studio et Visual Studio pour Mac. Il montre comment démarrer à partir d’un modèle de projet vide pour créer un écran de l’application dans un contrôleur en créant une hiérarchie des vues à partir de UIKit. Ensuite, il explique comment créer des vues personnalisées qui peuvent être chargées dans un contrôleur.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Utilisation des valeurs par défaut de l’utilisateur](~/ios/app-fundamentals/user-defaults.md)

La `NSUserDefaults` classe offre un moyen pour iOS, applications et des Extensions pour interagir avec le système par défaut à l’échelle du système. À l’aide du système par défaut, l’utilisateur peut configurer un comportement d’une application ou de styles pour répondre à leurs préférences (basés sur la conception de l’application). Par exemple, pour présenter des données dans Visual Studio métrique mesures impériales ou sélectionner un thème de l’interface utilisateur donné.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Touch](~/ios/app-fundamentals/touch/index.md)

Les écrans tactiles sur de nombreux périphériques actuels permettent aux utilisateurs rapidement et efficacement d’interagir avec les périphériques de façon naturelle et intuitive. Cette interaction n’est pas limitée uniquement à la détection tactile : il est possible d’utiliser des gestes également. Par exemple, le mouvement de pincement pour zoomer est un exemple courant de cette solution par pincement une partie de l’écran de l’utilisateur peut effectuer un zoom avant ou arrière de deux doigts. Ce guide examine tactiles et mouvements dans iOS.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Sécurité et confidentialité](~/ios/app-fundamentals/security-privacy.md)

Apple a apporté plusieurs améliorations qui aideront le développeur d’améliorer la sécurité de leurs applications et de garantir la confidentialité de l’utilisateur final pour la sécurité et confidentialité dans iOS 10 (et supérieur). Cet article décrit l’implémentation de ces fonctionnalités dans une application Xamarin.iOS.

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Localisation](~/ios/app-fundamentals/localization/index.md)

Ce guide couvre le l’ajout de codages à une application Xamarin.iOS pour prendre en charge d’internationalisation.