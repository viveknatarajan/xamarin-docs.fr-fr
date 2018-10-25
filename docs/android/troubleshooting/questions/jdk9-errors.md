---
title: Xamarin.Android et Kit de développement Java 9
description: Cet article explique comment résoudre le Kit de développement Java (JDK) 9 ou version ultérieure erreurs dans Xamarin.Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: d8c64ff79367d93e282edd9534ffb98f5bb90c93
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "36269671"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android et Kit de développement Java 9 ou version ultérieur

_Cet article explique comment résoudre le Kit de développement Java (JDK) 9 ou version ultérieure erreurs dans Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

Xamarin.Android utilise le Kit de développement Java (JDK) pour intégrer le SDK Android pour la création d’applications Android et Android designer en cours d’exécution. Les dernières versions d’Android SDK (API 24 et versions ultérieures) requièrent JDK 8 (1.8) ou la version préliminaire de Microsoft Mobile OpenJDK. **Les outils du Kit Android SDK disponibles à partir de Google n’étant pas encore compatibles avec JDK 9, Xamarin.Android ne fonctionne pas avec JDK 9 ou version ultérieure.**

## <a name="jdk-errors"></a>Erreurs JDK

Si vous essayez de générer un projet Xamarin.Android avec une version de JDK 8 au plus tard le JDK, vous obtiendrez une erreur explicite indiquant que cette version de JDK n’est pas pris en charge. Exemple :

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Pour résoudre ces erreurs, vous devez installer le JDK 8 (1.8) comme expliqué dans [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md).
Autrement, vous pouvez installer le [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) le OpenJDK Mobile de Microsoft va remplacer JDK 8 pour le développement de Xamarin.Android.


## <a name="checking-the-jdk-version"></a>Vérification de la Version JDK

Vous pouvez vérifier quelle version de Java que vous avez installé en entrant la commande suivante (le JDK `bin` répertoire doit être dans votre `PATH`) :

```shell
java -version
```

Si JDK 9 est installé, vous verrez un message semblable au suivant :

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Si JDK 9 ou version ultérieure est installé, vous devez installer Java JDK 8 (1.8) ou la version préliminaire de Microsoft Mobile OpenJDK. Pour plus d’informations sur l’installation de JDK 8, consultez [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md). Pour plus d’informations sur l’installation de Microsoft Mobile OpenJDK, consultez [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md).

Notez que vous n’êtes pas obligé de désinstaller une version ultérieure du JDK ; Toutefois, vous devez vous assurer que Xamarin est à l’aide de JDK 8 plutôt que d’une version ultérieure de JDK. Dans Visual Studio, cliquez sur **Outils > Options > Xamarin > Paramètres Android**. Si **emplacement du Java Development Kit** n’est pas définie dans un emplacement de JDK 8 (tel que **C:\\Program Files\\Java\\jdk1.8.0_111**), cliquez sur **modification**  et définissez-le sur l’emplacement où est installé JDK 8. Dans Visual Studio pour Mac, accédez à **Préférences > projets > emplacements SDK > Android > Kit de développement logiciel Java (JDK)** et cliquez sur **Parcourir** pour mettre à jour de ce chemin d’accès.

## <a name="known-issues-with-jdk-9"></a>Problèmes connus avec JDK 9

### <a name="apksigner"></a>apksigner

Il existe un problème connu avec apksigner et JDK 9 dans lequel le `apksigner.bat` fichier appelle le `apksigner.jar` avec `-Djava.ext.dirs` au lieu de `-classpath` qui attend de JDK 9. Il est recommandé d’utiliser le JDK 8 (1.8). Pour plus d’informations sur l’installation de JDK 8, consultez [comment mettre à jour la version du Kit de développement Java (JDK) ?](~/android/troubleshooting/questions/update-jdk.md)

Si vous avez installé JDK 9, assurez-vous que le chemin d’accès suivant n’est pas définie sur votre `PATH` variable d’environnement en tant qu’il pointe toujours vers JDK 9 : `C:\ProgramData\Oracle\Java\javapath`. Après l’avoir supprimée, `java-version` sur une ligne de commande doit afficher JDK 8.
