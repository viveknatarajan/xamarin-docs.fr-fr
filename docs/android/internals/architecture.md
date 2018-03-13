---
title: Architecture
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 47f90af1ed68e6c3aea5710b7181b4787fc0895c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="architecture"></a>Architecture

Applications de Xamarin.Android s’exécutent dans l’environnement d’exécution Mono.
Cette exécution environnement s’exécute-côte avec la machine virtuelle de Runtime Android (ART). Les deux environnements d’exécution s’exécutent sur le noyau Linux et exposent les diverses API au code utilisateur qui permet aux développeurs d’accéder au système sous-jacent. Le runtime Mono est écrit dans le langage C.

Vous pouvez utiliser la [système](http://msdn.microsoft.com/en-us/library/system.aspx), [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx), [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx) et le reste du .NET Framework classe bibliothèques pour les installations du système d’exploitation Linux sous-jacente d’accès.

Sur Android, la plupart des installations du système tels que Audio, des graphiques, OpenGL et téléphonie n’est pas accessible directement aux applications natives, ils ne sont plus exposés via les API de Java Runtime Android résidant dans un de le [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * espaces de noms ou le [Android](https://developer.xamarin.com/api/namespace/Android/). * espaces de noms. L’architecture est à peu près comme suit :

[![Diagramme de Mono et de créations au-dessus du noyau et sous .NET/Java + liaisons](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Les développeurs de Xamarin.Android accéder les diverses fonctionnalités du système d’exploitation en appelant des API .NET qu’ils connaissent (pour l’accès de bas niveau) ou l’utilisation des classes exposées dans les espaces de noms Android qui fournit un pont à l’API Java qui sont exposées par le Runtime Android.

Pour plus d’informations sur la façon dont les classes Android communiquent avec les classes d’exécution Android, consultez le [conception de l’API](~/android/internals/api-design.md) document.


## <a name="application-packages"></a>Packages d’application

Packages d’application Android sont des conteneurs ZIP avec un *.apk* extension de fichier. Les packages d’applications Xamarin.Android ont la même structure et la disposition en tant que les packages Android normales, avec les ajouts suivants :

-   Les assemblys d’application (qui contient le langage intermédiaire) sont *stockées* non compressé dans le *assemblys* dossier. Au cours du processus de démarrage dans la version génère la *.apk* est *mmap()* ed dans le processus et les assemblys sont chargés à partir de la mémoire. Cela permet le démarrage accéléré des applications, en tant qu’assemblys n’ont ne pas à extraire avant l’exécution. - *Remarque :* les informations d’emplacement Assembly comme [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/) et [Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *ne peut pas se reposer* de version builds. Ils n’existent pas sous forme d’entrées de système de fichiers distincts et n’avoir aucun emplacement utilisable.


-   Bibliothèques natives contenant le runtime Mono sont présents dans le *.apk* . Une application de Xamarin.Android doit contenir des bibliothèques natives pour les architectures Android ciblées souhaité, par exemple, *armeabi* , *armeabi-v7a* , *x86* . Impossible d’exécuter Xamarin.Android applications sur une plateforme, sauf si elle contient les bibliothèques runtime appropriées.


Contiennent également des applications de Xamarin.Android *Android Callable Wrappers* permettant d’Android pour l’appel de code managé.



## <a name="android-callable-wrappers"></a>Wrappers RCW Android

- **Les wrappers RCW Android** sont un [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) pont qui servent à tout moment, le runtime Android doit appeler le code managé. Les wrappers RCW Android sont les méthodes virtuelles peut être remplacée et d’interfaces Java peuvent être implémentées. Consultez le [vue d’ensemble de l’intégration de Java](~/android/platform/java-integration/index.md) sur la documentation pour plus d’informations.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Wrappers CCW managés

Wrappers CCW managés sont un pont JNI qui servent à tout moment, le code managé doit appeler le code Android et prennent en charge de substituer les méthodes virtuelles et d’implémentation d’interfaces Java. L’ensemble de [Android](https://developer.xamarin.com/api/namespace/Android/). * et les espaces de noms sont des wrappers managés pouvant être appelés générées via [.jar liaison](~/android/platform/binding-java-library/index.md).
Wrappers CCW managés sont responsables de la conversion entre les types managés et Android et appeler les méthodes sous-jacente de la plateforme Android via JNI.

Chacun créé managé wrapper CCW contient une référence globale Java, qui est accessible via la [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) propriété. Références globales sont utilisées pour fournir le mappage entre les instances de Java et des instances gérées. Références globales sont une ressource limitée : émulateurs autorisent uniquement 2000 références globales d’exister à la fois, tandis que la plupart des matériels permet plus de 52 000 références globales d’exister à la fois.

Pour suivre lors de la création et la destruction des références globales, vous pouvez définir le [debug.mono.log](~/android/troubleshooting/index.md) propriété système pour contenir [gref](~/android/troubleshooting/index.md).

Références globales peuvent être libérées explicitement en appelant [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) sur le wrapper managé. Vous supprimez le mappage entre l’instance de Java et de l’instance gérée et l’instance Java à collecter. Si l’instance Java est nouveau accessible à partir du code managé, un wrapper CCW managé sera créé pour lui.

Vous devez être prudent lors de la mettre à disposition gérés Callable Wrappers si l’instance peut être partagée par inadvertance entre les threads, comme la suppression de l’instance a un impact sur les références à partir de tous les autres threads. Pour une sécurité optimale, uniquement `Dispose()` d’instances qui ont été attribuées via `new` *ou* à partir des méthodes que vous pouvez *connaître* toujours allouer de nouvelles instances et les instances non mis en cache peuvent provoquer le partage entre les threads de l’instance accidentelle.



## <a name="managed-callable-wrapper-subclasses"></a>Géré des sous-classes de Wrapper RCW

Les sous-classes de wrapper CCW managé sont où toute la logique spécifique à l’application « intéressante » peut résider. Ces services personnalisés [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) sous-classes (telles que la [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) type dans le modèle de projet par défaut). (Plus précisément, ce sont les *Java.Lang.Object* sous-classes qui *pas* contiennent un [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) attribut personnalisé ou [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) est *false*, qui est la valeur par défaut.)

Comme gérés callable wrappers, gérés sous-classes du wrapper CCW contient également une référence globale, accessible via la [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) propriété. Tout comme avec les wrappers CCW gérés références globales peuvent être libérées explicitement en appelant [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Contrairement aux wrappers CCW gérés *précaution* doit être prise avant la suppression d’instances de ce type, en tant que *Dispose()*effectue une opération de l’instance s’arrêtera le mappage entre l’instance Java (une instance d’un Android Callable Wrapper) et l’instance gérée.


### <a name="java-activation"></a>Activation de Java

Lorsqu’un [Wrapper RCW Android](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) est créé à partir de Java, le constructeur ACW entraîne le constructeur c# correspondant à appeler. Par exemple, le ACW pour *MainActivity* contiendra un constructeur par défaut qui appelle *MainActivity*du constructeur par défaut. (Cette opération est effectuée via le *TypeManager.Activate()* appeler dans les constructeurs ACW.)

Il existe une signature de constructeur de conséquence : le *(IntPtr, JniHandleOwnership)* constructeur. Le *(IntPtr, JniHandleOwnership)* constructeur est appelé chaque fois qu’un objet Java est exposé au code managé et un Wrapper CCW gérés doit être construit pour gérer le handle JNI. Cela est généralement réalisée automatiquement.

Il existe deux scénarios dans lesquels le *(IntPtr, JniHandleOwnership)* constructeur doit être fourni manuellement une sous-classe gérés Callable Wrapper :

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) est sous-classée. *Application* est spéciale ; la valeur par défaut *application* constructeur sera *jamais* être appelé et le [(IntPtr, JniHandleOwnership) constructeur au lieu de cela doit être fourni ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Appel de méthode virtuelle à partir d’un constructeur de classe de base.


Notez que (2) est une abstraction de fuite. En Java, comme dans c#, les appels aux méthodes virtuelles à partir d’un constructeur appelle toujours l’implémentation de la méthode la plus dérivée. Par exemple, le [TextView (contexte, AttributeSet, int) constructeur](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) appelle la méthode virtuelle [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), qui est lié en tant que le [ Propriété de TextView.DefaultMovementMethod](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Par conséquent, si un type [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) ont été (1) [sous-classe TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [remplacer TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)et (3) [activer une instance de ce classe via XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) la *DefaultMovementMethod* propriété serait appelée avant le constructeur ACW avait une chance de s’exécuter, et il se produit avant que le constructeur c# ait une chance de s’exécuter.

Cela est pris en charge en instanciant une instance LogTextBox via la [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) constructeur lors de l’instance ACW LogTextBox entre d’abord dans le code géré et puis d’appeler le [ LogTextBox (contexte, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) constructeur *sur la même instance* lorsque le constructeur ACW s’exécute.

Ordre des événements :

1.  Mise en forme XML est chargé dans un [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android instancie le graphique d’objets de mise en page et instancie une instance de *monodroid.apidemo.LogTextBox* , le ACW pour *LogTextBox* .

3.  Le *monodroid.apidemo.LogTextBox* constructeur s’exécute le [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) constructeur.

4.  Le *TextView* constructeur appelle *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* appelle *LogTextBox.n_getDefaultMovementMethod()* , qui appelle *TextView.n_GetDefaultMovementMethod()* , qui appelle [Java.Lang.Object.GetObject&lt;TextView&gt; (gérer, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* vérifie s’il existe déjà un c# correspondant de l’instance pour *gérer* . S’il existe, il est retourné. Dans ce scénario, il n’existe pas, donc *Object.GetObject&lt;T&gt;()* devez en créer un.

7.  *Object.GetObject&lt;T&gt;()* recherche le *LogTextBox (IntPtr, JniHandleOwneship)* constructeur, il appelle, crée un mappage entre *gérer* et l’instance créée et retourne l’instance créée.

8.  *TextView.n_GetDefaultMovementMethod()* appelle la *LogTextBox.DefaultMovementMethod* accesseur Get de propriété.

9.  Le contrôle retourne à la *android.widget.TextView* constructeur, ce qui termine l’exécution.

10. Le *monodroid.apidemo.LogTextBox* constructeur s’exécute, appeler *TypeManager.Activate()* .

11. Le *LogTextBox (contexte, IAttributeSet, int)* constructeur exécute *sur la même instance créée dans (7)* .

12. ...


Si (IntPtr, JniHandleOwnership) Impossible de trouver un constructeur, puis un [System.MissingMethodException](https://developer.xamarin.com/api/type/System.MissingMethodException/) sera levée.


<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Appels de Dispose() prématurée

Il existe un mappage entre un handle JNI et l’instance correspondante de c#. Java.Lang.Object.Dispose() s’arrête à ce mappage. Si un handle JNI entre un code managé après le mappage a été interrompue, il semble que l’Activation de Java et le *(IntPtr, JniHandleOwnership)* constructeur sera vérifié et appelé. Si le constructeur n’existe pas, une exception sera levée.

Par exemple, étant donné la sous-classe gérés un CCW Wraper suivante :

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

Si nous créer une instance, Dispose() et entraîner le Managed Wrapper d’être recréés :

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Le programme sera matrice :

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

Si la sous-classe ne contient-elle pas un *(IntPtr, JniHandleOwnership)* constructeur, puis un *nouveau* instance du type est créée. Par conséquent, l’instance s’affiche « perte » de toutes les données d’instance, car il s’agit d’une nouvelle instance. (Notez que la valeur est null).

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Uniquement *Dispose()* de gérés sous-classes de wrapper RCW lorsque vous savez que l’objet Java ne sera pas plus utilisé, ou la sous-classe ne contient aucune donnée d’instance et un *(IntPtr, JniHandleOwnership)* constructeur a été fourni.



## <a name="application-startup"></a>Démarrage de l'application

Lorsqu’une activité, un service, etc. est lancé, Android vérifie d’abord pour voir s’il existe déjà un processus en cours d’exécution pour héberger l’activité de service, etc. Si aucun processus n’existe, puis à créer un nouveau processus, le [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) est lu et le type spécifié dans le [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) attribut est chargé et instancié. Ensuite, tous les types spécifiés par le [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) valeurs d’attribut sont instanciés et avoir leur [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) méthode appelée. Hook de Xamarin.Android à cela en ajoutant un *mono. MonoRuntimeProvider* *ContentProvider* à AndroidManifest.xml pendant le processus de génération. Le *mono. MonoRuntimeProvider.attachInfo()* méthode est responsable du chargement du runtime Mono dans le processus.
Toute tentative d’utiliser Mono avant ce point échoue. ( *Remarque*: c’est pourquoi les types qui sous-classe [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) devez fournir un [(IntPtr, JniHandleOwnership) constructeur](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), comme l’instance d’Application est créé avant de pouvoir initialiser Mono.)

Une fois terminé, l’initialisation du processus `AndroidManifest.xml` est consulté pour rechercher le nom de classe de l’activité/service/etc. à lancer. Par exemple, le [ /manifest/application/activity/@android:name attribut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) est utilisé pour déterminer le nom d’une activité à charger. Pour les activités, ce type doit hériter de [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Le type spécifié est chargé via [Class.forName](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (ce qui nécessite que le type soit un Java type, par conséquent, les Wrappers Android), puis instancié. Création d’une instance du Wrapper RCW Android déclenchera la création d’une instance du type c# correspondant. Android puis appelle [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , ce qui entraîne une correspondants [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) à appeler, et vous avez les courses.
