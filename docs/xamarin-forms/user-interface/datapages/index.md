---
title: Pages de données de Xamarin.Forms
description: Cet article présente les pages de données Xamarin.Forms, qui fournissent une API permettant de rapidement et facilement lier une source de données à des vues prédéfinies.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61407670"
---
# <a name="xamarinforms-datapages"></a>Pages de données de Xamarin.Forms

![](~/media/shared/preview.png "Cette API est actuellement en version préliminaire")

> [!IMPORTANT]
> Pages de données nécessite un [Xamarin.Forms thème](~/xamarin-forms/user-interface/themes/index.md) référence à restituer.

Xamarin.Forms de pages de données ont été annoncées lors de l’évolution 2016 et sont disponibles en version préliminaire pour les clients à tester et de fournir des commentaires.

Pages de données fournissent une API pour rapidement et facilement lier une source de données à des affichages prédéfinis. Éléments de liste et les pages de détail affiche automatiquement les données et peuvent être personnalisés à l’aide de thèmes.

Pour voir comment fonctionne la démo de discours évoluent, regardez la [guide Mise en route](get-started.md).

[![](images/demo-sml.png "Exemple de pages de données d’Application")](images/demo.png#lightbox "exemple des pages de données d’Application")

## <a name="introduction"></a>Introduction

Sources de données et les pages de données associées permettent aux développeurs de consommer une source de données pris en charge et l’afficher à l’aide intégrée de l’interface utilisateur de la génération de modèles automatique qui peut être personnalisé avec les thèmes rapidement et facilement.

Pages de données sont ajoutés à une application Xamarin.Forms en incluant le **Xamarin.Forms.Pages** package Nuget.

### <a name="data-sources"></a>Data Sources

La version préliminaire a certaines sources de données prédéfinis disponibles pour une utilisation :

* **JsonDataSource**
* **AzureDataSource** (séparez Nuget)
* **AzureEasyTableDataSource** (séparez Nuget)

Consultez le [guide Mise en route](get-started.md) pour obtenir un exemple utilisant un `JsonDataSource`.


### <a name="pages--controls"></a>Pages et contrôles

Les pages et les contrôles suivants sont inclus afin d’autoriser la simple liaison aux sources de données fourni :

* **ListDataPage** – consultez le [mise en route exemple](get-started.md).
* **DirectoryPage** – une liste avec le regroupement activé.
* **PersonDetailPage** – un affichage personnalisé pour un type d’objet spécifique (une entrée de contact) d’un élément de données.
* **DataView** – une vue pour exposer des données à partir de la source de manière générique.
* **CardView** – un style de vue qui contient une image, le texte du titre et le texte de description.
* **HeroImage** – un mode de rendu d’image.
* **ListItem** – un prédéfinis vue avec une disposition semblable à iOS natif et les éléments de liste Android.

Consultez le [pages de données contrôle référence](controls.md) pour obtenir des exemples.



### <a name="under-the-hood"></a>Sous le capot

Une source de données Xamarin.Forms adhère à la `IDataSource` interface.

L’infrastructure de Xamarin.Forms interagit avec une source de données via les propriétés suivantes :

* `Data` – une liste en lecture seule d’éléments de données qui peuvent être affichés.
* `IsLoading` – une valeur booléenne indiquant si les données sont chargés et disponibles pour le rendu.
* `[key]` – un indexeur pour extraire des éléments.

Il existe deux méthodes `MaskKey` et `UnmaskKey` qui peut être utilisé pour masquer (ou afficher) les propriétés d’élément de données (p. ex. les empêcher de rendu).
La clé correspond à l’une propriété nommée sur l’objet d’élément de données.
