---
title: Partie 4 - vous traitez des plateformes multiples
ms.topic: article
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 21cd08ad2eb9c78ba1bcd6b31400a38266c65e51
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Partie 4 - vous traitez des plateformes multiples

## <a name="handling-platform-divergence-amp-features"></a>La gestion des différences de plateforme &amp; fonctionnalités

Divergence n’est pas simplement un problème de 'cross-platform' ; périphériques sur la plateforme « même » ont des fonctions différentes (en particulier la diversité des appareils Android qui sont disponibles). La plus évidente et la base sont la taille de l’écran, mais les autres attributs d’appareil peuvent varier et requièrent une application à vérifier pour certaines fonctionnalités et se comportent différemment en fonction de leur présence (ou absence).

Cela signifie que toutes les applications doivent traiter dégradation progressive des fonctionnalités, ou bien de présenter un ensemble de la fonctionnalité potentiels, le plus bas qu’un dénominateur commun. Profonde intégration Xamarin avec les kits de développement natifs de chaque plateforme permettent aux applications de tirer parti des fonctionnalités de la plateforme spécifique, il est donc judicieux de concevoir des applications pour utiliser ces fonctionnalités.

Consultez la documentation de fonctionnalités de plateforme pour une vue d’ensemble des diffèrent entre les plateformes de fonctionnalités.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Exemples de Divergence de plateforme

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Éléments fondamentaux qui existent entre les plateformes

Il existe certaines caractéristiques des applications mobiles qui sont communs.
Il s’agit des concepts de niveau supérieur qui sont généralement remplies de tous les périphériques et peuvent par conséquent servent de conception de votre application :

-  Sélection des fonctionnalités via les onglets ou les menus
-  Listes de données et le défilement
-  Vues de données uniques
-  Modification des vues de données uniques
-  Naviguer vers l’arrière


Lors de la conception de flux de votre écran de haut niveau, vous pouvez baser une expérience utilisateur commune sur ces concepts.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>attributs spécifiques à la plateforme

Outre les éléments de base qui existent sur toutes les plateformes, vous devrez les différences de plateforme clé d’adresse dans votre conception. Vous devrez peut-être prendre en compte (et à écrire du code pour traiter spécifiquement) ces différences :

-   **Tailles d’écran** – certaines plateformes (par exemple, iOS et les versions antérieures de Windows Phone) ont normalisé de tailles d’écran sont relativement simples à cibler. Les appareils Android ont une grande variété de dimensions d’écran, qui nécessitent davantage d’efforts pour prendre en charge dans votre application.
-   **Les métaphores de navigation** – peut varier entre les plateformes (par exemple). bouton de matériel « arrière », contrôle d’interface utilisateur de Panorama) et dans les plateformes (Android 2 et 4, iPhone et iPad).
-   **Claviers** : les appareils Android certains ont des claviers physiques alors que d’autres ont uniquement un clavier logiciel. Code qui détecte un clavier soft-Éclipse partie de l’écran doit prendre en compte ces différences.
-   **Tactiles et des mouvements** – prise en charge de système d’exploitation pour la reconnaissance de mouvement varie, en particulier dans les versions antérieures de chaque système d’exploitation. Les versions antérieures d’Android ont très limité de prise en charge pour les opérations de contact, ce qui signifie que la prise en charge des périphériques plus anciens peut-être requiert code distinct
-   **Notifications Push** : il existe des fonctionnalités/implémentations différentes sur chaque plateforme (par exemple). Vignettes dynamiques sur Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Fonctionnalités spécifiques au périphérique

Déterminer ce qui doivent être les fonctionnalités minimales requises pour l’application ; ou lorsque décider quelles fonctionnalités supplémentaires pour tirer parti sur chaque plateforme. Code devra détecter les fonctionnalités et désactiver la fonctionnalité ou offrent des alternatives (par exemple). une alternative à l’emplacement géographique peut être à l’utilisateur entrer l’emplacement ou choisissez une carte) :

