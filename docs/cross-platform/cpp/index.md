---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Utiliser des bibliothèques C/C++ avec Xamarin
description: Visual Studio pour Mac peut être utilisé pour créer et intégrer des applications mobiles pour Android et iOS, du code de C/C++ inter-plateformes à l’aide de Xamarin et C#. Cet article explique comment installer et déboguer un projet C++ dans une application Xamarin.
author: mikeparker104
ms.author: miparker
ms.date: 12/17/2018
ms.openlocfilehash: a235a24d544e938d4bf29e6569564aface2f6972
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275579"
---
# <a name="use-cc-libraries-with-xamarin"></a>Utiliser des bibliothèques C/C++ avec Xamarin

## <a name="overview"></a>Vue d'ensemble

Xamarin permet aux développeurs de créer des applications mobiles natives multiplateformes avec Visual Studio. En règle générale, C# liaisons sont utilisées pour exposer des composants de plateforme existants pour les développeurs. Toutefois, il existe des heures lorsque des codes base Xamarin application doivent fonctionner avec existant. Parfois les équipes ne sont simplement le temps, budget ou des ressources au port une grande, bien testée et hautement optimisée codebase pour C#.

[Visual C++ pour le développement mobile multiplateforme](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) permet le C/C++ et C# code à générer dans le cadre de la même solution, offre de nombreux avantages, notamment une expérience de débogage unifiée. Microsoft a utilisé C/C++ et Xamarin de cette façon pour distribuer des applications telles que [Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) et [Pix caméra](https://www.microsoft.com/microsoftpix).

Toutefois, dans certains cas il est a un désir (ou une exigence) pour conserver les outils C/C++ et les processus en place et de conserver le code de bibliothèque découplé à partir de l’application, en traitant de la bibliothèque comme si elle était similaire à un composant tiers existants. Dans ces situations, le défi pas uniquement exposé par les membres concernés à C# , mais la gestion de la bibliothèque en tant que dépendance. Et, bien sûr, automatiser autant de ce processus que possible.  

Cet article présente une approche de haut niveau pour ce scénario et présente un exemple simple.

## <a name="background"></a>Présentation

C/C++ est considéré comme un langage inter-plateformes, mais doit faire très attention pour s’assurer que le code source est en effet multiplateforme, à l’aide uniquement C/C++ pris en charge par tous les compilateurs de cible et qui contient peu ou aucune plateforme inclus de manière conditionnelle ou code de compilateur spécifique.

Finalement, le code doit compiler et exécuter avec succès sur toutes les plateformes cibles par conséquent, cela revient aux points communs entre les plateformes (et les compilateurs) qui est ciblé. Problèmes peuvent toujours survenir à partir des différences mineures entre les compilateurs et par conséquent, effectuer des tests approfondis (de préférence automatisée) sur chaque cible de la plateforme prend une importance cruciale.  

## <a name="high-level-approach"></a>Approche de haut niveau

L’illustration ci-dessous représente l’approche de quatre étapes permettant de transformer le code source C/C++ dans une bibliothèque de Xamarin multiplateforme qui est partagée par le biais de NuGet et qui est ensuite utilisée dans une application Xamarin.Forms.
 

![Approche de haut niveau pour l’utilisation de C/C++ avec Xamarin](images/cpp-steps.jpg)

Les 4 étapes sont :

1.  Compilation du code source C/C++ dans les bibliothèques natives spécifiques à la plateforme.
2.  Encapsulant les bibliothèques natives avec une solution Visual Studio.
3.  Compression et l’envoi d’un package NuGet pour le wrapper .NET.
4.  Consomme le package NuGet à partir d’une application Xamarin.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>Étape 1 : Compiler le code source C/C++ dans les bibliothèques natives spécifiques à la plateforme

L’objectif de cette phase consiste à créer des bibliothèques natives qui peuvent être appelées par le C# wrapper. Cela peut être ou non pertinente en fonction de votre situation. Les nombreux outils et processus qui peuvent être amenés à porter dans ce scénario courant sortent du cadre de cet article. Considérations relatives à la clé sont le C/C++ codebase synchronisé avec n’importe quel code wrapper natif, suffisamment tests unitaires, de conservation et automatisation de la génération. 

