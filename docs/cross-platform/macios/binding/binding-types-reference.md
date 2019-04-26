---
title: Guide de référence des types de liaison
description: Ce guide de référence décrit les différents attributs et les concepts qui sont nécessaires pour comprendre lors de la création C# liaisons aux bibliothèques Objective-C.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: d460bf867ce09e614be76d0a4a7ffef01420cf82
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61266503"
---
# <a name="binding-types-reference-guide"></a>Guide de référence des types de liaison

Ce document décrit la liste des attributs que vous pouvez utiliser pour annoter vos fichiers de contrat d’API pour susciter de la liaison et le code généré

Xamarin.iOS et Xamarin.Mac API contrats sont écrits en C# principalement comme les définitions d’interface qui définissent la façon dont le code Objective-C est surfacé pour C#. Le processus implique un mélange de déclarations d’interface ainsi que des définitions de type de base qui le contrat d’API peut nécessiter. Pour une introduction aux types de liaison, consultez notre guide d’accompagnement [liaison Objective-C bibliothèques](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Définitions de type

Syntaxe :

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Chaque interface dans votre définition de contrat qui a le [ `[BaseType]` ](#BaseTypeAttribute) attribut qui déclare le type de base pour l’objet généré. Dans la déclaration ci-dessus un `MyType` classe C# type va être généré que lie à un type Objective-C appelée `MyType`.

Si vous spécifiez tous les types après le nom de type (dans l’exemple ci-dessus `Protocol1` et `Protocol2`) à l’aide de la syntaxe de l’héritage d’interface du contenu de ces interfaces va être inline comme s’ils avaient été partie du contrat pour `MyType`.
La façon dont les surfaces de Xamarin.iOS qu’un type adopte un protocole en incorporant tous les des méthodes et propriétés qui ont été déclarées dans le protocole dans le type lui-même.

Le suivant montre comment la déclaration Objective-C pour `UITextField` serait définie dans un contrat de Xamarin.iOS :

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Doit être écrite comme suit en tant qu’un C# contrat d’API :

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Vous pouvez contrôler les autres aspects de la génération de code en appliquant des autres attributs à l’interface, ainsi que la configuration du [ `[BaseType]` ](#BaseTypeAttribute) attribut.


### <a name="generating-events"></a>Génération d’événements

Une fonctionnalité de la conception de Xamarin.iOS et Xamarin.Mac API est que nous mappons Objective-C des classes de délégué en tant que C# événements et rappels. Les utilisateurs peuvent choisir dans chaque instance s’ils souhaitent adopter le modèle de programmation Objective-C, en affectant aux propriétés telles que `Delegate` une instance d’une classe qui implémente les différentes méthodes que vous appelez le runtime Objective-C, ou par en choisissant le C#-événements et les propriétés de style.

Nous voir un exemple montrant comment utiliser le modèle Objective-C :

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

Dans l’exemple ci-dessus, vous pouvez voir que nous avons choisi de remplacer les deux méthodes, une notification qu’un événement de défilement a eu lieu, le second qui est un rappel qui doit retourner une valeur booléenne indiquant le `scrollView` si elle doit faire défiler vers le Top ou non.

Le C# modèle permet à l’utilisateur de votre bibliothèque écouter les notifications à l’aide de la C# syntaxe de l’événement ou la syntaxe de propriété pour raccorder des rappels qui sont supposées retourner des valeurs.

Voici comment la C# de code pour la même fonctionnalité ressemble à l’aide d’expressions lambda :

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Dans la mesure où les événements ne retournent pas de valeurs (elles ont un type de retour void), vous pouvez connecter plusieurs copies. Le `ShouldScrollToTop` n’est pas un événement, il s’agit à la place une propriété avec le type `UIScrollViewCondition` qui possède cette signature :

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Elle retourne un `bool` valeur, dans ce cas la syntaxe lambda permet de simplement retourner la valeur de la `MakeDecision` (fonction).

Le Générateur de liaison prend en charge la génération des événements et des propriétés qui lient une classe telle que `UIScrollView` avec son `UIScrollViewDelegate` (également appelons la classe de modèle), cela en annotant votre [ `[BaseType]` ](#BaseTypeAttribute) définition avec le `Events` et `Delegates` paramètres (décrits ci-dessous). En plus d’annoter le [ `[BaseType]` ](#BaseTypeAttribute) avec ces paramètres, il est nécessaire informer le Générateur de quelques composants plus.

Pour les événements qui prennent plusieurs paramètres (en Objective-C la convention est que le premier paramètre dans une classe déléguée est l’instance de l’objet expéditeur) vous devez fournir le nom que vous aimeriez pour généré `EventArgs` classe soit. Cette opération est effectuée avec la [ `[EventArgs]` ](#EventArgsAttribute) attribut sur la déclaration de méthode dans votre classe de modèle. Exemple :

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

La déclaration ci-dessus générera un `UIImagePickerImagePickedEventArgs` classe qui dérive de `EventArgs` et compresse les deux paramètres, le `UIImage` et le `NSDictionary`. Le générateur produit ceci :

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Il présente ensuite les éléments suivants dans le `UIImagePickerController` classe :

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Méthodes qui retournent une valeur du modèle sont liés différemment. Celles-ci nécessitent à la fois un nom pour le texte généré C# délégué (la signature de la méthode) et également une valeur par défaut à retourner dans le cas où l’utilisateur ne fournit pas une implémentation lui-même. Par exemple, le `ShouldScrollToTop` définition s’agit-il :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

La méthode ci-dessus créera un `UIScrollViewCondition` délégué avec la signature qui a été indiqué ci-dessus, et si l’utilisateur ne fournit pas une implémentation, la valeur de retour sera true.

Outre le [ `[DefaultValue]` ](#DefaultValueAttribute) attribut, vous pouvez également utiliser le [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) attribut qui dirige le générateur pour retourner la valeur du paramètre spécifié dans l’appel ou le [ `[NoDefaultValue]` ](#NoDefaultValueAttribute) paramètre qui indique le générateur qu’il n’existe aucune valeur par défaut.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>BaseTypeAttribute

Syntaxe :

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Vous utilisez le `Name` propriété pour contrôler le nom auquel ce type sera lié dans le monde Objective-C. Cela est généralement utilisé pour donner le C# tapez un nom qui est compatible avec les règles de conception de .NET Framework, mais qui mappe à un nom en Objective-C qui ne respecte pas cette convention.

Exemple, dans le cas suivant, nous mappons le Objective-C `NSURLConnection` type `NSUrlConnection`, que les règles de conception de .NET Framework utilisent des « Url » au lieu de « URL » :

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Le nom spécifié est utilisé comme valeur pour le texte généré `[Register]` attribut dans la liaison. Si `Name` n’est pas spécifié, le nom du type court est utilisé comme valeur pour le `[Register]` attribut dans la sortie générée.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events et BaseType.Delegates

Ces propriétés sont utilisées pour la génération de C#-événements de style dans les classes générées. Ils sont utilisés pour lier une classe donnée avec sa classe de délégué Objective-C. Vous rencontrerez souvent où une classe utilise une classe déléguée à envoyer des notifications et des événements. Par exemple un `BarcodeScanner` aurait un accompagnement `BardodeScannerDelegate` classe. Le `BarcodeScanner` classe a généralement un `Delegate` propriété que vous devez assigner une instance de `BarcodeScannerDelegate` pour, alors que cela fonctionne, vous pouvez souhaiter exposer à vos utilisateurs un C#-comme interface d’événement de style et dans ce cas, vous utiliseriez le `Events` et `Delegates` propriétés de la [ `[BaseType]` ](#BaseTypeAttribute) attribut.

Ces propriétés sont toujours définies ensemble et doivent avoir le même nombre d’éléments et les garder synchronisées. Le `Delegates` tableau contient une chaîne pour chaque délégué faiblement typé que vous souhaitez inclure dans un wrapper, et le `Events` tableau contient un type pour chaque type que vous souhaitez lui associer.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

Si vous appliquez cet attribut lors de la création de nouvelles instances de cette classe, l’instance de cet objet sera conservé jusqu'à ce que la méthode référencée par le `KeepRefUntil` a été appelée. Cela est utile pour améliorer la facilité d’utilisation de vos API, lorsque vous ne souhaitez pas que votre utilisateur pour conserver une référence à un objet autour d’utiliser votre code. La valeur de cette propriété est le nom d’une méthode dans le `Delegate` classe, vous devez donc utiliser cela en association avec le `Events` et `Delegates` également des propriétés.

L’exemple suivant montre comment il est utilisé par `UIActionSheet` dans Xamarin.iOS :

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

Lorsque cet attribut est appliqué à la définition d’interface il empêche le Générateur de produire le constructeur par défaut.

Utilisez cet attribut lorsque vous avez besoin de l’objet à initialiser avec un des autres constructeurs dans la classe.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Lorsque cet attribut est appliqué à la définition d’interface qui signale le constructeur par défaut comme privé. Cela signifie que vous pouvez toujours instancier l’objet de cette classe en interne à partir de votre fichier d’extension, mais elles sont simplement pas plus accessibles aux utilisateurs de votre classe.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Utilisez cet attribut sur une définition de type pour lier des catégories de Objective-C et exposer en tant que C# méthodes d’extension pour mettre en miroir de la façon Objective-C expose les fonctionnalités.

Les catégories sont un mécanisme Objective-C permet d’étendre l’ensemble des méthodes et propriétés disponibles dans une classe.   Dans la pratique, ils sont utilisés pour étendre les fonctionnalités d’une classe de base (par exemple `NSObject`) quand un framework spécifique est lié dans (par exemple `UIKit`), rendre leurs méthodes, mais uniquement si la nouvelle infrastructure est liée dans.   Dans d’autres cas, ils sont utilisés pour organiser des fonctionnalités dans une classe par fonctionnalité.   Elles sont similaires à l’esprit pour C# méthodes d’extension.

Voici à quoi ressemblerait une catégorie dans Objective-c :

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

L’exemple ci-dessus se trouve sur une bibliothèque qui s’étend des instances de `UIView` avec la méthode `makeBackgroundRed`.

Pour lier les, vous pouvez utiliser la [ `[Category]` ](#CategoryAttribute) attribut sur une définition d’interface.   Lorsque vous utilisez le [ `[Category]` ](#CategoryAttribute) d’attribut, la signification de la [ `[BaseType]` ](#BaseTypeAttribute) attribut change d’être utilisé pour spécifier la classe de base pour étendre au type à étendre.

Le suivant montre comment la `UIView` extensions sont liées et transformées C# méthodes d’extension :

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

La méthode ci-dessus créera un `MyUIViewExtension` une classe qui contient le `MakeBackgroundRed` méthode d’extension.   Cela signifie que vous pouvez maintenant appeler `MakeBackgroundRed` sur n’importe quel `UIView` sous-classe, ce qui vous donne les mêmes fonctionnalités que vous obtiendriez sur Objective-C.

Dans certains cas, vous trouverez **statique** membres à l’intérieur de catégories, comme dans l’exemple suivant :

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Cela entraîne une **incorrect** catégorie C# définition d’interface :

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Cela est incorrect, car à utiliser le `BoolMethod` extension vous avez besoin d’une instance de `FooObject` mais vous liez un ObjC **statique** extension, il s’agit d’un effet secondaire en raison du fait de la façon C# implémentés des méthodes d’extension .

La seule façon d’utiliser les définitions ci-dessus est par le code horrible suivant :

```csharp
(null as FooObject).BoolMethod (range);
```

La recommandation pour éviter ce problème consiste à inline la `BoolMethod` définition à l’intérieur de la `FooObject` définition d’interface lui-même, cela vous permettra d’appeler cette extension, comme il est prévu `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Nous enverrons un message d’avertissement (BI1117) chaque fois que nous avons trouvé un [ `[Static]` ](#StaticAttribute) membre à l’intérieur d’un [ `[Category]` ](#CategoryAttribute) définition. Si vous voulez vraiment avoir [ `[Static]` ](#StaticAttribute) membres à l’intérieur de votre [ `[Category]` ](#CategoryAttribute) vous pouvez progressivement l’avertissement à l’aide de définitions `[Category (allowStaticMembers: true)]` ou en décorant votre membre ou [ `[Category]` ](#CategoryAttribute) définition avec l’interface [ `[Internal]` ](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

Lorsque cet attribut est appliqué à une classe il génère simplement une classe statique, qui ne dérive pas de `NSObject`, la [ `[BaseType]` ](#BaseTypeAttribute) attribut est ignoré. Les classes static sont utilisés pour héberger les variables publiques C que vous souhaitez exposer.

Exemple :

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Génère un C# classe avec l’API suivante :

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Modèle de protocole/définitions

Les modèles sont généralement utilisées par implémentation de protocole.
Elles diffèrent dans la mesure où le runtime enregistre une seule avec Objective-C les méthodes qui a réellement été remplacés.
Sinon, la méthode ne sera pas enregistrée.

Cela signifie en général que lorsque vous créez une sous-classe une classe qui a été marquée avec la `ModelAttribute`, vous ne devez pas appeler la méthode de base.   Cette méthode lève une exception, vous êtes supposé pour implémenter l’ensemble du comportement sur votre sous-classe pour les méthodes que vous substituez.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

Par défaut, les membres qui font partie d’un protocole ne sont pas obligatoires. Cela permet aux utilisateurs de créer une sous-classe de la `Model` objet dérivant de simplement la classe dans C# et en substituant uniquement les méthodes qui les intéressent. Parfois, le contrat de Objective-C requiert que l’utilisateur fournit une implémentation de cette méthode (ceux marqués avec le `@required` directive en Objective-C). Dans ces cas, vous devez marquer ces méthodes avec le `[Abstract]` attribut.

Le `[Abstract]` attribut peut être appliqué à des méthodes ou propriétés et provoque le générateur marquer le membre généré comme abstraite et la classe est une classe abstraite.

Le texte suivant provient de Xamarin.iOS :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

Spécifie la valeur par défaut doivent être retournées par une méthode de modèle si l’utilisateur ne fournit pas une méthode pour cette méthode particulière dans l’objet de modèle

Syntaxe :

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Par exemple, dans la classe de délégué imaginaire suivante pour un `Camera` classe, nous fournissons un `ShouldUploadToServer` qui est exposé en tant que propriété sur le `Camera` classe. Si l’utilisateur de la `Camera` classe ne définit pas explicitement une valeur à une expression lambda qui peut répondre à la valeur true ou false, la valeur par défaut en retour dans ce cas serait défini sur false, la valeur que nous avons spécifié dans le `DefaultValue` attribut :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Si l’utilisateur définit un gestionnaire de la classe imaginaire, cette valeur est ignorée :

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

Voir aussi : [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Syntaxe :

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Cet attribut lorsque fourni sur une méthode qui retourne une valeur sur une classe de modèle, vous demandez le générateur pour retourner la valeur du paramètre spécifié si l’utilisateur n’a pas fourni de sa propre méthode ou une expression lambda.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

Dans le cas ci-dessus si l’utilisateur de la `NSAnimation` classe a choisi d’utiliser une la C# événements et propriétés et vous n’avez pas défini `NSAnimation.ComputeAnimationCurve` à une méthode ou une expression lambda, la valeur de retour est la valeur passée dans le paramètre de progression.

Voir aussi : [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

Il est parfois intéressant pas à exposer un événement ou d’un délégué propriété une classe de modèle dans la classe d’hôte afin de l’ajout de cet attribut, vous demandez le générateur pour éviter la génération de n’importe quelle méthode décorée avec lui.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

Cet attribut est utilisé dans les méthodes de modèle qui retournent des valeurs pour définir le nom de la signature du délégué à utiliser.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Avec la définition ci-dessus, le générateur produit la déclaration publique suivante :

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Cet attribut est utilisé pour autoriser le générateur modifier le nom de la propriété générée dans la classe de l’hôte. Il est parfois utile lorsque le nom de la méthode de classe FooDelegate pertinente pour la classe Delegate, mais se présenterait impair dans la classe d’hôte en tant que propriété.

Cela est également très utiles (et nécessaire) lorsque vous disposez de deux ou plusieurs méthodes de surcharge qui a du sens pour les conserver nommé comme c’est dans la classe FooDelegate mais vous souhaitez les exposer dans la classe d’hôte avec un meilleur nom donné.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Avec la définition ci-dessus, le générateur produit la déclaration publique suivante dans la classe d’hôte :

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

Pour les événements qui prennent plusieurs paramètres (en Objective-C la convention est que le premier paramètre dans une classe déléguée est l’instance de l’objet expéditeur) vous devez fournir le nom que vous souhaitez que la classe EventArgs générée peut être. Cette opération est effectuée avec la `[EventArgs]` attribut sur la déclaration de méthode dans votre `Model` classe.

Exemple :

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

La déclaration ci-dessus générera un `UIImagePickerImagePickedEventArgs` classe qui dérive d’EventArgs et compresse les deux paramètres, le `UIImage` et `NSDictionary`. Le générateur produit ceci :

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Il présente ensuite les éléments suivants dans le `UIImagePickerController` classe :

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Cet attribut est utilisé pour autoriser le générateur modifier le nom d’un événement ou la propriété générée dans la classe. Il est parfois utile lorsque le nom de la méthode de classe de modèle adapté à la classe de modèle, mais se présenterait impair dans la classe d’origine comme un événement ou une propriété.

Par exemple, le `UIWebView` utilise le bit en partant du `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

L’expose ci-dessus `LoadingFinished` en tant que la méthode dans le `UIWebViewDelegate`, mais `LoadFinished` en tant que l’événement pour vous connecter dans un `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

Lorsque vous appliquez le `[Model]` attribut à une définition de type dans votre contrat d’API, le runtime génère un code spécial qui affichera uniquement les appels aux méthodes dans la classe si l’utilisateur a remplacé une méthode dans la classe. Cet attribut est généralement appliqué à toutes les API qui encapsulent une classe de délégué Objective-C.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Spécifie que la méthode sur le modèle ne fournit pas une valeur de retour par défaut.

Cela fonctionne avec le runtime Objective-C en répondant `false` à la demande du runtime Objective-C pour déterminer si le sélecteur spécifié est implémenté dans cette classe.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

Voir aussi : [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>Protocoles

Le concept de protocole Objective-C n’existe pas réellement dans C#. Protocoles sont similaires aux C# interfaces, mais ils diffèrent en ce que toutes les méthodes et propriétés déclarées dans un protocole doivent être implémentées par la classe qui il adopte. Au lieu de cela, certaines méthodes et propriétés sont facultatives.

Certains protocoles sont généralement utilisés comme classes de modèle, ceux doivent être lié à l’aide de la [ `[Model]` ](#ModelAttribute) attribut.

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
```

À partir de Xamarin.iOS 7.0, un fonctionnalité de liaison de protocole de nouvel et amélioré incorporée.  Toute définition qui contient le `[Protocol]` attribut génère en fait trois classes de prise en charge qui améliorent considérablement la façon que vous consommez des protocoles :

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

Le **implémentation de la classe** fournit une classe abstraite complète que vous pouvez substituer des méthodes individuelles d’et obtenir la sécurité de type complet. Mais en raison C# prenant ne pas en charge l’héritage multiple, il existe des scénarios où vous pouvez exiger une autre classe de base, mais que vous souhaitez toujours implémenter une interface.

C’est là où le texte généré **définition d’interface** arrive.  Il est une interface qui possède toutes les méthodes requises du protocole.  Cela permet aux développeurs qui souhaitent implémenter votre protocole pour simplement implémenter l’interface.  Le runtime s’inscriront automatiquement le type en tant que l’adoption du protocole.

Notez que l’interface répertorie les méthodes requises uniquement et n’expose pas les méthodes facultatives.   Cela signifie que les classes qui adoptent le protocole obtiendra complète vérification de type pour les méthodes requises, mais qu’il ont recours au typage faible (manuellement à l’aide des attributs d’exportation et correspondre à la signature) pour les méthodes de protocole facultatifs.

Pour faciliter le consommer une API qui utilise des protocoles, l’outil de liaison également produira une classe de méthode d’extensions qui expose toutes les méthodes facultatives.   Cela signifie que tant que vous consommez une API, vous serez en mesure de traiter des protocoles comme ayant toutes les méthodes.

Si vous souhaitez utiliser les définitions de protocole dans votre API, vous devez écrire des interfaces vides squelettes dans votre définition d’API.   Si vous souhaitez utiliser le MyProtocol dans une API, vous devez le faire :

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

La méthode ci-dessus est nécessaire, car au moment de la liaison la `IMyProtocol` n’existerait pas, autrement dit pourquoi vous devez fournir une interface vide.

### <a name="adopting-protocol-generated-interfaces"></a>Adoption d’interfaces générées par le protocole

Chaque fois que vous implémentez une des interfaces générées pour les protocoles, comme suit :

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

L’implémentation pour les méthodes d’interface obtient automatiquement exportée avec le nom approprié, par conséquent, il est équivalent à cet :

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Si l’interface est implémentée de manière implicite ou explicite n’a pas d’importance.

### <a name="protocol-inlining"></a>Protocole incorporation (inlining)

Bien que vous liez des types Objective-C existants qui ont été déclarés en tant que l’adoption d’un protocole, vous souhaiterez inline le protocole directement. Pour ce faire, déclarez simplement votre protocole en tant qu’interface sans aucune [ `[BaseType]` ](#BaseTypeAttribute) d’attribut et de répertorier le protocole dans la liste des interfaces de base pour votre interface.

Exemple :

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>Définitions de membre

Les attributs dans cette section sont appliqués aux membres individuels d’un type : propriétés et les déclarations de méthode.


### <a name="alignattribute"></a>AlignAttribute

Permet de spécifier la valeur d’alignement pour les types de retour de propriété. Certaines propriétés acceptent des pointeurs vers les adresses qui doivent être alignées sur certaines limites (dans Xamarin.iOS, cela se produit par exemple avec certains `GLKBaseEffect` alignement de propriétés qui doivent être de 16 octets). Vous pouvez utiliser cette propriété pour décorer l’accesseur Get et utiliser la valeur d’alignement. Cela est généralement utilisé avec la `OpenTK.Vector4` et `OpenTK.Matrix4` types lors de l’intégration avec les API Objective-C.

Exemple :

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

Le `[Appearance]` attribut est limité à iOS 5, dans lequel le Gestionnaire de l’apparence a été introduit.

Le `[Appearance]` attribut peut être appliqué à toute méthode ou propriété qui participent à la `UIAppearance` framework. Lorsque cet attribut est appliqué à une méthode ou propriété dans une classe, il dirige le Générateur de liaison pour créer une classe fortement typée une apparence qui est utilisée pour définir le style toutes les instances de cette classe, ou les instances qui correspondent à certains critères.

Exemple :

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

La méthode ci-dessus génère le code suivant dans UIToolbar :

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

Utilisez le `[AutoReleaseAttribute]` sur les méthodes et propriétés à inclure l’invocation de méthode à la méthode dans un `NSAutoReleasePool`.

En Objective-C, il existe certaines méthodes qui retournent des valeurs qui sont ajoutés à la valeur par défaut `NSAutoReleasePool`. Par défaut, ces accédaient à votre thread `NSAutoReleasePool`, mais étant donné que Xamarin.iOS conserve également une référence à vos objets tant que réside l’objet managé, vous souhaiterez peut-être pas conserver une référence supplémentaire dans le `NSAutoReleasePool` qui sera uniquement obtenir purgé jusqu'à ce que votre thread Retourne au prochain thread de contrôle, ou vous revenez à la boucle principale.

Cet attribut est appliqué par exemple sur les propriétés de lourdes (par exemple `UIImage.FromFile`) qui retourne des objets qui ont été ajoutées à la valeur par défaut `NSAutoReleasePool`. Sans cet attribut, les images seraient conservées tant que votre thread n’a pas retourné le contrôle à la boucle principale. Uf votre thread a été une sorte de téléchargeur de d’arrière-plan est toujours actif et en attente de travail, les images ne seraient jamais publiées.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

Le `[ForcedTypeAttribute]` est utilisé pour appliquer la création d’un type managé, même si l’objet non managé retourné ne correspond pas au type décrit dans la définition de liaison.

Cela est utile lorsque le type décrit dans un en-tête ne correspond pas au type retourné de la méthode native, par exemple prendre la définition suivante de Objective-C à partir de `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Il énonce clairement qu’elle renverra un `NSURLSessionDownloadTask` de l’instance, mais encore il **retourne** un `NSURLSessionTask`, qui est une superclasse et par conséquent pas convertible en `NSURLSessionDownloadTask`. Dans la mesure où nous sommes dans un contexte de type sécurisé une `InvalidCastException` se produira.

Conforme à la description de l’en-tête et d’éviter le `InvalidCastException`, le `[ForcedTypeAttribute]` est utilisé.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

Le `[ForcedTypeAttribute]` accepte également une valeur booléenne nommée `Owns` c'est-à-dire `false` par défaut `[ForcedType (owns: true)]`. La possède le paramètre est utilisé pour suivre la [stratégie de propriété](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) pour **Core Foundation** objets.

Le `[ForcedTypeAttribute]` est valide uniquement sur la valeur de retour, paramètres et propriétés.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

Le `[BindAsAttribute]` autorise la liaison `NSNumber`, `NSValue` et `NSString`(énumérations) en plus précises C# types. L’attribut peut être utilisé pour créer la meilleure et plus précis, les API .NET sur l’API native.

Vous pouvez décorer les méthodes (sur la valeur de retour), les paramètres et les propriétés avec `BindAs`. La seule restriction est que votre membre **ne doit pas** être à l’intérieur d’un `[Protocol]` ou [ `[Model]` ](#ModelAttribute) interface.

Exemple :

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Devrait produire :

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

En interne, nous ferons le `bool?`  <->  `NSNumber` et `CGRect`  <->  `NSValue` conversions.

Les types pris en charge d’encapsulation actuels sont :

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

Ce qui suit C# types de données sont pris en charge pour être encapsulées dans/depuis `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

Ce qui suit C# types de données sont pris en charge pour être encapsulées dans/depuis `NSNumber`:

* bool
* byte
* double
* float
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* Enums

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) fonctionne conjointement avec [enums soutenu par une constante de chaîne NSString](#enum-attributes) pour pouvoir créer une meilleure API .NET, par exemple :

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Devrait produire :

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Nous nous chargerons le `enum`  <->  `NSString` conversion uniquement si le type de l’énumération fournie à [ `[BindAs]` ](#BindAsAttribute) est [soutenu par une constante de chaîne NSString](#enum-attributes).

#### <a name="arrays"></a>Tableaux

[`[BindAs]`](#BindAsAttribute) prend également en charge les tableaux de tous les types pris en charge, vous pouvez avoir la définition d’API suivante comme exemple :

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Devrait produire :

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

Le `rects` paramètre sera encapsulé dans un `NSArray` qui contient un `NSValue` pour chaque `CGRect` et vous obtenez en retour un tableau de `CAScroll?` qui a été créé en utilisant les valeurs de retourné `NSArray` contenant `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

Le `[Bind]` attribut a deux utilisations d’un lorsqu’il est appliqué à une méthode ou déclaration de propriété et une autre lorsqu’il est appliqué à l’accesseur Get individuel ou un Set dans une propriété.

Lorsqu’il est utilisé pour une méthode ou propriété, l’effet de la `[Bind]` attribut consiste à générer une méthode qui appelle le sélecteur spécifié. Mais la méthode générée qui en résulte n’est pas décorée avec le [ `[Export]` ](#ExportAttribute) attribut, ce qui signifie qu’il ne peut pas être utilisé lors de la substitution de méthode. Cela est généralement utilisé en association avec le `[Target]` attribut pour l’implémentation des méthodes d’extension Objective-C.

Exemple :

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Lorsqu’il est utilisé dans une méthode getter ou un accesseur Set, le `[Bind]` attribut est utilisé pour modifier les valeurs par défaut déduits par le Générateur de code lors de la génération des noms de sélecteur Objective-C getter et setter d’une propriété. Par défaut lorsque vous marquez une propriété portant le nom `fooBar`, le générateur générerait un `fooBar` exporter de la méthode getter et `setFooBar:` pour l’accesseur Set. Dans certains cas, Objective-C ne suit pas cette convention, généralement ils modifier le nom de l’accesseur Get peut être `isFooBar`.
Vous utiliserez cet attribut pour informer le Générateur de cela.

Exemple :

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Uniquement disponible sur Xamarin.iOS 6.3 et les versions ultérieures.

Cet attribut peut être appliqué aux méthodes qui prennent un gestionnaire d’achèvement comme dernier argument.

Vous pouvez utiliser le `[Async]` attribut sur les méthodes dont le dernier argument est un rappel.  Lorsque vous appliquez ce à une méthode, le Générateur de liaison générera une version de cette méthode avec le suffixe `Async`.  Si le rappel ne prend aucun paramètre, la valeur de retour sera un `Task`, si le rappel prend un paramètre, le résultat sera un `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Ce qui suit génère cette méthode asynchrone :

```csharp
Task LoadFileAsync (string file);
```

Si le rappel prend plusieurs paramètres, vous devez définir le `ResultType` ou `ResultTypeName` pour spécifier le nom de votre choix du type généré qui contiendra toutes les propriétés.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Le code suivant génère cette méthode async, où `FileLoading` contient des propriétés pour accéder à des `files` et `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

Si le dernier paramètre du rappel est un `NSError`, puis généré `Async` méthode vérifie si la valeur n’est pas null, et si tel est le cas, la méthode async généré définira l’exception de la tâche.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

La méthode ci-dessus génère la méthode async suivante :

```csharp
Task<string> UploadAsync (string file);
```

Et en cas d’erreur, la tâche résultante aura l’exception définie sur une `NSErrorException` qui encapsule le résultat `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Utilisez cette propriété pour spécifier la valeur de la retourner `Task` objet.   Ce paramètre prend un type existant, il doit donc être défini dans un de vos définitions d’api core.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Utilisez cette propriété pour spécifier la valeur de la retourner `Task` objet.   Ce paramètre prend le nom de votre nom de type souhaité, le générateur produira une série de propriétés, un pour chaque paramètre qui accepte le rappel.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Cette propriété permet de personnaliser le nom des méthodes async généré.   La valeur par défaut consiste à utiliser le nom de la méthode et ajoutez le texte « Async », vous pouvez l’utiliser pour modifier cette valeur par défaut.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Cet attribut est appliqué aux paramètres de chaîne ou des propriétés de chaîne et indique le Générateur de code de ne pas utiliser le marshaling des chaînes de la copie de zéro pour ce paramètre et au lieu de cela créer une nouvelle instance de chaîne NSString à partir de la C# chaîne.
Cet attribut est requis uniquement sur les chaînes si vous demandez le Générateur à utiliser le marshaling des chaînes de la copie de zéro à l’aide de la `--zero-copy` option de ligne de commande ou en définissant l’attribut de niveau assembly `ZeroCopyStringsAttribute`.

Cela est nécessaire dans les cas où la propriété est déclarée en Objective-C pour être un `retain` ou `assign` propriété au lieu d’un `copy` propriété. Ceux-ci se produisent généralement dans des bibliothèques tierces qui ont été mal « optimisées » par les développeurs. En règle générale, `retain` ou `assign` `NSString` propriétés sont incorrectes depuis `NSMutableString` ou classes dérivées d’utilisateur de `NSString` peut modifier le contenu des chaînes sans la connaissance du code de bibliothèque, dernières légèrement le application. Cela se produit généralement en raison d’une optimisation prématurée.

L’exemple suivant montre deux de ces propriétés dans Objective-c :

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

Lorsque vous appliquez le `[DisposeAttribute]` à une classe, vous fournissez un extrait de code qui sera ajouté à la `Dispose()` implémentation de la méthode de la classe.

Dans la mesure où le `Dispose` méthode est générée automatiquement par le `bmac-native` et `btouch-native` outils, vous devez utiliser le `[Dispose]` attribut d’injecter du code généré `Dispose` implémentation de la méthode.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

Le `[Export]` attribut est utilisé pour marquer une méthode ou propriété à être exposés au runtime Objective-C. Cet attribut est partagé entre l’outil de liaison et les runtimes de Xamarin.iOS et Xamarin.Mac réels. Pour les méthodes, le paramètre est passé mot pour mot à du code généré, pour les exportations de propriétés, des méthodes getter et setter sont générées en fonction de la déclaration de base (voir la section sur la [ `[BindAttribute]` ](#BindAttribute) pour plus d’informations sur la façon de modifier le comportement de l’outil de liaison).

Syntaxe :

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

Le [sélecteur](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) représente le nom de la méthode Objective-C sous-jacente ou la propriété qui est liée.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

Cet attribut est utilisé pour exposer une variable globale C en tant que champ qui est chargé à la demande et exposé à C# code. En général, cela est nécessaire pour obtenir les valeurs des constantes qui sont définies en C ou Objective-C et qui peut être soit jetons utilisés dans certaines API, ou dont les valeurs sont opaques et doivent être utilisés en tant que-est par le code utilisateur.

Syntaxe :

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

Le `symbolName` est le symbole C à se lier. Par défaut, il sera chargé à partir d’une bibliothèque dont le nom est déduit à partir de l’espace de noms dans lequel le type est défini. S’il ne s’agit pas de la bibliothèque où le symbole est recherché, vous devez passer le `libraryName` paramètre. Si vous liez une bibliothèque statique, utilisez `__Internal` comme le `libraryName` paramètre.

Les propriétés générées sont toujours statiques.

Propriétés marquées avec l’attribut de champ peuvent être des types suivants :

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* Enums

Méthodes setter n’est pas pris en charge pour [enums soutenu par des constantes de chaîne NSString](#enum-attributes), mais elles peuvent être liées manuellement si nécessaire.

Exemple :

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

Le `[Internal]` attribut peut être appliqué aux méthodes ou propriétés, et il a pour effet de marquer le code généré par le `internal` C# mot clé qui peut le rendre accessible uniquement au code dans l’assembly généré. Cela est généralement utilisé pour masquer les API qui sont trop bas niveau ou fournir une API publique non optimal que vous souhaitez améliorer une fois ou pour les API qui ne sont pas pris en charge par le générateur et requièrent du codage de main.

Lorsque vous concevez la liaison, vous généralement masquer la méthode ou propriété à l’aide de cet attribut et fournir un nom différent pour la méthode ou propriété, puis sur votre C# fichier de support complémentaires, vous devez ajouter un wrapper fortement typé qui expose le fonctionnalité sous-jacente.

Exemple :

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Ensuite, dans votre fichier de prise en charge, vous pouvez avoir du code comme suit :

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Cet attribut marque le champ de stockage pour une propriété à annoter avec .NET `[ThreadStatic]` attribut. Cela est utile si le champ est une variable statique de thread.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

Cet attribut rend une méthode prise en charge (Objective-C) les exceptions natives.
Au lieu d’appeler `objc_msgSend` directement, l’appel passe par un trampoline personnalisé qui intercepte les exceptions ObjectiveC et marshale les dans les exceptions managées.

Actuellement uniquement quelques `objc_msgSend` signatures sont prises en charge (vous trouverez différentes si une signature n’est pas pris en charge lors de la liaison native d’une application qui utilise la liaison échoue avec un monotouch_ manquant *_objc_msgSend* symbole), mais plus peuvent être ajouté à la demande.


### <a name="newattribute"></a>NewAttribute

Cet attribut est appliqué aux méthodes et propriétés pour que le Générateur de génèrent le `new` mot-clé devant la déclaration.

Il est utilisé pour éviter les avertissements du compilateur lors de la même méthode ou le nom de propriété est introduite dans une sous-classe qui existaient déjà dans une classe de base.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

Vous pouvez appliquer cet attribut aux champs d’avoir alimentaires générateur un programme d’assistance fortement typée classe Notifications.

Cet attribut peut être utilisé sans arguments pour les notifications qui ne transmettent aucune charge utile, ou vous pouvez spécifier un `System.Type` qui fait référence à une autre interface dans la définition d’API, généralement avec le nom se terminant par « EventArgs ». Le générateur activera l’interface dans une classe qui sous-classe `EventArgs` et inclut toutes les propriétés qui y figurent. Le [ `[Export]` ](#ExportAttribute) attribut doit être utilisé dans le `EventArgs` classe pour répertorier le nom de la clé utilisée pour rechercher le dictionnaire Objective-C pour extraire la valeur.

Exemple :

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
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Les utilisateurs de votre code peuvent ensuite facilement s’abonner aux notifications publiées sur le [NSDefaultCenter](xref:Foundation.NSNotificationCenter.DefaultCenter) à l’aide de code similaire à celui-ci :

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Ou pour définir un objet spécifique à observer. Si vous transmettez `null` à `objectToObserve` cette méthode se comporte comme son autre homologue.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

La valeur retournée à partir de `ObserveDidStart` peut être utilisé pour facilement cesser de recevoir des notifications, comme suit :

```csharp
token.Dispose ();
```

Ou vous pouvez appeler [NSNotification.DefaultCenter.RemoveObserver](xref:Foundation.NSNotificationCenter.RemoveObserver(Foundation.NSObject)) et passer le jeton. Si votre notification contient des paramètres, vous devez spécifier un programme d’assistance `EventArgs` interface, comme suit :

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

La méthode ci-dessus génère un `MyScreenChangedEventArgs` classe avec le `ScreenX` et `ScreenY` propriétés qui permettent d’extraire les données à partir de la [NSNotification.UserInfo](xref:Foundation.NSNotification.UserInfo) dictionnaire à l’aide de clés `ScreenXKey` et `ScreenYKey` respectivement et appliquer les conversions appropriées. Le `[ProbePresence]` attribut est utilisé pour le générateur pour détecter si la clé est définie le `UserInfo`, au lieu de tenter d’extraire la valeur. Cela est utilisé pour les cas où la présence de la clé est la valeur (en général pour les valeurs booléennes).

Cela vous permet d’écrire du code comme suit :

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

Dans certains cas, il n’existe aucune constante associée à la valeur passée dans le dictionnaire.  Apple utilise parfois des constantes de symboles publics et parfois appel à des constantes de chaîne.  Par défaut le [ `[Export]` ](#ExportAttribute) attribut dans votre fourni `EventArgs` classe utilisera le nom spécifié comme un symbole public recherchés lors de l’exécution.  Si ce n’est pas le cas, au lieu de cela elle est censée être recherchés en tant que constante de chaîne, puis transmettre la `ArgumentSemantic.Assign` valeur à l’attribut Export.

**Nouveau dans Xamarin.iOS 8.4**

Parfois, notifications commenceront à vie sans aucun argument, par conséquent, l’utilisation de [ `[Notification]` ](#NotificationAttribute) sans arguments n’est pas acceptable.  Mais parfois, les paramètres à la notification seront introduites.  Pour prendre en charge ce scénario, l’attribut peut être appliqué plusieurs fois.

Si vous développez une liaison, et que vous souhaitez éviter de casser le code utilisateur existant, vous activez une notification existante à partir de :

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

Dans une version qui répertorie l’attribut de notification à deux reprises, comme suit :

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Lorsque cela est appliqué à une propriété qui signale la propriété comme autorisant la valeur `null` lui être attribué. Cela est uniquement valide pour les types référence.

Lorsque cela est appliqué à un paramètre dans une signature de méthode il indique que le paramètre spécifié peut être null et qu’aucune vérification ne doit être effectuée pour les passer `null` valeurs.

Si le type de référence n’a pas cet attribut, l’outil de liaison génère une vérification de la valeur assignée avant de le transmettre à Objective-C et génère une vérification lèvera une `ArgumentNullException` si la valeur assignée est `null`.

Exemple :

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

Utilisez cet attribut pour indiquer le Générateur de liaison que la liaison pour cette méthode particulière doit être marquée avec un `override` mot clé.

### <a name="presnippetattribute"></a>PreSnippetAttribute

Vous pouvez utiliser cet attribut pour injecter du code à insérer après que les paramètres d’entrée ont été validées, mais avant le code appelle dans Objective-C.

Exemple :

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Vous pouvez utiliser cet attribut pour injecter du code à insérer avant que les paramètres sont validés dans la méthode générée.

Exemple :

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

Indique le Générateur de liaison pour appeler la propriété spécifiée à partir de cette classe pour extraire une valeur à partir de celui-ci.

Cette propriété est généralement utilisée pour actualiser le cache qui pointe vers les objets de référence qui empêchent le graphique d’objet référencé. Il affiche généralement dans le code qui a des opérations telles qu’Ajouter/supprimer. Cette méthode est utilisée afin qu’une fois que les éléments sont ajoutés ou supprimés de mise à jour le cache interne pour vous assurer que nous conservons les références managées aux objets qui sont réellement en cours d’utilisation. Cela est possible, car l’outil de liaison génère un champ de stockage pour tous les objets de référence dans une liaison donnée.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

Dans ce cas, le `Dependencies` propriété sera appelée après l’ajout ou la suppression des dépendances à partir de la `NSOperation` objet, en garantissant que nous avons un graphique qui représente le texte réel des objets chargés, empêche les fuites de mémoire ainsi que sur une corruption de mémoire.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

Vous pouvez utiliser cet attribut pour injecter certaines C# le code source à insérer après le code a appelé la méthode Objective-C sous-jacente

Exemple :

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

Cet attribut est appliqué pour retourner des valeurs pour les marquer comme étant des objets proxy. Certains objets de retour proxy Objective-C API qui ne peuvent pas être différenciés de liaisons de l’utilisateur. L’effet de cet attribut consiste à marquer l’objet comme étant un `DirectBinding` objet. Pour un scénario de Xamarin.Mac, vous pouvez voir le [discussion sur ce bogue](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>RetainListAttribute

Indique le générateur pour conserver une référence managée pour le paramètre ou de supprimer une référence interne pour le paramètre. Cela permet de garder les objets référencés.

Syntaxe :

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Si la valeur de `doAdd` est true, le paramètre est ajouté à la `__mt_{0}_var List<NSObject>;`. Où `{0}` est remplacé par la donnée `listName`. Vous devez déclarer ce champ de stockage dans votre classe partielle complémentaire à l’API.

Pour obtenir un exemple, consultez [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) et [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

Cela peut être appliqué pour retourner des types pour indiquer que le générateur doit appeler `Release` sur l’objet avant de le renvoyer. Cela est nécessaire uniquement lorsqu’une méthode vous donne un objet conservé (par opposition à un objet autoreleased, qui est le scénario le plus courant)

Exemple :

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

En outre, cet attribut est propagé au code généré, afin que le runtime Xamarin.iOS sache qu’il doit conserver l’objet une fois de retour et Objective-C à partir d’une telle fonction.


### <a name="sealedattribute"></a>SealedAttribute

Indique le générateur pour marquer la méthode générée comme sealed. Si cet attribut n’est pas spécifié, la valeur par défaut consiste à générer une méthode virtuelle (une méthode virtuelle, une méthode abstraite ou une substitution en fonction de la façon dont les autres attributs sont utilisés).

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

Lorsque le `[Static]` l’attribut est appliqué à une méthode ou propriété, cela génère une méthode statique ou une propriété. Si cet attribut n’est pas spécifié, le générateur produit une méthode d’instance ou une propriété.


### <a name="transientattribute"></a>TransientAttribute

Utilisez cet attribut aux propriétés de l’indicateur dont les valeurs sont temporaires, autrement dit, les objets qui sont créés temporairement par iOS, mais ne sont pas durables. Lorsque cet attribut est appliqué à une propriété, le générateur ne crée pas un champ de stockage pour cette propriété, ce qui signifie que la classe managée ne conserve pas une référence à l’objet.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Dans la conception des liaisons Xamarin.iOS/Xamarin.Mac, le `[Wrap]` attribut est utilisé pour encapsuler un objet faiblement typé avec un objet fortement typé. Cela intervient principalement avec les objets délégué Objective-C qui sont généralement déclarés comme étant de type `id` ou `NSObject`. La convention utilisée par Xamarin.iOS et Xamarin.Mac consiste à exposer ces délégués ou sources de données comme étant de type `NSObject` et sont nommés à l’aide de la convention « Weak » + le nom qui est exposé. Un `id delegate` propriété à partir d’Objective-C est exposée comme un `NSObject WeakDelegate { get; set; }` propriété dans le fichier de contrat d’API.

Mais en général, la valeur est assignée à ce délégué est de type fort, afin de faire apparaître le type fort et appliquer le `[Wrap]` attribut, cela signifie que les utilisateurs peuvent choisir d’utiliser des types faibles s’ils ont besoin d’une fine-control ou s’ils doivent recourir à tric de bas niveau KS, ou ils peuvent utiliser la propriété fortement typée pour la plupart de leur travail.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Voici comment l’utilisateur préférable d’utiliser la version faiblement typée du délégué :

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

Et c’est la façon dont l’utilisateur utiliserait la version fortement typée, notez que l’utilisateur tire parti de C#du système de type et utilise le mot clé override pour déclarer son intention et qu’il n’a pas décorer manuellement la méthode avec `[Export]`, dans la mesure où avez-vous qui fonctionnent dans la liaison pour l’utilisateur :

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Une autre utilisation de la `[Wrap]` attribut consiste à prendre en charge de la version fortement typée de méthodes.  Exemple :

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Les membres générés par `[Wrap]` ne sont pas `virtual` par défaut, si vous avez besoin d’un `virtual` membre que vous pouvez définir sur `true` facultatif `isVirtual` paramètre.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>Attributs de paramètre

Cette section décrit les attributs que vous pouvez appliquer aux paramètres dans une définition de méthode, ainsi que les `[NullAttribute]` qui s’applique à une propriété dans sa globalité.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Cet attribut est appliqué aux types de paramètre dans C# les déclarations de délégué pour avertir le binder le paramètre en question est conforme à la Objective-C bloquer la convention d’appel et doit marshaler de cette façon.

Cela est généralement utilisé pour les rappels qui sont définis comme suit dans Objective-c :

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Voir aussi : [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Cet attribut est appliqué aux types de paramètre dans C# déléguer des déclarations pour informer le classeur que le paramètre en question est conforme au pointeur de fonction C ABI convention d’appel et qu’il doit la marshaler de cette façon.

Cela est généralement utilisé pour les rappels qui sont définis comme suit dans Objective-c :

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Voir aussi : [BlockCallback](#BlockCallback).

### <a name="params"></a>params

Vous pouvez utiliser le `[Params]` attribut sur le dernier paramètre de tableau d’une définition de méthode pour que le Générateur d’injecter un « params » dans la définition.   Cela permet la liaison permet facilement pour les paramètres facultatifs.

Par exemple, la définition suivante :

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Permet l’écriture du code suivant :

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

Cela a l’avantage qu’il ne nécessite pas aux utilisateurs de créer un tableau purement pour passer des éléments.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Vous pouvez utiliser la `[PlainString]` attribut devant les paramètres de chaîne de demande au Générateur de liaison pour passer la chaîne sous forme de chaîne C, au lieu de passer le paramètre comme un `NSString`.

Utiliser la plupart des API Objective-C `NSString` paramètres, mais un certain nombre d’API exposer un `char *` API pour passer des chaînes, au lieu du `NSString` variante.
Utilisez `[PlainString]` dans ces cas.

Par exemple, les déclarations Objective-C suivantes :

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Doit être lié comme suit :

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

Indique le Générateur de conserver une référence au paramètre spécifié. Le générateur fournira le magasin de stockage pour ce champ ou vous pouvez spécifier un nom (le `WrapName`) pour stocker la valeur à. Cela est utile pour stocker une référence à un objet managé qui est passé en tant que paramètre à Objective-C et lorsque vous savez que Objective-C conserve uniquement cette copie de l’objet. Par exemple, une API comme `SetDisplay (SomeObject)` utilise cet attribut car il est probable que le SetDisplay peut afficher uniquement un seul objet à la fois. Si vous avez besoin effectuer le suivi de plusieurs objets (par exemple, pour une API de pile similaire) que vous utiliseriez le `[RetainList]` attribut.

Syntaxe :

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Indique le générateur pour conserver une référence managée pour le paramètre ou de supprimer une référence interne pour le paramètre. Cela permet de garder les objets référencés.

Syntaxe :

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Si la valeur de `doAdd` est true, le paramètre est ajouté à la `__mt_{0}_var List<NSObject>`. Où `{0}` est remplacé par la donnée `listName`. Vous devez déclarer ce champ de stockage dans votre classe partielle complémentaire à l’API.

Pour obtenir un exemple, consultez [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) et [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Cet attribut est appliqué aux paramètres et est utilisé uniquement lors de la transition à partir d’Objective-C à C#.  Au cours de ces transitions les diverses Objective-C `NSObject` paramètres sont encapsulés dans une représentation managée de l’objet.

Le runtime prend une référence à l’objet natif et conserver la référence jusqu'à ce que la dernière référence managée à l’objet a disparu et que le garbage collector a une chance de s’exécuter.

Dans certains cas, il est important pour le C# runtime pour ne pas conserver une référence à l’objet natif.  Cela se produit parfois lorsque le code natif sous-jacent a attaché un comportement spécial pour le cycle de vie du paramètre.  Par exemple : le destructeur pour le paramètre effectuent une action de nettoyage, ou supprimer certaines ressources précieuses.

Cet attribut informe le runtime que vous désirez l’objet à être supprimé si possible lors du retour à Objective-C à partir de votre méthode remplacé.

La règle est simple : si le runtime devait créer une nouvelle représentation managée à partir de l’objet natif, puis à la fin de la fonction, le nombre de conservation pour l’objet natif est supprimé, et la propriété de Handle de l’objet managé est effacée.   Cela signifie que si vous avez conservé une référence à l’objet managé, cette référence devient inutile (appel de méthodes sur elle lève une exception).

Si l’objet passé n’a pas été créé, ou s’il existe déjà une représentation managée en attente de l’objet, la méthode dispose forcée n’a pas lieu. 


## <a name="property-attributes"></a>Attributs de propriété

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Cet attribut est utilisé pour prendre en charge un idiome Objective-C, où une propriété avec un accesseur Get est introduite dans une classe de base, et une sous-classe mutable introduit un accesseur Set.

Dans la mesure où C# ne prend pas en charge ce modèle, la classe de base doit avoir l’accesseur Set et la méthode getter et une sous-classe peut utiliser le [OverrideAttribute](#OverrideAttribute).

Cet attribut est utilisé uniquement dans les accesseurs Set de propriété et est utilisé pour prendre en charge l’idiome mutable dans Objective-C.

Exemple :

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Enum (attribut)

Mappage `NSString` constantes aux valeurs enum est un moyen facile de créer de meilleures API .NET. Elle :

* permet la complétion de code afin d’être plus utile, en montrant **uniquement** les valeurs correctes pour l’API ;
* Ajoute la sécurité de type, vous ne pouvez pas utiliser un autre `NSString` constante dans un contexte incorrect ; et
* permet de masquer certaines constantes, rendre complétion de code à afficher la liste des API plus court sans perdre la fonctionnalité.

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
```

À partir de la définition de liaison ci-dessus, le générateur crée le `enum` lui-même et crée également un `*Extensions` type statique qui inclut des méthodes de conversion de deux modes entre les valeurs d’énumération et la `NSString` constantes. Cela signifie que les constantes reste disponible pour les développeurs même s’ils ne sont pas partie de l’API.

Exemples :

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

Vous pouvez décorer **un** valeur enum avec cet attribut. Cela deviendra la constante qui est retournée si la valeur enum n’est pas connue.

À partir de l’exemple ci-dessus :

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Si aucune valeur d’énumération n’est décorée un `NotSupportedException` sera levée.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Codes d’erreur sont liées en tant que valeurs d’énumération. Il est généralement un domaine d’erreur pour les et il n’est pas toujours facile de trouver celle qui s’applique (ou si une même existe).

Vous pouvez utiliser cet attribut pour associer le domaine d’erreur à l’enum proprement dit.

Exemple :

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

Vous pouvez ensuite appeler la méthode d’extension `GetDomain` pour obtenir le domaine constante d’une erreur.

### <a name="fieldattribute"></a>FieldAttribute

Il s’agit du même `[Field]` attribut utilisé pour les constantes à l’intérieur du type. Il peut également servir à l’intérieur des énumérations pour mapper une valeur avec une constante spécifique.

Un `null` valeur peut être utilisée pour spécifier la valeur enum doit être retournée si un `null` `NSString` constante est spécifiée.

À partir de l’exemple ci-dessus :

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Si aucun `null` valeur est présente, une `ArgumentNullException` sera levée.

## <a name="global-attributes"></a>Attributs globaux

Les attributs globaux sont appliqués soit à l’aide de la `[assembly:]` modificateur d’attribut comme le [ `[LinkWithAttribute]` ](#LinkWithAttribute) ou peut être utilisée n’importe où, par exemple le [ `[Lion]` ](#SinceAndLionAttributes) et [ `[Since]` ](#SinceAndLionAttributes) attributs.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

Il s’agit d’un attribut de niveau assembly, ce qui permet aux développeurs de spécifier les indicateurs de liaison requis pour réutiliser une bibliothèque liée sans forcer le consommateur de la bibliothèque pour configurer manuellement le gcc_flags et les arguments mtouch supplémentaires passés à une bibliothèque.

Syntaxe :

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Cet attribut est appliqué au niveau de l’assembly, par exemple, voici à quoi le [CorePlot liaisons](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) utiliser :

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

Lorsque vous utilisez le `[LinkWith]` attribut spécifié `libraryName` est incorporée dans l’assembly résultant, permettant aux utilisateurs de livrer une même DLL qui contient les dépendances non managés ainsi que les indicateurs de ligne de commande nécessaires pour utiliser correctement le bibliothèque de Xamarin.iOS.

Il est également possible de ne pas fournir un `libraryName`, auquel cas la `LinkWith` attribut peut être utilisé pour spécifier uniquement les indicateurs de l’éditeur de liens supplémentaires :

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>Constructeurs LinkWithAttribute

Ces constructeurs permettent de spécifier la bibliothèque pour créer un lien avec et incorporer dans votre assembly résultant, les cibles prises en charge qui prend en charge de la bibliothèque et tous les indicateurs de bibliothèque qui sont nécessaires pour créer un lien avec la bibliothèque.

Notez que le `LinkTarget` argument est déduit par Xamarin.iOS et ne doit pas être définie.

Exemples :

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

Le `ForceLoad` propriété est utilisée pour décider ou non la `-force_load` indicateur de lien est utilisé pour la liaison de la bibliothèque native. Pour l’instant, il doit toujours être vraie.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Si la bibliothèque en cours de liaison a une règle stricte sur n’importe quel infrastructures (autre que `Foundation` et `UIKit`), vous devez définir le `Frameworks` propriété vers une chaîne contenant une liste délimitée par des infrastructures de plateforme nécessaire. Par exemple, si vous liez une bibliothèque qui nécessite `CoreGraphics` et `CoreText`, vous devez définir le `Frameworks` propriété `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Définissez cette propriété sur true si le fichier exécutable obtenu doit être compilée à l’aide d’un compilateur C++ au lieu de la valeur par défaut, qui est un compilateur C. À utiliser si la bibliothèque que vous liez a été écrit en C++.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Le nom de la bibliothèque non managée à regrouper. Il s’agit d’un fichier avec l’extension « .a » et il peut contenir du code d’objet pour plusieurs plateformes (par exemple, ARM et x86 pour le simulateur).

Les versions antérieures de Xamarin.iOS vérifié le `LinkTarget` propriété afin de déterminer la plateforme de votre bibliothèque de prise en charge, mais cela n’est désormais détecté automatiquement et le `LinkTarget` propriété est ignorée.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

Le `LinkerFlags` chaîne offre un moyen pour les auteurs de liaison spécifier des indicateurs de l’éditeur de liens supplémentaires nécessaires lors de la liaison de la bibliothèque native dans l’application.

Par exemple, si la bibliothèque native nécessite libxml2 et zlib, vous devez définir le `LinkerFlags` string en `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Les versions antérieures de Xamarin.iOS vérifié le `LinkTarget` propriété afin de déterminer la plateforme de votre bibliothèque de prise en charge, mais cela n’est désormais détecté automatiquement et le `LinkTarget` propriété est ignorée.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Définissez cette propriété sur true si la bibliothèque que vous liez requiert la bibliothèque de gestion des exceptions de GCC (gcc_eh)

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

Le `SmartLink` propriété doit être définie sur true pour permettre à Xamarin.iOS de déterminer si `ForceLoad` est nécessaire ou non.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

Le `WeakFrameworks` propriété fonctionne de la même façon que le `Frameworks` propriété, à ceci près qu’au moment de la liaison, le `-weak_framework` spécificateur est passé à gcc pour chacun des frameworks répertoriés.

`WeakFrameworks` rend possible pour les bibliothèques et des applications à lier faiblement sur les infrastructures de plate-forme afin qu’ils puissent les utiliser éventuellement s’ils sont disponibles, mais ne prennent pas une dépendance dure sur ces derniers, ce qui est utile si votre bibliothèque sert à ajouter des fonctionnalités supplémentaires sur plus récente versions d’iOS. Pour plus d’informations sur la liaison faible, consultez la documentation d’Apple sur [liaison faible](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Bons candidats pour la liaison faible serait `Frameworks` comme des comptes, `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` et `Twitter` dans la mesure où elles sont uniquement disponibles dans iOS 5.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) et LionAttribute (macOS)

Vous utilisez le `[Since]` attribut indicateur API comme ayant introduit à un certain point dans le temps. L’attribut doit uniquement être utilisé pour marquer des types et méthodes qui peut entraîner un problème de runtime si la classe sous-jacente, méthode ou propriété n’est pas disponible.

Syntaxe :

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Il doit en général pas être appliqué à des énumérations, des contraintes ou des nouvelles structures comme ceux pas provoque une erreur d’exécution si elles sont exécutées sur un appareil avec une version antérieure du système d’exploitation.

Exemple lorsqu’il est appliqué à un type :

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Exemple lorsqu’il est appliqué à un nouveau membre :

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

Le `[Lion]` attribut est appliqué dans la même façon, mais des types introduits avec Lion. La raison d’utiliser `[Lion]` et le numéro de version plus spécifique qui est utilisé dans iOS est que l’option iOS a été modifié très souvent, tandis que les versions principales du système d’exploitation X se produisent rarement et il est plus facile à mémoriser le système d’exploitation par leur nom de code que par leur numéro de version

### <a name="adviceattribute"></a>AdviceAttribute

Utilisez cet attribut pour permettre aux développeurs un indicateur sur les autres API qui peut être plus facile à utiliser.   Par exemple, si vous fournissez une version fortement typée d’une API, vous pouvez utiliser cet attribut sur l’attribut faiblement typée pour diriger le développeur à l’API de mieux.

Les informations à partir de cet attribut s’affichent dans la documentation et outils peuvent être développés pour fournir des suggestions des utilisateurs sur l’amélioration des

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Uniquement disponible dans Xamarin.iOS 5.4 et les versions ultérieures.

Cet attribut indique le générateur que la liaison pour cette bibliothèque spécifique (si appliqué avec `[assembly:]`) ou de type doit utiliser le marshaling des chaînes de la copie rapide de zéro. Cet attribut équivaut au passage de l’option de ligne de commande `--zero-copy` au générateur.

Lorsque vous utilisez la copie de zéro pour les chaînes, le générateur utilise efficacement les mêmes C# chaîne en tant que la chaîne Objective-C consomme sans impliquer la création d’un nouveau `NSString` objet et en évitant de copier les données à partir de la C# des chaînes à la Chaîne de objective-C. Le seul inconvénient de l’utilisation de chaînes de la copie de zéro est que vous devez vous assurer que n’importe quelle propriété de chaîne que vous encapsulez qui se produit est indiqué comme étant `retain` ou `copy` a le `[DisableZeroCopy]` ensemble d’attributs. Cette opération est nécessaire parce que le handle pour les chaînes de la copie de zéro est alloué sur la pile et n’est pas valide lors du retour de fonction.

Exemple :

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Vous pouvez également appliquer l’attribut au niveau de l’assembly, et il s’applique à tous les types de l’assembly :

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Dictionnaires fortement typées

Avec Xamarin.iOS 8.0, nous avons introduit la prise en charge pour créer facilement des classes fortement typées qui encapsulent `NSDictionaries`.

Pendant qu’il a toujours été possible d’utiliser le [DictionaryContainer](xref:Foundation.DictionaryContainer) de type de données avec une API manuelle, il est désormais beaucoup plus simple pour ce faire.  Pour plus d’informations, consultez [en exposant des Types forts](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

Quand cet attribut est appliqué à une interface, le générateur produit une classe avec le même nom que l’interface qui dérive de [DictionaryContainer](xref:Foundation.DictionaryContainer) et transforme chaque propriété définie dans l’interface dans un fortement typées méthodes getter et setter pour le dictionnaire.

Cela génère automatiquement une classe qui peut être instanciée à partir d’un existant `NSDictionary` ou qui a été créé à nouveau.

Cet attribut prend un paramètre, le nom de la classe contenant les clés utilisées pour accéder aux éléments dans le dictionnaire.   Par défaut, chaque propriété de l’interface avec l’attribut va rechercher un membre dans le type spécifié pour un nom avec le suffixe « Key ».

Exemple :

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

Dans le cas ci-dessus, le `MyOption` classe produira une propriété de chaîne pour `Name` qui utilisera le `MyOptionKeys.NameKey` comme clé dans le dictionnaire pour récupérer une chaîne.   Et utilisera le `MyOptionKeys.AgeKey` comme clé dans le dictionnaire pour récupérer un `NSNumber` qui contient un entier.

Si vous souhaitez utiliser une autre clé, vous pouvez utiliser l’attribut export sur la propriété, par exemple :

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Types de dictionnaires fort

Les types de données suivants sont pris en charge dans le `StrongDictionary` définition :

|C#Type d’interface|`NSDictionary` Type de stockage|
|---|---|
|`bool`|`Boolean` stockées dans un `NSNumber`|
|Valeurs d’énumération|Entier stocké dans un `NSNumber`|
|`int`|entier 32 bits stockée dans un `NSNumber`|
|`uint`|entier non signé 32 bits stockée dans un `NSNumber`|
|`nint`|`NSInteger` stockées dans un `NSNumber`|
|`nuint`|`NSUInteger` stockées dans un `NSNumber`|
|`long`|entier 64 bits stockée dans un `NSNumber`|
|`float`|entier 32 bits stocké en tant qu’un `NSNumber`|
|`double`|entier 64 bits stocké en tant qu’un `NSNumber`|
|`NSObject` et sous-classes|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array` de `NSObject`|`NSArray`|
|C#`Array` d’énumérations|`NSArray` contenant `NSNumber` valeurs|
