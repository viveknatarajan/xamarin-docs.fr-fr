---
title: DataPages
ms.topic: article
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 10c473656947dfea8bf832ae5a5704897ef4ce63
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="datapages"></a>DataPages

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!IMPORTANT]
> Pages de données requiert un [Xamarin.Forms thème](~/xamarin-forms/user-interface/themes/index.md) référence à restituer.

Xamarin.Forms de pages de données ont été annoncées lors Evolve 2016 et sont disponibles en version préliminaire pour les clients de tenter d’envoyer des commentaires.

Pages de données fournissent une API pour rapidement et facilement lier une source de données à des vues prédéfinies. Éléments de liste et les pages détaillées seront automatiquement le rendu des données et peuvent être personnalisés à l’aide de thèmes.

Pour voir comment la démo keynote Evolve fonctionne, consultez la [mise en route](get-started.md).

[ ![](images/demo-sml.png "Exemple de pages de données d’Application")](images/demo.png "exemple des pages de données d’Application")

## <a name="introduction"></a>Introduction

Sources de données et les pages de données associées permettent aux développeurs de rapidement et facilement consommer une source de données pris en charge et de l’afficher à l’aide intégrée à l’interface utilisateur de la génération de modèles automatique qui peut être personnalisé avec des thèmes.

Pages de données sont ajoutées à une application de Xamarin.Forms en incluant le **Xamarin.Forms.Pages** package Nuget.

### <a name="data-sources"></a>Data Sources

La version d’évaluation a certaines sources de données prédéfinies disponibles :

* **JsonDataSource**
* **AzureDataSource** (séparez Nuget)
* **AzureEasyTableDataSource** (séparez Nuget)

Consultez le [mise en route](get-started.md) pour un exemple en utilisant un `JsonDataSource`.


### <a name="pages--controls"></a>Pages et contrôles

Les pages et les contrôles suivants sont inclus afin d’autoriser la liaison simple pour les sources de données fournies :

* **ListDataPage** : consultez le [exemple de mise en route](get-started.md).
* **DirectoryPage** – une liste avec regroupement activée.
* **PersonDetailPage** – un affichage personnalisé pour un type d’objet spécifique (une entrée de contact) d’un élément de données.
* **DataView** – une vue pour exposer des données à partir de la source de façon générique.
* **CardView** – un style de vue qui contient une image, le texte du titre et le texte de description.
* **HeroImage** – un mode de rendu d’image.
* **ListItem** – un prégénérées vue avec une disposition semblable à iOS natif et des éléments de liste Android.

Consultez le [pages de données contrôle référence](controls.md) pour obtenir des exemples.



### <a name="under-the-hood"></a>Sous le capot

Une source de données Xamarin.Forms conforme à la `IDataSource` interface.

L’infrastructure Xamarin.Forms interagit avec une source de données via les propriétés suivantes :

* `Data` – une liste en lecture seule d’éléments de données qui peuvent être affichés.
* `IsLoading` – une valeur booléenne indiquant si les données sont chargés et disponibles pour le rendu.
* `[key]` – un indexeur pour extraire des éléments.

Il existe deux méthodes `MaskKey` et `UnmaskKey` qui peut être utilisé pour masquer (ou afficher) (c'est-à-dire, les propriétés d’élément de données les empêcher d’être restitués).
La clé correspond à l’une propriété nommée sur l’objet d’élément de données.

