---
title: Fonctionnalités de la plateforme Windows
description: Cet article explique la prise en charge de plate-forme pour Windows est disponible dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
ms.openlocfilehash: 9367e22ede733ee2d93feccc001836fb4dc02564
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507108"
---
# <a name="windows-platform-features"></a>Fonctionnalités de la plateforme Windows

Développement d’applications Xamarin.Forms pour les plateformes Windows requiert Visual Studio. Le [page spécifications](~/get-started/requirements.md) contient plus d’informations sur les conditions préalables.

![](images/allhanselman.png "Applications Xamarin.Forms exécutées sur Windows")

## <a name="platform-specifics"></a>Caractéristiques de la plateforme

Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets.

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour les vues Xamarin.Forms, les pages et les dispositions sur la plateforme Windows universelle (UWP) :

- Définition d’une clé d’accès pour un [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [VisualElement clés d’accès Windows](visualelement-access-keys.md).
- Désactiver le mode hérité de couleur sur la prise en charge [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Pour plus d’informations, consultez [VisualElement Mode de couleur hérité sur Windows](legacy-color-mode.md).

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour les vues Xamarin.Forms sur UWP :

- Détection de l’ordre de lecture à partir du texte contenu dans [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), et [ `Label` ](xref:Xamarin.Forms.Label) instances. Pour plus d’informations, consultez [InputView l’ordre de lecture sur Windows](inputview-reading-order.md).
- L’activation de la prise en charge des mouvements de drainage dans un [ `ListView` ](xref:Xamarin.Forms.ListView). Pour plus d’informations, consultez [ListView SelectionMode sur Windows](listview-selectionmode.md).
- L’activation une [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) pour interagir avec le moteur de vérification orthographique. Pour plus d’informations, consultez [SearchBar le vérificateur orthographique sur Windows](searchbar-spell-check.md).
- L’activation une [ `WebView` ](xref:Xamarin.Forms.WebView) pour afficher les alertes de JavaScript dans une boîte de dialogue de message UWP. Pour plus d’informations, consultez [WebView JavaScript alertes sur Windows](webview-javascript-alert.md).

Les fonctionnalités spécifiques à la plateforme suivantes sont fournies pour les pages Xamarin.Forms sur UWP :

- Réduire le [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) barre de navigation. Pour plus d’informations, consultez [MasterDetailPage barre de Navigation sur Windows](masterdetailpage-navigation-bar.md).
- Définition des options de placement de barre d’outils. Pour plus d’informations, consultez [Placement de barre d’outils de Page sur Windows](page-toolbar-placement.md).
- L’activation des icônes de page doit être affiché sur un [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) barre d’outils. Pour plus d’informations, consultez [sur Windows, les icônes de TabbedPage](tabbedpage-icons.md).

## <a name="platform-support"></a>Plateforme prise en charge

Les modèles Xamarin.Forms disponibles dans Visual Studio contiennent un projet de plateforme universelle Windows (UWP).

> [!NOTE]
> Prise en charge 1.x et 2.x Xamarin.Forms _Windows Phone 8 Silverlight_, _Windows Phone 8.1_, et _Windows 8.1_ développement d’applications. Toutefois, ces types de projets ont été déconseillées.

## <a name="getting-started"></a>Bien démarrer

Accédez à **fichier > Nouveau > projet** dans Visual Studio et choisissez une de la **multiplateforme > application vide (Xamarin.Forms)** modèles pour bien démarrer.

Les solutions Xamarin.Forms plus anciennes, ou ceux créés sur macOS, n’aura pas tous les projets Windows répertoriés ci-dessus (mais ils doivent être ajoutés manuellement). Si la plateforme Windows que vous souhaitez cibler n’est pas déjà dans votre solution, visitez le [instructions d’installation](installation/index.md) pour ajouter les Windows souhaitées projet type/s.

## <a name="samples"></a>Exemples

[Tous les exemples](https://github.com/xamarin/xamarin-forms-book-preview-2) livre de Petzold [ *création d’applications mobiles avec Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) incluent des projets de plateforme Windows universelle (pour Windows 10).

Le [« Scott Hanselman » application de démonstration](https://github.com/jamesmontemagno/Hanselman.Forms) est disponible séparément et inclut également des projets de l’Apple Watch et Android Wear (à l’aide de Xamarin.iOS et Xamarin.Android respectivement, Xamarin.Forms ne s’exécute pas sur ces plateformes).

## <a name="related-links"></a>Liens connexes

- [Projets d’installation Windows](~/xamarin-forms/platform/windows/installation/index.md)
