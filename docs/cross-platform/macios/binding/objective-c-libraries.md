---
title: "Bibliothèques de liaison Objective-C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: d1c4c46b62b95d70dd2832c96ffd2686163990a5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="binding-objective-c-libraries"></a>Bibliothèques de liaison Objective-C

Lorsque vous travaillez avec Xamarin.iOS ou Xamarin.Mac, vous pouvez rencontrer des cas où vous souhaitez consommer une bibliothèque de Objective-C tiers. Dans ces situations, vous pouvez utiliser des projets de liaison Xamarin pour créer une liaison C# aux bibliothèques natives Objective-C. Le projet utilise les mêmes outils que nous utilisons pour rétablir l’iOS et Mac API c#.

Ce document décrit comment lier les API Objective-C, si vous liez simplement les API C, vous devez utiliser le mécanisme de .NET standard pour ce faire, [l’infrastructure de P/Invoke](http://mono-project.com/Dllimport).
Pour plus d’informations sur la façon de lier statiquement une bibliothèque C sont disponibles sur le [lier des bibliothèques natives](~/ios/platform/native-interop.md) page.

Voir le Guide de notre [Guide de référence des Types de liaison](~/cross-platform/macios/binding/binding-types-reference.md).
En outre, si vous souhaitez en savoir plus sur ce qui se passe dans les coulisses, vérifier notre [vue d’ensemble de la liaison](~/cross-platform/macios/binding/overview.md) page.

Les liaisons peuvent être créées pour iOS et les bibliothèques Mac.
Cette page décrit l’utilisation sur un iOS de liaison, toutefois, les liaisons Mac sont très similaires.

**Exemple de Code pour iOS**

Vous pouvez utiliser la [iOS de liaison, exemple](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) projet à faire des essais avec des liaisons.

<a name="Getting_Started" />

## <a name="getting-started"></a>Bien démarrer

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour créer une liaison, le plus simple consiste à créer un projet de liaison Xamarin.iOS.
Faire cela à partir de Visual Studio pour Mac en sélectionnant le type de projet **iOS > bibliothèque > bibliothèque de liaisons**:

[![](objective-c-libraries-images/00-sml.png "Cela à partir de Visual Studio pour Mac, sélectionnez le type de projet bibliothèque de liaisons d’iOS")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour créer une liaison, le plus simple consiste à créer un projet de liaison Xamarin.iOS.
Cela en sélectionnant le type de projet à partir de Visual Studio sur Windows **Visual c# > iOS > bibliothèque de liaisons (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS iOS de la bibliothèque de liaisons")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Remarque : Des projets de liaison pour **Xamarin.Mac** sont uniquement pris en charge dans Visual Studio pour Mac.

-----

Le projet généré un modèle petit que vous pouvez modifier, il contient deux fichiers : `ApiDefinition.cs` et `StructsAndEnums.cs`.

Le `ApiDefinition.cs` est où vous allez définir le contrat d’API, il s’agit du fichier qui décrit comment l’API Objective-C sous-jacente est projeté dans c#. La syntaxe et le contenu de ce fichier est le sujet principal de la discussion de ce document et le contenu de celui-ci est limité aux interfaces c# et aux déclarations de délégué c#. Le `StructsAndEnums.cs` est le fichier dans lequel vous devrez entrer toutes les définitions qui sont requises par les interfaces et les délégués. Cela inclut les valeurs d’énumération et les structures que votre code peut utiliser.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Une API de liaison

Pour effectuer une liaison complète, vous devez comprendre la définition d’API de Objective-C et de vous familiariser avec les instructions de conception de .NET Framework.

Pour lier votre bibliothèque vous démarre généralement avec un fichier de définition d’API. Un fichier de définition d’API est simplement un fichier source c# qui contient les interfaces c# qui ont été annotés avec un certain nombre d’attributs qui permettent la liaison.  Ce fichier est ce que définit le contrat entre c# et Objective-C.

Par exemple, il s’agit d’un fichier api simple pour une bibliothèque :

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

L’exemple ci-dessus définit une classe appelée `Cocos2D.Camera` qui dérive de la `NSObject` type de base (provient de ce type `Foundation.NSObject`) et qui définit une propriété statique (`ZEye`), deux méthodes qui prennent aucun argument et une méthode qui prend trois arguments.

Vous trouverez une présentation approfondie du format de fichier de l’API et les attributs que vous pouvez utiliser la [fichier de définition d’API](~/cross-platform/macios/binding/objective-c-libraries.md) section ci-dessous.

Pour produire une liaison terminée, vous gérerez en général avec quatre composants :

-  Le fichier de définition d’API (`ApiDefinition.cs` dans le modèle).
-  Facultatif : les enums, les types, les structs requis par le fichier de définition d’API (`StructsAndEnums.cs` dans le modèle).
-  Facultatifs : sources supplémentaires susceptibles de développer la liaison générée ou fournissent une plus c# conviviale API (les fichiers c# que vous ajoutez au projet).
-  La bibliothèque native que vous établissez une liaison.

Ce graphique montre la relation entre les fichiers :

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Ce graphique montre la relation entre les fichiers")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Le fichier de définition de l’API : contiendra uniquement les définitions d’espaces de noms et d’interface (avec tous les membres contenant une interface) et ne doivent contenir que des classes, des énumérations, des délégués ou des structures. Le fichier de définition d’API est simplement le contrat qui servira à générer l’API.

Tout code supplémentaire que vous avez besoin comme énumérations ou classes de prise en charge doit être hébergé sur un fichier distinct, dans l’exemple ci-dessus le « CameraMode » est une valeur d’énumération qui n’existe pas dans le fichier CS et doit être hébergée dans un fichier distinct, par exemple `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

Le `APIDefinition.cs` fichier est associé le `StructsAndEnum` classe et sont utilisés pour générer la liaison de base de la bibliothèque. Vous pouvez utiliser la bibliothèque résultante en tant que-est, mais en règle générale, vous pouvez paramétrer la bibliothèque résultante pour ajouter des fonctionnalités C# avantage de vos utilisateurs. Voici quelques exemples implémentant un `ToString()` (méthode), fournissent des indexeurs c#, ajoutez des conversions implicites vers et à partir de certains types natifs ou fournissent des versions fortement typées de certaines méthodes. Ces améliorations sont stockées dans des fichiers c# supplémentaires. Simplement ajouter les fichiers c# à votre projet et ils seront inclus dans ce processus de génération.

Cet exemple montre comment implémenter le code dans votre `Extra.cs` fichier. Notez que vous utiliserez les classes partielles comme ils augmentent les classes partielles qui sont générés à partir de la combinaison de la `ApiDefinition.cs` et `StructsAndEnums.cs` liaison de base :

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Génération de la bibliothèque produira votre liaison natif.

Pour effectuer cette liaison, vous devez ajouter la bibliothèque native au projet.  Vous pouvez cela en ajoutant la bibliothèque native à votre projet, soit en faisant glisser la bibliothèque native à partir de la recherche sur le projet dans l’Explorateur de solutions, soit en cliquant sur le projet et en choisissant **ajouter**  >  **Ajouter des fichiers** pour sélectionner la bibliothèque native.
Par convention, les bibliothèques natives commençant par le mot « lib » et se terminent par l’extension « .a ». Dans ce cas, Visual Studio pour Mac ajoute deux fichiers : le `.a` fichier et un fichier c# automatiquement rempli qui contient des informations sur ce que contient la bibliothèque native :

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Par convention, les bibliothèques natives commencent par lib word et se terminent par l’extension .a")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Le contenu de la `libMagicChord.linkwith.cs` fichier contient des informations sur l’utilisation de cette bibliothèque et fait en sorte que votre interface IDE pour créer un package dans le fichier DLL qui en résulte ce binaire :

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

En savoir plus sur l’utilisation de l’attribut LinkWith sont documentées dans notre [Guide de référence des Types de liaison](~/cross-platform/macios/binding/binding-types-reference.md).

Maintenant lorsque vous générez le projet se retrouve avec un `MagicChords.dll` fichier qui contient la liaison et la bibliothèque native. Vous pouvez distribuer ce projet ou la DLL résultante à d’autres développeurs pour leurs propres utiliser.

Parfois, vous constaterez peut-être que vous avez besoin de plusieurs valeurs d’énumération, les définitions de délégué ou d’autres types. Ne placez pas celles figurant dans le fichier de définitions API, car il s’agit simplement d’un contrat

 <a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Le fichier de définition d’API

Le fichier de définition d’API se compose d’un nombre d’interfaces. Les interfaces dans la définition de l’API seront activés dans une déclaration de classe, et elles doivent être décorées avec le [[BaseType]](~/cross-platform/macios/binding/binding-types-reference.md) attribut pour spécifier la classe de base pour la classe.

Vous vous demandez peut-être pourquoi nous n’avez pas utilisé classes au lieu d’interfaces pour la définition du contrat. Nous avons choisi d’interfaces, car il nous a permis d’écrire le contrat pour une méthode sans avoir à fournir un corps de méthode dans le fichier de définition d’API, ou avoir à fournir un corps qui avait pour lever une exception ou retourner une valeur significative.

Mais étant donné que nous utilisons l’interface comme squelette pour générer une classe que nous avons eu à recourir à la décoration des différentes parties du contrat avec des attributs à la liaison.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Méthodes de liaison

Est de la liaison la plus simple, que vous pouvez effectuer pour lier une méthode. Déclarer une méthode dans l’interface avec les conventions d’affectation de noms c# uniquement et de décorer la méthode avec le [[Exporter]](~/cross-platform/macios/binding/binding-types-reference.md) attribut. L’attribut [Exporter] fait le lien votre nom c# avec le nom de Objective-C dans le runtime Xamarin.iOS. Le paramètre de l’attribut de l’exportation est le nom du sélecteur Objective-C, quelques exemples :

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

Les exemples ci-dessus montrent comment vous pouvez lier les méthodes d’instance. Pour lier les méthodes statiques, vous devez utiliser le `[Static]` attribut, comme suit :

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Cela est nécessaire, car le contrat fait partie d’une interface et les interfaces n’ont aucune notion de déclarations de ports statiques et d’instance, il est donc nécessaire à nouveau de recourir aux attributs. Si vous souhaitez masquer une méthode particulière de la liaison, vous pouvez la décorer la méthode avec le [[interne]](~/cross-platform/macios/binding/binding-types-reference.md) attribut.

Le `btouch-native` commande introduira vérifie les paramètres de référence ne pas être null. Si vous souhaitez autoriser les valeurs null pour un paramètre particulier, utilisez le [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) attribut sur le paramètre, comme suit :

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Lorsque vous exportez un type référence, par le `[Export]` (mot clé), vous pouvez également spécifier la sémantique d’allocation. Cela est nécessaire pour s’assurer qu’aucune donnée n’est intégrée.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Propriétés de liaison

Tout comme les méthodes, les propriétés de Objective-C sont liées à l’aide de la [[Exporter]](~/cross-platform/macios/binding/binding-types-reference.md) attribut et mappent directement aux propriétés c#. Tout comme les méthodes, propriétés peuvent être décorées avec le [[statique]](~/cross-platform/macios/binding/binding-types-reference.md) et [[interne]](~/cross-platform/macios/binding/binding-types-reference.md) attributs.

Lorsque vous utilisez la `[Export]` attribut sur une propriété sous couvre btouch-natif lie en fait deux méthodes : la méthode getter et setter. Le nom que vous fournissez à exporter est la **basename** et l’accesseur Set est calculée en ajoutant le préfixe du mot « set », l’activation de la première lettre de la **basename** en majuscules et rendre le sélecteur de prendre un argument. Cela signifie que `[Export ("label")]` appliqué à une propriété lie réellement l’étiquette « » et « setLabel : « les méthodes Objective-C.

Parfois, les propriétés Objective-C ne suivent pas le modèle décrit ci-dessus et le nom est remplacé manuellement. Dans ce cas, vous pouvez contrôler la façon que la liaison est générée à l’aide de la `[Bind]` attribut sur la méthode getter ou setter, par exemple :

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Ceci lie « isMenuVisible » et « setMenuVisible : ». Si vous le souhaitez, une propriété peut être liée à l’aide de la syntaxe suivante :

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

Où les méthodes getter et setter sont explicitement définies comme dans le `name` et `setName` liaisons ci-dessus.

Outre la prise en charge pour les propriétés statiques à l’aide de `[Static]`, vous pouvez la décorer les propriétés statiques de thread avec [[IsThreadStatic]](~/cross-platform/macios/binding/binding-types-reference.md), par exemple :

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Tout comme les méthodes permettent de certains paramètres marquage avec [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md), vous pouvez appliquer [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) à une propriété pour indiquer que la valeur null est une valeur valide pour la propriété, par exemple :

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

Le [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) paramètre peut également être spécifié directement sur l’accesseur Set :

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Restrictions de liaison de contrôles personnalisés

En prenant les précautions suivantes doivent être pris en compte lorsque vous configurez la liaison pour un contrôle personnalisé :

1. **Propriétés de liaison doivent être statiques** - lors de la définition de la liaison des propriétés, la `Static` attribut doit être utilisé.
2. **Les noms de propriété doivent correspondre exactement** -le nom utilisé pour lier la propriété doit correspondre exactement à celui de la propriété dans le contrôle personnalisé.
3. **Types de propriété doivent correspondre exactement à** -le type de variable utilisé pour lier la propriété doit correspondre exactement au type de la propriété dans le contrôle personnalisé.
4. **Points d’arrêt et l’accesseur Get/Set** - points d’arrêt sont placés dans l’accesseur Get ou méthodes setter de la propriété ne seront jamais atteint.
5. **Observez les rappels** -vous devez utiliser les rappels d’observation à être averti des modifications dans les valeurs de propriété des contrôles personnalisés.

Pour observer les avertissements répertoriés ci-dessus peut entraîner la liaison échoue en mode silencieux lors de l’exécution.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Propriétés et modèle mutable de objective-C

Les infrastructures objective-C utilisent un idiome où certaines classes sont immuables avec une sous-classe mutable.   Par exemple `NSString` est la version immuable, tandis que `NSMutableString` est la sous-classe qui permet de mutation.

Dans ces classes, il est courant de voir la classe de base immuable contiennent des propriétés avec un accesseur Get, mais pas d’accesseur Set.   Et pour la version mutable introduire la méthode setter.   Étant donné que cela n’est pas vraiment possible avec c#, nous avons dû mapper cet idiome dans un idiome qui fonctionne avec c#.

La manière qu’il est mappé vers c# est par ajout de la méthode getter et setter sur la classe de base, mais le marquage de la méthode setter avec un `[NotImplemented]` attribut.

Ensuite, sur la sous-classe mutable, vous utilisez la `[Override]` attribut sur la propriété pour vous assurer que la propriété est en réalité substitution du comportement du parent.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

 <a name="Binding_Constructors" />

### <a name="binding-constructors"></a>Constructeurs de liaison

Le **btouch natif** outil génère automatiquement des constructeurs de quotidiennes dans votre classe, pour une classe donnée `Foo`, il génère :

-  `Foo ()`: le constructeur par défaut (qui correspond à un constructeur de « init » du Objective-C)
-  `Foo (NSCoder)`: le constructeur utilisé lors de la désérialisation de fichiers NIB (mappe à Objective-C » initWithCoder : « constructeur).
-  `Foo (IntPtr handle)`: le constructeur pour la création de handle, elle est appelée par le runtime lorsque le runtime doit exposer un objet managé à partir d’un objet non managé.
-  `Foo (NSEmptyFlag)`: il est utilisé par les classes dérivées pour empêcher l’initialisation double.

Pour les constructeurs que vous définissez, ils doivent être déclarés à l’aide de la signature suivante à l’intérieur de la définition d’Interface : elles doivent retourner un `IntPtr` valeur et le nom de la méthode doivent être de constructeur. Par exemple, pour lier la `initWithFrame:` constructeur, voici ce que vous utilisez :

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

 <a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Protocoles de liaison

Comme décrit dans le document de conception d’API, dans la section [traitant des modèles et des protocoles](~/ios/internals/api-design/index.md), Xamarin.iOS mappe les protocoles Objective-C dans les classes qui ont été marquées avec la [[modèle]](~/cross-platform/macios/binding/binding-types-reference.md) attribut. Cela est généralement utilisé lors de l’implémentation des classes de délégué Objective-C.

La grande différence entre une classe liée normale et une classe déléguée est que la classe déléguée peut avoir une ou plusieurs méthodes facultatives.

Examinons, par exemple le `UIKit` classe `UIAccelerometerDelegate`, voici comment elle est liée dans Xamarin.iOS :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Comme il s’agit d’une méthode facultative sur la définition de `UIAccelerometerDelegate` il n’a rien à faire. Mais s’il existe une méthode requise sur le protocole, vous devez ajouter le [[abstraite]](~/cross-platform/macios/binding/binding-types-reference.md) d’attribut à la méthode. Cela force l’utilisateur de l’implémentation pour fournir un corps pour la méthode.

En général, les protocoles sont utilisés dans les classes qui répondent aux messages. Cela est généralement effectué dans Objective-C en affectant à la propriété « délégué » d’une instance d’un objet qui répond aux méthodes dans le protocole.

La convention Xamarin.iOS est pour prendre en charge les deux le Objective-C faiblement couplés style où une instance d’un `NSObject` et peuvent être alloués au délégué, d’exposer également une version fortement typée de celui-ci. Pour cette raison, nous fournissent en général une propriété « Délégué » qui est fortement typée et un « WeakDelegate » est faiblement typé. Nous lions généralement la version faiblement typée avec l’exportation, et nous utilisons le [[Wrap]](~/cross-platform/macios/binding/binding-types-reference.md) attribut pour fournir la version fortement typée.

Cet exemple montre comment nous liée la `UIAccelerometer` classe :

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

 <a name="iOS7ProtocolSupport" />

**Nouveauté de MonoTouch 7.0**

À partir de MonoTouch 7.0, un fonctionnalité de liaison de protocole de nouveau et amélioré a été incorporée.  Cette nouvelle prise en charge rend plus simple d’utiliser les idiomes Objective-C pour arrêter un ou plusieurs protocoles dans une classe donnée.

Pour chaque définition de protocole `MyProtocol` dans Objective-C, il existe désormais une `IMyProtocol` interface qui répertorie toutes les méthodes requises du protocole, ainsi que d’une classe d’extension qui fournit toutes les méthodes facultatives.  Ci-dessus, combinée avec la nouvelle prise en charge dans l’éditeur permet aux développeurs d’implémenter des méthodes de protocole sans avoir à utiliser les sous-classes distincts des classes de modèle abstrait précédente de Xamarin Studio.

Toute définition qui contient le `[Protocol]` attribut génère en fait trois classes de prise en charge d’améliorer considérablement la façon que vous consommez des protocoles :

```csharp
// Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

Le **implémentation de la classe** fournit une classe abstraite complète que vous pouvez substituer des méthodes individuelles d’et obtenir la sécurité de type complet.  Mais en raison de c# ne prend ne pas en charge l’héritage multiple, il existe des scénarios où vous pouvez doit disposer d’une autre classe de base, mais vous souhaitez implémenter une interface, qui est l’emplacement où le

Le texte généré **définition d’interface** arrive.  Il est une interface qui possède toutes les méthodes requises du protocole.  Cela permet aux développeurs qui souhaitent mettre en œuvre votre protocole pour simplement implémenter l’interface.  Le runtime inscrira automatiquement le type en tant que l’adoption du protocole.

Notez que l’interface répertorie les méthodes requises uniquement et expose les méthodes facultatives.  Cela signifie que les classes qui adoptent le protocole obtiennent complète vérification de type pour les méthodes requises, mais devront recourir au typage faible (manuellement à l’aide des attributs d’exportation et correspondre à la signature) pour les méthodes de protocole facultatifs.

Pour le rendre plus commode de consommez une API qui utilise les protocoles, l’outil de liaison également génère une classe de méthode d’extensions qui expose toutes les méthodes facultatives.  Cela signifie que tant que vous consommez une API, vous serez en mesure de traiter des protocoles comme ayant toutes les méthodes.

Si vous souhaitez utiliser les définitions de protocole dans votre API, vous devrez écrire des interfaces vides squelettes dans votre définition de l’API.  Si vous souhaitez utiliser le MyProtocol dans une API, vous devez pour cela :

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

La commande ci-dessus est nécessaire, car au moment de la liaison la `IMyProtocol` n’existerait pas, autrement dit pourquoi vous devez fournir une interface vide.

#### <a name="adopting-protocol-generated-interfaces"></a>Adoption des interfaces générées par le protocole

Chaque fois que vous implémentez une des interfaces générées pour les protocoles, à ceci :

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

L’implémentation pour les méthodes d’interface obtient automatiquement exportée avec le nom approprié, par conséquent, elle est équivalente à cette :

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Si l’interface est implémentée de manière implicite ou explicite n’a pas d’importance.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Extensions de classe de liaison

<!--In Objective-C it is possible to extend classes with new methods,
similar in spirit to C#'s extension methods. When one of these methods
is present, you can use the `[Target]` attribute to flag the first
parameter of a method as being the receiver of the Objective-C
message.

For example, in Xamarin.iOS we bound the extension methods that are defined on
`NSString` when `UIKit` is imported as methods in the `UIView`, like this:

```csharp
[BaseType (typeof (UIResponder))]
interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

-->

Dans Objective-C, il est possible d’étendre des classes avec de nouvelles méthodes, même esprit que les méthodes d’extension de #. Lorsqu’une de ces méthodes est présente, vous pouvez utiliser la `BaseType` attribut pour marquer la méthode comme étant le récepteur du message Objective-C.

Par exemple, dans Xamarin.iOS nous liée qui sont définies sur les méthodes d’extension `NSString` lorsque `UIKit` est importée en tant que méthodes dans le `NSStringDrawingExtensions`, comme suit :

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

 <a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Liaison de listes d’arguments Objective-C

Objective-C prend en charge les arguments de variadiques, vous pouvez utiliser la technique suivante décrite par Zach Gris dans [ce billet](http://forums.monotouch.net/yaf_postst311_SOLVED-Binding-ObjectiveC-Argument-Lists.aspx).

Un message Objective-C ressemble à ceci :

```csharp
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Pour appeler cette méthode à partir de c#, vous devez créer une signature comme suit :

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Cela déclare la méthode comme internes, le masquage de l’API ci-dessus à partir d’utilisateurs, mais les exposer à la bibliothèque. Vous pouvez écrire une méthode comme suit :

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

 <a name="Binding_Fields" />

### <a name="binding-fields"></a>Champs de liaison

Parfois, vous devez accéder à des champs publics qui ont été déclarés dans une bibliothèque.

Ces champs contiennent généralement des valeurs de chaînes ou des entiers qui doivent être référencées. Elles sont utilisées en tant que chaîne qui représente une notification spécifique et en tant que clés dans les dictionnaires.

Pour lier un champ, ajouter une propriété à votre fichier de définition d’interface et décorer la propriété avec le [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) attribut. Cet attribut prend un seul paramètre : le nom de C du symbole à la recherche. Exemple :

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Si vous voulez encapsuler des différents champs dans une classe statique qui ne dérive pas de `NSObject`, vous pouvez utiliser la `[Static]` attribut sur la classe, comme suit :

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Ci-dessus génère une `LonelyClass` qui ne dérive pas de `NSObject` et contient une liaison à la `NSSomeEventNotification` 
 `NSString` exposées en tant qu’un `NSString`.

Le `[Field]` attribut peut être appliqué aux types de données suivants :

-  `NSString` références (uniquement pour les propriétés en lecture seule)
-  `NSArray` références (uniquement pour les propriétés en lecture seule)
-  les entiers 32 bits (`System.Int32`)
-  les entiers 64 bits (`System.Int64`)
-  valeurs en virgule flottante 32 bits (`System.Single`)
-  valeurs en virgule flottante 64 bits (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

En plus du nom de champ natif, vous pouvez spécifier le nom de la bibliothèque où se trouve le champ, en passant le nom de la bibliothèque :

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Si vous effectuez une liaison statique, il n’est aucune bibliothèque à lier, vous devez donc utiliser le `__Internal` nom :

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Énumérations de liaison

Vous pouvez ajouter `enum` directement dans votre liaison fichiers rend plus facile à utiliser au sein des définitions de l’API - sans l’aide d’un fichier source différent (qui doit être compilé dans les liaisons et le projet final).

Exemple :

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Il est également possible de créer votre propre enums remplacer `NSString` constantes. Dans ce cas le générateur est **automatiquement** créer les méthodes pour convertir les valeurs des énumérations et les constantes de NSString pour vous.

Exemple :

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

Dans l’exemple ci-dessus, vous pouvez choisir décorer `void Perform (NSString mode);` avec un `[Internal]` attribut. Ce sera **masquer** l’API basée sur une constante à partir de vos consommateurs de liaison.

Toutefois, cela limiterait sous-classement le type de l’autre qui utilise API agréable un `[Wrap]` attribut. Ces méthodes générées ne sont pas `virtual`, par exemple, vous ne pourrez remplacer les - peut, ou non, un bon choix.

Une alternative consiste à marquer l’original, `NSString`-basée, définition en tant que `[Protected]`. Cela permettra de sous-classement au travail, à la demande, et la version wrap'ed sera toujours de travail et d’appeler la méthode substituée.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Liaison NSValue, NSNumber et NSString à un type de mieux

Le [[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md) attribut autorise la liaison `NSNumber`, `NSValue` et `NSString`(enum) dans les types c# plus précis. L’attribut peut être utilisé pour créer la meilleure et plus précis, les API .NET sur l’API native.

Vous pouvez la décorer les méthodes (sur la valeur de retour), les paramètres et les propriétés avec [[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md). La seule restriction est que votre membre **ne doit pas** se trouve dans un `[Protocol]` ou `[Model]` interface.

Exemple :

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Affiche :

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

En interne, nous ferons le `bool?`  <->  `NSNumber` et `CGRect`  <->  `NSValue` conversions.

[[BindAs] ](~/cross-platform/macios/binding/binding-types-reference.md) prend également en charge les tableaux de `NSNumber` `NSValue` et `NSString`(enum).

Exemple :

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Affiche :

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` est un `NSString` sauvegardé enum, nous permet d’extraire le droit `NSString` valeur et de gérer la conversion de type.

Consultez [[BindAs] documentation](~/cross-platform/macios/binding/binding-types-reference.md) pour voir les types de conversion prises en charge.

 <a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Notifications de liaison

Les notifications sont des messages qui sont publiés dans le `NSNotificationCenter.DefaultCenter` et sont utilisés comme un mécanisme pour diffuser des messages à partir d’une partie de l’application à l’autre. Les développeurs s’abonner aux notifications en général à l’aide de la [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)de [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) (méthode). Lorsqu’une application envoie un message au centre de notification, il contient généralement une charge utile stockée dans le [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) dictionnaire. Ce dictionnaire est faiblement typé et l’obtention des informations de sortie est sujette aux erreurs, comme les utilisateurs doivent en général de lecture dans la documentation, les clés sont disponibles dans le dictionnaire et les types des valeurs qui peuvent être stockées dans le dictionnaire. La présence de clés parfois est utilisée comme une valeur booléenne également.

Le Générateur de liaison de Xamarin.iOS fournit la prise en charge pour les développeurs à lier des notifications. Pour ce faire, vous définissez la [[Notification]](~/cross-platform/macios/binding/binding-types-reference.md) attribut sur une propriété qui a été également été marquées avec un [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) propriété (il peut être public ou privé).

Cet attribut peut être utilisé sans arguments pour les notifications qui n’effectuer aucune charge utile ou vous pouvez spécifier un `System.Type` qui fait référence à une autre interface dans la définition d’API, généralement avec le nom se terminant par « EventArgs ». Le Générateur d’activer l’interface dans une classe qui sous-classe `EventArgs` et inclut toutes les propriétés proposées. Le `[Export]` attribut doit être utilisé dans la classe EventArgs pour répertorier le nom de la clé utilisée pour rechercher le dictionnaire Objective-C pour extraire la valeur.

Exemple :

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Le code ci-dessus génère une classe imbriquée `MyClass.Notifications` avec les méthodes suivantes :

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Les utilisateurs de votre code peuvent ensuite facilement s’abonner aux notifications validées à la [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) à l’aide de code similaire à celui-ci :

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

La valeur retournée à partir de `ObserveDidStart` peut être utilisé pour facilement arrêter de recevoir des notifications, comme suit :

```csharp
token.Dispose ();
```

Vous pouvez également appeler [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) et passez le jeton. Si votre notification contient des paramètres, vous devez spécifier un programme d’assistance `EventArgs` interface, comme suit :

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Ci-dessus génère une `MyScreenChangedEventArgs` classe avec le `ScreenX` et `ScreenY` propriétés qui extrait les données à partir de la [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) dictionnaire à l’aide de clés « ScreenXKey » et « ScreenYKey » respectivement et appliquer les conversions appropriées. Le `[ProbePresence]` attribut est utilisé pour le générateur pour détecter si la clé est définie le `UserInfo`, au lieu d’essayer d’extraire la valeur. Il est utilisé pour les cas où la présence de la clé est la valeur (en général pour les valeurs booléennes).

Cela vous permet d’écrire du code comme suit :

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

 <a name="Binding_Categories" />

### <a name="binding-categories"></a>Catégories de liaison

Les catégories sont un mécanisme Objective-C permettent d’étendre l’ensemble des méthodes et propriétés disponibles dans une classe.   Dans la pratique, ils sont utilisés pour étendre les fonctionnalités d’une classe de base (par exemple `NSObject`) lorsqu’une infrastructure spécifique est liée dans (par exemple `UIKit`), rendre leurs méthodes, mais uniquement si la nouvelle infrastructure est liée dans.   Dans d’autres cas, ils sont utilisés pour organiser des fonctionnalités dans une classe par la fonctionnalité.   Ils sont similaires esprit aux méthodes d’extension c#. Il s’agit d’une catégorie de l’aspect dans C: de l’objectif

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

L’exemple ci-dessus si trouvé dans une bibliothèque étendrait des instances de `UIView` avec la méthode `makeBackgroundRed`.

Pour lier les, vous pouvez utiliser la `[Category]` attribut sur une définition d’interface.  Lorsque l’aide de la catégorie de l’attribut, la signification de la `[BaseType]` attribut change d’être utilisé pour spécifier la classe de base pour étendre, le type à étendre.

Le suivant montre comment la `UIView` extensions sont liées et converties en méthodes d’extension c# :

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

La commande ci-dessus crée un `MyUIViewExtension` une classe qui contient le `MakeBackgroundRed` méthode d’extension.  Cela signifie que vous pouvez maintenant appeler « MakeBackgroundRed » sur n’importe quel `UIView` sous-classe, ce qui vous donne la même fonctionnalité que vous obtiendriez sur objectif-C. Dans d’autres cas, les catégories sont utilisées ne pas étendre une classe système, mais pour organiser des fonctionnalités, exclusivement à des fins de décoration.  Comme ceci :

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Bien que vous puissiez utiliser le `Category` attribut également pour ce style de décoration de déclarations, vous pouvez également simplement ajouter les tous à la définition de classe.  Ces deux éléments atteindre la même :

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

Il est simplement plus court dans ce cas de fusionner les catégories :

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

 <a name="Binding_Blocks" />

### <a name="binding-blocks"></a>Blocs de liaison

Les blocs sont une nouvelle construction d’introduits par Apple pour rétablir l’équivalent de méthodes anonymes c# objectif-C. Par exemple, la `NSSet` classe expose maintenant cette méthode :

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

La description ci-dessus déclare une méthode appelée «*enumerateObjectsUsingBlock :*» qui accepte un argument nommé *bloc*. Ce bloc est similaire à une méthode anonyme c#, car il a prise en charge pour la capture de l’environnement actuel (le pointeur « this », l’accès aux variables locales et les paramètres). La méthode ci-dessus dans `NSSet` appelle le bloc avec deux paramètres un `NSObject` (la partie « obj id ») et un pointeur vers une valeur booléenne (la « BOOL * arrêter ») partie.

Pour lier ce type d’API avec btouch, vous devez d’abord déclarer la signature de type de bloc que c# déléguer et ensuite le référencer à partir d’un point d’entrée d’API, comme suit :

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

Et maintenant votre code peut appeler la fonction à partir de c# :

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Vous pouvez également utiliser des expressions lambda si vous préférez :

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

 <a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Méthodes asynchrones

Le Générateur de liaison peut activer une certaine classe de méthodes dans les méthodes async conviviaux (les méthodes qui retournent une tâche ou&lt;T&gt;).

Vous pouvez utiliser la `[Async]` attribut sur les méthodes qui retournent void et dernière dont l’argument est un rappel.  Lorsque vous appliquez cette à une méthode, le Générateur de liaison génère une version de cette méthode avec le suffixe `Async`.  Si le rappel ne prend aucun paramètre, la valeur de retour sera une `Task`, si le rappel prend un paramètre, le résultat sera un `Task<T>`.  Si le rappel accepte plusieurs paramètres, vous devez définir le `ResultType` ou `ResultTypeName` pour spécifier le nom de votre choix du type généré qui contiendra toutes les propriétés.

Exemple :

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Le code ci-dessus génère à la fois la méthode LoadFile, ainsi que :

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Dans les surfaces de types forts pour les paramètres NSDictionary faibles

Dans de nombreux endroits dans l’API Objective-C, les paramètres sont passés comme faiblement typé `NSDictionary` API avec des clés et valeurs, mais ces derniers est des erreurs (vous pouvez passer des clés non valides et n’obtenir aucun avertissement ; vous pouvez passer des valeurs non valides et n’obtenir aucun avertissement) et frustrant Pour utiliser car ils nécessitent plusieurs allers-retours à la documentation pour rechercher les noms de clés possibles et les valeurs.

La solution consiste à fournir une version fortement typée qui fournit la version fortement typée de l’API et les coulisses mappe les sous-jacent clés et les différentes valeurs.

Ainsi par exemple, si l’API Objective-C accepté une `NSDictionary` et elle est décrite comme prenant la clé « XyzVolumeKey », qui prend un `NSNumber` avec une valeur comprise entre 0.0 à 1.0 et « XyzCaptionKey » qui accepte une chaîne, vous souhaiteriez vos utilisateurs d’avoir une bonne API qui ressemble à ceci :

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

Le `Volume` propriété est définie comme float nullable, comme la convention dans Objective-C ne nécessite pas de ces dictionnaires pour que la valeur, donc il existe des scénarios où la valeur ne peut pas être définie.

Pour ce faire, vous devez effectuer certaines opérations :

* Créer une classe fortement typée, qui sous-classe [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) et fournit les méthodes getter et setter différents pour chaque propriété.
* Déclarer des surcharges pour les méthodes prenant `NSDictionary` à prendre la nouvelle version fortement typée.

Vous pouvez créer la classe fortement typée soit manuellement ou utiliser le générateur pour effectuer le travail pour vous.  Tout d’abord, nous explorons comment procéder manuellement afin de comprendre ce qui se passe, puis l’approche automatique.

Vous devez créer un fichier de support pour cela, il n’aborde pas votre contrat API.  C’est ce que vous devez écrire créer votre classe XyzOptions :

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Vous devez ensuite fournir une méthode de wrapper qui expose l’API de haut niveau, au-dessus de l’API de bas niveau.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Si votre API ne doit pas être remplacé, vous pouvez masquer en toute sécurité de l’API NSDictionary à l’aide de la [interne](~/cross-platform/macios/binding/binding-types-reference.md) attribut.

Comme vous pouvez le voir, nous utilisons le `[Wrap]` d’attribut pour un nouveau point d’entrée API de surface, et nous surface à l’aide de notre classe XyzOptions fortement typée.
Il permet également la méthode de wrapper pour la valeur null à passer.

À présent, une chose que nous ne pas mentionner est l’emplacement où le `XyzOptionsKeys` provient de valeurs.  Vous devez généralement regrouper les clés qu’un surfaces d’API dans une classe statique comme XyzOptionsKeys, comme suit :

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Examinons la prise en charge automatique pour la création de ces dictionnaires fortement typées.  Cela évite tout le code, et vous pouvez définir le dictionnaire directement dans votre contrat d’API, au lieu d’utiliser un fichier externe.

Pour créer un dictionnaire fortement typé, introduire une interface dans votre API et la décorer avec le [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md) attribut.  Cela indique le générateur qu’il doit créer une classe avec le même nom que votre interface doit dériver `DictionaryContainer` et fournit des accesseurs typés fort pour celle-ci.

Le `StrongDictionary` attribut prend un paramètre, qui est le nom de la classe statique qui contient vos clés de dictionnaire.  Chaque propriété de l’interface deviennent alors un accesseur fortement typé.  Par défaut, le code utilisera le nom de la propriété avec le suffixe « Key » dans la classe statique pour créer l’accesseur.

Cela signifie que la création de votre accesseur fortement typé ne requiert plus un fichier externe, ni avoir à créer manuellement des accesseurs Get et Set pour chaque propriété, ni avoir à rechercher les clés manuellement vous-même.

Voici à quoi ressemblera votre ensemble de la liaison :

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Au cas où vous devez le référencer dans votre `XyzOption` membres un autre champ (c'est-à-dire pas le nom de la propriété avec le suffixe `Key`), vous pouvez la décorer la propriété avec un `Export` attribut avec le nom que vous souhaitez utiliser.

 <a name="Type_mappings" />

## <a name="type-mappings"></a>Mappages de types

Cette section décrit comment les types de Objective-C sont mappés aux types c#.

<a name="Simple_Types" />

### <a name="simple-types"></a>Types simples

Le tableau suivant montre comment vous devez mapper les types à partir de la Objective-C et CocoaTouch world au monde Xamarin.iOS :

<table border="1" cellpadding="1" cellspacing="1" width="80%">
      <caption> Mappages de types </caption>
      <tbody>
        <tr>
          <td>
Nom du type de objective-C </td>
          <td>
Type d’API unifiée Xamarin.iOS </td>
        </tr>
        <tr>
          <td>
BOOL, GLboolean </td>
          <td>
bool </td>
        </tr>
        <tr>
          <td>
NSInteger </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSUInteger </td>
          <td>
nuint </td>
        </tr>
        <tr>
          <td>
CFTimeInterval / NSTimeInterval </td>
          <td>
double </td>
        </tr>
        <tr>
          <td>
NSString (<a href="~/ios/internals/api-design/nsstring.md">d’autres informations sur la liaison NSString</a>) </td>
          <td>
chaîne </td>
        </tr>
        <tr>
          <td>
Char * </td>
          <td>
            <a href="~/cross-platform/macios/binding/binding-types-reference.md#plainstring"> [PlainString]</a> string </td>
        </tr>
        <tr>
          <td>
CGRect </td>
          <td>
CGRect </td>
        </tr>
        <tr>
          <td>
CGPoint </td>
          <td>
CGPoint </td>
        </tr>
        <tr>
          <td>
CGSize </td>
          <td>
CGSize </td>
        </tr>
        <tr>
          <td>
CGFloat, GLfloat </td>
          <td>
nfloat </td>
        </tr>
        <tr>
          <td>
Types de CoreFoundation (CF *) </td>
          <td>
CoreFoundation.CF* </td>
        </tr>
        <tr>
          <td>
Reflets violets </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
GLfloat </td>
          <td>
nfloat </td>
        </tr>
        <tr>
          <td>
Types de base (NS *) </td>
          <td>
Foundation.NS* </td>
        </tr>
        <tr>
          <td>
ID </td>
          <td>
Foundation.NSObject </td>
        </tr>
        <tr>
          <td>
NSGlyph </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSSize </td>
          <td>
CGSize </td>
        </tr>
        <tr>
          <td>
NSTextAlignment </td>
          <td>
UITextAlignment </td>
        </tr>
        <tr>
          <td>
SÉLECTIONNER UN DOSSIER </td>
          <td>
ObjCRuntime.Selector </td>
        </tr>
        <tr>
          <td>
dispatch_queue_t </td>
          <td>
CoreFoundation.DispatchQueue </td>
        </tr>
        <tr>
          <td>
CFTimeInterval </td>
          <td>
double </td>
        </tr>
        <tr>
          <td>
CFIndex </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSGlyph </td>
          <td>
nuint </td>
        </tr>
      </tbody>
    </table>

 <a name="Arrays" />

### <a name="arrays"></a>Tableaux

Le runtime Xamarin.iOS prend automatiquement en charge de la conversion de tableaux c# à `NSArrays` et effectuer la conversion, par exemple la méthode Objective-C imaginaire qui retourne un `NSArray` de `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Est lié à ceci :

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

L’idée est d’utiliser un tableau c# fortement typé que cela permettra l’IDE fournir l’achèvement de code approprié avec le type réel sans obliger l’utilisateur à deviner ou rechercher la documentation pour connaître le type réel des objets contenus dans le tableau.

Dans les cas où vous ne pouvez pas suivre le type réel de la plus dérivée contenus dans le tableau, vous pouvez utiliser `NSObject []` comme valeur de retour.

 <a name="Selectors" />

### <a name="selectors"></a>Sélecteurs

Sélecteurs apparaissent sur l’API Objective-C en tant que le type spécial « Libre ». Lorsque vous liez un sélecteur, vous devez mapper le type à `ObjCRuntime.Selector`.  En général, les sélecteurs sont exposés dans une API avec un objet, l’objet cible et un sélecteur pour appeler dans l’objet cible. Ces deux éléments fournissant essentiellement correspond au délégué c# : un élément qui encapsule à la fois la méthode à appeler, ainsi que l’objet pour appeler la méthode dans.

Voici à quoi ressemble la liaison :

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

Et Voici comment la méthode doit généralement être utilisée dans une application :

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

Pour rendre la liaison agréable pour les développeurs c#, vous en général fournissent une méthode qui prend un `NSAction` paramètre qui permet de délégués en c# et les expressions lambda être utilisé au lieu du `Target+Selector`. Pour ce faire, vous serez généralement masquer la méthode « SetTarget » en le marquant avec un attribut « Interne » et puis vous expose une nouvelle méthode d’assistance, comme suit :

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Maintenant, votre code utilisateur peut être écrite comme suit :

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

 <a name="Strings" />

### <a name="strings"></a>Chaînes

Lorsque vous liez une méthode qui prend un `NSString`, vous pouvez remplacer qu’avec un type de chaîne c#, à la fois sur types de paramètres et de retour.

Le seul cas lorsque vous souhaiterez peut-être utiliser un `NSString` directement est lors de la chaîne est utilisée comme un jeton. Pour plus d’informations sur les chaînes et `NSString`, lisez le [conception de l’API sur NSString](~/ios/internals/api-design/nsstring.md) document.

Dans certains cas rares, une API peut exposer une chaîne de type C (`char *`) au lieu d’une chaîne Objective-C (`NSString *`). Dans ce cas, vous pouvez annoter le paramètre avec le [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) attribut.

 <a name="outref_parameters" />

### <a name="outref-parameters"></a>out / paramètres ref

Certaines API retourne les valeurs de leurs paramètres, ou passer des paramètres par référence.

En règle générale, la signature se présente comme suit :

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

Le premier exemple montre un idiome commun de Objective-C pour retourner des codes d’erreur, un pointeur vers un `NSError` pointeur est passé, et au moment du retour, la valeur est définie.   La deuxième méthode montre comment une méthode Objective-C peut prendre un objet et modifier son contenu.   Il s’agit d’un passage par référence plutôt qu’une valeur de sortie pur.

Votre liaison ressemble à ceci :

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

 <a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Attributs de gestion de mémoire

Lorsque vous utilisez la `[Export]` attribut et que vous transmettez des données qui seront conservées par la méthode appelée, vous pouvez spécifier la sémantique de l’argument en lui passant comme second paramètre, par exemple :

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Ci-dessus serait indicateur la valeur comme étant la sémantique de « Durée de stockage ». La sémantique disponible est :

-  Affecter :
-  Copier :
-  Conserver :

 <a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Règles de style

 <a name="Using_[Internal]" />

#### <a name="using-internal"></a>À l’aide de [interne]

Vous pouvez utiliser la [[interne]](~/cross-platform/macios/binding/binding-types-reference.md) attribut pour masquer une méthode à partir de l’API publique. Vous souhaiterez faire dans les cas où le API exposée est trop bas niveau et que vous souhaitez fournir une implémentation de haut niveau dans un fichier distinct, en fonction de cette méthode.

Vous pouvez également utiliser lorsque vous exécutez des limitations dans le Générateur de liaison, par exemple certains scénarios avancés peuvent exposer des types qui ne sont pas liés et vous souhaitez lier votre propre façon, et vous voulez encapsuler ces types de votre propre manière.

 <a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Rappels et les gestionnaires d’événements

En général, les classes objective-C diffuser des notifications ou demandent des informations en envoyant un message sur une classe déléguée (délégué Objective-C).

Ce modèle, tandis qu’entièrement pris en charge et signalées par Xamarin.iOS peut parfois s’avérer lourde. Xamarin.iOS expose le modèle d’événement c# et un système de rappel de la méthode de la classe qui peut être utilisé dans ces situations. Cela permet au code comme suit pour exécuter :

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Le Générateur de liaison est capable de réduire la quantité de caractères requis pour mapper le modèle Objective-C dans le modèle c#.

Compter Xamarin.iOS 1.4, il est possible également indiquer le Générateur de produire des liaisons pour un délégués Objective-C spécifique et d’exposer le délégué en tant que propriétés sur le type d’hôte et les événements c#.

Il existe deux classes impliquées dans ce processus, de la classe d’hôte qui est celle qui émet des événements et en envoie le `Delegate` ou `WeakDelegate` et la classe déléguée réel.

Prise en compte la configuration suivante :

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

Vous devez effectuer les opérations suivantes pour encapsuler la classe :

-  Dans votre classe d’hôte, ajoutez à votre `[BaseType]` déclaration de type qui agit comme le nom de son délégué et le langage c# que vous avez exposée. Dans notre exemple ci-dessus ceux sont « typeof (MyClassDelegate) » et « WeakDelegate » respectivement.
-  Dans votre classe de délégué, sur chaque méthode qui a plus de deux paramètres, vous devez spécifier le type que vous souhaitez utiliser pour la classe EventArgs générée automatiquement.

Le Générateur de liaison n’est pas limité à wrapping uniquement une destination d’événement unique, il est possible que certaines classes Objective-C à émettre des messages à plus d’un délégué, donc vous devez fournir des tableaux pour prendre en charge de ce programme d’installation. La plupart des configurations ne le n'aurez pas besoin, mais le générateur est prêt à prendre en charge les cas.

Le code résultant sera :

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

Le `EventArgs` est utilisé pour spécifier le nom de la `EventArgs` classe à générer. Vous devez utiliser une par signature (dans cet exemple, le `EventArgs` contient une propriété « Avec » de type nint).

Avec les définitions ci-dessus, le générateur produit l’événement suivant dans la MyClass générée :

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Par conséquent, vous pouvez maintenant utiliser le code comme suit :

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Rappels sont comme les appels d’événements, la différence est que, au lieu d’avoir plusieurs abonnés potentiels (par exemple, plusieurs méthodes peuvent se raccordent à un événement « Clicked » ou un « Téléchargement fini ») rappels ne peuvent avoir qu’un seul abonné.

Le processus est identique, la seule différence est qu’au lieu d’exposer le nom de la classe EventArgs qui est générée, EventArgs réellement est utilisé pour nommer le nom du délégué c# résultant.

Si la méthode dans la classe de délégué retourne une valeur, le Générateur de liaison cela mappe à une méthode de délégué dans la classe parente à la place d’un événement. Dans ce cas, vous devez fournir la valeur par défaut qui doit être retournée par la méthode si l’utilisateur ne pas raccorder au délégué. Cela à l’aide de la `[DefaultValue]` ou `[DefaultValueFromArgument]` attributs.

DefaultValue sera coder une valeur de retour, alors que `[DefaultValueFromArgument]` est utilisée pour spécifier les arguments d’entrée sont retournées.

 <a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Énumérations et les types de base

Vous pouvez également référencer des énumérations ou des types de base qui ne sont pas directement pris en charge par le système de définition d’interface btouch. Pour ce faire, placez votre énumérations et les types de base dans un fichier distinct et les inclure dans le cadre de l’un des fichiers supplémentaires que vous fournissez à btouch.

 <a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Les dépendances de liaison

Si vous liez des API qui ne font pas partie de votre application, vous devez vous assurer que votre fichier exécutable est lié par rapport à ces bibliothèques.

Vous devez indiquer la procédure pour lier vos bibliothèques Xamarin.iOS, cela peut être effectué par la modification de votre configuration de build pour appeler la commande mtouch avec des arguments de build supplémentaires qui spécifient comment créer des liens avec les bibliothèques de nouveau à l’aide de la «-gcc_flags « option de suivi par une chaîne entre guillemets qui contient toutes les bibliothèques supplémentaires sont requis pour votre programme, comme suit :

```csharp
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

L’exemple ci-dessus établit un lien `libMyLibrary.a`, `libSystemLibrary.dylib` et `CFNetwork` bibliothèque framework dans votre fichier exécutable final.

Ou vous pouvez tirer parti de niveau de l’assembly `LinkWithAttribute`, que vous pouvez incorporer dans vos fichiers de contrat (tel que `AssemblyInfo.cs`). Lorsque vous utilisez la `LinkWithAttribute`, vous devez disposer de votre bibliothèque native au moment où vous apportez votre liaison, comme cette opération incorpore la bibliothèque native avec votre application. Exemple :

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Vous vous demandez peut-être, pourquoi avez-vous besoin de commande de « force_load », et la raison est que le ObjC - indicateur bien qu’il compile le code, il ne conserve pas les métadonnées nécessaires pour prendre en charge les catégories (supprime l’élimination de code mort de l’éditeur de liens/du compilateur il) dont vous avez besoin lors de l’exécution de Xamarin.iOS.

 <a name="Assisted_References" />

## <a name="assisted-references"></a>Références assistées

Certains objets temporaires, comme les feuilles de l’action et de zones de l’alerte sont difficiles à effectuer le suivi pour les développeurs et le Générateur de liaison permettent un peu ici.

Exemple si vous avez une classe qui a donné lieu à un message et ensuite généré un événement de « Terminé », la méthode traditionnelle de gérer cela serait :

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

Dans le scénario ci-dessus, le développeur doit conserver la référence à l’objet lui-même et soit fuite ou activement effacer la référence pour sa propre zone.  Prend en charge du code de liaison, le Générateur de suivi de la référence pour vous et clair, le cas d’une méthode spéciale est appelée, le code ci-dessus serait alors :

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Notez la façon dont il n’est plus nécessaire de conserver la variable dans une instance, qu’il fonctionne avec une variable locale et qu’il n’est pas nécessaire effacer la référence lors de la perte de l’objet.

Pour tirer parti de cela, votre classe doit avoir une propriété d’événements définie dans le `[BaseType]` déclaration et également le `KeepUntilRef` variable définie sur le nom de la méthode est appelée lorsque l’objet a terminé son travail, comme suit :

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

 <a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Héritage des protocoles

À compter de Xamarin.iOS v3.2, nous prenons en charge héritant de protocoles qui ont été marqués avec le `[Model]` propriété. Cela est utile dans certains modèles d’API, comme dans `MapKit` où le `MKOverlay` de protocole, hérite de la `MKAnnotation` de protocole et est adoptée par un nombre de classes qui héritent de `NSObject`.

Nous nécessitaient copie le protocole pour chaque implémentation, mais dans ce cas maintenant nous pouvons ont le `MKShape` classe hérite de la `MKOverlay` protocole et il génère toutes les méthodes requises automatiquement.

### <a name="related-links"></a>Liens connexes

- [Exemple de liaison](https://developer.xamarin.com/samples/BindingSample/)
