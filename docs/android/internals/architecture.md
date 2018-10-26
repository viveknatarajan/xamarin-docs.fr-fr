---
title: Architecture
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 219c6bb4cd5718c969ba83a55596ad7b0bab8baf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121124"
---
# <a name="architecture"></a>Architecture

Les applications Xamarin.Android s’exécutent dans l’environnement d’exécution Mono.
Cette exécution environnement exécutions-côte avec la machine virtuelle de Runtime Android (ART). Les deux environnements d’exécution s’exécutent sur le noyau Linux et exposent les diverses API au code utilisateur qui permet aux développeurs d’accéder au système sous-jacent. Le runtime Mono est écrit dans le langage C.

Vous pouvez utiliser la [système](xref:System), [System.IO](xref:System.IO), [System.Net](xref:System.Net) et bibliothèques d’accès les installations de système d’exploitation Linux sous-jacente à la classe le reste de .NET.

Sur Android, la plupart des installations du système tels que Audio, graphiques, OpenGL et téléphonie n’est pas directement accessible aux applications natives, elles sont exposées uniquement via les API de Java Runtime Android résidant dans un de le [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * espaces de noms ou le [Android](https://developer.xamarin.com/api/namespace/Android/). * espaces de noms. L’architecture est à peu près comme ceci :

[![Diagramme de Mono et ART au-dessus du noyau et en dessous de .NET/Java + liaisons](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Les développeurs Xamarin.Android accéder aux fonctions du système d’exploitation en appelant des API de .NET qui ils savent (pour l’accès de bas niveau) ou en utilisant les classes exposées dans les espaces de noms Android qui fournit un pont à l’API Java qui sont exposées par le Runtime Android.

Pour plus d’informations sur la façon dont les classes Android communiquent avec les classes de Runtime Android, consultez le [conception d’API](~/android/internals/api-design.md) document.


## <a name="application-packages"></a>Packages d’application

Packages d’applications Android sont des conteneurs ZIP avec une *.apk* extension de fichier. Packages d’applications Xamarin.Android ont la même structure et disposition sous forme de packages d’Android normales, avec les ajouts suivants :

-   Les assemblys d’application (qui contient le langage intermédiaire) sont *stockées* non compressé dans le *assemblys* dossier. Au cours du processus de démarrage dans la version génère la *.apk* est *mmap()* ed dans le processus et les assemblys sont chargés à partir de la mémoire. Cela permet un démarrage plus rapide d’application, comme assemblys n’êtes pas obligé d’être extrait avant l’exécution. - *Remarque :* les informations d’emplacement Assembly comme [Assembly.Location](xref:System.Reflection.Assembly.Location) et [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *ne peut pas se reposer* dans la mise en production builds. Ils n’existent pas sous forme d’entrées de système de fichiers distinct, et ils n’ont à aucun emplacement utilisable.


-   Contenant le runtime Mono de bibliothèques natives sont présents dans le *.apk* . Une application Xamarin.Android doit contenir les bibliothèques natives pour les architectures Android ciblées souhaité, par exemple, *armeabi* , *armeabi-v7a* , *x86* . Impossible d’exécuter les applications Xamarin.Android sur une plateforme, sauf si elle contient les bibliothèques runtime approprié.


Contiennent également des applications Xamarin.Android *Android Callable Wrappers* pour autoriser Android à appeler dans du code managé.



## <a name="android-callable-wrappers"></a>Wrappers RCW Android

- **Les wrappers RCW Android** sont un [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) pont qui servent à tout moment, le runtime Android a besoin pour appeler le code managé. Les wrappers RCW Android sont des méthodes virtuelles peut être substituée et d’interfaces Java peuvent être implémentées. Consultez le [présentation de l’intégration Java](~/android/platform/java-integration/index.md) doc pour plus d’informations.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Wrappers RCW gérés

Les wrappers RCW gérés sont un pont JNI qui servent à tout moment, le code managé doit appeler le code Android et prennent en charge la substitution de méthodes virtuelles et d’implémentation d’interfaces de Java. L’intégralité de [Android](https://developer.xamarin.com/api/namespace/Android/). * et les espaces de noms sont des wrappers RCW managés générés par le biais de [.jar liaison](~/android/platform/binding-java-library/index.md).
Les wrappers RCW managés sont responsables de conversion entre les types managés et Android et d’appeler les méthodes de plateforme Android sous-jacent via JNI.

Chacun créé managé wrapper CCW contient une référence globale Java, qui est accessible via la [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) propriété. Références globales sont utilisées pour fournir le mappage entre les instances de Java et des instances gérées. Références globales sont une ressource limitée : émulateurs autorisent uniquement 2000 références globales d’exister à la fois, tandis que la plupart des matériels permet plus de 52 000 références globales d’exister à la fois.

Pour effectuer le suivi des références globales sont créés et détruits, vous pouvez définir le [debug.mono.log](~/android/troubleshooting/index.md) propriété système pour contenir [gref](~/android/troubleshooting/index.md).

Références globales peuvent être libérées explicitement en appelant [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) sur le wrapper managé. Cela supprime le mappage entre l’instance de Java et l’instance gérée et permettre à l’instance de Java à collecter. Si l’instance Java est nouveau accessible à partir du code managé, un nouveau callable wrapper géré sera créé pour lui.

Vous devez être prudent lors de la mettre à disposition gérés des Wrappers RCW si l’instance peut être partagée par inadvertance entre les threads, comme l’instance en cours de suppression aura un impact sur les références à partir de tous les autres threads. Pour une sécurité optimale, uniquement `Dispose()` d’instances qui ont été alloués par le biais de `new` *ou* à partir de méthodes que vous *savoir* toujours allouer de nouvelles instances et des instances non mis en cache qui peut provoquer le partage entre les threads de l’instance accidentelle.



## <a name="managed-callable-wrapper-subclasses"></a>Managed Wrapper RCW sous-classes

Les sous-classes de wrapper RCW managé sont où réside peut-être toute la logique spécifique à l’application « intéressante ». Ceux-ci incluent custom [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) sous-classes (telles que la [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) type dans le modèle de projet par défaut). (Plus précisément, ce sont les *Java.Lang.Object* sous-classes ainsi faire *pas* contiennent un [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) attribut personnalisé ou [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) est *false*, qui est la valeur par défaut.)

Gérés comme des wrappers RCW gérés sous-classes de wrapper CCW contient également une référence globale, accessible via le [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) propriété. Tout comme avec des wrappers RCW gérés, des références globales peuvent être libérées explicitement en appelant [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Contrairement à callable wrappers gérés *plus grand soin* doit être effectuée avant la suppression des instances de ce type, comme *Dispose()*- binaire de l’instance s’arrêtera le mappage entre l’instance Java (une instance d’un Wrapper CCW Android) et l’instance gérée.


### <a name="java-activation"></a>Activation de Java

Quand un [Android Callable Wrapper](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) est créée à partir de Java, le constructeur ACW entraîne le constructeur c# correspondant à appeler. Par exemple, le ACW pour *MainActivity* contiendra un constructeur par défaut qui appelle *MainActivity*du constructeur par défaut. (Cette opération est effectuée via le *TypeManager.Activate()* appeler dans les constructeurs ACW.)

Il existe une autre signature de constructeur de conséquence : le *(IntPtr, JniHandleOwnership)* constructeur. Le *(IntPtr, JniHandleOwnership)* constructeur est appelé chaque fois qu’un objet Java est exposé au code managé et un Wrapper RCW gérés doit être construit pour gérer le handle JNI. Cela s’effectue habituellement automatiquement.

Il existe deux scénarios dans lesquels le *(IntPtr, JniHandleOwnership)* constructeur doit être fourni manuellement sur une sous-classe gérés Callable Wrapper :

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) est une sous-classe. *Application* est spécial ; la valeur par défaut *application* constructeur sera *jamais* être appelé et le [(IntPtr, JniHandleOwnership) constructeur doit plutôt être fourni ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Appel de méthode virtuelle à partir d’un constructeur de classe de base.


Notez que (2) est une abstraction de fuite. En Java, comme dans c#, les appels aux méthodes virtuelles à partir d’un constructeur toujours appeler l’implémentation de la méthode la plus dérivée. Par exemple, le [TextView (contexte, AttributeSet, int) constructeur](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) appelle la méthode virtuelle [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), qui est lié en tant que le [ Propriété de TextView.DefaultMovementMethod](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Par conséquent, si un type [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) ont été (1) [sous-classe TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [remplacer TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)et (3) [activer une instance de ce classe par le biais de XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) substituées *DefaultMovementMethod* propriété peut être appelée avant le constructeur ACW avait une chance de s’exécuter, et il se produit avant le C# constructeur eu l’occasion exécuter.

Cela est pris en charge en instanciant une instance LogTextBox via la [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) constructeur lorsque l’instance ACW LogTextBox entre d’abord dans le code géré et puis en appelant le [ LogTextBox (contexte, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) constructeur *sur la même instance* lorsque le constructeur ACW s’exécute.

Ordre des événements :

1.  Mise en forme XML est chargé dans un [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android instancie le graphique d’objets de mise en page et instancie une instance de *monodroid.apidemo.LogTextBox* , le ACW pour *LogTextBox* .

3.  Le *monodroid.apidemo.LogTextBox* constructeur s’exécute le [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) constructeur.

4.  Le *TextView* constructeur appelle *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* appelle *LogTextBox.n_getDefaultMovementMethod()* , qui appelle *TextView.n_GetDefaultMovementMethod()* , qui appelle [Java.Lang.Object.GetObject&lt;TextView&gt; (gérer, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* vérifie s’il existe déjà un c# correspondant de l’instance pour *gérer* . S’il existe, elle est retournée. Dans ce scénario, il n’existe pas, donc *Object.GetObject&lt;T&gt;()* devez en créer un.

7.  *Object.GetObject&lt;T&gt;()* recherche le *LogTextBox (IntPtr, JniHandleOwneship)* constructeur, il appelle, crée un mappage entre *gérer* et l’instance créée et retourne l’instance créée.

8.  *TextView.n_GetDefaultMovementMethod()* appelle le *LogTextBox.DefaultMovementMethod* accesseur Get de propriété.

9.  Le contrôle retourne à la *android.widget.TextView* constructeur, ce qui termine l’exécution.

10. Le *monodroid.apidemo.LogTextBox* constructeur s’exécute, appel *TypeManager.Activate()* .

11. Le *LogTextBox (contexte, IAttributeSet, int)* constructeur exécute *sur la même instance créée dans (7)* .

12. Si (IntPtr, JniHandleOwnership) constructeur ne peut pas être trouvé, puis un System.MissingMethodException](xref:System.MissingMethodException) sera levée.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Appels de Dispose() prématurée

Il existe un mappage entre un handle JNI et l’instance correspondante de c#. Java.Lang.Object.Dispose() s’arrête à ce mappage. Si un handle JNI entre code managé une fois que le mappage a été interrompue, elle ressemble à l’Activation de Java et le *(IntPtr, JniHandleOwnership)* constructeur sera recherché et appelé. Si le constructeur n’existe pas, une exception sera levée.

Par exemple, étant donné la sous-classe gérés un RCW Wraper suivante :

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

Si nous avons créer une instance de Dispose() et provoquer le Wrapper pouvant être appelé gérés d’être recréés :

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Le programme fait de même :

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

Si la sous-classe ne contient-elle pas une *(IntPtr, JniHandleOwnership)* constructeur, puis un *nouveau* instance du type sera créée. Par conséquent, l’instance s’affichent pour toutes les données d’instance, « perdre » car il s’agit d’une nouvelle instance. (Notez que la valeur est null).

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Uniquement *Dispose()* de gérés des sous-classes de wrapper RCW lorsque vous savez que l’objet Java ne sera pas plus utilisé, ou la sous-classe ne contient aucune donnée d’instance et un *(IntPtr, JniHandleOwnership)* constructeur a été fourni.



## <a name="application-startup"></a>Démarrage de l'application

Lorsqu’une activité, un service, etc. est lancé, Android vérifie d’abord pour voir s’il existe déjà un processus en cours d’exécution pour héberger l’activité/service/etc. Si aucun processus n’existe, un nouveau processus sera créé, le [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) est en lecture et le type spécifié dans le [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) attribut est chargé et instancié. Ensuite, tous les types spécifiés par le [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) valeurs d’attribut sont instanciés et ont leurs [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) méthode appelée. Xamarin.Android hook cela en ajoutant un *mono. MonoRuntimeProvider* *ContentProvider* à AndroidManifest.xml pendant le processus de génération. Le *mono. MonoRuntimeProvider.attachInfo()* méthode est responsable du chargement du runtime Mono dans le processus.
Toute tentative d’utilisation de Mono avant ce point échouera. ( *Remarque*: c’est pourquoi les types qui sont une sous-classe [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) devez fournir un [(IntPtr, JniHandleOwnership) constructeur](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), en l’état de l’instance d’Application créé avant l’initialisation de Mono.)

Une fois terminé, l’initialisation du processus `AndroidManifest.xml` est consulté pour rechercher le nom de classe de l’activité/service/etc. pour lancer. Par exemple, le [ /manifest/application/activity/@android:name attribut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) est utilisé pour déterminer le nom d’une activité à charger. Pour les activités, ce type doit hériter [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Le type spécifié est chargé par le biais de [Class.forName](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (qui nécessite que le type soit un Java tapez, par conséquent, les Wrappers RCW Android), puis instancié. Création d’une instance de Wrapper RCW Android déclenchera la création d’une instance du type c# correspondant. Android invoquera ensuite [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , ce qui provoquera le correspondantes [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) à appeler, et vous êtes hors tension pour les concurrences.
