---
title: Xamarin.Android et JDK 9
description: "Cet article explique comment résoudre les erreurs du Kit de développement Java (JDK) 9 dans Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1dd2fac015783e06bad6656f09a687f3abba2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-and-jdk-9"></a>Xamarin.Android et JDK 9

_Cet article explique comment résoudre les erreurs du Kit de développement Java (JDK) 9 dans Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

Xamarin.Android utilise le Kit de développement Java (JDK) pour intégrer le SDK Android pour la création d’applications Android et en cours d’exécution du concepteur Android. Les dernières versions du SDK Android (API 24 et versions ultérieures) requièrent JDK 8 (1,8). **Étant donné que les outils du SDK Android de Google ne sont pas encore compatibles avec JDK 9, Xamarin.Android ne fonctionne pas avec JDK 9.**

## <a name="jdk-9-errors"></a>JDK 9 erreurs

Si vous essayez de générer un projet de Xamarin.Android avec JDK 9 est installé, vous obtiendrez une erreur explicite indiquant que JDK 9 n’est pas pris en charge.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Pour résoudre ces erreurs, vous devez installer le JDK 8 (1,8) comme expliqué dans [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>La vérification de la Version du JDK

Vous pouvez vérifier quelle version de Java que vous avez installé en entrant la commande suivante (le JDK `bin` répertoire doit être dans votre `PATH`) :

```shell
java -version
```

Si les 9 JDK installé, vous verrez un message comme suit :

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Si JDK 9 est installé, vous devez installer Java JDK 8 (1,8). Pour plus d’informations sur l’installation de JDK 8, consultez [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md)

## <a name="known-issues-with-jdk-9"></a>Problèmes connus avec JDK 9

### <a name="apksigner"></a>apksigner

Il existe un problème connu avec apksigner et JDK 9 dans lequel le `apksigner.bat` fichier appelle la `apksigner.jar` avec `-Djava.ext.dirs` à la place de `-classpath` qui attend de JDK 9. Il est recommandé d’utiliser le JDK 8 (1,8). Pour plus d’informations sur l’installation de JDK 8, consultez [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md)

Après la désinstallation de JDK 9, assurez-vous que le chemin d’accès suivant n’est pas définie sur votre `PATH` variable d’environnement telle qu’elle pointe toujours vers JDK 9 : `C:\ProgramData\Oracle\Java\javapath`. Après la suppression, `java -version` sur une ligne de commande doit afficher JDK 8.
