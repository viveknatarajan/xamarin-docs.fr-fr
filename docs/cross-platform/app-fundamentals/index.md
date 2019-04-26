---
title: Partage de Code sur plusieurs plateformes
description: Ce document contient des liens vers des guides différents qui décrivent les techniques de partage de code, y compris les bibliothèques de classes portables, les projets partagés, .NET Standard et NuGet.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 35c210cc75033f2ad2c83ed6f5196f71d26d000f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227020"
---
# <a name="sharing-code-on-multiple-platforms"></a>Partage de code sur plusieurs plateformes

Ces articles décrivent les différentes options disponibles pour partager du code entre les plateformes, notamment Windows, Android, iOS et bien plus encore.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Vue d’ensemble de partage de code](code-sharing.md)

En savoir plus sur le code de différentes options disponibles pour les projets Xamarin, y compris les bibliothèques .NET Standard et les projets partagés de partage. Bibliothèques de classes portables sont également autorisés, ils sont toutefois considérés comme déconseillé en faveur de .NET Standard.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard est l’option recommandée pour partager du code entre plateformes. Code est généré par rapport à une version spécifique (version 2.0 fournit la meilleure compatibilité d’API avec le code .NET Framework existant) et peut ensuite être consommé par d’autres projets qui prennent en charge ce niveau ou une version ultérieure. Projets .NET standard sont pris en charge dans Visual Studio 2019 et Visual Studio 2019 pour Mac.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)

Projets partagés vous permettent d’écrire du code commun qui est référencé par un nombre de projets d’application différents. Le code est compilé dans le cadre de chaque projet de référence et peut inclure des directives de compilateur pour incorporer des fonctionnalités spécifiques à la plateforme dans la base de code partagé. Cet article décrit comment les projets partagés fonctionnent et comment créer et de les utiliser avec les projets Xamarin.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md)

Les projets de bibliothèque de classes portables vous permettent de créer et distribuer des assemblys qui contiennent du code partagé pour s’exécuter sur plusieurs plateformes. Pour créer une bibliothèque de classes portables (ou « Portable »), vous sélectionnez tout d’abord les plateformes à cibler, puis écrire du code par rapport à un ensemble de sous-chemin du .NET Framework qui est disponible dans le profil défini pour ces plateformes. Bibliothèques de classes portables sont considérées comme déconseillées dans les dernières versions de Visual Studio ; les développeurs sont encouragés à utiliser à la place de .NET Standard 2.0.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Projets de NuGet : Bibliothèques multiplateformes pour le partage de code](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Les packages NuGet peuvent être générées automatiquement à partir de projets standards .NET ou de la bibliothèque de classes portable ; et les projets partagés peut être empaquetés dans les packages NuGet « appât and commutateur » à l’aide du type de projet NuGet distinct. Cette section explique comment créer des packages NuGet pour chaque scénario de partage de code.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Créer manuellement des packages NuGet pour Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Conseils relatifs à la création de packages NuGet qui fonctionnent avec la plateforme Xamarin.

## <a name="use-cc-libraries-in-cross-platform-xamarin-projectscross-platformcppindexmd"></a>[Utiliser les bibliothèques C/C++ dans les projets multiplateformes Xamarin](~/cross-platform/cpp/index.md)

Cette technique vous permet de découpler l’évolution de vos bibliothèques C/C++, un C# de liaison dans un package NuGet et vos applications Xamarin. Fonctionnalité est fournie par la bibliothèque de C/C++ natives multiplateformes, mais tout le code spécifique à la plateforme est isolé des applications Xamarin finales, ce qui permet des performances le plus élevé possible avec aucune duplication de code. 
