---
title: Introduction aux pipelines de contenu
description: Les pipelines sont des applications ou des parties d’applications de contenu, qui servent à convertir des fichiers dans un format qui peut être chargé par les projets de jeux. Le Pipeline de contenu MonoGame est une implémentation de pipeline de contenu spécifiques pour la conversion de fichiers pour les projets CocosSharp et MonoGame.
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 712c430fb6309ba0f5c3e573267c59e422de8ad2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61029522"
---
# <a name="introduction-to-content-pipelines"></a>Introduction aux pipelines de contenu

_Les pipelines sont des applications ou des parties d’applications de contenu, qui servent à convertir des fichiers dans un format qui peut être chargé par les projets de jeux. Le Pipeline de contenu MonoGame est une implémentation de pipeline de contenu spécifiques pour la conversion de fichiers pour les projets CocosSharp et MonoGame._

Cet article explique le concept des pipelines de contenu, principalement en mettant l’accent sur la *MonoGame Content Pipeline*, qui est une implémentation de pipeline de contenu utilisée avec CocosSharp et MonoGame.


## <a name="what-is-a-content-pipeline"></a>Qu’est un pipeline de contenu ?

Le terme *pipeline de contenu* est un terme général qui désigne le processus de conversion d’un fichier d’un format vers un autre. Le *d’entrée* du pipeline de contenu est généralement un fichier générée par un outil de création, comme les fichiers image à partir de Photoshop. Le pipeline de contenus crée le *sortie* fichier dans un format qui peut être chargé directement par un projet de jeu. En général, les fichiers de sortie sont optimisés pour un chargement rapide et réduit la taille du disque.

Contenu de pipelines peuvent être en ligne de commande exécutables, les applications GUI dédiées ou plug-ins incorporé dans une autre application, tels que Visual Studio. Pipelines de contenu s’exécutent généralement avant que le jeu s’exécute. Si le pipeline de contenu est associé à une application, tels que Visual Studio, puis il peut s’exécuter pendant la compilation. Si le pipeline de contenus est une application autonome, puis l’exécution lorsque explicitement dit à le faire par l’utilisateur. L’application ou une logique chargée de convertir un fichier d’entrée spécifique (comme un **.png**) vers une sortie associée fichier est appelé un *Générateur*. 

Nous pouvons visualiser le chemin d’accès nécessaire à partir de la création en cours de chargement comme suit lors de l’exécution d’un fichier :

![](introduction-images/image1.png "Le chemin d’accès nécessaire à partir de la création en cours de chargement lors de l’exécution d’un fichier n’est affichée dans ce diagramme")

## <a name="why-use-a-content-pipeline"></a>Pourquoi utiliser un pipeline de contenu ?

Pipelines de contenus introduisent une étape supplémentaire entre l’application de création et le jeu, ce qui peut augmenter les délais de compilation et compliquer le processus de développement. En dépit de ces considérations, les pipelines contenus introduisent un certain nombre d’avantages au développement de jeux :


### <a name="converting-to-a-format-understood-by-the-game"></a>Conversion en un format compris par le jeu

CocosSharp et MonoGame fournissent des méthodes pour le chargement des différents types de contenu ; Toutefois, le contenu doit être formaté correctement avant leur chargement. La plupart des types de contenu nécessitent un certain type de conversion avant leur chargement. Par exemple, les effets sonores dans le **.wav** format doit être converti en un **.xnb** fichier à charger lors de l’exécution dans la mesure où CocosSharp et MonoGame ne prennent pas en charge le chargement du **.wav** format de fichier.


### <a name="converting-to-a-format-native-to-the-hardware"></a>Conversion en un format natif pour le matériel

Un matériel différent peut traiter un contenu différemment lors de l’exécution. Par exemple, les jeux de CocosSharp peuvent charger des fichiers d’image lors de la création un `CCSprite` instance. Bien que le même code peut être utilisé pour charger les fichiers sur iOS et Android, chaque plateforme stocke le fichier chargé différemment. Par conséquent, le Pipeline de contenus MonoGame formats de texture **.xnb** fichiers différemment en fonction de la plateforme cible.


### <a name="reducing-size-on-disk"></a>Réduction de taille sur disque 

Pipelines peuvent être utilisés pour supprimer les informations de contenu, qui est utile au moment de l’auteur mais n’est pas nécessaire lors de l’exécution. Le fichier d’origine (entrée) peut stocker toutes les informations qui peuvent aider les créateurs de contenu à mettre à jour le contenu existant, mais le fichier de sortie peut être simplifiée pour minimiser le fichier global du jeu. Cette considération est particulièrement utile pour les jeux mobiles qui sont téléchargés et non distribuées sur le support d’installation.


### <a name="reducing-load-time"></a>Ce qui réduit le temps de chargement

Jeux peuvent nécessiter des modifications de contenu pour améliorer les performances d’exécution, pour améliorer les éléments visuels, ou pour ajouter de nouvelles fonctionnalités. Par exemple, de nombreux jeux 3D calculer l’éclairage d’une seule fois, puis utilisent le résultat de ce calcul lors du rendu de scènes complexes. Depuis la réalisation de ces calculs lors du chargement du contenu peut être prohibitif le calcul peut être effectué à la place lorsque le jeu est généré. Les calculs qui en résulte peuvent être inclus dans le contenu, l’activation du contenu à charger beaucoup plus rapidement que ne le possible. 


## <a name="xnb-file-extension"></a>extension de fichier xnb

Le **.xnb** extension de fichier est l’extension pour tous les fichiers générés par le Pipeline de contenus Monogame. Cela correspond à l’extension des fichiers générés par le Pipeline de contenu de Microsoft XNA.

Le **.xnb** extension est utilisée, quel que soit le type de fichier d’origine. En d’autres termes, les fichiers image (**.png**), fichiers audio (**.wav**), et les types de fichiers personnalisés seront générés en tant que **.xnb** quand il est passé à travers le pipeline de contenu des fichiers. Étant donné que l’extension ne peut pas être utilisée pour faire la distinction entre différents formats de fichier puis CocosSharp et MonoGame des méthodes qui chargent **.xnb** fichiers n’attendent pas les extensions lors du chargement du fichier.

Les fichiers de .xnb CocosSharp et MonoGame peuvent être créés à l’aide de l’outil de Pipeline de Monogame qui est abordé [dans cette procédure pas à pas](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


## <a name="summary"></a>Récapitulatif

Cet article fournit une vue d’ensemble et les avantages de pipelines de contenu en général, ainsi qu’une introduction au Pipeline de contenu MonoGame.

## <a name="related-links"></a>Liens connexes

- [Documentation du Pipeline de MonoGame](http://www.monogame.net/documentation/?page=Pipeline)
