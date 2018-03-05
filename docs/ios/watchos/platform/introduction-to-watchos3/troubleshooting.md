---
title: "watchOS 3 Résolution des problèmes"
description: "Cet article fournit des conseils de dépannage plusieurs pour travailler avec watchOS 3 dans les applications Xamarin Apple Watch."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 02ec4c3c9827f7cfac48184eb12491faaa0c92c8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 Résolution des problèmes

_Cet article fournit des conseils de dépannage plusieurs pour travailler avec watchOS 3 dans les applications Xamarin Apple Watch._

Cette page répertorie certains problèmes connus qui peuvent se produire lors de l’utilisation de watchOS 3 avec Xamarin et la solution à ces problèmes.

## <a name="activities"></a>Activités

Pour l’activité de partage pour fonctionner correctement, toutes les observations Apple appariés doit exécuter watchOS 3.

Problèmes connus :

- Réponse à une Notification de partage d’activité parfois échoue.
- Réponse à une Notification de partage d’activité avec un message risque d’échouer.
- Texte contextuel au-dessus d’un message de Notification de partage d’activité est incorrecte.


## <a name="apple-pay"></a>Apple Pay

Problèmes connus :

- Si un code d’un calcul ou de date d’expiration incorrect est entré pour un nouveau soins de paiement dans Apple Pay, lors de l’atteinte **suivant** se bloquera le processus en cours d’exécution.
- Achats dans l’application Apple Pay nécessitant un code confidentiel peuvent se bloquer.



## <a name="auto-mac-unlock"></a>Le déverrouillage automatique Mac

Avec watchOS 3 2 (ou version ultérieure) et macOS Sierra bêta 2 (ou supérieure), si l’authentification à deux facteurs est activée sur le compte d’utilisateur iCloud, ils peuvent utiliser leurs Apple Watch automatique déverrouiller leur Mac.



## <a name="background-refresh"></a>Actualisation en arrière-plan

Violer les ressources système entraîne une panne d’application watchOS 3 avec les codes d’exception suivants :

- **0xc51bad01** -l’application consommé trop de temps processeur.
- **0xc51bad02** -l’application consommé trop de durée d’exécution.
- **0xc51bad03** -l’application n’avait pas suffisamment runtime pour terminer la tâche en cours.



## <a name="clock"></a>Clock

Complications à partir d’applications d’Apple Watch nouvellement installées peuvent apparaître comme étant vide. Redémarrez l’Apple Watch pour résoudre ce problème.


## <a name="connectivity"></a>Connectivité

Problèmes connus :

- watchOS invite pas l’utilisateur pour l’autorisation d’accès pour les données utilisateur protégé sur l’Apple Watch. Autoriser l’accès à l’application iPhone avant d’utiliser des données dans l’application de surveillance.
- L’Apple Watch peut entrer dans un état où toutes les transmissions WatchConnectivity échouent, redémarrez l’Apple Watch à corriger.


## <a name="notifications"></a>Notifications

Si une pièce jointe de média est trop volumineux, il s’affiche sur l’iPhone de l’utilisateur, mais pas leurs Apple Watch.


## <a name="nsurlconnection"></a>NSURLConnection

N’importe quel `NSURLConnection` les connexions à l’aide des protocoles TLS plus anciens échouent. Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est maintenant désactivé par défaut. En outre, l’API de Transport sécurisé ne prend plus en SSLv3 et il est recommandé que l’application s’arrêter à l’aide d’un chiffrement SHA-1 et 3DES dès que possible.

À compter de watchOS 3, sécurité des connexions SSL/TLS est strictement appliquée par Apple. Les applications et les services affectés doivent mis à jour les serveurs web à utiliser les dernières versions du protocole TLS.


## <a name="nsurlsession"></a>NSURLSession

À compter de watchOS 3, le `HTTPBodyStream` propriété de la `NSMutableURLRequest` classe doit être définie sur un flux non ouvert depuis `NSURLConnection` et `NSURLSession` maintenant impose cette exigence.


## <a name="privacy"></a>Confidentialité

Problèmes connus :

Lorsque vous travaillez avec `https://` URL à la fois `NSURLSession` et `NSURLConnection` n’est plus prise en charge des suites de chiffrement RC4 pendant la négociation TLS. Un des codes d’erreur suivants peut être généré :

- **-1200 ou-98** - pour `NSURLErrorSecurityConnectionFailed` et SecureTransport erreurs.
- **-1200 [3 :-9824]** -charge http a échoué.
- **-1200**  -  `NSURLConnection` a rencontré une erreur.

À compter de watchOS 3, sécurité des connexions SSL/TLS est strictement appliquée par Apple. Les applications et les services affectés doivent mis à jour les serveurs web à utiliser les dernières versions du protocole TLS. Consultez [NSURLConnection](#NSURLConnection) ci-dessus pour plus d’informations.


## <a name="snapshots"></a>Snapshots

Applications WatchKit qui n’ont pas adopté la nouvelle `HandelBackgroundTask` API ne recevra plus les mises à jour périodiques dans watchOS 3. 


## <a name="watchkit"></a>WatchKit

Scènes SpriteKit et SceneKit va être suspendues lorsqu’une application entre l’arrière-plan de la station d’accueil watchOS.


## <a name="related-links"></a>Liens associés

- [Exemples watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Quelles sont les nouveautés dans watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
