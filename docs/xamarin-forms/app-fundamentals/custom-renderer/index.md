---
title: Convertisseurs personnalisés de Xamarin.Forms
description: Les convertisseurs personnalisés permettent aux développeurs de substituer le rendu des contrôles natifs sur chaque plateforme, pour personnaliser l’apparence et le comportement des contrôles de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a88462052906e68fd85a07161e8b5bb63a61e69d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239888"
---
# <a name="xamarinforms-custom-renderers"></a>Convertisseurs personnalisés de Xamarin.Forms

_Interfaces utilisateur de Xamarin.Forms sont rendus à l’aide des contrôles natifs de la plateforme cible, qui permet aux applications de Xamarin.Forms conserver l’apparence appropriée pour chaque plateforme. Les convertisseurs personnalisés permettent aux développeurs de substituer ce processus pour personnaliser l’apparence et le comportement des contrôles de Xamarin.Forms sur chaque plateforme._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Introduction aux convertisseurs personnalisés](introduction.md)

Les convertisseurs personnalisés fournissent une approche puissante pour personnaliser l’apparence et le comportement des contrôles de Xamarin.Forms. Ils peuvent être utilisés pour les modifications de conception de styles de petite ou sophistiquée spécifique à la plateforme disposition et personnalisation du comportement. Cet article fournit une introduction aux convertisseurs personnalisés et décrit le processus de création d’un convertisseur personnalisé.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Classes de base de convertisseur et des contrôles natifs](renderers.md)

Chaque contrôle Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Cet article répertorie les classes de contrôle natif qui implémentent chaque page Xamarin.Forms, disposition, afficher et cellule et le convertisseur.

## <a name="customizing-an-entryentrymd"></a>[Personnalisation d’une entrée](entry.md)

Le Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle permet à une seule ligne de texte à modifier. Cet article explique comment créer un convertisseur personnalisé pour le `Entry` (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Personnalisation d’une page de contenu](contentpage.md)

A [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran. Cet article explique comment créer un convertisseur personnalisé pour le `ContentPage` page, permettant ainsi aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme.

## <a name="customizing-a-mapmapindexmd"></a>[Personnalisation d’une carte](map/index.md)

Xamarin.Forms.Maps fournit une abstraction de multiplateforme pour l’affichage des cartes qui utilisent le mappage natif rencontrer des API sur chaque plateforme, pour fournir une carte rapide et familière pour les utilisateurs. Cette rubrique montre comment créer des convertisseurs personnalisés pour le `Map` (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme.

## <a name="customizing-a-listviewlistviewmd"></a>[Personnalisation d’un ListView](listview.md)

Un Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est une vue qui affiche une collection de données sous la forme d’une liste verticale. Cet article explique comment créer un convertisseur personnalisé qui encapsule des contrôles de liste de spécifique à la plateforme et des dispositions de cellule native, ce qui permet de mieux contrôler les performances du contrôle de liste natif.

## <a name="customizing-a-viewcellviewcellmd"></a>[Personnalisation d’une ViewCell](viewcell.md)

Un Xamarin.Forms [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) est une cellule qui peut être ajoutée à un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ou [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), qui contient une vue définie par le développeur. Cet article explique comment créer un convertisseur personnalisé pour un `ViewCell` qui est hébergé à l’intérieur d’un Xamarin.Forms `ListView` contrôle. Cela arrête les calculs de disposition Xamarin.Forms à partir de l’appel à plusieurs reprises au cours de `ListView` le défilement.

## <a name="implementing-a-viewviewmd"></a>[Implémentation d’un affichage](view.md)

Contrôles d’interfaces Xamarin.Forms utilisateur personnalisé doivent dériver de la [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) (classe), qui est utilisé pour placer des mises en page et des contrôles sur l’écran. Cet article explique comment créer un convertisseur personnalisé pour un contrôle personnalisé Xamarin.Forms qui est utilisé pour afficher un flux vidéo de la version d’évaluation à partir photo de l’appareil.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implémentation d’un HybridWebView](hybridwebview.md)

Cet article explique comment créer un convertisseur personnalisé pour un `HybridWebView` un contrôle personnalisé, qui montre comment améliorer les contrôles spécifiques à une plateforme web pour autoriser le code c# pour être appelée à partir de JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implémentation d’un lecteur vidéo](video-player/index.md)

Cet article explique comment écrire des convertisseurs pour implémenter un `VideoPlayer` contrôle qui peut lire des vidéos à partir du web, les vidéos incorporées en tant que ressources de l’application ou vidéos stockées dans la bibliothèque de vidéo sur l’appareil de l’utilisateur. Plusieurs techniques sont présentées, y compris la mise en œuvre des méthodes et des propriétés pouvant être liées en lecture seule.


## <a name="related-links"></a>Liens associés

- [Effets](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Convertisseurs personnalisés (vidéo de l’université Xamarin)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Exemple de convertisseurs personnalisés (vidéo de l’université Xamarin)](http://bit.ly/xf-customrenderer)