Les bibliothèques dans la procédure ont été créés à l’aide de Visual Studio Code avec un script shell qui accompagne cet article. Vous trouverez une version étendue de cette procédure pas à pas dans le [Mobile CAT référentiel](https://github.com/xamarin/mobcat/blob/dev/samples/cppwithxamarin/README.md) qui traite de cette partie de l’exemple de manière plus approfondie. Les bibliothèques natives sont traitées comme une dépendance tierce dans ce cas toutefois, cette étape est illustrée pour le contexte.


Par souci de simplicité, la procédure pas à pas cible uniquement un sous-ensemble des architectures. Pour iOS, il utilise l’utilitaire lipo pour créer un seul fat binaire des binaires propres à l’architecture individuels. Android utilisera les fichiers binaires dynamiques avec une extension .so et iOS utilisera un statique fat binaire avec une extension .a. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>Étape 2 : Encapsulant les bibliothèques natives avec une solution Visual Studio

L’étape suivante consiste à encapsuler les bibliothèques natives pour qu’ils sont facilement utilisés à partir de .NET. Cela est effectué avec une solution Visual Studio avec quatre projets. Un projet partagé contient le code commun. Les projets ciblant chacun de Xamarin.Android, Xamarin.iOS et .NET Standard que la bibliothèque devant être référencé de manière indépendante de la plateforme.

Utilise le wrapper[l’astuce soit](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/), « décrite par Paul Betts. Ce n’est pas la seule façon, mais il facilite référencer la bibliothèque et elle évite d’avoir à gérer explicitement les implémentations spécifiques à la plateforme au sein de l’application elle-même. L’astuce consiste essentiellement à s’assurer que les cibles (.NET Standard, Android, iOS) partagent le même espace de noms, le nom de l’assembly et la structure de classe. Étant donné que NuGet préfère toujours une bibliothèque spécifique à la plateforme, la version .NET Standard n’est jamais utilisée lors de l’exécution.

L’essentiel du travail dans cette étape se concentrera sur à l’aide de P/Invoke pour appeler les méthodes de la bibliothèque native et de gérer les références aux objets sous-jacents. L’objectif est d’exposer les fonctionnalités de la bibliothèque au consommateur tout en extrayant des tout niveau de complexité. Les développeurs Xamarin.Forms est inutile d’avoir des connaissances sur le fonctionnement interne de la bibliothèque non managée. Il doit le sentiment qu’ils sont à l’aide de n’importe quel autre gérés C# bibliothèque.

Finalement, la sortie de cette phase est un ensemble de bibliothèques .NET, un par la cible, ainsi que d’un document nuspec qui contient les informations nécessaires pour générer le package à l’étape suivante.

**Étape 3 : Compression et l’envoi d’un package NuGet pour le wrapper .NET**

La troisième étape consiste à créer un package NuGet avec les artefacts de build à partir de l’étape précédente. Le résultat de cette étape est un package NuGet qui peut être consommé à partir d’une application Xamarin. La procédure pas à pas utilise un répertoire local pour servir le flux NuGet. En production, cette étape doit publier un package à un public ou privé NuGet flux et doit être entièrement automatisé.

**Étape 4 : Utilisation du package NuGet à partir d’une application Xamarin.Forms**

L’étape finale consiste à référencer et utiliser le package NuGet à partir d’une application Xamarin.Forms. Cela nécessite la configuration du package NuGet de flux dans Visual Studio pour utiliser le flux défini dans l’étape précédente.

Une fois que le flux est configuré, le package doit être référencé à partir de chaque projet dans l’application Xamarin.Forms multiplateforme. « L’astuce bait-and-switch » fournit les interfaces identiques, les fonctionnalitésent de la bibliothèque native peut donc être appelée à l’aide de code défini dans un emplacement unique.

Le référentiel de code source contient un [liste des articles complémentaires](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up) qui comprend des articles sur la configuration d’un flux sur Azure DevOps de NuGet privé et comment envoyer le package à ce flux. Tout en nécessitant un peu plus de temps le programme d’installation à un répertoire local, ce type de flux est préférable dans un environnement de développement d’équipe.

## <a name="walk-through"></a>Procédure pas à pas

Les étapes indiquées sont spécifiques à **Visual Studio pour Mac**, mais fonctionne de la structure de **Visual Studio 2017** également.

### <a name="prerequisites"></a>Prérequis

Pour suivre la procédure, le développeur doit :

-   [NuGet, ligne de commande (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

-   [*Visual Studio* *pour Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> Un actif [ **compte de développeur Apple** ](https://developer.apple.com/) est requis pour déployer des applications sur un iPhone.

## <a name="creating-the-native-libraries-stage-1"></a>Création des bibliothèques natives (étape 1)

La fonctionnalité de bibliothèque native est basée sur l’exemple à partir de [procédure pas à pas : Création et utilisation d’une bibliothèque statique (C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017).

Cette procédure pas à pas ignore la première phase, créer les bibliothèques natives, étant donné que la bibliothèque est fournie comme une dépendance tierce dans ce scénario. Les bibliothèques natives précompilés sont inclus en même temps que le [exemple de code](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin) ou peut être [téléchargé](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) directement.

### <a name="working-with-the-native-library"></a>Utilisation de la bibliothèque native

La version d’origine *MathFuncsLib* exemple inclut une classe unique appelée MyMathFuncs avec la définition suivante : 

```cpp
namespace MathFuncs
{
    class MyMathFuncs
    {
    public:
        double Add(double a, double b);
        double Subtract(double a, double b);
        double Multiply(double a, double b);
        double Divide(double a, double b);
    };
}
```

Une classe supplémentaire définit les fonctions de wrapper qui permettent un consommateur de .NET créer, supprimer et interagir avec la classe MyMathFuncs native sous-jacente.

```cpp
#include "MyMathFuncs.h"
using namespace MathFuncs;

extern "C" {
    MyMathFuncs* CreateMyMathFuncsClass();
    void DisposeMyMathFuncsClass(MyMathFuncs* ptr);
    double MyMathFuncsAdd(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsSubtract(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsMultiply(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsDivide(MyMathFuncs *ptr, double a, double b);
}
```

Il s’agit de ces fonctions wrapper utilisées sur le [Xamarin](https://visualstudio.microsoft.com/xamarin/) côté.

## <a name="wrapping-the-native-library-stage-2"></a>Habillage de la bibliothèque native (étape 2)

Cette étape nécessite la [précompilé bibliothèques](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) décrit dans le [section précédente](https://docs.microsoft.com/xamarin/cross-platform/cpp/index).

### <a name="creating-the-visual-studio-solution"></a>Création de la solution Visual Studio

1. Dans **Visual Studio pour Mac**, cliquez sur **nouveau projet** (à partir de la *Page d’accueil*) ou **nouvelle Solution** (à partir de la *fichier* menu).
2. À partir de la **nouveau projet** fenêtre, choisissez **projet partagé** (depuis *multiplateforme > bibliothèque*) puis cliquez sur **suivant**.
3. Mettre à jour les champs suivants, puis cliquez sur **créer**:

    - **Nom du projet :** MathFuncs.Shared  
    - **Nom de la solution :** MathFuncs  
    - **Emplacement :** Utilisez la valeur par défaut emplacement d’enregistrement (ou choisir une alternative)   
    - **Créez un projet dans le répertoire de solution :** Définissez cette option pour activée
4. À partir de **l’Explorateur de solutions**, double-cliquez sur le **MathFuncs.Shared** de projet et accédez à **principaux paramètres**.
5. Supprimer **. Partagé** à partir de la **Namespace par défaut** , elle est définie **MathFuncs** uniquement, puis cliquez sur **OK**.
6. Ouvrez **MyClass.cs** (créée par le modèle), puis renommez la classe et le nom de fichier à **MyMathFuncsWrapper** et modifiez l’espace de noms **MathFuncs**.
7. **CONTRÔLE + clic** sur la solution **MathFuncs**, puis choisissez **ajouter un nouveau projet...**  à partir de la **ajouter** menu.
8. À partir de la **nouveau projet** fenêtre, choisissez **bibliothèque .NET Standard** (depuis *multiplateforme > bibliothèque*) puis cliquez sur **suivant**.
9. Choisissez **.NET Standard 2.0** puis cliquez sur **suivant**.
10. Mettre à jour les champs suivants, puis cliquez sur **créer**:

    - **Nom du projet :** MathFuncs.Standard  
    - **Emplacement :** Utiliser le même emplacement que le projet partagé d’enregistrement   

11. À partir de **l’Explorateur de solutions**, double-cliquez sur le **MathFuncs.Standard** projet.
12. Accédez à **principaux paramètres**, puis mettez à jour **Namespace par défaut** à **MathFuncs**.
13. Accédez à la **sortie** paramètres, puis mettez à jour **nom de l’Assembly** à **MathFuncs**.
14. Accédez à la **compilateur** modifier les paramètres, le **Configuration** à **version**, la définition **les informations de débogage** à  **Symboles uniquement** puis cliquez sur **OK**.
15. Supprimer **Class1.cs/Getting démarré** à partir du projet (si un d’eux a été inclus dans le cadre du modèle).
16. **CONTRÔLE + clic** sur le projet **dépendances/références** dossier, puis choisissez **modifier les références**.
17. Sélectionnez **MathFuncs.Shared** à partir de la **projets** onglet, puis cliquez sur **OK**.
18. Répétez les étapes 7-17 (il ignore l’étape 9) en utilisant les configurations suivantes :

    | **NOM DU PROJET**  | **NOM DU MODÈLE**   | **MENU NOUVEAU PROJET**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | Bibliothèque de classes       | Android > bibliothèque      |
    | MathFuncs.iOS     | Bibliothèque de liaison     | iOS > bibliothèque          |

19. À partir de **l’Explorateur de solutions**, double-cliquez sur le **MathFuncs.Android** de projet, puis accédez à la **compilateur** paramètres.

20. Avec le **Configuration** définie sur **déboguer**, modifier **définissent les symboles** à inclure **Android ;**.

21. Modifier le **Configuration** à **version**, puis modifiez **définissent les symboles** pour inclure également les **Android ;**.

22. Répétez les étapes 19 et 20 pour **MathFuncs.iOS**, Édition **définissent les symboles** à inclure **iOS ;** au lieu de **Android ;** dans les deux cas.

23. Générez la solution **version** configuration (**contrôle + commande + B**) et valider que tous les assemblys de sortie trois (Android, iOS, .NET Standard) (dans les dossiers bin du projet respectifs) partagent le même nom **MathFuncs.dll**.

À ce stade, la solution doit avoir trois cibles, une pièce pour Android, iOS et .NET Standard et un projet partagé référencé par chacun des trois cibles. Ceux-ci doivent être configurés pour utiliser les assemblys d’espace de noms et de sortie par défaut mêmes portant le même nom. Cela est nécessaire pour l’approche « appât and switch » mentionné précédemment.

### <a name="adding-the-native-libraries"></a>Ajouter les bibliothèques natives

Le processus d’ajout des bibliothèques natives à la solution de wrapper varie légèrement entre Android et iOS.

#### <a name="native-references-for-mathfuncsandroid"></a>Références natives pour MathFuncs.Android

1. **CONTRÔLE + clic** sur le **MathFuncs.Android** de projet, puis choisissez **nouveau dossier** à partir de la **ajouter** menu nommant **libs**.

2. Pour chaque **ABI** (Application Binary Interface), **contrôle + clic** sur le **libs** dossier, puis choisissez **nouveau dossier** à partir de la  **Ajouter** menu, nommez-le après cela respectifs **ABI**. Dans ce cas :

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > Pour une présentation plus détaillée, consultez le [Architectures et les unités centrales](https://developer.android.com/ndk/guides/arch) rubrique à partir de la [guide du développeur NDK](https://developer.android.com/ndk/guides/), en particulier la section sur l’adressage [code natif dans des packages d’application](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages) .

3. Vérifiez la structure de dossiers :  

    ```
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. Ajouter à le **.so** à chacune des bibliothèques de la **ABI** dossiers en fonction du mappage suivant :

    **arm64-v8a :** libs/Android/arm64

    **armeabi-v7a :** libs/Android/arm  

    **x86 :** libs/Android/x86

    **x86_64 :** libs/Android/x86_64

    > [!NOTE]
    > Pour ajouter des fichiers, **contrôle + clic** sur le dossier représentant respectifs **ABI**, puis choisissez **ajouter des fichiers...**  à partir de la **ajouter** menu. Choisir la bibliothèque appropriée (à partir de la **PrecompiledLibs** directory) puis cliquez sur **Open** puis cliquez sur **OK** en laissant l’option par défaut à *copie le fichier dans le répertoire*.

5. Pour chacun de la **.so** fichiers, **contrôle + clic** puis choisissez le **EmbeddedNativeLibrary** option à partir de la **Action de génération** menu.

Maintenant le **libs** dossier doit apparaître comme suit :

```bash
- lib
    - arm64-v8a
        - libMathFuncs.so
    - armeabi-v7a
        - libMathFuncs.so
    - x86 
        - libMathFuncs.so
    - x86_64 
        - libMathFuncs.so
```

#### <a name="native-references-for-mathfuncsios"></a>Références natives pour MathFuncs.iOS

1. **CONTRÔLE + clic** sur le **MathFuncs.iOS** de projet, puis choisissez **ajouter une référence Native** à partir de la **ajouter** menu. 
2. Choisissez le **libMathFuncs.a** bibliothèque (à partir de bibliothèques/ios sous le **PrecompiledLibs** directory) puis cliquez sur **ouvert** 
3. **CONTRÔLE + clic** sur le **libMathFuncs** fichier (au sein de la **références natives** dossier, puis choisissez le **propriétés** option dans le menu  
4. Configurer le **référence Native** propriétés afin qu’ils sont activés (affichant une icône représentant une coche) dans le **propriétés** panneau :
        
    - Charge de force
    - Est en C++
    - Liaison intelligente 

    > [!NOTE]
    > À l’aide d’un type de projet de bibliothèque de liaison avec un [référence native](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) incorpore la bibliothèque statique et lui permet d’être automatiquement liées avec l’application Xamarin.iOS qui y fait référence (même si elle est incluse à l’aide d’un package NuGet). 

5. Ouvrez **ApiDefinition.cs**, suppression le basé sur un modèle de code placé (ce qui laisse uniquement le **MathFuncs** espace de noms), puis effectuez la même procédure pour **Structs.cs** 

    > [!NOTE]
    > Un projet de bibliothèque de liaison requiert ces fichiers (avec le *ObjCBindingApiDefinition* et *ObjCBindingCoreSource* actions de génération) afin de créer. Toutefois, nous allons écrire le code, pour appeler notre bibliothèque native, en dehors de ces fichiers d’une façon qui peut être partagé entre les cibles de bibliothèque iOS et Android à l’aide de P/Invoke standard.

### <a name="writing-the-managed-library-code"></a>Écriture du code de la bibliothèque managée

Maintenant, écrire le C# code pour appeler la bibliothèque native. L’objectif est de masquer toute la complexité sous-jacente. Le consommateur ne devez pas toute connaissance des éléments internes de bibliothèque native ou des concepts de P/Invoke.  

#### <a name="creating-a-safehandle"></a>Création d’un SafeHandle

1. **CONTRÔLE + clic** sur le **MathFuncs.Shared** de projet, puis choisissez **ajouter un fichier...**  à partir de la **ajouter** menu. 
2. Choisissez **classe vide** à partir de la **nouveau fichier** fenêtre, nommez-le **MyMathFuncsSafeHandle** puis cliquez sur **New**
3. Implémentez le **MyMathFuncsSafeHandle** classe :

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => this.handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > Un [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) est la meilleure façon de travailler avec des ressources non managées dans le code managé. Cela élimine un grand nombre de code réutilisable lié au cycle de vie objet et finalisation critique. Le propriétaire de ce handle peut le traiter par la suite comme toute autre ressource gérée et que vous ne devrez pas mettre en œuvre de la version complète [modèle JETABLE](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose). 

#### <a name="creating-the-internal-wrapper-class"></a>Création de la classe wrapper interne

1. Ouvrez **MyMathFuncsWrapper.cs**, modifiez-le pour une classe statique interne

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. Dans le même fichier, ajoutez l’instruction conditionnelle suivante à la classe :

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > Cela définit le **DllName** valeur constante selon si la bibliothèque est générée pour **Android** ou **iOS**. Cela est de relever les conventions d’affectation de noms différents utilisées par chaque plateforme correspondante, mais aussi le type de bibliothèque qui est utilisée dans ce cas. Android utilise une bibliothèque dynamique et par conséquent, attend un nom de fichier, y compris d’extension. Pour iOS, ’*__Internal*' est nécessaire, car nous utilisons une bibliothèque statique.

3. Ajoutez une référence à **System.Runtime.InteropServices** en haut de la **MyMathFuncsWrapper.cs** fichier

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. Ajoutez les méthodes de wrapper pour gérer la création et la suppression de la **MyMathFuncs** classe :

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > Nous passons dans notre constante **DllName** à la **DllImport** attribut avec le **EntryPoint** qui indique explicitement au runtime .NET le nom de la fonction à appeler dans cette bibliothèque. Techniquement, nous n’avez pas besoin de fournir le **EntryPoint** valeur si les noms de méthode managée étaient identiques à celle qui est non managé. Si aucune n’est pas fournie, le nom de méthode managée serait être utilisé comme le **EntryPoint** à la place. Toutefois, il est préférable d’être explicite.  

5. Ajoutez les méthodes de wrapper pour nous permettre de travailler avec les **MyMathFuncs** classe utilisant le code suivant :

    ```csharp
    [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
    internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
    internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
    internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
    internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
    ```

    > [!NOTE]
    > Nous utilisons des types simples pour les paramètres dans cet exemple. Étant donné que le regroupement est une copie au niveau du bit dans ce cas il ne requiert aucune action supplémentaire de notre part. Notez également l’utilisation de la **MyMathFuncsSafeHandle** classe au lieu de la norme **IntPtr**. Le **IntPtr** est automatiquement mappé à la **SafeHandle** dans le cadre du processus de marshaling.

6. Vérifiez que le terminé **MyMathFuncsWrapper** classe apparaît sous la forme ci-dessous :

    ```csharp
    using System.Runtime.InteropServices;

    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
            #if Android
                const string DllName = "libMathFuncs.so";
            #else
                const string DllName = "__Internal";
            #endif

            [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
            internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

            [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
            internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);

            [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
            internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
            internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
            internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
            internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
        }
    }
    ```

#### <a name="completing-the-mymathfuncssafehandle-class"></a>Fin de la classe MyMathFuncsSafeHandle
1. Ouvrir le **MyMathFuncsSafeHandle** class, accédez à l’espace réservé **TODO** commentaire dans le **ReleaseHandle** méthode :
    ```csharp
    // TODO: Release the handle here
    ```
2. Remplacez le **TODO** ligne :

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>Écriture de la classe MyMathFuncs

Maintenant que le wrapper est terminé, créez une classe MyMathFuncs qui gèrera la référence à l’objet MyMathFuncs de C++ non managé.  

1. **CONTRÔLE + clic** sur le **MathFuncs.Shared** de projet, puis choisissez **ajouter un fichier...**  à partir de la **ajouter** menu. 
2. Choisissez **classe vide** à partir de la **nouveau fichier** fenêtre, nommez-le **MyMathFuncs** puis cliquez sur **New**
3. Ajoutez les membres suivants à la **MyMathFuncs** classe :

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. Implémenter le constructeur de la classe afin qu’il crée et stocke un handle natif **MyMathFuncs** objet lors de la classe est instanciée :

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. Implémentez le **IDisposable** interface utilisant le code suivant :

    ```csharp
    public class MyMathFuncs : IDisposable
    {
        ...

        protected virtual void Dispose(bool disposing)
        {
            if (handle != null && !handle.IsInvalid)
                handle.Dispose();
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        // ...
    }
    ```

6. Implémentez le **MyMathFuncs** méthodes à l’aide de la **MyMathFuncsWrapper** classe pour effectuer le travail sous le capot en passant le pointeur que nous avons stocké à l’objet non managé sous-jacent. Le code doit se présenter comme suit :

    ```csharp
    public double Add(double a, double b)
    {
        return MyMathFuncsWrapper.Add(handle, a, b);
    }

    public double Subtract(double a, double b)
    {
        return MyMathFuncsWrapper.Subtract(handle, a, b);
    }

    public double Multiply(double a, double b)
    {
        return MyMathFuncsWrapper.Multiply(handle, a, b);
    }

    public double Divide(double a, double b)
    {
        return MyMathFuncsWrapper.Divide(handle, a, b);
    }
    ```

#### <a name="creating-the-nuspec"></a>Créer le fichier nuspec

Pour que la bibliothèque empaquetés et distribués via NuGet, la solution doit un **nuspec** fichier. Cela identifie parmi les assemblys qui en résulte à inclure pour chaque plateforme prise en charge.

1.  **CONTRÔLE + clic** sur la solution **MathFuncs**, puis choisissez **ajouter un dossier Solution** à partir de la **ajouter** menu nommant **SolutionItems**.
2.  **CONTRÔLE + clic** sur le **SolutionItems** dossier, puis choisissez **nouveau fichier...**  à partir de la **ajouter** menu.
3.  Choisissez **fichier XML vide** à partir de la **nouveau fichier** fenêtre, nommez-le **MathFuncs.nuspec** puis cliquez sur **New**.
4.  Mise à jour **MathFuncs.nuspec** avec les métadonnées de package de base à afficher pour le **NuGet** consommateur. Exemple :


    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>
            <id>MathFuncs</id>
            <version>$version$</version>
            <authors>Microsoft Mobile Customer Advisory Team</authors>
            <description>Sample C++ Wrapper Library</description>
            <requireLicenseAcceptance>false</requireLicenseAcceptance>
            <copyright>Copyright 2018</copyright>
        </metadata>
    </package>
    ```

    > [!NOTE]
    >  Consultez le [de référence sur nuspec](https://docs.microsoft.com/nuget/reference/nuspec) document pour plus d’informations sur le schéma utilisé pour ce manifeste.

5. Ajouter un `<files>` élément en tant qu’enfant de le `<package>` élément (juste en dessous `<metadata>`), identifiant chaque fichier avec un distinct `<file>` élément :

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->        

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > Lorsqu’un package est installé dans un projet et où il existe plusieurs assemblys spécifiés par le même nom, NuGet choisit efficacement l’assembly qui est plus spécifique à la plateforme donnée.

6. Ajouter le `<file>` éléments pour le **Android** assemblys :

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. Ajouter le `<file>` éléments pour le **iOS** assemblys :

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. Ajouter le `<file>` éléments pour le **netstandard2.0** assemblys :

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. Vérifiez le **nuspec** manifeste :

    ```xml
    <?xml version="1.0"?>
    <package>
    <metadata>
        <id>MathFuncs</id>
        <version>$version$</version>
        <authors>Microsoft Mobile Customer Advisory Team</authors>
        <description>Sample C++ Wrapper Library</description>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <copyright>Copyright 2018</copyright>
    </metadata>
    <files>
    
        <!-- Android -->
        <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
        <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
        
        <!-- iOS -->
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
        
        <!-- netstandard2.0 -->
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />

    </files>
    </package>
    ```

    > [!NOTE]
    > Ce fichier spécifie les chemins de sortie d’assembly à partir d’un **version** build, par conséquent, veillez à générer la solution à l’aide de cette configuration.

À ce stade, la solution contient 3 assemblys .NET et une prise en charge **nuspec** manifeste.

## <a name="distributing-the-net-wrapper-with-nuget"></a>Distribuer le wrapper .NET avec NuGet

L’étape suivante consiste à empaqueter et distribuer le package NuGet afin qu’il peut être facilement consommée par l’application et géré en tant que dépendance. Retour à la ligne et la consommation de peuvent être effectuées dans une seule solution, mais la distribution de la bibliothèque via contribue à NuGet découplage et permet de nous de gérer ces codes base indépendamment.

### <a name="preparing-a-local-packages-directory"></a>Préparation d’un répertoire local de packages

La forme la plus simple de flux NuGet est un répertoire local :

1.  Dans **Finder**, accédez à un répertoire approprié. Par exemple, **/utilisateurs**.
2.  Choisissez **nouveau dossier** à partir de la **fichier** menu, donnez un nom significatif comme **local-nuget-flux**.

### <a name="creating-the-package"></a>Création du package

1.  Définir le **Configuration de Build** à **version**et exécuter une build à l’aide **commande + B**.
2.  Ouvrez **Terminal** , puis accédez au dossier contenant le **nuspec** fichier.
3.  Dans **Terminal**, exécuter le **nuget pack** commande en spécifiant le **nuspec** fichier, le **Version** (par exemple, 1.0.0) et le  **OutputDirectory** à l’aide du dossier créé à le [étape précédente](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed), autrement dit, **local-nuget-flux**. Exemple :

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **Confirmer** qui **MathFuncs.1.0.0.nupkg** a été créé dans le **local-nuget-flux** directory.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[FACULTATIF] À l’aide d’une privée le flux NuGet avec Azure DevOps

Une technique plus robuste est décrite dans [prise en main les packages NuGet dans Azure DevOps](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package), qui montre comment créer un flux privé et de transmettre le package (généré à l’étape précédente) à ce flux.

Il est fortement conseillé de disposer de ce flux de travail entièrement automatisée, par exemple à l’aide de [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts). Pour plus d’informations, consultez [prise en main Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts).

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Utilisation du wrapper de .NET à partir d’une application Xamarin.Forms
Pour exécuter la procédure pas à pas, créez un **Xamarin.Forms** application pour utiliser le package simplement publiée local **NuGet** flux.

### <a name="creating-the-xamarinforms-project"></a>Création de la **Xamarin.Forms** projet

1. Ouvrez une nouvelle instance de **Visual Studio pour Mac**. Cela peut être effectuée depuis **Terminal**:

    ```bash
    open -n -a "Visual Studio"
    ```

2. Dans **Visual Studio pour Mac**, cliquez sur **nouveau projet** (à partir de la *Page d’accueil*) ou **nouvelle Solution** (à partir de la *fichier* menu).
3. À partir de la **nouveau projet** fenêtre, choisissez **application Forms vide** (depuis *multiplateforme > application*) puis cliquez sur **suivant**.
4. Mettre à jour les champs suivants, puis cliquez sur **suivant**:

    - **Nom de l’application :** MathFuncsApp.
    - **Identificateur d’organisation :** Utiliser un espace de noms inverse, par exemple, _com. {your_org}_.
    - **Plateformes cibles :** Utilisez la valeur par défaut (iOS et Android cibles).
    - **Code partagé :** Définissez ce paramètre sur .NET Standard (une solution « Bibliothèque partagée » est possible, mais dépasse le cadre de cette procédure pas à pas).

5. Mettre à jour les champs suivants, puis cliquez sur **créer**:

    - **Nom du projet :** MathFuncsApp.
    - **Nom de la solution :** MathFuncsApp.  
    - **Emplacement :** Utilisez la valeur par défaut emplacement d’enregistrement (ou choisir une alternative).

6. Dans **l’Explorateur de solutions**, **contrôle + clic** sur la cible (**MathFuncsApp.Android** ou **MathFuncs.iOS**) pour le test initial, puis Choisissez **définir comme projet de démarrage**.
7. Choisissez la méthode favorite **appareil** ou **simulateur**/**émulateur**. 
8. Exécuter la solution (**commande + retour**) pour valider le basé sur un modèle **Xamarin.Forms** projet généré et exécuté OK. 

    > [!NOTE]
    > **iOS** (en particulier le simulateur) a tendance à avoir le temps de génération/déploiement le plus rapide.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>Ajout du package NuGet local flux à la configuration de NuGet

1. Dans **Visual Studio**, choisissez **préférences** (à partir de la **Visual Studio** menu).
2. Choisissez **Sources** sous le **NuGet** section, puis cliquez sur **ajouter**.
3. Mettre à jour les champs suivants, puis cliquez sur **ajouter une Source de**:

    - **Nom :** Entrez un nom explicite, par exemple, Local-Packages.  
    - **Emplacement :** Spécifiez le **local-nuget-flux** dossier créé dans le [étape précédente](#preparing-a-local-packages-directory).

    > [!NOTE]
    > Dans ce cas, il est inutile de spécifier un **nom d’utilisateur** et **mot de passe**. 

4. Cliquez sur **OK**.

### <a name="referencing-the-package"></a>Référencement du package

Répétez les étapes suivantes pour chaque projet (**MathFuncsApp**, **MathFuncsApp.Android**, et **MathFuncsApp.iOS**).

1. **CONTRÔLE + clic** sur le projet, puis choisissez **ajouter des Packages NuGet...**  à partir de la **ajouter** menu.
2. Recherchez **MathFuncs**. 
3. Vérifier le **Version** du package est **1.0.0** et les autres détails s’affichent comme prévu telles que la **titre** et **Description**, c'est-à-dire , *MathFuncs* et *exemple de bibliothèque de Wrapper C++*. 
4. Sélectionnez le **MathFuncs** du package, puis cliquez sur **ajouter un Package**.

### <a name="using-the-library-functions"></a>En utilisant les fonctions de bibliothèque

Maintenant, avec une référence à la **MathFuncs** package dans chacun des projets, les fonctions sont disponibles pour le C# code.

1.  Ouvrez **MainPage.xaml.cs** depuis le **MathFuncsApp** courantes **Xamarin.Forms**projet (référencé par les deux **MathFuncsApp.Android**et **MathFuncsApp.iOS**).
2.  Ajouter **à l’aide de** instructions pour **System.Diagnostics** et **MathFuncs** en haut du fichier :

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. Déclarez une instance de la `MyMathFuncs` classe en haut de la `MainPage` classe :

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. Remplacer le `OnAppearing` et `OnDisappearing` méthodes à partir de la `ContentPage` classe de base :

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
    }
    ```

5. Mise à jour le `OnAppearing` méthode pour initialiser le `myMathFuncs` variable déclarée précédemment :

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. Mise à jour le `OnDisappearing` méthode à appeler le `Dispose` méthode sur `myMathFuncs`:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. Implémenter une méthode privée appelée **TestMathFuncs** comme suit :

    ```csharp
    private void TestMathFuncs()
    {
        var numberA = 1;
        var numberB = 2;

        // Test Add function
        var addResult = myMathFuncs.Add(numberA, numberB);

        // Test Subtract function
        var subtractResult = myMathFuncs.Subtract(numberA, numberB);

        // Test Multiply function
        var multiplyResult = myMathFuncs.Multiply(numberA, numberB);

        // Test Divide function
        var divideResult = myMathFuncs.Divide(numberA, numberB);

        // Output results
        Debug.WriteLine($"{numberA} + {numberB} = {addResult}");
        Debug.WriteLine($"{numberA} - {numberB} = {subtractResult}");
        Debug.WriteLine($"{numberA} * {numberB} = {multiplyResult}");
        Debug.WriteLine($"{numberA} / {numberB} = {divideResult}");
    }
    ```

8. Enfin, appelez `TestMathFuncs` à la fin de la `OnAppearing` méthode :

    ```csharp
    TestMathFuncs();
    ```

9. Exécutez l’application sur chaque plateforme cible et de valider la sortie dans le **sortie de l’Application** panneau s’affiche comme suit :

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Si vous rencontrez un «*DLLNotFoundException*» lorsque tests sur Android, ou une erreur de build sur iOS, veillez à vérifier que l’architecture du processeur du périphérique/émulateur/simulateur vous utilisez est compatible avec le sous-ensemble que nous avons choisi de prise en charge. 

## <a name="summary"></a>Récapitulatif

Cet article a expliqué comment créer une application Xamarin.Forms qui utilise des bibliothèques natives via un wrapper .NET common distribués via un package NuGet. L’exemple fourni dans cette procédure pas à pas est intentionnellement très simple pour illustrer plus facilement l’approche. Une application réelle aura à gérer les complexités telles que la gestion des exceptions, les rappels, le marshaling de types plus complexes et la liaison avec d’autres bibliothèques de dépendance. Un aspect clé est le processus par lequel l’évolution du code C++ est coordonnée et synchronisée avec les wrapper et les applications clientes. Ce processus peut varier selon si une ou les deux de ces problèmes sont la responsabilité d’une équipe unique. Dans les deux cas, l’automatisation est un réel avantage. Voici quelques ressources fournissant des informations supplémentaires sur certains des concepts clés, ainsi que les téléchargements pertinentes. 

### <a name="downloads"></a>Téléchargements

- [Outils de ligne de commande NuGet (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>Exemples

- [Développement mobile de Hyperlapse multiplateforme en C++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++ et Xamarin)](https://blog.xamarin.com/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles exemple Port](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)

### <a name="further-reading"></a>informations supplémentaires

[Articles relatifs au contenu de cette publication](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
