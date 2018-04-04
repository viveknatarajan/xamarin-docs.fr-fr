---
title: Conseils pour la mise à jour du Code à l’API unifiée
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b23a84c990eb2418e94b414cc9750b3060c572ad
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Conseils pour la mise à jour du Code à l’API unifiée

À l’aide de l’outil de migration automatisés dans Visual Studio pour Mac convertira iOS et projets Mac pour faire référence à l’API unifiée (Xamarin.iOS.dll ou Xamarin.Mac.dll) et effectuer un grand nombre des modifications de code nécessaires à votre place (voir le [mise en production de Xamarin Studio Notes de section 'Standard à l’outil de migration de code API unifiée'](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) pour plus d’informations).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>NSInvalidArgumentException n’a pas trouvé d’animation erreur

Il existe un [bogue](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) dans la version actuelle de Visual Studio pour Mac qui peut se produire après l’utilisation de l’outil de migration automatisée à convertir votre projet à l’API unifiée. Après la mise à jour, si vous obtenez un message d’erreur sous la forme :

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...

```

Vous pouvez effectuer les opérations suivantes pour résoudre ce problème, recherchez le fichier cible de build suivant :

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

Dans ce fichier, vous devez rechercher la déclaration suivante de la cible :

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Ajoutez le `DependsOnTargets="_CollectBundleResources"` attribut. Comme ceci :

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Enregistrez le fichier, redémarrez Visual Studio pour Mac et effectuez une nouvelle & reconstruire de votre projet. Un correctif pour résoudre ce problème doit être libéré par Xamarin peu de temps.

## <a name="useful-tips"></a>Conseils utiles

Après avoir utilisé l’outil de migration, vous pouvez toujours obtenir certaines erreurs nécessitant une intervention manuelle.
Les éléments qui doivent être corrigées manuellement sont :

* Comparaison `enum`s peut nécessiter un `(int)` cast.

* `NSDictionary.IntValue` Retourne un `nint`, il existe un `Int32Value` qui peut être utilisé à la place.

* `nfloat` et `nint` types ne peuvent pas être marqués `const`;   `static readonly nint` constitue une alternative raisonnable.

* Les éléments qui vous permettant d’être directement dans le `MonoTouch.` espace de noms sont désormais généralement dans le `ObjCRuntime.` espace de noms (par exemple : `MonoTouch.Constants.Version` est désormais `ObjCRuntime.Constants.Version`).

* Code qui sérialise les objets peuvent être supprimés lors de la tentative de sérialiser `nint` et `nfloat` types. Veillez à vérifier que votre code de sérialisation fonctionne comme prévu après la migration.

* Parfois, l’outil automatisé absences dans le code à l’intérieur `#if #else` directives de compilation conditionnelle. Dans ce cas, vous devrez apporter les corrections manuellement (voir les erreurs courantes ci-dessous).

