---
title: "Partie 1 : présentation de la plateforme Mobile de Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: FBCEF258-D3D8-A420-79ED-3AAB4A7308E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8b3908bcbf13063daced19c9d85b95bcd0a69c37
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="part-1--understanding-the-xamarin-mobile-platform"></a>Partie 1 : présentation de la plateforme Mobile de Xamarin

La plateforme de Xamarin se compose d’un nombre d’éléments qui vous permettent de développer des applications pour iOS et Android :

-   **Langage c#** – vous permet d’utiliser une syntaxe familière et des fonctionnalités avancées telles que les génériques, LINQ et la bibliothèque parallèle de tâches.
-   **Mono .NET framework** – fournit une implémentation multiplateforme des fonctionnalités étendues de Microsoft .NET framework.
-   **Compilateur** : selon la plateforme, génère une application native (par exemple). e/s) ou une application .NET intégrée et l’exécution (par exemple). Android). Le compilateur effectue également le nombre d’optimisations de déploiement mobile tels que la liaison de rangement code non utilisé.
-   **Outils IDE** – Visual Studio sur Mac et Windows vous permet de créer, générer et déployer des projets de Xamarin.


En outre, étant donné que le langage sous-jacent est c# avec le .NET framework, les projets peuvent être structurés pour partager du code qui peut également être déployé sur Windows Phone.

 <a name="Under_the_Hood" />


## <a name="under-the-hood"></a>Sous le capot

Bien que Xamarin autorise l’écriture d’applications en c#, et de partager le même code sur plusieurs plateformes, l’implémentation réelle sur chaque système est très différente.

 <a name="Compilation" />


## <a name="compilation"></a>Compilation

La source c# effectue sa manière dans une application native de manière très différente sur chaque plateforme :

-   **iOS** – c# est anticipée de temps (AOA) compilées en langage d’assembly ARM. Le .NET framework est inclus, avec les classes inutilisés en cours sont supprimés lors de la liaison pour réduire la taille de l’application. Apple n’autorise pas la génération de code de runtime sur iOS, certaines fonctionnalités de langage ne sont pas disponibles (consultez [Xamarin.iOS Limitations](~/ios/internals/limitations.md) ).
-   **Android** – c# est compilé en langage intermédiaire et empaqueté avec MonoVM + JIT'ing. Classes non utilisés dans le framework sont supprimés lors de la liaison. L’application s’exécute à côte avec Java/ART (Android runtime) et interagit avec les types natifs via JNI (consultez [Xamarin.Android Limitations](~/android/internals/limitations.md) ).
-   **Windows** – c# est compilé en langage intermédiaire et exécuté par le runtime intégré et ne nécessite pas d’outils Xamarin. Conception d’applications Windows les conseils suivants Xamarin, plus facile de réutiliser le code sur iOS et Android.
  Notez que la plateforme Windows universelle a également un **.NET Native** option qui se comporte de la même façon pour la compilation des Xamarin.iOS AOA.


La documentation de l’éditeur de liens pour [Xamarin.iOS](~/ios/deploy-test/linker.md) et [Xamarin.Android](~/android/deploy-test/linker.md) fournit plus d’informations sur cette partie du processus de compilation.

Runtime 'compilation' – génération dynamique du code avec `System.Reflection.Emit` – doit être évitée.

Noyau de d’Apple empêche la génération de code dynamique sur les appareils iOS, par conséquent, l’émission de code à la volée ne fonctionnera pas dans Xamarin.iOS. De même, les fonctionnalités du Dynamic Language Runtime ne peut pas être utilisées avec des outils Xamarin.

Certaines fonctionnalités de réflexion ne fonctionnent pas (par exemple). MonoTouch.Dialog utilise l’API de réflexion), mais pas la génération de code.

 <a name="Platform_SDK_Access" />


## <a name="platform-sdk-access"></a>Accès de kit de développement logiciel de plate-forme

Xamarin fournit les fonctionnalités fournies par le Kit de développement spécifiques à une plateforme facilement accessible avec la syntaxe c# classique :

