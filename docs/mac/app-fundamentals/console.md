---
title: Pour les applications de Console à l’aide de liaisons de Xamarin.Mac
description: Créer une application console sans périphérique de contrôle à l’aide de Xamarin.Mac pour accéder aux API de macOS native.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378274"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Liaisons de Xamarin.Mac dans les applications de console

Il existe certains scénarios où vous souhaitez utiliser certaines des API natives Apple dans c# pour créer une application sans affichage &ndash; qui n’a pas d’interface utilisateur &ndash; à l’aide de c#.

Les modèles de projet pour les applications Mac incluent un appel à `NSApplication.Init()` suivie d’un appel à `NSApplication.Main(args)`, il se présente généralement comme suit :

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

L’appel à `Init` prépare le runtime de Xamarin.Mac, l’appel à `Main(args)` démarre la boucle principale d’application Cocoa, qui prépare l’application pour recevoir des événements de clavier et souris et afficher la fenêtre principale de votre application.   L’appel à `Main` tente également de localiser les ressources de Cocoa, préparer une fenêtre de niveau supérieur et attend le programme doit faire partie d’une offre groupée d’applications (programmes distribués dans un répertoire portant le `.app` extension et une mise en page très spécifique).

Applications sans périphérique de contrôle ne nécessitent pas d’un utilisateur de l’interface et n’avez pas besoin de les exécuter en tant que partie d’une offre groupée d’application.

## <a name="creating-the-console-app"></a>Création de l’application de console

Il est donc préférable de commencer avec un type de projet de Console .NET normal.

Vous devez effectuer quelques opérations :

- Créer un projet vide.
- Référence de la bibliothèque de Xamarin.Mac.dll.
- Mettre la dépendance non managée à votre projet.

Ces étapes sont expliquées plus en détail ci-dessous :

### <a name="create-an-empty-console-project"></a>Créer un projet de Console vide

Créer un nouveau projet de Console .NET, assurez-vous qu’il s’agit de .NET et non pas .NET Core, en tant que Xamarin.Mac.dll ne s’exécute pas sous le runtime .NET Core, elle ne s’exécute avec le runtime Mono.

### <a name="reference-the-xamarinmac-library"></a>Référence de la bibliothèque de Xamarin.Mac

Pour compiler votre code, vous pouvez référencer le `Xamarin.Mac.dll` assembly à partir de ce répertoire : `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Pour ce faire, accédez aux références du projet, sélectionnez le **Assembly .NET** onglet, puis cliquez sur le **Parcourir** pour localiser le fichier sur le système de fichiers.  Naviguez jusqu'à l’emplacement ci-dessus, puis sélectionnez le **Xamarin.Mac.dll** à partir de ce répertoire.

Cela vous donnera accès aux API Cocoa au moment de la compilation.   À ce stade, vous pouvez ajouter `using AppKit` vers le haut de votre fichier et appelez le `NSApplication.Init()` (méthode).   Il existe uniquement une étape supplémentaire avant de pouvoir exécuter votre application.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Importer la bibliothèque de prise en charge non gérés dans votre projet

Avant d’exécute votre application, vous devez mettre le `Xamarin.Mac` bibliothèque de prise en charge dans votre projet.   Pour ce faire, ajoutez un nouveau fichier à votre projet (dans les options du projet, sélectionnez **ajouter**, puis **ajouter un fichier existant**) et accédez à ce répertoire :

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

Ici, sélectionnez le fichier **libxammac.dylib**.   Vous aurez un choix de copie, de liaison ou de déplacement.   Personnellement, je préfère liaison, mais copie fonctionne aussi bien.    Vous devez sélectionner le fichier et dans le panneau de la propriété (sélectionnez **vue > panneaux > Propriétés** si le bloc de propriété n’est pas visible), accédez à la **générer** section et définissez le **copier dans la sortie Répertoire** à **Copier si plus récent**.

Vous pouvez maintenant exécuter votre application Xamarin.Mac.

Le résultat dans le répertoire bin doit ressembler à ceci :

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Pour exécuter cette application, vous devez tous les fichiers dans le même répertoire.

## <a name="building-a-standalone-application-for-distribution"></a>Création d’une application autonome pour la distribution

Vous pouvez souhaiter distribuer un seul exécutable à vos utilisateurs.  Pour ce faire, vous pouvez utiliser le `mkbundle` outil pour activer les différents fichiers dans un fichier exécutable autonome.

Tout d’abord, assurez-vous que votre application se compile et s’exécute.   Une fois que vous êtes satisfait des résultats, vous pouvez exécuter la commande suivante à partir de la ligne de commande :

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

Dans l’appel de ligne de commande ci-dessus, l’option `-o` est utilisé pour spécifier la sortie générée, dans ce cas, nous avons transmis `/tmp/consoleapp`.   Il s’agit désormais une application autonome que vous pouvez distribuer et n’a aucune dépendance externe sur Mono ou Xamarin.Mac, il est un fichier exécutable autonome entièrement.

La ligne de commande spécifiée manuellement le **machine.config** fichier à utiliser et un fichier de configuration de mappage de bibliothèque de l’échelle du système.   Ils ne sont pas nécessaires pour toutes les applications, mais il est utile de les regrouper, car ils sont utilisés lorsque vous utilisez d’autres fonctionnalités de .NET

## <a name="project-less-builds"></a>Builds de projet sans

Vous n’avez pas besoin d’un projet complet pour créer une application Xamarin.Mac autonome, vous pouvez également utiliser des makefiles Unix simples pour faire leur travail.   L’exemple suivant montre comment vous pouvez configurer un makefile pour une application de ligne de commande simple :

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)


bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

La méthode ci-dessus `Makefile` fournit trois cibles :

- `make` génère le programme
- `make run` génération et l’exécution du programme dans le répertoire actif
- `make bundle` Crée un fichier exécutable autonome
