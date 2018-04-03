---
title: Introduction aux pipelines de contenu
description: Contenu pipelines sont des applications ou des parties des applications, qui permettent de convertir des fichiers dans un format qui peut être chargé par les projets de jeux. Le Pipeline de contenu MonoGame est une implémentation d’un pipeline de contenu spécifiques pour la conversion de fichiers pour les projets CocosSharp et MonoGame.
ms.topic: article
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 7394ae5ddacb20a10e603fa50376799b82d2a3dc
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2018
---
# <a name="introduction-to-content-pipelines"></a>Introduction aux pipelines de contenu

_Contenu pipelines sont des applications ou des parties des applications, qui permettent de convertir des fichiers dans un format qui peut être chargé par les projets de jeux. Le Pipeline de contenu MonoGame est une implémentation d’un pipeline de contenu spécifiques pour la conversion de fichiers pour les projets CocosSharp et MonoGame._

Cet article fournit des informations conceptuelles sur les pipelines de contenu, principalement en mettant l’accent sur la *MonoGame contenu Pipeline*, qui est une implémentation de pipeline de contenu utilisée avec CocosSharp et MonoGame.


## <a name="what-is-a-content-pipeline"></a>Qu’est un pipeline de contenu ?

Le terme *contenu pipeline* est un terme général qui désigne le processus de conversion d’un fichier d’un format vers un autre. Le *d’entrée* du pipeline de contenu est généralement un fichier de sortie par un outil de création, tels que les fichiers image à partir de Photoshop. Le pipeline de contenu crée le *sortie* fichier dans un format qui peut être chargé directement par un projet de jeu. En général, les fichiers de sortie sont optimisées pour le chargement rapide et réduit la taille du disque.

Contenu de pipelines peuvent être en ligne de commande exécutables, les applications GUI dédiées ou plug-ins incorporé dans une autre application, tel que Visual Studio. Pipelines contenus générale s’exécuter avant l’exécution de la partie. Si le pipeline de contenu est associé à une application, tels que Visual Studio, puis il peut s’exécuter pendant la compilation. Si le pipeline de contenu est une application autonome, il peut s’exécuter lorsque explicitement dit à le faire par l’utilisateur. L’application ou le responsable de la conversion d’un fichier d’entrée spécifique de la logique (comme un **.png**) vers une sortie associée fichier porte une *Générateur*. 

Nous pouvons visualiser le chemin d’accès nécessaire d’un fichier à partir de la création en cours de chargement lors de l’exécution comme suit :

![](introduction-images/image1.png "Le chemin d’accès nécessaire à partir de la création en cours de chargement lors de l’exécution d’un fichier n’est affichée dans ce diagramme")

## <a name="why-use-a-content-pipeline"></a>Pourquoi utiliser un pipeline de contenu ?

Pipelines contenus introduisent une étape supplémentaire entre l’application de création et le jeu, ce qui peut augmenter les délais de compilation et compliquer le processus de développement. En dépit de ces considérations, les pipelines contenus introduisent un certain nombre d’avantages pour le développement de jeux :


### <a name="converting-to-a-format-understood-by-the-game"></a>Conversion en un format compréhensible par le jeu

CocosSharp et MonoGame fournissent des méthodes pour le chargement des différents types de contenu ; Toutefois, le contenu doit être correctement mis en forme avant leur chargement. La plupart des types de contenu requièrent un type de conversion avant leur chargement. Par exemple, les effets sonores dans le **.wav** format doit être converti en un **.xnb** fichier à charger lors de l’exécution depuis CocosSharp et MonoGame ne prennent pas en charge le chargement du **.wav** format de fichier.


### <a name="converting-to-a-format-native-to-the-hardware"></a>Conversion en un format natif vers le matériel

Un matériel différent peut traiter un contenu différemment lors de l’exécution. Par exemple, CocosSharp jeux peuvent charger des fichiers image lorsque vous créez un `CCSprite` instance. Bien que le même code peut être utilisé pour charger les fichiers sur iOS et Android, chaque plateforme stocke le fichier chargé différemment. Par conséquent, le Pipeline de contenu MonoGame des formats de texture **.xnb** fichiers différemment selon la plateforme cible.


### <a name="reducing-size-on-disk"></a>La réduction de taille sur disque 

Le contenu des pipelines peuvent être utilisés pour supprimer les informations, qui est utile au moment de l’auteur mais n’est pas nécessaire lors de l’exécution. Le fichier (entrée) d’origine peut stocker toutes les informations qui permettent aux créateurs de contenu de mettre à jour le contenu existant, mais le fichier de sortie peut être simplifiée afin de minimiser le fichier global du jeu. Cette considération est particulièrement utile pour les jeux mobiles qui sont téléchargés et non distribués sur le support d’installation.


### <a name="reducing-load-time"></a>Ce qui réduit le temps de chargement

Jeux peuvent nécessiter des modifications de contenu pour améliorer les performances d’exécution, pour améliorer les éléments visuels, ou ajouter de nouvelles fonctionnalités. Par exemple, de nombreux jeux 3D calculer l’éclairage d’une seule fois, puis utilisent le résultat de ce calcul lors du rendu des scènes complexes. Depuis la réalisation de ces calculs lors du chargement du contenu peut être prohibitif le calcul au lieu de cela est possible lorsque le jeu est généré. Les calculs qui en résulte peuvent être inclus dans le contenu, activer le contenu à charger beaucoup plus rapidement que ne le possible. 


## <a name="xnb-file-extension"></a>extension de fichier xnb

Le **.xnb** extension de fichier est l’extension pour tous les fichiers générés par le Pipeline de contenu Monogame. Cela correspond à l’extension des fichiers générés par le Pipeline de contenu de Microsoft XNA.

Le **.xnb** extension est utilisée quel que soit le type de fichier d’origine. En d’autres termes, les fichiers image (**.png**), les fichiers audio (**.wav**), et les types de fichier personnalisés seront affichées en tant que **.xnb** lorsqu’il est passé à travers le pipeline de contenu des fichiers. Étant donné que l’extension ne peut pas être utilisée pour distinguer les différents formats de fichiers puis les méthodes CocosSharp et MonoGame qui chargent **.xnb** fichiers n’attendent pas les extensions lors du chargement du fichier.

Les fichiers de .xnb CocosSharp et MonoGame peuvent être créés à l’aide de l’outil Monogame Pipeline qui est abordée [dans cette procédure pas à pas](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


## <a name="summary"></a>Récapitulatif

Cet article a fourni une vue d’ensemble et les avantages des pipelines de contenu en général, ainsi qu’une introduction au Pipeline de contenu MonoGame.

## <a name="related-links"></a>Liens connexes

- [Documentation du Pipeline de MonoGame](http://www.monogame.net/documentation/?page=Pipeline)
