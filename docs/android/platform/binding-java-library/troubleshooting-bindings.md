---
title: Résolution des problèmes de liaisons
description: Cet article présente plusieurs erreurs courantes qui peuvent se produire lors de la génération des liaisons, ainsi que les causes et les solutions suggérées pour les résoudre.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: da6286eed091114c117c723f462bbb8cac77034b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting-bindings"></a>Résolution des problèmes de liaisons

_Cet article présente plusieurs erreurs courantes qui peuvent se produire lors de la génération des liaisons, ainsi que les causes et les solutions suggérées pour les résoudre._


## <a name="overview"></a>Vue d'ensemble

Liaison d’une bibliothèque Android (un **.aar** ou un **.jar**) fichier est rarement un simple aléatoire ; il nécessite généralement un effort supplémentaire pour atténuer les problèmes qui résultent de différences entre Java et .NET.
Ces problèmes empêchera Xamarin.Android à partir de la bibliothèque Android de liaison et se présente comme messages d’erreur dans le journal de génération. Ce guide fournissent des conseils pour résoudre les problèmes, répertorie certains des scénarios plus courants des problèmes / et fournissent des solutions possibles à la liaison avec succès de la bibliothèque Android.

Lors de la liaison d’une bibliothèque Android existante, il est nécessaire à prendre en compte les points suivants :

- **Les dépendances externes pour la bibliothèque** &ndash; dépendances Any Java requises par la bibliothèque Android doivent être inclus dans le projet Xamarin.Android, comme un **ReferenceJar** ou comme une  **EmbeddedReferenceJar**.

- **Le niveau de l’API Android que la bibliothèque Android est ciblé** &ndash; il n’est pas possible de niveau de l’API Android de « rétrograder » ; Assurez-vous que le projet de liaison Xamarin.Android cible la même API niveau (ou version ultérieure) en tant que la bibliothèque Android.

- **La version du JDK Android qui a été utilisé pour créer un package la bibliothèque Android** &ndash; erreurs de liaison peuvent se produire si la bibliothèque Android a été créée avec une autre version du JDK que celui en cours d’utilisation par Xamarin.Android. Si possible, recompilez la bibliothèque Android à l’aide de la même version du JDK utilisé par votre installation de Xamarin.Android.

