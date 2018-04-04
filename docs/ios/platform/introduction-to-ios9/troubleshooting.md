---
title: Résolution des problèmes
description: Cet article fournit des conseils de dépannage plusieurs pour travailler avec iOS 9 dans les applications Xamarin.iOS.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 1b335fc6b19d87a46059511baf866433691b1b4d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Résolution des problèmes

_Cet article fournit des conseils de dépannage plusieurs pour travailler avec iOS 9 dans les applications Xamarin.iOS._

## <a name="there-was-a-problem-parsing-the-xml"></a>Un problème de l’analyse du XML

Xamarin iOS concepteur ne prend pas en charge les fonctionnalités de Xcode 7. Storyboards ne seront pas chargé dans le concepteur avec _« Problème lors de l’analyse du XML »_ lorsque vous tentez d’utiliser la nouvelle iOS 9 (Xcode 7) tels que StackView éléments du concepteur.

prise en charge du concepteur pour les fonctionnalités de Xcode 7 iOS est ciblé pour la prochaine version de la fonctionnalité Cycle 6. La version préliminaire du Cycle de 6 n’est actuellement disponible dans le canal Alpha et prend en charge les nouvelles fonctionnalités de Xcode 7 limitée.

Solution de contournement partielle pour Visual Studio pour Mac : avec le bouton droit de la table de montage séquentiel et choisissez **ouvrir avec** > **Xcode Interface Builder**.

## <a name="where-are-the-ios-8-simulators"></a>Où se trouvent les iOS 8 simulateurs ?

Si vous avez installé Xcode 7 (ou version ultérieure) il remplacera automatiquement toutes les simulateurs iOS 8 avec iOS 9 simulateurs par défaut. Si vous devez toujours tester sur iOS 8, démarrez Xcode, puis téléchargez et installez les simulateurs iOS 8.

Dans Xcode, sélectionnez le **Xcode** menu puis **préférences...**   >  **Télécharge**:

