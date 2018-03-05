---
title: "Conception de l’API"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1b0b1db6bf73b03eed99c5ede038d07bb3ccf284
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="api-design"></a>Conception de l’API

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Outre le noyau de bibliothèques de classes de Base qui font partie de Mono, Xamarin.Android est fourni avec des liaisons pour les différentes API Android permettre aux développeurs de créer des applications Android natives avec Mono.

Au cœur de Xamarin.Android est un moteur d’interopérabilité que world ponts c# avec le monde Java et permet aux développeurs d’accéder aux API Java à partir de c# ou d’autres langages .NET.

<a name="Design_Principles" />

## <a name="design-principles"></a>Principes de conception

Quelques-uns de nos principes de conception pour la liaison de Xamarin.Android

-  Est conforme à la [instructions de conception de .NET Framework](http://msdn.microsoft.com/en-us/library/ms229042.aspx).

-  Permet aux développeurs de classes Java de sous-classe.

-  Sous-classe doit fonctionner avec les constructions non standard de c#.

-  Dériver une classe existante.

-  Appelez le constructeur de base pour la chaîne.

-  Méthodes de substitution doit être effectuée avec le système de remplacement de #.

-  Tâches courantes de Java facile et les tâches Java durs rend possible.

-  Exposer des propriétés de JavaBean en tant que propriétés c#.

-  Exposent une API fortement typée :

    - Augmenter la sécurité de type.

    - Réduire les erreurs d’exécution.

    - Obtenir intellisense de l’IDE sur les types de retour.

    - Permet la documentation de fenêtre contextuelle IDE.

-  Encourage l’exploration dans l’IDE de l’API :

    - Utiliser des Alternatives de Framework à l’exposition des minimiser les Classlib Java.

    - Exposer délégués c# (les expressions lambda, méthodes anonymes et System.Delegate) au lieu d’interfaces de méthode unique lorsque approprié et applicable.

    - Fournissent un mécanisme pour appeler des bibliothèques Java arbitraires ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


<a name="Assemblies" />

## <a name="assemblies"></a>Assemblys

Xamarin.Android inclut un nombre d’assemblys qui constituent le *MonoMobile profil*. Le [assemblys](~/cross-platform/internals/available-assemblies.md) page comporte des informations supplémentaires.

Les liaisons pour la plateforme Android sont contenus dans le `Mono.Android.dll` assembly. Cet assembly contient l’ensemble de la liaison pour l’API Android beaucoup d’et de la communication avec le runtime Android machine virtuelle.

<a name="Binding_Design" />

## <a name="binding-design"></a>Conception de la liaison

<a name="Collections" />

### <a name="collections"></a>Collections

L’API Android utilise les collections java.util largement pour fournir des listes, des jeux et des cartes. Nous présentons ces éléments à l’aide de la [System.Collections.Generic](https://developer.xamarin.com/api/namespace/System.Collections.Generic/) interfaces dans notre liaison. Les mappages fondamentaux sont :

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) est mappé au type de système [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), classe d’assistance [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) est mappé au type de système [IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx), classe d’assistance [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](http://developer.android.com/reference/java/util/Map.html) est mappé au type de système [IDictionary < TKey, TValue >](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), classe d’assistance [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) est mappé au type de système [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), classe d’assistance [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Nous avons fourni des classes d’assistance afin de faciliter le plus rapidement copyless marshaling de ces types. Lorsque cela est possible, nous recommandons d’utiliser ces fourni des collections au lieu de l’implémentation de l’infrastructure fournie, comme [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/) ou [ `Dictionary<TKey, TValue>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/). Le [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) implémentations utilisent une collection de Java native en interne et par conséquent ne requièrent pas copie vers et à partir d’une collection native lors du passage à un membre de l’API Android.

Vous pouvez passer une implémentation d’interface à une méthode Android d’accepter cette interface, par exemple, passer un `List<int>` à la [ArrayAdapter&lt;int&gt;(contexte, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)constructeur. *Toutefois*, pour toutes les implémentations *sauf* pour les implémentations Android.Runtime, cela implique la *copie* la liste à partir de la machine virtuelle de Mono dans le runtime Android machine virtuelle. Si la liste est ultérieure modifié dans le runtime Android (par exemple, en appelant le [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) méthode), ces modifications *ne seront pas* visibles dans le code managé. Si un `JavaList<int>` ont été utilisés, ces modifications sont visibles.

Rephrased, qui sont des implémentations d’interface collections *pas* un des éléments ci-dessus répertoriés **classe d’assistance**es marshaler uniquement [In] :

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```

<a name="Properties" />

### <a name="properties"></a>Propriétés

Méthodes Java sont transformés en propriétés, lorsque cela est approprié :

-  La paire de méthodes Java `T getFoo()` et `void setFoo(T)` sont transformés en le `Foo` propriété. Exemple : [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  La méthode Java `getFoo()` est transformé en la propriété Foo en lecture seule. Exemple : [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Propriétés à définir uniquement ne sont pas générées.

-  Les propriétés sont *pas* générée si le type de propriété serait un tableau.


<a name="Events_and_Listeners" />

### <a name="events-and-listeners"></a>Les événements et les écouteurs

L’API Android s’appuient sur Java et ses composants suivent le modèle de Java pour raccorder les écouteurs d’événements. Ce modèle a tendance à s’avérer peu pratique car elle nécessite l’utilisateur de créer une classe anonyme et déclarez les méthodes à substituer, par exemple, voici comment les choses seront effectuées dans Android avec Java :

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Le code équivalent en c# à l’aide d’événements serait :

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Notez que les deux des mécanismes ci-dessus sont disponibles avec Xamarin.Android. Vous pouvez implémenter une interface d’écouteur et attacher avec View.SetOnClickListener, ou vous pouvez attacher un délégué créé par l’intermédiaire des paradigmes c# habituels pour l’événement Click.

Lorsque la méthode de rappel d’écouteur a retournent void, nous créons des éléments d’API basées sur un [EventHandler&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/) déléguer. Nous générons un événement comme dans l’exemple ci-dessus pour ces types d’écouteur. Toutefois, si le rappel de l’écouteur retourne un non void et non- **booléenne** valeur, des événements et EventHandlers ne sont pas utilisés. Au lieu de cela, nous générer un délégué spécifique pour la signature du rappel et ajouter des propriétés au lieu d’événements. La raison est à traiter avec l’ordre d’appel de délégué et retourner la gestion. Cette approche reflète ce qui est fait avec l’API Xamarin.iOS.

Les propriétés ou les événements c# uniquement générées automatiquement si la méthode d’inscription aux événements Android :

1. A un `set` de préfixe, par exemple, [ *définir*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. A un `void` type de retour.

1. Accepte qu’un seul paramètre, le type de paramètre est une interface, l’interface possède une seule méthode, et se termine par le nom de l’interface `Listener` , par exemple [View.OnClick *écouteur*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


En outre, si l’écouteur de la méthode d’interface a un type de retour de **booléenne** au lieu de **void**, puis généré *EventArgs* sous-classe contiendra un *Handled* propriété. La valeur de la *Handled* propriété est utilisée comme valeur de retour pour la *écouteur* (méthode) et il est par défaut `true`.

Par exemple, le Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) méthode accepte le [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) interface et le [View.OnKeyListener.onKey (vue, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) méthode a un type de retour boolean. Xamarin.Android génère correspondante [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) événement, qui est un [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
Le *KeyEventArgs* classe à son tour possède un [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) propriété, qui est utilisée comme valeur de retour pour la *View.OnKeyListener.onKey()* (méthode).

Nous avons l’intention d’ajouter des surcharges pour les autres méthodes et les constructeurs pour exposer la connexion basée sur un délégué. Écouteurs de rappels multiples nécessitent également certaines inspection supplémentaire pour déterminer si l’implémentation des rappels individuels est raisonnable, donc nous convertissons ces mesure qu’ils sont identifiés. S’il n’existe aucun événement correspondant, écouteurs doivent être utilisés dans c#, mais mettez celles que vous pensez que peut avoir l’utilisation de délégué à notre attention. Nous avons également certaines conversions des interfaces sans le suffixe « Écouteur » lorsqu’il était clair qu’ils tireront parti d’un autre délégué.

Toutes les interfaces d’écouteurs implémentent le [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interface, en raison des détails d’implémentation de la liaison, les classes de l’écouteur doivent implémenter cette interface. Cela est possible en implémentant l’interface de l’écouteur sur une sous-classe de [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) ou tout autre objet Java, comme une activité Android.

<a name="Runnables" />

### <a name="runnables"></a>Runnables

Java utilise le [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) interface afin de fournir un mécanisme de délégation. Le [java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) classe est un consommateur importants de cette interface. Android a utilisé l’interface dans l’API ainsi.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) et [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) sont des exemples importants.

Le `Runnable` interface contient une seule méthode void, [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Par conséquent prêter à liaison dans c# comme un [System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx) déléguer. Nous vous avons fourni dans la liaison des surcharges qui acceptent un `Action` paramètre pour tous les membres de l’API qui consomment un `Runnable` dans l’API native, par exemple, [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) et [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Nous avons laissé le [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) surcharges en place au lieu de les remplacer, car plusieurs types implémentent l’interface et peuvent par conséquent être passés en tant que runnables directement.

<a name="Inner_Classes" />

### <a name="inner-classes"></a>Classes internes

Java a deux types différents de [classes imbriquées](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statique imbriquées des classes et les classes non statiques.

Les classes imbriquées static Java sont identiques aux types imbriqués c#.

Non statiques classes imbriquées, également appelés *classes internes*, sont très différents. Ils contiennent une référence implicite à une instance de leur type englobant et ne peut pas contenir de membres statiques (parmi d’autres différences en dehors de cette vue d’ensemble).

Lorsqu’il s’agit de liaison et l’utilisation de c#, classes imbriquées statiques sont traités comme des types imbriqués normales. Classes internes, ont pendant ce temps, les deux différences importantes :

1. La référence implicite au type conteneur doit être fournie explicitement en tant que paramètre de constructeur.

1. Lorsque vous héritez d’une classe interne, la classe interne *doit* être imbriqué dans un type qui hérite du type contenant la classe de base interne, et le type dérivé doit fournir un constructeur de même type que le c# contenant le type.


Par exemple, considérez la [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) classe interne. Dans la mesure où il s’agit d’une classe interne, le [WallpaperService.Engine() constructeur](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) prend une référence à un [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) instance (compare et contraste pour Java [WallpaperService.Engine () constructeur)](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) qui ne prend aucun paramètre).

Une dérivation de l’exemple d’une classe interne est CubeWallpaper.CubeEngine :

```csharp
class CubeWallpaper : WallpaperService {
        public override WallpaperService.Engine OnCreateEngine ()
        {
                return new CubeEngine (this);
        }

        class CubeEngine : WallpaperService.Engine {
                public CubeEngine (CubeWallpaper s)
                        : base (s)
                {
                }
        }
}
```

Notez comment `CubeWallpaper.CubeEngine` est imbriquée dans `CubeWallpaper`, `CubeWallpaper` hérite de la classe de conteneur de `WallpaperService.Engine`, et `CubeWallpaper.CubeEngine` a un constructeur qui accepte le type déclarant-- `CubeWallpaper` dans ce cas, comme spécifié ci-dessus.

<a name="Interfaces" />

### <a name="interfaces"></a>Interfaces

Interfaces Java peuvent contenir trois jeux de membres, dont deux entraîner des problèmes à partir de c# :

1. Méthodes

1. Types

1. Champs


Interfaces Java sont traduites en deux types :

1. Interface (facultative) contenant des déclarations de méthode. Cette interface a le même nom que l’interface Java, *sauf* qu’il comporte également une « *I* ' préfixe.

1. Une classe statique (facultative) qui contient tous les champs déclarés dans l’interface Java.

Les types imbriqués sont « déplacées » pour être frères de l’interface englobant à la place des types imbriqués, avec le nom d’interface englobante comme préfixe.

Par exemple, considérez la [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) interface.
Le *Parcelable* interface contient les méthodes, types imbriqués et les constantes. Le *Parcelable* méthodes d’interface sont placés dans le [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) interface.
Le *Parcelable* constantes d’interface sont placés dans le [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) type. Imbriqué [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) et [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) types ne sont actuellement pas lié en raison des limitations de la prise en charge des génériques ; Si elles ont été prises en charge, ils seraient présents sous la forme la *Android.OS.IParcelableClassLoaderCreator* et *Android.OS.IParcelableCreator* interfaces. Par exemple, l’imbriquée [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) interface est lié en tant que le [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) interface.


> [!NOTE]
> **Remarque :** avec Xamarin.Android 1.9, constantes d’interface Java sont <em>dupliqué</em> dans le but de simplifier Java de portage de code. Cela contribue à améliorer le portage de code Java qui s’appuie sur [fournisseur android](http://developer.android.com/reference/android/provider/package-summary.html) constantes de l’interface.

Outre les types ci-dessus, il existe quatre autres modifications :

1. Un type portant le même nom que l’interface Java est généré pour contenir des constantes.

1. Les types qui contient également des constantes de l’interface contiennent toutes les constantes qui proviennent d’interfaces Java implémentées.

1. Toutes les classes qui implémentent une interface Java contenant des constantes d’obtenir un nouveau type InterfaceConsts imbriqué qui contient des constantes à partir de toutes les interfaces implémentées.

1. Le *les constructeurs de la* type est désormais obsolète.


Pour le *android.os.Parcelable* interface, cela signifie qu’il sera désormais une [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) type pour contenir les constantes. Par exemple, le [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) constante va être liée en tant que le [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) constante, à la place de comme le  *ParcelableConsts.ContentsFileDescriptor* constante.

Pour les interfaces contenant des constantes qui implémentent des autres interfaces contenant encore plus de constantes, l’union de toutes les constantes est maintenant générée. Par exemple, le [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) interface implémente le [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) interface. Toutefois, avant de 1.9, le [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) type n’a aucun moyen de l’accès à l’une des constantes déclarées sur [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Par conséquent, l’expression Java *MediaStore.Video.VideoColumns.TITLE* doit être lié à l’expression c# *MediaStore.Video.MediaColumnsConsts.Title* qui est difficile de détecter sans effectuer de lecture grand nombre de documentation Java. Dans 1.9, l’expression c# équivalente sera [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

En outre, envisagez la [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) type, qui implémente le Java *Parcelable* interface. Car il implémente l’interface, toutes les constantes de cette interface sont accessibles « par « le type de groupe, par exemple, *Bundle.CONTENTS_FILE_DESCRIPTOR* est une expression Java parfaitement valide.
Auparavant, pour porter cette expression en c# vous devez examiner toutes les interfaces qui sont implémentées pour voir à partir de quel type le *CONTENTS_FILE_DESCRIPTOR* provient. À compter de Xamarin.Android 1.9, classes implémentant des interfaces Java qui contiennent des constantes ont imbriquée *InterfaceConsts* type, qui contient toutes les constantes de l’interface héritée. Cela permettra de traduction *Bundle.CONTENTS_FILE_DESCRIPTOR* à [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Enfin, les types avec un *les constructeurs de la* tels que *Android.OS.ParcelableConsts* sont désormais obsolètes, autres que les nouveaux InterfaceConsts les types imbriqués. Ils seront supprimés dans Xamarin.Android 3.0.

<a name="Resources" />

## <a name="resources"></a>Ressources

Images, des descriptions de disposition, BLOB et les dictionnaires de chaîne peuvent être inclus dans votre application en tant que [les fichiers de ressources](http://developer.android.com/guide/topics/resources/providing-resources.html).
Diverses API Android sont conçus pour [ne fonctionne pas sur l’ID de ressource](http://developer.android.com/guide/topics/resources/accessing-resources.html) au lieu de traiter les images, binaire ou chaînes d’objets BLOB directement.

Par exemple, une application exemple Android qui contient une disposition d’interface utilisateur ( `main.axml`), une chaîne de la table internationalisation ( `strings.xml`) et des icônes ( `drawable-*/icon.png`) conserve ses ressources dans le répertoire « Ressources » de l’application :

    Resources/
        drawable-hdpi/
            icon.png

        drawable-ldpi/
            icon.png

        drawable-mdpi/
            icon.png

        layout/
            main.axml

        values/
            strings.xml

Natif API Android ne fonctionnent pas directement avec les noms de fichiers, mais au lieu de cela ne fonctionne pas sur l’ID de ressource. Lorsque vous compilez une application Android qui utilise des ressources, le système de génération les ressources pour la distribution du package et générer une classe appelée `Resource` qui contient les jetons pour chacune des ressources incluses. Par exemple, dans la disposition de ressources ci-dessus, voici la classe R exposerait :

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

Vous devez utiliser `Resource.Drawable.icon` référence le `drawable/icon.png` fichier, ou `Resource.Layout.main` référence le `layout/main.xml` fichier, ou `Resource.String.first_string` pour faire référence à la première chaîne dans le fichier de dictionnaire `values/strings.xml`.

<a name="Constants_and_Enumerations" />

## <a name="constants-and-enumerations"></a>Constantes et énumérations

Natif API Android ont de nombreuses méthodes qui acceptent ou retournent int qui doit être mappé à un champ constant pour déterminer ce que signifie le type int. Pour utiliser ces méthodes, l’utilisateur est nécessaire pour consulter la documentation pour voir les constantes sont des valeurs appropriées, ce qui est inférieur à idéal.

Par exemple, considérez [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

Dans ce cas, nous nous efforçons de regroupent les constantes associées dans une énumération .NET et remapper la méthode à suivre à la place de l’énumération.
Ce faisant, nous sommes en mesure d’offrir la sélection IntelliSense des valeurs possibles.

L’exemple ci-dessus devient : [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Notez qu’il s’agit d’un processus très manuel pour déterminer les constantes associées et les API consomme ces constantes. Veuillez classer les bogues pour toute utilisation de constantes dans l’API qui serait mieux exprimée comme une énumération.
