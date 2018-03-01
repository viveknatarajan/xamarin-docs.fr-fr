---
title: Wrappers RCW Android
ms.topic: article
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f618f7257ab082a2a5b0aa587b135ad169d15133
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-callable-wrappers"></a>Wrappers RCW Android

Les Wrappers RCW Android (ACWs) sont requis chaque fois que le runtime Android appelle du code managé. Ces wrappers sont requis, car il n’existe aucun moyen pour inscrire des classes avec l’ART (le runtime Android) lors de l’exécution. (Plus précisément, le [JNI DefineClass() fonction](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) n’est pas pris en charge par le runtime Android.} Des Wrappers RCW Android constituent donc de l’absence de prise en charge de l’inscription de type runtime. 

*Chaque fois que* code Android doit exécuter un `virtual` ou de la méthode qui est de l’interface `overridden` ou implémentée en code managé, Xamarin.Android doit fournir un proxy Java pour que cette méthode est distribuée vers le type managé approprié. Ces types de proxy Java sont le code Java qui dispose de la classe de base « même » et la liste des interfaces Java en tant que le type managé, en implémentant les constructeurs mêmes et en déclarant n’importe quelle classe de base substituée et les méthodes d’interface. 

Les wrappers RCW Android sont générés par le **monodroid.exe** programme pendant la [processus de génération](~/android/deploy-test/building-apps/build-process.md): qu’ils sont générés pour tous les types qui héritent de (directement ou indirectement) [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 


<a name="ACW_Naming" />

## <a name="android-callable-wrapper-naming"></a>Wrapper RCW Android d’affectation de noms

Les noms de package pour Android Callable Wrappers sont basés sur le MD5SUM du nom qualifié d’assembly du type en cours d’exportation. Cette technique d’affectation de noms rend possible pour le même nom de type qualifié complet pour être mises à disposition par différents assemblys sans introduire une erreur d’empaquetage. 

En raison de ce schéma d’affectation de noms MD5SUM, vous ne peut pas accéder directement à vos types par nom. Par exemple, `adb` commande ne fonctionne pas car le nom de type `my.ActivityType` n’est pas généré par défaut : 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

En outre, vous pouvez voir les erreurs comme suit si vous tentez de référencer un type par nom :

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Si vous *faire* requièrent l’accès aux types par nom, vous pouvez déclarer un nom pour ce type dans une déclaration d’attribut. Par exemple, voici le code qui déclare une activité dont le nom qualifié complet `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Le `ActivityAttribute.Name` propriété peut être définie à déclarer explicitement le nom de cette activité : 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Une fois que ce paramètre de propriété est ajouté, `my.ActivityType` sont accessibles par nom à partir du code externe et de `adb` des scripts. Le `Name` attribut peut être défini pour de nombreux types `Activity`, `Application`, `Service`, `BroadcastReceiver`, et `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Md5sum-ACW affectation de nom a été introduite dans Xamarin.Android 5.0. Pour plus d’informations sur le nom d’attribut, consultez [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 


<a name="Implementing_Interfaces" />

## <a name="implementing-interfaces"></a>Implémentation des interfaces

Il se peut que lorsque vous devez implémenter une interface Android, tel que [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Étant donné que toutes les classes Android et interface étendre le [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interface, la question se pose : comment nous implémentons `IJavaObject`? 

Répondre à la question ci-dessus : la raison pour laquelle tous les types Android doivent implémenter `IJavaObject` est afin que Xamarin.Android a un wrapper CCW Android à fournir pour Android, autrement dit, un proxy Java pour le type donné. Étant donné que **monodroid.exe** recherche uniquement `Java.Lang.Object` sous-classes, et `Java.Lang.Object` implémente `IJavaObject,` la réponse est évidente : sous-classe `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

<a name="Implementation_Details" />

## <a name="implementation-details"></a>Détails d’implémentation

*Le reste de cette page fournit des détails d’implémentation susceptibles de changer sans préavis* (et est présenté ici uniquement parce que les développeurs seront obtenir des informations sur ce qui se passe). 

Par exemple, étant donné la source c# suivante :

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

Le **mandroid.exe** programme génère le Wrapper Android suivantes : 

```java
package mono.samples.helloWorld;

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Notez que la classe de base est conservée, et `native` les déclarations de méthode sont fournies pour chaque méthode de substitution dans le code managé. 
