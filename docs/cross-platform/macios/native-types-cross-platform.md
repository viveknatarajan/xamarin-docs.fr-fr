---
title: Utilisation des Types natifs dans des applications multiplateformes
description: Cet article couvre l’utilisation de la nouvelle iOS types unifiée l’API Native (nint, nuint, nfloat) dans une application multiplateforme où le code est partagé avec des périphériques non-iOS Android ou des systèmes d’exploitation Windows Phone.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: eb6979691eeef6dd7436d74fdfd501c747d9b3c6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Utilisation des Types natifs dans des applications multiplateformes

_Cet article couvre l’utilisation de la nouvelle iOS types unifiée l’API Native (nint, nuint, nfloat) dans une application multiplateforme où le code est partagé avec des périphériques non-iOS Android ou des systèmes d’exploitation Windows Phone._


Les types natifs 64-types travailler avec iOS et Mac API. Si vous écrivez du code partagé qui s’exécute également sur Android ou Windows, vous devez gérer la conversion de types d’unifiée en normal des types .NET que vous pouvez partager.

Ce document décrit les différentes façons d’interagir avec l’API unifiée à partir de votre code commun partagé.

## <a name="when-to-use-the-native-types"></a>Quand utiliser les Types natifs

Xamarin.iOS et API unifiée de Xamarin.Mac incluent toujours le `int`, `uint` et `float` des types de données, ainsi que le `RectangleF`, `SizeF` et `PointF` types. Ces types de données existants doivent continuer à être utilisés dans n’importe quel code partagé, inter-plateformes. Les nouveaux types de données en mode natif doivent uniquement être utilisées durant un appel à une API Mac ou iOS où prennent en charge pour les types prenant en charge architecture sont requis.

Selon la nature du code partagé, il est possible que certaines où code multiplateforme peut-être avoir besoin pour traiter les `nint`, `nuint` et `nfloat` des types de données. Par exemple : une bibliothèque qui gère les transformations des données rectangulaires qui utilisant précédemment `System.Drawing.RectangleF` pour partager les fonctionnalités entre les versions d’une application, Xamarin.iOS et Xamarin.Android devra être mis à jour pour gérer les Types natifs sur iOS.

Modalités de ces modifications dépend de la taille et la complexité de l’application et la forme de code qui partage a été utilisée, comme nous le verrons dans les sections suivantes.

## <a name="code-sharing-considerations"></a>Considérations sur le partage de code

Comme indiqué dans le [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) de document, il existe deux façons de partager du code entre les projets interplateforme : les projets partagés et les bibliothèques de classes portables. Parmi les deux types a été utilisé, limite les options disponibles lors de la gestion des types de données natifs dans code multiplateforme.

### <a name="portable-class-library-projects"></a>Projets de bibliothèque de classes portable

Une bibliothèque de classes Portable (PCL) vous permet de cibler les plateformes que vous souhaitez prendre en charge et utilisez les Interfaces pour fournir des fonctionnalités spécifiques à la plateforme.

Étant donné que le type de projet de bibliothèque PCL est compilé pour une `.DLL` et il n’a n’a aucun sens de l’API unifiée, vous allez devoir continuer à utiliser les types de données existante (`int`, `uint`, `float`) dans la bibliothèque PCL code source et le type converti les appels aux PCLs les classes et méthodes dans les applications front-end. Par exemple :

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Projets partagés

Le type de projet d’actifs partagés permet d’organiser votre code source dans un projet distinct puis obtient inclus et compilé dans les applications individuelles frontal spécifique à la plateforme, et utiliser `#if` comme requis pour gérer les directives de compilateur exigences de plateforme spécifique.

La taille et la complexité de la face avant arrêter des applications mobiles qui consomment de code partagé, ainsi que la taille et la complexité du code partagé, doit être pris en compte lorsque les types de choisir la méthode de prise en charge des données natives inter-plateformes avec le Type de projet d’actifs partagé.

En fonction de ces facteurs, les types suivants de solutions peuvent être implémentés à l’aide de la `if __UNIFIED__ ... #endif` directives de compilateur pour gérer les changements spécifiques d’API unifiée pour le code.

#### <a name="using-duplicate-methods"></a>À l’aide de méthodes en double

Prenons l’exemple d’une bibliothèque qui effectue les transformations de données rectangulaires ci-dessus. Si la bibliothèque ne contient qu’une ou deux méthodes très simples, vous pouvez choisir de créer des versions en double de ces méthodes pour Xamarin.iOS et Xamarin.Android. Par exemple :

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

