---
title: API unifiée
description: Le nouveau style API rend plus facile que jamais à partager du code entre Mac et iOS, ainsi que ce qui permet de prendre en charge 32 et 64 bits des applications avec le même binaire.
ms.prod: xamarin
ms.assetid: 14311617-1BC2-42CC-AF3F-9F97733EE2D0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d0ad5b08aaf5e7b14f75611a28d389e3f7e0477c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="unified-api"></a>API unifiée

_Le nouveau style API rend plus facile que jamais à partager du code entre Mac et iOS, ainsi que ce qui permet de prendre en charge 32 et 64 bits des applications avec le même binaire._

Pour améliorer le partage de code entre Mac, iOS et pour permettre aux développeurs d’avoir une seule base de code qui fonctionne sur 32 et 64 bits dans tôt 2015 nous avons introduit une nouvelle API de produits Xamarin.Mac et Xamarin.iOS appelé l’API unifiée.

> [!IMPORTANT]
> **Désapprobation de profil classique :** que de nouvelles plateformes sont ajoutés dans Xamarin.iOS nous avons commencé à déconseiller progressivement des fonctionnalités à partir du profil classique (monotouch.dll). Par exemple, l’option non-NRC (nouvelle-ref-count) a été supprimée. NRC a toujours été activé pour unifiée de toutes les applications (c'est-à-dire non NRC n’a jamais été une option) et ne présente aucun problème connu. Les versions futures supprime la possibilité d’utiliser Boehm en tant que le garbage collector. Cela était également une option jamais disponible pour les applications unifiées. La suppression complète de la prise en charge classique est planifiée pour automne suivant avec la version de Xamarin.iOS 10.0.

## <a name="overviewoverviewmd"></a>[Vue d’ensemble](overview.md)

Décrit les raisons qui justifient l’API unifiée et explique les différences de l’API classique en détail. Reportez-vous à cette page pour comprendre les modifications qui ont été apportées dans l’API unifiée.

## <a name="updating-classic-api-based-apps"></a>Mise à jour des applications basées sur des API classiques

Suivez les instructions appropriées pour votre plateforme :

- [Mettre à jour des applications existantes](updating-apps.md)
- [Mettre à jour des applications iOS existantes](updating-ios-apps.md)
- [Mettre à jour des applications Mac existantes](updating-mac-apps.md)
- [Mettre à jour des applications Xamarin.Forms existantes](updating-xamarin-forms-apps.md)
- [Migrer une liaison vers l’API unifiée](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[Conseils pour la mise à jour du code vers l’API unifiée](updating-tips.md)

Quel que soit le fonctionnement des applications que vous migrez, l’extraction [ces conseils](updating-tips.md) pour vous aider à mettre à jour à l’API unifiée.

## <a name="library-split"></a>Fractionnement de la bibliothèque

À ce stade, nos API sera visible de deux manières :

-  **API classique :** limité à 32 bits (uniquement) et exposée dans le `monotouch.dll` et `XamMac.dll` assemblys.
-  **Une API unifiée :** prennent en charge le développement bits 32 et 64 avec une seule API disponible dans le `Xamarin.iOS.dll` et `Xamarin.Mac.dll` assemblys.

Cela signifie que pour Enterprise, les développeurs (pas ciblé du magasin d’applications), vous pouvez continuer à l’aide des API classique existante, comme nous sera conserver en conservant les indéfiniment, ou vous peuvent mettre à niveau vers les nouvelles API.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Modifications de Namespace

Pour réduire la friction pour partager du code entre nos produits Mac et iOS, nous modifions les espaces de noms pour les API dans les produits.

Nous sommes suppression le préfixe « MonoTouch » à partir de notre produit des e/s et un « MonoMac » à partir de notre produit Mac sur les types de données.

Cela facilite le partager du code entre les plateformes Mac et iOS sans avoir recours à la compilation conditionnelle et permet de réduire le bruit en haut de vos fichiers de code source.

-  **API classique :** utilisent des espaces de noms `MonoTouch.` ou `MonoMac.` préfixe.
-  **Une API unifiée :** aucun préfixe d’espace de noms

## <a name="api-changes"></a>Modifications de l’API

L’API unifiée supprime les méthodes déconseillées et qu’il existe quelques cas il y a eu des fautes de frappe dans les noms de l’API lorsqu’ils ont été liés à l’origine MonoTouch MonoMac espaces de noms et dans les API classique. Ces instances ont été corrigées dans les nouvelles API unifiée et devront être mis à jour dans votre composant, les e/s et les applications Mac. Voici une liste des causes plus courantes que vous pouvez rencontrer :

|Nom de la méthode d’API classique|Nom de la méthode d’API unifiée|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Pour obtenir une liste complète des modifications lorsque vous passez à l’API unifiée standard, consultez notre [classique (monotouch.dll) vs différences d’API d’unifié (Xamarin.iOS.dll)](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) documentation.

## <a name="updating-to-unified"></a>Mise à jour à unifiée

Plusieurs API ancien/rompu/déconseillée dans **classique** ne sont pas disponibles dans le **unifié** API. Il peut être plus facile de corriger la `CS0616` mise à niveau des avertissements avant de commencer votre (manuel ou automatique), car vous aurez le `[Obsolete]` vous guide à l’API de droite à l’attribut de message (partie de l’avertissement).

Notez que nous publions un [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) le VS classique unifiée des modifications de l’API qui peuvent être utilisées avant ou après les mises à jour de votre projet. Fixation toujours le son des appels dans les seront classique souvent vous faire gagner du temps (moins de recherches de documentation).

Suivez ces instructions pour [mettre à jour des applications iOS existantes](~/cross-platform/macios/unified/updating-ios-apps.md), ou [les applications Mac](~/cross-platform/macios/unified/updating-mac-apps.md) à l’API unifiée.
Passez en revue le reste de cette page, et [ces conseils](~/cross-platform/macios/unified/updating-tips.md) pour plus d’informations sur la migration de votre code.

### <a name="nuget"></a>NuGet

Les packages NuGet qui prenait en charge Xamarin.iOS via l’API classique publié leurs assemblys à l’aide de la **Monotouch10** moniker de la plateforme.

L’API unifiée introduit un nouvel identificateur de plateforme pour les packages compatibles - **Xamarin.iOS10**. Vous devez être mis à jour pour prendre en charge cette plateforme, par la construction par rapport à l’API unifiée les packages NuGet existants.

> [!IMPORTANT]
> Si vous avez une erreur dans le formulaire _« erreur 3 ne peut pas inclure 'monotouch.dll' et 'Xamarin.iOS.dll' dans le même projet Xamarin.iOS - 'Xamarin.iOS.dll' est référencé explicitement, alors que 'monotouch.dll' est référencé par ' xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null' «_ après la conversion de votre application à l’API unifiée, il est généralement parce qu’un composant ou un NuGet Package dans le projet n’a pas été mis à jour à l’API unifiée. Vous devez supprimer le composant existant/NuGet, mettre à jour vers une version qui prend en charge les API unifiée, puis effectuez un nettoyage de build.

### <a name="the-road-to-64-bits"></a>Déplacement à 64 Bits

Pour plus d’informations sur la prise en charge 32 et 64 bits les applications et informations sur les infrastructures de voir les [32 et 64 bits des considérations relatives à la plateforme](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types" />

#### <a name="new-data-types"></a>Nouveaux types de données

Au cœur de la différence, Mac et iOS API utiliser un type de données spécifiques à l’architecture est toujours 32 bits sur les plateformes 32 bits et 64 bits sur les plateformes 64 bits.

Par exemple, Objective-C mappe le `NSInteger` type de données à `int32_t` sur les systèmes 32 bits et en `int64_t` sur les systèmes 64 bits.

Pour correspondre à ce problème, dans notre API unifiée, nous sommes en remplaçant les utilisations précédentes de `int` (qui dans .NET est définie comme étant toujours `System.Int32`) à un type de données : `System.nint`.  Vous pouvez considérer le « n » en ce qui signifie « natifs » pour l’entier natif de type de la plateforme.

Nous vous présentons `nint`, `nuint` et `nfloat` fournissant ainsi des types de données reposant sur les cas de besoin.

Pour plus d’informations sur ces modifications de types de données, consultez la [Types natifs](~/cross-platform/macios/nativetypes.md) document.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>Comment faire pour détecter l’architecture des applications iOS

Il peut y avoir des situations où votre application a besoin de savoir si elle s’exécute sur un 32 bits ou un système d’e/s 64 bits. Le code suivant peut être utilisé pour vérifier l’architecture :

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>Tableaux et System.Collections.Generic

Étant donné que les indexeurs c# attendent un type de `int`, vous devrez effectuer un cast explicite `nint` valeurs `int` pour accéder aux éléments dans une collection ou un tableau. Par exemple :

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Ce comportement est attendu, étant donné que le cast de `int` à `nint` est avec perte de données 64 bits, une conversion implicite est.

### <a name="converting-datetime-to-nsdate"></a>Conversion de DateTime en NSDate

Lorsque vous utilisez l’API unifiée, la conversion implicite de `DateTime` à `NSDate` valeurs n’est plus effectuée. Ces valeurs devrez être explicitement converti à partir d’un type à un autre. Les méthodes d’extension peuvent être utilisés pour automatiser ce processus :

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

À l’intérieur de Xamarin.iOS API classique (monotouch.dll) le `[Obsolete]` attribut a été utilisé de deux manières différentes :

-  **Déconseillé API iOS :** c’est lorsque les indicateurs d’Apple pour vous arrêter à l’aide d’une API, car il est en cours remplacée par une version plus récente. L’API classique est toujours correctement et souvent requises (si vous prenez en charge la version antérieure d’e/s).
 Ce API (et le `[Obsolete]` attribut) sont inclus dans les nouveaux assemblys Xamarin.iOS.
-  **API incorrect** certaines API avait des fautes de frappe sur leurs noms.

Pour les assemblys d’origine (monotouch.dll et XamMac.dll), nous avons conservé l’ancien code disponible pour la compatibilité, mais ils ont été supprimés à partir des assemblys API unifiée (Xamarin.iOS.dll et Xamarin.Mac)

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject sous-classes .ctor(IntPtr)

Chaque `NSObject` sous-classe possède un constructeur qui accepte un `IntPtr`. Voici comment nous pouvons instancier une nouvelle instance de managé à partir d’un handle ObjC natif.

Dans classic il s’agissait d’un `public` constructeur. Toutefois, il est facile d’utilisation malveillante de cette fonctionnalité dans le code utilisateur, par exemple, créez plusieurs les instances managées pour une seule instance ObjC *ou* création d’une instance managée qui serait ne disposent pas de l’état attendu managé (pour les sous-classes).

Pour éviter ce genre de problèmes le `IntPtr` constructeurs sont maintenant `protected` dans **unifiée** API, pour être utilisé uniquement pour le sous-classement. Cela permet de garantir que la corriger/safe API permet de créer d’instance gérée à partir des handles, c'est-à-dire

    var label = Runtime.GetNSObject<UILabel> (handle);

Cette API retourne une instance managée existante (s’il existe déjà) ou va créer un nouveau (si nécessaire). Il est déjà disponible dans l’API classique et unifiée.

Notez que la `.ctor(NSObjectFlag)` est maintenant aussi `protected` , mais celle-ci était rarement utilisé en dehors de la création de sous-classes.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction remplacée par Action

Avec les API unifiée, `NSAction` a été supprimé en faveur de .NET standard `Action`. Cela constitue une amélioration notable car `Action` est un type .NET commun, alors que `NSAction` était spécifique à Xamarin.iOS. Ils sont tous deux font la même chose, mais ils étaient des types distincts et incompatible et a généré dans le code plus devoir être établies pour obtenir le même résultat.

Par exemple, si votre application Xamarin existante inclus le code suivant :

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Il peut être remplacé par une expression lambda simple :

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Précédemment qui serait une erreur du compilateur, car un `Action` ne peut pas être assigné à `NSAction`, mais puisque `UITapGestureRecognizer` accepte désormais un `Action` au lieu d’un `NSAction` il n’est valide dans les API unifiée.

### <a name="custom-delegates-replaced-with-actiont"></a>Remplacé par l’Action des délégués personnalisés<T>

Dans **unifiée** certaines simples (par exemple, un paramètre) délégués .net ont été remplacés par `Action<T>`. Par exemple,

    public delegate void NSNotificationHandler (NSNotification notification);

peut désormais être utilisé comme un `Action<NSNotification>`. Ce code de promotion réutiliser et réduire la duplication de code à l’intérieur de Xamarin.iOS et de vos propres applications.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Tâche<bool> remplacé par la tâche < booléen, NSError >>

Dans **classique** a été certaines API async retournant `Task<bool>`. Toutefois certains d'entre eux où sont à utiliser lors une `NSError` faisait partie de la signature, par exemple, le `bool` était déjà `true` et vous deviez intercepter une exception pour obtenir le `NSError`.

Étant donné que certaines erreurs sont très courantes et la valeur de retournée n’est pas utile ce modèle a été modifié dans **unifiée** pour renvoyer un `Task<Tuple<Boolean,NSError>>`. Cela vous permet de vérifier la réussite et toute erreur qui a pu se produire lors de l’appel asynchrone.

### <a name="nsstring-vs-string"></a>Chaîne de vs NSString

Dans certains cas certaines constantes devaient être modifié à partir de `string` à `NSString`, par exemple `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

En règle générale, nous préférons .NET `System.String` type. Toutefois, malgré les recommandations d’Apple, certaines API native comparez pointeurs constants (pas la chaîne) et que cela ne fonctionnent que lorsque nous exposer l’une des constantes en tant que `NSString`.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Protocoles objective-C

Le MonoTouch d’origine n’avait pas de prise en charge complète des protocoles de ObjC d’autres, non optimales, les API ont été ajoutées pour prendre en charge la plupart des cas. Cette limitation n’existe pas plus mais, pour la compatibilité descendante, plusieurs API est conservées à l’intérieur de `monotouch.dll` et `XamMac.dll`.

Ces limitations ont été supprimées et nettoyées sur les API unifiée. La plupart des modifications doit ressembler à ceci :

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

Le `I` préfixe signifie **unifiée** exposer une interface, au lieu d’un type spécifique, pour le protocole ObjC. Cela facilitera le cas où vous ne souhaitez pas sous-classe le type spécifique fourni par Xamarin.iOS.

Il est également autorisé certaines API pour être le plus précis et plus facile à utiliser, par exemple :

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Ce API sont désormais plus faciles à us, sans référence à la documentation et exécution de votre code IDE vous fournira plus utiles suggestions basées sur le protocole / l’interface.

#### <a name="nscoding-protocol"></a>Protocole de NSCoding

Notre liaison d’origine inclus un .ctor(NSCoder) pour chaque type - même si elle ne prenait pas en charge la `NSCoding` protocole.  Un seul `Encode(NSCoder)` méthode n’est pas présente dans le `NSObject` pour encoder l’objet.
Mais cette méthode fonctionne uniquement si l’instance conforme NSCoding protocole.

Sur l’API unifiée nous avons résolu ce problème.  Les nouveaux assemblys aura uniquement le `.ctor(NSCoder)` si le type est conforme à `NSCoding`. Ces types ont désormais également un `Encode(NSCoder)` méthode qui est conforme à la `INSCoding` interface.

Faible Impact : Dans la plupart des cas cette modification n’affecte pas les applications que les constructeurs ancien, supprimés, n’a pas pu être utilisés.

## <a name="further-tips"></a>Conseils supplémentaires

Des modifications supplémentaires à connaître sont répertoriées dans le [conseils de mise à jour des applications à l’API unifiée](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Exemple de code

Depuis le 31 juillet, nous avons publié des ports des exemples de cette nouvelle API iOS sur le `magic-types` créer une branche à [monotouch-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Pour Mac, nous vérifions les exemples à la fois dans le [mac-samples](https://github.com/xamarin/mac-samples) référentiel (affichant les nouvelles API dans Mavericks/Yosemite), ainsi que des exemples 32/64 bits dans la branche de types de magic [mac-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).
-->

## <a name="related-links"></a>Liens associés

- [Mise à jour des applications iOS](updating-ios-apps.md)
- [Mise à jour les applications Mac](updating-mac-apps.md)
- [Mise à jour des applications de Xamarin.Forms](updating-xamarin-forms-apps.md)
- [Mise à jour des liaisons](update-binding.md)
- [Conseils de mise à jour](updating-tips.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
