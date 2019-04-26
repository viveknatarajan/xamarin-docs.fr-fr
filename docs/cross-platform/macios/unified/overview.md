---
title: Présentation de l’API unifiée
description: API unifiée de Xamarin permet de partager du code entre les applications Mac et iOS et prise en charge 32 et 64 bits avec le même fichier binaire.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 1d159d280bd3b8855c32e3e437dfdefcbe0463cb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261127"
---
# <a name="unified-api-overview"></a>Présentation de l’API unifiée

API unifiée de Xamarin permet de partager du code entre les applications Mac et iOS et prise en charge 32 et 64 bits avec le même fichier binaire. L’API unifiée est utilisé par défaut dans les nouveaux projets Xamarin.iOS et Xamarin.Mac.

> [!IMPORTANT]
> L’API classique Xamarin, ce qui a précédé l’API unifiée, a été déconseillée. 
> - La dernière version de Xamarin.iOS pour prendre en charge de l’API classique (monotouch.dll) a été 9.10 de Xamarin.iOS.
> - Xamarin.Mac prend toujours en charge l’API classique, mais il n’est plus mis à jour. Dans la mesure où il est déconseillé, les développeurs doivent déplacer leurs applications vers l’API unifiée.

## <a name="updating-classic-api-based-apps"></a>La mise à jour des applications basées sur API classiques

Suivez les instructions appropriées pour votre plateforme :

