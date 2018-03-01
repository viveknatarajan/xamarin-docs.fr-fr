---
title: .NET Standard
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 294d28c57978218986d62d1ee6579e8d283b8f72
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>À l’aide de projets de bibliothèque Standard .NET à partager du code

La bibliothèque .NET Standard est une spécification formelle des API .NET qui sont destinées à être disponibles sur tous les runtimes .NET. La motivation derrière la bibliothèque standard consiste à établir une meilleure uniformité dans l’écosystème .NET.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) continue d’établir l’uniformité du comportement du runtime .NET, mais il n’existe aucune spécification similaire pour les bibliothèques de classes de base .NET en ce qui concerne les implémentations de bibliothèque .NET.

Vous pouvez considérer que simplifiée, la génération suivante de [bibliothèque de classes portables](https://msdn.microsoft.com/library/gg597391.aspx).
C’est une bibliothèque unique avec une API uniforme pour toutes les plateformes .NET, y compris .NET Core. Vous simplement créez une bibliothèque Standard .NET unique et l’utilisez à partir de n’importe quel runtime qui prend en charge de la plateforme .NET Standard.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="xamarin-studio"></a>Xamarin Studio

Les projets de bibliothèque .NET standard peuvent être créés dans Xamarin Studio 6.2, en commençant par créer un projet de bibliothèque Portable :

[ ![](net-standard-images/xs01-sml.png "Créez un projet de bibliothèque portable")](net-standard-images/xs01.png)

Une fois que le projet a été créé, avec le bouton droit et ouvrez le **Options du projet** fenêtre.
Dans le **général** section le projet peut être converti en .NET Standard et configuré pour utiliser une version spécifique dans le **plateforme** liste déroulante :

[ ![](net-standard-images/xs02-sml.png "Convertir vers .NET Standard options")](net-standard-images/xs02.png)

Vous pouvez ensuite [créer un package NuGet](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library.md) pour partager la bibliothèque avec d’autres développeurs.

## <a name="visual-studio-for-mac-walkthrough"></a>Procédure pas à pas Visual Studio pour Mac

Cette section vous montre comment créer et utiliser une bibliothèque Standard .NET à l’aide de Visual Studio pour Mac. Consultez la section exemple de bibliothèque .NET Standard pour une implémentation complète.

### <a name="creating-a-net-standard-library"></a>Création d’une bibliothèque Standard de .NET

Ajout d’une bibliothèque Standard .NET à votre solution est assez simple.

1. Dans la boîte de dialogue Ajouter un nouveau projet, sélectionnez le `.NET Core` catégorie, puis sélectionnez `Class Library(.NET Core)`.

  **Remarque :** ce modèle sera renommé `.NET Standard` dans une future version de Visual Studio pour Mac.

  ![Créer une bibliothèque de classes .NET Core](net-standard-images/vsm01.png)

2. Le projet de bibliothèque Standard de .NET apparaîtra comme indiqué dans l’Explorateur de solutions. Le nœud dépendances indique que la bibliothèque utilise la [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Nœud de dépendances dans la solution indique .NET Standard](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Modification des paramètres de la bibliothèque Standard .NET

Les paramètres de la bibliothèque Standard de .NET peuvent être affichées et modifiées en cliquant sur le projet et en sélectionnant `Options` comme indiqué dans cette capture d’écran :

![Modifier le Standard .NET framework cible dans les Options du projet](net-standard-images/vsm03.png)

Dans, vous pouvez modifier votre version de `netstandard` en modifiant le `Target Framework` valeur de liste déroulante.

**En outre :** que vous pouvez modifier le `.csproj` directement pour modifier cette valeur.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-windows-walkthrough"></a>Procédure pas à pas Visual Studio (Windows)

Cette section décrit comment créer et utiliser une bibliothèque Standard .NET à l’aide de Visual Studio. Consultez la section exemple de bibliothèque .NET Standard pour une implémentation complète.

### <a name="creating-a-net-standard-library"></a>Création d’une bibliothèque Standard de .NET

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Ajout d’une bibliothèque Standard .NET à votre solution est assez simple.

1. Dans la boîte de dialogue Ajouter un nouveau projet, sélectionnez le `.NET Standard` catégorie, puis sélectionnez `Class Library(.NET Standard)`.

  ![](net-standard-images/vs01.png "Créer la nouvelle bibliothèque de classes .NET Standard")

2. Le projet de bibliothèque Standard de .NET apparaîtra comme indiqué dans l’Explorateur de solutions. Le nœud dépendances indique que la bibliothèque utilise la [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![](net-standard-images/vs02.png "Projet .NET standard dans la solution")

#### <a name="editing-net-standard-library-settings"></a>Modification des paramètres de la bibliothèque Standard .NET

Les paramètres de la bibliothèque Standard de .NET peuvent être affichées et modifiées en cliquant sur le projet et en sélectionnant `Properties` comme indiqué dans cette capture d’écran :

![](net-standard-images/vs03.png "Référencer une bibliothèque .NET Standard la même façon que les autres projets.")

Dans, vous pouvez modifier votre version de `netstandard` en modifiant le `Target Framework` valeur de liste déroulante.

**En outre :** que vous pouvez modifier le `.csproj` directement pour modifier cette valeur.

#### <a name="using-net-standard-library"></a>À l’aide de la bibliothèque Standard de .NET

Une fois qu’une bibliothèque Standard .NET a été créée, vous pouvez ajouter une référence à celui-ci à partir d’un projet d’Application ou une bibliothèque compatible de la même façon que vous ajoutez des références normalement. Dans Visual Studio, avec le bouton droit sur le nœud Références et choisissez `Add Reference...` vous basculez ensuite vers le `Solution : Projects` onglet comme indiqué :

![](net-standard-images/vs04.png "Dans Visual Studio, avec le bouton droit sur le nœud Références et sélectionnez Ajouter une référence... puis basculez vers l’onglet de projets de la Solution, comme indiqué")

-----


## <a name="related-links"></a>Liens associés

- [Notes de publication](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#.NET_Standard_Support)
