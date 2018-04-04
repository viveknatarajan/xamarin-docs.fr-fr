---
title: Plus intelligente Xamarin Android prennent en charge v4 / v13 les Packages NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 47ae63fbd7062be97be04bfc1f1244ec63a1c5bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>Plus intelligente Xamarin Android prennent en charge v4 / v13 les Packages NuGet

## <a name="about-the-android-support-libraries"></a>Sur les bibliothèques Android prennent en charge

Google a créé la prise en charge des bibliothèques pour que les nouvelles fonctionnalités disponibles pour les versions antérieures d’Android. En règle générale, les bibliothèques de prise en charge figurent un numéro de version dans leur nom, qui est le plus bas niveau d’API Android qu’ils sont compatibles avec (par ex. : prise en charge-v4 est utilisable uniquement sur les API de niveau 4 et versions ultérieures. Plus d’informations dans ce [débordement de pile de discussion](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Deux des bibliothèques de prise en charge : `Support-v4` et `Support-v13` ne peuvent pas être utilisées ensemble dans la même application, autrement dit, ils s’excluent mutuellement. C’est parce que `Support-v13` réellement contient tous les types et l’implémentation de `Support-v4`. Si vous essayez font référence à la fois dans le même projet, vous rencontrerez des erreurs de type en double.

## <a name="problems-with-referencing"></a>Problèmes avec de référence

Étant donné que `Support-v4` est devenu si populaire, un grand nombre de bibliothèques tierces que 3e maintenant en dépendent. Ils auraient pu choisis dépendent v13 de prise en charge à la place, mais il est plus commun de dépendre de _v4_ depuis qui permet de toutes les applications à l’aide de ces bibliothèques tiers 3e de prendre en charge les niveaux d’API jusque 4.

Si une bibliothèque de tiers Xamarin 3e fait référence à la `Xamarin.Android.Support.v4.dll` liaison à `Support-v4`, toute application qui utilise cette bibliothèque doit également référencer `Xamarin.Android.Support.v4.dll`. Cela devient un problème lors de la même application souhaite également utiliser certaines des fonctionnalités à partir de la `Xamarin.Android.Support.v13.dll` liaison à `Support-v13`. Si vous référencez les deux liaisons, vous rencontrerez des erreurs de type en double.

## <a name="type-forwarded-v4-binding-assembly"></a>Assembly de liaison v4 de type transféré

Pour contourner ce problème, nous avons créé une spéciale `Xamarin.Android.Support.v4.dll` assembly qui n’a aucune implémentation, mais simplement `[assembly: TypeForwardedTo (..)]` attributs de transférer toutes les `Support-v4` types à l’implémentation dans le `Xamarin.Android.Support.v13.dll` assembly.

Cela signifie que cela peut faire référence à un développeur _transférés_ assembly dans leur application qui satisfait la référence à `Xamarin.Android.Support.v4.dll` par toutes les bibliothèques 3e partie, tout en autorisant toujours `Xamarin.Android.Support.v13.dll` à utiliser dans l’application.

## <a name="nuget-assistance"></a>Assistance de NuGet

Alors que le développeur peut ajouter manuellement les références correctes nécessaires, nous sommes en mesure d’utiliser NuGet pour aider à choisir le bon assembly (soit la normale _v4_ liaison ou le type transmis _v4_ assembly) lors de la le package NuGet est installé.

Par conséquent, le `Xamarin.Android.Support.v4` package NuGet contient désormais la logique suivante :

Si votre application cible 13 de niveau API (personnage en pain 3.2) ou une version ultérieure :

*   `Xamarin.Android.Support.v13` NuGet est automatiquement ajoutée en tant que dépendance
*   Le _transférés_ `Xamarin.Android.Support.v4.dll` sera référencé dans le projet

Si votre application cible quoi que ce soit inférieure à 13 de niveau API, vous obtiendrez la normale `Xamarin.Android.Support.v4.dll` liaison référencé dans votre projet.

## <a name="do-i-have-to-use-support-v13"></a>Dois-je utiliser v13 de prise en charge ?

Si votre application cible niveau 13 ou une version ultérieure de l’API et si vous choisissez d’utiliser le `Xamarin Android Support-v4` package NuGet, puis le `Xamarin Android Support v13` package NuGet est une dépendance requise.

Nous estimons que l’augmentation est mineure de taille de l’application (les deux fichiers .jar diffèrent par 17 Ko) est en vaut la peine la compatibilité et les problèmes moins qu'il en résulte.

Si vous êtes catégorique sur l’utilisation de `Support-v4` dans une application qui cible des API niveau 13 ou une version ultérieure, vous pouvez télécharger toujours manuellement le `.nupkg`, extrayez-le et référencer l’assembly.
