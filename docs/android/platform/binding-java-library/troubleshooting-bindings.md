---
title: Résolution des problèmes de liaisons
description: Cet article résume plusieurs erreurs courantes qui peuvent se produire lors de la génération des liaisons, ainsi que les causes et les solutions suggérées pour les résoudre.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b0bb7cbb6160865af5b1e40d40c7b999a8bd5ebc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60956537"
---
# <a name="troubleshooting-bindings"></a>Résolution des problèmes de liaisons

_Cet article résume plusieurs erreurs courantes qui peuvent se produire lors de la génération des liaisons, ainsi que les causes et les solutions suggérées pour les résoudre._


## <a name="overview"></a>Vue d'ensemble

Liaison d’une bibliothèque Android (un **.aar** ou un **.jar**) le fichier est rarement une simple demande ; elle nécessite généralement des efforts supplémentaires pour atténuer les problèmes qui résultent de différences entre Java et .NET.
Ces problèmes empêchera Xamarin.Android à partir de la liaison de la bibliothèque Android et se présentent sous forme de messages d’erreur dans le journal de génération. Ce guide fournissent des conseils pour résoudre les problèmes, répertorie certains des scénarios plus courants des problèmes / et fournissent des solutions possibles à la liaison avec succès de la bibliothèque Android.

Lors de la liaison d’une bibliothèque Android existante, il est nécessaire de garder à l’esprit les points suivants :

- **Les dépendances externes pour la bibliothèque** &ndash; dépendances Any Java requises par la bibliothèque Android doivent être inclus dans le projet Xamarin.Android, comme un **ReferenceJar** ou comme un  **EmbeddedReferenceJar**.

- **Le niveau d’API Android que la bibliothèque Android est ciblé** &ndash; il n’est pas possible de « rétrograder » le niveau de l’API Android ; Assurez-vous que le projet de liaison Xamarin.Android cible la même API niveau (ou version ultérieure) en tant que la bibliothèque Android.

- **La version du JDK Android qui a été utilisé pour créer un package de la bibliothèque Android** &ndash; erreurs de liaison peuvent se produire si la bibliothèque Android a été créée avec une autre version du JDK que celui en cours d’utilisation par Xamarin.Android. Si possible, recompiler la bibliothèque Android à l’aide de la même version du JDK qui est utilisé par votre installation de Xamarin.Android.

