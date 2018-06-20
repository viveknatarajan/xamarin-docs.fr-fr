---
title: Xamarin.Android et Kit de développement Java 9
description: Cet article explique comment résoudre les erreurs du Kit de développement Java (JDK) 9 dans Xamarin.Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 529062f820cd682dc6a9c22f706dbceecef1c836
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269671"
---
# <a name="xamarinandroid-and-java-development-kit-9"></a>Xamarin.Android et Kit de développement Java 9

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

Notez que vous n’avez pas désinstaller JDK 9. Toutefois, vous devez vous assurer que Xamarin est à l’aide de JDK 8 au lieu de JDK 9. Dans Visual Studio, cliquez sur **Outils > Options > Xamarin > Paramètres Android**. Si **l’emplacement du Kit de développement Java** n’est pas définie dans un emplacement de JDK 8 (tel que **C:\\Program Files\\Java\\jdk1.8.0_111**), cliquez sur **modification**  et définissez-le sur l’emplacement où le JDK 8 est installé. Dans Visual Studio pour Mac, accédez à **Préférences > projets > emplacements du SDK > Android > Kit de développement Java (JDK)** et cliquez sur **Parcourir** pour mettre à jour ce chemin d’accès.

## <a name="known-issues-with-jdk-9"></a>Problèmes connus avec JDK 9

### <a name="apksigner"></a>apksigner

Il existe un problème connu avec apksigner et JDK 9 dans lequel le `apksigner.bat` fichier appelle la `apksigner.jar` avec `-Djava.ext.dirs` à la place de `-classpath` qui attend de JDK 9. Il est recommandé d’utiliser le JDK 8 (1,8). Pour plus d’informations sur l’installation de JDK 8, consultez [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md)

Si vous avez installé le JDK 9, assurez-vous que le chemin d’accès suivant n’est pas définie sur votre `PATH` variable d’environnement telle qu’elle pointe toujours vers JDK 9 : `C:\ProgramData\Oracle\Java\javapath`. Après la suppression, `java-version` sur une ligne de commande doit afficher JDK 8.
