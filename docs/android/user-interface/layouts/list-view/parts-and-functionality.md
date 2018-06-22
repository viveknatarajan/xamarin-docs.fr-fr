---
title: Fonctionnalités et composants de ListView
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: ec9b6583cac9478957e14f709c7c7ee8eb273cbc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763980"
---
# <a name="listview-parts-and-functionality"></a>Fonctionnalités et composants de ListView


## <a name="overview"></a>Vue d'ensemble

A `ListView` se compose des éléments suivants :

- **Lignes** &ndash; la représentation visible des données dans la liste.

- **Carte** &ndash; une classe non visuel qui lie la source de données à la vue liste.

- **Défilement rapide** &ndash; un descripteur qui permet à l’utilisateur de faire défiler la longueur de la liste.

- **Index de section** &ndash; un élément d’interface utilisateur qui flotte au-dessus de la faire défiler les lignes pour indiquer où se trouvent les lignes en cours dans la liste.

Ces captures d’écran utilisent un basic `ListView` contrôle pour afficher le mode de rendu de défilement rapide et l’Index de Section :

[![Captures d’écran des applications utilisant les lignes anciennes simples, rapide le défilement et l’index de section](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Les éléments qui composent un `ListView` sont décrits plus en détail ci-dessous :


## <a name="rows"></a>Lignes

Chaque ligne possède son propre `View`. La vue peut être l’un des affichages prédéfinis défini dans `Android.Resources`, ou un affichage personnalisé. Chaque ligne peut utiliser la même disposition de la vue, ou ils peuvent tous être différents. Il existe des exemples dans ce document de l’utilisation des dispositions intégrées et autres expliquant comment définir des dispositions personnalisées.


## <a name="adapter"></a>Adaptateur

Le `ListView` contrôle requiert une `Adapter` pour fournir la mise en forme `View` pour chaque ligne. Android a intégré des cartes et des vues qui peuvent être utilisés, ou les classes personnalisées peuvent être créées.


## <a name="fast-scrolling"></a>Défilement rapide

Lorsqu’un `ListView` contient de nombreuses lignes de données défilement rapide peut être activé pour aider l’utilisateur à accéder à n’importe quelle partie de la liste. Le fast-défilement « barre de défilement » peut être éventuellement activé (et personnalisées au niveau de l’API 11 et versions ultérieures).


## <a name="section-index"></a>Index de section

Pendant le défilement de longues listes, l’index de section facultative fournit à l’utilisateur avec des commentaires sur la partie de la liste actuellement visualisé. Il n’est plus approprié sur longues listes, généralement en association avec un défilement rapide.


## <a name="classes-overview"></a>Vue d’ensemble des classes

Classes principales utilisées pour afficher les `ListViews` sont indiquées ici :

[![Diagramme UML illustrant les relations entre ListView et des classes associées](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

L’objectif de chaque classe est décrite ci-dessous :

- **ListView** &ndash; élément d’interface utilisateur qui affiche une collection de lignes de défilement. Sur les téléphones il généralement utilise tout l’écran (dans ce cas, la `ListActivity` classe peut être utilisée) ou il peut faire partie d’une mise en page plus volumineux sur les téléphones ou les tablettes.

- **Vue** &ndash; une vue dans Android peut être n’importe quel élément d’interface utilisateur, mais dans le contexte d’un `ListView` il nécessite un `View` doit être fourni pour chaque ligne.

- **BaseAdapter** &ndash; classe de Base pour les implémentations d’adaptateur lier un `ListView` à une source de données.

- **ArrayAdapter** &ndash; classe adaptateur intégré qui lie un tableau de chaînes à un `ListView` pour l’affichage. Le type générique `ArrayAdapter<T>` fait de même pour les autres types.

- **CursorAdapter** &ndash; utilisez `CursorAdapter` ou `SimpleCursorAdapter` pour afficher des données basées sur une requête SQLite.

Ce document contient des exemples simples qui utilisent une `ArrayAdapter` , ainsi que des exemples plus complexes qui requièrent des implémentations personnalisées de `BaseAdapter` ou `CursorAdapter`.

