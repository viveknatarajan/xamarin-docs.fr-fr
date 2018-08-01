---
title: 'Partie 4 : gérer plusieurs plateformes'
description: Ce document décrit comment gérer la divergence application basée sur la plateforme ou de fonctionnalité. Il aborde la taille de l’écran, métaphores basées sur la navigation, tactile et mouvements, notifications push et paradigmes interface telles que des listes et des onglets.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4a60c99cbc9819f07b77bfe9abe046ea92a550a5
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403323"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>Partie 4 : gérer plusieurs plateformes

## <a name="handling-platform-divergence-amp-features"></a>Divergence de plateforme de gestion des &amp; fonctionnalités

Divergence n’est pas simplement un problème de 'cross-platform' ; appareils sur la plateforme « même » ont des fonctionnalités différentes (en particulier la diversité des appareils Android qui sont disponibles). La plus évidente et la base sont la taille de l’écran, mais les autres attributs de l’appareil peuvent varier et nécessitent une application de vérifier certaines fonctionnalités et se comportent différemment selon leur présence (ou l’absence).

Cela signifie que toutes les applications doivent gérer dégradation progressive des fonctionnalités, ou bien de présenter un ensemble de fonctionnalités attractives, le plus bas qu’un dénominateur commun. Étroite intégration de Xamarin avec les kits de développement natifs de chaque plateforme permettent aux applications de tirer parti des fonctionnalités spécifiques à la plateforme, il est donc logique de concevoir des applications d’utiliser ces fonctionnalités.

Consultez la documentation sur les fonctionnalités de plateforme pour une vue d’ensemble de la façon dont les plateformes diffèrent dans les fonctionnalités.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>Exemples de Divergence de plateforme

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>Éléments fondamentaux qui existent sur plusieurs plateformes

Il existe certaines caractéristiques des applications mobiles qui sont appliquent.
Il s’agit des concepts de niveau supérieurs qui sont généralement remplies de tous les périphériques et peuvent par conséquent servent de conception de votre application :

-  Sélection des fonctionnalités via les onglets ou les menus
-  Listes de données et le défilement
-  Vues de données uniques
-  Modification des vues uniques de données
-  Navigation vers l’arrière


Lorsque vous concevez votre flux d’écran de haut niveau, vous pouvez baser une expérience utilisateur commune sur ces concepts.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>Attributs spécifiques à la plateforme

Outre les éléments de base qui existent sur toutes les plateformes, vous devrez résolution des différences clés de la plateforme dans votre conception. Vous devrez peut-être prendre en compte (et écrire du code pour traiter spécifiquement) ces différences :

-   **Tailles d’écran** – certaines plates-formes (comme iOS et les versions antérieures de Windows Phone) utilisent les mêmes tailles d’écran sont relativement simples à cibler. Les appareils Android ont une grande variété de dimensions d’écran, qui nécessitent davantage d’efforts pour prendre en charge dans votre application.
-   **Les métaphores de navigation** – peut varier entre plateformes (par ex. matériel « arrière », l’interface utilisateur de Panorama contrôle button) et dans les plateformes (Android 2 et 4, iPhone et iPad).
-   **Claviers** : les appareils Android certains ont des claviers physiques tandis que d’autres ont uniquement un clavier logiciel. Code qui détecte un soft-clavier dissimule partie de l’écran doit être sensible aux ces différences.
-   **Touchers et gestes** – prise en charge de système d’exploitation pour la reconnaissance de mouvement varie, en particulier dans les versions antérieures de chaque système d’exploitation. Les versions antérieures d’Android ont très limité de prise en charge pour les opérations tactile, ce qui signifie que la prise en charge des appareils plus anciens peut nécessiter code distinct
-   **Notifications Push** : il existe différentes fonctionnalités/implémentations sur chaque plateforme (par exemple). Vignettes dynamiques sur Windows).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>Fonctionnalités spécifiques au périphérique