La première étape de résolution des problèmes avec la liaison d’une bibliothèque de Xamarin.Android consiste à activer [diagnostic de sortie MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Après l’activation de la sortie de diagnostic, régénérez le projet de liaison Xamarin.Android et examinez le journal de génération pour rechercher des indices sur la cause du problème.

Il peut également s’avérer utile de décompiler la bibliothèque Android et examiner les types et méthodes Xamarin.Android essaie de lier. Ce sujet est abordé plus en détail plus loin dans ce guide.


## <a name="decompiling-an-android-library"></a>Décompiler une bibliothèque Android

Inspecter les classes et méthodes des classes Java peut fournir des informations précieuses qui va vous aider à une bibliothèque de liaison.
[Interface utilisateur graphique de JD](http://jd.benow.ca/) est un utilitaire graphique qui peut afficher le code source Java du **classe** fichiers contenus dans un fichier JAR. Il peut être exécuté comme une application autonome ou comme un plug-in IntelliJ ou Eclipse.

Décompiler open bibliothèque Android le **. JAR** fichier avec le décompilateur Java. Si la bibliothèque est un **. AAR** fichier, il est nécessaire extraire le fichier **classes.jar** à partir du fichier d’archive. Voici une capture d’écran de l’exemple de l’utilisation d’interface utilisateur graphique de JD pour analyser les [Picasso](http://square.github.io/picasso/) JAR :

![À l’aide de la décompilateur Java pour analyser picasso-2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Une fois que vous avez décompilé la bibliothèque Android, examinez le code source. En règle générale, recherchez :

- **Les classes qui ont des caractéristiques de brouillage** &ndash; caractéristiques des classes obscurcis incluent :

    - Le nom de classe inclut un **$**, c'est-à-dire **un .class $**
    - Le nom de classe est entièrement compromis de caractères minuscules, c'est-à-dire **a.class**      

- **`import` instructions pour les bibliothèques non référencés** &ndash; identifier la bibliothèque non référencée et ajouter ces dépendances pour le projet de liaison de Xamarin.Android avec un **Action de génération** de **ReferenceJar**  ou **EmbedddedReferenceJar**.

> [!NOTE]
> Décompiler une bibliothèque Java est interdit ou soumis aux restrictions juridiques en fonction des lois ou la licence sous lequel la bibliothèque de Java a été publiée. Si nécessaire, intégrer les services d’un professionnel du juridique avant de tenter de décompiler une bibliothèque Java et inspecter le code source.


## <a name="inspect-apixml"></a>Inspectez les API. XML

Dans le cadre de la création d’un projet de liaison, Xamarin.Android génère un nom de fichier XML **obj/Debug/api.xml**:

![Api.xml généré sous obj/Debug](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Ce fichier fournit une liste de toutes les API Java que Xamarin.Android est la tentative de liaison. Le contenu de ce fichier peut aider à identifier les types ou les méthodes manquant, la liaison en double. Bien que l’inspection de ce fichier est long et fastidieux, elle peut fournir des indices sur ce qui pourrait provoquer des problèmes de liaison. Par exemple, **api.xml** peut révéler qu’une propriété retourne un type inapproprié, ou qu’il existe deux types qui partagent le même nom managé.


## <a name="known-issues"></a>Problèmes connus

Cette section répertorie certains des problèmes ou des messages d’erreur courants que mon se produisent lorsque vous tentez de lier une bibliothèque Android.


### <a name="problem-java-version-mismatch"></a>Problème : Incompatibilité de Version Java

Parfois, les types ne seront pas générés ou des pannes inattendues peuvent se produire, car vous utilisez une version plus récente ou plus anciennes de Java par rapport à la bibliothèque qui a été compilée avec. Recompilez la bibliothèque Android avec la même version du JDK qui utilise votre projet Xamarin.Android.


### <a name="problem-at-least-one-java-library-is-required"></a>Problème : au moins une bibliothèque de Java est requise

Vous recevez l’erreur « au moins une bibliothèque de Java est requise, » même si un. JAR a été ajouté.

#### <a name="possible-causes"></a>Causes possibles :

Assurez-vous que l’action de génération est définie `EmbeddedJar`. Dans la mesure où il existe plusieurs actions de génération pour. Fichiers JAR (tel que `InputJar`, `EmbeddedJar`, `ReferenceJar` et `EmbeddedReferenceJar`), le Générateur de liaison ne peut pas deviner automatiquement l’application à utiliser par défaut. Pour plus d’informations sur les actions de génération, consultez [générer les Actions](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Problème : Outils de liaison ne peut pas charger le. Bibliothèque JAR

Le Générateur de bibliothèque de liaison ne peut pas charger le. Bibliothèque JAR.

#### <a name="possible-causes"></a>Causes possibles

Certains. Impossible de charger les bibliothèques JAR qui utilisent l’obscurcissement de code (via des outils tels que Proguard) par les outils Java. Étant donné que notre outil rend l’utilisation de la réflexion de Java et le code d’octet ASM bibliothèque d’ingénierie, ces outils dépendants peuvent rejeter les bibliothèques obscurcis tandis que les outils Android runtime peuvent passer. La solution consiste à lier manuellement ces bibliothèques au lieu d’utiliser le Générateur de liaison.



### <a name="problem-missing-c-types-in-generated-output"></a>Problème : Pas de types c# dans la sortie générée.

La liaison **.dll** builds manque, mais certains types Java, ou la source c# générée ne génère pas en raison d’une erreur indiquant les types manquant.

#### <a name="possible-causes"></a>Causes possibles :

Cette erreur peut se produire en raison de plusieurs raisons répertoriées ci-dessous :

-   La bibliothèque liée peut faire référence à une deuxième bibliothèque Java. Si l’API publique de la bibliothèque liée utilise des types à partir de la deuxième bibliothèque, vous devez référencer une liaison managée pour la bibliothèque de seconde.

-   Il est possible qu’une bibliothèque a été générée en raison d’une réflexion Java, similaire à la raison de l’erreur de chargement de bibliothèque ci-dessus, à l’origine de l’inattendue lors du chargement des métadonnées. Les outils de Xamarin.Android Impossible actuellement de résoudre cette situation. Dans ce cas, la bibliothèque doit être liée manuellement.

-   Il a été un bogue dans .NET 4.0 runtime qui n’a pas pu charger les assemblys lorsqu’elle doit avoir. Ce problème a été résolu dans le runtime .NET 4.5.

-   Java permet de dériver une classe publique de la classe non publique, mais cela non pris en charge dans .NET. Étant donné que le Générateur de liaison ne génère pas de liaisons pour les classes non publiques, les classes dérivées telles que celles-ci ne peut pas être générés correctement. Pour résoudre ce problème, supprimez l’entrée de métadonnées pour les classes dérivées à l’aide du nœud de suppression dans **Metadata.xml**, ou corrigez les métadonnées de la classe non publique public. Bien que la deuxième solution créez la liaison afin que la source c# générerez, il se peut que la classe non publics ne doit pas utilisée.

    Par exemple :

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Les outils qui masquent les bibliothèques Java peuvent interférer avec le Générateur de liaison de Xamarin.Android et sa capacité à générer des classes wrapper c#. L’extrait de code suivant montre comment mettre à jour **Metadata.xml** à unobfuscate un nom de classe :

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Problème : La source c# ne génère pas en raison d’une incompatibilité de type de paramètre

Ne génère pas de la source c# générée. Substitution de paramètre de la méthode types ne correspondent pas.

#### <a name="possible-causes"></a>Causes possibles :

Xamarin.Android inclut divers champs Java qui sont mappés aux énumérations dans les liaisons de c#. Ces peuvent entraîner des incompatibilités de type dans les liaisons générés. Pour résoudre ce problème, les signatures de méthode créés à partir du Générateur de liaison doivent être modifiées pour utiliser les énumérations. Pour plus d’imformation, consultez [Enums de correction](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Problème : Les NoClassDefFoundError dans un package

`java.lang.NoClassDefFoundError` est levée lors de l’étape d’empaquetage.

#### <a name="possible-causes"></a>Causes possibles :

La raison la plus probable de cette erreur est qu’une bibliothèque Java obligatoire doit être ajouté au projet d’application (**.csproj**). . Fichiers JAR ne sont pas résolues automatiquement. Une liaison de la bibliothèque Java n’est pas toujours générée par rapport à un assembly d’utilisateur qui n’existe pas dans l’appareil cible ou l’émulateur (tels que Google Maps **maps.jar**). Cela n’est pas le cas pour la prise en charge du projet de bibliothèque Android, que la bibliothèque. JAR est incorporée dans la dll de bibliothèque. Par exemple : [bogue 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Problème : Dupliquer des types personnalisés EventArgs

Échec de la build en raison de types d’EventArgs personnalisés en double. Ce type d’erreur se produit :

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Causes possibles :

Il s’agit, car il existe des conflits entre les types d’événements provenant de plusieurs types de « écouteur » d’interface qui partage des méthodes ayant des noms identiques. Par exemple, s’il existe deux interfaces Java comme indiqué dans l’exemple ci-dessous, le générateur crée `DismissScreenEventArgs` pour les deux `MediationBannerListener` et `MediationInterstitialListener`, se traduisant par l’erreur.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Ceci est volontaire afin que les noms longs sur les types d’arguments événements sont évités. Pour éviter ces conflits, une transformation de métadonnées est requise. Modifier [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) et ajoutez un `argsType` attribut sur une des interfaces (ou sur la méthode d’interface) :

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

### <a name="problem-class-does-not-implement-interface-method"></a>Problème : La classe n’implémente pas de méthode d’interface

Un message d’erreur est généré indiquant qu’une classe générée n’implémente pas une méthode qui est requise pour une interface qui met en œuvre la classe générée. Toutefois, le code généré, vous pouvez voir que la méthode est implémentée.

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

Il s’agit d’un problème qui se produit avec les méthodes Java de liaison avec des types de retour covariants. Dans cet exemple, la méthode `Oauth.Signpost.Http.IHttpRequest.UnWrap()` doit retourner `Java.Lang.Object`. Toutefois, la méthode `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` a un type de retour de `HttpURLConnection`. Il existe deux façons de résoudre ce problème :

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

-   Supprimez la covariance du code c# généré. Cela implique l’ajout de la transformation suivante à **Transforms\Metadata.xml** ce qui entraîne le code c# généré à avoir un type de retour de `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Problème : Les Collisions sur les Classes internes de nom / propriétés

Visibilité en conflit sur les objets hérités.

En Java, il exige pas qu’une classe dérivée ont la même visibilité que son parent. Java qui réparera juste pour vous. En c#, qui doit être explicite, vous devez vous assurer toutes les classes de la hiérarchie ont la visibilité appropriée. L’exemple suivant montre comment modifier un nom de package Java à partir de `com.evernote.android.job` à `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Problème : Un **.so** bibliothèque requis par la liaison est n’est pas chargé

Certains projets de liaison peuvent également dépendre de fonctionnalités dans un **.so** bibliothèque. Il est possible que Xamarin.Android se chargeront pas automatiquement le **.so** bibliothèque. Lors de l’exécution, le code Java encapsulé Xamarin.Android ne pourront pas effectuer l’appel JNI et le message d’erreur _java.lang.UnsatisfiedLinkError : méthode Native introuvable :_ s’affiche dans le logcat out pour l’application.

La solution à ce problème consiste à charger manuellement le **.so** bibliothèque avec un appel à `Java.Lang.JavaSystem.LoadLibrary`. Par exemple, en supposant qu’un projet Xamarin.Android a partagé bibliothèque **libpocketsphinx_jni.so** inclus dans le projet de liaison avec une action de génération **EmbeddedNativeLibrary**, ce qui suit extrait de code (exécutée avant d’utiliser la bibliothèque partagée) chargera le **.so** bibliothèque :

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Récapitulatif

Dans cet article, nous répertoriés de résolution des problèmes courants associés aux liaisons de Java et explique comment les résoudre.


## <a name="related-links"></a>Liens associés

- [Projets de bibliothèque](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Activer la sortie de Diagnostic](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin pour les développeurs Android](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