Dans le code ci-dessus, dans la mesure où le `CalculateArea` routine est très simple, nous avons utilisé la compilation conditionnelle et créé un distinct, la version d’API unifiée de la méthode. En revanche, si la bibliothèque de contenu de routines ou plusieurs routines complexes, cette solution ne serait pas possible, car il présente un problème de synchronisation toutes les méthodes pour les modifications ou des correctifs de bogues.

#### <a name="using-method-overloads"></a>À l’aide de la méthode de surcharge

Dans ce cas, la solution peut consister à créer une version de surcharge des méthodes à l’aide des types de données 32 bits afin qu’ils prennent désormais `CGRect` en tant que paramètre ou valeur de retour, convertir cette valeur en un `RectangleF` (sachant que conversion à partir de `nfloat` à `float` est une conversion avec perte de données) et appeler la version d’origine de la routine pour exécuter le travail réel. Par exemple :

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

            // Call original routine to calculate area
            return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Là encore, il s’agit d’une bonne solution tant que la perte de précision n’affecte pas les résultats pour les besoins spécifiques de votre application.

#### <a name="using-alias-directives"></a>À l’aide de Directives d’Alias

Pour les zones où la perte de précision est un problème, une autre solution consiste à utiliser `using` directives pour créer un alias pour les types de données natif et CoreGraphics en incluant le code suivant en haut de fichiers de code source partagé et de conversion un nécessaire `int`, `uint` ou `float` valeurs `nint`, `nuint` ou `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Pour que notre exemple de code devient alors :

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Notez qu’ici, nous avons modifié le `CalculateArea` retour de la méthode un `nfloat` au lieu de la norme `float`. Cela a été fait afin que nous n’obtiendriez pas une erreur de compilation lors de _implicitement_ convertir le `nfloat` résultat de notre calcul (étant donné que les deux valeurs multipliés sont `nfloat`) dans un `float` valeur de retour.

Si le code est compilé et exécuté sur un appareil non API unifiée, les `using nfloat = global::System.Single;` mappe le `nfloat` à un `Single` qui convertit implicitement en un `float` permettant à l’application consommatrice frontale appeler le `CalculateArea` méthode sans modification.


#### <a name="using-type-conversions-in-the-front-end-app"></a>À l’aide des Conversions de types dans l’application frontale

Dans le cas où vos applications front-end apporter uniquement quelques exemples d’appels de votre bibliothèque de code partagé, une autre solution peut consister à laisser la bibliothèque inchangée et cast de type dans l’application Xamarin.iOS ou Xamarin.Mac lors de l’appel de la routine existante. Par exemple :

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Si l’application consommatrice apporte des centaines d’appels à la bibliothèque de code partagé, ce nouveau, peut-être pas une bonne solution.

Selon l’architecture de notre application, nous pouvons finir à l’aide d’une ou plusieurs des solutions ci-dessus pour prendre en charge des données en mode natif des Types (le cas échéant) dans notre code multiplateforme.


## <a name="xamarinforms-applications"></a>Applications de Xamarin.Forms

Les éléments suivants sont requis pour utiliser Xamarin.Forms pour les interfaces utilisateur inter-plateformes qui est également partagés avec une application API unifiée :

- La solution complète doit être à l’aide de la version 1.3.1 (ou supérieure) du NuGet Package Xamarin.Forms.
- Pour n’importe quel restitue personnalisé Xamarin.iOS, utilisez les mêmes types de solutions présentées ci-dessus en fonction de la façon dont le code de l’interface utilisateur a été partagé (projet partagé ou PCL).

Comme dans une application multiplateforme standard, 32 bits existante données types doivent être utilisés dans du code partagé, inter-plateformes pour la plupart des toutes les situations. Les nouveaux Types de données natif doit uniquement être utilisées durant un appel à une API Mac ou iOS où prennent en charge pour les types prenant en charge architecture sont requis.

Pour plus d’informations, consultez notre [mise à jour les applications existantes Xamarin.Forms](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) documentation.

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons voir lorsque nous devons utiliser les Types de données natifs dans une application API unifiée et leurs implications inter-plateformes. Nous avons présenté plusieurs solutions qui peuvent être utilisées dans les situations où les nouveaux Types de données natif doit être utilisés dans des bibliothèques multiplateformes. Et nous l’avons vu un guide rapide unifiée des API de prise en charge dans les applications inter-plateformes Xamarin.Forms.



## <a name="related-links"></a>Liens associés

- [API unifiée](~/cross-platform/macios/unified/index.md)
- [Types natifs](~/cross-platform/macios/nativetypes.md)
- [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md)
- [Exemple de partage de code](https://developer.xamarin.com/samples/mobile/SharingCode/)
