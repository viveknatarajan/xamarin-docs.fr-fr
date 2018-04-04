---
title: Bien démarrer avec C
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8dff45de6de7c9492b199f323656778ac5c34d57
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-c"></a>Bien démarrer avec C


## <a name="requirements"></a>Spécifications

Pour utiliser .NET incorporation avec C, vous devez un ordinateur Mac ou Windows qui exécute :

* macOS 10.12 (Sierra) ou version ultérieure
* Xcode 8.3.2 ou version ultérieure

* Windows 7, 8, 10 ou version ultérieure
* Visual Studio 2015 ou version ultérieure

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>Installation

L’étape suivante consiste à télécharger et installer les outils de l’incorporation de .NET sur votre ordinateur.

Les builds binaire pour les générateurs de C et Java sont toujours pas disponibles, mais seront bientôt disponibles.

Comme alternative, il est possible de générer à partir de notre référentiel git, consultez le [contribuant](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) document pour obtenir des instructions.


## <a name="generation"></a>Génération

Pour générer le code C, appelez l’outil .NET incorporation en passant les indicateurs de droite pour cibler le langage C :

### <a name="windows"></a>Windows :

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Veillez à appeler à partir d’une invite de commandes Visual Studio spécifique à la version de Visual Studio vous êtes ciblé.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>Fichiers de sortie

Si tout se passe bien, la sortie suivante s’affiche :

```csharp
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

Étant donné que la `--compile` indicateur a été passé à l’outil, l’incorporation de .NET doit également avoir compilé les fichiers de sortie dans une bibliothèque partagée, ce qui se trouve en regard des fichiers générés, un `libmanaged.dylib` fichier sur macOS, et `managed.dll` sous Windows.

Pour utiliser la bibliothèque partagée, vous pouvez inclure le `managed.h` fichier d’en-tête C, qui fournit les déclarations C correspondant aux gérés API de bibliothèque et de lien avec mentionnées précédemment compilée bibliothèque partagée.

## <a name="further-reading"></a>informations supplémentaires

* [Limitations de Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Contribue au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
