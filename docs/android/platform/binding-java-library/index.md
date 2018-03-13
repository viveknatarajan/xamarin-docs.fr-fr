---
title: "Liaison d’une bibliothèque de Java"
description: "La Communauté Android a de nombreuses bibliothèques Java que vous pouvez utiliser dans votre application ; Ce guide explique comment incorporer des bibliothèques Java à votre application de Xamarin.Android en créant une bibliothèque de liaisons."
ms.topic: article
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: f336767cb6aea8bd8c7ce44f6479850a63d473a6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="binding-a-java-library"></a>Liaison d’une bibliothèque de Java

_La Communauté Android a de nombreuses bibliothèques Java que vous pouvez utiliser dans votre application ; Ce guide explique comment incorporer des bibliothèques Java à votre application de Xamarin.Android en créant une bibliothèque de liaisons._

## <a name="overview"></a>Vue d'ensemble

L’écosystème de bibliothèque tierce pour Android est énorme. Pour cette raison, il est souvent judicieux d’utiliser une bibliothèque Android existante à to créer un nouveau. Xamarin.Android offre deux façons d’utiliser ces bibliothèques :

-   Créer un *bibliothèque de liaisons* qui encapsule automatiquement la bibliothèque de wrappers c# afin de vous permet d’appeler le code Java via c# appelle.

-   Utilisez le *Java Native Interface* (*JNI*) pour appeler directement des appels dans le code de bibliothèque Java. JNI est une infrastructure de programmation qui permet d’appeler et d’être appelée par les applications natives ou des bibliothèques de code Java.

