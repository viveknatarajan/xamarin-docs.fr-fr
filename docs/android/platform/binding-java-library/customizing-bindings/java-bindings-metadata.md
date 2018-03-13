---
title: "Métadonnées de liaisons de Java"
description: "Code c# dans Xamarin.Android appelle bibliothèques Java via des liaisons qui sont un mécanisme qui résume les détails de bas niveau qui sont spécifiés dans Interface JNI (Java Native). Xamarin.Android fournit un outil qui génère ces liaisons. Cet outils vous permet du contrôle du développeur comment une liaison est créée à l’aide de métadonnées, qui permet aux procédures telles que la modification des espaces de noms et la modification du nom des membres. Ce document explique le fonctionnement des métadonnées, récapitule les attributs que les métadonnées prend en charge et explique comment résoudre les problèmes de liaison en modifiant ces métadonnées."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: edf25ebd089994c01b2fa45e77b35fad9a51e350
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="java-bindings-metadata"></a>Métadonnées de liaisons de Java

_Code c# dans Xamarin.Android appelle bibliothèques Java via des liaisons qui sont un mécanisme qui résume les détails de bas niveau qui sont spécifiés dans Interface JNI (Java Native). Xamarin.Android fournit un outil qui génère ces liaisons. Cet outils vous permet du contrôle du développeur comment une liaison est créée à l’aide de métadonnées, qui permet aux procédures telles que la modification des espaces de noms et la modification du nom des membres. Ce document explique le fonctionnement des métadonnées, récapitule les attributs que les métadonnées prend en charge et explique comment résoudre les problèmes de liaison en modifiant ces métadonnées._


## <a name="overview"></a>Vue d'ensemble

Un Xamarin.Android **bibliothèque de liaison Java** tente automatise une grande partie du travail nécessaire pour la liaison d’une bibliothèque Android existante à l’aide d’un outil, parfois appelé le _Générateur de liaisons_. Lors de la liaison d’une bibliothèque Java, Xamarin.Android sera inspecter les classes Java et générer une liste de tous les packages, les types et les membres qui est lié. Cette liste d’API est stockée dans un fichier XML qui se trouvent à  **\{projet directory}\obj\Release\api.xml** pour un **version** générer et à  **\{projet Directory}\obj\Debug\api.XML** pour un **déboguer** générer.

![Emplacement du fichier dans le dossier obj/Debug api.xml](java-bindings-metadata-images/java-bindings-metadata-01.png)

Le Générateur de liaisons utilisera le **api.xml** fichier comme indication pour générer les classes de wrapper c# nécessaires. Le contenu de ce fichier XML est une variante de Google _ouvrir le projet Source Android_ format.
L’extrait de code suivant est un exemple de contenu de **api.xml**:

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

Dans de nombreux cas, l’assistance humaine est requis pour rendre l’API Java sentir plus « .NET comme » ou pour corriger les problèmes qui empêchent l’assembly de liaison à partir de la compilation. Par exemple, il peut être nécessaire modifier les noms de package Java aux espaces de noms .NET, de renommer une classe ou de modifier le type de retour d’une méthode.

Ces modifications ne sont pas atteints en modifiant **api.xml** directement.
Au lieu de cela, les modifications sont enregistrées dans des fichiers XML spéciaux qui sont fournis par le modèle de bibliothèque de liaison de Java. Lors de la compilation de l’assembly de liaison de Xamarin.Android, le Générateur de liaisons sont tributaires de ces fichiers de mappage lors de la création de l’assembly de liaison

Ces fichiers de mappage XML peuvent être trouvés dans le **transforme** dossier du projet :

-   **MetaData.xml** &ndash; permet d’apporter des modifications à apporter à l’API final, telles que la modification de l’espace de noms de la liaison générée. 

-   **EnumFields.xml** &ndash; contient le mappage entre Java `int` constantes et c# `enums` . 

-   **EnumMethods.xml** &ndash; permet de modifier les paramètres de méthode et les types de retour à partir de Java `int` constantes en c# `enums` . 

Le **MetaData.xml** fichier est le meilleur parti d’importation de ces fichiers, car elle permet des modifications à usage général pour la liaison telles que :

-   Renommer les espaces de noms, classes, méthodes ou champs afin qu’ils suivent les conventions de .NET. 

-   Suppression des espaces de noms, classes, méthodes ou champs qui ne sont pas nécessaires. 

-   Déplacement de classes pour les espaces de noms différents. 

-   Ajout de classes de prise en charge supplémentaire pour rendre la conception de la liaison de suivre les modèles de .NET framework. 

Vous permet de passer à discuter **Metadata.xml** plus en détail.


## <a name="metadataxml-transform-file"></a>Fichier de transformation de fichier Metadata.Xml

