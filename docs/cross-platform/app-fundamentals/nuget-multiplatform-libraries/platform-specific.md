---
title: Création de projets de bibliothèque spécifique à la plateforme pour NuGet
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0f244e614a40e444139d51a9466ccc7225a7fe68
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Création de projets de bibliothèque spécifique à la plateforme pour NuGet

Les projets de bibliothèque multiplateformes qui ciblent des plateformes spécifiques, telles qu’iOS et Android, fonctionnent le mieux avec les projets partagés.

NuGet peut contenir de code iOS et Android-spécifique, ainsi que de code .NET commun aux deux.

Plusieurs assemblys sont créés et intégrées à un seul package NuGet. Les normes de NuGet Assurez-vous que le package peut être ajouté à tous les types de projet pris en charge, tels que des projets Xamarin iOS et Android.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Étapes de création d’une bibliothèque multiplateforme de NuGet

1. Sélectionnez **fichier > Nouvelle Solution** (ou cliquez avec le bouton droit sur une solution existante et choisissez **Ajouter > Nouveau projet**).

2. Choisissez **multiplateforme bibliothèque** à partir de la **multiplateforme > bibliothèque** section :

  [![](platform-specific-images/mulitplatform-library-sml.png "Configurer la bibliothèque de multi-plateforme pour une seule base de code")](platform-specific-images/multiplatform-library.png#lightbox)

3. Entrez un **nom** et **Description**, puis choisissez **spécifique à la plateforme**:

  [![](platform-specific-images/specific-configure-sml.png "Configurer la bibliothèque spécifique à la plateforme pour iOS et Android")](platform-specific-images/specific-configure.png#lightbox)

4. Effectuez toutes les étapes de l'Assistant. Les projets suivants sont ajoutés à la solution :

  - **Projet Android** – code Android spécifique peut éventuellement être ajouté à ce projet.
  - **iOS projet** – code spécifique à iOS peut éventuellement être ajouté à ce projet.
  - **Projet de NuGet** – aucun code n’est ajouté à ce projet. Il fait référence à d’autres projets et contient la configuration de métadonnées de la sortie du package NuGet.
  - **Projet partagés** – code commun doit être ajouté à ce projet, y compris le code spécifique à la plateforme à l’intérieur de `#if` directives de compilateur.

5. Avec le bouton droit sur le projet NuGet et choisissez **Options**, puis ouvrez le **NuGet Package > métadonnées** section et entrez le [métadonnées requises](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (comme facultatif ainsi que toutes les métadonnées) :

  [![](platform-specific-images/specific-metadata-sml.png "Entrez les métadonnées requises")](platform-specific-images/specific-metadata.png#lightbox)

6. Également dans le **Options du projet** fenêtre, ouvrez le **les assemblys de référence** section et choisissez les profils de la bibliothèque PCL prend en charge la bibliothèque partagée via « commutateur et de leurre » :

  ![](platform-specific-images/specific-reference-assemblies.png "Également dans la fenêtre des Options de projet, ouvrez la section d’assemblys de référence et choisissez les profils de la bibliothèque PCL prend en charge la bibliothèque partagée par leurre et commutateur")

  > [!NOTE]
> « Commutateur et de leurre » signifie que les assemblys de bibliothèque de classes portables ne contiennent que le API exposée par la bibliothèque (il ne peut pas contenir le code spécifique à la plateforme). Lorsque NuGet est ajouté à un projet Xamarin, les bibliothèques partagées seront compilées par rapport à la bibliothèque PCL, mais les assemblys spécifiques à la plateforme contiennent le code qui est utilisé par l’iOS ou Android projet.

7. Avec le bouton droit sur le projet et choisissez **créer un NuGet Package** (ou générer ou déployer la solution) et le **.nupkg** fichier de package NuGet est enregistré dans le **/bin/** dossier ( Debug ou Release, selon la configuration).

  ![](platform-specific-images/create-nuget-package.png "Fichier de package NuGet sera enregistré dans le dossier bin Debug ou Release, selon la configuration")


## <a name="verifying-the-output"></a>Vérification de la sortie

Les packages NuGet sont également des fichiers ZIP, il est donc possible d’examiner la structure interne du package généré.

Cet écran montre le contenu d’un NuGet spécifiques à une plateforme qui prend en charge d’iOS et Android et que deux assemblys de référence sélectionnés :

![](platform-specific-images/nuget-output.png "Fichiers contenus dans le package NuGet")


## <a name="related-links"></a>Liens associés

- [Guide sur les métadonnées](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
