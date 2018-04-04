---
title: Garbage Collection
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 49bb340edcad0c5ce39a2d9db6da72d488a114b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="garbage-collection"></a>Garbage Collection

Xamarin.Android utilise de Mono [Simple générations garbage collector](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/). Il s’agit d’un RÉCUPÉRATEUR de marque et balayage avec deux générations et un *espace d’objet volumineux*, avec deux types de collections : 

-   Collections mineures (segment de mémoire de collecte de génération 0) 
-   Collections principales (collecte de génération 1 et les objets volumineux espacement segments). 

> [!NOTE]
> En l’absence d’une collection explicite via [GC. Collect()](xref:System.GC.Collect) sont des collections *à la demande*, en fonction des allocations de tas. *Cela n’est pas un système de comptage de références*; objets *ne seront pas recueillies dès qu’il n’existe aucune référence en suspens*, ou lorsqu’une étendue s’est arrêté. Le garbage collector s’exécute lorsque le tas secondaire a suffisamment de mémoire pour les nouvelles allocations. S’il n’y a pas d’allocations, il ne fonctionnera pas.


Collections secondaires sont bon marché et fréquentes et sont utilisées pour collecter les objets morts et récemment alloués. Collections secondaires sont exécutées après chaque Mo certains des objets alloués. Collections secondaires peuvent être effectuées manuellement en appelant [GC. Collecter (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Principaux regroupements sont coûteux et moins fréquentes et sont utilisées pour récupérer tous les objets morts. Principaux regroupements sont effectuées une fois que la mémoire est épuisée pour la taille de segment de mémoire actuelle (avant le redimensionnement du tas). Collections principales peuvent être effectuées manuellement en appelant [GC. Collecter ()](xref:System.GC.Collect) ou en appelant [GC. Collecter (int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) avec l’argument [GC. MaxGeneration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>Collections d’objets croisée-VM

Il existe trois catégories de types d’objets.

-   **Objets managés**: les types qui *pas* hériter [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , par exemple [System.String](xref:System.String). 
    Ceux-ci sont collectés normalement par le garbage collector. 

-   **Objets Java**: les types Java qui sont présents dans le runtime Android machine virtuelle, mais n’apparaissant pas à la machine virtuelle Mono. Ils sont ennuyeux et ne sont pas abordés plus en détail. Ceux-ci sont collectés normalement par le runtime Android machine virtuelle. 

-   **Objets de l’homologue**: les types qui implémentent [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , par exemple, tous les [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) et [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) sous-classes. Instances de ces types possèdent deux « halfs » un *homologue managé* et un *homologue natif*. L’homologue managé est une instance de la classe c#. L’homologue natif est une instance de la classe Java dans le runtime Android machine virtuelle et C# [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) propriété contient une référence globale JNI à l’homologue natif. 


Il existe deux types d’homologues natifs :

-   **Les homologues de Framework** : les types Java de « Normal » qui ne savent rien de Xamarin.Android, par exemple, [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/).

-   **Utilisateur homologues** : [Android Callable Wrappers](~/android/platform/java-integration/working-with-jni.md) qui sont généré au moment de la génération de chaque sous-classe Java.Lang.Object présente dans l’application.


Qu’il existe deux machines virtuelles dans un processus de Xamarin.Android, il existe deux types de garbage collection :

-   Collections de runtime Android 
-   Collections mono 

Collections de runtime Android fonctionnent normalement, mais avec un inconvénient : une référence globale JNI est traitée comme une racine GC. Par conséquent, s’il existe un JNI global référence maintient un runtime Android objet ordinateur virtuel, l’objet *ne peut pas* collectés, même s’il est éligible pour la collection.

Collections mono sont où fun se produit. Les objets managés sont collectés normalement. Objets homologues sont collectées en effectuant le processus suivant :

1.  Tous les objets homologues éligibles pour la collection Mono ont leur référence globale JNI remplacée par une référence de global faible JNI. 

2.  Un runtime Android GC de machine virtuelle est appelé. N’importe quelle instance homologue natif peut-être être collectée. 

3.  Les références faibles globales JNI que créés dans (1) sont vérifiées. Si la référence faible a été collectée, l’objet d’homologue est collecté. Si la référence faible a *pas* été collectés, puis la référence faible est remplacée par une référence globale JNI et l’objet de l’homologue n’est pas collecté. Remarque : sur les API 14+, cela signifie que la valeur retournée par `IJavaObject.Handle` peut changer après un catalogue global. 

Le résultat final est qu’une instance d’un objet homologue se trouvera tant qu’il est référencé par une de code managé (par exemple, stockée dans un `static` variable) ou référencée par le code Java. En outre, la durée de vie des homologues natifs sera étendue au-delà de ce qu’ils avaient sinon live, comme l’homologue natif ne pouvant être collecté jusqu'à ce que l’homologue natif et l’homologue managés pouvant être collectés.


## <a name="object-cycles"></a>Cycles de l’objet

Objets homologues sont logiquement présentes dans le runtime Android et la machine virtuelle Mono. Par exemple, un [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) instance homologue managé aura correspondante [android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) instance Java de l’homologue framework. Tous les objets qui héritent de [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) peut s’attendre à ont des représentations dans les deux machines virtuelles. 

Tous les objets qui ont la représentation dans les deux ordinateurs virtuels aura des durées de vie qui sont étendues par rapport aux objets qui sont présents uniquement dans une seule machine virtuelle (comme un [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). Appel de [GC. Collecter les](xref:System.GC.Collect) ne sont pas nécessairement collecter de ces objets, que le garbage collector Xamarin.Android a besoin pour s’assurer que l’objet n’est pas référencée par une machine virtuelle avant sa collecte. 

Pour réduire la durée de vie, [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) doit être appelé. Cela manuellement « annulle « la connexion sur l’objet entre les deux machines virtuelles en libérant de la référence globale, ce qui permet les objets soient collectées plus rapidement. 


## <a name="automatic-collections"></a>Collections automatique

À partir de [version 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), Xamarin.Android effectue automatiquement un GC complet lorsqu’un seuil gref est atteint. Ce seuil est de 90 % de la grefs connus maximale pour la plateforme : grefs 1800 sur l’émulateur (2000 max) et 46800 grefs sur du matériel (52000 maximale). *Remarque :* Xamarin.Android ne compte que le grefs créé par [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)et ne connaît pas de n’importe quel autre grefs créé dans le processus. Il s’agit d’une méthode heuristique *uniquement*. 

Lorsqu’une collection automatique est effectuée, un message semblable au suivant s’affichera dans le journal de débogage :

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

L’occurrence de ce n’est pas déterministe et peut se produire à des moments inopportuns (par exemple, au milieu d’un rendu du graphique). Si vous voyez ce message, vous pouvez souhaiter effectuer une collection explicite ailleurs ou souhaité essayer de [réduire la durée de vie des objets homologues](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>Options de pont GC

Xamarin.Android offre une gestion de mémoire transparent avec Android et le runtime Android. Il est implémenté en tant qu’extension au garbage collector Mono appelé le *GC pont*. 

Le pont GC fonctionne pendant un Mono garbage collection et chiffres out quels homologues objets doit leur « activité » vérifiée avec le tas du runtime Android. Le pont GC à cette conclusion en effectuant les étapes suivantes (dans l’ordre) :

1.  Induire le graphe de référence mono d’objets homologues inaccessible dans les objets Java qu’ils représentent. 

2.  Effectuer un GC de Java.

3.  Vérifiez quels objets sont vraiment morts. 

Ce processus complexe est ce qui permet aux sous-classes de `Java.Lang.Object` référence librement des objets ; il supprime toutes les restrictions sur le Java objets peuvent être liés à c#. En raison de cette complexité, le processus de pont peut être très coûteux et cela peut provoquer des pauses notables dans une application. Si l’application rencontre des pauses significatifs, il est conseillé de rechercher une des trois implémentations GC pont suivantes : 

-   **Tarjan** -une nouvelle conception du pont GC selon [algorithme de Robert Tarjan et vers l’arrière de référence propagation](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    Il offre les meilleures performances sur notre simulés charges de travail, mais il possède également la plus grande part des code expérimentale. 

-   **Nouvelle** -révision majeure du code d’origine, résolution des deux instances de comportement quadratique mais en conservant l’algorithme principal (en fonction de [algorithme de Kosaraju](http://en.wikipedia.org/wiki/Kosaraju's_algorithm) de recherche fortement connectée composants). 

-   **Ancien** -l’implémentation d’origine (considérée comme la plus stable des trois). Il s’agit d’une application doit utiliser si le pont le `GC_BRIDGE` les pauses sont acceptables. 


La seule façon de mieux identifier qui fonctionne dans le catalogue global pont est en expérimentant des modèles dans une application et en analysant la sortie. Il existe deux façons de collecter les données pour des tests de performances : 

-   **Activer la journalisation** -activer la journalisation (comme décrit dans la [Configuration](~/android/internals/garbage-collection.md) section) pour chaque option de pont de catalogue global, puis capturer et comparer les sorties de journal à partir de chaque paramètre. Inspecter le `GC` messages pour chaque option, en particulier la `GC_BRIDGE` messages. S’arrête jusqu'à 150ms pour les applications non interactives sont inacceptables, mais les pauses ci-dessus 60 MS pour les applications très interactives (par exemple, les jeux) sont un problème. 

-   **Activer la gestion de pont** -gestion des comptes de pont affiche le coût moyen des objets pointés par chaque objet impliqué dans le processus de pont. Ces informations de tri par taille fournit des conseils sur ce qui retarde la plus grande quantité d’objets supplémentaires. 


Pour spécifier les `GC_BRIDGE` option une application doit utiliser, passer `bridge-implementation=old`, `bridge-implementation=new` ou `bridge-implementation=tarjan` à la `MONO_GC_PARAMS` variable d’environnement, par exemple : 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Le paramètre par défaut est **Tarjan**. Si vous trouvez une régression, il peut s’avérer nécessaire de définir cette option **ancien**. En outre, vous pouvez choisir d’utiliser la plus stable **ancien** option si **Tarjan** ne produit pas d’améliorer les performances. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>Aider le catalogue global

Il existe plusieurs façons pour aider au catalogue global pour réduire les temps d’utilisation et de la collection de mémoire.



### <a name="disposing-of-peer-instances"></a>Suppression d’instances de l’homologue

Le garbage collector a une vue complète des processus et peut ne pas exécuter lorsque la mémoire est faible, car le garbage collector ne sait pas que la mémoire est faible. 

Par exemple, une instance d’un [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) type ou type dérivé est au moins de 20 octets (susceptibles de changer sans préavis, etc., etc..). 
[Des Callable Wrappers managés](~/android/internals/architecture.md) n’ajoutez pas de membres d’instance supplémentaires, par conséquent, lorsque vous avez un [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) instance qui fait référence à un objet blob de 10 Mo de mémoire, GC de Xamarin.Android ne sont pas informés qui &ndash; le catalogue global voient un objet de 20 octets et ne pourra pas déterminer qu’il est lié à Android objets alloués par le runtime qui reste à 10 Mo de mémoire actif. 

Il est souvent nécessaire aider au garbage collector. Malheureusement, *GC. AddMemoryPressure()* et *dans le catalogue global. RemoveMemoryPressure()* ne sont pas pris en charge, par conséquent, si vous *savoir* vous vient de libérer un graphique de grand objet alloué par Java vous devrez appeler manuellement [GC. Collect()](xref:System.GC.Collect) invite un catalogue global pour libérer la Java côté mémoire, ou vous pouvez supprimer de façon explicite *Java.Lang.Object* sous-classes, rompre le mappage entre le wrapper managé et l’instance Java. Par exemple, consultez [bogue 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Vous devez être *extrêmement* prudent lors de la suppression de `Java.Lang.Object` instances de la sous-classe.

Pour réduire le risque d’altération de la mémoire, suivez les instructions suivantes lors de l’appel `Dispose()`.


#### <a name="sharing-between-multiple-threads"></a>Partage entre plusieurs Threads

Si le *Java ou gérés* instance peut être partagée entre plusieurs threads, *ne doit pas être `Dispose()`d*, **jamais**. Par exemple, [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) peut retourner un *instance mise en cache*. Plusieurs threads fournissent les mêmes arguments, ils obtiendront le *même* instance. Par conséquent, `Dispose()`binaire de la `Typeface` instance à partir d’un thread peut invalider des autres threads, ce qui peuvent entraîner `ArgumentException`s à partir de `JNIEnv.CallVoidMethod()` (entre autres), car l’instance a été supprimée à partir d’un autre thread. 


#### <a name="disposing-bound-java-types"></a>Suppression de Types Java liée

Si l’instance est d’un type Java lié, l’instance peut être supprimé de *tant que* l’instance n’est pas réutilisé depuis du code managé *et* l’instance Java ne peut pas être partagé entre les threads (voir la section précédente `Typeface.Create()` discussion). (Cette décision peut être difficile.) La prochaine fois que l’instance Java entre dans code managé, un *nouveau* wrapper sera créé pour elle. 

Cela est souvent utile lorsqu’il s’agit de Drawables et d’autres instances de ressources :

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Ci-dessus est sécurisée, car l’homologue qui [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) retourne font référence à un homologue de Framework, *pas* un homologue de l’utilisateur. Le `Dispose()` appeler à la fin de la `using` bloc rompt la relation entre la [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) et framework [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) instances, ce qui permet de l’instance Java collectées dès que le runtime Android a besoin. Cela serait *pas* être sécurisée si l’instance d’homologue auquel un homologue de l’utilisateur ; ici, nous utilisons les informations « externes » à *savoir* qui le `Drawable` ne peut pas faire référence à un homologue de l’utilisateur et par conséquent la `Dispose()` appeler est sécurisée. 


#### <a name="disposing-other-types"></a>Suppression d’autres Types 

Si l’instance fait référence à un type qui n’est pas une liaison d’un type Java (tels que personnalisé `Activity`), **ne sont pas** appeler `Dispose()` , sauf si vous *savoir* qu’aucun code Java n’appelle les méthodes substituées sur ce instance. Cela entraîne [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

Par exemple, si vous avez personnalisé sur le port d’écoute :

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Vous *ne doivent pas* dispose de cette instance, comme Java essaiera d’appeler des méthodes sur celle-ci à l’avenir :

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>À l’aide de contrôles explicites pour éviter les Exceptions

Si vous avez implémenté un [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) surcharge de méthode, évitez de toucher les objets qui impliquent JNI. Cela peut créer un *dispose de deux* situation qui permet à votre code (mortellement) essayez d’accéder à un objet Java sous-jacent qui a déjà été l’opération de garbage collection. Cela génère une exception semblable à ce qui suit : 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

Souvent cette situation se produit lorsque la premier dispose d’un objet entraîne un membre à devenir null, puis une tentative d’accès suivantes sur ce membre null entraîne la levée d’une exception. Plus précisément, l’objet `Handle` (qui lie une instance gérée à son instance Java sous-jacent) est non valide sur la premier dispose, mais le code managé tente toujours d’accéder à cette instance Java sous-jacente même si elle n’est plus disponible (voir [ Gérés Callable Wrappers](~/android/internals/architecture.md#Managed_Callable_Wrappers) pour plus d’informations sur le mappage entre les instances de Java et des instances managées). 

Une bonne solution pour éviter cette exception consiste à vérifier explicitement dans votre `Dispose` méthode le mappage entre l’instance gérée et l’instance Java sous-jacente est toujours valide ; c'est-à-dire, vérifiez si l’objet `Handle` a la valeur null (`IntPtr.Zero`) avant d’accéder à ses membres. Par exemple, `Dispose` accède à la méthode un `childViews` objet : 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

Si une suppression initiale passez causes `childViews` avoir non valide `Handle`, le `for` accès à la boucle lèvera une `ArgumentException`. En ajoutant un texte explicite `Handle` null vérification avant la première `childViews` accéder à ce qui suit, `Dispose` méthode empêche l’exception se produise : 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>Réduire les Instances référencés

Chaque fois qu’une instance d’un `Java.Lang.Object` type ou une sous-classe est analysé pendant le garbage collector, l’ensemble *graphique d’objet* que l’instance fait référence à doit également être analysé. Le graphique d’objets est l’ensemble des instances d’objets que l’instance « racine » fait référence à, *plus* tous les éléments référencés par l’instance de racine fait référence à, de manière récursive. 

Considérons la classe suivante :

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

Lorsque `BadActivity` est construit, le graphique d’objet contient des 10004 instances (1 x `BadActivity`, 1 x `strings`, 1 x `string[]` détenues par `strings`, 10000 x instances de chaîne), *tous les* de qui devront être analyse chaque fois que le `BadActivity` instance est analysée. 

Cela peut avoir d’impact négatif sur vos heures de collecte, ce qui augmente les temps de pause dans le catalogue global. 

Vous pouvez aider le garbage collector par *réduisant* la taille des graphiques d’objets qui sont enracinés par les instances utilisateur homologue. Dans l’exemple ci-dessus, cela est possible en déplaçant `BadActivity.strings` dans une classe distincte qui n’hérite pas de Java.Lang.Object : 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>Collections secondaires

Collections secondaires peuvent être effectuées manuellement en appelant [GC. Collect(0)](xref:System.GC.Collect). Collections secondaires sont coûteuses (comparé aux collections principale), mais ont un coût fixe, donc vous ne souhaitez pas déclencher les trop souvent et doit avoir un temps de pause de quelques millisecondes. 

Si votre application dispose d’un « cycle d’utilisation » dans laquelle la même chose est effectuée plusieurs fois, il peut être recommandé d’effectuer manuellement une collection secondaire une fois le cycle de service terminée. Exemples de cycles de droit sont les suivantes : 

-  Le cycle de rendu d’une seule image de jeu.
-  Toute l’interaction avec une boîte de dialogue d’application donnée (ouverture, remplissage, fermeture) 
-  Un groupe de demandes de réseau à l’actualisation/synchroniser les données d’application.



## <a name="major-collections"></a>Principales Collections

Collections principales peuvent être effectuées manuellement en appelant [GC. Collect()](xref:System.GC.Collect) ou `GC.Collect(GC.MaxGeneration)`. 

Ils doivent être effectuées rarement et peut-être un temps de pause d’une seconde sur un appareil Android-style lors de la collecte d’un segment de mémoire de 512 Mo. 

Collections majeures doivent uniquement être appelées manuellement, si jamais : 

-   À la fin du droit de long cycles et quand un long suspendre ne présentent un problème à l’utilisateur. 

-   Dans un substituée [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) (méthode). 



## <a name="diagnostics"></a>Diagnostics

Pour suivre lors de la création et la destruction des références globales, vous pouvez définir le [debug.mono.log](~/android/troubleshooting/index.md) propriété système pour contenir [ *gref* ](~/android/troubleshooting/index.md) et/ou [ *gc*](~/android/troubleshooting/index.md). 



## <a name="configuration"></a>Configuration

Le garbage collector de Xamarin.Android peut être configuré en définissant le `MONO_GC_PARAMS` variable d’environnement. Variables d’environnement peuvent être définies avec une action de génération [AndroidEnvironment](~/android/deploy-test/environment.md).

Le `MONO_GC_PARAMS` variable d’environnement est une liste séparée par des virgules des paramètres suivants : 

-   `nursery-size` = *taille* : définit la taille de la reproduction. La taille est exprimée en octets et doit être une puissance de deux. Les suffixes `k` , `m` et `g` peut être utilisé pour spécifier des kilo, méga et gigaoctets, respectivement. La reproduction est la première génération (des deux). Une plus grande reproduction généralement accélère le programme, mais évidemment utilisera plus de mémoire. La reproduction par défaut de taille 512 Ko. 

-   `soft-heap-limit` = *taille* : le nombre maximal de cibles gérées la consommation de mémoire pour l’application. Lors de l’utilisation de la mémoire est inférieur à la valeur spécifiée, le garbage collector est optimisé pour le moment de l’exécution (collections moins). 
    Au-dessus de cette limite, le garbage collector est optimisé pour l’utilisation de mémoire (d’autres collections). 

-   `evacuation-threshold` = *seuil* : définit le seuil d’évacuation en pourcentage. La valeur doit être un entier compris entre 0 et 100. La valeur par défaut est 66. Si la phase de balayage de la collection détecte que le niveau d’occupation d’un type de bloc de segment de mémoire spécifique est inférieure à ce pourcentage, il effectuera une collection de copie pour ce type de bloc dans la collection majeure suivante, restaurant ainsi le niveau d’occupation à proche de 100 pour cent. La valeur 0 désactive évacuation. 

-   `bridge-implementation` = *implémentation de pont* : cela configurera l’option de catalogue global pont pour aider à résoudre dans le catalogue global les problèmes de performances. Il existe trois valeurs possibles : *ancien* , *nouveau* , *tarjan*.

-   `bridge-require-precise-merge`: Le Tarjan pont contient une optimisation, ce qui peut, dans de rares occasions, un objet à être collectées un GC dès qu’il est tout d’abord garbage. Y compris de cette option désactive cette optimisation, ce qui rend les catalogues globaux plus prévisible, mais potentiellement plus lent.

Par exemple, pour configurer le catalogue global avec une limite de taille de segment de mémoire de 128 Mo, ajouter un nouveau fichier à votre projet avec un **action de génération** de `AndroidEnvironment` avec le contenu : 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