-   **iOS** – Xamarin.iOS expose les infrastructures de CocoaTouch SDK d’Apple sous la forme d’espaces de noms que vous pouvez référencer à partir de c#. Par exemple, le framework UIKit qui contient tous les contrôles d’interface utilisateur peut être inclus dans un simple `using UIKit;` instruction.
-   **Android** – Xamarin.Android expose Kit de développement logiciel Android de Google en tant qu’espaces de noms, donc vous pouvez référencer n’importe quelle partie du Kit de développement pris en charge avec un à l’aide de l’instruction, tel que `using Android.Views;` pour accéder aux contrôles d’interface utilisateur.
-   **Windows** – les applications Windows sont générées à l’aide de Visual Studio sur Windows. Types de projets incluent des Windows Forms, WPF, WinRT et la plateforme Windows universelle (UWP).


 <a name="Seamless_Integration_for_Developers" />


## <a name="seamless-integration-for-developers"></a>Intégration transparente pour les développeurs

L’avantage de Xamarin est que malgré les différences dans les coulisses, Xamarin.iOS et Xamarin.Android (couplée avec les kits de développement logiciel de Microsoft Windows) offrent une expérience transparente pour l’écriture de code c# qui peut être réutilisé entre les trois plateformes.

Logique métier, l’utilisation de base de données, un accès au réseau et les autres fonctions courantes peuvent écrites une seule fois et réutilisées sur chaque plateforme, et constitue une base pour les interfaces utilisateur spécifiques à la plateforme rechercher et exécutent en tant qu’applications natives.

 <a name="Integrated_Development_Environment_(IDE)_Availability" />


## <a name="integrated-development-environment-ide-availability"></a>Disponibilité de l’environnement (IDE de) développement intégré

Développement de Xamarin est possible dans Visual Studio sur Mac ou Windows. L’IDE que vous choisissez sera déterminée par les plateformes que vous souhaitez cibler.

Étant donné que les applications Windows peuvent uniquement être développées sur Windows, de génération pour iOS, Android, _et_ Windows nécessite que Visual Studio pour Windows. Toutefois, il est possible de partager des projets et des fichiers entre les ordinateurs Windows et Mac, afin que les applications Android et iOS peuvent reposer sur un Mac et code partagé peut être ajouté ultérieurement à un projet Windows.

Les spécifications de développement pour chaque plateforme sont discutées en détail dans les [exigence](~/cross-platform/get-started/requirements.md) guide.


<a name="iOS" />

### <a name="ios"></a>iOS

Développement d’applications iOS requiert un ordinateur Mac, macOS en cours d’exécution. Vous pouvez également utiliser Visual Studio pour écrire et de déployer des applications iOS avec Xamarin dans Visual Studio. Toutefois, un Mac est toujours nécessaire de build et à des fins de gestion des licences.

Apple Xcode IDE doit être installé pour fournir le compilateur et le simulateur de test. Vous pouvez tester sur votre propre appareils [gratuitement](~/ios/get-started/installation/device-provisioning/free-provisioning.md), mais pour créer des applications pour la distribution (par exemple). l’App Store), vous devez joindre Developer Program d’Apple (99 USD par an). Chaque fois que vous envoyez ou mettre à jour une application, il doit être révisé et approuvé par Apple avant d’être rendu disponible en téléchargement.

Code est écrit à l’IDE de Visual Studio et dispositions d’écran peuvent être générées par programme ou modifiées à l’aide iOS de Xamarin concepteur dans l’IDE.

Reportez-vous à la [Guide d’Installation de Xamarin.iOS](~/ios/get-started/installation/index.md) pour obtenir des instructions détaillées sur la configuration d’un projet.

<a name="Android" />

### <a name="android"></a>Android

Développement d’applications Android requiert le Java et le SDK Android pour être installé. Elles fournissent le compilateur, émulateur et autres outils requis pour la génération, déploiement et test. Les outils Java, Google Android SDK et Xamarin peuvent tous être installés et exécutés sur les configurations suivantes :

-  Mac OS X El Capitan et ci-dessus (10.11 +) avec Visual Studio pour Mac
-  Windows 7 & ci-dessus avec Visual Studio 2015 ou 2017


Xamarin fournit un programme d’installation unifiée qui permettra de configurer votre système avec le Java préalable, les outils Android et Xamarin (y compris un concepteur visuel pour les dispositions d’écran). Reportez-vous à la [Guide d’Installation de Xamarin.Android](~/android/get-started/installation/index.md) pour obtenir des instructions détaillées.

