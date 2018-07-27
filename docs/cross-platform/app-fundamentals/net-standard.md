---
title: Utiliser des bibliothèques .NET Standard pour partager du Code
description: Ce document décrit comment utiliser des bibliothèques .NET Standard pour partager du code. Il explique comment créer une bibliothèque .NET Standard, modifier ses paramètres et l’utiliser dans une application.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 65ba1915a2a968a14f0ce21bcada76e1b83531b0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270652"
---
# <a name="net-standard-library-code-sharing"></a>Partage de code de bibliothèque .NET standard

Bibliothèques .NET standard ont une API uniforme pour toutes les plateformes .NET, y compris Xamarin et .NET Core. Créer une bibliothèque .NET Standard unique et l’utiliser à partir de n’importe quel runtime qui prend en charge de la plateforme .NET Standard. Reportez-vous à [ce graphique](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) pour plus d’informations de plateformes prises en charge.

Tandis que les versions de .NET Standard 1.0 à 1.6 fournissent des sous-ensembles de façon incrémentielle supérieure du .NET Framework, .NET Standard 2.0 fournit le meilleur niveau de prise en charge pour les applications Xamarin et pour le portage de bibliothèques de classes portables existantes.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

Cette section décrit comment créer et utiliser une bibliothèque .NET Standard à l’aide de Visual Studio pour Mac.

### <a name="creating-a-net-standard-library"></a>Création d’une bibliothèque .NET Standard

Ajout d’une bibliothèque .NET Standard à votre solution est assez simple.

1. Dans le **ajouter un nouveau projet** boîte de dialogue, sélectionnez le **.NET Core** catégorie, puis sélectionnez **bibliothèque .NET Standard**:

    ![Créer une bibliothèque .NET Standard](net-standard-images/vsm01-m157.png "création d’une bibliothèque .NET de nouveau Standard")

2. Dans l’écran suivant, choisissez le framework cible - **.NET Standard 2.0** est recommandé de :

    [![Choisissez .NET Standard 2.0](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. Dans le dernier écran, tapez le nom du projet et cliquez sur **créer**.

4. Le projet de bibliothèque .NET Standard s’affiche comme indiqué dans l’Explorateur de solutions. Le nœud dépendances indiquera que la bibliothèque utilise la [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![Nœud de dépendances dans la solution indique .NET Standard](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>Modification des paramètres de la bibliothèque .NET Standard

Les paramètres de la bibliothèque .NET Standard peuvent être affichées et modifiées en effectuant un clic droit sur le projet et en sélectionnant `Options` comme indiqué dans cette capture d’écran :

![Modifier le framework cible .NET Standard dans Options du projet](net-standard-images/vsm03-m157.png "modifier la version du Framework .NET Standard cible dans Options du projet")

À l’intérieur, vous pouvez modifier votre version de `netstandard` en modifiant le `Target Framework` valeur de liste déroulante.

**En outre :** vous pouvez modifier le `.csproj` directement pour modifier cette valeur.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Cette section décrit comment créer et utiliser une bibliothèque .NET Standard à l’aide de Visual Studio.

### <a name="creating-a-net-standard-library"></a>Création d’une bibliothèque .NET Standard

Ajout d’une bibliothèque .NET Standard à votre solution est assez simple.

1. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **.NET Standard** catégorie, puis sélectionnez **bibliothèque de classes (.NET Standard)**.

    ![Création d’une nouvelle bibliothèque de classes Standard .NET](net-standard-images/vs01-w157.png "créer une bibliothèque de classes .NET Standard")

2. Le projet de bibliothèque .NET Standard s’affiche comme indiqué dans l’Explorateur de solutions. Le nœud dépendances indiquera que la bibliothèque utilise la [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![NETStandard.Library dans le dossier du projet](net-standard-images/vs02-w157.png "projet .NET Standard dans la solution")

### <a name="editing-net-standard-library-settings"></a>Modification des paramètres de la bibliothèque .NET Standard

Les paramètres de la bibliothèque .NET Standard peuvent être affichées et modifiées en effectuant un clic droit sur le projet et en sélectionnant **propriétés** comme indiqué dans cette capture d’écran :

![Modifier des frameworks de cibles standard de .NET dans les propriétés du projet](net-standard-images/vs03-w157.png "référencer une bibliothèque .NET Standard la même façon que les autres projets")

**En outre :** vous pouvez modifier le `.csproj` directement pour modifier le `TargetFramework` élément modification quelle version est la cible (par exemple). `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>À l’aide d’un projet de bibliothèque .NET Standard

Une fois qu’une bibliothèque .NET Standard a été créée, vous pouvez ajouter une référence à celui-ci à partir de n’importe quel projet d’Application ou une bibliothèque compatible de la même façon que vous ajoutez généralement des références. Dans Visual Studio, avec le bouton droit sur le nœud Références et choisissez **ajouter une référence...**  puis basculez vers le **projets > Solution** onglet comme indiqué :

![Référencement d’une bibliothèque .NET Standard](net-standard-images/vs04.png "dans Visual Studio, avec le bouton droit sur le nœud Références et choisissez Ajouter une référence... puis basculez vers l’onglet projets de la Solution comme indiqué")

-----

## <a name="related-links"></a>Liens associés

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) -des informations détaillées et comparaison avec la bibliothèque de classes portable.
