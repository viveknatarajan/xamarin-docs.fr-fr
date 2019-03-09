---
title: Xamarin.iOS 9 – résolution des problèmes
description: Cet article fournit des conseils de dépannage différents pour travailler avec iOS 9 dans Xamarin.iOS. Conseils couvrent l’analyse XML simulateurs, contraintes de disposition, les problèmes de réseau et beaucoup d’autres rubriques.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: f8fae79af654339b54a8df0d2ea32eef38f34adb
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668450"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin.iOS 9 – résolution des problèmes

_Cet article fournit plusieurs conseils de dépannage pour travailler avec iOS 9 dans les applications Xamarin.iOS._

## <a name="there-was-a-problem-parsing-the-xml"></a>Un problème est survenu l’analyse du XML

Le Concepteur de Xamarin iOS ne prend pas en charge les fonctionnalités de Xcode 7. Storyboards ne seront pas chargé dans le concepteur avec _« Un problème est survenu l’analyse du XML »_ lorsque vous tentez d’utiliser la nouvelle iOS 9 (Xcode 7) des éléments concepteurs, tels que StackView.

prise en charge du concepteur pour les fonctionnalités de Xcode 7 iOS est destiné à la prochaine version de la fonctionnalité Cycle 6. La version préliminaire de Cycle de 6 est actuellement disponible dans le canal Alpha et prend en charge pour les nouvelles fonctionnalités de Xcode 7 limitée.

Solution de contournement partielle pour Visual Studio pour Mac : Avec le bouton droit de la table de montage séquentiel et choisissez **ouvrir avec** > **Xcode Interface Builder**.

## <a name="where-are-the-ios-8-simulators"></a>Où se trouvent l’iOS 8 simulateurs ?

Si vous avez installé Xcode 7 (ou version ultérieure) elle remplacera automatiquement toutes les simulateurs iOS 8 avec des simulateurs iOS 9 par défaut. Si vous devez toujours tester sur iOS 8, vous pouvez démarrer Xcode, puis téléchargez et installez les simulateurs iOS 8.

Dans Xcode, sélectionnez le **Xcode** menu puis **préférences...**   >  **Télécharge**:

[![](troubleshooting-images/ios8.png "Téléchargements de simulateurs iOS 8")](troubleshooting-images/ios8.png#lightbox)

Cliquez sur le **Check et installer maintenant** bouton pour réinstaller les simulateurs iOS 8.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Contrainte de mise en page avec des erreurs de l’attribut de gauche/droite

Dans iOS 8 (et versions antérieures), les éléments d’interface utilisateur dans les tables de montage séquentiel peuvent utiliser une combinaison des deux **droite** & **gauche** attributs (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) et  **Début** & **fin** attributs (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) dans la même disposition.

Si la même table de montage séquentiel dans exécuté dans iOS 9, il entraîne une exception sous la forme suivante :

> Arrêt de l’application en raison d’une exception non interceptée 'NSInvalidArgumentException', motif : « *** + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant :] : Une contrainte ne peut pas être établie entre un attribut de début ou de fin et un attribut de droite/gauche. Utilisez les de début/fin pour les deux ou aucune des deux. »

iOS 9 applique des dispositions pour utiliser **droite** & **gauche** _ou_ **début**  &   **Fin** attributs mais *pas* à la fois. Pour résoudre ce problème, modifiez toutes les contraintes de disposition pour utiliser le même attribut défini à l’intérieur de votre fichier de Storyboard.

