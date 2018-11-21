---
title: Liaison d’une bibliothèque Java
description: La Communauté Android a de nombreuses bibliothèques Java que vous pouvez utiliser dans votre application ; Ce guide explique comment incorporer des bibliothèques Java dans votre application Xamarin.Android en créant une bibliothèque de liaisons.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: c41aecf5f8c65ad5bfba5361b77d7c7fc047cda4
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171610"
---
# <a name="binding-a-java-library"></a>Liaison d’une bibliothèque Java

_La Communauté Android a de nombreuses bibliothèques Java que vous pouvez utiliser dans votre application ; Ce guide explique comment incorporer des bibliothèques Java dans votre application Xamarin.Android en créant une bibliothèque de liaisons._

## <a name="overview"></a>Vue d'ensemble

L’écosystème de la bibliothèque tierce pour Android est énorme. Pour cette raison, il est souvent judicieux d’utiliser une bibliothèque Android existante que to créez-en un. Xamarin.Android offre deux façons d’utiliser ces bibliothèques :

-   Créer un *bibliothèque de liaisons* qui encapsule automatiquement la bibliothèque avec C# donc vous pouvez appeler Java des wrappers de code via C# appels.

-   Utilisez le *Java Native Interface* (*JNI*) pour appeler directement des appels dans le code de bibliothèque Java. JNI est une infrastructure de programmation qui permet d’appeler et d’être appelée par les applications natives ou des bibliothèques de code Java.

