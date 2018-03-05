---
title: "Résolution des problèmes"
description: "Cet article fournit des conseils de dépannage plusieurs pour travailler avec macOS Sierra dans les applications Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 739cffb2b5418e4fc52bd91f97f4123b01abf0c7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Résolution des problèmes

_Cet article fournit des conseils de dépannage plusieurs pour travailler avec macOS Sierra dans les applications Xamarin.Mac._

il les sections suivantes répertorie certains problèmes connus qui peuvent se produire lors de l’utilisation de macOS Sierra avec Xamarin.mac et la solution à ces problèmes :

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Compatibilité binaire](#Binary-Compatibility)
- [Protocole HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [Notifications](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>Magasin d’applications

Problèmes connus :

- Lorsque vous testez les achats dans l’application dans l’environnement de bac à sable, la boîte de dialogue d’authentification peut-être apparaître deux fois.
- Lorsque vous testez les achats dans l’application avec le contenu hébergé dans l’environnement de bac à sable, la boîte de dialogue de mot de passe s’affiche chaque fois que l’application est mise au premier plan jusqu'à ce que le téléchargement de contenu se termine.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Si un code sécurité ou de la date d’expiration incorrecte (PA) est entré lors de l’ajout d’une nouvelle carte de paiement pour payer d’Apple, la processus de configuration de carte va être interrompue.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Compatibilité binaire

Problèmes connus :

- Appel de `NSObject.ValueForKey` sera un `null` clé entraîne une exception.
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

## <a name="coreimage"></a>CoreImage

Le `CIImageProcessor` API prend désormais en charge un nombre arbitraire image d’entrée. `CIImageProcessor` API qui a été inclus dans macOS bêta de Sierra 1 sera supprimée.

<a name="Notifications" />

## <a name="notifications"></a>Notifications

Lorsque vous travaillez avec des Extensions de contenu de Notification, affichage des contrôleurs ne sont pas libérés correctement et peut entraîner un blocage lors de l’Extension mémoire limites sont atteintes.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Après une opération de transfert, le `UserInfo` propriété d’un `NSUserActivity` objet peut être vide. Appeler explicitement `BecomeCurrent` `NSUserActivity` objet comme solution de contournement actuelle.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation requiert un sécurisé (`https://`) URL pour travailler sur iOS 10 et macOS Sierra pour empêcher toute utilisation malveillante de données d’emplacement.







## <a name="related-links"></a>Liens associés

- [Exemples Mac](https://developer.xamarin.com/samples/mac/)
- [Nouveautés du système d’exploitation X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
