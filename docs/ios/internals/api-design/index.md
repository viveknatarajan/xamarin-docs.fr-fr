---
title: "Conception de l’API"
description: "Point de vue de la conception de l’API de Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5fab7579be256e478c69b76b5e41b8c1b0568ba6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="api-design"></a>Conception de l’API

Outre le cœur des bibliothèques de classes de Base qui font partie de Mono, [Xamarin.iOS](http://www.xamarin.com/iOS) est fourni avec des liaisons pour iOS diverses API permettant aux développeurs de créer des applications iOS native avec Mono.

Au cœur de Xamarin.iOS, il existe un interopérabilité moteur qui établit un pont entre le monde c# avec le monde Objective-C comme, ainsi que les liaisons pour iOS API C comme CoreGraphics et [OpenGLES](#OpenGLES).

Le runtime de bas niveau pour communiquer avec le code Objective-C est dans le [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Au-dessus de cette option, les liaisons pour [Foundation](#MonoTouch.Foundation), CoreFoundation et [UIKit](#MonoTouch.UIKit) sont fournies.

## <a name="design-principles"></a>Principes de conception

Voici certaines de nos principes de conception pour la liaison de Xamarin.iOS (ces également appliquent Xamarin.Mac, les liaisons Mono pour Objective-C sur OS X) :


- Suivez les instructions de conception d’infrastructure
- Permet aux développeurs de classes de sous-classe Objective-C :

  - Dériver une classe existante
  - Appelez le constructeur de base pour la chaîne
  - Méthodes de substitution doit être effectuée avec le système de remplacement de #

- Sous-classe doit fonctionner avec des constructions non standard c#
- N’exposez pas aux développeurs de sélecteurs de Objective-C
- Fournissent un mécanisme pour appeler des bibliothèques Objective-C arbitraires
- Rendre les tâches courantes Objective-C facile et dur possible de tâches Objective-C
- Exposer des propriétés de Objective-C en tant que propriétés c#
- Exposent une API fortement typée :
- Augmenter la sécurité de type
- Réduire les erreurs d’exécution
- Accès à intellisense de l’IDE sur les types de retour
- Permet de documentation de fenêtre contextuelle IDE
- Encourage l’exploration dans l’IDE de l’API :
- Types c# natifs :

    - Exemple : au lieu d’exposer le tableau faiblement typée comme suit :
        ```
        NSArray *getViews
        ```
        Nous les exposer des types forts, comme suit :
    
        ```
        NSView [] Views { get; set; }
        ```
    
    Cela permet à Visual Studio pour Mac d’effectuer la saisie semi-automatique lors de l’exploration de l’API et permet également à tous le `System.Array` opérations soient disponibles sur la valeur retournée et permet à la valeur de retour de participer à LINQ

- [NSString devient la chaîne](~/ios/internals/api-design/nsstring.md)
- Activer les paramètres de type int et uint qui aurait dû être enums en tant qu’énumérations c# et énumérations c# avec des attributs [Flags]
- Au lieu de type indépendant NSArray objets exposent des tableaux en tant que tableau fortement typé.
- Événements et les notifications, vous pouvez donner aux utilisateurs un choix entre :

    - Version fortement typée est la valeur par défaut
    - Version faiblement typée de manière anticipée

- Prend en charge le modèle de délégué Objective-C :

    - Système d’événement c#
    - Exposer des délégués c# (les expressions lambda, méthodes anonymes et System.Delegate) aux API de Objective-C en tant que « blocs »

### <a name="assemblies"></a>Assemblys

Xamarin.iOS inclut un nombre d’assemblys qui constituent le *Xamarin.iOS profil*. Le [assemblys](~/cross-platform/internals/available-assemblies.md) page comporte des informations supplémentaires.

### <a name="major-namespaces"></a>Espaces de noms de principaux 

 <a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

Le [ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) espace de noms permet aux développeurs de combler les mondes entre c# et objectif-C.
Il s’agit d’une nouvelle liaison, spécifiquement conçue pour iOS, en fonction de l’expérience à partir de/Cocoa # et Gtk #.

 <a name="MonoTouch.Foundation" />


#### <a name="foundation"></a>Foundation

Le [Foundation](https://developer.xamarin.com/api/namespace/Foundation/) espace de noms fournit les types de base de données est conçu pour interagir avec l’infrastructure Objective-C Foundation qui fait partie de l’e/s et il s’agit de la base de la programmation orientée objet dans un objectif-C.

Xamarin.iOS reflète en c#, la hiérarchie de classes à partir de l’objectif-C. Par exemple, la classe de base Objective-C [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) est utilisable à partir de c# via [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Bien que cet espace de noms fournit des liaisons pour les types Objective-C Foundation sous-jacent, dans certains cas, nous avons mappé les types sous-jacents aux types .NET. Exemple :

- Au lieu de traitement de [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) et [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), le runtime expose comme c# [chaîne](https://developer.xamarin.com/api/type/System.String/)s et fortement typées [tableau](https://developer.xamarin.com/api/type/System.Array/)s tout au long de l’API.

- API d’assistance différents sont exposés ici pour permettre aux développeurs de lier le tiers API Objective-C, autres iOS API ou des API qui n’est pas liés par Xamarin.iOS.


Pour plus d’informations sur la liaison d’API, consultez le [Générateur de liaison de Xamarin.iOS](~/cross-platform/macios/binding/binding-types-reference.md) section.

 <a name="NSObject" />


##### <a name="nsobject"></a>NSObject

Le [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) type constitue la base pour toutes les liaisons Objective-C. Types de Xamarin.iOS mise en miroir de deux classes de types à partir de l’e/s CocoaTouch APIs : les types C (en général référencés en tant que types de CoreFoundation) et les types Objective-C (ces dérivent toutes de la classe NSObject).

Pour chaque type qui reflète un type non managé, il est possible d’obtenir l’objet natif via la [gérer](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) propriété.

Mono offrent un garbage collection pour tous les objets, la `Foundation.NSObject` implémente la [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) interface. Cela signifie que de libérer explicitement les ressources de n’importe quel NSObject donné sans devoir attendre le Garbage Collector de la réunion de lancement. Ceci est important lorsque vous utilisez NSObjects importante, par exemple, les UIImages qui peut contenir des pointeurs vers les grands blocs de données.

Si votre type doit exécuter la finalisation déterministe, substituez le [NSObject.Dispose(bool) méthode](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) le paramètre à Dispose est « bool disposing », et si défini sur true elle signifie que votre méthode Dispose est appelée, car l’utilisateur appelés explicitement (suppression) sur l’objet. Si la valeur est false, cela signifie que votre méthode Dispose (bool disposing) est appelée à partir du finaliseur sur le thread finaliseur. []()

<a name="Categories" />

##### <a name="categories"></a>Categories

Compter Xamarin.iOS 8.10, il est possible de créer des catégories de Objective-C à partir de c#.

Cette opération est effectuée à l’aide de la `Category` attribut, en spécifiant le type à étendre en tant qu’argument à l’attribut. L’exemple suivant étend pour l’instance NSString.

    [Category (typeof (NSString))]

Chaque méthode de catégorie à l’aide du mécanisme normal pour l’exportation des méthodes pour Objective-C à l’aide du `Export` attribut :

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Toutes les méthodes d’extension managée doivent être statiques, mais il est possible de créer des méthodes d’instance Objective-C à l’aide de la syntaxe standard pour les méthodes d’extension dans c# :

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

et le premier argument de la méthode d’extension sera l’instance sur laquelle la méthode a été appelée.

Exemple complet :

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

Cet exemple ajoutera une méthode d’instance toUpper native à la classe NSString, qui peut être appelée à partir de l’objectif-C.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Où cela est utile dans un scénario est Ajout d’une méthode à un ensemble de classes dans votre code base, par exemple, serait alors tous les `UIViewController` instances signalent qu’ils peuvent faire pivoter :

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```

<a name="PreserveAttribute" />

##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute est un attribut personnalisé qui permet d’indiquer mtouch : l’outil de déploiement Xamarin.iOS – pour conserver un type ou un membre d’un type, pendant la phase de traitée lorsque l’application afin de réduire sa taille.

Chaque membre qui n’est pas liée de manière statique par l’application est soumise à supprimer. Par conséquent, cet attribut est utilisé pour marquer les membres qui ne sont pas statiquement référencés, mais qui sont toujours requis par votre application.

Par exemple, si vous instanciez dynamiquement des types, il pouvez que vous souhaitez conserver le constructeur par défaut de vos types. Si vous utilisez la sérialisation XML, vous souhaiterez éventuellement conserver les propriétés de vos types.

Vous pouvez appliquer cet attribut sur tous les membres d’un type, ou sur le type lui-même. Si vous souhaitez conserver l’ensemble du type, vous pouvez utiliser la syntaxe [conserver (AllMembers = true)] sur le type.

 <a name="MonoTouch.UIKit" />


#### <a name="uikit"></a>UIKit

Le [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) espace de noms contient un mappage de tous les composants d’interface utilisateur qui composent CocoaTouch sous la forme de classes c#. L’API a été modifié pour suivre les conventions utilisées dans le langage c#.

Les délégués c# sont fournies pour les opérations courantes. Consultez le [délégués](#Delegates) section pour plus d’informations.

 <a name="OpenGLES" />


#### <a name="opengles"></a>OpenGLES

Pour OpenGLES, nous distribuer un [version modifiée](https://developer.xamarin.com/api/namespace/OpenTK/) de la [OpenTK](http://www.opentk.com/) API, une liaison et orienté objet pour OpenGL qui a été modifié pour utiliser les structures et les types de données CoreGraphics, mais aussi exposer uniquement les fonctionnalité qui est disponible sur iOS.

Fonctionnalités de OpenGLES 1.1 sont disponibles dans le type ES11.GL, documenté [ici](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) type.

La fonctionnalité OpenGLES 2.0 est disponible via le type ES20.GL documenté [ici](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) type.

Fonctionnalité OpenGLES 3.0 est disponible via le type ES30.GL documenté [ici](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) type.

 <a name="Binding_Design" />


### <a name="binding-design"></a>Conception de la liaison

Xamarin.iOS n’est pas simplement une liaison à la plateforme sous-jacente Objective-C. Il étend le système de type .NET et système de distribution pour une meilleure blend c# et objectif-C.

Tout comme les P/Invoke est un outil utile pour appeler des bibliothèques natives sur Windows et Linux, ou en tant que IJW prise en charge peut être utilisé pour l’interopérabilité COM sur Windows, Xamarin.iOS étend le runtime pour prendre en charge des objets de liaison c# pour les objets Objective-C.

La discussion dans la prochaine certaines sections n’est pas nécessaire pour les utilisateurs qui créent des applications de Xamarin.iOS, mais seront d’aider les développeurs à comprendre comment les opérations sont effectuées et aideront lors de la création d’applications plus complexes.


 <a name="Types" />


#### <a name="types"></a>Types

Où il était logique, les types c# sont exposés au lieu de bas niveau types Foundation, à l’univers c#.  Cela signifie que [l’API utilise le type « string » c# au lieu de NSString](~/ios/internals/api-design/nsstring.md) et il utilise des tableaux C# fortement typées au lieu d’exposition NSArray.

En général, dans la conception de Xamarin.iOS et Xamarin.Mac, sous-jacent `NSArray` objet n’est pas exposé. Au lieu de cela, le runtime convertit automatiquement `NSArray`s pour les tableaux fortement typées de certains `NSObject` classe. Par conséquent, Xamarin.iOS n’expose pas une méthode faiblement typée comme GetViews pour retourner une NSArray :

```csharp
NSArray GetViews ();
```

Au lieu de cela, la liaison expose une valeur de retour fortement typée, comme suit :

```csharp
UIView [] GetViews ();
```

Il existe quelques exemples de méthodes exposées dans `NSArray`, pour les cas extrêmes, vous pouvez souhaiter utiliser un `NSArray` directement, mais leur utilisation est déconseillée dans la liaison d’API.

En outre, dans le **Classic API** au lieu de l’exposition `CGRect`, `CGPoint` et `CGSize` à partir de l’API CoreGraphics, nous avons remplacé ceux dont le `System.Drawing` implémentations `RectangleF`, `PointF`et `SizeF` comme ils peut vous aider aux développeurs conservent le code OpenGL existant qui utilise OpenTK. Lors de l’utilisation de la nouvelle 64 bits **API unifiée**, l’API CoreGraphics doit être utilisé.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Héritage

La conception de l’API de Xamarin.iOS permet aux développeurs d’étendre les types natifs Objective-C de la même façon qu’ils sont étend d’un type c#, à l’aide du mot clé « substitution » sur une classe dérivée, ainsi que le chaînage des propriétés l’implémentation de base à l’aide du mot clé c# « base ».

Cette conception permet aux développeurs d’éviter d’être confronté à des sélecteurs de Objective-C dans le cadre de leurs processus de développement, car l’ensemble du système Objective-C est déjà encapsulé dans les bibliothèques Xamarin.iOS.

 <a name="Types_and_Interface_Builder" />


#### <a name="types-and-interface-builder"></a>Types et le constructeur d’Interface

Lorsque vous créez des classes .NET qui sont des instances de types créés par le Générateur de l’Interface, vous devez fournir un constructeur qui accepte un seul `IntPtr` paramètre.
Cela est nécessaire pour lier l’instance de l’objet managé avec l’objet non managé.
Le code se compose d’une seule ligne, comme suit :

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```


 <a name="Delegates" />


#### <a name="delegates"></a>Délégués

Objective-C et c# ont des significations différentes pour le délégué word dans chaque langue.

Dans le monde Objective-C et dans la documentation que vous trouverez en ligne sur CocoaTouch, un délégué est généralement une instance d’une classe qui répondra à un ensemble de méthodes. Cela est très similaire à une interface c#, à la différence près que les méthodes ne sont pas toujours obligatoires.

Ces délégués jouent un rôle important dans UIKit et d’autres APIs CocoaTouch. Elles sont utilisées pour effectuer diverses tâches :

-  Pour fournir des notifications à votre code (semblable à la remise d’événement en c# ou Gtk +).
-  Pour implémenter des modèles pour les contrôles de visualisation de données.
-  Le comportement d’un contrôle de lecteur.


Le modèle de programmation a été conçu pour réduire la création de classes dérivées de modifier le comportement d’un contrôle. Cette solution est la même esprit autres kits de ressources de l’interface graphique utilisateur qui ont effectué au fil des années : Gtk signaux, emplacements de Qt, événements de Windows Forms, WPF/Silverlight et ainsi de suite. Pour éviter les centaines d’interfaces (une pour chaque action) ou l’exigence d’aux développeurs d’implémenter des méthodes trop, qu'ils n’ont pas besoin, Objective-C prend en charge les définitions de méthode facultatif. Cela est différent de celui des interfaces c# qui nécessitent toutes les méthodes à implémenter.

Dans les classes Objective-C, vous verrez que les classes qui utilisent ce modèle de programmation exposent une propriété, généralement appelée `delegate`, ce qui est nécessaire pour implémenter les parties obligatoires de l’interface et les parties de zéro ou plusieurs des facultatif.

Dans Xamarin.iOS trois mécanismes s’excluent mutuellement pour lier à ces délégués sont proposés :

1.  [Via des événements](#Events) .
2.  [Fortement typé via un `Delegate`propriété](#StrongDelegate) .
3.  [Faiblement typé un `WeakDelegate`propriété](#WeakDelegate) .


Par exemple, considérez la [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) classe. Il distribue à un [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) instance, ce qui est affectée à la [déléguer](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) propriété.

 <a name="Via_Events" />


##### <a name="via-events"></a>Via des événements

Pour de nombreux types, Xamarin.iOS crée automatiquement un délégué approprié qui transfère le `UIWebViewDelegate` appels sur les événements c#. Pour `UIWebView`:

-  Le [webViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) méthode mappée à la [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) événement.
-  Le [webViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) méthode mappée à la [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) événement.
-  Le [webView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) méthode mappée à la [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) événement.


Par exemple, ce programme simple enregistre les heures de début et de fin lorsque le chargement d’un site web d’affichage :

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

 <a name="Via_Properties" />


##### <a name="via-properties"></a>Propriétés de via

Les événements sont utiles lorsqu’il peut y avoir plusieurs abonnés à l’événement. En outre, les événements sont limités aux cas où il n’existe aucune valeur de retour à partir du code.

Pour les cas où le code est attendu pour retourner une valeur, nous avons choisi à la place pour les propriétés. Cela signifie que seule méthode peut être définie à un moment donné dans un objet.

Par exemple, vous pouvez utiliser ce mécanisme pour faire disparaître le clavier à l’écran sur le gestionnaire pour un `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

Le `UITextField`de `ShouldReturn` propriété dans ce cas prend comme argument un délégué qui retourne une valeur booléenne et détermine si l’objet TextField doit faire quelque chose avec le comportement du bouton retour. Dans notre méthode, nous renvoyons *true* à l’appelant, mais nous supprimer également le clavier de l’écran (cela se produit lorsque l’objet textfield appelle `ResignFirstResponder`).


##### <a name="strongly-typed-via-a-delegate-property"></a>Fortement typées via une propriété de délégué

Si vous préférez ne doit ne pas utiliser des événements, vous pouvez fournir votre propre [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) sous-classe et assignez-la à la [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) propriété. Une fois que UIWebView.Delegate a été affectée, le mécanisme de dispatch d’événement UIWebView ne fonctionnera plus et les méthodes UIWebViewDelegate seront appelés lorsque les événements correspondants se produisent.

Par exemple, ce type simple enregistre le temps que nécessaire pour charger un affichage web :

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

Dans le code ci-dessus est utilisé :

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

La commande ci-dessus crée un UIWebViewer et demandera à envoyer des messages à une instance de notification, une classe que vous avez créée pour répondre aux messages.

Ce modèle est également utilisé pour contrôler le comportement de certains contrôles, par exemple dans le cas UIWebView, le [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) propriété permet la `UIWebView` instance au contrôle si le `UIWebView` chargera un page ou non.

Le modèle est également utilisé pour fournir les données à la demande pour des contrôles. Par exemple, le [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) contrôle est un contrôle de rendu de la table puissant, et l’apparence et le contenu est dues à une instance d’un [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)


@### Faiblement typée via la propriété WeakDelegate

En plus de la propriété fortement typée, il existe également un délégué typé faible qui permet au développeur de lier les choses différemment si vous le souhaitez.
Everywhere fortement typé `Delegate` est exposée dans Xamarin.iOS liaison correspondante `WeakDelegate` propriété est également exposée.

Lorsque vous utilisez la `WeakDelegate`, vous êtes responsable de la décoration correctement de votre classe à l’aide de la [exporter](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) attribut pour spécifier le sélecteur. Exemple :

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

Notez qu’une fois la `WeakDelegate` propriété a été affectée, le `Delegate` propriété ne sera pas utilisée. En outre, si vous implémentez la méthode dans une classe de base héritée que vous souhaitez [exportation], vous devez configurer une méthode publique.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Mappage du modèle délégué Objective-C à C &#35;

Lorsque vous consultez des exemples de Objective-C qui ressemblent à ceci :

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Cela indique la langue pour créer et construire une instance de la classe « SomethingDelegate » et affecter la valeur à la propriété de délégué de la variable foo. Ce mécanisme est pris en charge par Xamarin.iOS et c#, la syntaxe est :

```csharp
foo.Delegate = new SomethingDelegate ();
```

Dans Xamarin.iOS, nous avons fourni des classes fortement typées qui correspondent à la Objective-C les classes déléguées. Pour les utiliser, vous utiliserez le sous-classement et substituer les méthodes définies par la mise en œuvre de Xamarin.iOS. Pour plus d’informations sur leur fonctionnement, consultez les section « modèles » ci-dessous.


##### <a name="mapping-delegates-to-c35"></a>Mappage des délégués à C &#35;

UIKit utilise en général les délégués Objective-C sous deux formes.

La première forme fournit une interface pour les modèles d’un composant. Par exemple, comme un mécanisme pour fournir des données à la demande pour une vue, telles que la fonctionnalité de stockage de données pour un affichage de liste.  Dans ce cas, vous devez toujours créer une instance de la classe appropriée et affectez la variable.

Dans l’exemple suivant, nous fournissons les `UIPickerView` avec une implémentation d’un modèle qui utilise des chaînes :

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

La deuxième forme est de fournir la notification des événements. Dans ce cas, bien que nous exposons toujours l’API sous la forme décrite ci-dessus, nous fournissons également événements c#, qui doivent être plus simple à utiliser pour les opérations rapides et intégrée aux délégués anonymes et les expressions lambda en c#.

Par exemple, vous pouvez vous abonner à `UIAccelerometer` événements :

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Les deux options sont disponibles lorsqu’ils ont un sens, mais en tant que programmeur, vous devez choisir un ou l’autre. Si vous créez votre propre instance d’un répondeur/délégué fortement typé et affectez, les événements c# ne seront pas fonctionnels. Si vous utilisez les événements c#, les méthodes dans votre classe de récepteur ou le délégué ne seront jamais appelées.

L’exemple précédent utilisé `UIWebView` peuvent être écrites à l’aide d’expressions lambda de c# 3.0 comme suit :

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Réponse aux événements

Dans le code Objective-C, parfois, les gestionnaires d’événements pour plusieurs contrôles et les fournisseurs d’informations pour plusieurs contrôles, sera hébergée dans la même classe. Cela est possible, car les classes de répondent aux messages, et tant que classes répondent aux messages, il est possible de lier les objets.

Comme indiqué précédemment, Xamarin.iOS prend en charge les deux le c# basé sur événement modèle de programmation et le modèle délégué Objective-C, où vous pouvez créer une nouvelle classe qui implémente le délégué et substitue les méthodes de votre choisis.

Il est également possible de prendre en charge les modèle de Objective-C où des répondeurs pour plusieurs opérations différents sont tous hébergés dans la même instance d’une classe. Pour ce faire Cependant, vous devez utiliser les fonctionnalités de bas niveau de la liaison Xamarin.iOS.

Par exemple, si vous souhaitez que votre classe pour répondre à la fois à la `UITextFieldDelegate.textFieldShouldClear`: message et le `UIWebViewDelegate.webViewDidStartLoad`: dans la même instance d’une classe, vous devez utiliser la déclaration d’attribut [Exporter] :

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

Les noms c# pour les méthodes ne sont pas importants ; tout ce qui vous intéressent sont les chaînes passées à l’attribut [Exporter].

Lorsque vous utilisez ce style de programmation, assurez-vous que les paramètres c# correspondent aux types réels qui passe par le moteur d’exécution.


#### <a name="models"></a>Modèles

Dans les installations de stockage UIKit, ou dans des répondeurs qui sont implémentées à l’aide des classes d’assistance, ceux-ci sont généralement appelés dans le code Objective-C en tant que délégués, et elles sont implémentées comme des protocoles.

Les protocoles objective-C ressemblent à des interfaces, mais ils prennent en charge les méthodes facultatives : autrement dit, toutes les méthodes besoin d’être implémentée pour le protocole à utiliser.

Il existe deux façons d’implémentation d’un modèle. Vous pouvez implémenter manuellement ou utiliser les définitions fortement typées existantes.


Le mécanisme manuels est nécessaire lorsque vous essayez d’implémenter une classe qui n’a pas été liée par Xamarin.iOS. Il est très facile à suivre :

-  Indicateur de votre classe d’enregistrement avec le runtime
-  Appliquer l’attribut [Exporter] avec le nom de sélecteur réelle sur chaque méthode que vous souhaitez remplacer
-  Instancier la classe et lui passer.

Par exemple, les éléments suivants implémentent uniquement une des méthodes facultatif dans la définition du protocole UIApplicationDelegate :

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Le nom de sélecteur Objective-C (« applicationDidFinishLaunching : ») est déclarée avec l’attribut de l’exportation et la classe est enregistrée avec le `[Register]` attribut.

Xamarin.iOS fournit fortement typées déclarations, prêt à l’emploi, qui ne nécessitent pas de liaison manuelle. Pour prendre en charge ce modèle de programmation, le runtime Xamarin.iOS prend en charge l’attribut [modèle] sur une déclaration de classe. Cela informe le runtime qu’il doit rattachez pas toutes les méthodes dans la classe, à moins que les méthodes sont est implémenté de façon explicite.

Cela signifie que dans UIKit, les classes qui représentent un protocole avec les méthodes facultatives sont écrites comme suit :

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

Lorsque vous souhaitez implémenter un modèle qui implémente uniquement certaines des méthodes, vous devez effectuer qu’à substituer les méthodes que vous êtes intéressé par et ignorez les autres méthodes. L’exécution va se raccorder uniquement les méthodes remplacées, pas les méthodes d’origine au monde Objective-C.

L’équivalent à l’exemple manuelle précédente est la suivante :

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Les avantages sont qu’il n’est pas nécessaire d’examiner les fichiers d’en-tête Objective-C pour trouver le sélecteur, les types d’arguments, ou le mappage en c#, et que vous obtenez intellisense dans Visual Studio pour Mac, ainsi que des types forts


#### <a name="xib-outlets-and-c35"></a>XIB prises et C &#35;

> [!IMPORTANT]
> Cette section explique l’intégration d’IDE avec prises lors de l’utilisation de fichiers XIB. Lorsque vous utilisez le Concepteur de Xamarin pour iOS, cela est tous remplacé en entrant un nom sous **identité > nom** dans la section des propriétés de votre IDE, comme indiqué ci-dessous :
>
> [![](images/designeroutlet.png "Entrer un nom d’élément dans le concepteur iOS")](images/designeroutlet.png)
>
>Pour plus d’informations sur le concepteur iOS, passez en revue les [présentation dans le concepteur iOS](~/ios/user-interface/designer/introduction.md#how-it-works) document.

Ceci est une description de bas niveau de comment intégrant des prises avec c# et est fournie pour les utilisateurs expérimentés de Xamarin.iOS. Lorsque l’aide de Visual Studio pour Mac le mappage s’effectue automatiquement en arrière-plan à l’aide, code sur le vol est généré pour vous.

Lorsque vous concevez votre interface utilisateur avec le Générateur de l’Interface, vous devrez créer uniquement l’aspect de l’application et établirez des connexions par défaut. Si vous souhaitez extraire des informations par programme, altérer le comportement d’un contrôle à l’exécution ou modifier le contrôle à l’exécution, il est nécessaire de lier des contrôles à votre code managé.

Cette opération s’effectue en quelques étapes :

1.  Ajouter le **déclaration de sortie** à votre **propriétaire du fichier**.
1.  Se connecter à votre contrôle le **propriétaire du fichier**.
1.  Stocker l’interface utilisateur ainsi que les connexions dans votre fichier XIB/NIB.
1.  Charger le fichier NIB lors de l’exécution.
1.  Accéder à la variable de sortie.


Les étapes (1) à (3) sont décrites dans la documentation d’Apple pour la création d’interfaces avec le Générateur de l’Interface.

Lorsque vous utilisez Xamarin.iOS, votre application doit créer une classe qui dérive de UIViewController. Il est implémenté, comme ceci :

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Puis pour charger votre ViewController à partir d’un fichier NIB, vous procédez comme suit :

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Il charge l’interface utilisateur à partir de la plume. Maintenant, pour accéder aux prises, il est nécessaire informer le runtime que nous souhaitons pour y accéder. Pour ce faire, le `UIViewController` sous-classe doit déclarer les propriétés et les annoter avec l’attribut [connexion]. Comme ceci :

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

L’implémentation de la propriété est celui qui extrait et stocke la valeur pour le type natif réel.

Vous n’avez pas besoin en inquiéter lors de l’utilisation de Visual Studio pour Mac et InterfaceBuilder. Visual Studio pour Mac reflète automatiquement toutes les prises déclarés avec le code dans une classe partielle qui est compilée en tant que partie de votre projet.

#### <a name="selectors"></a>Sélecteurs

Un concept fondamental de la programmation de Objective-C est sélecteurs. Souvent, vous serez probablement sur les API qui vous obligent à passer un sélecteur ou attend votre code pour répondre à un sélecteur.

Création de nouveaux sélecteurs en c# est très facile : vous créez simplement une nouvelle instance de la `ObjCRuntime.Selector` classe et d’utiliser le résultat dans n’importe où dans l’API qui le requiert. Exemple :

```csharp
var selector_add = new Selector ("add:plus:");
```

Pour un c# méthode en réponse à un appel de sélecteur, il doit hériter de la `NSObject` type et la méthode c# doivent être décorées avec le nom de sélecteur avec la `[Export]` attribut. Exemple :

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Notez ce sélecteur noms **doit** correspondent exactement, y compris tous les signes deux-points d’intermédiaires et de fin (« : »), le cas échéant.

#### <a name="nsobject-constructors"></a>Constructeurs NSObject

La plupart des classes dans Xamarin.iOS qui dérivent `NSObject` exposera constructeurs spécifiques à la fonctionnalité de l’objet, mais ils seront également exposer plusieurs constructeurs qui ne sont pas immédiatement évidents.

Les constructeurs sont utilisés comme suit :

```csharp
public Foo (IntPtr handle)
```

Ce constructeur est utilisé pour instancier votre classe lorsque le runtime a besoin de mappage de votre classe à une classe non managée. Cela se produit lorsque vous chargez un fichier XIB/NIB.  À ce stade, le runtime Objective-C crée un objet dans l’environnement non managé, et ce constructeur sera appelé pour initialiser le côté managé.

En règle générale, vous devez simplement est appeler le constructeur de base avec le paramètre de la poignée et dans le corps, effectuer une initialisation n’est nécessaire.

```csharp
public Foo ()
```

Il s’agit du constructeur par défaut pour une classe, et dans Xamarin.iOS fourni des classes, il initialise la classe Foundation.NSObject et toutes les classes d’entre et à la fin, cela remonte à l’Objective-C `init` méthode sur la classe.

```csharp
public Foo (NSObjectFlag x)
```

Ce constructeur est utilisé pour initialiser l’instance, mais que le code appelant la méthode de « init » Objective-C à la fin. Est généralement utilisée lorsque vous avez déjà enregistré pour l’initialisation (lorsque vous utilisez `[Export]` sur votre constructeur) ou lorsque l’initialisation de votre via un autre moyen n’est déjà fait.

```csharp
public Foo (NSCoder coder)
```

Ce constructeur est fourni pour les cas où l’objet est initialisé à partir d’une instance NSCoding. Pour plus d’informations, consultez Apple [Archives et Guide de programmation de sérialisation.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Exceptions

La conception de l’API de Xamarin.iOS ne déclenche pas d’exceptions de Objective-C en tant qu’exceptions c#. La conception impose qu’aucun garbage ne soit envoyé au monde Objective-C en premier lieu, et que toutes les exceptions doivent être produites sont fabriquées par la liaison elle-même avant jamais des données non valides passées au monde Objective-C.

#### <a name="notifications"></a>Notifications

Dans iOS et OS X, les développeurs peuvent s’abonner aux notifications sont diffusées par la plateforme sous-jacente. Il incombe à l’aide de la `NSNotificationCenter.DefaultCenter.AddObserver` (méthode). Le `AddObserver` méthode accepte deux paramètres ; un est la notification que vous souhaitez vous abonner à l’autre est la méthode à appeler lorsque la notification est déclenchée.

Dans Xamarin.iOS et Xamarin.Mac, les clés pour les différentes notifications sont hébergés sur la classe qui déclenche les notifications. Par exemple, les notifications déclenché par le `UIMenuController` sont hébergés en tant que `static NSString` propriétés dans le `UIMenuController` les classes qui se terminent par le nom « Notification ».

### <a name="memory-management"></a>Gestion de la mémoire

Xamarin.iOS a un garbage collector qui s’occupe de libération des ressources pour vous quand ils ne sont plus en cours d’utilisation. Outre le garbage collector, tous les objets qui dérivent de `NSObject` implémenter la `System.IDisposable` interface.

#### <a name="nsobject-and-idisposable"></a>NSObject et IDisposable

Exposer le `IDisposable` interface est un moyen pratique d’aider les développeurs dans la libération d’objets qui peuvent encapsuler les grands blocs de mémoire (par exemple, un `UIImage` peut se présenter comme un pointeur inoffensif, mais peut pointer vers une image de 2 mégaoctets ) et d’autres ressources importantes et limitées (par exemple, une mémoire tampon de décodage vidéo).

NSObject implémente l’interface IDisposable et également le [modèle Dispose de .NET](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx). Cela permet aux développeurs qui sous-classe NSObject pour substituer le comportement de suppression et de libérer leurs ressources à la demande. Par exemple, considérez ce contrôleur d’affichage qui conserve un ensemble d’images :

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

Lorsqu’un objet managé est supprimé, il n’est plus utile. Il peut toutefois être une référence aux objets, mais l’objet est par tous les cas non valide à ce stade. Certaines API .NET Vérifiez cela en levant une exception ObjectDisposedException si vous tentez d’accéder aux méthodes sur un objet supprimé, par exemple :

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Même si vous pouvez toujours accéder à la variable « image », il est réellement une référence non valide et n’est plus pointe vers l’objet Objective-C possédant l’image.

Mais la suppression d’un objet en c# ne signifie pas que l’objet sera nécessairement être détruit. Il suffit de libérer la référence ayant c# à l’objet. Il est possible que l’environnement/Cocoa peut conserver une référence pour sa propre utilisation. Par exemple, si vous définissez les propriétés de l’Image d’un UIImageView à une image, et puis de la suppression de l’image, le UIImageView sous-jacente avaient eu sa propre référence et conserve une référence à cet objet jusqu'à ce qu’il est terminé à l’utiliser.

#### <a name="when-to-call-dispose"></a>Quand il doit appeler la méthode Dispose

Vous devez appeler Dispose lorsque vous avez besoin Mono dans débarrasser de votre objet. Un cas d’utilisation possible est lorsque Mono n’a aucune connaissance que votre NSObject réellement maintient une référence à une ressource importante, telles que la mémoire ou un pool d’informations. Dans ce cas, vous devez appeler la méthode Dispose pour libérer immédiatement la référence à la mémoire, au lieu d’attendre Mono effectuer un cycle de garbage collection.

Crée en interne, lorsque Mono [NSString fait référence à partir de chaînes C#](~/ios/internals/api-design/nsstring.md), il supprime immédiatement afin de réduire la quantité de travail que le garbage collector doit effectuer. Les objets moins environ à gérer, plus le garbage collector seront exécute.

#### <a name="when-to-keep-references-to-objects"></a>Lorsque conserver des références aux objets

Un effet secondaire qui a de la gestion automatique de la mémoire est que le garbage collector permet de supprimer les objets non utilisés tant qu’il n’existe aucune référence à ceux-ci. Cela parfois peut avoir des effets étonnants, par exemple, si vous créez une variable locale pour stocker votre contrôleur de l’affichage de niveau supérieur, ou votre fenêtre de niveau supérieur et celles dont disparaissent derrière votre sauvegarde.

Si vous ne conservez pas une référence dans votre statiques ou des variables d’instance à vos objets, Mono heureusement appelle la méthode Dispose() sur ces derniers et libérer la référence à l’objet. Étant donné que cela peut être la référence uniquement en attente, le runtime Objective-C détruira l’objet pour vous.

## <a name="related-links"></a>Liens associés

- [Champs de liaison](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
