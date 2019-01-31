---
title: Fonctionnalités de la plateforme Android
description: Cet article explique comment ajouter des fonctionnalités Android propres aux applications de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2018
ms.openlocfilehash: a90ae27bb4e0085d1cd0c5b36cf7c00fe5ebfec6
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55292218"
---
# <a name="android-platform-features"></a>Fonctionnalités de la plateforme Android

Développement d’applications Xamarin.Forms pour Android nécessite Visual Studio. Le [page spécifications](~/get-started/installation.md) contient plus d’informations sur les conditions préalables.

## <a name="platform-specifics"></a>Caractéristiques de la plateforme

Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour les vues Xamarin.Forms, les pages et les dispositions sur Android :

- Contrôler l’ordre de plan des éléments visuels pour déterminer l’ordre de dessin. Pour plus d’informations, consultez [VisualElement l’élévation sur Android](visualelement-elevation.md).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [VisualElement Mode de couleur hérité sur Android](legacy-color-mode.md).

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour les vues Xamarin.Forms sur Android :

- À l’aide de la marge intérieure par défaut et les valeurs de clichés instantanés des boutons Android. Pour plus d’informations, consultez [bouton remplissage et les ombres sur Android](button-padding-shadow.md).
- Définition des options de l’éditeur pour le clavier logiciel pour la méthode d’entrée une [ `Entry` ](xref:Xamarin.Forms.Entry). Pour plus d’informations, consultez [Options d’éditeur de méthode d’entrée entrée sur Android](entry-ime-options.md).
- L’activation d’une ombre sur un `ImageButton`. Pour plus d’informations, consultez [ImageButton ombres sur Android](imagebutton-drop-shadow.md).
- Activer le défilement rapide dans un [ `ListView` ](xref:Xamarin.Forms.ListView) pour plus d’informations, consultez [ListView le défilement rapide sur Android](listview-fast-scrolling.md).
- Contrôle si un [ `WebView` ](xref:Xamarin.Forms.WebView) peut afficher un contenu mixte. Pour plus d’informations, consultez [WebView contenu mixte sur Android](webview-mixed-content.md).

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour les pages Xamarin.Forms sur Android :

- Définition de la hauteur de la barre de navigation sur un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Pour plus d’informations, consultez [NavigationPage hauteur de la barre sur Android](navigationpage-bar-height.md).
- La désactivation des animations de transition en parcourant les pages dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Pour plus d’informations, consultez [TabbedPage les Animations de Transition de Page sur Android](tabbedpage-transition-animations.md).
- L’activation de glissement entre les pages dans un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Pour plus d’informations, consultez [TabbedPage Page balayant sur Android](tabbedpage-page-swiping.md).
- Définition de la position de la barre d’outils et les couleurs sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Pour plus d’informations, consultez [TabbedPage barre d’outils de Placement et couleur sur Android](tabbedpage-toolbar-placement-color.md).

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) classe sur Android :

- Définition du mode de fonctionnement d’un clavier logiciel. Pour plus d’informations, consultez [réversible Mode d’entrée de clavier sur Android](soft-keyboard-input-mode.md).
- La désactivation de la [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) et [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) page événements de cycle de vie sur Suspendre et reprendre respectivement, pour les applications qui utilisent AppCompat. Pour plus d’informations, consultez [événements de cycle de vie de Page sur Android](page-lifecycle-events.md).

## <a name="platform-support"></a>Plateforme prise en charge

À l’origine, la valeur par défaut du projet Xamarin.Forms Android utilisé un ancien style de rendu du contrôle qui était courant avant Android 5.0. Les applications générées à l’aide du modèle ont `FormsApplicationActivity` comme classe de base de leur activité principale.

## <a name="material-design-via-appcompat"></a>Conception matérielle via AppCompat

Utilisent désormais les projets Android de Xamarin.Forms `FormsAppCompatActivity` comme classe de base de leur activité principale. Cette classe utilise **AppCompat** fonctionnalités fournies par Android pour implémenter des thèmes de Material Design.

Pour ajouter des thèmes de Material Design à votre projet Android de Xamarin.Forms, suivez le [prennent en charge des instructions d’installation pour AppCompat](appcompat-material-design.md)

Voici le **Todo** exemple avec la valeur par défaut `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "Application exemple TODO sans AppCompat")](images/before-appcompat.png#lightbox "Application exemple Todo sans AppCompat")

Et c’est le même code après la mise à niveau le projet à utiliser `FormsAppCompatActivity` (et en ajoutant les informations de thème supplémentaires) :

[![](images/post-appcompat-sml.png "Application exemple TODO avec AppCompat et des thèmes")](images/post-appcompat.png#lightbox "Application exemple Todo avec AppCompat et des thèmes")

> [!NOTE]
> Lorsque vous utilisez `FormsAppCompatActivity`, le [classes de base pour certains convertisseurs personnalisées Android](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) sera différente.

## <a name="related-links"></a>Liens connexes

- [Ajouter la prise en charge de la conception matérielle](appcompat-material-design.md)