Déterminer les fonctionnalités minimales requises pour l’application doivent être ; ou lorsque décider quelles fonctionnalités supplémentaires pour tirer parti de sur chaque plateforme. Code doit détecter les fonctionnalités et ou désactiver les fonctionnalités offrent des alternatives (par ex. une alternative à l’emplacement géographique peut être à l’utilisateur entrer l’emplacement ou choisir à partir d’une carte) :

-   **Appareil photo** – fonctionnalités varie en fonction des appareils : certains appareils ne comporte pas de caméra, d’autres ont deux caméras face avant et arrière. Certains appareils sont capables d’enregistrement vidéo.
-   **Emplacement géographique & mappages** – prise en charge pour l’emplacement GPS ou Wi-Fi n’est pas présent sur tous les appareils. Vous devez également veiller aux différents niveaux de précision qui est pris en charge par chaque méthode applications.
-   **Accéléromètre, gyroscope et la boussole** – ces fonctionnalités souvent figurent dans uniquement une sélection de périphériques sur chaque plateforme, donc applications presque toujours à fournir une solution de secours lorsque le matériel n’est pas prise en charge.
-   **Twitter et Facebook** – uniquement « intégrées » sur iOS5 et iOS6 respectivement. Sur les versions antérieures et d’autres plates-formes vous devez fournir vos propres fonctions d’authentification et d’interagir directement avec chaque API services.
-   **Près de Communications en champ (NFC)** : uniquement sur (certaines) les téléphones Android (au moment de l’écriture).


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>Traitement de Divergence de plateforme

Il existe deux approches différentes pour prendre en charge plusieurs plateformes à partir du même code base, chacun avec son propre ensemble d’avantages et inconvénients.

-   **Abstraction de plateforme** – modèle Façade métier, fournit un accès unifié sur plusieurs plateformes et extrait ensuite les implémentations de plateforme spécifique dans une API unifiée.
-   **Implémentation divergente** – appel de plateforme spécifique des fonctionnalités via des implémentations divergentes via des outils architectures tels que les interfaces et héritage ou de compilation conditionnelle.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>Abstraction de plateforme

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>Abstraction de classe

À l’aide des interfaces ou classes de base défini dans le code partagé et implémentée ou étendus dans les projets spécifiques à la plateforme. Écriture et l’extension de code partagé avec les abstractions de classe sont particulièrement adaptée aux bibliothèques de classes portables, car elles ont un sous-ensemble limité du framework à leur disposition et ne peut pas contenir des directives de compilateur pour prendre en charge des branches de code spécifique à la plateforme.

 <a name="Interfaces" />


#### <a name="interfaces"></a>Interfaces

À l’aide des interfaces vous permet d’implémenter les classes spécifiques à la plateforme qui peuvent toujours être passés dans vos bibliothèques partagées pour tirer parti du code commun.

L’interface est définie dans le code partagé et passée dans la bibliothèque partagée comme un paramètre ou une propriété.

Les applications spécifiques à la plateforme peuvent ensuite implémenter l’interface et quand même tirer parti du code partagé pour « processus ».

 **Avantages**

L’implémentation peut contenir le code spécifique à la plateforme et même faire référence aux bibliothèques externes spécifiques à la plateforme.

 **Inconvénients**

Avoir à créer et transmettre des implémentations dans le code partagé. Si l’interface est utilisée dans le code partagé puis il finit par être transmise à plusieurs paramètres de méthode ou sinon poussé vers le bas via la chaîne d’appel. Si le code partagé utilise un grand nombre d’interfaces différentes puis ils doivent tous être créées et définies dans le code partagé quelque part.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Héritage

Le code partagé peut implémenter des classes abstraites ou virtuelles qui peut être étendues dans un ou plusieurs projets spécifiques à la plateforme. Cela est similaire à l’aide des interfaces, mais avec un comportement déjà implémenté. Il existe des points de vue différents selon que l’héritage ou les interfaces sont un meilleur choix de conception : en particulier, car c# autorise uniquement l’héritage unique qu’il peut déterminent la façon dont votre API permettre être conçus à l’avenir. Utiliser l’héritage avec précaution.

Les avantages et inconvénients des interfaces s’appliquent également à l’héritage, avec l’avantage supplémentaire de la classe de base peut contenir du code d’implémentation (peut-être un ensemble de la plateforme agnostique implémentation qui peut être étendue si vous le souhaitez).

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

