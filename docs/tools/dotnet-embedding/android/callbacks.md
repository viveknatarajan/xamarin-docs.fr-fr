---
title: Rappels sur Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 72786ac4bceca2635747ebcc844a98b0ce60383f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="callbacks-on-android"></a>Rappels sur Android

L’appel de Java à partir de c# est un peu une *risqué*. C'est-à-dire qu’il existe un *modèle* pour les rappels à partir de c#, Java ; Toutefois, il est plus complexe que nous souhaitons.

Nous aborderons les trois options permettant d’effectuer des rappels la plus pertinente pour Java :

- Classes abstraites
- Interfaces
- Méthodes virtuelles

## <a name="abstract-classes"></a>Classes abstraites

Il s’agit du chemin le plus facile pour les rappels, je recommande d’utiliser `abstract` si vous voulez simplement obtenir un rappel dans la forme la plus simple.

Commençons par une classe c# que nous aimerions Java à implémenter :

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

- `[Register]` génère un nom de package intéressante dans Java, vous obtenez un nom généré automatiquement le package sans lui.
- Sous-classement `Java.Lang.Object` Embeddinator pour exécuter la classe de générateur de Java de Xamarin.Android par le biais de signaux.
- Constructeur vide : est ce que vous souhaitez utiliser à partir de code Java.
- `(IntPtr, JniHandleOwnership)` constructeur : quel Xamarin.Android utilisera pour créer le c#-équivalent d’objets Java.
- `[Export]` signaux Xamarin.Android pour exposer la méthode pour Java. Nous pouvons modifier le nom de la méthode, étant donné que le monde Java nécessaire d’utiliser les méthodes de minuscules.

Suivant nous allons en créer une méthode c# pour le scénario de test :

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
`JavaCallbacks` peut être n’importe quelle classe de test, à condition qu’il soit un `Java.Lang.Object`.

Exécutez maintenant Embeddinator sur votre assembly .NET pour générer un AAR. Consultez le [guide Mise en route](~/tools/dotnet-embedding/get-started/java/android.md) pour plus d’informations.

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
- Assurez-vous que notre instance retourne `"Java"` à partir de c#
- Ajouté `throws Throwable`, car les constructeurs c# sont actuellement marquées avec `throws`

Si nous avons ce test unitaire en tant que-est, elle échoue avec une erreur tels que :

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Que manque-t-il ici est un `Invoker` type. Il s’agit d’une sous-classe de `AbstractClass` qui transfère les appels c# pour Java. Si un objet de Java entre dans le monde c# et le type c# équivalent est abstrait, Xamarin.Android recherche automatiquement pour un type c# avec le suffixe `Invoker` pour une utilisation dans le code c#.

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

Il existe quelques passe ici, nous avons :

- Ajouter une classe avec le suffixe `Invoker` qui sous-classe `AbstractClass`
- Ajouté `class_ref` pour contenir le JNI font référence à la classe Java qui sous-classe notre classe c#
- Ajouté `id_gettext` contenant la référence JNI à Java `getText` (méthode)
- Inclure un `(IntPtr, JniHandleOwnership)` constructeur
- Implémenté `ThresholdType` et `ThresholdClass` comme une exigence pour Xamarin.Android en savoir plus sur la `Invoker`
- `GetText` nécessaire pour rechercher le Java `getText` méthode avec la signature appropriée JNI et appelez-le
- `Dispose` est uniquement nécessaire pour supprimer la référence à `class_ref`

Après l’ajout de cette classe et de générer un nouveau AAR, notre unité test passes. Comme vous pouvez voir ce modèle pour les rappels n’est pas *idéale*, mais réalisable.

Pour plus d’informations sur l’interopérabilité de Java, consultez l’incroyable [Xamarin.Android documentation](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/working_with_jni/) à ce sujet.

## <a name="interfaces"></a>Interfaces

Les interfaces sont à peu près comme les classes abstraites, à l’exception d’un détail : Xamarin.Android ne génère pas de Java pour eux. Il s’agit, car avant l’incorporation de .NET, il n’existe pas beaucoup de scénarios où Java implémente une interface c#.

Supposons que nous avons l’interface c# suivante :

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```
`IJavaObject` signale qu’il s’agit d’une interface de Xamarin.Android, mais sinon c’est exactement identique à Embeddinator un `abstract` classe.

Étant donné que Xamarin.Android ne générera pas actuellement le code Java pour cette interface, ajoutez Java suivant à votre projet c# :

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```
Vous pouvez placer le fichier n’importe où, mais veillez à définir son action de génération `AndroidJavaSource`. Cela signale Embeddinator pour le copier dans le répertoire approprié à sont compilés dans votre fichier AAR.

Ensuite, le `Invoker` implémentation seront identiques :

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

Après avoir généré un fichier AAR, dans Android Studio, nous pouvons écrire l’unité de transmission suivantes de test :

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

Substitution un `virtual` dans Java est possible, mais pas une expérience optimale.

Supposons que vous disposez de la classe c# suivante :

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

Si vous avez suivi la `abstract` exemple de classe ci-dessus, il fonctionne à l’exception d’un détail : _Xamarin.Android ne recherche le `Invoker`_ .

Pour résoudre ce problème, modifiez la classe c# pour être `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Cela n’est pas idéale, mais il obtient ce travail de scénario. Xamarin.Android prennent en charge la `VirtualClassInvoker` et Java peut utiliser `@Override` sur la méthode.

## <a name="callbacks-in-the-future"></a>Rappels à l’avenir

Il existe deux éléments nous n’a pu pour améliorer ces scénarios :

1. `throws Throwable` sur le langage c#, constructeurs est fixe sur ce [PR](https://github.com/xamarin/java.interop/pull/170).
1. Rendre le Générateur de Java dans Xamarin.Android prend en charge les interfaces.
    - Cela supprime la nécessité pour l’ajout du fichier source Java avec une action de génération `AndroidJavaSource`.
1. Rendre un moyen de Xamarin.Android charger un `Invoker` pour les classes virtuelles.
    - Cela supprime la nécessité pour marquer la classe dans notre `virtual` exemple `abstract`.
1. Générer `Invoker` automatiquement des classes pour Embeddinator
    - Cela va être complexe, mais réalisable. Xamarin.Android déjà fait quelque chose de similaire à ce pour les projets de liaison de Java.

Il y a beaucoup de travail à effectuer ici, mais ces améliorations Embeddinator sont possibles.

## <a name="further-reading"></a>informations supplémentaires

* [Mise en route sur Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Recherche Android préliminaire](~/tools/dotnet-embedding/android/index.md)
* [Limitations de Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Contribue au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)
