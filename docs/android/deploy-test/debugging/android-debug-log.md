---
title: "Journal de débogage Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 99b9ed9e3c71766f483f7b00996137aae7a247d1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-debug-log"></a>Journal de débogage Android

## <a name="android-debug-log-overview"></a>Vue d’ensemble du journal de débogage Android

L’une des astuces couramment utilisées par les développeurs pour déboguer leurs applications consiste à utiliser `Console.WriteLine`. Toutefois, sur une plateforme mobile comme Android, il n’y a pas de console. Les appareils Android proposent un journal que vous pourrez utiliser pendant l’écriture d’applications. On l’appelle parfois « logcat » en raison de la commande à taper pour le récupérer.

## <a name="accessing-from-visual-studio"></a>Accès depuis Visual Studio

Dans Visual Studio 2015 et ultérieur, les blocs de journaux iOS et Android sont unifiés.

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 et 2107

La nouvelle fenêtre outil Journal de l’appareil pour Visual Studio affiche les journaux des appareils Android et iOS. Pour l’afficher, exécutez l’une des commandes suivantes : 

-   **Affichage > Autres fenêtres > Journal de l’appareil**
-   **Outils > Android > Journal de l’appareil**
-   **Barre d’outils Android > Journal de l’appareil**

Une fois la fenêtre outil affichée, l’appareil physique peut être sélectionné dans la zone de liste modifiable des appareils. Lorsque l’appareil est sélectionné, il commence automatiquement à ajouter des entrées de journal à la table depuis une application en cours d’exécution. Le passage d’un appareil à l’autre arrête et démarre la journalisation des appareils. Pour que les appareils figurent dans la zone de liste modifiable, un projet Android doit être chargé. Si l’appareil ne figure pas dans la zone de liste modifiable, commencez par vérifier qu’il est disponible dans la liste déroulante Démarrer le débogage. 

Cette fenêtre outil fournit les éléments suivants : une table des entrées de journal, une zone de liste modifiable pour sélectionner l’appareil, un moyen d’effacer des entrées de journal, une zone de recherche et des boutons de lecture/d’arrêt/de pause. 


<a name="Accessing_from_the_Command_Line" />

## <a name="accessing-from-the-command-line"></a>Accès à partir de la ligne de commande

Le journal de débogage peut également être affiché via la ligne de commande. Ouvrez une fenêtre de console et accédez au dossier des outils de plateforme du kit Android SDK (comme **C:\android-sdk-windows\platform-tools**). 

Si seul un appareil est connecté, le journal peut être consulté avec :

```shell
$ adb logcat
```

Si plusieurs appareils sont connectés, l’appareil doit être identifié. Par exemple, `adb -d logcat` affiche le journal de l’unique appareil physique connecté, tandis que `adb -e logcat` affiche le journal de l’unique émulateur en cours d’exécution. 

D’autres commandes sont disponibles simplement en exécutant **adb**.

<a name="Writing_to_the_Debug_Log" />


## <a name="writing-to-the-debug-log"></a>Écriture dans le journal de débogage

Des messages peuvent être écrits dans le journal de débogage en utilisant les méthodes de la classe [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) : 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Le résultat est le suivant :

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

<a name="Interesting_Messages" />

## <a name="interesting-messages"></a>Messages intéressants

Lors de la lecture du journal, et en particulier si des extraits du journal sont fournis à d’autres personnes (sachant que fournir le fichier journal complet est (1) exagéré et (2) illisible), la ligne la *plus importante* par laquelle commencer doit être ressembler à celle-ci :

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Plus précisément, une ligne correspondant à l’expression régulière :

```shell
^I.*ActivityManager.*Starting: Intent
```

et contenant le nom du paquet d’application. Cette ligne correspond au début d’une activité et *la plupart* des messages qui suivront sont normalement en rapport avec l’application. 

En particulier, chaque message contient l’identificateur de processus (pid) du processus générant le message. Dans le message `ActivityManager` ci-dessus, le processus `12944` a généré le message. Pour déterminer quel processus est le processus de l’application en cours de débogage, recherchez le message mono. MonoRuntimeProvider : 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Ce message provient du processus qui a été démarré, et tous les messages suivants contenant ce même pid proviennent du même processus. 
