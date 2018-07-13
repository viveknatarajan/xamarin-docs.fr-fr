---
title: Navigation de Xamarin.Forms
description: Ce guide explique comment effectuer la navigation dans les applications Xamarin.Forms. Xamarin.Forms fournit un nombre d’expériences de navigation de page différente, en fonction du type de Page utilisé.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994726"
---
# <a name="xamarinforms-navigation"></a>Navigation de Xamarin.Forms

_Xamarin.Forms fournit un nombre d’expériences de navigation de page différente, en fonction du type de Page utilisé._

![](images/page-types.png "Types de Page de Xamarin.Forms")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Navigation hiérarchique](hierarchical.md)

La classe [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) propose une expérience de navigation hiérarchique où l’utilisateur est en mesure de parcourir les pages, vers l’avant et vers l’arrière, comme il le souhaite. La classe implémente la navigation comme une pile d’objets [`Page`](xref:Xamarin.Forms.Page) LIFO (dernier entré, premier sorti).

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) se compose d’une liste des onglets et une plus grande zone de détail, avec chaque onglet du chargement du contenu dans la zone de détails.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) est une page qui les utilisateurs peuvent balayer de gauche à droite pour parcourir les pages de contenu, comme une galerie.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) est une page qui gère deux pages d’informations liées à une page maître qui présente les éléments et une page de détails qui présente les détails des éléments sur la page maître.

## <a name="modal-pagesmodalmd"></a>[Pages modales](modal.md)

Xamarin.Forms fournit également la prise en charge des pages modales. Une page modale encourage les utilisateurs à effectuer une tâche autonome et ne peut pas être quittée tant que la tâche n’est pas terminée ou annulée.

## <a name="displaying-pop-upspop-upsmd"></a>[Affichage des fenêtres contextuelles](pop-ups.md)

Xamarin.Forms fournit deux éléments d’interface utilisateur pop-inscription de type : une alerte et une feuille de l’action. Ces éléments d’interface peuvent être utilisés pour poser des questions simples utilisateurs et pour guider les utilisateurs dans les tâches.
