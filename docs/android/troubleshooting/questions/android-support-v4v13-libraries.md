---
title: Xamarin Android plus intelligents prennent en charge v4 / v13 les Packages NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 43627884c2f8bc4d9e5b5faa2c3af08f74487b65
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114639"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>Xamarin Android plus intelligents prennent en charge v4 / v13 les Packages NuGet

## <a name="about-the-android-support-libraries"></a>Sur les bibliothèques de prise en charge Android

Google a créé des bibliothèques de prise en charge pour que les nouvelles fonctionnalités disponibles pour les versions antérieures d’Android. En règle générale, les bibliothèques de prise en charge bénéficient d’un numéro de version dans leur nom, qui est le plus bas niveau d’API Android qu’ils sont compatibles avec (ex : prise en charge-v4 est utilisable uniquement sur les API de niveau 4 et versions ultérieures. Plus d’informations dans ce [discussion Stack Overflow](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Deux des bibliothèques de prise en charge : `Support-v4` et `Support-v13` ne peuvent pas être utilisées ensemble dans la même application, autrement dit, ils s’excluent mutuellement. Il s’agit, car `Support-v13` réellement contient tous les types et l’implémentation de `Support-v4`. Si vous essayez de référence à la fois dans le même projet, vous rencontrerez des erreurs de type en double.

## <a name="problems-with-referencing"></a>Problèmes liés au référencement

Étant donné que `Support-v4` est devenu si populaire, un grand nombre de bibliothèques de tiers 3e maintenant en dépendent. Choisi peut dépendre v13 de prise en charge à la place, mais il est plus courant de dépendent _v4_ dans la mesure où qui donne à toutes les applications à l’aide de ces bibliothèques tierces 3e la possibilité de prendre en charge les niveaux d’API jusqu'à 4.

Si une bibliothèque de tiers Xamarin 3e fait référence à la `Xamarin.Android.Support.v4.dll` liaison à `Support-v4`, n’importe quelle application qui utilise cette bibliothèque doit également référencer `Xamarin.Android.Support.v4.dll`. Cela pose un problème lors de la même application souhaite également utiliser certaines des fonctionnalités à partir de la `Xamarin.Android.Support.v13.dll` liaison à `Support-v13`. Si vous référencez les deux liaisons, vous rencontrerez des erreurs de type en double.

## <a name="type-forwarded-v4-binding-assembly"></a>Assembly de liaison v4 type transféré

Pour contourner ce problème, nous avons créé une spéciale `Xamarin.Android.Support.v4.dll` assembly qui n’a aucune implémentation, mais simplement `[assembly: TypeForwardedTo (..)]` attributs qui transmettent toutes les `Support-v4` types à l’implémentation dans le `Xamarin.Android.Support.v13.dll` assembly.

Cela signifie qu’un développeur peut faire référence à ce _type transféré_ assembly dans leur application qui satisfait la référence à `Xamarin.Android.Support.v4.dll` par toutes les bibliothèques tierces 3e, tout en autorisant toujours `Xamarin.Android.Support.v13.dll` à utiliser dans l’application.

## <a name="nuget-assistance"></a>Assistance de NuGet

Alors que le développeur peut ajouter manuellement les références correctes nécessaires, nous sommes en mesure d’utiliser NuGet pour aider à choisir le bon assembly (soit la normale _v4_ liaison ou le type transféré _v4_ assembly) lors de la le package NuGet est installé.

Par conséquent, le `Xamarin.Android.Support.v4` package NuGet contient désormais la logique suivante :

Si votre application cible API niveau 13 (Gingerbread 3.2) ou une version ultérieure :

*   `Xamarin.Android.Support.v13` NuGet est automatiquement ajouté en tant que dépendance
*   Le _type transféré_ `Xamarin.Android.Support.v4.dll` sera référencé dans le projet

Si votre application cible quoi que ce soit inférieure à 13 de niveau API, vous obtiendrez la normale `Xamarin.Android.Support.v4.dll` liaison référencé dans votre projet.

## <a name="do-i-have-to-use-support-v13"></a>Je dois utiliser v13 de prise en charge ?

Si votre application cible niveau d’API 13 ou une version ultérieure et que vous choisissez d’utiliser le `Xamarin Android Support-v4` package NuGet, puis le `Xamarin Android Support v13` package NuGet est une dépendance requise.

Nous estimons que l’augmentation de taille de l’application (les deux fichiers .jar diffèrent par 17 Ko) mineure vaut la peine de la compatibilité et les problèmes moins qu'il en résulte.

Si vous êtes catégorique sur l’utilisation de `Support-v4` dans une application qui cible le 13 de niveau API ou une version ultérieure, vous pouvez télécharger toujours manuellement le `.nupkg`, extrayez-le et référencer l’assembly.
