---
title: Création d’une bibliothèque multiplateforme pour NuGet
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 85403ee2ab8b5c1433e0e070b3af22fa42cd5efa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Création d’une bibliothèque multiplateforme pour NuGet

Création d’un projet de bibliothèque de multi-plateforme qui utilise PCL ou .NET Standard signifie que NuGet résultant peut être ajouté à tout projet .NET qui prend en charge le profil cible, y compris les projets ASP.NET ou les applications de bureau à l’aide de Windows Forms, WPF ou UWP.

La bibliothèque peut contenir uniquement du code pris en charge par le profil sélectionné PCL ou .NET Standard, ainsi que toutes les autres NuGets qui sont ajoutés.
Cela est adapté à la logique métier et les algorithmes qui peuvent être exprimées entièrement dans la bibliothèque de classes .NET.

Un assembly unique est créé et intégré à un package NuGet.

Si vous devez ultérieurement des fonctionnalités spécifiques à la plateforme, [projets spécifiques à la plateforme peuvent être ajoutés](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Étapes de création d’une bibliothèque multiplateforme de NuGet

1. Sélectionnez **fichier > Nouvelle Solution** (ou cliquez avec le bouton droit sur une solution existante et choisissez **Ajouter > Nouveau projet**).

2. Choisissez **multiplateforme bibliothèque** à partir de la **multiplateforme > bibliothèque** section :

  [![](single-codebase-images/mulitplatform-library-sml.png "Configurer la bibliothèque de multi-plateforme pour une seule base de code")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Entrez un **nom** et **Description**, puis choisissez **unique pour toutes les plateformes**:

  [![](single-codebase-images/single-configure-sml.png "Configurer la bibliothèque de multi-plateforme pour une seule base de code")](single-codebase-images/single-configure.png#lightbox)

4. Effectuez toutes les étapes de l'Assistant. Un projet de bibliothèque unique est créé dans la solution.

5. Avec le bouton droit sur le nouveau projet de bibliothèque, puis sélectionnez **Options**. Le **Générer > Général** section permet le **cible de .NET Framework** à définir : choisissez un profil de bibliothèque de classes portables .NET Portable ou une version .NET Standard :

  [![](single-codebase-images/single-choose-type-sml.png "Choisissez PCL ou .NET Standard pour le type de bibliothèque")](single-codebase-images/single-choose-type.png#lightbox)

6. Également dans le **Options du projet** fenêtre, ouvrez le **NuGet Package > métadonnées** section et entrez le [métadonnées requises](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (ainsi que toutes les métadonnées facultatif) :

  [![](single-codebase-images/single-metadata-sml.png "Entrez les métadonnées requises")](single-codebase-images/single-metadata.png#lightbox)

7. Avec le bouton droit sur le projet de bibliothèque et choisissez **créer un NuGet Package** (ou générer ou déployer la solution) et le **.nupkg** fichier de package NuGet est enregistré dans le **/bin/** dossier (Debug ou Release, selon la configuration) :

  ![](single-codebase-images/create-nuget-package.png "Le fichier de package NuGet sera être enregistré dans le dossier bin Debug ou Release, selon la configuration")


## <a name="verifying-the-output"></a>Vérification de la sortie

Les packages NuGet sont également des fichiers ZIP, il est donc possible d’examiner la structure interne du package généré.

Cette capture d’écran montre le contenu d’un NuGet basée sur la bibliothèque de classes portables : un seul assembly de bibliothèque PCL est inclus :

![](single-codebase-images/nuget-output.png "Fichiers contenus dans le package NuGet")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Ajout d’un Code spécifique à la plateforme

Les projets bibliothèque de classes portables et les projets .NET Standard ne peut pas contenir de références spécifique à la plateforme (par exemple, iOS ou Android fonctionnalités).

Si un projet de bibliothèque de classes portables existant ou d’un projet .NET Standard doit être étendu pour inclure un code spécifique à la plateforme, cela est possible en cliquant sur le projet et en sélectionnant **Ajouter > Ajouter une implémentation de plate-forme...** :

[![](single-codebase-images/add-later-sml.png "Ajouter le menu de mise en œuvre de plate-forme")](single-codebase-images/add-later.png#lightbox)

Un ou plusieurs projets de plateforme peuvent être ajoutés à la solution, et la bibliothèque PCL ou .NET Standard existante peut éventuellement être convertie en un projet partagé :

[![](single-codebase-images/add-later-platforms-sml.png "Ajouter des options de plateforme comme iOS, Android et projet partagé")](single-codebase-images/add-later-platforms-sml.png#lightbox)

Après la conversion en un projet partagé, visitez le **Options du projet > NuGet Package > assemblys de référence**
[section](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) et assurez-vous que les requis profils sont sélectionnés (afin que le NuGet continue d’être compatible avec elle était précédemment utilisé dans les projets).


## <a name="related-links"></a>Liens associés

- [Guide sur les métadonnées](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
