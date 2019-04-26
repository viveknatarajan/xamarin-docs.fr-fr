---
title: Métadonnées de liaisons Java
description: C#code de Xamarin.Android appelle les bibliothèques Java par le biais des liaisons qui constituent un mécanisme qui résume les détails de bas niveau qui sont spécifiés dans Interface JNI (Java Native). Xamarin.Android fournit un outil qui génère ces liaisons. Ces outils permet le contrôle du développeur comment une liaison est créée à l’aide de métadonnées, qui permet aux procédures telles que la modification des espaces de noms et attribution des membres. Ce document décrit le fonctionnement de métadonnées, résume les attributs que les métadonnées prend en charge et explique comment résoudre les problèmes de liaison en modifiant ces métadonnées.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 06a7a3b00934b7a2f3eeb4fcfa6fc90071901ba0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60955661"
---
# <a name="java-bindings-metadata"></a>Métadonnées de liaisons Java

_C#code de Xamarin.Android appelle les bibliothèques Java par le biais des liaisons qui constituent un mécanisme qui résume les détails de bas niveau qui sont spécifiés dans Interface JNI (Java Native). Xamarin.Android fournit un outil qui génère ces liaisons. Ces outils permet le contrôle du développeur comment une liaison est créée à l’aide de métadonnées, qui permet aux procédures telles que la modification des espaces de noms et attribution des membres. Ce document décrit le fonctionnement de métadonnées, résume les attributs que les métadonnées prend en charge et explique comment résoudre les problèmes de liaison en modifiant ces métadonnées._


## <a name="overview"></a>Vue d'ensemble

Une application Xamarin.Android **bibliothèque de liaison Java** tente d’automatiser une grande partie du travail nécessaire pour la liaison d’une bibliothèque Android existante à l’aide d’un outil connu sous le _Générateur de liaisons_. Lors de la liaison d’une bibliothèque Java, Xamarin.Android sera inspecter les classes Java et générer une liste de tous les packages, les types et les membres qui à lier. Cette liste d’API est stockée dans un fichier XML qui se trouve à  **\{projet directory}\obj\Release\api.xml** pour un **version** générer et à  **\{projet Directory}\obj\Debug\api.XML** pour un **déboguer** générer.

![Emplacement du fichier dans le dossier obj/Debug api.xml](java-bindings-metadata-images/java-bindings-metadata-01.png)

Le Générateur de liaisons utilisera le **api.xml** fichier comme indication pour la génération nécessaires C# classes wrapper. Le contenu de ce fichier XML est une variante de Google _Android Open Source Project_ format.
L’extrait de code suivant est un exemple du contenu de **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

Dans cet exemple, **api.xml** déclare une classe dans le `android` package nommé `Manifest` qui étend la `java.lang.Object`.

Dans de nombreux cas, l’assistance humaine est nécessaire pour rendre l’API Java plus « .NET tels que » l’impression ou pour corriger les problèmes qui empêchent l’assembly de liaison à partir de la compilation. Par exemple, il peut être nécessaire de modifier les noms de package Java aux espaces de noms .NET, de renommer une classe ou de modifier le type de retour d’une méthode.

Ces modifications ne sont pas atteints en modifiant **api.xml** directement.
Au lieu de cela, les modifications sont enregistrées dans des fichiers XML spéciaux qui sont fournis par le modèle de bibliothèque de liaison de Java. Quand vous compilez l’assembly de liaison Xamarin.Android, le Générateur de liaisons sont tributaires de ces fichiers de mappage lors de la création de l’assembly de liaison

Ces fichiers de mappage XML peuvent se trouver dans le **transforme** dossier du projet :

-   **Fichier MetaData.xml** &ndash; permet des modifications à apporter à l’API finale, telles que la modification de l’espace de noms de la liaison générée. 

-   **EnumFields.xml** &ndash; contient le mappage entre Java `int` constantes et C# `enums` . 

-   **EnumMethods.xml** &ndash; permet de changer les paramètres de méthode et les types de retour à partir de Java `int` constantes à C# `enums` . 

Le **MetaData.xml** fichier est le meilleur parti d’importation de ces fichiers car elle permet des modifications à usage général à la liaison telles que :

-   Renommer les espaces de noms, classes, méthodes ou champs afin qu’ils suivent les conventions de .NET. 

-   Suppression des espaces de noms, classes, méthodes ou champs qui ne sont pas nécessaires. 

-   Déplacement de classes à différents espaces de noms. 

-   Ajout de classes de prise en charge supplémentaire pour rendre la conception de la liaison suivre des modèles de .NET framework. 

Vous permet de passer à discuter **Metadata.xml** plus en détail.


## <a name="metadataxml-transform-file"></a>Fichier de transformation de fichier Metadata.Xml

