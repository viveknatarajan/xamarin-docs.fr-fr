---
title: Résolution des problèmes de tvOS 10 applications développées avec Xamarin
description: Cet article fournit des conseils de dépannage plusieurs pour travailler avec tvOS 10 dans des applications Xamarin. Il décrit les problèmes liés à l’App Store, la compatibilité binaire, CFNetwork HttpProtocol, CloudKit, Image Core, NSUserActivity et UIKit.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 4332caca2804da52bb565fe382932af691c39dab
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788808"
---
# <a name="troubleshooting-tvos-10-apps-built-with-xamarin"></a>Résolution des problèmes de tvOS 10 applications développées avec Xamarin

Les sections suivantes répertorient certains problèmes connus qui peuvent se produire lors de l’utilisation de tvOS 10 avec Xamarin et la solution à ces problèmes :

- [App Store](#App-Store)
- [Compatibilité binaire](#Binary-Compatibility)
- [Protocole HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Image de base](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>Magasin d’applications

Problèmes connus :

 - Lorsque vous testez les achats dans l’application dans l’environnement de bac à sable, la boîte de dialogue d’authentification peut-être apparaître deux fois.
 - Lorsque vous testez les achats dans l’application avec le contenu hébergé dans l’environnement de bac à sable, la boîte de dialogue de mot de passe s’affiche chaque fois que l’application est mise au premier plan jusqu'à ce que le téléchargement de contenu se termine.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Compatibilité binaire

Problèmes connus :

 - Appel de `NSObject.ValueForKey` sera un `null` clé entraîne une exception.
 - Référencement d’une police par nom lors de l’appel `UIFont.WithName` provoque un blocage.
 - Les deux `NSURLSession` et NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' URL.
 - Applications peuvent se bloquer si elle modifie la géométrie d’un super-vue, que ce soit le `ViewWillLayoutSubviews` ou `LayoutSubviews` méthodes.
 - Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est maintenant désactivé par défaut. En outre, l’API de Transport sécurisé ne prend plus en SSLv3 et il est recommandé que l’application s’arrêter à l’aide d’un chiffrement SHA-1 et 3DES dès que possible.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>Protocole HTTP CFNetwork

Le `HTTPBodyStream` propriété de la `NSMutableURLRequest` classe doit être définie sur un flux non ouvert depuis `NSURLConnection` et `NSURLSession` maintenant impose cette exigence.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Opérations longues retournera un _« Vous n’êtes pas autorisé à enregistrer le fichier »._ Erreur.

<a name="CoreImage" />

## <a name="core-image"></a>Image de base

Le `CIImageProcessor` API prend désormais en charge un nombre arbitraire image d’entrée. `CIImageProcessor` API qui a été inclus dans la version bêta 1 de tvOS 10 sera supprimé.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Après une opération de transfert, le `UserInfo` propriété d’un `NSUserActivity` objet peut être vide. Appeler explicitement `BecomeCurrent` NSUserActivity' objet comme solution de contournement actuelle.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Problèmes connus :

 - Modifie l’apparence de l’arrière-plan du `UINavigationBar`, `UITabBar` ou `UIToolBar` peut entraîner une passe de disposition pour résoudre la nouvelle apparence. Tente de modifier ces aspects à l’intérieur d’un `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` ou `DidUpdateSubviews` événement peut entraîner une boucle infinie de disposition.
 - Dans l’appel de tvOS 10, le `RemoveGestureRecognizer` méthode d’un `UIView` objet annule explicitement n’importe quel module de reconnaissance de mouvement en cours.
 - Contrôleurs présenté de la vue peut affecter maintenant l’apparence de la barre d’état.
 - tvOS 10 requiert le développeur à appeler `base.AwakeFromNib` lors du sous-classement `UIViewController` et en remplaçant le `AwakeFromNib` (méthode).
 - Applications avec personnalisé `UIView` sous-classes qui substituent `LayoutSubviews` et incorrectes de la mise en page avant d’appeler `base.LayoutSubviews` peuvent déclencher une boucle infinie de disposition dans tvOS 10.
 - Les ressources spécifiques à la direction ou PLIABLE images ne sont aucun retournement lorsque affectés à `UIButton` objets.

## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Nouveautés de tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
