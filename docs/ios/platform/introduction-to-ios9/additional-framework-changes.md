---
title: Modifications des infrastructures supplémentaires iOS 9
description: Ce document décrit les modifications de framework supplémentaires introduites dans iOS 9. Il traite des AVFoundation, AVKit et CloudKit.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 15c9364cf3bdcb8c797882cc9ac76219959de439
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787706"
---
# <a name="additional-ios-9-frameworks-changes"></a>Modifications des infrastructures supplémentaires iOS 9

_Cet article traite des modifications supplémentaires, secondaires ou des améliorations apportées aux infrastructures existantes pour iOS 9._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

En plus des principales modifications apportées à iOS, Apple a apporté des modifications et améliorations apportées à plusieurs infrastructures existantes dans iOS 9.

## <a name="avfoundation-framework-additions"></a>Ajouts de AVFoundation Framework

Dans le framework AVFoundation, le [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) classe maintenant vous permet de spécifier une voix par identificateur en plus de la langue.

Par exemple, le code suivant obtient une liste de tous les voix disponibles :

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Vous pouvez ensuite utiliser une des voix dans la liste en la définissant comme le `Voice` propriété d’une instance de la [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) classe.

Le [AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) classe prend désormais en charge un mélange de support de diffusion en continu et de fichier internet dans la file d’attente. Les versions précédentes peut uniquement les supports de file d’attente du même type.

