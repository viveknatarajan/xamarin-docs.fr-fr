---
title: Introduction à Renderscript
description: Ce guide présente les Renderscript et explique comment utiliser l’intrinsèque Renderscript API dans une application de Xamarin.Android ce niveau cibles API 17 ou une version ultérieure.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-renderscript"></a>Introduction à Renderscript

_Ce guide présente les Renderscript et explique comment utiliser l’intrinsèque Renderscript API dans une application de Xamarin.Android ce niveau cibles API 17 ou une version ultérieure._

## <a name="overview"></a>Vue d'ensemble

RenderScript est une infrastructure de programmation créée par Google en vue d’améliorer les performances des applications Android qui nécessite d’importantes ressources de calculs. Il s’agit d’un niveau inférieur à hautes performances API basée sur [C99](http://en.wikipedia.org/wiki/C99). S’agissant d’un faible niveau API qui s’exécutera sur les unités centrales, GPU ou DSP, Renderscript est parfaitement adaptée pour les applications Android qui devra peut-être effectuer les opérations suivantes :

* Graphiques
* Traitement d’image
* Chiffrement
* Traitement des signaux
* Routines mathématiques

RenderScript utilisera `clang` et compiler les scripts à un code d’octet LLVM qui est fourni dans le fichier APK. Lorsque l’application est exécutée pour la première fois, le code d’octet LLVM sera compilé en code machine pour les processeurs sur l’appareil. Cette architecture permet à une application Android exploiter les avantages du code machine sans les développeurs eux-mêmes avoir à écrire pour chaque processeur sur l’appareil eux-mêmes.

Il existe deux composants dans une routine Renderscript :

1. **Le runtime Renderscript** &ndash; il s’agit de l’API natives qui sont responsables de l’exécution de la Renderscript. Cela inclut tout Renderscripts écrit pour l’application.

2. **Des Wrappers managés à partir de l’infrastructure Android** &ndash; gérés des classes qui permettent à une application Android contrôler et interagir avec les Renderscript runtime et les scripts. En plus des classes d’infrastructure fournie pour le contrôle de l’exécution de Renderscript, la chaîne d’outils Android examine le code source Renderscript et générer des classes de wrapper managé pour une utilisation par l’application Android.

Le diagramme suivant illustre la façon dont ces composants sont liés :

![Diagramme illustrant comment le Framework Android interagit avec le Renderscript Runtime](renderscript-images/renderscript-01.png)

Il existe trois concepts importants pour l’utilisation de Renderscripts dans une application Android :

1. **Un contexte** &ndash; API fournie par le SDK Android qui alloue des ressources à Renderscript et permet à l’application Android transmettre et recevoir des données à partir de la Renderscript managé.

2. **A _du noyau de calcul_**  &ndash; également connu sous le _noyau de racine_ ou _noyau_, cette une routine qui effectue le travail. Le noyau est très similaire à une fonction C ; Il s’agit d’une routine parallèles qui sera exécutée sur toutes les données dans la mémoire allouée.

3. **La mémoire allouée** &ndash; données sont passées vers et à partir d’un noyau via une  _[Allocation](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Un noyau peut avoir une entrée ou de sortie d’Allocation.

Le [Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) espace de noms contient les classes pour interagir avec le runtime Renderscript. En particulier, la [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) classe gère le cycle de vie et les ressources du moteur Renderscript. L’application Android doit initialiser une ou plusieurs [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) objets. Une Allocation est une API managée qui est responsable de l’allocation et l’accès à la mémoire est partagée entre l’application Android et le runtime Renderscript. En règle générale, une Allocation est créée pour l’entrée, et éventuellement une autre affectation est créée pour stocker la sortie du noyau. Le moteur d’exécution Renderscript et les classes wrapper managées associées seront gérer l’accès à la mémoire détenue par les Allocations, il n’est pas nécessaire pour un développeur d’application Android effectuer des tâches supplémentaires.

Une Allocation contient un ou plusieurs [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Les éléments sont un type spécialisé qui décrivent les données de chaque Allocation.
Les types d’éléments de la sortie de l’Allocation doit correspondre à la les types de l’élément d’entrée. Lors de l’exécution, un Renderscript itérer sur chaque élément dans l’Allocation d’entrée en parallèle et écrire les résultats vers la sortie d’Allocation. Il existe deux types d’éléments :

- **type simple** &ndash; Conceptuellement, cela est identique à un type de données C `float` ou `char`.

- **type complexe** &ndash; ce type est similaire à un C `struct`.

Le moteur Renderscript effectue une vérification de l’exécution pour vous assurer que les éléments dans chaque Allocation sont compatibles avec ce qui est requis par le noyau. Si le type de données des éléments de l’Allocation ne correspondre pas le type de données que le noyau est attendu, une exception est levée.

Tous les Renderscript noyaux seront encapsulés par un type qui est un descendant de la [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) classe. Le `Script` classe est utilisée pour définir les paramètres d’un Renderscript, définissez les `Allocations`, et exécuter le Renderscript. Il existe deux `Script` sous-classes dans le Kit de développement logiciel Android :


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Certaines des tâches de Renderscript plus courantes sont fournies dans le Kit de développement logiciel Android et sont accessibles par une sous-classe de la [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) classe. Il est inutile pour un développeur de prendre d’étapes supplémentaires pour utiliser ces scripts dans leurs applications, telles qu’elles sont déjà fournies.

- **`ScriptC_XXXXX`** &ndash; Également appelé _scripts utilisateur_, il s’agit des scripts qui sont écrites par les développeurs et empaquetées dans APK. Au moment de la compilation, la chaîne d’outils Android génère des classes wrapper managées qui autorise les scripts à utiliser dans l’application Android.
  Le nom de ces classes générées est le nom du fichier Renderscript, avec le préfixe `ScriptC_`. L’écriture et en incorporant des scripts de l’utilisateur ne sont pas officiellement pris en charge par Xamarin.Android dépasse le cadre de ce guide.

Ces deux types, le `StringIntrinsic` est pris en charge par Xamarin.Android. Ce guide explique comment utiliser des scripts intrinsèques dans une application de Xamarin.Android.

## <a name="requirements"></a>Spécifications

Ce guide a trait aux applications de Xamarin.Android ce niveau de l’API cible 17 ou une version ultérieure. L’utilisation de _scripts utilisateur_ n’est pas couverte dans ce guide.

Le [bibliothèque de prise en charge de Xamarin.Android V8](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports les intrinsèques Renderscript de l’API pour les applications qui ciblent des versions antérieures du SDK Android. Ajout de ce package à un projet Xamarin.Android doit autoriser les applications qui ciblent des versions antérieures du SDK Android d’exploiter les scripts intrinsèques.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>À l’aide des intrinsèques Renderscripts dans Xamarin.Android

Les scripts intrinsèques sont un excellent moyen d’effectuer des tâches de calcul intensifs avec une quantité minimale de code supplémentaire. Ils ont été main réglé pour offrir des performances optimales sur une section volumineuse de périphériques.
Il n’est pas rare qu’un script intrinsèque exécuter 10 fois plus rapide que le code managé et 2-3 x heures à une implémentation personnalisée de C. La plupart des scénarios de traitement par défaut sont couverts par les scripts intrinsèques. Des scripts intrinsèques de la liste suivante décrit les scripts en cours de Xamarin.Android :

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; convertit RVB à RGBA à l’aide d’une table de recherche 3D. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh performances Renderscript APIs à [BLAS](http://www.netlib.org/blas/). BLAS (base sous-programmes d’algèbre linéaire) sont des routines qui fournissent des blocs de construction standard pour effectuer des opérations de la matrice et de vecteur de base. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; fusionne deux Allocations ensemble.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; flou gaussienne une Allocation.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; s’applique une matrice de couleurs pour une Allocation (autrement dit, modification de couleurs, adaptez teinte).

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; s’applique une matrice 3 x 3 couleur pour une Allocation.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; s’applique une matrice de couleurs de 5 x 5 pour une Allocation.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; un filtre d’histogramme intrinsèque.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; s’applique une table de recherche par canal vers une mémoire tampon.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; Script pour le redimensionnement d’une allocation 2D.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; convertit une mémoire tampon YUV RVB.

Consultez la documentation API pour plus d’informations sur chacun des scripts intrinsèques.

Les étapes de base pour l’utilisation de Renderscript dans une application Android sont décrites ci-après.

**Créer un contexte Renderscript** &ndash; le [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) classe est un wrapper managé autour du contexte Renderscript et détermine l’initialisation, la gestion des ressources et nettoyer. L’objet Renderscript est créé à l’aide de la `RenderScript.Create` méthode de fabrique, qui prend un contexte Android (par exemple, une activité) en tant que paramètre. La ligne de code suivante montre comment initialiser le contexte Renderscript :

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Créer des Allocations** &ndash; selon le script intrinsèque, il peut être nécessaire de créer un ou deux `Allocation`s. Le [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) classe dispose de plusieurs méthodes de fabrique pour aider l’instanciation d’une allocation pour un type intrinsèque. Par exemple, l’extrait de code suivant montre comment créer d’Allocation pour les images bitmap.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Souvent, il sera nécessaire créer un `Allocation` pour contenir les données de sortie d’un script. Cet extrait de code suivant montre comment utiliser le `Allocation.CreateTyped` application d’assistance pour instancier une seconde `Allocation` que le même type que l’original :

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Instanciez le wrapper de Script** &ndash; chacune des classes wrapper script intrinsèque doit avoir des méthodes d’assistance (généralement appelé `Create`) pour instancier un objet de wrapper pour ce script. L’extrait de code suivant est un exemple de comment instancier un `ScriptIntrinsicBlur` flou d’objet. Le `Element.U8_4` méthode d’assistance créera un élément qui décrit un type de données est de 4 champs de valeurs d’entier non signé 8 bits, pouvant être contenant les données d’une `Bitmap` objet :

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Affecter Allocation(s), définir les paramètres et exécuter le Script** &ndash; le `Script` classe fournit un `ForEach` méthode pour exécuter réellement le Renderscript. Cette méthode effectue une itération sur chaque `Element` dans le `Allocation` contenant les données d’entrée. Dans certains cas, il peut être nécessaire de fournir un `Allocation` qui contient la sortie.
`ForEach` remplace le contenu de la sortie de l’Allocation. Pour poursuivre avec les extraits de code de l’étape précédente, cet exemple montre comment affecter une Allocation d’entrée, définissez un paramètre, et enfin exécutez le script (copier les résultats vers la sortie d’Allocation) :

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Vous pouvez souhaiter extraire les [flou d’une Image avec Renderscript](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/) recette, il s’agit d’un exemple complet montrant comment utiliser un script intrinsèque dans Xamarin.Android.

## <a name="summary"></a>Récapitulatif

Ce guide a introduit Renderscript et comment l’utiliser dans une application Xamarin.Android. Il décrit brièvement Renderscript What ' s et comment il fonctionne dans une application Android. Il décrit certains des principaux composants dans Renderscript et la différence entre _scripts utilisateur_ et _intrinsèques scripts_. Enfin, ce guide décrit les étapes à l’aide d’un script intrinsèque dans une application Xamarin.Android.



## <a name="related-links"></a>Liens associés

- [Espace de noms Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Flou d’une Image avec Renderscript](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Didacticiel : Mise en route avec Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