Ce guide explique la première option : comment créer un *bibliothèque de liaisons* qui encapsule une ou plusieurs bibliothèques Java existants dans un assembly que vous pouvez lier dans votre application. Pour plus d’informations sur l’utilisation de JNI, consultez [travaillez JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android implémente des liaisons à l’aide de *gérés Callable Wrappers* (*MCW*). MCW est un pont JNI qui est utilisé lorsque le code managé doit appeler le code Java. Wrappers CCW managés prennent également en charge de sous-classement types Java et de substitution des méthodes virtuelles des types Java. De même, chaque fois que le code d’exécution Android (ART) souhaite appeler du code managé, il le fait via un autre pont JNI connu en tant que Android Callable Wrappers (ACW). Cela [architecture](~/android/internals/architecture.md) est illustré dans le diagramme suivant :

[![Architecture de pont Android JNI](images/architecture.png)](images/architecture.png#lightbox)

Une bibliothèque de liaisons est un assembly contenant géré Callable Wrappers pour les types Java. Par exemple, est un type Java, `MyClass`, que nous souhaitons encapsuler dans une bibliothèque de liaisons :

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Une fois que nous générons une bibliothèque de liaisons pour le **.jar** contenant `MyClass`, nous pouvons instancier et appeler des méthodes sur celle-ci à partir de c# :

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Pour créer cette bibliothèque de liaisons, vous utilisez la Xamarin.Android *bibliothèque de liaisons Java* modèle. Le projet résultant de la liaison crée un assembly .NET avec les classes MCW, **.jar** ou les fichiers et ressources pour les projets de bibliothèque Android est incorporés. Vous pouvez également créer des bibliothèques de liaisons pour un archivage Android (. Les fichiers AAR) et les projets de bibliothèque Android Eclipse. En faisant référence à l’assembly résultant de la DLL de bibliothèque de liaisons, vous pouvez réutiliser une bibliothèque Java existante dans votre projet Xamarin.Android.

Lorsque vous référencez des types dans votre bibliothèque de liaison, vous devez utiliser l’espace de noms de la bibliothèque de liaison. En général, vous ajoutez un `using` directive en haut de votre code source c# des fichiers qui est la version d’espace de noms .NET du nom de package Java. Par exemple, si le package Java de nom pour votre limite **.jar** est le suivant :

```csharp
com.company.package
```

Ensuite, vous devez placer les éléments suivants `using` instruction en haut de vos fichiers sources c# pour accéder aux types dans la limite **.jar** fichier :

```csharp
using Com.Company.Package;
```


Lors de la liaison d’une bibliothèque Android existante, il est nécessaire à l’esprit les points suivants :

* **Y a-t-il des dépendances externes pour la bibliothèque ?** &ndash; Toutes les dépendances Java requises par la bibliothèque Android doivent être inclus dans le projet Xamarin.Android, comme un **ReferenceJar** ou comme une **EmbeddedReferenceJar**. Tous les assemblys natifs doivent être ajoutés au projet de liaison dans un **EmbeddedNativeLibrary**.  

* **Quelle version de l’API Android est la cible de la bibliothèque Android ?** &ndash; Il n’est pas possible de « rétrograder » au niveau de l’API Android ; Assurez-vous que le projet de liaison Xamarin.Android cible la même API niveau (ou version ultérieure) en tant que la bibliothèque Android.

* **Détermine la version du JDK a été utilisée pour compiler la bibliothèque ?** &ndash; Erreurs de liaison peuvent se produire si la bibliothèque Android a été créée avec une autre version du JDK à en cours d’utilisation par Xamarin.Android. Si possible, recompilez la bibliothèque Android à l’aide de la même version du JDK utilisé par votre installation de Xamarin.Android.


## <a name="build-actions"></a>Actions de génération

Lorsque vous créez une bibliothèque de liaisons, vous définissez *des actions de génération* sur la **.jar** ou. Les fichiers AAR que vous incorporer dans votre projet de bibliothèque de liaisons &ndash; chaque action de génération détermine comment la **.jar** ou. Les fichiers AAR seront incorporée dans (ou référencé par) la bibliothèque de liaisons. La liste suivante résume ces actions de génération :

* `EmbeddedJar` &ndash; Incorpore la **.jar** dans la DLL de bibliothèque de liaisons résultant comme une ressource incorporée. C’est la plus simple et la plupart des action de génération de couramment utilisés. Utilisez cette option lorsque vous souhaitez que le **.jar** automatiquement compilé en code d’octet et empaquetées dans la bibliothèque de liaisons.

* `InputJar` &ndash; Ne pas incorporer le **.jar** dans la bibliothèque de liaisons qui en résulte. DLL. Votre bibliothèque de liaisons. DLL a une dépendance sur ce **.jar** lors de l’exécution. Utilisez cette option lorsque vous ne souhaitez pas inclure le **.jar** dans votre bibliothèque de liaisons (par exemple, pour des raisons de licence). Si vous utilisez cette option, vous devez vous assurer que l’entrée **.jar** est disponible sur le périphérique qui exécute votre application.

* `LibraryProjectZip` &ndash; Incorpore un. Fichier AAR dans la bibliothèque de liaisons qui en résulte. DLL. Cela est similaire à EmbeddedJar, à ceci près que vous pouvez accéder aux ressources (comme code) dans la limite. Fichier de AAR. Utilisez cette option lorsque vous souhaitez incorporer un. AAR dans votre bibliothèque de liaisons.

* `ReferenceJar` &ndash; Spécifie une référence **.jar**: une référence **.jar** est un **.jar** celle de votre limite **.jar** ou. Les fichiers AAR dépend. Cette référence **.jar** est utilisée uniquement pour satisfaire les dépendances de compilation. Lorsque vous utilisez cette action de génération, les liaisons c# ne sont pas créées pour la référence **.jar** et il n’est pas incorporé dans la bibliothèque de liaisons résultante. DLL. Utilisez cette option lorsque vous créez une bibliothèque de liaisons pour la référence **.jar** mais n'avez pas déjà fait. Cette action de génération est utile pour empaqueter plusieurs **.jar**s (et/ou. AARs) dans plusieurs bibliothèques de liaisons interdépendants.

* `EmbeddedReferenceJar` &ndash; Incorpore une référence **.jar** dans la bibliothèque de liaisons qui en résulte. DLL. Utilisez cette action de build lorsque vous souhaitez créer des liaisons c# pour les deux l’entrée **.jar** (ou). AAR) et tous ses référence **.jar**(s) dans votre bibliothèque de liaisons.

* `EmbeddedNativeLibrary` &ndash; Incorpore natif **.so** dans la liaison. Cette action de génération est utilisée pour **.so** les fichiers qui sont requis par le **.jar** de fichiers en cours de liaison. Il peut être nécessaire charger manuellement le **.so** bibliothèque avant l’exécution de code à partir de la bibliothèque Java. Cela est décrit ci-dessous.

Ces actions sont expliquées plus en détail dans les guides suivants de génération.

En outre, les actions de génération suivantes sont utilisées pour aider à l’importation de documentation de l’API Java et les convertissent dans la documentation XML c# :

* `JavaDocJar` est utilisé pour pointer vers Javadoc archive Jar pour une bibliothèque de Java est conforme à un style de package Maven (généralement `FOOBAR-javadoc**.jar**`).
* `JavaDocIndex` est utilisé pour pointer vers `index.html` fichier au sein de la documentation de référence API HTML.
* `JavaSourceJar` permet de compléter `JavaDocJar`pour JavaDoc d’abord générer à partir de sources et traitez les résultats en tant que `JavaDocIndex`, pour une bibliothèque de Java est conforme à un Maven package style (généralement `FOOBAR-sources**.jar**`).

La documentation de l’API doit être la valeur par défaut doclet Java8, Java7 ou kit de développement logiciel Java6 (il s’agit de tout autre format), ou le style DroidDoc.

## <a name="including-a-native-library-in-a-binding"></a>Y compris une bibliothèque Native dans une liaison

Il peut être nécessaire d’inclure un **.so** bibliothèque dans un projet de liaison de Xamarin.Android dans le cadre de la liaison d’une bibliothèque de Java. Lors de l’exécution, le code Java encapsulé Xamarin.Android ne pourront pas effectuer l’appel JNI et le message d’erreur _java.lang.UnsatisfiedLinkError : méthode Native introuvable :_ s’affiche dans le logcat out pour l’application.

La solution à ce problème consiste à charger manuellement le **.so** bibliothèque avec un appel à `Java.Lang.JavaSystem.LoadLibrary`. Par exemple, en supposant qu’un projet Xamarin.Android a partagé bibliothèque **libpocketsphinx_jni.so** inclus dans le projet de liaison avec une action de génération **EmbeddedNativeLibrary**, ce qui suit extrait de code (exécutée avant d’utiliser la bibliothèque partagée) chargera le **.so** bibliothèque :

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Adaptation des API Java à C&eparsl;

Le Générateur de liaison de Xamarin.Android change certains idiomes de Java et les modèles afin qu’ils correspondent aux modèles de .NET. La liste suivante décrit comment Java est mappée à C# / .NET :

-   _Méthodes d’accesseur set/get_ dans Java sont _propriétés_ dans .NET.

-   _Champs_ dans Java sont _propriétés_ dans .NET.

-   _Interfaces d’écouteurs/écoute_ dans Java sont _événements_ dans .NET. Les paramètres de méthodes dans les interfaces de rappel seront représentés par un `EventArgs` sous-classe.

-   A _imbriquées statiques de classe_ dans Java est un _classe imbriquée_ dans .NET.

-   Un _classe interne_ dans Java est un _classe imbriquée_ avec un constructeur d’instance en c#.



## <a name="binding-scenarios"></a>Scénarios de liaison

Les guides de scénario de liaison suivants peuvent vous aider à lier une bibliothèque Java (ou les bibliothèques) pour l’incorporation dans votre application :

-   [Liaison d’un. JAR](~/android/platform/binding-java-library/binding-a-jar.md) est une procédure pas à pas pour la création de bibliothèques de liaisons pour **.jar** fichiers.

-   [Liaison d’un. AAR](~/android/platform/binding-java-library/binding-an-aar.md) est une procédure pas à pas pour la création de bibliothèques de liaisons pour. Fichiers AAR. Lisez cette procédure pas à pas pour apprendre à lier les bibliothèques Android Studio.

-   [Liaison d’un projet de bibliothèque Eclipse](~/android/platform/binding-java-library/binding-a-library-project.md) est une procédure pas à pas pour la création de bibliothèques de liaison à partir de projets de bibliothèque Android. Cette procédure pas à pas pour apprendre à lier des projets de bibliothèque Android Eclipse de lecture.

-   [Personnalisation des liaisons](~/android/platform/binding-java-library/customizing-bindings/index.md) explique comment apporter des modifications manuelles à la liaison pour résoudre des erreurs de build et l’API résultant de la forme afin qu’il soit plus « c#-comme ».

-   [Résolution des problèmes de liaisons](~/android/platform/binding-java-library/troubleshooting-bindings.md) répertorie les scénarios d’erreur de liaison courantes, décrit les causes possibles et propose des suggestions pour résoudre ces erreurs.


## <a name="related-links"></a>Liens associés

- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Métadonnées GAPI](http://www.mono-project.com/GAPI#Metadata)
- [Utilisation de bibliothèques natives](~/android/platform/native-libraries.md)