Comme nous avons déjà appris, le fichier **Metadata.xml** est utilisé par le Générateur de liaisons pour influencer la création de l’assembly de liaison.
Le format de métadonnées utilise [XPath](https://www.w3.org/TR/xpath/) syntaxe et est presque identique à la *GAPI métadonnées* décrit dans [GAPI métadonnées](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) guide. Cette implémentation est presque une implémentation complète de XPath 1.0 et par conséquent prend en charge des éléments dans la norme 1.0. Ce fichier est un XPath basé un mécanisme puissant pour modifier, ajouter, masquer ou déplacer un élément ou attribut dans le fichier de l’API. Tous les éléments de règle dans les spécifications de métadonnées incluent un attribut de chemin d’accès pour identifier le nœud auquel la règle doit être appliquée. Les règles sont appliquées dans l’ordre suivant :

* **Ajout de nœud** &ndash; ajoute un nœud enfant au nœud spécifié par l’attribut de chemin d’accès.
* **attr** &ndash; définit la valeur d’un attribut de l’élément spécifié par l’attribut de chemin d’accès.
* **suppression de nœud** &ndash; supprime les nœuds correspondant à une expression XPath spécifié.

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

Voici quelques-uns des éléments XPath plus couramment utilisés pour l’API Java :

-   `interface` &ndash; Utilisé pour localiser une interface Java. par exemple, `/interface[@name='AuthListener']`.

-   `class` &ndash; Utilisé pour localiser une classe. par exemple, `/class[@name='MapView']`.

-   `method` &ndash; Utilisé pour localiser une méthode sur une classe Java ou une interface. par exemple, `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Identifier un paramètre pour une méthode. Par exemple `/parameter[@name='p0']`



### <a name="adding-types"></a>Ajout de Types

Le `add-node` élément indiquera le projet de liaison de Xamarin.Android pour ajouter une nouvelle classe de wrapper pour **api.xml**. Par exemple, l’extrait suivant dirigera le Générateur de liaison pour créer une classe avec un constructeur et un champ unique :

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Suppression de Types

Il est possible de demande au Générateur de liaisons de Xamarin.Android d’ignorer un type Java et pas le lier. Cela est fait en ajoutant un `remove-node` élément XML à le **metadata.xml** fichier :

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Modification du nom des membres

Renommer les membres ne peut pas être effectuée en modifiant directement le **api.xml** le fichier car Xamarin.Android requiert les noms d’Interface JNI (Java Native) d’origine. Par conséquent, le `//class/@name` attribut ne peut pas être modifié ; s’il est, la liaison ne fonctionnera pas.

Prenons le cas où vous souhaitez renommer un type, `android.Manifest`.
Pour ce faire, nous pouvons essayez de modifier directement **api.xml** et renommez la classe comme suit :

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Il en résulte dans le Générateur de liaisons de création du code c# suivant pour la classe wrapper :

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Notez que la classe wrapper a été renommée en `NewName`, tandis que le type Java d’origine est toujours `Manifest`. Il n’est plus possible de la classe de liaison de Xamarin.Android pour accéder à toutes les méthodes dans `android.Manifest`; la classe wrapper est liée à un type de Java inexistant.

Pour modifier correctement le nom managé de type encapsulé (ou méthode), il est nécessaire de définir la `managedName` d’attributs comme indiqué dans cet exemple :

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Changement de nom `EventArg` Classes Wrapper

Lorsque le Générateur de liaison de Xamarin.Android identifie une `onXXX` méthode setter pour une _type d’écouteur_, un événement de c# et `EventArgs` sous-classe sera générée pour prendre en charge .NET préparés à l’API pour l’écouteur basée sur Java modèle. Par exemple, considérez la classe Java suivante et la méthode :

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android supprimera le préfixe `on` à partir de la méthode setter, mais plutôt `2DSignNextManuever` comme base pour le nom de la `EventArgs` sous-classe. La sous-classe est nommée quelque chose de similaire à :

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Cela n’est pas un nom de classe c# juridique. Pour corriger ce problème, l’auteur de la liaison doive utiliser le `argsType` d’attribut et fournir un nom c# valide pour le `EventArgs` sous-classe :
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Attributs pris en charge

Les sections suivantes décrivent certains des attributs pour la transformation des API Java.

### <a name="argstype"></a>argsType

Cet attribut est placé sur les méthodes d’accesseur Set pour nommer le `EventArg` sous-classe qui est générée pour prendre en charge les écouteurs de Java. Cet exemple est décrit plus en détail ci-dessous dans la section [changement de nom des Classes Wrapper EventArg](#Renaming_EventArg_Wrapper_Classes) plus loin dans ce guide.

### <a name="eventname"></a>eventName

Spécifie un nom d’un événement. Si elle est vide, il empêche la génération d’événements.
Cet exemple est décrit plus en détail dans le titre de section [changement de nom des Classes Wrapper EventArg](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Cela permet de modifier le nom du package, de classe, de méthode ou de paramètre. Par exemple, pour modifier le nom de la classe Java `MyClass` à `NewClassName`:

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

`managedType` permet de modifier le type de retour d’une méthode. Dans certaines situations, le Générateur de liaisons incorrectement déduit le type de retour d’une méthode Java, ce qui entraîne une erreur de compilation. Une solution possible dans cette situation consiste à modifier le type de retour de la méthode.

Par exemple, le Générateur de liaisons considère que la méthode Java `de.neom.neoreadersdk.resolution.compareTo()` doit retourner un `int`, ce qui entraîne le message d’erreur **erreur CS0535 : ' DE. Neom.Neoreadersdk.Resolution' n’implémente pas le membre d’interface 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)'**. L’extrait de code suivant montre comment modifier le type de retour de la méthode c# généré à partir d’un `int` à un `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Modifie le type de retour d’une méthode. Cela ne modifie pas l’attribut de retour (comme les modifications apportées aux attributs peuvent entraîner des changements incompatibles à la signature JNI de retour). Dans l’exemple suivant, le type de retour de la `append` méthode est passée de `SpannableStringBuilder` à `IAppendable` (Souvenez-vous que c# ne prend pas en charge les types de retour covariants) :

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obscurcis

Les outils qui masquent les bibliothèques Java peuvent interférer avec le Générateur de liaison de Xamarin.Android et sa capacité à générer des classes wrapper c#. Caractéristiques des classes obscurcis incluent : * le nom de classe inclut un  **$** , c'est-à-dire **un .class $** * le nom de classe est entièrement compromis de caractères minuscules, c'est-à-dire  **a.class**

Cet extrait de code est un exemple montrant comment générer un type c# « non obscurci » :

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Cet attribut peut être utilisé pour modifier le nom d’une propriété managée.

Un cas spécialisé à l’aide de `propertyName` implique la situation où une classe Java a uniquement une méthode getter d’un champ. Dans ce cas, le Générateur de liaison est souhaitable créer une propriété en écriture seule, ce qui est déconseillé dans .NET. L’extrait de code suivant montre comment « supprimer » sur les propriétés .NET en définissant le `propertyName` sur une chaîne vide :

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Notez que les méthodes d’accesseur Get et Set seront toujours créées par le Générateur de liaisons.

### <a name="sender"></a>sender

Spécifie le paramètre d’une méthode doit être le `sender` paramètre lorsque la méthode est mappée à un événement. La valeur peut être `true` ou `false`. Exemple :

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>visibilité

Cet attribut est utilisé pour modifier la visibilité d’une classe, une méthode ou une propriété. Par exemple, il peut être nécessaire promouvoir un `protected` méthode Java afin que le wrapper c# correspondant est `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml et EnumMethods.xml

Il existe des cas où les bibliothèques Android utilisent des constantes entières pour représenter les États qui sont passés aux propriétés ou méthodes des bibliothèques. Dans de nombreux cas, il est utile lier ces constantes entières pour les enums en c#. Pour faciliter ce mappage, utilisez le **EnumFields.xml** et **EnumMethods.xml** fichiers dans votre projet de liaison. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Définition d’un Enum à l’aide de EnumFields.xml

Le **EnumFields.xml** fichier contient le mappage entre Java `int` constantes et c# `enums`. Prenons l’exemple suivant d’un enum c# en cours de création d’un ensemble de `int` constantes : 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Ici, nous avons pris la classe Java `SKRealReachSettings` et défini un enum c# appelé `SKRealReachSettings` dans l’espace de noms `Skobbler.Ngx.Map.RealReach`. Le `field` entrées définit le nom de la constante de Java (exemple `UNIT\_SECOND`), le nom de l’entrée de l’enum (exemple `Second`) et la valeur entière représentée par les deux entités (exemple `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Définition des méthodes d’accesseur Get/Set à l’aide de EnumMethods.xml

Le **EnumMethods.xml** fichier permet de changer les paramètres de méthode et les types de retour à partir de Java `int` constantes en c# `enums`. En d’autres termes, il mappe la lecture et l’écriture d’enums c# (défini dans le **EnumFields.xml** fichier) Java `int` constante `get` et `set` méthodes.

Étant donné la `SKRealReachSettings` définie ci-dessus, les éléments suivants de l’énumération **EnumMethods.xml** fichier définirait l’accesseur Get/Set pour cet enum :

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

La première `method` ligne mappe la valeur de retour de Java `getMeasurementUnit` méthode à la `SKRealReachSettings` enum. La seconde `method` ligne mappe le premier paramètre de la `setMeasurementUnit` à la même énumération.

Toutes ces modifications en place, vous pouvez utiliser le code suivant dans Xamarin.Android pour définir le `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Récapitulatif

Cet article décrit comment Xamarin.Android utilise les métadonnées pour transformer une définition d’API à partir de la *Google* *format AOSP*. Une fois les modifications sont possibles à l’aide de *Metadata.xml*, elle examinait les limitations rencontrées lors de la modification du nom des membres et qu’elle présente la liste des attributs pris en charge de XML, décrivant où chaque attribut doit être utilisé.



## <a name="related-links"></a>Liens associés

- [Utilisation de JNI](~/android/platform/java-integration/working-with-jni.md)
- [Liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md)
- [Métadonnées GAPI](http://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
