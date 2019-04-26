---
title: Rappels sur Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: c6eaf4dd90b172053b4b87e3427cfe35213c6727
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215876"
---
# <a name="callbacks-on-android"></a>Rappels sur Android

Appel de Java à partir de C# est quelque peu un *risqué*. C'est-à-dire qu’il existe un *modèle* pour les rappels de C# à Java ; Toutefois, il est plus compliqué que nous voudrions.

Nous aborderons les trois options permettant d’effectuer des rappels qui sont les plus pertinents pour Java :

- Classes abstraites
- Interfaces
- Méthodes virtuelles

## <a name="abstract-classes"></a>Classes abstraites

Cela étant l’itinéraire le plus simple pour les rappels, je recommande d’utiliser `abstract` si vous essayez simplement obtenir un rappel de travail sous la forme la plus simple.

Commençons par un C# classe nous aimerions Java pour implémenter :

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

Voici les détails pour résoudre ce problème :

- `[Register]` génère un nom de package intéressant dans Java, vous obtenez un nom généré automatiquement le package sans lui.
- Sous-classement `Java.Lang.Object` signaux à l’incorporation de .NET pour exécuter la classe de générateur de Java de Xamarin.Android.
- Constructeur vide : est ce que vous souhaitez utiliser à partir de code Java.
- `(IntPtr, JniHandleOwnership)` constructeur : Xamarin.Android l’utiliserez pour créer le C#-équivalent d’objets Java.
- `[Export]` signaux Xamarin.Android pour exposer la méthode à Java. Nous pouvons également modifier le nom de la méthode, étant donné que le monde de Java aime à utiliser les méthodes de minuscules.

Suivant nous allons apporter une C# méthode pour tester le scénario :

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` peut être n’importe quelle classe pour tester cela, à condition qu’il soit un `Java.Lang.Object`.

Exécutez maintenant l’incorporation de .NET sur votre assembly .NET pour générer un AAR. Consultez le [guide Mise en route](~/tools/dotnet-embedding/get-started/java/android.md) pour plus d’informations.

Après avoir importé le fichier AAR dans Android Studio, nous allons écrire un test unitaire :

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
Par conséquent, nous avons :

- Implémenté le `AbstractClass` dans Java avec un type anonyme
- Assurez-vous que notre instance retourne `"Java"` à partir de Java
- Assurez-vous que notre instance retourne `"Java"` à partir deC#
- Ajouté `throws Throwable`, étant donné que C# constructeurs sont actuellement marquées avec `throws`

Si nous avons exécuté ce test unitaire en tant que-est, elle échoue avec une erreur tels que :

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Que se passe-t-il ici est un `Invoker` type. Il s’agit d’une sous-classe de `AbstractClass` qui transfère C# appels à Java. Si un objet Java entre dans le C# world et leur équivalent C# type est abstrait, puis Xamarin.Android recherche automatiquement un C# type avec le suffixe `Invoker` pour une utilisation dans C# code.

Xamarin.Android utilise cette `Invoker` modèle pour les projets de liaison Java entre autres choses.

Voici notre implémentation de `AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Il est tout à fait un peu ici, nous avons :

- Ajout d’une classe avec le suffixe `Invoker` qui sous-classe `AbstractClass`
- Ajouté `class_ref` contenant la référence JNI à la classe Java qui sous-classe notre C# classe
- Ajouté `id_gettext` contenant la référence JNI à Java `getText` (méthode)
- Inclus un `(IntPtr, JniHandleOwnership)` constructeur
- Implémenté `ThresholdType` et `ThresholdClass` comme une exigence pour Xamarin.Android en savoir plus sur la `Invoker`
- `GetText` nécessaire pour rechercher le Java `getText` méthode avec la signature JNI appropriée et appelez-le
- `Dispose` est uniquement nécessaire pour effacer la référence à `class_ref`

Après avoir ajouté cette classe et générer un nouveau AAR, notre test unitaire est réussi. Comme vous pouvez le voir ce modèle pour les rappels n’est pas *idéale*, mais réalisable.

Pour plus d’informations sur l’interopérabilité de Java, consultez l’innovation avec [Xamarin.Android documentation](~/android/platform/java-integration/working-with-jni.md) sur ce sujet.

## <a name="interfaces"></a>Interfaces

Les interfaces sont similaires en tant que classes abstraites, à l’exception d’un détail : Xamarin.Android ne génère pas de Java pour eux. Il s’agit, car avant l’incorporation de .NET, il n’existe pas de nombreux scénarios où Java implémenterait un C# interface.

Supposons que nous avons ce qui suit C# interface :

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` signale à l’incorporation de .NET qu’il s’agit d’une interface de Xamarin.Android, mais sinon c’est exactement identique à un `abstract` classe.

Étant donné que Xamarin.Android ne générera pas actuellement le code Java pour cette interface, ajoutez Java suivant à votre C# projet :

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

Vous pouvez placer le fichier de n’importe où, mais veillez à définir son action de génération `AndroidJavaSource`. Il signale l’incorporation de .NET pour le copier dans le répertoire approprié à sont compilés dans votre fichier AAR.

Ensuite, le `Invoker` implémentation sera tout à fait identique :

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Après avoir généré un fichier AAR, dans Android Studio, nous pourrions écrire l’unité de transmission suivantes de test :

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>Méthodes virtuelles

Remplacer un `virtual` dans Java est possible, mais pas une expérience optimale.

Supposons que vous disposez des éléments suivants C# classe :

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

Si vous avez suivi le `abstract` exemple de classe ci-dessus, elle s’exécutera à l’exception d’un détail : _Xamarin.Android ne pas rechercher les `Invoker`_ .

Pour résoudre ce problème, modifiez le C# classe soit `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Ce n’est pas idéale, mais il obtient ce travail de scénario. Xamarin.Android sélectionnera le `VirtualClassInvoker` et Java peut utiliser `@Override` sur la méthode.

## <a name="callbacks-in-the-future"></a>Rappels à l’avenir

Il existe quelques aspects nous n’a pu pour améliorer ces scénarios :

1. `throws Throwable` sur C# constructeurs est fixe sur ce [PR](https://github.com/xamarin/java.interop/pull/170).
1. Rendre le Générateur de Java dans Xamarin.Android prend en charge les interfaces.
    - Ceci élimine la nécessité pour l’ajout de fichier source Java avec une action de génération `AndroidJavaSource`.
1. Rendre un moyen pour Xamarin.Android charger un `Invoker` pour les classes virtuelles.
    - Ceci élimine la nécessité pour marquer la classe dans notre `virtual` exemple `abstract`.
1. Générer `Invoker` automatiquement des classes pour l’incorporation de .NET
    - Cela va être compliqué, mais réalisable. Xamarin.Android est déjà faire quelque chose similaire à ceci pour les projets de liaison Java.

Il y a beaucoup de travail à faire ici, mais ces améliorations à l’incorporation de .NET sont possibles.

## <a name="further-reading"></a>informations supplémentaires

* [Mise en route sur Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Recherche Android préliminaire](~/tools/dotnet-embedding/android/index.md)
* [Limitations de l’incorporation de .NET](~/tools/dotnet-embedding/limitations.md)
* [Contribution au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