Consultez le [Xamarin.Forms](~/xamarin-forms/get-started/index.md) documentation.


### <a name="plug-in-cross-platform-functionality"></a>Plug-in des fonctionnalités multiplateformes

Vous pouvez également étendre des applications multiplateformes de manière cohérente à l’aide de plug-ins.

Lié à partir de notre [github de plug-ins](https://github.com/xamarin/plugins), la plupart des plug-ins sont open source projets (généralement disponibles pour une installation via Nuget) qui vous aident à implémentent une variété de fonctionnalités spécifiques à la plateforme à partir de l’état de la batterie aux paramètres avec un API commune qui est facile à utiliser dans les applications de la plateforme Xamarin et Xamarin.Forms.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>Autres bibliothèques inter-plateformes

Il existe un nombre de bibliothèques de tiers qui fournissent des fonctionnalités inter-plateformes 3e :

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **Couramment** (pour la localisation) -  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (pour les jeux XNA) -  [http://www.monogame.net](http://www.monogame.net)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) et son précurseur [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>Implémentation divergente

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>Compilation conditionnelle

Il existe certaines situations où votre code partagé toujours devrez se comportent différemment sur chaque plateforme, éventuellement l’accès à des classes ou des fonctionnalités qui se comportent différemment. Compilation conditionnelle fonctionne mieux avec les projets d’actifs partagés, où le même fichier source est référencé dans plusieurs projets qui ont différents symboles définis.

Projets Xamarin toujours définissent `__MOBILE__` qui est vrai pour les projets iOS et Android application (Notez le trait de soulignement double pré- et post-correctif sur ces symboles).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Xamarin.iOS définit `__IOS__` que vous pouvez utiliser pour détecter les appareils iOS.

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

Le code qui doit être compilé uniquement pour des applications Xamarin.Android peut utiliser les éléments suivants

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Chaque version de l’API définit également une directive de compilateur nouvelle, permettra de code tel que celui-ci vous ajouter fonctionnalités API plus récentes est ciblés. Chaque niveau d’API inclut tous les symboles de niveau « inférieurs ». Cette fonctionnalité n’est pas vraiment utile pour prendre en charge plusieurs plates-formes ; en règle générale le `__ANDROID__` symbole sera suffisant.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

Il n’est pas actuellement un symbole prédéfini pour Xamarin.Mac, mais vous pouvez ajouter votre propre dans le Mac projet d’application **Options > Build > compilateur** dans le **définissent les symboles** zone, ou modifier le **.csproj**  fichier, puis ajoutez il (par exemple `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Les applications Windows Phone définit deux symboles – `WINDOWS_PHONE` et `SILVERLIGHT` – qui peut être utilisé pour cibler le code vers la plateforme. Elles n’ont pas les traits de soulignement qui les entoure tels que les symboles de la plateforme Xamarin faire.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>À l’aide de la Compilation conditionnelle

Un exemple d’étude de cas simple de compilation conditionnelle consiste à définir l’emplacement du fichier pour le fichier de base de données SQLite. Les trois plateformes ont des exigences légèrement différentes pour spécifier l’emplacement du fichier :

-   **iOS** : Apple préfère non-utilisateur des données à placer dans un emplacement spécifique (le répertoire de bibliothèque), mais il n’existe aucune constante système pour cet annuaire. Code de spécifique à la plateforme est nécessaire pour générer le chemin correct.
-   **Android** : le chemin d’accès système retourné par `Environment.SpecialFolder.Personal` est un emplacement acceptable pour stocker le fichier de base de données.
-   **Windows Phone** – le mécanisme de stockage isolé n’autorise pas un chemin d’accès complet de la définir, simplement un chemin d’accès relatif et nom de fichier.
-   **Universal Windows Platform** – utilise `Windows.Storage` API.

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

Le résultat est une classe qui peut être créée et utilisée sur toutes les plateformes, en plaçant le fichier de base de données SQLite dans un emplacement différent sur chaque plateforme.