-   **Appareil photo** – fonctionnalités différent à travers des périphériques : certains périphériques n’ont une caméra, d’autres ont les deux appareils photo de face et arrière. Certains appareils sont capables d’enregistrement vidéo.
-   **Emplacement géographique & mappages** – prise en charge pour l’emplacement du GPS ou Wi-Fi n’est pas présent sur tous les appareils. Les applications doivent également prenant en charge pour les différents niveaux de précision qui est pris en charge par chaque méthode.
-   **Accéléromètre, d’un gyroscope et boussole** – ces fonctionnalités souvent figurent dans uniquement une sélection de périphériques sur chaque plateforme, de sorte que les applications doivent presque toujours fournir un secours lorsque le matériel n’est pas prise en charge.
-   **Twitter et Facebook** – uniquement « intégré » sur iOS5 et iOS6 respectivement. Sur les versions antérieures et d’autres plateformes, vous devez le fournir vos propres fonctions d’authentification directement l’interface avec chaque API des services.
-   **Approche de Communications en champ (NFC)** – uniquement sur les (certains) les téléphones Android (au moment de l’écriture).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Traitement de Divergence de plateforme

Il existe deux approches différentes pour prendre en charge plusieurs plateformes à partir de la même-base de code, chacun avec son propre ensemble d’avantages et inconvénients.

-   **Abstraction de plateforme** – modèle de Façade métier, fournit un accès unifié sur plusieurs plateformes et résume les implémentations de plateforme spécifique dans une API unifiée.
-   **Implémentation divergente** – appel de plateforme spécifique des fonctionnalités via des implémentations divergentes via Outils architecture telles que les interfaces et l’héritage ou la compilation conditionnelle.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Abstraction de plateforme

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Abstraction de classe

À l’aide des interfaces ou classes de base défini dans le code partagé et implémentée ou étendus dans les projets de plateforme spécifique. L’écriture et l’extension de code partagé avec les abstractions de classe sont particulièrement adapté aux bibliothèques de classes portables, car ils ont un sous-ensemble limité du framework à leur disposition et ne peut pas contenir des directives de compilateur pour prendre en charge des branches de code spécifique à la plateforme.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Interfaces

À l’aide des interfaces vous permet d’implémenter les classes spécifiques à la plateforme qui peuvent toujours être passés dans vos bibliothèques partagées pour tirer parti du code commun.

L’interface est définie dans le code partagé et passée dans la bibliothèque partagée comme un paramètre ou une propriété.

Les applications spécifiques à la plateforme peuvent ensuite implémenter l’interface et tirer parti du code partagé ' traiter '.

 **Avantages**

L’implémentation peut contenir du code de plateforme spécifique et même référencer les bibliothèques externes spécifiques à la plateforme.

 **Inconvénients**

Devoir créer et transmettre des implémentations dans le code partagé. Si l’interface est utilisée dans le code partagé puis il finit par être transmises via plusieurs paramètres de méthode ou sinon poussée vers le bas dans la chaîne d’appel. Si le code partagé utilise un grand nombre d’interfaces différentes puis ils doivent tous être créées et définies dans le code partagé quelque part.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Héritage

Le code partagé peut implémenter des classes abstraites ou virtuelles qui peut être étendues dans un ou plusieurs projets spécifiques à la plateforme. Cela est similaire à l’aide des interfaces, mais avec un comportement déjà implémenté. Il existe des points de vue différents selon que l’héritage ou les interfaces sont un meilleur choix de conception : en particulier, car le langage c# autorise uniquement l’héritage unique qu’il peut déterminent la façon dont votre API peut être conçu à l’avenir. Utiliser l’héritage avec précaution.

Les avantages et inconvénients des interfaces s’appliquent également à l’héritage, avec l’avantage supplémentaire de la classe de base peut contenir du code d’implémentation (peut-être un ensemble de la plate-forme agnostique implémentation qui peut être étendue si vous le souhaitez).

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Consultez le [Xamarin.Forms](~/xamarin-forms/get-started/index.md) documentation.


