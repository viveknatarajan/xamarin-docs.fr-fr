---
title: "Introduction à la 3D Touch"
description: "Cet article couvre l’utilisation de la nouvelle iPhone 6 s et iPhone 6 s Plus 3D Touch mouvements dans votre application."
ms.topic: article
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 66cc67b38d70992fe815732407317fab3dc52528
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-3d-touch"></a>Introduction à la 3D Touch

_Cet article couvre l’utilisation de la nouvelle iPhone 6 s et iPhone 6 s Plus 3D Touch mouvements dans votre application._

[ ![](3d-touch-images/info01.png "Exemples de 3D Touch des applications activé")](3d-touch-images/info01.png)

Cet article fournit et introduction à l’aide des API Touch 3D nouveau pour ajouter des mouvements de pression sensibles à vos applications Xamarin.iOS qui sont exécutent sur le nouveau iPhone 6 s et iPhone 6 s ainsi que les appareils.

Avec 3D Touch, une application iPhone peut désormais non seulement indiquer que l’utilisateur touche l’écran du périphérique, mais il est en mesure de détecter la quantité l’utilisateur est exerçant une pression et répondre aux niveaux de pression différents.

3D Touch fournit les fonctionnalités suivantes à votre application :

- [Sensibilité à la pression](#Pressure-Sensitivity) - applications peuvent mesurer maintenant manuels ou lumière l’utilisateur touche l’écran et profitez de ces informations.
  Par exemple, une application de peinture peut rendre une ligne épaisse ou plus fin en fonction de laquelle l’utilisateur touche l’écran.
- [Lire et Pop](#Peek-and-Pop) -votre application peut permettent désormais d’interagir avec ses données sans avoir à naviguer en dehors de leur contexte actuel de l’utilisateur. En appuyant sur disque dur sur l’écran de l’écran, il peuvent lire l’élément qui que les intéresse (par exemple, pour afficher un aperçu d’un message). En appuyant sur plus difficile, elles consiste à faire apparaître dans l’élément.
- [Actions rapides](#Quick-Action) -considérer des Actions rapides, comme les menus contextuels qui peuvent être dépilés à distance lorsqu’un utilisateur clique sur un élément dans une application de bureau.
  À l’aide des Actions rapides, vous pouvez ajouter des raccourcis vers les fonctions dans votre application directement à partir de l’icône d’application sur l’écran d’accueil.
- [Dans le simulateur de test 3D Touch](#Testing-3D-Touch-in-the-Simulator) -avec le matériel Mac, vous pouvez tester des applications tactile 3D dans le simulateur iOS.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Pression

Comme indiqué précédemment, à l’aide des nouvelles propriétés de la [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) classe que vous pouvez mesurer la quantité de pression de l’application de l’utilisateur à écran de l’appareil iOS et utiliser ces informations dans votre interface utilisateur. Par exemple, rendre un trait de pinceau plus transparent ou opaque en fonction de la quantité de pression.

[ ![](3d-touch-images/pressure01.png "Un trait de pinceau rendu plus transparent ou opaque selon la quantité de pression")](3d-touch-images/pressure01.png)

À la suite de 3D Touch, si votre application s’exécute sur iOS 9 (ou version ultérieure) et l’appareil iOS est capable de prise en charge 3D Touch, modifications apportées à une sollicitation de la provoqueront la `TouchesMoved` événement soit déclenché.

Par exemple, lors de l’analyse du `TouchesMoved` événements d’un [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), vous pouvez utiliser le code suivant pour obtenir la pression actuelle qui applique l’utilisateur à l’écran :

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

Le `MaximumPossibleForce` propriété retourne la valeur la plus élevée possible pour le `Force` propriété de la [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) basé sur l’appareil iOS qui l’application est en cours d’exécution.

> [!IMPORTANT]
> **Remarque :** entraînent des modifications apportées à une sollicitation de la la `TouchesMoved` événement soit déclenché, même si X / Y coordonnées n’ont pas changé. En raison de ce changement de comportement, vos applications iOS doivent être préparées pour la `TouchesMoved` événements à appeler plus souvent et pour le X / coordonnées Y être le même que le dernier `TouchesMoved` appeler.




Pour plus d’informations, consultez le site d’Apple [TouchCanvas : à l’aide de UITouch efficacement](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) exemple d’application et [référence de classe UITouch](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Lire et Pop

3D Touch fournit de nouvelles façons pour un utilisateur d’interagir avec les informations dans votre application plus rapide que jamais, sans avoir à accéder à partir de leur emplacement actuel.

Par exemple, si votre application affiche une table des messages, l’utilisateur peut appuyer sur dur sur un élément pour afficher un aperçu de son contenu dans un mode de superposition (laquelle Apple désigne comme un *aperçu*).

[ ![](3d-touch-images/peekandpop01.png "Un exemple de lecture au contenu")](3d-touch-images/peekandpop01.png)

Si l’utilisateur appuie sur plus difficile, celui-ci doit entrer la vue de message normal (qui est appelé *Pop*-ping dans la vue).

### <a name="checking-for-3d-touch-availability"></a>Vérifie la disponibilité de la 3D Touch

Lorsque vous travaillez avec un [UIViewController]() vous pouvez utiliser le code suivant pour voir si l’appareil iOS, l’application est en cours d’exécution prend en charge 3D Touch :

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Cette méthode peut être appelée avant *ou après* `ViewDidLoad()`. 

### <a name="handling-peek-and-pop"></a>POP et gestion d’aperçu

Sur un appareil iOS qui peut gérer 3D Touch, nous pouvons utiliser une instance de la `UIViewControllerPreviewingDelegate` pour gérer l’affichage de classe **aperçu** et **Pop** détails d’article. Par exemple, si nous avons eu un contrôleur de la vue Table appelée `MasterViewController ` nous pouvons utiliser le code suivant pour prendre en charge **aperçu** et **Pop**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

Le `GetViewControllerForPreview` méthode est utilisée pour effectuer le **aperçu** opération. Il peut accéder aux données de cellule et de stockage de la table, puis charge le `DetailViewController` à partir de l’animation en cours. En définissant le `PreferredContentSize` (0,0), nous demandons pour la valeur par défaut **aperçu** taille de l’affichage. Enfin, nous avons tout sauf la cellule nous affichons avec flou `previewingContext.SourceRect = cell.Frame` et nous renvoyons le nouvel affichage pour l’affichage.

Le `CommitViewController` réutilise la vue que vous avez créée à le **aperçu** pour le **Pop** afficher lorsque l’utilisateur appuie plus difficile.

### <a name="registering-for-peek-and-pop"></a>L’enregistrement pour lire et Pop

À partir du contrôleur de la vue que vous souhaitez autoriser l’utilisateur à **aperçu** et **Pop** éléments à partir de, nous avons besoin pour vous inscrire à ce service. Dans l’exemple ci-dessus d’un contrôleur de vue de Table (`MasterViewController`), nous, utilisez le code suivant :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Ici, nous appelons le `RegisterForPreviewingWithDelegate` méthode avec une instance de la `PreviewingDelegate` créée précédemment. Sur les appareils iOS qui prennent en charge 3D Touch, l’utilisateur peut appuyer sur dur sur un élément pour lire sur celui-ci. Si elles appuyez sur encore plus difficile, l’élément s’affiche dans ce standard l’affichage.

Pour plus d’informations, consultez notre [iOS 9 ApplicationShortcuts exemple](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) et d’Apple [ViewControllerPreviews : à l’aide de l’aperçu des API de UIViewController](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) exemple d’application, [ Référence de classe de UIPreviewAction](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [UIPreviewActionGroup classe référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) et [UIPreviewActionItem Protocol Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Actions rapides

À l’aide de 3D tactiles et des Actions rapides, vous pouvez ajouter courantes, rapide et facile aux raccourcis d’accès à des fonctions dans votre application à partir de l’icône d’écran d’accueil sur l’appareil iOS.

Comme indiqué ci-dessus, vous pouvez considérer Actions rapides comme les menus contextuels qui peuvent être dépilés à distance lorsqu’un utilisateur clique sur un élément dans une application de bureau. Vous devez utiliser des Actions rapides pour fournir des raccourcis vers les fonctions ou les fonctionnalités de votre application plus courantes.

[ ![](3d-touch-images/quickactions01.png "Un exemple d’un menu d’Actions rapides")](3d-touch-images/quickactions01.png)

<a name="Defining-Static-Quick-Actions" />

### <a name="defining-static-quick-actions"></a>Définition des Actions rapides statiques

Si un ou plusieurs des Actions rapides requis par votre application sont statiques et n’avez pas besoin de les modifier, vous pouvez les définir dans l’application `Info.plist` fichier. Modifier ce fichier dans un éditeur externe et ajoutez les clés suivantes :

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

Nous examinons ici définir deux éléments d’Action rapide statiques avec les clés suivantes :

- `UIApplicationShortcutItemIconType` -Définit l’icône qui sera affichée par l’élément Action rapide comme l’une des valeurs suivantes :
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -Définit le sous-titre pour l’élément.
* `UIApplicationShortcutItemTitle` -Définit le titre de l’élément.
* `UIApplicationShortcutItemType` -Est une valeur de chaîne que nous utiliserons pour identifier l’élément dans notre application. Pour plus d'informations, consultez la section suivante.

> [!IMPORTANT]
> **Remarque :** éléments contextuel Action rapide qui sont définies dans le `Info.plist` ne peut pas accéder au fichier avec le `Application.ShortcutItems` propriété. Ils sont transmis uniquement à la `HandleShortcutItem` Gestionnaire d’événements. 




<a name="Identifying-Quick-Action-Items" />

### <a name="identifying-quick-action-items"></a>Identification des éléments d’Action rapide

Comme vous l’avez vu ci-dessus, lorsque vous avez défini vos éléments d’Action rapide dans votre application `Info.plist`, vous avez affecté la valeur de chaîne à la `UIApplicationShortcutItemType` clé pour les identifier.

Pour simplifier ces identificateurs à utiliser dans le code, ajoutez une classe appelée `ShortcutIdentifier` de projet de votre application et qu’elle ressemble à ce qui suit :

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Gestion d’une Action rapide

Ensuite, vous devez modifier votre application `AppDelegate.cs` fichier pour gérer l’utilisateur en sélectionnant un élément d’Action rapide à partir de l’icône de votre application sur l’écran d’accueil.

Effectuez les modifications suivantes :

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

Tout d’abord, nous définissons un public `LaunchedShortcutItem` propriété pour suivre le dernier élément d’Action rapide sélectionné par l’utilisateur. Ensuite, nous remplaçons le `FinishedLaunching` méthode et voir si `launchOptions` ont été passées et si elles contient un élément Action rapide. Dans le cas, nous stockons l’Action rapide dans le `LaunchedShortcutItem` propriété.

Ensuite, nous remplaçons le `OnActivated` méthode et passe les sélectionné d’élément de menu de lancement rapide à la `HandleShortcutItem` méthode à être traitées. Actuellement nous écrivons uniquement un message à la **Console**. Dans une application réelle, vous pouvez gérer le scénario ever action était nécessaire. Une fois que l’action a été effectuée, le `LaunchedShortcutItem` propriété est désactivée.

Enfin, si votre application a été déjà en cours d’exécution, le `PerformActionForShortcutItem` méthode serait appelée pour gérer l’élément Action rapide afin de nous devons remplacer et appeler notre `HandleShortcutItem` méthode ici également.

<a name="Creating-Dynamic-Quick-Action-Items" />

### <a name="creating-dynamic-quick-action-items"></a>Création d’actions rapide dynamique

Outre la définition d’Action rapide statique d’éléments dans votre application `Info.plist` fichier, vous pouvez créer dynamique à la volée des Actions rapides. Pour définir deux nouvelles Actions rapide dynamique, modifiez votre `AppDelegate.cs` à nouveau de fichiers et de modifier le `FinishedLaunching` méthode à rechercher comme suit :

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Maintenant nous vérifions si le `application` contient déjà un ensemble de créés de façon dynamique `ShortcutItems`, si ce n’est pas nous allons créer deux nouveaux `UIMutableApplicationShortcutItem` objets pour définir les nouveaux éléments et les ajouter à la `ShortcutItems` tableau.

Le code que nous avons ajouté déjà dans le [une Action rapide de la gestion des](#Handling-a-Quick-Action) ci-dessus gère ces Actions rapides dynamique comme ceux statique.

Il convient de noter que vous pouvez créer un mélange d’éléments d’Action rapide statiques et dynamiques (comme nous le faisons ici), vous n’êtes pas limité à un ou l’autre.

Pour plus d’informations, consultez notre [iOS 9 ViewControllerPreview exemple](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) et consultez d’Apple [ApplicationShortcuts : à l’aide de UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) exemple d’application, [ Référence de classe de UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [UIMutableApplicationShortcutItem classe référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) et [UIApplicationShortcutIcon classe référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Test 3D Touch dans le simulateur

Lorsqu’à l’aide de la dernière version de Xcode et le simulateur iOS sur un Mac compatible avec une Force Touch activez du pavé tactile, vous pouvez tester la fonctionnalité tactile 3D dans le simulateur.

Pour activer cette fonctionnalité, exécutez n’importe quelle application dans le matériel simulé iPhone qui prend en charge 3D Touch (iPhone 6 s et supérieur). Ensuite, sélectionnez le **matériel** menu dans iOS Simulator et activer la **Force de pavé tactile d’utilisation pour les fonctions tactiles 3D** élément de menu :

[ ![](3d-touch-images/simulator01.png "Sélectionnez le menu de matériel dans le simulateur iOS et activer la Force du pavé tactile utilisation pour l’élément de menu 3D touch")](3d-touch-images/simulator01.png)

Avec cette fonctionnalité active, vous pouvez appuyer sur plus difficile du pavé tactile du Mac pour activer la 3D Touch comme vous le feriez sur du matériel de l’iPhone réel.

## <a name="summary"></a>Récapitulatif

Cet article a introduit les nouvelles 3D Touch API mis à disposition dans iOS 9 pour l’iPhone 6 s et iPhone 6 s Plus. Il couvert par pression ajout à une application ; à l’aide de Pop et Aperçu pour afficher rapidement des informations dans l’application à partir du contexte actuel sans navigation ; et les fonctionnalités à l’aide des Actions rapides pour fournir des raccourcis vers votre application de plus couramment utilisées.



## <a name="related-links"></a>Liens associés

- [iOS 9 ViewControllerPreview exemple](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts exemple](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Préparer vos applications iPhone 3D Touch](https://developer.apple.com/ios/3d-touch/)
