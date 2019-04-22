---
title: Bien démarrer avec C
description: Ce document décrit comment utiliser l’incorporation de .NET pour incorporer du code .NET dans une application C. Elle explique comment utiliser l’incorporation de .NET dans Visual Studio 2019 et Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: lobrien
ms.author: laobri
ms.date: 04/19/2018
ms.openlocfilehash: 342ba2a6b51483983df7bd04034a4cef62fd57ff
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58854910"
---
# <a name="getting-started-with-c"></a>Bien démarrer avec C

## <a name="requirements"></a>Configuration requise

Pour utiliser l’incorporation de .NET avec C, vous aurez besoin d’un ordinateur Mac ou Windows qui exécute :

### <a name="macos"></a>macOS

* macOS 10.12 (Sierra) ou version ultérieure
* Xcode 8.3.2 ou version ultérieure
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 ou version ultérieure
* Visual Studio 2015 ou version ultérieure

## <a name="installing-net-embedding-from-nuget"></a>Installation d’incorporation de .NET à partir de NuGet

Suivez ces [instructions](~/tools/dotnet-embedding/get-started/install/install.md) pour installer et configurer .NET incorporation pour votre projet.

L’appel de commande, vous devez configurer ressemblera (éventuellement avec des numéros de version différents et les chemins d’accès) :

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Génération

### <a name="output-files"></a>Fichiers de sortie

Si tout se passe bien, la sortie suivante s’affiche :

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Dans la mesure où le `--compile` indicateur a été passé à l’outil, l’incorporation de .NET doit également avoir compilé les fichiers de sortie dans une bibliothèque partagée, vous pouvez trouver en regard des fichiers générés, un **libmanaged.dylib** fichier sur macOS et **managed.dll** sur Windows.

Pour utiliser la bibliothèque partagée, vous pouvez inclure le **managed.h** fichier d’en-tête C, qui fournit les déclarations C correspondant à respectifs managed API de bibliothèque et un lien avec mentionné précédemment compilé bibliothèque partagée.

## <a name="further-reading"></a>informations supplémentaires

* [Limitations de l’incorporation de .NET](~/tools/dotnet-embedding/limitations.md)
* [Contribution au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
