---
title: Limitations
ms.topic: article
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 11a7c9dcbe9481209d2eb90ff9d981f7437a1c99
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Limitations

Étant donné que les applications sur Android requièrent la génération de types de proxy Java pendant le processus de génération, il n’est pas possible de générer tout le code lors de l’exécution.

Voici les limitations de Xamarin.Android par rapport au bureau Mono :

<a name="Limited_Dynamic_Language_Support" />

## <a name="limited-dynamic-language-support"></a>Prise en charge limitée de langage dynamique

 [Les wrappers RCW Android](~/android/platform/java-integration/android-callable-wrappers.md) sont nécessaires à tout moment, le runtime Android doit appeler le code managé. Les wrappers RCW Android sont générées au moment de la compilation, en fonction de l’analyse statique du code IL. Le résultat net de cet : vous *ne peut pas* utiliser les langages dynamiques (IronPython, IronRuby, etc.) dans tout scénario où le sous-classement de types Java est requis (y compris le sous-classement indirect), car il n’existe aucun moyen de l’extraction de ces types dynamiques au moment de la compilation pour générer les wrappers Android nécessaires.

<a name="Limited_Java_Generation_Support" />

## <a name="limited-java-generation-support"></a>Prise en charge de la génération Java limité

[Les Wrappers RCW Android](~/android/platform/java-integration/android-callable-wrappers.md) doivent être générés dans l’ordre pour le code Java appeler du code managé. *Par défaut*, les wrappers RCW Android contiendra uniquement les (certains) déclaré de constructeur et les méthodes qui remplacent une méthode virtuelle de Java (autrement dit, il a [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) ou implémenter une (méthode) interface Java interface même a `Attribute`).
  
Avant la version 4.1, aucune des méthodes supplémentaires ne peuvent être déclarés. Avec la version 4.1, [le `Export` et `ExportField` des attributs personnalisés peuvent être utilisés pour déclarer des méthodes de Java et les champs dans le Wrapper Android](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Constructeurs manquants

Constructeurs restent difficile, sauf si [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) est utilisé. L’algorithme permettant de générer des constructeurs de wrapper RCW Android est qu’un constructeur de Java sera émis si :

1. Il existe un mappage de Java pour tous les types de paramètre
2. La classe de base déclare du même constructeur &ndash; cela est nécessaire car le wrapper Android *doit* appeler le constructeur de classe de base correspondante ; les arguments par défaut peuvent être utilisés (tel qu’il n’existe aucun moyen facile de déterminer les valeurs doivent être utilisées dans Java).

Par exemple, considérons la classe suivante :

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Lors de cette recherche parfaitement logique, le wrapper RCW Android résultant *dans les versions Release* ne contient pas un constructeur par défaut. Par conséquent, si vous essayez de démarrer ce service (par exemple, [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), l’opération échoue :

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

La solution de contournement consiste à déclarer un constructeur par défaut, avec orner le `ExportAttribute`et définissez la [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```

<a name="Generic_Csharp_classes" />

### <a name="generic-c-classes"></a>Classes génériques c#

Classes génériques c# ne sont que partiellement prises en charge. Les limitations suivantes :


-   Types génériques ne peuvent pas utiliser `[Export]` ou `[ExportField`]. Toute tentative génère une `XA4207` erreur.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Les méthodes génériques ne peuvent pas utiliser `[Export]` ou `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` ne peut pas être utilisé sur les méthodes qui retournent `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Instances de types génériques _ne doit pas_ être créés à partir de code Java.
    Uniquement en toute sécurité, ils peuvent être créés à partir du code managé :

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```

<a name="Partial_Java_Generics_Support" />

## <a name="partial-java-generics-support"></a>Prise en charge des génériques Java partielle

Le Java génériques liaison prennent en charge est limité. En particulier, les membres d’une classe d’instance générique qui est dérivé d’une autre classe générique (non instanciée) sont laissés exposées en tant que Java.Lang.Object. Par exemple, [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) méthode Java.Lang.Object est retournée. Cela est dû effacées génériques de Java.
Nous avons des classes qui ne s’appliquent pas cette limitation, mais elles sont ajustées manuellement.


## <a name="related-links"></a>Liens associés

- [Wrappers RCW Android](~/android/platform/java-integration/android-callable-wrappers.md)
- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
