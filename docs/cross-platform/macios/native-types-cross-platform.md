---
title: Utilisation de types natifs dans des applications multiplateformes
description: Cet article traite de l’utilisation de la nouvelle iOS types unifiée l’API Native (nint, nuint, nfloat) dans une application multiplateforme où le code est partagé avec des appareils non-iOS comme Android ou les systèmes d’exploitation de Windows Phone.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 847566feec2069dea924bcd2a18abf2b3ddb250b
ms.sourcegitcommit: b986460787677cf8c2fc7cc8c03f4bc60c592120
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66213288"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Utilisation de types natifs dans des applications multiplateformes

_Cet article traite de l’utilisation de la nouvelle iOS types unifiée l’API Native (nint, nuint, nfloat) dans une application multiplateforme où le code est partagé avec des appareils non-iOS comme Android ou les systèmes d’exploitation de Windows Phone._


Les types natifs travailler avec iOS et Mac API 64-types. Si vous écrivez du code partagé qui s’exécute également sur Android ou Windows, vous devez gérer la conversion de types d’unifiée en types .NET standards que vous pouvez partager.

Ce document aborde les diverses manières d’interagir avec l’API unifiée à partir de votre code commun partagé.

## <a name="when-to-use-the-native-types"></a>Quand utiliser les Types natifs

Xamarin.iOS et Xamarin.Mac Unified API incluent toujours le `int`, `uint` et `float` des types de données, ainsi que le `RectangleF`, `SizeF` et `PointF` types. Ces types de données existants doivent continuer à être utilisée dans n’importe quel code partagé, inter-plateformes. Les nouveaux types de données en mode natif doivent uniquement servir lors d’un appel à une API Mac ou iOS à où prendre en charge pour les types prenant en charge les architecture sont requis.

Selon la nature du code partagé, il peut arriver où inter-plateformes code devra peut-être traiter le `nint`, `nuint` et `nfloat` des types de données. Par exemple : une bibliothèque qui gère des transformations sur les données rectangulaires qui utilisant précédemment `System.Drawing.RectangleF` pour partager les fonctionnalités entre les versions de Xamarin.iOS et Xamarin.Android d’une application, devra être mis à jour pour gérer des Types natifs sur iOS.

Modalités de ces modifications dépend de la taille et la complexité de l’application et la forme de code de partage qui a été utilisée, comme nous le verrons dans les sections suivantes.

## <a name="code-sharing-considerations"></a>Considérations sur le partage de code

Comme indiqué dans le [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) documenter, il existe deux façons de partager du code entre des projets multiplateformes : Partagé des projets et des bibliothèques de classes portables. Parmi les deux types a été utilisé, limite les options disponibles lors de la gestion des types de données natifs dans le code multiplateforme.

### <a name="portable-class-library-projects"></a>Projets de bibliothèque de classes portable

Une bibliothèque de classes Portable (PCL) vous permet de cibler les plateformes que vous souhaitez prendre en charge et utiliser des interfaces pour fournir des fonctionnalités spécifiques à la plateforme.

Étant donné que le type de projet de bibliothèque PCL est compilé vers le bas dans un `.DLL` et ne qu’aucune idée de l’API unifiée, vous allez être obligé de continuer à utiliser les types de données existante (`int`, `uint`, `float`) dans la bibliothèque PCL code source et type convertissent les appels à la bibliothèque de classes portable classes et méthodes dans les applications frontales. Exemple :

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Projets partagés

Le type de projet de ressource partagé vous permet d’organiser votre code source dans un projet distinct puis obtient inclus et compilé dans les applications individuels spécifiques à la plateforme front-end, utilisez `#if` comme requis pour gérer les directives de compilateur configuration requise spécifique à la plateforme.

La taille et la complexité des applications mobiles front-end qui consomment le code partagé, ainsi que la taille et la complexité du code partagé, il faut prendre en considération lors du choix de la méthode de prise en charge pour les types de données natifs dans une inter-plateformes Projet de ressources partagées.

En fonction de ces facteurs, les types suivants de solutions peuvent être implémentés à l’aide de la `if __UNIFIED__ ... #endif` directives de compilateur pour gérer les changements spécifiques API unifiée pour le code.

#### <a name="using-duplicate-methods"></a>À l’aide de méthodes en double

Prenons l’exemple d’une bibliothèque qui effectue les transformations de données rectangulaires ci-dessus. Si la bibliothèque ne contient qu’une ou deux méthodes très simples, vous pouvez choisir de créer des versions en double de ces méthodes pour Xamarin.iOS et Xamarin.Android. Exemple :

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