### <a name="plug-in-cross-platform-functionality"></a>Plug-ins inter-plateformes

Vous pouvez également étendre des applications multiplateformes de façon cohérente à l’aide des plug-ins.

À partir de notre [github de plug-ins](https://github.com/xamarin/plugins), la plupart de plug-ins sont open source projets (généralement disponibles pour une installation via Nuget) qui vous aident à implémentent une gamme de fonctionnalités spécifiques à la plateforme à partir de l’état de la batterie sur les paramètres avec un API commune qui est facile à utiliser dans les applications de plateforme de Xamarin et Xamarin.Forms.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Autres bibliothèques multiplateforme

Il existe un nombre 3e partie des bibliothèques de disponibles qui fournissent des fonctionnalités multiplateforme :

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Couramment** (pour la localisation) - [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (pour les jeux XNA) - [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) et son précurseur [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Implémentation divergente

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Compilation conditionnelle

Il existe certaines situations où votre code partagé toujours devrez fonctionnent différemment sur chaque plateforme, éventuellement l’accès à des classes ou des fonctionnalités qui se comportent différemment. Compilation conditionnelle fonctionne mieux avec les projets d’actifs partagés, où le même fichier source est référencé dans plusieurs projets qui ont des symboles définis.

Les projets Xamarin toujours définissent `__MOBILE__` qui a la valeur true pour iOS et les projets d’application Android (Notez le trait de soulignement double pré- et post-correctif sur ces symboles).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Xamarin.iOS définit `__IOS__` qui vous permet de détecter des périphériques iOS.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

Il existe également des symboles espion et de télévision spécifiques :

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

<a name="Android" />

##### <a name="android"></a>Android

Le code qui doit uniquement être compilé dans les applications Xamarin.Android permettre utiliser les éléments suivants

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Chaque version de l’API définit également une nouvelle directive de compilateur, afin de code tel que cela permettra d’ajouter des fonctionnalités si plus récent API est ciblés. Chaque niveau de l’API comprend tous les symboles de niveau « inférieurs ». Cette fonctionnalité n’est pas vraiment utile pour prendre en charge plusieurs plateformes ; en règle générale le `__ANDROID__` symbole sera suffisant.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Il n’existe pas actuellement un symbole intégré pour Xamarin.Mac, mais vous pouvez ajouter votre propre Mac projet d’application **Options > Générer > compilateur** dans les **définir les symboles de** zone, ou modifier le **.csproj**  et ajoutez il (par exemple `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Les applications Windows Phone définit deux symboles – `WINDOWS_PHONE` et `SILVERLIGHT` – qui peut être utilisé pour le code à la plateforme cible. Elles n’ont pas les traits de soulignement entourant les tels que les symboles de plateforme Xamarin faire.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>À l’aide de la Compilation conditionnelle

La configuration de l’emplacement du fichier pour le fichier de base de données SQLite est un exemple simple d’étude de cas de compilation conditionnelle. Les trois plateformes ont des exigences légèrement différentes pour spécifier l’emplacement du fichier :

-   **iOS** – Apple préfère non-utilisateur les données à placer dans un emplacement spécifique (le répertoire des bibliothèques), mais il n’existe aucune constante système pour cet annuaire. Code de plateforme spécifique est requis pour générer le chemin d’accès correct.
-   **Android** : le chemin d’accès système retourné par `Environment.SpecialFolder.Personal` est un emplacement acceptable pour stocker le fichier de base de données.
-   **Windows Phone** – le mécanisme de stockage isolé n’autorise pas un chemin d’accès complet spécifier uniquement un chemin d’accès relatif et nom de fichier.
-   **Plateforme Windows universelle** – utilise `Windows.Storage` API.

Le code suivant utilise la compilation conditionnelle pour garantir la `DatabaseFilePath` est correct pour chaque plateforme :

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

Le résultat est une classe qui peut être générée et utilisée sur toutes les plateformes, en plaçant le fichier de base de données SQLite dans un emplacement différent sur chaque plateforme.

