---
title: "Création d’un NuGet à partir de projets de bibliothèque existant"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 44a97114e7d325a1fa196d2c9828855ad1a30c94
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Création d’un NuGet à partir de projets de bibliothèque existant

Bibliothèques PCL existant ou .NET Standard peuvent être transformées en NuGets via la **Options du projet** fenêtre :

1. Avec le bouton droit sur le projet de bibliothèque dans le **Solution remplissage** et choisissez **Options**.

2. Accédez à la **NuGet Package > métadonnées** section et toutes les le [les informations nécessaires](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) dans les **général** onglet :

  [ ![](existing-library-images/existing-metadata-sml.png "Entrez les métadonnées requises")](existing-library-images/existing-metadata.png)

3. Si vous le souhaitez, [ajouter des métadonnées supplémentaires](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) dans les **détails** onglet.

4. Une fois que les métadonnées sont configurée, vous pouvez avec le bouton droit sur le projet et choisissez **créer un NuGet Package** et **.nupkg** fichier de package NuGet est enregistré dans le **/bin/** dossier (Debug ou Release, selon la configuration).

  ![](existing-library-images/create-nuget-package.png "Choisissez de créer un NuGet Package dans le menu contextuel")

5. Pour créer le package NuGet sur _chaque_ générer ou déployer, accédez à la **NuGet Package > Générer** section et graduations **créer un NuGet Package lors de la génération du projet**:

    [ ![](existing-library-images/existing-tickbox-sml.png "Graduations pour créer un package NuGet")](existing-library-images/existing-tickbox.png)

> [!NOTE]
> Création de NuGet package peut ralentir le processus de génération. Si cette case n’est pas cochée, vous pouvez toujours générer manuellement un package NuGet à tout moment à partir du menu contextuel du projet (indiqué à l’étape 4 ci-dessus).

## <a name="verifying-the-output"></a>Vérification de la sortie

Les packages NuGet sont également des fichiers ZIP, il est donc possible d’examiner la structure interne du package généré.

Cette capture d’écran montre le contenu d’un NuGet basée sur la bibliothèque de classes portables : un seul assembly de bibliothèque PCL est inclus :

![](existing-library-images/nuget-output.png "Fichiers contenus dans le package NuGet")


## <a name="related-links"></a>Liens associés

- [Guide de métadonnées](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
