---
title: contrôles d’interface utilisateur macOS dans Xamarin.Mac
description: Ce document contient des liens vers des guides qui décrivent les différents contrôles d’interface utilisateur qui permettent aux développeurs de Xamarin.Mac. Contenu lié examine de windows, boîtes de dialogue, alertes, menus, barres d’outils, des vues de table, modes plan et bien plus encore.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: a12553cf0b7b9584bb8ff7bc04ed326ad4a7ad2a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107831"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>contrôles d’interface utilisateur macOS dans Xamarin.Mac

_Cette article fournit des directives qui décrivent les différents contrôles d’interface utilisateur macOS._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à l’utilisateur d’interface qui contrôle un développeur travaillant *Objective-C* et *Xcode* est. Comme Xamarin.Mac s’intègre directement à Xcode, vous pouvez utiliser de Xcode _Interface Builder_ pour créer et gérer vos interfaces utilisateur (ou éventuellement pour les créer directement en code c#).

Les guides ci-dessous donnent des informations détaillées sur l’utilisation des éléments d’interface utilisateur macOS dans une application Xamarin.Mac. Il est fortement recommandé que vous travaillez via le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) et [Outlets et Actions](~/mac/get-started/hello-mac.md#outlets-and-actions) sections, tel qu’il couvre les principaux concepts et techniques que nous allons utiliser dans chaque article.

Vous pouvez souhaiter de jeter un coup de œil à la [C# exposition de classes / méthodes vers Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) section de la [éléments internes de Xamarin.Mac](~/mac/internals/how-it-works.md) de document, car il explique le `Register` et `Export` attributs utilisés pour la communication à distance vos classes c# aux objets de Objective-C et des éléments d’interface utilisateur.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

Cet article aborde l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Il aborde la création et gestion des fenêtres et des panneaux dans Xcode et Interface Builder, le chargement de windows et des panneaux à partir de fichiers .storyboard ou .xib, à l’aide de windows et réponse aux fenêtres en code c#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Boîtes de dialogue](~/mac/user-interface/dialog.md)

Cet article aborde l’utilisation des boîtes de dialogue et fenêtres modales dans une application Xamarin.Mac. Il aborde la création et gestion des fenêtres modales dans Xcode et Interface Builder, utilisation des boîtes de dialogue standard et d’affichage et de réponse aux fenêtres en code c#.

## <a name="alertsmacuser-interfacealertmd"></a>[Alertes](~/mac/user-interface/alert.md)

Cet article aborde l’utilisation des alertes dans une application Xamarin.Mac. Il couvre la création et affichage des alertes à partir de code c# et répondre aux alertes.

## <a name="menusmacuser-interfacemenumd"></a>[Menus](~/mac/user-interface/menu.md)

Les menus sont utilisés dans différentes parties de l’interface utilisateur d’une application Mac à partir du menu principal de l’application en haut de l’écran pour les menus déroulants et menus contextuels qui peuvent apparaître n’importe où dans une fenêtre. Menus font partie intégrante de l’expérience utilisateur d’une application Mac. Cet article aborde l’utilisation des menus Cocoa dans une application Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Contrôles standard](~/mac/user-interface/standard-controls.md)

Fonctionne avec les contrôles AppKit standards tels que des boutons, des étiquettes, des champs de texte, des cases à cocher et des contrôles segmentés dans une application Xamarin.Mac. Ce guide couvre les ajouter à une conception d’interface utilisateur dans l’Interface Builder de Xcode, les exposer au code via les outlets et actions et l’utilisation de contrôles AppKit en code c#.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Barres d’outils](~/mac/user-interface/toolbar.md)

Cet article aborde l’utilisation des barres d’outils dans une application Xamarin.Mac. Il aborde la création et la maintenance des barres d’outils dans Xcode et Interface Builder, comment exposer les éléments de barre d’outils au code à l’aide des outlets et actions, l’activation et désactivation des éléments de barre d’outils et enfin la réponse aux éléments de barre d’outils en code c#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Vues de table](~/mac/user-interface/table-view.md)

Cet article aborde l’utilisation des vues de table dans une application Xamarin.Mac. Il aborde la création et la gestion des vues de table dans Xcode et Interface Builder, comment exposer la vue de la table éléments de code à l’aide des outlets et actions, remplissant les vues de table et la réponse aux éléments de vue de table dans le code c#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Modes plan](~/mac/user-interface/outline-view.md)

Cet article aborde l’utilisation des modes plan dans une application Xamarin.Mac. Il aborde la création et gestion des modes plan dans Xcode et Interface Builder, comment exposer le mode plan éléments de code à l’aide des outlets et actions, remplissant des modes plan et répondre à l’ébauche d’afficher les éléments dans le code c#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Listes sources](~/mac/user-interface/source-list.md)

Cet article aborde l’utilisation des listes de sources dans une application Xamarin.Mac. Il aborde la création et la gestion des listes de sources dans Xcode et Interface Builder, l’exposition des éléments de liste source au code à l’aide des outlets et actions, le remplissage des listes sources et la réponse aux éléments de liste source en code c#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Vues de collection](~/mac/user-interface/collection-view.md)

Cet article aborde l’utilisation des vues de collection dans une application Xamarin.Mac. Il aborde la création et la gestion des vues de collection dans Xcode et Interface Builder, comment exposer la vue de collection éléments de code à l’aide des outlets et actions, le remplissage des vues de collection et réponse aux vues de collection dans le code c#.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Création de contrôles personnalisés](~/mac/user-interface/custom-controls.md)

Cet article aborde la création des contrôles d’interface utilisateur personnalisée (en héritant de `NSControl`), une interface personnalisée pour le contrôle de dessin et la création d’actions personnalisées qui peuvent être utilisées avec Interface Builder de Xcode.

## <a name="mac-samples-gallery"></a>Galerie d’exemples Mac

Nous vous suggérons également de jeter un coup de œil à la [Galerie d’exemples Mac](https://developer.xamarin.com/samples/mac/all/). Il comprend une multitude de code prêts à l’emploi qui permettre vous aider à un projet Xamarin.Mac décoller rapidement.

## <a name="related-links"></a>Liens connexes

- [macOS Human Interface Guidelines pour](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
