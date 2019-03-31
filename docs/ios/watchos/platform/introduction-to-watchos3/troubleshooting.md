---
title: watchOS 3 Résolution des problèmes
description: Ce document fournit plusieurs conseils de dépannage utiles lorsque vous travaillez avec watchOS 3 dans Xamarin. Conseils liés aux activités, Apple Pay, actualisation en arrière-plan, NSURLConnection, confidentialité et bien plus encore.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6d2aaf12bd6c45f6268cf87a77d2ee03a9d7a888
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677727"
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 Résolution des problèmes

_Cet article fournit plusieurs conseils de dépannage pour travailler avec watchOS 3 dans les applications Xamarin Apple Watch._

Cette page répertorie certains problèmes connus qui peuvent se produire lors de l’utilisation de watchOS 3 avec Xamarin et de la solution à ces problèmes.

## <a name="activities"></a>Activités

Pour l’activité de partage pour fonctionner correctement, toutes les Apple Watches jumelées doit être en cours d’exécution watchOS 3.

Problèmes connus :

- Réponse à une Notification de partage d’activité parfois échoue.
- Réponse à une Notification de partage d’activité avec un message risque d’échouer.
- Texte contextuel au-dessus d’un message de Notification de partage d’activité seront incorrecte.

## <a name="apple-pay"></a>Apple Pay

Problèmes connus :

- Si une date d’expiration incorrecte ou un code de PV est entré pour un nouveau soins de paiement dans Apple Pay, lors de l’atteinte **suivant** se bloquera le processus en cours d’exécution.
- Achats dans l’application Apple Pay nécessitant un code confidentiel peuvent se bloquer.

## <a name="auto-mac-unlock"></a>Déverrouillage automatique Mac

À l’aide de watchOS 3 2 (ou supérieur) et macOS Sierra bêta 2 (ou supérieur), si l’authentification à deux facteurs est activée sur le compte d’utilisateur iCloud, ils peuvent utiliser leurs Apple Watch pour auto déverrouiller leur Mac.

## <a name="background-refresh"></a>Actualisation en arrière-plan

Violation de ressources système entraîne un blocage d’application watchOS 3 avec les codes d’exception suivants :

- **0xc51bad01** -l’application consommé beaucoup trop de temps processeur.
- **0xc51bad02** -l’application consommé beaucoup trop de temps mur.
- **0xc51bad03** -l’application ne disposait pas de suffisamment de runtime pour terminer la tâche actuelle.

## <a name="clock"></a>Clock

Complications à partir des applications Apple Watch nouvellement installées peuvent apparaître comme étant vide. Redémarrez l’Apple Watch pour résoudre ce problème.

## <a name="connectivity"></a>Connectivité

Problèmes connus :

- watchOS ne demande pas de l’utilisateur l’autorisation d’accès pour les données utilisateur protégé sur l’Apple Watch. Accorder l’accès sur l’application iPhone avant d’utiliser des données dans l’application watch.
- L’Apple Watch peuvent entrer dans un état où toutes les transmissions WatchConnectivity échouent, redémarrez l’Apple Watch à corriger.

## <a name="notifications"></a>Notifications

Si une pièce jointe de média est trop volumineux, il s’affiche sur l’iPhone de l’utilisateur, mais pas leurs Apple Watch.

## <a name="nsurlconnection"></a>NSURLConnection

N’importe quel `NSURLConnection` connexions à l’aide d’anciens protocoles TLS échouera. Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est désormais désactivé par défaut. En outre, l’API de Transport sécurisé n’est plus prend en charge que les SSLv3 et il est recommandé que l’application s’arrêter à l’aide d’un chiffrement SHA-1 et 3DES dès que possible.

À compter de watchOS 3, sécurité des connexions SSL/TLS est strictement appliquée par Apple. Applications et services concernés doivent mis à jour des serveurs web pour utiliser les dernières versions de protocole TLS.

## <a name="nsurlsession"></a>NSURLSession

À compter de watchOS 3, le `HTTPBodyStream` propriété de la `NSMutableURLRequest` classe doit être définie sur un flux non ouvert depuis `NSURLConnection` et `NSURLSession` applique désormais strictement cette exigence.

## <a name="privacy"></a>Confidentialité

Problèmes connus :

Lorsque vous travaillez avec `https://` URL à la fois `NSURLSession` et `NSURLConnection` n’est plus prise en charge les suites de chiffrement RC4 pendant la négociation TLS. Un des codes d’erreur suivants peut-être être généré :

- **-1200 ou-98** : pour `NSURLErrorSecurityConnectionFailed` et SecureTransport erreurs.
- **-1200 [3 :-9824]** -échouée de la charge http.
- **-1200**  -  `NSURLConnection` terminé avec erreur.

À compter de watchOS 3, sécurité des connexions SSL/TLS est strictement appliquée par Apple. Applications et services concernés doivent mis à jour des serveurs web pour utiliser les dernières versions de protocole TLS. Consultez [NSURLConnection](#nsurlconnection) ci-dessus pour plus d’informations.

## <a name="snapshots"></a>Snapshots

Applications WatchKit qui n’ont pas adopté la nouvelle `HandelBackgroundTask` API ne recevra plus les mises à jour périodiques dans watchOS 3. 

## <a name="watchkit"></a>WatchKit

Arrière-plan SpriteKit et SceneKit sera interrompue lorsqu’une application entre l’arrière-plan dans le Dock watchOS.

## <a name="related-links"></a>Liens associés

- [Exemples watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Quelles sont les nouveautés dans watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