La première étape de résolution des problèmes avec la liaison d’une bibliothèque de Xamarin.Android consiste à activer [sortie MSBuild de diagnostic](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Après l’activation de la sortie de diagnostic, régénérez le projet de liaison Xamarin.Android et examinez le journal de build pour localiser des indices sur la cause du problème.

Il peut également s’avérer utile de décompiler la bibliothèque Android et examiner les types et méthodes Xamarin.Android essaie de lier. Ce point est abordé plus en détail plus loin dans ce guide.


## <a name="decompiling-an-android-library"></a>Décompilation une bibliothèque Android

Inspection des classes et méthodes des classes Java peut fournir des informations précieuses qui vous aideront dans une bibliothèque de liaison.
[Interface utilisateur graphique de JD](http://jd.benow.ca/) est un utilitaire graphique qui peut afficher le code de source de Java à partir de la **classe** fichiers contenus dans un fichier JAR. Il peut être exécuté comme une application autonome ou comme un plug-in pour IntelliJ ou Eclipse.

À la décompiler une ouverture de la bibliothèque Android le **. JAR** fichier avec le décompilateur Java. Si la bibliothèque est un **. AAR** fichier, il est nécessaire extraire le fichier **classes.jar** à partir du fichier d’archive. Voici une capture d’écran de l’exemple de l’utilisation de JD-interface utilisateur graphique pour analyser le [Picasso](http://square.github.io/picasso/) JAR :

![À l’aide du décompilateur Java pour analyser picasso-2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Une fois que vous avez décompilé la bibliothèque Android, examinez le code source. En règle générale, recherchez :

- **Les classes qui ont des caractéristiques d’obscurcissement** &ndash; les caractéristiques des classes obscurcis comprennent :

    - Le nom de classe inclut un **$**, c'est-à-dire **un .class $**
    - Le nom de classe est entièrement compromis entre les caractères minuscules, par exemple, **a.class**      

- **`import` instructions pour les bibliothèques non référencés** &ndash; identifier la bibliothèque non référencée et ajoutez ces dépendances au projet de liaison Xamarin.Android avec un **Action de génération** de **ReferenceJar**  ou **EmbedddedReferenceJar**.

> [!NOTE]
> Décompilation une bibliothèque Java est interdit ou soumis à des restrictions légales selon les lois locales ou de la licence sous lequel la bibliothèque Java a été publiée. Si nécessaire, intégrer les services d’un professionnel juridique avant de tenter de décompiler une bibliothèque Java et inspecter le code source.


## <a name="inspect-apixml"></a>Inspecter des API. XML

Dans le cadre de la création d’un projet de liaison, Xamarin.Android génère un nom de fichier XML **obj/Debug/api.xml**:

![Api.xml généré sous obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Ce fichier fournit une liste de toutes les API Java que Xamarin.Android essaie de liaison. Le contenu de ce fichier peut aider à identifier tous les types ou méthodes manquant, la liaison en double. Bien que l’inspection de ce fichier est longue et fastidieuse, il peut fournir des indices sur ce qui peut être à l’origine des problèmes de liaison. Par exemple, **api.xml** peut révéler qu’une propriété retourne un type inapproprié ou qu’il existe deux types qui partagent le même nom managé.


## <a name="known-issues"></a>Problèmes connus

Cette section répertorie certains des problèmes ou messages d’erreur courants que mon se produisent lorsque vous tentez de lier une bibliothèque Android.


### <a name="problem-java-version-mismatch"></a>Problème : Incompatibilité de Version de Java

Parfois, les types ne seront pas générés ou des pannes inattendues peuvent se produire, car vous utilisez une version plus récente ou une version antérieure de Java par rapport à la bibliothèque qui a été compilée avec. Recompiler la bibliothèque Android avec la même version du JDK qui utilise votre projet Xamarin.Android.


### <a name="problem-at-least-one-java-library-is-required"></a>Problème : Au moins une bibliothèque Java est requise

Vous recevez l’erreur « au moins une bibliothèque Java est requise, » même si un. Fichier JAR a été ajoutée.

#### <a name="possible-causes"></a>Causes possibles :

Vérifiez que l’action de génération est définie `EmbeddedJar`. Dans la mesure où il existe plusieurs actions de génération pour. Fichiers JAR (tel que `InputJar`, `EmbeddedJar`, `ReferenceJar` et `EmbeddedReferenceJar`), le Générateur de liaison ne peuvent pas deviner automatiquement l’application à utiliser par défaut. Pour plus d’informations sur les actions de génération, consultez [générer les Actions](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problème : Outils de liaison ne peut pas charger le. Bibliothèque JAR

Le Générateur de bibliothèque de liaison ne parvient pas à charger le. Bibliothèque JAR.

#### <a name="possible-causes"></a>Causes possibles

Certains. Les bibliothèques JAR qui utilisent l’obfuscation de code (via Outils tel que Proguard) ne peut pas être chargés par les outils Java. Étant donné que notre outil rend l’utilisation de la réflexion de Java et le code d’octet ASM bibliothèque d’ingénierie, ces outils dépendants peuvent rejeter les bibliothèques obscurcis tandis que les outils Android runtime peuvent passer. Pour cela, la solution de contournement consiste à lier manuellement ces bibliothèques au lieu d’utiliser le Générateur de liaison.



### <a name="problem-missing-c-types-in-generated-output"></a>Problème : Manquant C# types dans la sortie générée.

La liaison **.dll** versions, mais certains types Java ou généré des absences dans le C# source ne génère pas en raison d’une erreur indiquant les types manquants.

#### <a name="possible-causes"></a>Causes possibles :

Cette erreur peut se produire dû à plusieurs raisons, comme indiqué ci-dessous :

-   Une deuxième bibliothèque Java peut faire référence à la bibliothèque en cours de liaison. Si l’API publique pour la bibliothèque liée utilise des types à partir de la deuxième bibliothèque, vous devez référencer une liaison managée pour la bibliothèque de deuxième.

-   Il est possible qu’une bibliothèque a été générée en raison de la réflexion Java, similaire à la raison de l’erreur de chargement de bibliothèque ci-dessus, qui provoque le chargement inattendu des métadonnées. Les outils de Xamarin.Android actuellement impossible de résoudre cette situation. Dans ce cas, la bibliothèque doit être liée manuellement.

-   Un bogue s’est produite dans le runtime .NET 4.0 qui n’a pas pu charger les assemblys quand il doit avoir. Ce problème a été résolu dans le runtime .NET 4.5.

-   Java permet de dériver une classe publique à partir de la classe non publique, mais cela non pris en charge dans .NET. Étant donné que le Générateur de liaison ne génère pas de liaisons pour les classes non publiques, les classes dérivées telles que celles-ci ne peut pas être générés correctement. Pour résoudre ce problème, supprimez l’entrée de métadonnées pour ces classes dérivées en utilisant le nœud de suppression dans **Metadata.xml**, ou corrigez les métadonnées qui effectue la classe non publique publique. Bien que la deuxième solution créera la liaison afin que le C# source va générer, de la classe non publique ne doit pas être utilisée.

    Exemple :

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Les outils qui masquent les bibliothèques Java peuvent interférer avec le Générateur de liaison de Xamarin.Android et sa capacité à générer C# classes wrapper. L’extrait de code suivant montre comment mettre à jour **Metadata.xml** à unobfuscate un nom de classe :

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problème : Généré C# source ne génère pas en raison d’une incompatibilité de type de paramètre

Le texte généré C# source ne génère pas. Substitution de paramètre de la méthode types ne correspondent pas.

#### <a name="possible-causes"></a>Causes possibles :

Xamarin.Android inclut une variété de champs de Java qui sont mappés aux enums dans le C# liaisons. Cela peut entraîner des incompatibilités de type dans les liaisons générés. Pour résoudre ce problème, les signatures de méthode créées à partir du Générateur de liaison doivent être modifiés pour utiliser les énumérations. Pour plus d’imformation, consultez [Enums correction](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problème : NoClassDefFoundError dans emballage

`java.lang.NoClassDefFoundError` est levée dans l’étape d’empaquetage.

#### <a name="possible-causes"></a>Causes possibles :

La raison la plus probable de cette erreur est qu’une bibliothèque Java obligatoire doit être ajouté au projet d’application (**.csproj**). . Fichiers JAR ne sont pas résolues automatiquement. Une liaison de la bibliothèque Java n’est pas toujours générée par rapport à un assembly d’utilisateur qui n’existe pas dans l’appareil cible ou l’émulateur (tels que Google Maps **maps.jar**). Cela n’est pas le cas pour la prise en charge du projet de bibliothèque Android, comme la bibliothèque. Fichier JAR est incorporé dans la dll de bibliothèque. Exemple : [Bogue 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problème : Types d’EventArgs personnalisés en double

Build échoue en raison de types d’EventArgs personnalisés en double. Ce type d’erreur se produit :

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Causes possibles :

Il s’agit, car il existe des conflits entre les types d’événements provenant de plusieurs types de « écouteur » d’interface qui partage les méthodes ayant des noms identiques. Par exemple, s’il existe deux interfaces Java comme indiqué dans l’exemple ci-dessous, le générateur crée `DismissScreenEventArgs` pour les deux `MediationBannerListener` et `MediationInterstitialListener`, se traduisant par l’erreur.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Il s’agit par conception afin que les noms longs sur les types d’argument événements sont évités. Pour éviter ces conflits, une transformation de métadonnées est nécessaire. Modifier [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) et ajoutez un `argsType` attribut sur une des interfaces (ou sur la méthode d’interface) :

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>Problème : Classe n’implémente pas de méthode d’interface

Un message d’erreur est généré indiquant qu’une classe générée n’implémente pas une méthode qui est requise pour une interface qui implémente la classe générée. Toutefois, en examinant le code généré, vous pouvez voir que la méthode est implémentée.

Voici un exemple de l’erreur :

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Causes possibles :

Il s’agit d’un problème qui se produit avec les méthodes Java de liaison avec les types de retour covariants. Dans cet exemple, la méthode `Oauth.Signpost.Http.IHttpRequest.UnWrap()` doit retourner `Java.Lang.Object`. Toutefois, la méthode `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` a un type de retour `HttpURLConnection`. Il existe deux façons de résoudre ce problème :

-   Ajoutez une déclaration de classe partielle pour `HttpURLConnectionRequestAdapter` et implémentez explicitement `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   Supprimez la covariance généré C# code. Cela implique l’ajout de la transformation suivante à **Transforms\Metadata.xml** ce qui entraîne le généré C# code pour avoir un type de retour `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problème : Nommez les conflits entre les Classes internes / propriétés

Visibilité en conflit sur les objets hérités.

En Java, pas obligatoire qu’une classe dérivée avoir la même visibilité que son parent. Java qui résoudra simplement pour vous. Dans C#, qui doit être explicite, donc vous devez vous assurer que toutes les classes dans la hiérarchie ont la visibilité appropriée. L’exemple suivant montre comment modifier un nom de package Java à partir de `com.evernote.android.job` à `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problème : Un **.so** bibliothèque requise par la liaison est n’est pas chargé

Certains projets de liaison peuvent également dépendre de fonctionnalités dans un **.so** bibliothèque. Il est possible que Xamarin.Android automatiquement ne chargera pas le **.so** bibliothèque. Lorsque le code Java encapsulé s’exécute, Xamarin.Android ne pourra pas passer l’appel JNI et le message d’erreur _java.lang.UnsatisfiedLinkError : Méthode native introuvable :_ apparaîtra dans le logcat out pour l’application.

La solution à ce problème consiste à charger manuellement le **.so** bibliothèque avec un appel à `Java.Lang.JavaSystem.LoadLibrary`. Par exemple, en supposant qu’un projet Xamarin.Android a partagé bibliothèque **libpocketsphinx_jni.so** inclus dans le projet de liaison avec une action de génération **EmbeddedNativeLibrary**, l’extrait de code suivant (exécuté avant d’utiliser la bibliothèque partagée) chargera le **.so** bibliothèque :

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Récapitulatif

Dans cet article, nous répertoriées de résolution des problèmes courants associés aux liaisons Java et expliqué comment les résoudre.


## <a name="related-links"></a>Liens associés

- [Projets de bibliothèque](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Activer la sortie de Diagnostic](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin pour les développeurs Android](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