Vous pouvez générer et tester des applications sur un périphérique réel sans aucune licence à partir de Google, toutefois à distribuer votre application dans un magasin (tels que Google Play, Amazon ou Barnes &amp; Noble) une cotisation peut-être être dues à l’opérateur. Google Play publier votre application instantanément, alors que les autres magasins ont un processus d’approbation similaire à Apple.

 <a name="Windows_Phone" />


### <a name="windows"></a>Windows

Les applications Windows (Windows Forms, WPF ou UWP) sont créées avec Visual Studio. Ils n’utilisent pas directement de Xamarin. Toutefois, le code c# peut être partagé entre Windows, iOS et Android.
Visitez Microsoft [centre de développement](https://developer.microsoft.com/) pour en savoir plus sur les outils requis pour le développement Windows.

 <a name="Creating_the_User_Interface_(UI)" />


## <a name="creating-the-user-interface-ui"></a>Création de l'interface utilisateur

Des principaux avantages de l’utilisation de Xamarin est que l’interface utilisateur utilise des contrôles natifs sur chaque plateforme, créer des applications qui ne se distinguent dans une application écrite en Objective-C ou Java (pour iOS et Android respectivement).

Lors de la création d’écrans dans votre application, vous pouvez disposer les contrôles dans le code ou créer des écrans complètes à l’aide des outils de conception disponibles pour chaque plateforme.

 <a name="Programmatically_Create_Controls" />


### <a name="create-controls-programmatically"></a>Créer des contrôles par programmation

Chaque plateforme permet à des contrôles d’interface à ajouter à un écran à l’aide du code utilisateur. Cela peut être très longue, car elle peut être difficile de visualiser la conception lorsque coordonne les pixels de coder en dur pour les tailles et les positions des contrôles.

Créer par programmation des contrôles a avantages, notamment sur iOS pour la création de vues qui redimensionnement ou s’affichent différemment sur les tailles d’écran iPhone et iPad.

 <a name="Visual_Designer" />


### <a name="visual-designer"></a>Concepteur visuel

Chaque plateforme possède une autre méthode de mise en page visuellement écrans :

-   **iOS** – iOS de Xamarin concepteur facilite la création d’affichages à l’aide de glisser-déposer les champs des fonctionnalités et propriétés. Collectivement ces vues constituent un plan conceptuel et sont accessibles dans le **. Table de montage séquentiel** fichier qui est inclus dans votre projet.
-   **Android** – Xamarin fournit un concepteur de l’interface utilisateur de glisser-déposer Android pour Visual Studio. Dispositions d’écran Android sont enregistrées en tant que **. AXML** fichiers lors de l’utilisation des outils Xamarin.
-   **Windows** – Microsoft fournit un concepteur de l’interface utilisateur de glisser-déplacer dans Visual Studio et Blend. Les mises en page de l’écran sont stockés en tant que. Fichiers XAML.


Ces captures d’écran affichent les concepteurs visual écran disponibles sur chaque plateforme :

 [ ![](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png "Ces captures d’écran affichent les concepteurs visual écran disponibles sur chaque plateforme")](part-1-understanding-the-xamarin-mobile-platform-images/designer-all1.png)

Dans tous les cas, les éléments que vous créez visuellement peuvent être référencées dans votre code.

 <a name="User_Interface_Considerations" />


### <a name="user-interface-considerations"></a>Considérations sur l’Interface utilisateur

Des principaux avantages de l’utilisation de Xamarin pour créer des applications inter-plateformes est qu’ils peuvent tirer parti de boîtes à outils de l’interface utilisateur native pour présenter une interface familière pour l’utilisateur. L’interface utilisateur sera également aussi rapides que toute autre application native.

Certains thèmes de l’interface utilisateur fonctionnent sur plusieurs plateformes (par exemple, les trois plateformes utilisent un contrôle de liste de défilement similaire) mais pour votre application se sentir droite l’interface utilisateur doit tirer parti d’un utilisateur spécifique à la plateforme des éléments d’interface lorsque cela est approprié. Voici quelques exemples de métaphores d’interface utilisateur spécifique à la plateforme :

-   **iOS** – navigation hiérarchique avec logicielle bouton précédent, onglets au bas de l’écran.
-   **Android** – matériel/système logiciel nouveau bouton, menu action, les onglets en haut de l’écran.
-   **Windows** – les applications de Windows peuvent s’exécuter sur les ordinateurs de bureau, les tablettes (tels que Microsoft Surface) et les téléphones. Appareils Windows 10 peuvent avoir bouton Précédent et vignettes dynamiques, par exemple.


Il est recommandé de lire les règles de conception pertinents pour les plateformes que vous ciblez :

-   **iOS** – [indications de l’Interface humaine Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html)
-   **Android** – [indications de l’Interface utilisateur de Google](http://developer.android.com/guide/practices/ui_guidelines/index.html)
-   **Windows** – [règles de conception d’expérience utilisateur pour Windows](https://developer.microsoft.com/en-us/windows/design)


 <a name="Library_and_Code_Re-use" />


## <a name="library-and-code-re-use"></a>Bibliothèque et la réutilisation de Code

La plateforme Xamarin permet de réutiliser du code c# existant sur toutes les plateformes, ainsi que l’intégration de bibliothèques écrits en mode natif pour chaque plateforme.

 <a name="C#_Source_and_Libraries" />


### <a name="c-source-and-libraries"></a>Bibliothèques et Source c#

Étant donné que les produits Xamarin utilisent c# et le .NET framework, une grande quantité de code source existant (à la fois open source et les projets internes) peut être utilisée dans les projets Xamarin.iOS et Xamarin.Android à nouveau. Souvent la source peut simplement être ajoutée à une solution Xamarin et elle sera immédiatement. Si une fonctionnalité de framework .NET non pris en charge a été utilisée, quelques ajustements peuvent être requis.

Exemples de code source c# qui peut être utilisé dans Xamarin.iOS ou Xamarin.Android : NET de SQLite, NewtonSoft.JSON et SharpZipLib.

 <a name="Objective-C_Bindings_+_Binding_Projects" />


### <a name="objective-c-bindings--binding-projects"></a>Les liaisons objective-C + des projets de liaison

Xamarin fournit un outil appelé *btouch* que vous aide à créer des liaisons qui autorisent les bibliothèques Objective-C à utiliser dans les projets Xamarin.iOS. Reportez-vous à la [documentation de la liaison des Types Objective-C](~/cross-platform/macios/binding/binding-types-reference.md) pour plus d’informations sur la façon de procéder.

Exemples de bibliothèques Objective-C qui peuvent être utilisés dans Xamarin.iOS : code-barres RedLaser analyse, l’intégration Analytique de Google et PayPal. Liaisons de Xamarin.iOS Open source sont disponibles sur [github](https://github.com/mono/monotouch-bindings).

 <a name=".jar_Bindings_+_Binding_Projects" />


### <a name="jar-bindings--binding-projects"></a>les liaisons .jar + projets de liaison

Xamarin prend en charge l’utilisation de bibliothèques de Java existants dans Xamarin.Android. Reportez-vous à la [liaison une documentation bibliothèque Java](~/android/platform/binding-java-library/index.md) pour plus d’informations sur l’utilisation d’un. Fichier JAR à partir de Xamarin.Android.

Liaisons de Xamarin.Android Open source sont disponibles sur [github](https://github.com/mono/monodroid-bindings).

 <a name="C_via_PInvoke" />


### <a name="c-via-pinvoke"></a>C via PInvoke

Technologie de « L’appel de plate-forme » (P/Invoke) permet à du code managé (c#) pour appeler les méthodes dans les bibliothèques natives ainsi que la prise en charge des bibliothèques natives appeler dans du code managé.

Par exemple, le [SQLite-NET](https://github.com/praeclarum/sqlite-net) bibliothèque utilise des instructions telles que :

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open", CallingConvention=CallingConvention.Cdecl)]
public static extern Result Open (string filename, out IntPtr db);
```

Cela lie à l’implémentation de langage C SQLite native dans iOS et Android.
Les développeurs familiarisés avec une API C existante peuvent construire un jeu de classes c# pour mapper à l’API native et utiliser le code existant de la plateforme. Il existe une documentation pour [liaison bibliothèques natives](~/ios/platform/native-interop.md) dans Xamarin.iOS, les mêmes principes s’appliquent à Xamarin.Android.

 <a name="C++_via_Cxxi" />


### <a name="c-via-cppsharp"></a>C++ via CppSharp

Miguel explique CXXI (maintenant appelé [CppSharp](https://github.com/mono/CppSharp)) sur son [blog](http://tirania.org/blog/archive/2011/Dec-19.html). Une liaison directement à une bibliothèque C++ consiste à créer un wrapper de C et de lier à celle via P/Invoke.

