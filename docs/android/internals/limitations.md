---
title: Visual Studio Xamarin.Android. Bureau - différences dans le Runtime Mono
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 115d715214d7af3174c41d9d82e894ce429dab42
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953342"
---
# <a name="limitations"></a>Limitations

Dans la mesure où les applications sur Android requièrent la génération de types de proxy de Java pendant le processus de génération, il n’est pas possible de générer tout le code lors de l’exécution.

Voici les limitations de Xamarin.Android par rapport au bureau Mono :


## <a name="limited-dynamic-language-support"></a>Prise en charge limitée de langage dynamique

 [Les wrappers RCW Android](~/android/platform/java-integration/android-callable-wrappers.md) sont nécessaires à tout moment, le runtime Android a besoin pour appeler le code managé. Les wrappers RCW Android sont générées au moment de la compilation, en fonction de l’analyse statique du langage intermédiaire. Le résultat net de cela : vous *ne peut pas* utiliser des langages dynamiques (IronPython, IronRuby, etc.) dans tout scénario où le sous-classement de types Java est requis (y compris le sous-classement indirect), comme il n’existe aucun moyen d’extraction de ces types dynamiques au moment de la compilation pour générer les wrappers RCW Android nécessaires.


## <a name="limited-java-generation-support"></a>Prise en charge de la génération Java limitée

[Les Wrappers RCW Android](~/android/platform/java-integration/android-callable-wrappers.md) doivent être générés dans l’ordre pour le code Java appeler du code managé. *Par défaut*, les wrappers RCW Android contiendra uniquement (certains) déclaré de constructeur et les méthodes de substituent une méthode virtuelle de Java (par exemple, il a [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) ou implémenter un (méthode) interface Java interface même a `Attribute`).
  
Avant la version 4.1, aucune des méthodes supplémentaires ne peuvent être déclarés. Avec la version 4.1, [le `Export` et `ExportField` des attributs personnalisés peuvent être utilisés pour déclarer des méthodes Java et les champs dans le Wrapper Android](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Constructeurs manquants

Constructeurs restent difficile, sauf si [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) est utilisé. L’algorithme permettant de générer des constructeurs de wrapper RCW Android est qu’un constructeur de Java sera émis si :

1. Il existe un mappage de Java pour tous les types de paramètre
2. La classe de base déclare le constructeur même &ndash; cela est nécessaire car le wrapper RCW Android *doit* appeler le constructeur de classe de base correspondante ; les arguments par défaut peuvent être utilisés (comme il n’existe aucun moyen simple déterminer les valeurs doivent être utilisées dans Java).

Par exemple, considérons la classe suivante :

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Bien que cette recherche parfaitement logique, le wrapper RCW Android résultant *dans les versions Release* ne contient pas un constructeur par défaut. Par conséquent, si vous essayez de démarrer ce service (par exemple, [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), il échoue :

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

La solution de contournement consiste à déclarer un constructeur par défaut, avec orner le `ExportAttribute`et définissez le [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

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


### <a name="generic-c-classes"></a>Générique C# classes

Générique C# sont partiellement, en classes. Les limitations suivantes existent :


-   Types génériques ne peuvent pas utiliser `[Export]` ou `[ExportField`]. Une telle tentative génère une `XA4207` erreur.

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

-   `[ExportField]` ne peut pas être utilisé sur les méthodes qui retournent des `void`:

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


## <a name="partial-java-generics-support"></a>Prise en charge des génériques Java partielle

Le Java génériques liaison prennent en charge est limité. En particulier, les membres dans une classe d’instance générique qui est dérivée d’une autre classe générique (non instanciée) sont laissés exposées en tant que Java.Lang.Object. Par exemple, [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) méthode Java.Lang.Object est retournée. Cela est dû effacés génériques de Java.
Nous disposons des classes qui ne s’appliquent pas cette limitation, mais elles sont ajustées manuellement.


## <a name="related-links"></a>Liens associés

- [Wrappers pouvant être appelés par Android](~/android/platform/java-integration/android-callable-wrappers.md)
- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
