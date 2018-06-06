---
title: Modification des métadonnées de NuGet
description: Ce document décrit comment utiliser les options de projet pour modifier les métadonnées de NuGet pour les bibliothèques multiplateformes. Il décrit les métadonnées obligatoires et facultatifs.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779968"
---
# <a name="editing-nuget-metadata"></a>Modification des métadonnées de NuGet

_Utilisez les options de projet pour modifier les métadonnées de NuGet pour les bibliothèques multiplateformes_

Types de projets de bibliothèque (par exemple, la bibliothèque PCL ou .NET Standard ou le nouveau type de projet de NuGet) ont une **NuGet Package** section dans le **Options du projet** fenêtre.

Le **métadonnées** section configure les valeurs utilisées dans les [ **.nuspec** fichier manifeste du package NuGet](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Informations requises

Le **général** onglet contient quatre champs qui doivent être entrés pour générer un package NuGet :

[![](metadata-images/metadata-general-sml.png "Fenêtre de métadonnées requises de package NuGet.")](metadata-images/metadata-general.png#lightbox)

- **ID** : l’identificateur de package, qui doit être unique dans Nuget.org (ou, là où le package sera distribué). Suivez cette [conseils](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) et utiliser uniquement des caractères qui sont valides dans une URL (sans espaces et évitez les caractères spéciaux plus).
- **Version** : choisissez un numéro de version cohérent avec [les règles de contrôle de version de NuGet](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Les auteurs de** – séparées par des virgules de liste de noms.
- **Description** : vue d’ensemble des fonctionnalités du package qui s’affiche lorsque les utilisateurs sont en sélectionnant le package.

> [!NOTE]
> N’oubliez pas d’incrémenter le numéro de version lors de la création de nouvelles versions pour la distribution de NuGet ou d’autres utilisateurs.

Pour plus d’informations, consultez la [référence des éléments requis](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) pour plus d’informations, ainsi que ces instructions détaillées sur [en choisissant un identificateur unique du package et en définissant le numéro de version](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) et [ Définition d’un type de package](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Sous cet onglet doivent tous être entrés ; Sinon, un message d’erreur apparaît : _« le projet n’a pas les métadonnées de NuGet un package NuGet ne sera donc pas créé. Les métadonnées du package NuGet peuvent être spécifiée dans la section de métadonnées dans les Options du projet »_

## <a name="optional-metadata"></a>Métadonnées facultatives

Le **détails** onglet contient les champs facultatifs à inclure dans le fichier manifeste du package NuGet.

[![](metadata-images/metadata-detail-sml.png "Fenêtre de métadonnées optionnelles de package NuGet")](metadata-images/metadata-detail.png#lightbox)

Reportez-vous à la [référence des éléments facultatifs](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) pour plus d’informations sur les champs obligatoires et facultatifs.

> [!NOTE]
> Si le package NuGet en cours de distribution sur [NuGet.org](https://www.nuget.org) il est recommandé de fournir autant d’informations que possible.


## <a name="related-links"></a>Liens associés

- [Informations de référence sur le fichier nuspec](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