- [Mettre à jour des applications existantes](updating-apps.md)
- [Mettre à jour des applications iOS existantes](updating-ios-apps.md)
- [Mettre à jour des applications Mac existantes](updating-mac-apps.md)
- [Mettre à jour des applications Xamarin.Forms existantes](updating-xamarin-forms-apps.md)
- [Migrer une liaison vers l’API unifiée](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[Conseils pour la mise à jour du code vers l’API unifiée](updating-tips.md)

Quel que soit les applications que vous effectuez une migration, consultez [ces conseils](updating-tips.md) pour vous aider à mettre à jour correctement vers l’API unifiée.

## <a name="library-split"></a>Fractionnement de la bibliothèque

À ce stade, nos API est visibles de deux manières :

-  **API classique :** Limité à 32 bits (uniquement) et exposées dans le `monotouch.dll` et `XamMac.dll` assemblys.
-  **API unifiée :** Prend en charge le développement bits 32 et 64 avec une seule API disponible dans le `Xamarin.iOS.dll` et `Xamarin.Mac.dll` assemblys.

Cela signifie que pour l’entreprise, les développeurs (pas ciblant l’App Store), vous pouvez continuer à l’aide de l’API classique existant, comme nous sera conserver respectant les indéfiniment, ou vous peuvent mettre à niveau vers les nouvelles API.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Modifications de Namespace

Pour réduire la friction pour partager du code entre nos produits Mac et iOS, nous allons modifier les espaces de noms pour les API dans les produits.

Nous allons supprimer le préfixe « MonoTouch » à partir de notre produit iOS et le « MonoMac » à partir de notre produit Mac sur les types de données.

Cela rend plus simple de partager du code entre les plateformes Mac et iOS sans avoir recours à la compilation conditionnelle et réduit le bruit en haut de vos fichiers de code source.

-  **API classique :** Utilisent des espaces de noms `MonoTouch.` ou `MonoMac.` préfixe.
-  **API unifiée :** Aucun préfixe d’espace de noms

## <a name="runtime-defaults"></a>Valeurs par défaut du runtime

L’API unifiée par défaut, utilise le **SGen** RÉCUPÉRATEUR de mémoire et le [nouveau décompte de références](~/ios/internals/newrefcount.md) système pour le suivi de la propriété d’objet. Cette même fonctionnalité a été déplacée vers Xamarin.Mac.

Cela résout un certain nombre de problèmes que les développeurs face à l’ancien système et également facilitent [gestion de la mémoire](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Notez qu’il est possible d’activer de nouveau Refcount même pour l’API classique, mais la valeur par défaut est conservateur et ne nécessite pas les utilisateurs d’apporter des modifications. Avec l’API unifiée, nous avons profité de la modification de la valeur par défaut et permettre aux développeurs de toutes les améliorations en même temps qu’ils refactoriser et testez de nouveau leur code.

## <a name="api-changes"></a>Modifications de l’API

L’API unifiée supprime des méthodes déconseillées et il existe quelques cas où il y avait des fautes de frappe dans les noms d’API lorsqu’ils ont été liés à l’origine MonoTouch MonoMac espaces de noms et dans les API classique. Ces instances ont été corrigées dans les nouvelles API unifiée et devront être mise à jour dans votre composant, iOS et les applications Mac. Voici une liste des causes plus courantes que vous pouvez rencontrer :

|Nom de la méthode d’API classique|Nom de la méthode d’API unifiée|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Pour obtenir une liste complète des modifications lorsque vous passez du modèle Classic vers l’API unifiée, consultez notre [classique (monotouch.dll) vs différences d’API d’unifié (Xamarin.iOS.dll)](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) documentation.

## <a name="updating-to-unified"></a>La mise à jour à unifiée

Plusieurs API ancien/rompu/déconseillées dans **classic** ne sont pas disponibles dans le **unifiés** API. Il peut être plus facile à corriger le `CS0616` mise à niveau des avertissements avant de commencer votre (manuelle ou automatique) dans la mesure où vous avez le `[Obsolete]` vous guide à l’API appropriée à l’attribut de message (partie de l’avertissement).

Notez que nous publions un [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) le VS classique unifiée des modifications de l’API qui peuvent être utilisées avant ou après les mises à jour de votre projet. Fixation toujours l’obsolètes ; appels du va de classique souvent vous faire gagner du temps (moins de recherches de documentation).

Suivez ces instructions pour [mettre à jour des applications iOS existantes](~/cross-platform/macios/unified/updating-ios-apps.md), ou [applications Mac](~/cross-platform/macios/unified/updating-mac-apps.md) vers l’API unifiée.
Passez en revue le reste de cette page, et [ces conseils](~/cross-platform/macios/unified/updating-tips.md) pour plus d’informations sur la migration de votre code.

### <a name="nuget"></a>NuGet

Les packages NuGet qui prenait en charge Xamarin.iOS via l’API classique publié leurs assemblys à l’aide de la **Monotouch10** moniker de la plateforme.

L’API unifiée introduit un nouvel identificateur de plateforme pour les packages compatibles - **Xamarin.iOS10**. Les packages NuGet existants devra être mise à jour pour prendre en charge cette plateforme, en vous appuyant sur l’API unifiée.

> [!IMPORTANT]
> Si vous avez une erreur dans le formulaire _« erreur 3 ne peut pas inclure 'monotouch.dll' et 'Xamarin.iOS.dll' dans le même projet Xamarin.iOS : 'Xamarin.iOS.dll' est référencé explicitement, alors que « monotouch.dll » est référencé par ' xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null » «_ après la conversion de votre application à l’API unifiée, il est généralement dû à avoir un composant ou un NuGet Package dans le projet qui n’a pas été mis à jour vers l’API unifiée. Vous devez supprimer le composant existant/NuGet, mise à jour vers une version qui prend en charge les API unifiée, puis effectuez une génération propre.

### <a name="the-road-to-64-bits"></a>La route à 64 Bits

Pour plus d’informations sur la prise en charge 32 et 64 bits applications et informations sur les infrastructures, consultez le [32 et 64 bits des considérations relatives aux plateformes](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types" />

#### <a name="new-data-types"></a>Nouveaux types de données

Au cœur de la différence, Mac et iOS API utilisent un type de données spécifiques à l’architecture est toujours 32 bits sur les plateformes 32 bits et 64 bits sur les plateformes 64 bits.

Par exemple, Objective-C mappe le `NSInteger` type de données à `int32_t` sur les systèmes 32 bits et en `int64_t` sur les systèmes 64 bits.

Pour faire correspondre ce comportement, sur notre API unifiée, nous sommes en remplaçant les utilisations précédentes de `int` (qui, dans .NET est défini comme étant toujours `System.Int32`) en un nouveau type de données : `System.nint`.  Vous pouvez considérer le « n » comme signifiant « native », afin du natif type entier de la plateforme.

Nous vous présentons `nint`, `nuint` et `nfloat` fournit également des types de données conçues pour eux lorsque cela est nécessaire.

Pour en savoir plus sur ces modifications de type de données, consultez le [Types natifs](~/cross-platform/macios/nativetypes.md) document.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>Comment faire pour détecter l’architecture des applications iOS

Il peut exister des situations où votre application a besoin de savoir si elle est en cours d’exécution sur un 32 bits ou un système d’e/s de 64 bits. Le code suivant peut être utilisé pour vérifier l’architecture :

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>Tableaux et System.Collections.Generic

Étant donné que C# indexeurs attendent un type de `int`, vous devrez effectuer un cast explicite `nint` valeurs `int` pour accéder aux éléments dans une collection ou un tableau. Exemple :

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Il s’agit du comportement attendu, étant donné que le cast de `int` à `nint` est avec perte de données 64 bits, une conversion implicite est contraire.

### <a name="converting-datetime-to-nsdate"></a>Conversion de DateTime à NSDate

Lorsque vous utilisez l’API unifiée, la conversion implicite de `DateTime` à `NSDate` valeurs n’est plus effectuée. Ces valeurs seront doivent être explicitement converti à partir d’un type vers un autre. Les méthodes d’extension suivante peuvent être utilisés pour automatiser ce processus :

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos" />

### <a name="deprecated-apis-and-typos"></a>Fautes de frappe et les API déconseillées

API classique de Xamarin.iOS à l’intérieur (monotouch.dll) le `[Obsolete]` attribut a été utilisé de deux manières différentes :

-  **API déconseillées iOS :** C’est lorsque les indicateurs d’Apple pour vous arrêter à l’aide d’une API, car elle est en cours remplacée par une version plus récente. L’API classique est toujours pas un problème et souvent nécessaire (si vous prenez en charge la version antérieure d’e/s).
 Ce API (et le `[Obsolete]` attribut) sont inclus dans les nouveaux assemblys Xamarin.iOS.
-  **API incorrect** certaines API avait des fautes de frappe sur leurs noms.

Pour les assemblys d’origine (monotouch.dll et XamMac.dll), nous avons conservé l’ancien code disponible pour la compatibilité, mais ils ont été supprimés à partir des assemblys API unifiée (Xamarin.iOS.dll et Xamarin.Mac)

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject sous-classes .ctor(IntPtr)

Chaque `NSObject` sous-classe possède un constructeur qui accepte un `IntPtr`. Voici comment nous pouvons instancier une nouvelle instance de managé à partir d’un handle ObjC natif.

Dans classique que c’était un `public` constructeur. Toutefois, il était facile d’abuser de cette fonctionnalité dans le code utilisateur, par exemple, création de plusieurs des instances managées pour une seule instance ObjC *ou* création d’une instance gérée qui serait ne disposent pas de l’état géré attendu (pour les sous-classes).

Pour éviter ce genre de problèmes la `IntPtr` constructeurs sont désormais `protected` dans **unifiée** API, qui sera utilisée uniquement pour le sous-classement. Cela garantit que la corriger/safe API permet de créer d’instance gérée à partir des handles, par exemple

    var label = Runtime.GetNSObject<UILabel> (handle);

Cette API retourne une instance gérée existante (si elle existe déjà) ou en crée un nouveau (si nécessaire). Il est déjà disponible dans l’API classique et unifiée.

Notez que le `.ctor(NSObjectFlag)` est désormais également `protected` mais celui-ci a été rarement utilisé en dehors de sous-classement.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction remplacée par Action

Avec les API unifiée, `NSAction` a été supprimé en faveur de .NET standard `Action`. Cela constitue une amélioration notable, car `Action` est un type commun de .NET, tandis que `NSAction` était spécifique à Xamarin.iOS. Les deux font exactement la même chose, mais ils étaient des types distincts et incompatibles et ont abouti à code plus avoir à écrire pour atteindre le même résultat.

Par exemple, si votre application Xamarin existante inclus le code suivant :

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Il peut maintenant être remplacée par une expression lambda simple :

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Précédemment qui serait une erreur du compilateur, car un `Action` ne peut pas être assigné à `NSAction`, mais comme `UITapGestureRecognizer` accepte désormais un `Action` au lieu d’un `NSAction` est valide dans les API unifiée.

### <a name="custom-delegates-replaced-with-actiont"></a>Remplacé par Action des délégués personnalisés<T>

Dans **unifiée** certaines simples (par exemple, un seul paramètre) délégués .net ont été remplacés par `Action<T>`. Par exemple,

    public delegate void NSNotificationHandler (NSNotification notification);

peut maintenant être utilisé comme un `Action<NSNotification>`. Ce code de promotion réutiliser et réduire la duplication de code à l’intérieur de Xamarin.iOS et vos propres applications.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Tâche<bool> remplacé par la tâche < Boolean, NSError >>

Dans **classic** portaient sur certaines API asynchrones retournant `Task<bool>`. Néanmoins, certains d'entre eux où sont à utiliser lorsque un `NSError` faisait partie de la signature, par exemple, le `bool` était déjà `true` et que vous deviez intercepter une exception pour obtenir le `NSError`.

Étant donné que certaines erreurs sont très courantes et la valeur de retour n’a pas été utile ce modèle a été modifié dans **unifiée** pour retourner un `Task<Tuple<Boolean,NSError>>`. Cela vous permet de vérifier la réussite et toute erreur qui a pu se produire lors de l’appel asynchrone.

### <a name="nsstring-vs-string"></a>Chaîne de vs chaîne NSString

Dans certains cas certaines constantes devaient être modifié à partir de `string` à `NSString`, par exemple `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unifiée**

    public virtual NSString ReuseIdentifier { get; }

En règle générale, nous préférons .NET `System.String` type. Toutefois, malgré les instructions d’Apple, certaines API native comparez pointeurs constante (pas la chaîne elle-même) et cela peut fonctionner uniquement lorsque nous exposons les constantes en tant que `NSString`.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Protocoles objective-C

Le MonoTouch d’origine n’avait pas de prise en charge complète pour les protocoles ObjC d’autres, non optimales, les API ont été ajoutées pour prendre en charge le scénario le plus courant. Cette limitation n’existe pas de plus, mais, pour la compatibilité descendante, plusieurs API est conservés à l’intérieur de `monotouch.dll` et `XamMac.dll`.

Ces limitations ont été supprimées et nettoyées sur les API unifiée. La plupart des modifications doit ressembler à ceci :

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unifiée**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

Le `I` préfixe signifie **unifiée** exposer une interface, au lieu d’un type spécifique, pour le protocole ObjC. Cela facilitera les cas où vous ne souhaitez pas sous-classe le type spécifique qui a fourni de Xamarin.iOS.

Il est également autorisé certaines API pour être plus précis et facile à utiliser, par exemple :

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unifiée**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Ce API sont désormais plus faciles à nous, sans faire référence à la documentation, et la saisie semi-automatique du code de votre IDE vous fournira plus utiles suggestions basées sur la protocole/une interface.

#### <a name="nscoding-protocol"></a>Protocole de NSCoding

Notre liaison d’origine inclus un .ctor(NSCoder) pour chaque type - même si elle ne prenait pas en charge la `NSCoding` protocole.  Un seul `Encode(NSCoder)` méthode était présente dans le `NSObject` pour encoder l’objet.
Mais cette méthode fonctionne uniquement si l’instance conformés aux NSCoding protocole.

Sur l’API unifiée nous avons résolu ce problème.  Les nouveaux assemblys aura uniquement le `.ctor(NSCoder)` si le type est conforme à `NSCoding`. Outre ces types ont maintenant un `Encode(NSCoder)` méthode qui est conforme à la `INSCoding` interface.

Impact faible : Dans la plupart des cas cette modification n’affecte pas les applications comme les constructeurs ancien, supprimés, n’a pas pu être utilisés.

## <a name="further-tips"></a>D’autres conseils

Des modifications supplémentaires à connaître sont répertoriées dans le [conseils pour la mise à jour des applications à l’API unifiée](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Exemple de code

À compter du 31 juillet, nous avons publié des exemples iOS pour cette nouvelle API, les ports sur le `magic-types` créer une branche à [monotouch-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Pour Mac, nous vérifions les exemples à la fois dans le [mac-samples](https://github.com/xamarin/mac-samples) référentiel (affichant les nouvelles API dans Mavericks/Yosemite), ainsi que des échantillons 32/64 bits dans la branche de types de magie [mac-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>Liens associés

- [La mise à jour des applications iOS](updating-ios-apps.md)
- [La mise à jour des applications Mac](updating-mac-apps.md)
- [La mise à jour des applications Xamarin.Forms](updating-xamarin-forms-apps.md)
- [La mise à jour de liaisons](update-binding.md)
- [Conseils de mise à jour](updating-tips.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
