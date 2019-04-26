---
title: L’installation de l’incorporation de .NET
description: Ce document décrit comment installer l’incorporation de .NET. Elle explique comment exécuter les outils à la main, comment générer des liaisons automatiquement, l’utilisation de cibles de MSBuild personnalisées et les étapes post-builds nécessaires.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 2a572748c21d2a640add3346d1162f4b6bdc8e99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076613"
---
# <a name="installing-net-embedding"></a>L’installation de l’incorporation de .NET

## <a name="installing-net-embedding-from-nuget"></a>Installation d’incorporation de .NET à partir de NuGet

Choisissez **Ajouter > ajouter des Packages NuGet...**  et installer **Embeddinator-4000** à partir du Gestionnaire de package NuGet :

![NuGet Package Manager](images/visualstudionuget.png)

Cela installera **Embeddinator-4000.exe** et **objcgen** dans le **packages/Embeddinator-4000/tools** directory.

En règle générale, la dernière version de la Embeddinator-4000 doit être sélectionnée pour le téléchargement. Prise en charge objective-C requiert 0.4 ou version ultérieure.

## <a name="running-manually"></a>Exécution manuelle

Maintenant que le package NuGet est installé, vous pouvez exécuter les outils à la main.

- Ouvrez un Terminal (macOS) ou une invite de commandes (Windows)
- Accédez au répertoire racine de votre solution
- Les outils sont installé dans :
    - **. / packages/Embeddinator-4000. [VERSION] / tools/objcgen** (Objective-C)
    - **. / packages/Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C)
- Sur macOS, **objcgen** peut être exécuté directement.
- Sur Windows, **Embeddinator-4000.exe** peut être exécuté directement.
- Sur macOS, **Embeddinator-4000.exe** doit être exécuté avec **mono**:
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Chaque appel de la commande aurez besoin d’un nombre de paramètres figurant dans la documentation spécifique à la plateforme.

## <a name="automatic-binding-generation"></a>Génération de la liaison automatique

Il existe deux approches permettant d’exécuter automatiquement l’incorporation de .NET de partie de votre processus de génération.

- Cibles de MSBuild personnalisées
- Étapes post-builds

Bien que ce document décrit les deux, il est préférable d’utiliser une cible MSBuild personnalisée. Étapes de génération POST ne fonctionnera pas nécessairement lors de la génération à partir de la ligne de commande.

### <a name="custom-msbuild-targets"></a>Cibles de MSBuild personnalisées

Pour personnaliser votre build avec des cibles MSbuild, commencez par créer un **Embeddinator-4000.targets** fichier en regard de votre csproj semblable au suivant :

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Ici, texte de la commande doit être rempli avec un des appels .NET incorporation répertoriés dans la documentation spécifique à la plateforme.

Pour utiliser cette cible :

- Fermez votre projet dans Visual Studio 2017 ou Visual Studio pour Mac
- Ouvrez la bibliothèque csproj dans un éditeur de texte
- Ajoutez la ligne en bas, au-dessus de la dernière `</Project>` ligne :

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Rouvrez votre projet

### <a name="post-build-steps"></a>Étapes post-builds

Les étapes pour ajouter une étape post-build pour exécuter l’incorporation de .NET varient en fonction de l’IDE :

#### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

Dans Visual Studio pour Mac, accédez à **Options du projet > Build > commandes personnalisées** et ajoutez un **après génération** étape.

Configurer la commande indiquée dans la documentation spécifique à la plateforme.

> [!NOTE]
> Veillez à utiliser le numéro de version que vous avez installé à partir de NuGet

Si vous vous apprêtez à faire le développement en cours sur le C# projet, vous pouvez également ajouter une commande personnalisée pour nettoyer le **sortie** répertoire avant d’exécuter l’incorporation de .NET :

```shell
rm -Rf '${SolutionDir}/output/'
```

![Action de génération personnalisée](images/visualstudiocustombuild.png)

> [!NOTE]
> La liaison générée sera placée dans le répertoire indiqué par le `--outdir` ou `--out` paramètre. Le nom de liaison généré peut varier car certaines plateformes ont des limitations sur les noms de package.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Nous allons définir essentiellement la même chose, mais les menus dans Visual Studio 2017 sont légèrement différentes. Les commandes d’environnement sont également légèrement différentes.

Accédez à **Options du projet > événements de Build** et entrez la commande répertoriée dans la documentation spécifique à la plateforme dans le **ligne de commande d’événement post-build** boîte. Exemple :

![.NET incorporation sur Windows](images/visualstudiowindows.png)
