---
title: Wrappers pouvant être appelés par Android
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 7edbdaa5a690a641523cb5baad7909ed01992aa5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61090865"
---
# <a name="android-callable-wrappers"></a>Wrappers pouvant être appelés par Android

Les Wrappers RCW Android (ACWs) sont requis chaque fois que le runtime Android appelle du code managé. Ces wrappers sont requis, car il n’existe aucun moyen pour inscrire des classes avec l’ART (le runtime Android) lors de l’exécution. (Plus précisément, le [JNI DefineClass() fonction](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) n’est pas pris en charge par le runtime Android.} Les Wrappers RCW Android constituent ainsi l’absence de prise en charge de l’inscription de type runtime. 

*Chaque fois que* code Android doit exécuter un `virtual` ou de la méthode qui est l’interface `overridden` ou implémentée en code managé, Xamarin.Android doit fournir un proxy Java pour que cette méthode est distribuée vers le type managé approprié. Ces types de proxy Java sont du code Java qui dispose de la classe de base « même » et de la liste des interfaces Java en tant que type managé, implémenter les constructeurs mêmes et la déclaration de n’importe quelle classe de base substituée et méthodes d’interface. 

Android wrappers RCW sont générés par le **monodroid.exe** du programme pendant le [du processus de génération](~/android/deploy-test/building-apps/build-process.md): ils sont générés pour tous les types qui héritent de (directement ou indirectement) [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 



## <a name="android-callable-wrapper-naming"></a>Wrapper RCW Android d’affectation de noms

Les noms de package pour Android Wrappers RCW sont basés sur le MD5SUM du nom qualifié d’assembly du type en cours d’exportation. Cette technique d’affectation de noms rend possible pour le même nom de type qualifié complet à la disposition par les différents assemblys sans introduire une erreur d’empaquetage. 

En raison de ce schéma d’affectation de noms MD5SUM, vous ne pouvez pas accéder directement vos types par nom. Par exemple, ce qui suit `adb` commande ne fonctionnera pas car le nom de type `my.ActivityType` n’est pas généré par défaut : 

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

Le `ActivityAttribute.Name` propriété peut être définie pour déclarer explicitement le nom de cette activité : 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Une fois ce paramètre de propriété est ajouté, `my.ActivityType` peut être accédé par nom de code externe et de `adb` scripts. Le `Name` attribut peut être défini pour de nombreux types différents, y compris `Activity`, `Application`, `Service`, `BroadcastReceiver`, et `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Md5sum ACW affectation de nom a été introduite dans Xamarin.Android 5.0. Pour plus d’informations sur le nom d’attribut, consultez [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 



## <a name="implementing-interfaces"></a>Implémentation des interfaces

Parfois, lorsque vous devez implémenter une interface Android, tel que [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Étant donné que toutes les classes Android et interface étendent le [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interface, la question se pose : comment nous implémentons `IJavaObject`? 

Répondre à la question ci-dessus : la raison pour laquelle tous les types Android doivent implémenter `IJavaObject` est de pouvoir Xamarin.Android dispose d’un wrapper RCW Android pour fournir à Android, par exemple, un proxy Java pour le type donné. Dans la mesure où **monodroid.exe** recherche uniquement `Java.Lang.Object` sous-classes, et `Java.Lang.Object` implémente `IJavaObject,` la réponse est évidente : sous-classe `Java.Lang.Object`: 

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


## <a name="implementation-details"></a>Détails de l’implémentation

*Le reste de cette page fournit des détails d’implémentation susceptibles d’être modifiées sans préavis* (et est présentée ici uniquement parce que les développeurs seront curieux de savoir ce qui se passait). 

Par exemple, étant donné ce qui suit C# source :

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

Le **mandroid.exe** programme générera le Wrapper Android suivants : 

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

Notez que la classe de base est conservée, et `native` les déclarations de méthode sont fournies pour chaque méthode est substituée dans du code géré. 