Pour plus d’informations, consultez le site d’Apple [AVSpeechSynthesisVoice référence](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>Ajouts de AVKit Framework

Pour utiliser la nouvelle fonctionnalité d’image (PIP), le framework AVKit inclut la nouvelle `AVPictureInPictureController` et [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) classes :

- **AVPictureInPictureController** -cette classe permet à une application iOS 9 répondre à l’utilisateur de lancer la lecture d’une vidéo dans une fenêtre PIP redimensionnable flottante sur un iPad.
- **AVPlayerViewController** -gère un `AVPlayer` contrôleur utilisé pour présenter une vidéo dans une fenêtre PIP redimensionnable flottante sur un iPad.

Pour plus d’informations, consultez notre [multitâche pour iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) documentation et Apple [AVPictureInPictureController référence](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) et [AVPlayerViewController référence](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Présentation des Services Web de CloudKit

Le framework CloudKit simplifie le développement d’applications qui iCloud de l’accès. Cela inclut la récupération des données d’application et droits de l’élément multimédia ainsi que la possibilité de stocker en toute sécurité les informations de l’application. Ce kit fournit aux utilisateurs une couche d’anonymat en autorisant l’accès aux applications avec leur ID iCloud sans partager des informations personnelles.

La nouvelle _CloudKit Web Services_ framework fournit une bibliothèque JavaScript (JS CloudKit) qui peut être incorporée dans votre site Web pour fournir l’accès aux données de CloudKit basé et le contenu en tant que votre application Xamarin.iOS même.

> [!IMPORTANT]
> Avant de pouvoir accéder, présenter ou mettre à jour le contenu à partir d’une base de données CloudKit à l’aide de CloudKit JS, vous devez avoir préalablement défini schéma de cette base de données.




Pour plus d’informations, consultez les documents suivants :

- [Introduction à CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) -notre introduction à l’utilisation de CloudKit dans une application Xamarin.iOS.
- [Démarrage rapide CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -CloudKit présentation d’Apple.
- [Référence de JS CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -documentation de CloudKit JS d’Apple.
- [Référence de Services Web CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -référence d’Apple qui décrit l’interface HTTP CloudKit.
- [Catalogue de CloudKit : Présentation de CloudKit (/ Cocoa et JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -exemple d’application d’Apple à l’aide de CloudKit et CloudKit JS.

> [!IMPORTANT]
> Apple [fournit des outils](https://developer.apple.com/support/allowing-users-to-manage-data/) pour aider les développeurs à gérer correctement le Règlement général sur la protection des données (RGPD) de l’Union européenne.

## <a name="foundation-framework-additions"></a>Ajouts de Framework Foundation

Apple comprenait les modifications suivantes à l’infrastructure Foundation dans iOS 9 :

### <a name="changes-to-nsbundle"></a>Modifications apportées à NSBundle

Les modifications suivantes ont été apportées à la [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) classe IOS 9 :

* `GetPreservationPriorityForTag (NSString tag)` -Obtient la priorité actuelle de la conservation des ressources avec la balise spécifiée. Les valeurs valides sont dans la plage `0.0` à `1.0`, ressources avec la priorité la plus faible seront tout d’abord purgés.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -Définit la priorité actuelle de la conservation des ressources avec les balises donnés. Les valeurs valides sont dans la plage `0.0` à `1.0`, ressources avec la priorité la plus faible seront tout d’abord purgés.

Pour plus d’informations, consultez le site d’Apple [NSBundle référence](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>Modifications apportées à NSProcessInfo

Chaque processus en cours d’exécution sur un appareil iOS a un seul _Agent des informations de processus_ (PIA). Utilisez le [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) classe afin de fournir des informations sur l’assembly PIA et contrôle courant et gestion thermique pour un processus donné.

Par exemple, pour contrôler l’arrêt automatique d’un processus, vous pouvez utiliser le code suivant :

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Pour plus d’informations, consultez le site d’Apple [NSProcessInfo référence](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Réagir à Mode alimentation basse

Utilisez le `LowPowerModeEnabled` propriété de la [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) classe pour déterminer si le Mode alimentation basse a été activé sur l’appareil iOS qui l’application est en cours d’exécution. Exemple :

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>Modifications du Framework HealthKit

Apple comprenait les modifications suivantes à la [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) framework dans iOS 9 :

- Prise en charge pour le suivi de suppression d’entrées dans la base de données HealthKit et de suppression en bloc. Consultez d’Apple [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) et [référence de classe HKHealthStore](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) pour plus d’informations.
- Les caractéristiques et les nouvelles catégories de suivi ont été ajoutés à la `HKQuantityTypeIdentifier` classe (tel que `UVExposure`) et en le `HKCategoryTypeIdentifier` classe (tel que `OvulationTestResult`). Consultez d’Apple [HealthKit constantes référence](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) pour plus d’informations.

Veuillez consulter notre [présentation HealthKit](~/ios/platform/healthkit.md) documentation pour plus d’informations sur l’utilisation de HealthKit dans Xamarin.iOS.

## <a name="local-authentication-framework-changes"></a>Modifier le Framework d’authentification local

Apple comprenait les modifications suivantes à la [l’authentification locale](https://developer.xamarin.com/api/namespace/LocalAuthentication/) framework dans iOS 9 :

- À l’aide de la `EvaluateAccessControl` et `EvaluatePolicy` méthodes de la [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) (classe), vous pouvez maintenant tente de réutilisation Touch ID établit des correspondances de déverrouillage réussi précédent.
- La possibilité d’obtenir la liste des doigts actuellement inscrits.
- Prise en charge pour le suivi lorsqu’un doigt est ajouté ou supprimé de l’authentification.
- La possibilité d’utiliser _contexte d’authentification_ dans les appels de trousseau et prise en charge pour l’évaluation de contrôle de trousseau d’accès répertorie.
- La capacité d’annulation d’une invite utilisateur à partir du code.

Veuillez consulter notre [présentation Touch ID](~/ios/platform/touchid.md) documentation pour plus d’informations sur l’utilisation des ID tactile dans Xamarin.iOS.

### <a name="lacontext-changes"></a>Modifications LAContext

Les modifications suivantes ont été apportées à la [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) classe IOS 9 :

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -renvoie la durée maximale que l’authentification d’un ID tactile peut être réutilisée.
- **EvaluatedPolicyDomainState** : Obtient ou définit l’état d’une stratégie évaluée.
- **MaxBiometryFailures** -a été supprimée dans iOS 9.
- **TouchIdAuthenticationAllowableReuseDuration** Obtient ou définit la durée pendant laquelle l’authentification d’un ID tactile peut être réutilisée.
- **EvaluateAccessControl** , de façon asynchrone évalue une stratégie d’authentification.
- **Invalider** -invalide l’authentification d’un ID tactile donné.
- **IsCredentialSet** -retourne `true` si les informations d’identification sont actuellement définies.
- **SetCredentialType** définit le type d’informations d’identification spécifiées.

Consultez d’Apple [LAContext référence](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) pour plus d’informations.

## <a name="mapkit-framework-changes"></a>Modifications du Framework MapKit

Apple comprenait les modifications suivantes à la [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) framework dans iOS 9 :

- MapKit prend désormais en charge pour le lancement de l’application de la carte directement dans le sens de transit et pour l’interrogation de transit durée (estimée) de l’utilisation de notre le [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) et [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) classes.
- Les résultats retournés par MapKit et [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) classe peut également fournir horaire du résultat.
- Vous pouvez maintenant personnaliser entièrement les Annotations de mappage présenté par votre application iOS à l’aide du `DetailCalloutAccessoryView` propriété de la [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) classe.

Veuillez consulter notre [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) et [procédure pas à pas : exploration des annotations et les superpositions dans MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) documentation pour plus d’informations sur l’utilisation des cartes et des Annotations dans Xamarin.iOS et Apple [CLGeocoder référence](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) pour plus d’informations.

## <a name="passkit-framework-additions"></a>Ajouts de PassKit Framework

Apple comprenait les modifications suivantes à la [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) framework dans iOS 9 :

- Apple Pay prend désormais en charge débit de magasin et des cartes de crédit, ainsi que des cartes de découverte. Consultez le **des réseaux de paiement** section d’Apple [référence de classe PKPaymentRequest](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) pour plus d’informations.
- À partir de directement dans une application Xamarin.iOS, vous pouvez maintenant ajouter les réseaux de paiement et les émetteurs de carte pour payer d’Apple. Consultez d’Apple [référence de classe PKAddPaymentPassViewController](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) pour plus d’informations.

Veuillez consulter notre [présentation PassKit](~/ios/platform/passkit.md) documentation pour plus d’informations sur l’utilisation de PassKit dans Xamarin.iOS.

## <a name="safari-services-framework-additions"></a>Safari Services Framework ajouts

Apple comprenait les modifications suivantes à la [Safari Services](https://developer.xamarin.com/api/namespace/SafariServices/) framework dans iOS 9 :

- Vous pouvez désormais utiliser la nouvelle [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) classe pour afficher le contenu web dans une application Xamarin.iOS. Il offre la possibilité de partager des données du site Web et les cookies avec l’application Safari et inclut plusieurs fonctionnalités de Safari (par exemple, le lecteur et le remplissage automatique). [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) fonctionnalités un **fait** bouton qui retournera des utilisateurs à votre application lorsqu’ils ont terminé d’affichage du contenu web.

Étant donné que la [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) classe est conçue pour afficher une seule page de contenu web, vous pouvez l’utiliser pour remplacer les [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) ou [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)contrôles au sein de vos applications Xamarin.iOS existantes.

### <a name="displaying-a-website"></a>Affichage d’un site Web

Le code ci-dessous est un exemple d’appel un [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) à partir d’à l’intérieur d’un autre contrôleur de vue :

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Modifications du Framework UIKit

Apple a inclus les nombreuses améliorations à plusieurs éléments de la [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) framework IOS 9. Les sections suivantes décrit en détail ces modifications.

### <a name="3d-touch-events"></a>Événements tactiles 3D

Nouvelle iOS 9 iPhone 6 s et iPhone 6 s Plus, 3D Touch ajoute mouvements sensibles de pression à vos applications iOS. Par conséquent, si votre application est en cours d’exécution sur iOS 9 (ou version ultérieure) et que l’appareil iOS est capable de prise en charge 3D Touch, modifications apportées à la pression provoqueront la `TouchesMoved` événement soit déclenché. 

En raison de ce changement de comportement, vos applications iOS doivent être préparées pour la `TouchesMoved` événement à invoquer le plus souvent, même si X / Y coordonnées n’ont pas changé. 

Pour plus d’informations, consultez notre [présentation 3D Touch](~/ios/platform/3d-touch.md) guide.

### <a name="document-open-in-place-functionality"></a>Fonctionnalités de Open in situ de document

À l’aide du `FinishedLaunching (application, launchOptions)` ou `WillFinishLaunching (Application, launchOptions)` méthodes de la [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) (classe), vous pouvez maintenant ouvrir un document et le modifier en place (par opposition à une copie de travail).

Pour prendre en charge les nouvelles fonctionnalités d’open in situ, ajoutez le `LSSupportsOpeningDocumentsInPlace` clé à votre application de Xamarin.iOS **Info.plist** fichier avec une valeur de `YES`.

Consultez d’Apple [UIApplicationDelegate référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) pour plus d’informations.

### <a name="enhanced-touch-events"></a>Événements tactiles améliorée

Apple a fourni plusieurs améliorations pour les événements tactiles dans iOS 9. Citons notamment la possibilité d’utiliser Touch de prédiction et à accéder à des touches finales intermédiaires entre les actualisations d’affichage.

Consultez d’Apple [Guide de la gestion des événements pour iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) pour plus d’informations.

### <a name="fetching-tailored-content"></a>L’extraction de contenu

La nouvelle `NSDataAsset` classe permet à une application Xamarin.iOS récupérer le contenu adapté à la mémoire et les fonctionnalités graphiques de l’appareil iOS, il est en cours d’exécution sur.

### <a name="new-layout-anchors"></a>Les nouvelles ancres de disposition

La nouvelle `NSLayoutAnchor` et `NSLayoutDimension` classes de disposition d’ancrage fonctionnent avec les nouvelles propriétés d’ancrage de la [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) classe (tel que `LeadingAnchor` et `WidthAnchor`) pour faciliter la mise en page dans iOS 9.

Veuillez consulter notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) documentation pour plus d’informations sur l’utilisation de la mise en forme automatique et les Classes de taille dans une application Xamarin.iOS et de d’Apple [NSLayoutAnchor référence](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ Référence de NSLayoutDimension](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) et [UIView référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) pour plus d’informations.

### <a name="new-readable-content-margins"></a>Nouvelles marges contenus lisibles

La nouvelle `UILayoutGuide` classe peut être utilisée pour fournir des marges contenus lisibles et définir les régions de dessin pour le contenu à l’intérieur d’une vue. Consultez d’Apple [UILayoutGuide référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) pour plus d’informations.

### <a name="text-input-in-notifications-modifications"></a>Entrée de texte dans les Modifications de Notifications

Le [UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) classe dispose d’un nouveau `Behavior` propriété qui peut être utilisée pour prendre en charge la saisie de texte à partir de notifications.

### <a name="uiapplicationdelegate-changes"></a>Modifications UIApplicationDelegate

Alors que pas officiellement déconseillée par Apple, ils suggèrent en remplaçant tous les appels à la `FinishedLaunching (UIApplication application)` méthode de la [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) classe avec l’option le `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` ou `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` méthodes.

Consultez d’Apple [UIApplicationDelegate référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) pour plus d’informations.

### <a name="uikit-dynamics-changes"></a>Modifications de Dynamics UIKit

Apple comprenait les modifications suivantes à la dynamique UIKit dans iOS 9 :

- Dynamics prend désormais en charge pour les limites de collision non rectangulaires.
- Le nouveau personnalisable `UIFieldBehavior` classe est utilisée pour prendre en charge différents types de champs.
- Types de pièces jointes supplémentaires ont été ajoutés à la `UIAttachmentBehavior` classe.

Consultez d’Apple [UIAttachment référence](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) pour plus d’informations.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView et UIDatePicker modifications

Avant d’iOS 9, le [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) et [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) contrôles ont été non redimensionnable et redimensionnez automatiquement pour remplir la largeur de leur conteneur (généralement la largeur de l’appareil iOS a été de l’application en cours d’exécution).

Dans iOS 9, redimensionnement automatique ne se produit pas et les contrôles sont rendus à une largeur de 320 point sur tous les appareils iOS, quelle que soit la taille de l’écran et l’orientation.

Pour corriger cette situation, utilisez disposition automatique et les Classes de taille pour épingler la largeur du contrôle sur les bords du conteneur parent (vue) et spécifiez la hauteur requise. Veuillez consulter notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) documentation pour plus d’informations sur l’utilisation des Classes de taille et de disposition automatique dans une application Xamarin.iOS.

### <a name="new-uitextinputassistantitem-class"></a>Nouvelle classe UITextInputAssistantItem

Utilisez la nouvelle `UITextInputAssistantItem` classe aux groupes de bouton de barre de mise en page dans un _gestionnaire_. Le gestionnaire est une nouvelle zone qui est disponible dans le clavier logiciel pour fournir des raccourcis de frappes.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Introduction à iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
