---
title: Renderers personnalisés de Xamarin.Forms
description: Renderers personnalisés permettent aux développeurs de substituer le rendu des contrôles natifs sur chaque plateforme, pour personnaliser l’apparence et le comportement des contrôles Xamarin.Forms.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998741"
---
# <a name="xamarinforms-custom-renderers"></a>Renderers personnalisés de Xamarin.Forms

_Interfaces utilisateur de Xamarin.Forms sont rendus à l’aide de contrôles natifs de la plateforme cible, qui permet aux applications Xamarin.Forms de conserver l’apparence appropriée pour chaque plateforme. Renderers personnalisés permettent aux développeurs de substituer ce processus pour personnaliser l’apparence et le comportement des contrôles Xamarin.Forms sur chaque plateforme._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Introduction aux convertisseurs personnalisés](introduction.md)

Renderers personnalisés fournissent une approche puissante permettant de personnaliser l’apparence et le comportement des contrôles Xamarin.Forms. Elles peuvent servir pour les styles de petites modifications ou sophistiquée disposition spécifique à la plateforme et personnalisation du comportement. Cet article fournit une introduction aux convertisseurs personnalisés et décrit le processus de création d’un convertisseur personnalisé.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Les classes de base de convertisseur et contrôles natifs](renderers.md)

Chaque contrôle Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Cet article répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms page, disposition, vue et la cellule et le convertisseur.

## <a name="customizing-an-entryentrymd"></a>[Personnalisation d’une entrée](entry.md)

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle permet à une seule ligne de texte à modifier. Cet article montre comment créer un convertisseur personnalisé pour le `Entry` (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Personnalisation d’une page de contenu](contentpage.md)

Un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran. Cet article montre comment créer un convertisseur personnalisé pour le `ContentPage` page, permettant ainsi aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme.

## <a name="customizing-a-mapmapindexmd"></a>[Personnalisation d’une carte](map/index.md)

Xamarin.Forms.Maps fournit une abstraction d’inter-plateformes pour l’affichage des mappages qui utilisent le mappage natif expérience API sur chaque plateforme, pour fournir une carte rapide et familier pour les utilisateurs. Cette rubrique montre comment créer des convertisseurs personnalisés pour le `Map` (contrôle), permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme.

## <a name="customizing-a-listviewlistviewmd"></a>[Personnalisation d’un ListView](listview.md)

Un Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) est une vue qui affiche une collection de données sous la forme d’une liste verticale. Cet article montre comment créer un convertisseur personnalisé qui encapsule des contrôles de liste de spécifique à la plateforme et les dispositions de cellule native, ce qui permet de mieux contrôler les performances du contrôle de liste natif.

## <a name="customizing-a-viewcellviewcellmd"></a>[Personnalisation d’une ViewCell](viewcell.md)

Un Xamarin.Forms [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) est une cellule qui peut être ajoutée à un [ `ListView` ](xref:Xamarin.Forms.ListView) ou [ `TableView` ](xref:Xamarin.Forms.TableView), qui contient une vue définie par le développeur. Cet article montre comment créer un convertisseur personnalisé pour un `ViewCell` qui est hébergé à l’intérieur d’un Xamarin.Forms `ListView` contrôle. Cela arrête les calculs de dispositions Xamarin.Forms d’être appelée à plusieurs reprises au cours de `ListView` le défilement.

## <a name="implementing-a-viewviewmd"></a>[Implémentation d’un affichage](view.md)

Contrôles d’interfaces Xamarin.Forms utilisateur personnalisé doivent dériver de la [ `View` ](xref:Xamarin.Forms.View) (classe), qui est utilisé pour placer des dispositions et des contrôles sur l’écran. Cet article montre comment créer un convertisseur personnalisé pour un contrôle personnalisé Xamarin.Forms qui est utilisé pour afficher un flux vidéo d’aperçu à partir de photo l’appareil.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implémentation d’un HybridWebView](hybridwebview.md)

Cet article montre comment créer un convertisseur personnalisé pour un `HybridWebView` contrôle personnalisé, qui montre comment améliorer les contrôles spécifiques à la plateforme web pour autoriser le code c# à appeler à partir de JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implémentation d’un lecteur vidéo](video-player/index.md)

Cet article montre comment écrire des convertisseurs pour implémenter un `VideoPlayer` contrôle qui peut lire des vidéos à partir du web, les vidéos incorporées en tant que ressources de l’application ou des vidéos stockées dans la bibliothèque vidéo sur l’appareil de l’utilisateur. Plusieurs techniques sont présentées, y compris la mise en œuvre de méthodes et propriétés pouvant être liées en lecture seule.


## <a name="related-links"></a>Liens associés

- [Effets](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Renderers personnalisés (vidéo Xamarin University)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Exemple de convertisseurs personnalisés (vidéo Xamarin University)](http://bit.ly/xf-customrenderer)