* Les méthodes exportées manuellement à l’aide de `[Export]` ne peut pas être corrigés automatiquement par l’outil de migration, par exemple dans cette snippert de code que vous devez mettre à jour manuellement le type de retour `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * L’API unifiée ne fournit pas une conversion implicite entre NSDate et DateTime .NET, car il n’est pas une conversion sans perte. Pour éviter les erreurs liées à `DateTimeKind.Unspecified` convertir .NET `DateTime` locaux ou l’heure UTC avant d’effectuer un cast en `NSDate`.

 * Objective-C catégorie méthodes sont désormais générées en tant que méthodes d’extension dans l’API unifiée. Par exemple, le code qui utilisait précédemment `UIView.DrawString` serait maintenant référencer `NSString.DrawString` dans l’API unifiée.

 * Code à l’aide des classes AVFoundation avec `VideoSettings` de modifier et pour utiliser le `WeakVideoSettings` propriété. Cela nécessite un `Dictionary`, qui est disponible en tant que propriété sur les classes de paramètres, par exemple :

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * Le NSObject `.ctor(IntPtr)` constructeur a été modifié de public à protégé ([pour empêcher toute utilisation incorrecte](~/cross-platform/macios/unified/index.md#NSObject_ctor)).

 * `NSAction` a été [remplacé](~/cross-platform/macios/unified/index.md#NSAction) avec le .NET starndard `Action`. Des délégués simple (paramètre unique) ont également été remplacées par `Action<T>`.

Enfin, reportez-vous à la [différences d’API unifiée de classique v](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) pour rechercher les modifications apportées aux API dans votre code. Recherche [cette page](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) permettra de trouver les API classique et qu’ils ont été mise à jour.

**Remarque :** le `MonoTouch.Dialog` espace de noms reste le même après la migration. Si votre code utilise **MonoTouch.Dialog** vous devez continuer à utiliser cet espace de noms, de faire *pas* modifier `MonoTouch.Dialog` à `Dialog`!

## <a name="common-compiler-errors"></a>Erreurs de compilateur communes

Obtenir des exemples d’erreurs courantes sont répertoriées ci-dessous, ainsi que la solution :

**Erreur CS0012 : Le type 'MonoTouch.UIKit.UIView' est défini dans un assembly qui n’est pas référencé.**

Correctif : Cela signifie généralement le projet fait référence à un composant ou un package NuGet qui n’ont pas été créée avec l’API unifiée. Vous devez supprimer et rajouter tous les composants et NuGet packages. Si cela ne résout pas l’erreur, la bibliothèque externe peut prend pas en charge l’API unifiée.

**Erreur MT0034 : Ne peut pas inclure 'monotouch.dll' et 'Xamarin.iOS.dll' dans le même projet Xamarin.iOS - 'Xamarin.iOS.dll' est référencé explicitement, alors que 'monotouch.dll' est référencé par ' Xamarin.Mobile, Version = 0.6.3.0, Culture = neutral, PublicKeyToken = null'.**

Correctif : Supprimer le composant qui provoque cette erreur et ajoutez de nouveau au projet.

**Erreur CS0234 : Nom du type ou espace de noms 'Base' n’existe pas dans l’espace de noms 'MonoTouch'. Une référence d’assembly est-elle manquante ?**

Correctif : L’outil de migration automatisée dans Visual Studio pour Mac *doit* tout mettre à jour `MonoTouch.Foundation` fait référence aux `Foundation`, mais dans certains cas, elles devront être mis à jour manuellement. Des erreurs similaires peuvent apparaître pour les espaces de noms est précédemment contenues dans `MonoTouch`, tel que `UIKit`.

**Erreur CS0266 : Ne peut pas convertir implicitement le type 'double' à 'System.float'**

Correctif : modifier le type et converti en `nfloat`. Cette erreur peut également se produire pour les autres types avec les équivalents 64 bits (tels que `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**Erreur CS0266 : Impossible de convertir implicitement le type 'CoreGraphics.CGRect' à 'System.Drawing.RectangleF'. Une conversion explicite existe (un cast est-il manquant ?)**

Correctif : Modifier des instances de `RectangleF` à `CGRect`, `SizeF` à `CGSize`, et `PointF` à `CGPoint`. L’espace de noms `using System.Drawing;` doit être remplacé par `using CoreGraphics;` (si elle n’est pas déjà présente).

**erreur CS1502 : la meilleure méthode surchargée pour ' CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])' a des arguments non valides**

Correctif : Modifiez le type de tableau `nfloat[]` et cast explicitement `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Erreur CS0115 : 'WordsTableSource.RowsInSection (UIKit.UITableView, int)' est marqué comme un remplacement, mais aucune méthode adéquate trouvée pour remplacer**

Correctif : Modifier les types de paramètres et la valeur de retour pour `nint`. Cela se produit souvent dans les substitutions de méthodes telles que celles sur `UITableViewSource`, y compris `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`, etc.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Erreur CS0508 : `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

Correctif : Lorsque le type de retour est modifié en `nint`, effectuer un cast de la valeur de retour pour `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Erreur CS1061 : Type 'CoreGraphics.CGPath' ne contient pas une définition pour 'AddElipseInRect'**

Correctif : Corrigez l’orthographe `AddEllipseInRect`. Autres changements de nom sont les suivantes :

* Remplacez « Color.Black » `NSColor.Black`.
* Remplacez MapKit 'AddAnnotation' `AddAnnotations`.
* Remplacez AVFoundation 'DataUsingEncoding' `Encode`.
* Remplacez AVFoundation 'AVMetadataObject.TypeQRCode' `AVMetadataObjectType.QRCode`.
* Remplacez AVFoundation 'VideoSettings' `WeakVideoSettings`.
* Modifier PopViewControllerAnimated à `PopViewController`.
* Remplacez CoreGraphics 'CGBitmapContext.SetRGBFillColor' `SetFillColor`.

**Erreur CS0546 : substitution impossible, car 'MapKit.MKAnnotation.Coordinate' n’a pas d’accesseur set substituable (CS0546)**

Lors de la création d’une annotation personnalisée en sous-classant MKAnnotation le champ de coordonnée n’a pas d’accesseur Set, uniquement un accesseur Get.

[Corriger](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Ajouter un champ pour effectuer le suivi de la coordonnée
* Ce champ de retour dans l’accesseur Get de la propriété de coordonnée
* Substituez la méthode SetCoordinate et définissez votre champ
* Appelez SetCoordinate dans votre constructeur avec le paramètre passé de coordonnées

Il doit ressembler à ce qui suit :

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>Liens associés

- [Mise à jour des applications](~/cross-platform/macios/unified/updating-apps.md)
- [Mise à jour des applications iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mise à jour les applications Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Mise à jour des applications de Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Mise à jour des liaisons](~/cross-platform/macios/unified/update-binding.md)
- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