[![](troubleshooting-images/ios8.png "Télécharge des simulateurs iOS 8")](troubleshooting-images/ios8.png#lightbox)

Cliquez sur le **Check et installer maintenant** bouton pour réinstaller les simulateurs iOS 8.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Contrainte de mise en page avec des erreurs de l’attribut de gauche/droite

Dans iOS 8 (et versions antérieures), les éléments d’interface utilisateur dans les plans conceptuels peuvent utiliser une combinaison des deux **droite** & **gauche** attributs (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) et  **Début** & **fin** attributs (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) dans la même disposition.

Si la même table de montage séquentiel de s’exécuter dans iOS 9, il entraîne une exception sous la forme suivante :

> Arrêt de l’application en raison d’une exception non interceptée 'NSInvalidArgumentException', motif : ' *** + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant :] : Impossible d’établir une contrainte entre le début/fin attribut et un attribut de droite à gauche. Utilisation de début ou de fin pour les deux ou aucune des deux. »

iOS 9 applique des dispositions pour utiliser **droite** & **gauche** _ou_ **début**  &   **Fin** attributs mais *pas* à la fois. Pour résoudre ce problème, modifiez toutes les contraintes de disposition pour utiliser le même attribut à l’intérieur de votre fichier d’animation.

Pour plus d’informations, consultez la [erreur de contrainte d’iOS 9](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) discussion de débordement de pile.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>90535-erreur éléments : Clé de CFBundleExecutable inattendue

Après avoir basculé vers iOS 9, à partir d’une application utilise des 3e partie composants (spécifiquement notre composant existant Google Maps) compilé et exécuté sur iOS 8 (ou une version antérieure), lors de la tentative de validation de la nouvelle build à iTunes Connect, vous pouvez obtenir une erreur dans le formulaire :

> ÉLÉMENTS de l’erreur-90535 : Clé de CFBundleExecutable inattendue. Le groupe à 'Payload/app-name.app/component.bundle' ne contient pas un exécutable d’application...

Ce problème peut généralement être résolu en recherchant le groupe nommé dans le projet puis - tout comme le message d’erreur suggère - modifier la `Info.plist` qui se trouve dans le groupe en supprimant le `CFBundleExecutable` clé. Le `CFBundlePackageType` clé doit être définie sur `BNDL` également.

Après avoir apporté ces modifications, effectuez un nettoyage et régénérez le projet entier. Vous devez être en mesure de présenter à iTunes Connect sans problème après avoir apporté ces modifications.

Pour plus d’informations, consultez cette [Stack Overflow](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) discussion.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>Échec de CFNetwork SSLHandshake (-9824) erreur

Lorsque vous tentez de vous connecter à internet, directement ou à partir d’un affichage web dans iOS 9, vous pouvez obtenir une erreur dans le formulaire :

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Ou sous la forme :

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

Sécurité de Transport d’application (ATS) iOS9, garantit des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et de votre application. En outre, ATS nécessite la communication à l’aide du `HTTPS` protocole et haut niveau communication API à l’aide du protocole TLS version 1.2 avec le secret de transfert.

Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de `NSURLConnection`, `CFURL` ou `NSURLSession` sera soumis à des exigences de sécurité de couches d’application. Si vos connexions ne répondent pas à ces requise, ne pourront pas avec une exception.

Consultez le [hors-Opting ATS](~/ios/app-fundamentals/ats.md) section de notre [sécurité de Transport d’application](~/ios/app-fundamentals/ats.md) guide pour plus d’informations sur la façon de résoudre ce problème.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Mes applications existantes ne s’exécutent sur iOS 9

Consultez notre [informations sur la compatibilité iOS 9](~/ios/platform/introduction-to-ios9/ios9.md) pour obtenir des instructions sur la régénérer et redéployer vos applications existantes pour s’exécuter sur iOS 9.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView a la valeur Null dans les constructeurs

**Raison :** dans iOS 9 le `initWithFrame:` constructeur est désormais obligatoire, en raison de changements de comportement dans iOS 9 en tant que le [UICollectionView les États de documentation](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Si vous avez inscrit une classe pour l’identificateur spécifié et une nouvelle cellule doit être créée, la cellule est maintenant initialisée en appelant son `initWithFrame:` (méthode).

**Correctif :** ajouter la `initWithFrame:` constructeur comme suit :

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Liées exemples : [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView ne parvient pas initialiser avec codeur lors du chargement d’une vue à partir d’un Xib/Nib

**Raison :** le `initWithCoder:` constructeur est appelé lors du chargement d’une vue à partir d’un fichier de l’Interface générateur Xib celui. Si ce constructeur n’est pas exporté le code non managé ne peut pas appeler notre version gérée de celui-ci. Précédemment (par exemple). dans iOS 8) le `IntPtr` constructeur a été appelé pour initialiser la vue.

**Correctif :** créer et exporter le `initWithCoder:` constructeur comme suit :

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

**Raison :** il s’agit d’un bogue dans natif l’éditeur de liens Apple, ce qui se produit lorsqu’ils effectuent une infrastructure privée public (JavaScriptCore a été rendu public dans iOS 7, avant qu’il s’agissait d’une infrastructure privée), et la cible de déploiement de l’application est une version iOS lors de la Framework a été privé. Dans ce cas, l’éditeur de liens d’Apple allez lier avec la version privée du framework au lieu de la version publique.

**Correctif :** cela sera résolu pour iOS 9, mais il existe une solution de contournement facilement appliquer vous-même en attendant : simplement cibler une version iOS ultérieure dans votre projet (vous pouvez essayer d’iOS 7 dans ce cas). Autres infrastructures peuvent présenter des problèmes similaires, par exemple le framework WebKit a été rendu public dans iOS 8 (et pour cibler iOS 7 génère cette erreur ; vous devez cibler iOS 8 Utilisation WebKit dans votre application).

## <a name="untrusted-enterprise-developer"></a>Développeur d’entreprise non approuvées

Lorsque vous tentez d’exécuter la version d’iOS 9 de votre application Xamarin.iOS sur du matériel d’iOS réel, vous pouvez obtenir un message indiquant que votre compte de développeur n’a pas été approuvé sur l’appareil. Par exemple :

[![](troubleshooting-images/untrusted01.png "Alerte de développeur en entreprise non approuvée")](troubleshooting-images/untrusted01.png#lightbox)

Pour résoudre ce problème, procédez comme suit :

1. Démarrer Xcode (la dernière version bêta) sur le développement Mac.
2. Sélectionnez **périphériques** à partir de la **fenêtre** menu pour ouvrir la fenêtre de périphériques : 

    [![](troubleshooting-images/untrusted02.png "La fenêtre de périphériques")](troubleshooting-images/untrusted02.png#lightbox)
3. Sous le **périphériques** côté du Panneau de configuration, sélectionnez votre appareil, avec le bouton droit, puis sélectionnez **afficher les profils de configuration...** : 

    [![](troubleshooting-images/untrusted03.png "Profils d’approvisionnement SShow")](troubleshooting-images/untrusted03.png#lightbox)
4. Sélectionnez chaque profil de configuration actuellement sur le périphérique et cliquez sur le **-** bouton Supprimer : 

    [![](troubleshooting-images/untrusted04.png "Suppression d’un profil de configuration")](troubleshooting-images/untrusted04.png#lightbox)
5. À partir de la **Xcode** menu, sélectionnez **préférences...**  et **comptes**: 

    [![](troubleshooting-images/untrusted05.png "Préférences du compte Xcode")](troubleshooting-images/untrusted05.png#lightbox)
6. Cliquez sur le **afficher les détails...**  , puis cliquez sur le **télécharger tous les** bouton : 

    [![](troubleshooting-images/untrusted06.png "Télécharger tous les profils")](troubleshooting-images/untrusted06.png#lightbox)
7. Lorsque la liste a terminé la mise à jour, cliquez sur le **fait** bouton et fermer la fenêtre Préférences.
8. Supprimer la version existante de l’application Xamarin.iOS que vous essayiez de test à partir de l’appareil iOS.
9. Revenez à Visual Studio pour Mac, un nettoyage de build et essayez d’exécuter à nouveau l’application sur l’appareil.

Vous devrez peut-être arrêter et redémarrer Visual Studio pour Mac avant que les nouveaux profils de configuration chargés par Xcode sont visibles. Vous devrez peut-être également ajuster la **signature d’offre groupée iOS** options pour votre application Xamarin.iOS sélectionner les nouveaux profils de configuration.

## <a name="launch-screen-issues"></a>Problèmes de l’écran de lancement

iOS 9 applique désormais les spécifications de lancer l’écran afin que la même image de démarrage n’est plus peut être réutilisée pour prendre en charge les orientations de l’interface différente. Consultez d’Apple [UILanchImage référence](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) pour plus d’informations.

Si vous le souhaitez, vous pouvez utiliser un fichier d’animation pour présenter l’écran de lancement de votre application au lieu d’utiliser un ensemble de **.png** fichiers image. Il s’agit maintenant Apple par défaut permettant de présenter des écrans de lancement. Veuillez consulter notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) guide pour plus d’informations.

Enfin, votre application doit utiliser un fichier de la table de montage séquentiel pour son écran de lancement et prend en charge toutes les orientations de l’interface quatre (Portrait, envers Portrait, paysage gauche et droite de paysage) à prendre en considération pour l’exécution dans le panneau diapositive sur ou en mode d’affichage fractionné. Pour plus d’informations sur les nouvelles possibilités multitâches d’iOS 9, consultez notre [multitâche pour iPad](~/ios/platform/multitasking.md) guide.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException (Exception)

Lors de la compilation et exécution d’une application Xamarin.iOS existante pour iOS 9, vous pouvez obtenir une erreur dans le formulaire :

> Objective-C une exception.  Nom : NSInternalInconsistencyException raison : fenêtres d’Application sont censés avoir un contrôleur de vue racine à la fin de lancement de l’application

Il s’agit d’erreur est déclenché, car les fenêtres d’application sont censés avoir un contrôleur de vue racine à la fin de lancement de l’application et votre application existante ne.

Il existe au moins deux solutions de contournement possibles pour résoudre ce problème :

1. Application de mise à jour pour utiliser le fichier de plan conceptuel au lieu de `xib` fichiers pour définir son Interface utilisateur. Celle-ci nécessite beaucoup de temps en fonction de la taille de votre application et de savoir comment utiliser l’iOS concepteur (ou Interface Builder de Xcode) pour les animations de disposition. Pour plus d’informations, consultez notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) documentation.
2. Le programme d’installation `RootViewController` propriété de la fenêtre d’application dans `FinishedLaunching` méthode dans `AppDelegate` classe pour pointer vers un contrôleur d’affichage dans l’interface utilisateur de votre application.

## <a name="when-to-initialize-views-and-view-controllers"></a>Quand initialiser les vues et contrôleurs de la vue

Avec Xamarin.iOS, il est possible d’effectuer l’initialisation de vue ou de vue contrôleur à l’intérieur des constructeurs qui sont appelées lorsque quelque chose est exposée dans le code managé, mais interrompt le design d’iOS.

En général vous devez initialiser pas tout ce qui peut rappeler code Objective-C à partir du constructeur, car vous ne pouvez pas être sûr quand elle sera appelée. Cela signifie également qu’il existe un meilleures ressources (autres .ctor) ou des appels à remplacer (comme Objective-C a pas d’événements) où cette initialisation doit être effectuée.



## <a name="related-links"></a>Liens associés

- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Mise à jour de vos applications Xamarin.iOS à iOS9 (vidéo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