Ce guide explique la première option : comment créer un *bibliothèque de liaisons* qui encapsule une ou plusieurs bibliothèques Java existantes dans un assembly que vous pouvez lier à dans votre application. Pour plus d’informations sur l’utilisation de JNI, consultez [utilisation de JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implémente des liaisons à l’aide de *gérés des Wrappers RCW* (*MCW*). MCW est un pont JNI qui est utilisé lorsque le code managé doit appeler le code Java. Les wrappers RCW gérés prennent également en charge de sous-classement de types Java et de substitution des méthodes virtuelles sur les types Java. De même, chaque fois que le code de runtime Android (ART) souhaite appeler du code managé, il le fait via un autre pont JNI connu en tant que Android Callable Wrappers (ACW). Cela [architecture](~/android/internals/architecture.md) est illustrée dans le diagramme suivant :

[![Architecture de pont Android JNI](images/architecture.png)](images/architecture.png#lightbox)

Une bibliothèque de liaisons est un assembly contenant géré Callable Wrappers pour les types Java. Par exemple, Voici un type Java, `MyClass`, que nous souhaitons encapsuler dans une bibliothèque de liaisons :

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Une fois que nous générons une bibliothèque de liaisons pour le **.jar** contenant `MyClass`, nous pouvons l’instancier et appeler des méthodes sur celle-ci à partir de C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Pour créer cette bibliothèque de liaisons, vous utilisez le Xamarin.Android *bibliothèque de liaisons Java* modèle. Le projet résultant de la liaison crée un assembly .NET avec les classes MCW, **.jar** ou les fichiers et ressources pour les projets de bibliothèque Android est incorporés. Vous pouvez également créer des bibliothèques de liaisons pour Archive Android (. Les fichiers AAR) et les projets de bibliothèque Android Eclipse. En référençant l’assembly résultant de la DLL de bibliothèque de liaisons, vous pouvez réutiliser une bibliothèque Java existante dans votre projet Xamarin.Android.

Lorsque vous référencez des types dans votre bibliothèque de liaison, vous devez utiliser l’espace de noms de votre bibliothèque de liaison. En général, vous ajoutez un `using` directive en haut de votre C# des fichiers source qui est la version d’espace de noms .NET du nom du package de Java. Par exemple, si le package Java nom pour votre limite **.jar** est la suivante :

```csharp
com.company.package
```

Ensuite, vous placeriez suit `using` instruction en haut de votre C# de source de fichiers pour accéder aux types dans la limite **.jar** fichier :

```csharp
using Com.Company.Package;
```


Lors de la liaison d’une bibliothèque Android existante, il est nécessaire à l’esprit les points suivants :

* **Y a-t-il des dépendances externes pour la bibliothèque ?** &ndash; Toutes les dépendances Java requises par la bibliothèque Android doivent être inclus dans le projet Xamarin.Android, comme un **ReferenceJar** ou comme un **EmbeddedReferenceJar**. Tous les assemblys natifs doivent être ajoutés au projet de liaison en tant qu’un **EmbeddedNativeLibrary**.  

* **Version de l’API Android est la cible de la bibliothèque Android ?** &ndash; Il n’est pas possible de « rétrograder » le niveau d’API Android ; Assurez-vous que le projet de liaison Xamarin.Android cible la même API niveau (ou version ultérieure) en tant que la bibliothèque Android.

* **Quelle version du JDK a été utilisée pour compiler la bibliothèque ?** &ndash; Erreurs de liaison peuvent se produire si la bibliothèque Android a été créée avec une autre version du JDK à en cours d’utilisation par Xamarin.Android. Si possible, recompiler la bibliothèque Android à l’aide de la même version du JDK qui est utilisé par votre installation de Xamarin.Android.


## <a name="build-actions"></a>Actions de génération

Lorsque vous créez une bibliothèque de liaisons, vous définissez *actions de génération* sur le **.jar** ou. Les fichiers AAR vous incorporer dans votre projet de bibliothèque de liaisons &ndash; chaque action de génération détermine comment la **.jar** ou. Les fichiers AAR seront incorporée dans (ou référencé par) votre bibliothèque de liaisons. La liste suivante résume ces actions de génération :

* `EmbeddedJar` &ndash; Incorpore le **.jar** dans la DLL de bibliothèque de liaisons résultante comme ressource incorporée. C’est la plus simple et la plupart des action de génération couramment utilisés. Utilisez cette option lorsque vous souhaitez le **.jar** automatiquement compilé en code d’octet et regroupées dans la bibliothèque de liaisons.

* `InputJar` &ndash; Ne pas incorporer le **.jar** dans la bibliothèque de liaisons qui en résulte. DLL. Votre bibliothèque de liaisons. DLL ont une dépendance sur ce **.jar** lors de l’exécution. Utilisez cette option lorsque vous ne souhaitez pas inclure le **.jar** dans votre bibliothèque de liaisons (par exemple, pour des raisons de licence). Si vous utilisez cette option, vous devez vous assurer que l’entrée **.jar** est disponible sur l’appareil qui exécute votre application.

* `LibraryProjectZip` &ndash; Incorpore une. Fichier AAR dans la bibliothèque de liaisons qui en résulte. DLL. Cela revient à EmbeddedJar, à ceci près que vous pouvez accéder aux ressources (comme code) dans la limite. Fichier de AAR. Utilisez cette option lorsque vous souhaitez incorporer un. AAR dans votre bibliothèque de liaisons.

* `ReferenceJar` &ndash; Spécifie une référence **.jar**: une référence **.jar** est un **.jar** que celui de votre limite **.jar** ou. Les fichiers AAR dépend. Cette référence **.jar** est utilisé uniquement pour satisfaire des dépendances de compilation. Lorsque vous utilisez cette action de génération, C# liaisons ne sont pas créés pour la référence **.jar** et il n’est pas incorporé dans la bibliothèque de liaisons qui en résulte. DLL. Utilisez cette option lorsque vous créez une bibliothèque de liaisons pour la référence **.jar** mais n'avez pas déjà fait. Cette action de génération est utile pour l’empaquetage de plusieurs **.jar**s (et/ou. AARs) dans plusieurs bibliothèques de liaisons interdépendants.

* `EmbeddedReferenceJar` &ndash; Incorpore une référence **.jar** dans la bibliothèque de liaisons qui en résulte. DLL. Utilisez cette action de build lorsque vous souhaitez créer C# liaisons pour les deux l’entrée **.jar** (ou). AAR) et l’ensemble de sa référence **.jar**(s) dans votre bibliothèque de liaisons.

* `EmbeddedNativeLibrary` &ndash; Incorpore native **.so** dans la liaison. Cette action de génération est utilisée pour **.so** les fichiers qui sont requis par le **.jar** de fichiers en cours de liaison. Il peut être nécessaire charger manuellement le **.so** bibliothèque avant l’exécution de code à partir de la bibliothèque Java. Cette opération est décrite ci-dessous.

Ces actions sont expliquées plus en détail dans les guides suivants de la build.

En outre, les actions de génération suivantes sont utilisées à l’aide de l’importation de documentation de l’API Java et de les convertir en C# documentation XML :

* `JavaDocJar` est utilisé pour pointer vers Javadoc archive Jar pour une bibliothèque Java qui est conforme à un style de package Maven (généralement `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` est utilisé pour pointer vers `index.html` fichier au sein de la documentation de référence API HTML.
* `JavaSourceJar` est utilisé pour compléter `JavaDocJar`pour JavaDoc d’abord générer à partir de sources et traitez les résultats comme `JavaDocIndex`, pour une bibliothèque Java qui est conforme à un Maven package style (généralement `FOOBAR-sources**.jar**`).

La documentation de l’API doit être la valeur par défaut doclet Java8, Java7 ou kit de développement logiciel Java6 (il s’agit de tout autre format), ou le style DroidDoc.

## <a name="including-a-native-library-in-a-binding"></a>Y compris une bibliothèque Native dans une liaison

Il peut être nécessaire d’inclure un **.so** bibliothèque dans un projet de liaison Xamarin.Android dans le cadre de la liaison d’une bibliothèque Java. Lorsque le code Java encapsulé s’exécute, Xamarin.Android ne pourra pas passer l’appel JNI et le message d’erreur _java.lang.UnsatisfiedLinkError : méthode Native introuvable :_ apparaîtra dans le logcat out pour l’application.

La solution à ce problème consiste à charger manuellement le **.so** bibliothèque avec un appel à `Java.Lang.JavaSystem.LoadLibrary`. Par exemple, en supposant qu’un projet Xamarin.Android a partagé bibliothèque **libpocketsphinx_jni.so** inclus dans le projet de liaison avec une action de génération **EmbeddedNativeLibrary**, l’extrait de code suivant (exécuté avant d’utiliser la bibliothèque partagée) chargera le **.so** bibliothèque :

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Adaptation des API Java pour C&eparsl;

Le Générateur de liaison Xamarin.Android changera certains idiomes de Java et les modèles pour qu’elles correspondent aux modèles de .NET. La liste suivante décrit comment Java est mappé à C#/.NET :

-   _Méthodes d’accesseur set/get_ dans Java sont _propriétés_ dans .NET.

-   _Champs_ dans Java sont _propriétés_ dans .NET.

-   _Interfaces d’écouteurs/écoute_ dans Java sont _événements_ dans .NET. Les paramètres de méthodes dans les interfaces de rappel seront représentés par un `EventArgs` sous-classe.

-   Un _imbriquées statiques de classe_ dans Java est un _classe imbriquée_ dans .NET.

-   Un _classe interne_ dans Java est un _classe imbriquée_ avec un constructeur d’instance dans C#.



## <a name="binding-scenarios"></a>Scénarios de liaison

Les guides de scénario de liaison suivants peuvent vous aider à lier une bibliothèque Java (ou les bibliothèques) pour l’incorporation dans votre application :

-   [Liaison d’un. JAR](~/android/platform/binding-java-library/binding-a-jar.md) est une procédure pas à pas pour la création de bibliothèques de liaisons pour **.jar** fichiers.

-   [Liaison d’un. AAR](~/android/platform/binding-java-library/binding-an-aar.md) est une procédure pas à pas pour la création de bibliothèques de liaisons pour. Fichiers AAR. Lisez cette procédure pas à pas pour apprendre à lier des bibliothèques d’Android Studio.

-   [Liaison d’un projet de bibliothèque Eclipse](~/android/platform/binding-java-library/binding-a-library-project.md) est une procédure pas à pas pour créer des bibliothèques de liaison à partir de projets de bibliothèque Android. Lisez cette procédure pas à pas pour apprendre à lier les projets de bibliothèque Android Eclipse.

-   [Personnalisation des liaisons](~/android/platform/binding-java-library/customizing-bindings/index.md) explique comment apporter des modifications manuelles à la liaison pour résoudre les erreurs de build et mettre en forme l’API qui en résulte afin qu’il soit plus «C#-comme ».

-   [Résolution des problèmes de liaisons](~/android/platform/binding-java-library/troubleshooting-bindings.md) répertorie les scénarios courants d’erreur de liaison, explique les causes possibles et propose des suggestions pour résoudre ces erreurs.


## <a name="related-links"></a>Liens associés

- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Métadonnées GAPI](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Utilisation de bibliothèques natives](~/android/platform/native-libraries.md)
