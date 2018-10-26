---
title: Parties de ListView et fonctionnalité
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 248baa5daceff6db01098a155600ea204547e845
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116110"
---
# <a name="listview-parts-and-functionality"></a>Parties de ListView et fonctionnalité


## <a name="overview"></a>Vue d'ensemble

Un `ListView` se compose des éléments suivants :

- **Lignes** &ndash; la représentation des données dans la liste visible.

- **Adaptateur** &ndash; une classe non visuel qui lie la source de données à la vue de liste.

- **Défilement rapide** &ndash; un handle qui permet à l’utilisateur de faire défiler la longueur de la liste.

- **Index de section** &ndash; lignes d’un élément d’interface utilisateur qui flotte au-dessus le défilement pour indiquer où se trouvent les lignes en cours dans la liste.

Ces captures d’écran utilisent un base `ListView` contrôle pour afficher le mode de rendu de défilement rapide et l’Index de Section :

[![Captures d’écran des applications à l’aide des lignes anciennes simples, rapide le défilement et les index de la section](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Les éléments qui composent un `ListView` sont décrits plus en détail ci-dessous :


## <a name="rows"></a>Lignes

Chaque ligne possède son propre `View`. La vue peut être l’un des affichages prédéfinis définis dans `Android.Resources`, ou une vue personnalisée. Chaque ligne peut utiliser la même disposition de vue, ou ils peuvent tous être différents. Il existe des exemples dans ce document de l’utilisation de dispositions intégrées et autres qui explique comment définir des dispositions personnalisées.


## <a name="adapter"></a>Adaptateur

Le `ListView` contrôle nécessite un `Adapter` pour fournir la mise en forme `View` pour chaque ligne. Android a intégré des adaptateurs et des vues qui peuvent être utilisés, ou les classes personnalisées peuvent être créées.


## <a name="fast-scrolling"></a>Le défilement rapide

Quand un `ListView` contient de nombreuses lignes de données le défilement rapide peut être activé pour aider l’utilisateur à accéder à n’importe quelle partie de la liste. Le fast-défilement « barre de défilement » peut être éventuellement activée (et personnalisés dans le niveau d’API 11 et versions supérieures).


## <a name="section-index"></a>Index de section

Pendant le défilement de longues listes, l’index de la section facultative fournit à l’utilisateur avec des commentaires sur quelle partie de la liste actuellement visualisé. Il n’est plus approprié sur longues listes, généralement en conjonction avec le défilement rapide.


## <a name="classes-overview"></a>Vue d’ensemble de classes

Classes principales utilisées pour afficher `ListViews` sont indiquées ici :

[![Diagramme UML illustrant les relations entre ListView et des classes associées](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

L’objectif de chaque classe est décrit ci-dessous :

- **ListView** &ndash; élément d’interface utilisateur qui affiche une collection de lignes de défilement. Sur les téléphones il généralement utilise tout l’écran (auquel cas la `ListActivity` classe peut être utilisée) ou il peut faire partie d’une mise en page plus grand sur les téléphones ou tablettes.

- **Vue** &ndash; une vue dans Android peut être n’importe quel élément d’interface utilisateur, mais dans le contexte d’un `ListView` il nécessite un `View` doit être fourni pour chaque ligne.

- **BaseAdapter** &ndash; classe de Base pour les implémentations d’adaptateur lier un `ListView` à une source de données.

- **ArrayAdapter** &ndash; classe adaptateur intégré qui lie un tableau de chaînes à un `ListView` pour l’affichage. Le modèle générique `ArrayAdapter<T>` fait de même pour les autres types.

- **CursorAdapter** &ndash; utilisation `CursorAdapter` ou `SimpleCursorAdapter` pour afficher des données basées sur une requête SQLite.

Ce document contient des exemples simples qui utilisent un `ArrayAdapter` , ainsi que des exemples plus complexes qui nécessitent des implémentations personnalisées des `BaseAdapter` ou `CursorAdapter`.

