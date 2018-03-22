---
title: "Sécurité de Transport de l’application"
description: "Sécurité du Transport de l’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et de votre application."
ms.topic: article
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 357528c559de36329ca4bf12ab2597247a17222d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="app-transport-security"></a>Sécurité de Transport de l’application

_Sécurité du Transport de l’application (ATS) met en œuvre des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et de votre application._

Cet article vous présente les modifications de sécurité qui applique la sécurité de Transport d’application sur une application iOS 9 et [ce que cela signifie pour vos projets Xamarin.iOS](#xamarinsupport), qui couvre la [les options de configuration ATS](#config) et Elle couvre comment [en désengager de ATS](#optout) ATS si nécessaire. Étant donné que les couches d’application est activé par défaut, toutes les connexions internet non sécurisées lève une exception dans les applications iOS 9 (sauf si vous avez explicitement autorisé).


## <a name="about-app-transport-security"></a>Sur la sécurité de Transport de l’application

Comme indiqué ci-dessus, ATS garantit que toutes les communications internet dans iOS 9 et OS X El Capitan conforme pour la connexion sécurisée meilleures pratiques, ce qui évite la divulgation accidentelle d’informations sensibles directement par le biais de votre application ou une bibliothèque qu’il est consommation.

Pour les applications existantes, vous devez implémenter la `HTTPS` de protocole chaque fois que possible. Pour les nouvelles applications Xamarin.iOS, vous devez utiliser `HTTPS` exclusivement lors de la communication avec les ressources internet. En outre, les communications API haut niveau doivent être chiffrée à l’aide de TLS version 1.2 avec le secret de transfert.

Toute connexion établie avec [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) ou [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) utiliseront les couches d’application par défaut dans les applications générées pour iOS 9 et OS X 10.11 (El Capitan).

## <a name="default-ats-behavior"></a>Comportement de couches d’application par défaut

Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) ou [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) sera soumis pour Exigences de sécurité de couches d’application. Si vos connexions ne répondent pas à ces requise, ne pourront pas avec une exception.

### <a name="ats-connection-requirements"></a>Exigences de connexion des couches d’application

ATS appliquera les spécifications suivantes pour toutes les connexions internet :

- Tous les chiffrements de connexion doivent être à l’aide de secret de transfert. Consultez la liste de chiffrements acceptées ci-dessous.
- Le protocole de sécurité TLS (Transport Layer) doit être supérieure ou égale à 1.2.
- Au moins une empreinte de SHA256 avec un 2 048 bits ou supérieur clé RSA, ou un 256 bits ou une clé de cryptage à courbe elliptique (ECC) supérieur doit être utilisée pour tous les certificats.

Là encore, dans la mesure où les couches d’application est activée par défaut dans iOS 9, toute tentative pour établir une connexion qui ne respecte pas ces exigences entraîne une exception est levée. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Chiffrements Compatible ATS

