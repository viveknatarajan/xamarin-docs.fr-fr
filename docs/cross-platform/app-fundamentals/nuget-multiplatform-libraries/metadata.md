---
title: Modifier les métadonnées de NuGet
description: Ce document décrit comment utiliser les options de projet pour modifier les métadonnées de NuGet pour les bibliothèques multiplateformes. Il aborde les métadonnées obligatoires et facultatifs.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266868"
---
# <a name="editing-nuget-metadata"></a>Modifier les métadonnées de NuGet

_Utilisez les options de projet pour modifier les métadonnées de NuGet pour les bibliothèques multiplateformes_

Types de projets de bibliothèque (par exemple, la bibliothèque de classes portable ou .NET Standard ou le nouveau type de projet NuGet) ont un **NuGet Package** section dans le **Options du projet** fenêtre.

Le **métadonnées** section configure les valeurs utilisées dans le [ **.nuspec** fichier manifeste du package NuGet](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Informations requises

Le **général** onglet contient quatre champs qui doivent être entrés pour générer un package NuGet :

[![](metadata-images/metadata-general-sml.png "Fenêtre de métadonnées requises de package NuGet")](metadata-images/metadata-general.png#lightbox)

- **ID** – l’identificateur de package, qui doit être unique dans Nuget.org (ou, là où le package sera distribué). Suivez ce [conseils](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) et utilisent uniquement des caractères qui sont valides dans une URL (sans espaces et évitez les caractères spéciaux plus).
- **Version** : choisissez un numéro de version cohérent avec [règles de contrôle de version de NuGet](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Auteurs** – virgule comme séparateur de liste de noms.
- **Description** – vue d’ensemble des fonctionnalités du package qui s’affiche lorsque les utilisateurs sont en sélectionnant le package.

> [!NOTE]
> Pensez à incrémenter le numéro de version lors de la création de nouvelles versions pour la distribution de NuGet ou d’autres utilisateurs.

Pour plus d’informations, consultez le [référence des éléments requis](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) pour plus d’informations, ainsi que ces instructions détaillées sur [choix d’un identificateur de package unique et en définissant le numéro de version](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) et [ Définition d’un type de package](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Tous les champs de cet onglet doivent être entrées ; Sinon, un message d’erreur apparaît : _« Le projet n’a pas les métadonnées de NuGet pour un package NuGet ne sera pas créé. Métadonnées du package NuGet peuvent être spécifiée dans la section de métadonnées dans Options du projet »_

## <a name="optional-metadata"></a>Métadonnées facultatives

Le **détails** onglet contient les champs facultatifs à inclure dans le fichier de manifeste de package NuGet.

[![](metadata-images/metadata-detail-sml.png "Fenêtre de métadonnées optionnelles de package NuGet")](metadata-images/metadata-detail.png#lightbox)

Reportez-vous à la [référence des éléments facultatifs](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) pour plus d’informations sur les champs obligatoires et facultatifs.

> [!NOTE]
> Si le package NuGet est distribué sur [NuGet.org](https://www.nuget.org) il est recommandé de fournir autant d’informations que possible.


## <a name="related-links"></a>Liens associés

- [Informations de référence sur le fichier nuspec](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