Comme nous l’avons déjà appris, le fichier **Metadata.xml** est utilisée par le Générateur de liaisons pour influencer la création de l’assembly de liaison.
Le format de métadonnées utilise [XPath](https://www.w3.org/TR/xpath/) syntaxe et est presque identique à la *GAPI métadonnées* décrit dans [GAPI métadonnées](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) guide. Cette implémentation est presque une implémentation complète de XPath 1.0 et prend donc en charge les éléments dans la norme 1.0. Ce fichier est un puissant mécanisme de XPath en fonction de modifier, ajouter, masquer ou déplacer tout élément ou attribut dans le fichier de l’API. Tous les éléments de règle dans la spécification de métadonnées incluent un attribut de chemin d’accès pour identifier le nœud auquel la règle doit être appliquée. Les règles sont appliquées dans l’ordre suivant :

* **Ajout de nœud** &ndash; ajoute un nœud enfant au nœud spécifié par l’attribut de chemin d’accès.
* **attr** &ndash; définit la valeur d’un attribut de l’élément spécifié par l’attribut de chemin d’accès.
* **Remove-nœud** &ndash; supprime les nœuds correspondant à un XPath spécifié.

Voici un exemple d’un **Metadata.xml** fichier :

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

La liste suivante décrit certaines des éléments XPath plus couramment utilisés pour l’API Java :

-   `interface` &ndash; Utilisé pour localiser une interface Java. par exemple, `/interface[@name='AuthListener']`.

-   `class` &ndash; Utilisé pour localiser une classe. par exemple, `/class[@name='MapView']`.

-   `method` &ndash; Utilisé pour localiser une méthode sur une classe Java ou d’une interface. par exemple, `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Identifier un paramètre pour une méthode. Par exemple `/parameter[@name='p0']`



### <a name="adding-types"></a>Ajout de Types

Le `add-node` élément indiquera le projet de liaison Xamarin.Android pour ajouter une nouvelle classe de wrapper pour **api.xml**. Par exemple, l’extrait suivant dirigera le Générateur de liaison pour créer une classe avec un constructeur et un seul champ :

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Suppression de Types

Il est possible d’indiquer le Générateur de liaisons de Xamarin.Android pour ignorer un type Java et les lier pas. Cela s’effectue en ajoutant un `remove-node` élément XML à le **metadata.xml** fichier :

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Modification du nom des membres

Membres de changement de nom ne peut pas être effectuée en modifiant directement le **api.xml** fichier car Xamarin.Android nécessite les noms d’Interface JNI (Java Native) d’origine. Par conséquent, le `//class/@name` attribut ne peut pas être modifié ; dans le cas, la liaison ne fonctionnera pas.

Prenons le cas où nous voulons pour renommer un type, `android.Manifest`.
Pour ce faire, nous pourrions essayer de modifier directement **api.xml** et renommez la classe comme suit :

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Ainsi, le Générateur de liaisons de création de ce qui suit C# code pour la classe wrapper :

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Notez que la classe wrapper a été renommée en `NewName`, tandis que le type Java d’origine est toujours `Manifest`. Il n’est plus possible pour la classe de liaison Xamarin.Android pour accéder à toutes les méthodes dans `android.Manifest`; la classe wrapper est liée à un type de Java inexistantes.

Pour modifier correctement le nom managé d’un type encapsulé (ou la méthode), il est nécessaire de définir le `managedName` attribut comme illustré dans cet exemple :

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Changement de nom `EventArg` Classes Wrapper

Lorsque le Générateur de liaison Xamarin.Android identifie un `onXXX` méthode setter pour un _type d’écouteur_, un C# événement et `EventArgs` sous-classe sera générée pour prendre en charge .NET préparés à l’API pour le-basé sur Java modèle de l’écouteur. Par exemple, considérez la classe Java suivante et la méthode :

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android supprimera le préfixe `on` à partir de la méthode setter et à la place utiliser `2DSignNextManuever` comme base pour le nom de la `EventArgs` sous-classe. La sous-classe sera nommée quelque chose de similaire à :

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Ce n’est pas un juridiques C# nom de la classe. Pour corriger ce problème, l’auteur de la liaison doit utiliser le `argsType` d’attribut et fournir une valide C# nom pour le `EventArgs` sous-classe :
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Attributs pris en charge

Les sections suivantes décrivent certains des attributs pour la transformation des API Java.

### <a name="argstype"></a>argsType

Cet attribut est placé sur les méthodes setter pour nommer le `EventArg` sous-classe qui doit être généré pour prendre en charge les écouteurs de Java. Cette opération est décrite plus en détail ci-dessous dans la section [renommer des Classes Wrapper EventArg](#Renaming_EventArg_Wrapper_Classes) plus loin dans ce guide.

### <a name="eventname"></a>eventName

Spécifie un nom pour un événement. Si elle est vide, elle empêche la génération d’événements.
Cette opération est décrite plus en détail dans le titre de section [renommer des Classes Wrapper EventArg](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Cela permet de modifier le nom d’un package, classe, méthode ou du paramètre. Par exemple, pour modifier le nom de la classe Java `MyClass` à `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

L’exemple suivant illustre une expression XPath pour la modification du nom de la méthode `java.lang.object.toString` à `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` permet de modifier le type de retour d’une méthode. Dans certaines situations le Générateur de liaisons déduira correctement le type de retour d’une méthode Java, ce qui entraîne une erreur de compilation. Une solution possible dans cette situation consiste à modifier le type de retour de la méthode.

Par exemple, le Générateur de liaisons estime que la méthode Java `de.neom.neoreadersdk.resolution.compareTo()` doit retourner un `int`, ce qui entraîne le message d’erreur **erreur CS0535 : « ALLEMAGNE. Neom.Neoreadersdk.Resolution' n’implémente pas le membre d’interface 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)'**. L’extrait de code suivant montre comment modifier le type de retour de généré C# méthode à partir d’un `int` à un `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Modifie le type de retour d’une méthode. Cela ne modifie pas l’attribut de retour (en tant que modifications pour retourner les attributs peuvent entraîner des changements incompatibles à la signature JNI). Dans l’exemple suivant, le type de retour de la `append` méthode est passée de `SpannableStringBuilder` à `IAppendable` (n’oubliez pas que C# ne prend pas en charge les types de retour covariants) :

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>masqués

Les outils qui masquent les bibliothèques Java peuvent interférer avec le Générateur de liaison de Xamarin.Android et sa capacité à générer C# classes wrapper. Caractéristiques des classes obscurcis sont les suivantes : 

* Le nom de classe inclut un **$**, c'est-à-dire **un .class $**
* Le nom de classe est entièrement compromis entre les caractères minuscules, par exemple, **a.class**

Cet extrait de code est un exemple montrant comment générer un « non obscurci » C# type :

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Cet attribut peut être utilisé pour modifier le nom d’une propriété gérée.

Un cas spécialisé à l’aide de `propertyName` implique la situation où une classe Java a uniquement une méthode d’accesseur Get pour un champ. Dans ce cas, le Générateur de liaison souhaiteriez créer une propriété en écriture seule, quelque chose qui n’est pas recommandée dans .NET. L’extrait de code suivant montre comment « supprimer » les propriétés .NET en définissant le `propertyName` sur une chaîne vide :

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Notez que les méthodes getter et setter seront toujours créées par le Générateur de liaisons.

### <a name="sender"></a>sender

Spécifie le paramètre d’une méthode doit être le `sender` paramètre lorsque la méthode est mappée à un événement. La valeur peut être `true` ou `false`. Exemple :

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>visibilité

Cet attribut est utilisé pour modifier la visibilité d’une classe, une méthode ou une propriété. Par exemple, il peut être nécessaire promouvoir un `protected` méthode Java afin qu’il le correspondantes C# wrapper est `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml et EnumMethods.xml

Il existe des cas où les bibliothèques Android utilisent les constantes entières pour représenter les États qui sont passés aux propriétés ou aux méthodes des bibliothèques. Dans de nombreux cas, il est utile lier ces constantes entières aux enums dans C#. Pour faciliter ce mappage, utilisez le **EnumFields.xml** et **EnumMethods.xml** fichiers dans votre projet de liaison. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Définition d’un Enum à l’aide de EnumFields.xml

Le **EnumFields.xml** fichier contient le mappage entre Java `int` constantes et C# `enums`. Prenons l’exemple suivant d’un C# enum en cours de création d’un ensemble de `int` constantes : 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Ici, nous avons pris la classe Java `SKRealReachSettings` et défini un C# enum appelé `SKMeasurementUnit` dans l’espace de noms `Skobbler.Ngx.Map.RealReach`. Le `field` entrées définit le nom de la constante Java (exemple `UNIT_SECOND`), le nom de l’entrée de l’enum (exemple `Second`) et la valeur entière représentée par les deux entités (exemple `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Définition des méthodes Getter/Setter à l’aide de EnumMethods.xml

Le **EnumMethods.xml** fichier permet de changer les paramètres de méthode et les types de retour à partir de Java `int` constantes à C# `enums`. En d’autres termes, il mappe la lecture et l’écriture de C# enums (défini dans le **EnumFields.xml** fichier) pour Java `int` constante `get` et `set` méthodes.

Étant donné la `SKRealReachSettings` énumération définie ci-dessus, ce qui suit **EnumMethods.xml** fichier définirait la méthode getter/setter pour cet enum :

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

La première `method` ligne mappe la valeur de retour de Java `getMeasurementUnit` méthode à la `SKMeasurementUnit` enum. La seconde `method` ligne mappe le premier paramètre de la `setMeasurementUnit` à l’énumération de même.

Avec toutes ces modifications en place, vous pouvez utiliser le code suivant dans Xamarin.Android pour définir le `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Récapitulatif

Cet article décrit comment Xamarin.Android utilise les métadonnées pour transformer une définition d’API à partir de la *Google* *format AOSP*. Après avoir examiné les modifications sont possibles à l’aide de *Metadata.xml*, il a examiné les limitations rencontrées lors de la modification du nom des membres, et il affiche la liste des attributs XML pris en charge, qui décrit quand chaque attribut doit être utilisé.



## <a name="related-links"></a>Liens associés

- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md)
- [Métadonnées GAPI](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