Dans le code ci-dessus, dans la mesure où le `CalculateArea` routine est très simple, nous avons utilisé la compilation conditionnelle et créé un distinct, la version d’API unifiée de la méthode. En revanche, si la bibliothèque de contenu de routines ou plusieurs routines complexes, cette solution ne serait pas possible, car elle les présente un problème de synchronisation toutes les méthodes pour les modifications du ou des correctifs de bogues.

#### <a name="using-method-overloads"></a>À l’aide de la méthode de surcharge

Dans ce cas, il peut être la solution pour créer une version de surcharge des méthodes à l’aide des types de données 32 bits afin qu’ils prennent désormais `CGRect` comme un paramètre ou une valeur de retour, convertir cette valeur à un `RectangleF` (connaître cette conversion à partir de `nfloat` à `float` est une conversion avec perte de données) et appeler la version d’origine de la routine pour effectuer le travail réel. Exemple :

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

Là encore, il s’agit d’une bonne solution, que la perte de précision n’affecte pas les résultats pour les besoins spécifiques de votre application.

#### <a name="using-alias-directives"></a>À l’aide de Directives d’Alias

Pour les zones où la perte de précision est un problème, une autre solution consiste à utiliser `using` directives pour créer un alias pour les types de données natif et CoreGraphics en incluant le code suivant au début des fichiers de code source partagé et de conversion un nécessaire `int`, `uint` ou `float` valeurs `nint`, `nuint` ou `nfloat`:

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
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
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

Notez qu’ici nous avons modifié la `CalculateArea` méthode pour retourner un `nfloat` au lieu de la norme `float`. Ceci, afin que nous obtiendrions pas une erreur de compilation lors de _implicitement_ convertir le `nfloat` résultat de notre calcul (étant donné que les deux valeurs multipliés sont de type `nfloat`) dans un `float` valeur de retour.

Si le code est compilé et exécuté sur un appareil non API unifiée, le `using nfloat = global::System.Single;` mappe le `nfloat` à un `Single` qui convertit implicitement en un `float` permettant à l’application consommatrice frontal appeler le `CalculateArea` méthode sans modification.


#### <a name="using-type-conversions-in-the-front-end-app"></a>À l’aide de Conversions de types dans l’application Front-End

Dans le cas où vos applications front-end rendre uniquement un certain nombre d’appels à votre bibliothèque de code partagé, une autre solution peut consister à laisser la bibliothèque inchangée et cast de type dans l’application Xamarin.iOS ou Xamarin.Mac lors de l’appel de la routine existante. Exemple :

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Si l’application consommatrice offre des centaines d’appels à la bibliothèque de code partagé, cela là encore, peut être une bonne solution.

Selon notre l’architecture d’application, nous pourrions se retrouvent à l’aide d’une ou plusieurs des solutions ci-dessus pour prendre en charge des données en mode natif les Types (si nécessaire) dans notre code inter-plateformes.


## <a name="xamarinforms-applications"></a>Applications Xamarin.Forms

Les éléments suivants sont nécessaire pour utiliser Xamarin.Forms pour les interfaces utilisateur inter-plateformes qui seront également être partagées avec une application API unifiée :

- La solution entière doit être à l’aide de la version 1.3.1 (ou version ultérieure) du NuGet Package Xamarin.Forms.
- Pour n’importe quel rendus personnalisés Xamarin.iOS, utilisez les mêmes types de solutions présentées ci-dessus selon la façon dont le code de l’interface utilisateur a été partagé (projet partagé ou PCL).

Comme dans une application multiplateforme standard, les types de données 32 bits existants utiliser dans n’importe quel code partagé, inter-plateformes pour la plupart des situations. Les nouveaux Types de données natif doit uniquement servir lors de l’appel d’une API Mac ou iOS où prendre en charge pour les types d’architecture prenant en charge est nécessaire.

Pour plus d’informations, consultez notre [la mise à jour des applications Xamarin.Forms existantes](https://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) documentation.

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons vu quand utiliser les Types de données natifs dans une application API unifiée et leurs implications inter-plateformes. Nous avons présenté plusieurs solutions qui peuvent être utilisées dans les situations où les nouveaux Types de données natif doit être utilisés dans des bibliothèques multiplateformes. En outre, nous avons vu un guide rapide pour la prise en charge d’API unifiée dans Xamarin.Forms des applications multiplateformes.



## <a name="related-links"></a>Liens associés

- [API unifiée](~/cross-platform/macios/unified/index.md)
- [Types natifs](~/cross-platform/macios/nativetypes.md)
- [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md)
- [Exemple de partage de code](https://developer.xamarin.com/samples/mobile/SharingCode/)
