---
title: Navigation de Xamarin.Forms
description: Ce guide explique comment effectuer la navigation dans les applications de Xamarin.Forms. Xamarin.Forms contient un nombre d’expériences de navigation de page différente, selon le type de Page utilisé.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 90aedee42af7ed1788110e832fb3b435d870ee77
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241954"
---
# <a name="xamarinforms-navigation"></a>Navigation de Xamarin.Forms

_Xamarin.Forms contient un nombre d’expériences de navigation de page différente, selon le type de Page utilisé._

![](images/page-types.png "Types de Page de Xamarin.Forms")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Navigation hiérarchique](hierarchical.md)

La classe [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) propose une expérience de navigation hiérarchique où l’utilisateur est en mesure de parcourir les pages, vers l’avant et vers l’arrière, comme il le souhaite. La classe implémente la navigation comme une pile d’objets [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) LIFO (dernier entré, premier sorti).

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Le Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) se compose d’une liste des onglets et une plus grande zone de détails, avec chaque onglet du chargement du contenu dans la zone de détails.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Le Xamarin.Forms [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) est une page que les utilisateurs peuvent un glissement de gauche à droite pour parcourir les pages de contenu, par exemple une galerie.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Le Xamarin.Forms [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) est une page qui gère deux pages d’informations associées à une page maître qui présente les éléments et une page de détails qui présente les détails des éléments sur la page maître.

## <a name="modal-pagesmodalmd"></a>[Pages modales](modal.md)

Xamarin.Forms fournit également la prise en charge pour les pages modales. Une page modale encourage les utilisateurs à effectuer une tâche autonome et ne peut pas être quittée tant que la tâche n’est pas terminée ou annulée.

## <a name="displaying-pop-upspop-upsmd"></a>[Affichage des fenêtres contextuelles](pop-ups.md)

Xamarin.Forms fournit deux éléments d’interface utilisateur de pop haut-type : une alerte et une feuille de l’action. Ces éléments d’interface peuvent être utilisés pour demander aux utilisateurs des questions simples et guider les utilisateurs lors des tâches.
