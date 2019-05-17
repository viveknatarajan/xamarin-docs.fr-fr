---
title: Xamarin.Forms Shell
description: Ce guide explique comment utiliser Xamarin.Forms Shell, qui réduit la complexité des applications Xamarin.Forms en fournissant les fonctionnalités fondamentales nécessaires à la plupart des applications.
ms.prod: xamarin
ms.assetid: 85B322AA-808F-41B6-953A-5877264AE643
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 7699b39a6df6c64ae9a481d9171f23dc6a8eba57
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054189"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

![](~/media/shared/preview.png "Cette API est actuellement en préversion")

## <a name="introductionintroductionmd"></a>[Introduction](introduction.md)

Xamarin.Forms Shell réduit la complexité du développement d’applications mobiles en fournissant les fonctionnalités fondamentales nécessaires à la plupart des applications mobiles. Cela inclut une expérience utilisateur de navigation commune, un schéma de navigation basée sur des URI et un gestionnaire de recherche intégré.

## <a name="flyoutflyoutmd"></a>[Menu volant](flyout.md)

Le menu volant est le menu racine d’une application Shell, accessible via une icône ou en balayant depuis le côté de l’écran. Le menu volant se compose d’un en-tête facultatif, de différents éléments de menu volant et d’éléments de menu facultatifs.

## <a name="tabstabsmd"></a>[Onglets](tabs.md)

Après un menu volant, le niveau suivant de la navigation dans une application Shell est la barre d’onglets inférieure. Lorsqu’un onglet contient plusieurs pages, ces pages sont accessibles via des onglets situés en haut de la fenêtre.

## <a name="navigationnavigationmd"></a>[Navigation](navigation.md)

Les applications Shell peuvent utiliser un modèle de navigation basée sur des URI dans lequel des itinéraires permettent d’accéder à n’importe quelle page dans l’application, sans avoir à suivre une hiérarchie de navigation définie.

## <a name="searchsearchmd"></a>[Rechercher](search.md)

Les applications Shell peuvent utiliser la fonctionnalité de recherche intégrée fournie par une zone de recherche qui peut être ajoutée en haut de chaque page.

## <a name="custom-rendererscustomrenderersmd"></a>[Renderers personnalisés](customrenderers.md)

Les applications Shell sont hautement personnalisables via les propriétés et les méthodes présentées par les différentes classes Shell. Toutefois, il est également possible de créer un renderer Shell personnalisé lorsque des personnalisations plus sophistiquées spécifiques à une plateforme sont requises.