Pour plus d’informations, consultez le [erreur de contrainte d’iOS 9](https://stackoverflow.com/questions/32692841/ios-9-constraint-error) discussion Stack Overflow.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>ERREUR ITMS-90535 : Clé CFBundleExecutable inattendue

Après avoir basculé vers iOS 9, à partir d’une application utilise des 3e partie composants (spécifiquement notre composant existant Google Maps) compilé et exécuté sur iOS 8 (ou version antérieure), lorsque vous tentez d’envoyer la nouvelle build dans iTunes Connect, vous pouvez obtenir une erreur dans le formulaire :

> ERREUR ITMS-90535 : Clé CFBundleExecutable inattendue. Le groupe à 'Payload/app-name.app/component.bundle' ne contient pas un exécutable de l’offre groupée...

Ce problème peut généralement être résolu en recherchant le groupe nommé dans le projet puis - tout comme le message d’erreur suggère - modifié le `Info.plist` qui se trouve dans le groupe en supprimant le `CFBundleExecutable` clé. Le `CFBundlePackageType` clé doit être définie sur `BNDL` également.

Après avoir apporté ces modifications, effectuez un nettoyage et régénérez le projet entier. Soyez en mesure de soumettre à iTunes Connect sans problème après avoir apporté ces modifications.

Pour plus d’informations, consultez cet [Stack Overflow](https://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) discussion.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>Échec de CFNetwork SSLHandshake (-9824) erreur

Lorsque vous tentez de vous connecter à internet, directement ou à partir d’une vue web dans iOS 9, vous pouvez obtenir une erreur dans le formulaire :

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Ou sous la forme :

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

Dans iOS9, App Transport Security (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et votre application. En outre, ATS nécessite la communication à l’aide du `HTTPS` protocole et haut niveau communication API soient chiffrés à l’aide du protocole TLS version 1.2 avec secret de transfert.

Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de `NSURLConnection`, `CFURL` ou `NSURLSession` sera soumis à des exigences de sécurité ATS. Si vos connexions ne répondent pas à ces besoins, ils échoueront avec une exception.

Consultez le [hors-Opting ATS](~/ios/app-fundamentals/ats.md) section de notre [App Transport Security](~/ios/app-fundamentals/ats.md) guide pour plus d’informations sur la façon de résoudre ce problème.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Mes applications existantes ne s’exécutent sur iOS 9

Consultez notre [les informations de compatibilité iOS 9](~/ios/platform/introduction-to-ios9/ios9.md) pour obtenir des instructions sur la construction de nouveau et de redéployer vos applications existantes pour s’exécuter sur iOS 9.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView a la valeur Null dans les constructeurs

**Raison :** Dans iOS 9 le `initWithFrame:` constructeur est désormais nécessaire, en raison de changements de comportement dans iOS 9, comme le [UICollectionView les États de documentation](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Si vous avez inscrit une classe pour l’identificateur spécifié et une nouvelle cellule doit être créée, la cellule est maintenant initialisée en appelant son `initWithFrame:` (méthode).

**Correctif :** Ajouter le `initWithFrame:` constructeur comme suit :

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Exemples associés : [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView ne parvient pas à Init avec le codeur lors du chargement d’une vue à partir d’un Xib/Nib

**Raison :** Le `initWithCoder:` constructeur est celle appelée lors du chargement d’une vue à partir d’un fichier Xib de générateur d’Interface. Si ce constructeur n’est pas exporté code non managé ne peut pas appeler notre version managée de celui-ci. Précédemment (par exemple). dans iOS 8) le `IntPtr` constructeur a été appelé pour initialiser la vue.

**Correctif :** Créer et exporter le `initWithCoder:` constructeur comme suit :

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Exemple : [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld Message : Aucune Image de Cache par nom...

Vous pouvez rencontrer un incident avec les informations suivantes dans le journal :

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Raison :** Il s’agit d’un bogue dans natif l’éditeur de liens Apple, ce qui se produit quand ils réalisent une infrastructure privée public (JavaScriptCore a été rendu public dans iOS 7, avant qu’il s’agissait d’une infrastructure privée), et la cible de déploiement de l’application est une version iOS lors de l’infrastructure a été privée. Dans ce cas, l’éditeur de liens d’Apple sera liée avec la version privée du framework au lieu de la version publique.

**Correctif :** Il sera corrigé pour iOS 9, mais il existe une solution de contournement simple que vous pouvez les appliquer vous-même en attendant : simplement cibler une version iOS plus loin dans votre projet (vous pouvez essayer iOS 7 dans ce cas). Autres infrastructures peuvent présenter des problèmes similaires, par exemple le framework WebKit a été rendu public dans iOS 8 (et donc cibler iOS 7 génère cette erreur ; vous devez cibler iOS 8 pour utiliser WebKit dans votre application).

## <a name="untrusted-enterprise-developer"></a>Développeur entreprise non approuvé

Lorsque vous tentez d’exécuter la version d’iOS 9 de votre application Xamarin.iOS sur un matériel iOS réels, vous pouvez obtenir un message indiquant que votre compte de développeur n’a pas été approuvé sur l’appareil. Exemple :

[![](troubleshooting-images/untrusted01.png "Alerte de développeur entreprise non approuvé")](troubleshooting-images/untrusted01.png#lightbox)

Pour résoudre ce problème, procédez comme suit :

1. Démarrez Xcode (la dernière version bêta) sur le développement Mac.
2. Sélectionnez **appareils** à partir de la **fenêtre** menu pour ouvrir la fenêtre appareils : 

    [![](troubleshooting-images/untrusted02.png "La fenêtre d’appareils")](troubleshooting-images/untrusted02.png#lightbox)
3. Sous le **appareils** côté du panneau, sélectionnez votre appareil, avec le bouton droit, puis sélectionnez **afficher les profils de provisionnement...** : 

    [![](troubleshooting-images/untrusted03.png "Profils de provisionnement SShow")](troubleshooting-images/untrusted03.png#lightbox)
4. Sélectionnez chaque profil d’approvisionnement actuellement sur l’appareil et cliquez sur le **-** bouton pour le supprimer : 

    [![](troubleshooting-images/untrusted04.png "Suppression d’un profil de provisionnement")](troubleshooting-images/untrusted04.png#lightbox)
5. À partir de la **Xcode** menu, sélectionnez **préférences...**  et **comptes**: 

    [![](troubleshooting-images/untrusted05.png "Préférences du compte Xcode")](troubleshooting-images/untrusted05.png#lightbox)
6. Cliquez sur le **afficher les détails...**  , puis cliquez sur le **télécharger tous les** bouton : 

    [![](troubleshooting-images/untrusted06.png "Télécharger tous les profils")](troubleshooting-images/untrusted06.png#lightbox)
7. Lorsque la liste a terminé la mise à jour, cliquez sur le **fait** bouton et fermer la fenêtre Préférences.
8. Supprimer la version existante de l’application Xamarin.iOS que vous tentiez de test à partir de l’appareil iOS.
9. Retournez dans Visual Studio pour Mac, effectuez une génération propre et essayez de réexécuter l’application sur l’appareil.

Vous devrez peut-être arrêter et redémarrer Visual Studio pour Mac avant que les nouveaux profils d’approvisionnement chargés par Xcode sont visibles. Vous devrez peut-être également ajuster le **signature du Bundle iOS** options pour votre application Xamarin.iOS sélectionner les nouveaux profils d’approvisionnement.

## <a name="launch-screen-issues"></a>Problèmes de l’écran de lancement

iOS 9 applique désormais les exigences de l’écran de lancement afin que la même image de lancement n’est plus peut être réutilisée pour prendre en charge des orientations d’interface différente. Consultez d’Apple [UILanchImage référence](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) pour plus d’informations.

Si vous le souhaitez, vous pouvez utiliser un fichier de storyboard pour présenter l’écran de lancement de votre application au lieu d’utiliser un ensemble de **.png** fichiers image. Il s’agit maintenant Apple préféré du permettent de présenter des écrans de lancement. Consultez notre [Introduction aux Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md) guide pour plus d’informations.

Enfin, votre application doit utiliser un fichier de storyboard pour son écran de lancement et prendre en charge toutes les orientations de l’interface quatre (Portrait, envers Portrait, paysage gauche et droite du paysage) à prendre en compte pour l’exécution dans le panneau des diapositives sur ou en mode d’affichage fractionné. Pour en savoir plus sur les nouvelles possibilités de multitâche d’iOS 9, veuillez consulter notre [multitâche pour iPad](~/ios/platform/multitasking.md) guide.

## <a name="nsinternalinconsistencyexception-exception"></a>Exception de NSInternalInconsistencyException

Lors de la compilation et exécution d’une application Xamarin.iOS pour iOS 9, vous pouvez obtenir une erreur dans le formulaire :

> Objective-C une exception.  Nom : NSInternalInconsistencyException raison : Fenêtres d’application sont supposées comporter un contrôleur d’affichage racine à la fin de l’application à lancer

Il s’agit d’erreur est déclenché, car l’application Windows sont supposés avoir un contrôleur d’affichage racine à la fin de lancement de l’application et votre application existante ne.

Il existe au moins deux solutions possibles pour résoudre ce problème :

1. Application de mise à jour pour utiliser le fichier de plan conceptuel au lieu de `xib` fichiers pour définir son Interface utilisateur. Celle-ci nécessite beaucoup de temps selon la taille de votre application et savoir comment utiliser l’iOS concepteur (ou Interface Builder de Xcode) vers des animations de disposition. Pour plus d’informations, consultez notre [Introduction aux Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md) documentation.
2. Le programme d’installation `RootViewController` propriété de fenêtre d’application dans `FinishedLaunching` méthode dans `AppDelegate` classe pour pointer vers un contrôleur d’affichage dans l’interface utilisateur de votre application.

## <a name="when-to-initialize-views-and-view-controllers"></a>Quand initialiser les vues et contrôleurs d’affichage

Avec Xamarin.iOS, il est possible d’effectuer l’initialisation de vue ou du contrôleur d’affichage à l’intérieur des constructeurs qui sont appelées lorsque quelque chose est exposée dans le code managé, mais sauts de conception d’iOS.

En général vous devez initialiser pas tout ce qui peut rappeler code Objective-C à partir du constructeur, car vous ne pouvez pas être sûr quand elle sera appelée. Cela signifie également qu’il existe un meilleures ressources (autres .ctor) ou des appels à remplacer (comme Objective-C a pas d’événements) où cette initialisation doit être effectuée.

## <a name="related-links"></a>Liens associés

- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Quelles sont les nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [La mise à jour de vos applications Xamarin.iOS à iOS9 (vidéo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
