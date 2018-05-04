---
title: L’installation de l’incorporation de .NET
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
ms.technology: xamarin-cross-platform
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: d4ee3ef610611dd489d90a364d15f727ea49a79e
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-net-embedding"></a>L’installation de l’incorporation de .NET

## <a name="installing-net-embedding-from-nuget"></a>L’installation de .NET incorporation de NuGet

Choisissez **Ajouter > ajouter des Packages NuGet...**  et installer **Embeddinator-4000** à partir du Gestionnaire de package NuGet :

![Gestionnaire de package NuGet](images/visualstudionuget.png)

Ceci installe **Embeddinator-4000.exe** et **objcgen** dans les **packages/Embeddinator-4000/tools** active.

En règle générale, la dernière version de la Embeddinator-4000 doit être sélectionnée pour le téléchargement. Prise en charge objective-C requiert 0,4 ou version ultérieure.

## <a name="running-manually"></a>Exécution manuelle

Maintenant que NuGet est installé, vous pouvez exécuter les outils à la main.

- Ouvrez un Terminal (macOS) ou une invite de commandes (Windows)
- Remplacez le répertoire à la racine de votre solution
- Les outils sont installée dans :
    - **. / Embeddinator/packages-4000. [VERSION] / tools/objcgen** (Objective-C)
    - **. / Embeddinator/packages-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- Sur Mac OS, **objcgen** peut être exécuté directement. 
- Sous Windows, **Embeddinator-4000.exe** peut être exécuté directement.
- Sur Mac OS, **Embeddinator-4000.exe** doit être exécuté avec **mono**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Chaque appel de la commande devez un certain nombre de paramètres figurant dans la documentation spécifique à la plateforme.

## <a name="automatic-binding-generation"></a>Génération de la liaison automatique

Il existe deux approches permettant d’exécuter .NET incorporation de partie de votre processus de génération.

- Cibles de MSBuild personnalisées
- Étapes post-build

Alors que ce document décrit les deux, il est préférable d’utiliser une cible MSBuild personnalisée. Étapes de génération POST ne fonctionnera pas nécessairement lors de la génération à partir de la ligne de commande.

### <a name="custom-msbuild-targets"></a>Cibles de MSBuild personnalisées

Pour personnaliser votre build avec les cibles de MSbuild, commencez par créer un **Embeddinator-4000.targets** fichier en regard de votre csproj semblable au suivant :

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Ici, texte de la commande doit être rempli avec l’un des appels d’incorporation de .NET répertoriées dans la documentation spécifique à la plateforme.

Pour utiliser cette cible :

- Fermez votre projet dans Visual Studio 2017 ou Visual Studio pour Mac
- Ouvrez la bibliothèque csproj dans un éditeur de texte
- Ajoutez la ligne en bas, au-dessus de la dernière `</Project>` ligne :

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Rouvrez votre projet.

### <a name="post-build-steps"></a>Étapes post-build

Les étapes d’ajout d’une étape post-build pour exécuter .NET incorporation varient en fonction de l’IDE :

#### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

Dans Visual Studio pour Mac, accédez à **Options du projet > Générer > commandes personnalisées** et ajoutez un **après génération** étape.

Configurer la commande indiquée dans la documentation spécifique à la plateforme.

> [!NOTE]
> Vérifiez que vous utilisez le numéro de version que vous avez installé à partir de NuGet

Si vous souhaitez effectuer un développement continu sur le projet c#, vous pouvez également ajouter une commande personnalisée pour nettoyer le **sortie** répertoire avant d’exécuter l’incorporation de .NET :

```shell
rm -Rf '${SolutionDir}/output/'
```

![Action de génération personnalisée](images/visualstudiocustombuild.png)

> [!NOTE]
> La liaison générée sera placée dans le répertoire indiqué par le `--outdir` ou `--out` paramètre. Le nom de liaison générée peut varier que certaines plateformes ont des limitations sur les noms de package.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Nous allons définir essentiellement la même chose, mais les menus de Visual Studio 2017 sont légèrement différentes. Les commandes d’environnement sont également légèrement différents.

Accédez à **Options du projet > événements de Build** et entrez la commande figurant dans la documentation spécifique à la plateforme dans le **ligne de commande d’événement post-build** boîte. Par exemple :

![.NET incorporation sur Windows](images/visualstudiowindows.png)
