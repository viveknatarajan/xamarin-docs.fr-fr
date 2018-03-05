---
title: Interface utilisateur
description: "Cet article liens vers des guides qui décrivent les différents contrôles d’IU macOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: a8cb9488849dafc2cd720ecf59d654009a9ad781
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface"></a>Interface utilisateur

_Cet article liens vers des guides qui décrivent les différents contrôles d’IU macOS._

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès à la même Interface utilisateur qui contrôle un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour créer et gérer vos Interfaces utilisateur (ou éventuellement de les créer directement dans le code C#). 

Les guides ci-dessous donnent des informations détaillées sur l’utilisation des éléments d’interface utilisateur macOS dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utilisons dans chaque article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` commandes permet de câble des vos classes c# pour les objets Objective-C et les éléments d’interface utilisateur.

## <a name="windowsmacuser-interfacewindowmd"></a>[Fenêtres](~/mac/user-interface/window.md)

Cet article décrit l’utilisation des fenêtres et des panneaux dans une application Xamarin.Mac. Elle couvre la création et la gestion des fenêtres et des panneaux dans le Générateur de Xcode et d’Interface, le chargement des fenêtres et des panneaux de `.storyboard` ou `.xib` fichiers, à l’aide de Windows et de répondre à Windows en code c#.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Boîtes de dialogue](~/mac/user-interface/dialog.md)

Cet article décrit l’utilisation des boîtes de dialogue et fenêtres modales dans une application Xamarin.Mac. Il aborde la création et la gestion des fenêtres modales dans Xcode et Interface Builder, l’utilisation des boîtes de dialogue standard, l’affichage des fenêtres et la réponse aux fenêtres en code C#.

## <a name="alertsmacuser-interfacealertmd"></a>[Alertes](~/mac/user-interface/alert.md)

Cet article décrit l’utilisation des alertes dans une application Xamarin.Mac. Il aborde la création et l’affichage des alertes à partir de code C# ainsi que la réponse aux alertes.

## <a name="menusmacuser-interfacemenumd"></a>[Menus](~/mac/user-interface/menu.md)

Les menus sont utilisés dans différentes parties de l’interface utilisateur de l’application d’un Mac ; à partir du menu principal de l’application en haut de l’écran pour les menus contextuels et indépendantes qui peuvent apparaître n’importe où dans une fenêtre. Les menus sont une partie intégrante de l’expérience utilisateur d’une application Mac. Cet article aborde l’utilisation des menus Cocoa dans une application Xamarin.Mac.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Contrôles standard](~/mac/user-interface/standard-controls.md)

Utilisation des contrôles AppKit standard tels que des boutons, des étiquettes, des champs de texte, des cases à cocher et des contrôles segmentés dans une application Xamarin.Mac. Ce guide couvre en les ajoutant à une conception de l’Interface utilisateur dans le Générateur de Xcode Interface, les exposer au code par le biais des prises de courant et les Actions et l’utilisation des contrôles AppKit en Code c#.

 
## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Barres d’outils](~/mac/user-interface/toolbar.md)

Cet article décrit l’utilisation des barres d’outils dans une application Xamarin.Mac. Il aborde la création et la gestion des barres d’outils dans Xcode et Interface Builder, l’exposition des éléments de barres d’outils au code à l’aide des outlets et actions, l’activation et la désactivation des éléments de barres d’outils, et enfin la réponse aux éléments de barres d’outils en code C#.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Vues Table](~/mac/user-interface/table-view.md)

Cet article décrit l’utilisation des vues de Table dans une application Xamarin.Mac. Elle couvre la création et la maintenance des vues de Table dans le Générateur de Xcode et d’Interface, comment exposer les éléments de la vue Table au code à l’aide de sorties et les Actions, remplissage de vues de Table et enfin répond pour afficher les éléments de Table dans le code c#.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Modes Plan](~/mac/user-interface/outline-view.md)

Cet article décrit le travail avec des vues hiérarchiques dans une application Xamarin.Mac. Il aborde la création et la gestion des modes Plan dans Xcode et Interface Builder, l’exposition des éléments des modes Plan au code à l’aide des outlets et actions, le remplissage des éléments des modes Plan et enfin la réponse aux éléments des modes Plan en code C#.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Listes de sources](~/mac/user-interface/source-list.md)

Cet article décrit le travail avec la Source des listes dans une application Xamarin.Mac. Il aborde la création et la gestion des listes de sources dans Xcode et Interface Builder, l’exposition des éléments des listes de sources au code à l’aide des outlets et actions, le remplissage des éléments des listes de sources et enfin la réponse aux éléments des listes de sources en code C#.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Vues de collection](~/mac/user-interface/collection-view.md)

Cet article décrit l’utilisation des vues de Collection dans une application Xamarin.Mac. Il aborde la création et la gestion des vues des collections dans Xcode et Interface Builder, l’exposition des éléments des vues des collections au code à l’aide des outlets et actions, le remplissage des vues des collections et enfin la réponse aux vues des collections en code C#.

## <a name="creating-custom-user-controlsmacuser-interfacecustom-controlsmd"></a>[Création de contrôles utilisateur personnalisés](~/mac/user-interface/custom-controls.md)

Cet article décrit la création de contrôles d’Interface utilisateur personnalisés (en héritant de `NSControl`), une interface personnalisée pour le contrôle de dessin et la création d’actions personnalisées qui peuvent être utilisées avec le Générateur de Xcode Interface.

## <a name="mac-samples-gallery"></a>Galerie d’exemples Mac

Nous suggérons également aborder la [Galerie d’exemples Mac](http://developer.xamarin.com/samples/mac/all/), il inclut une quantité de code prêts à l’emploi qui peut vous aider à rapidement un projet Xamarin.Mac rien.

## <a name="related-links"></a>Liens associés

- [macOS indications de l’Interface humaine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
