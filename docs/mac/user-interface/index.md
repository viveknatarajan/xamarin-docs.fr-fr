---
title: interface utilisateur de macOS
description: Cet article liens vers des guides qui décrivent les différents contrôles d’IU macOS.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: d40faa29f2fe278377bf4eae42a032f3dc9086ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="macos-user-interface"></a>interface utilisateur de macOS

_Cet article liens vers des guides qui décrivent les différents contrôles d’IU macOS._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès au même utilisateur interface qui contrôle un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos interfaces utilisateur (ou éventuellement de les créer directement dans le code C#).

Les guides ci-dessous donnent des informations détaillées sur l’utilisation des éléments d’interface utilisateur macOS dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utilisons dans chaque article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, comme décrit le `Register` et `Export` attributs utilisés pour la transmission des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

## <a name="windowsmacuser-interfacewindowmd"></a>[Fenêtres](~/mac/user-interface/window.md)

Cet article décrit l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Elle couvre la création et gestion de windows et dans Xcode et Interface générateur, le chargement de windows et les panneaux des fichiers .storyboard ou .xib, à l’aide de windows et répondre à windows en code c#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Boîtes de dialogue](~/mac/user-interface/dialog.md)

Cet article décrit l’utilisation des boîtes de dialogue et fenêtres modales dans une application Xamarin.Mac. Elle couvre la création et la gestion des fenêtres modales dans Xcode et Interface générateur, utilisation des boîtes de dialogue standard et afficher et répondre à windows en code c#.

## <a name="alertsmacuser-interfacealertmd"></a>[Alertes](~/mac/user-interface/alert.md)

Cet article décrit l’utilisation des alertes dans une application Xamarin.Mac. Elle couvre la création et affichage des alertes à partir de code c# et répondre aux alertes.

## <a name="menusmacuser-interfacemenumd"></a>[Menus](~/mac/user-interface/menu.md)

Les menus sont utilisés dans différentes parties de l’interface utilisateur de l’application d’un Mac ; à partir du menu principal de l’application en haut de l’écran pour les menus contextuels et des menus contextuels qui peuvent apparaître n’importe où dans une fenêtre. Menus font partie intégrante de l’expérience utilisateur de l’application d’un Mac. Cet article décrit l’utilisation de menus/Cocoa dans une application Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Contrôles standard](~/mac/user-interface/standard-controls.md)

Utilisation des contrôles tels que des boutons, des étiquettes, des champs de texte, des cases à cocher et des contrôles de segmentés dans une application Xamarin.Mac AppKit standard. Ce guide couvre les ajouter à une conception de l’interface utilisateur dans le Générateur de Xcode Interface, les exposer au code par le biais des prises de courant et les actions et l’utilisation de contrôles AppKit en code c#.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Barres d’outils](~/mac/user-interface/toolbar.md)

Cet article décrit l’utilisation des barres d’outils dans une application Xamarin.Mac. Elle couvre la création et la maintenance des barres d’outils dans Xcode et Interface générateur, comment exposer les éléments de barre d’outils au code à l’aide de sorties et les actions, l’activation et désactivation d’éléments de barre d’outils et enfin répond à des éléments de barre d’outils dans le code c#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Vues de table](~/mac/user-interface/table-view.md)

Cet article décrit l’utilisation des vues de table dans une application Xamarin.Mac. Elle couvre la création et la maintenance des vues de table dans Xcode et Interface générateur, comment exposer l’affichage de la table éléments de code à l’aide de sorties et les actions, remplissage de vues de table et la réponse à afficher les éléments de table dans le code c#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Mode plan](~/mac/user-interface/outline-view.md)

Cet article décrit l’utilisation des vues hiérarchiques dans une application Xamarin.Mac. Elle couvre la création et la maintenance des vues hiérarchiques dans Xcode et Interface générateur, comment exposer le mode plan éléments de code à l’aide de sorties et les actions, les vues hiérarchiques de remplissage et répond à l’ébauche d’afficher les éléments dans le code c#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Listes sources](~/mac/user-interface/source-list.md)

Cet article traite de l’utilisation de listes de sources dans une application Xamarin.Mac. Elle couvre la création et la gestion des listes de sources dans Xcode et Interface générateur, comment exposer les éléments de liste source au code à l’aide de sorties et les actions, remplissage listes sources et répondre aux éléments de liste de code source en code c#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Vues de collection](~/mac/user-interface/collection-view.md)

Cet article décrit l’utilisation des vues de collection dans une application Xamarin.Mac. Elle couvre la création et la maintenance des vues de collection dans Xcode et Interface générateur, pour exposer la vue de collection d’éléments de code à l’aide de sorties et les actions, remplissage de vues de collection et répondre aux vues de collection dans le code c#.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Création de contrôles personnalisés](~/mac/user-interface/custom-controls.md)

Cet article décrit la création des contrôles d’interface utilisateur personnalisée (en héritant de `NSControl`), une interface personnalisée pour le contrôle de dessin et la création d’actions personnalisées qui peuvent être utilisées avec le Générateur de Xcode Interface.

## <a name="mac-samples-gallery"></a>Galerie d’exemples Mac

Nous vous suggérons également aborder la [la galerie d’exemples Mac](https://developer.xamarin.com/samples/mac/all/). Il inclut une quantité de code prêts à l’emploi qui peut vous aider à rapidement un projet Xamarin.Mac rien.

## <a name="related-links"></a>Liens connexes

- [macOS indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