Le type de chiffrement de confidentialité suivantes sont acceptées par ATS internet communications sécurisées :

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Pour plus d’informations sur l’utilisation des classes de communication internet iOS, consultez le site d’Apple [référence de classe NSURLConnection](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL référence](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) ou [référence de classe NSURLSession ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Prise en charge des couches d’application Xamarin.iOS

Étant donné que les couches d’application est activé par défaut dans iOS 9 et OS X El Capitan, si votre application Xamarin.iOS ou n’importe quel bibliothèque ou le service à l’aide de l’il établit la connexion à internet, vous devez entreprendre une action ou vos connexions entraîne une exception est levée.

Pour une application existante, Apple propose la prise en charge la `HTTPS` protocole dès que possible. Si vous soit impossible, car vous vous connectez à un 3e partie service web qui ne prend en charge `HTTPS` ou si la prise en charge `HTTPS` s’avère peu pratique, vous pouvez annuler des couches d’application. Consultez le [hors-Opting ATS](#optout) section ci-dessous pour plus de détails.

Pour une nouvelle application Xamarin.iOS, vous devez utiliser `HTTPS` exclusivement lors de la communication avec les ressources internet. Là encore, il peut arriver (par exemple, à l’aide d’un service web de 3e partie) où cela n’est pas possible et vous devrez les annulations de couches d’application.

En outre, ATS applique la communication d’API principales à l’aide du protocole TLS version 1.2 avec le secret de transfert. Consultez le [les exigences de connexion ATS](#ATS-Connection-Requirements) et [les chiffrements Compatible ATS](#ATS-Compatible-Ciphers) sections ci-dessus pour plus d’informations.

Pendant que vous n’êtes peut-être pas familiarisé avec le protocole TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) il est le successeur de SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) et fournit une collection de protocoles de chiffrement pour appliquer la sécurité sur connexions réseau.

Le niveau TLS est contrôlé par le service web que vous utilisez et est donc en dehors du contrôle de l’application. À la fois le `HttpClient` et `ModernHttpClient` doivent utiliser automatiquement le plus haut niveau de chiffrement TLS pris en charge par le serveur.

Selon le serveur que vous vous entretenez (en particulier s’il s’agit d’un service tiers 3e), vous devrez peut-être désactiver le secret de transfert, ou sélectionnez un niveau inférieur de TLS. Consultez le [configuration des Options ATS](#Configuring-ATS-Options) section ci-dessous pour plus de détails.

> [!IMPORTANT]
> Sécurité de Transport d’application ne s’applique pas pour les applications Xamarin à l’aide de **gérés de HTTPClient implémentations**. Il s’applique aux connexions à l’aide de CFNetwork **HTTPClient implémentations** ou **NSURLSession HTTPClient implémentations** uniquement.

### <a name="setting-the-httpclient-implementation"></a>Définition de l’implémentation du client HTTP

Pour définir l’implémentation de client HTTP utilisée par une application iOS, double-cliquez sur le **projet** dans les **l’Explorateur de solutions** pour ouvrir le **Options du projet**. Accédez à **Build iOS** et sélectionnez le type de client souhaité dans le **HttpClient implémentation** liste déroulante :

![](ats-images/client01.png "Définir les Options de Build iOS")


#### <a name="managed-handler"></a>Gestionnaire managé

Le Gestionnaire de géré est le gestionnaire HttpClient entièrement géré qui a été livré avec les versions précédentes de Xamarin.iOS et qui est le gestionnaire par défaut.

Avantage :

- Il est plus compatible avec Microsoft .NET et une version antérieure de Xamarin.

Inconvénient :

- Il n’est pas entièrement intégré avec iOS (par exemple, il est limité à TLS 1.0).
- Il est généralement plus lente que les API natives.
- Elle nécessite plus de code managé et crée des applications plus grandes.

#### <a name="cfnetwork-handler"></a>Gestionnaire de CFNetwork

Le Gestionnaire de CFNetwork basé est basé sur natif `CFNetwork` framework.

Avantage :

- Utilise l’API native pour améliorer les performances et réduit la taille des exécutable.
- Ajoute la prise en charge de nouvelles normes telles que TLS 1.2.

Inconvénient :

- Nécessite iOS 6 ou version ultérieure.
- Non disponible de watchOS.
- Certaines fonctionnalités de client HTTP et les options ne sont pas disponibles.

#### <a name="nsurlsession-handler"></a>Gestionnaire de NSUrlSession

Le Gestionnaire de NSUrlSession basé est basé sur natif `NSUrlSession` API.

Avantage :

- Utilise l’API native pour améliorer les performances et réduit la taille des exécutable.
- Ajoute la prise en charge de nouvelles normes telles que TLS 1.2.

Inconvénient :

- Nécessite iOS 7 ou version ultérieure.
- Certaines fonctionnalités de client HTTP et les options ne sont pas disponibles. 

## <a name="diagnosing-ats-issues"></a>Diagnostic des problèmes de couches d’application

Lorsque vous tentez de vous connecter à internet, directement ou à partir d’un affichage web dans iOS 9, vous pouvez obtenir une erreur dans le formulaire :

> Sécurité de Transport d’application a bloqué un texte en clair HTTP (http://www.-the-blocked-domain.com) chargement des ressources, car il n’est pas sécurisée. Exceptions temporaires peuvent être configurées via le fichier Info.plist de votre application.

Sécurité de Transport d’application (ATS) iOS9, garantit des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et de votre application. En outre, ATS nécessite la communication à l’aide du `HTTPS` protocole et haut niveau communication API à l’aide du protocole TLS version 1.2 avec le secret de transfert.

Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de `NSURLConnection`, `CFURL` ou `NSURLSession` sera soumis à des exigences de sécurité de couches d’application. Si vos connexions ne répondent pas à ces requise, ne pourront pas avec une exception.

Apple offre également la [TLSTool, exemple d’application](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) qui peut être compilé (ou éventuellement transcodé Xamarin et c#) et utilisé pour diagnostiquer les problèmes ATS/TLS. Consultez le [hors-Opting ATS](#optout) section ci-dessous pour plus d’informations sur la façon de résoudre ce problème.


<a name="config" />

## <a name="configuring-ats-options"></a>Configuration des Options de couches d’application

Vous pouvez configurer plusieurs fonctionnalités de couches d’application en définissant les valeurs de clés spécifiques dans votre application **Info.plist** fichier. Les clés suivantes sont disponibles pour le contrôle des couches d’application (_mis en retrait pour montrer comment elles sont imbriquées_) :

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Chaque clé a le type et la signification suivante :

- **NSAppTransportSecurity** (`Dictionary`) : contient toutes les clés de paramètres et les valeurs pour les couches d’application.
- **NSAllowsArbitraryLoads** (`Boolean`) - si `YES` ATS sera désactivée pour n’importe quel domaine **pas** répertoriés dans `NSExceptionDomains`. Pour les domaines répertoriés, les paramètres de sécurité spécifiés servira.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) - si `YES` permettra de pages web charger correctement lors de la protection de la sécurité de Transport Apple (ATS) est toujours activée pour le reste de l’application.
- **NSExceptionDomains** (`Dictionary`)-collection de domaines qui et les paramètres de sécurité que ATS doit utiliser pour un domaine donné.
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-une collection d’exceptions pour un domaine donné (par exemple). `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-la version minimale de TLS en tant que `TLSv1.0`, `TLSv1.1` ou `TLSv1.2` (qui est la valeur par défaut).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) - si `NO` le domaine n’a pas utiliser un chiffrement avec la sécurité de transfert. La valeur par défaut est `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - si `NO` (la valeur par défaut) toutes les communications avec ce domaine doit être dans le `HTTPS` protocole.
- **NSRequiresCertificateTransparency** (`Boolean`) - si `YES` couche SSL du domaine (Secure Sockets) doivent inclure les données de transparence valide. La valeur par défaut est `NO`.
- **NSIncludesSubdomains** (`Boolean`) - si `YES` ces paramètres remplacent tous les sous-domaines de ce domaine. La valeur par défaut est `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-version du protocole TLS utilisé lorsque le domaine est un service tiers 3e en dehors du contrôle du développeur.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) - si `YES` un domaine tiers 3e requiert la confidentialité.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - si `YES` l’ATS permettra non sécurisé de communication avec les domaines 3ème partie.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Si vous activez à la sortie de couches d’application

Lors de l’Apple suggère fortement à l’aide de la `HTTPS` protocole et une communication sécurisée à internet en fonction des informations, il peut arriver que cela n’est pas toujours possible. Par exemple, si vous communique avec un service web de tiers 3e ou à l’aide d’internet remis de publicités dans votre application.

Si votre application Xamarin.iOS doit faire une demande à un domaine non sécurisé, les modifications suivantes de votre application **Info.plist** fichier désactivera les paramètres par défaut de la sécurité qui applique des couches d’application pour un domaine donné :

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

À l’intérieur de Visual Studio pour Mac, double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions**, basculez vers le **Source** afficher et ajouter les clés ci-dessus :

[![](ats-images/ats01.png "La vue de Source du fichier Info.plist")](ats-images/ats01.png#lightbox)


Si votre application doit charger et afficher le contenu web à partir de sites non sécurisés, ajoutez le code suivant de votre application **Info.plist** pour permettre le chargement lors de la protection de la sécurité de Transport Apple (ATS) est toujours activée pour le reste des pages web de l’application :

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Si vous le souhaitez, vous pouvez apporter les modifications de suivantes à votre application **Info.plist** fichier désactiver complètement les couches d’application pour tous les domaines et la communication internet :

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

À l’intérieur de Visual Studio pour Mac, double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions**, basculez vers le **Source** afficher et ajouter les clés ci-dessus :

[![](ats-images/ats02.png "La vue de Source du fichier Info.plist")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> Si votre application nécessite une connexion à un site Web non sécurisé, vous devez **toujours** entrer le domaine comme une exception à l’aide de `NSExceptionDomains` au lieu de la désactivation ATS complètement à l’aide de `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` doit uniquement être utilisé dans des situations extrêmes d’urgence.




Là encore, la désactivation ATS doit _uniquement_ être utilisé qu’en dernier recours, si la commutation de connexions sécurisées est indisponible ou peu pratique.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a introduit la sécurité de Transport d’application (ATS) et décrit la façon qui garantit des communications sécurisées avec internet. Tout d’abord, nous avons abordé les modifications que nécessite des couches d’application pour une application Xamarin.iOS s’exécutant sur iOS 9. Ensuite, nous avons abordé contrôle ATS fonctionnalités et options. Enfin, nous avons abordé refus ATS dans votre application Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
