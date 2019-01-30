---
title: Sécurité de Transport de l’application dans Xamarin.iOS
description: Sécurité de Transport de l’application (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et votre application.
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: f9308d3a746a5a0a43cf47cc5ea809c0f82bbe7b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233820"
---
# <a name="app-transport-security-in-xamarinios"></a>Sécurité de Transport de l’application dans Xamarin.iOS

_Sécurité de Transport de l’application (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et votre application._

Cet article vous présente les modifications de sécurité qui applique la sécurité de Transport d’application sur une application iOS 9 et [ce que cela signifie pour vos projets Xamarin.iOS](#xamarinsupport), il traitera le [les options de configuration ATS](#config) et Il traitera comment [adhérer de ATS](#optout) ATS si nécessaire. ATS étant activé par défaut, toutes les connexions internet non sécurisée lèvent une exception dans les applications iOS 9 (sauf si vous avez explicitement autorisé).


## <a name="about-app-transport-security"></a>Sur la sécurité de Transport de l’application

Comme indiqué ci-dessus, ATS garantit que toutes les communications internet dans iOS 9 et OS X El Capitan soient conformes pour sécuriser la connexion meilleures pratiques, ce qui empêche la divulgation accidentelle d’informations sensibles directement par le biais de votre application ou une bibliothèque qu’il est consommation.

Pour les applications existantes, vous devez implémenter le `HTTPS` protocole chaque fois que possible. Pour les nouvelles applications Xamarin.iOS, vous devez utiliser `HTTPS` exclusivement lorsqu’il communique avec les ressources internet. En outre, communication d’API générale doit être chiffrée à l’aide du protocole TLS version 1.2 avec secret de transfert.

Toute connexion établie avec [NSUrlConnection](xref:Foundation.NSUrlConnection), [CFUrl](xref:CoreFoundation.CFUrl) ou [NSUrlSession](xref:Foundation.NSUrlSession) utilisera ATS par défaut dans les applications développées pour iOS 9 et OS X 10.11 (El Capitan).

## <a name="default-ats-behavior"></a>ATS par défaut

Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de [NSUrlConnection](xref:Foundation.NSUrlConnection), [CFUrl](xref:CoreFoundation.CFUrl) ou [NSUrlSession](xref:Foundation.NSUrlSession) sera soumis pour Exigences de sécurité ATS. Si vos connexions ne répondent pas à ces besoins, ils échoueront avec une exception.

### <a name="ats-connection-requirements"></a>Exigences de connexion ATS

ATS applique les exigences suivantes pour toutes les connexions internet :

- Tous les chiffrements de connexion doivent être à l’aide de confidentialité. Afficher la liste des chiffrements acceptés ci-dessous.
- Le protocole de sécurité TLS (Transport Layer) doit être la version 1.2 ou supérieure.
- Au moins une empreinte digitale SHA256 avec un 2 048 bits ou supérieur clé RSA, ou un 256 bits ou supérieur à courbe elliptique (ECC) clé doit être utilisée pour tous les certificats.

Là encore, dans la mesure où ATS est activé par défaut dans iOS 9, toute tentative pour établir une connexion qui ne respecte pas ces exigences entraîne une exception est levée. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Chiffrements Compatible ATS

Le type de chiffrement secret de transfert suivantes sont acceptées par ATS sécurisée des communications internet :

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

Pour plus d’informations sur l’utilisation des classes de communication internet iOS, consultez d’Apple [référence de classe NSURLConnection](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL référence](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) ou [référence de classe de NSURLSession ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Prise en charge d’ATS dans Xamarin.iOS

ATS étant activé par défaut dans iOS 9 et OS X El Capitan, si votre application Xamarin.iOS ou toute bibliothèque ou le service à l’aide de l’il établit la connexion à internet, vous aurez besoin de prendre des mesures ou vos connexions entraîne une exception est levée.

Pour une application existante, les suggestions d’Apple vous prenez en charge la `HTTPS` protocole dès que possible. Si vous soit impossible, car vous vous connectez à un 3e tiers service web qui ne prend en charge `HTTPS` ou si la prise en charge `HTTPS` serait peu pratique, vous pouvez ne pas adhérer d’ATS. Consultez le [hors-Opting ATS](#optout) section ci-dessous pour plus d’informations.

Pour une application Xamarin.iOS, vous devez utiliser `HTTPS` exclusivement lorsqu’il communique avec les ressources internet. Là encore, il peut arriver (par exemple, à l’aide d’un service web de 3ème partie) où cela n’est pas possible et vous devez adhérer de ATS.

En outre, ATS applique de haut niveau communication API soient chiffrés à l’aide du protocole TLS version 1.2 avec secret de transfert. Consultez le [les exigences de connexion ATS](#ATS-Connection-Requirements) et [chiffrements Compatible ATS](#ATS-Compatible-Ciphers) sections ci-dessus pour plus d’informations.

Bien que vous n’êtes peut-être pas familiarisé avec TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) il est le successeur de SSL ([protocole SSL (Secure Socket Layer)](https://en.wikipedia.org/wiki/Transport_Layer_Security)) et fournit une collection de protocoles cryptographiques pour appliquer la sécurité sur connexions de réseau.

Le niveau TLS est contrôlé par le service web que vous consommez et est donc en dehors du contrôle de l’application. À la fois le `HttpClient` et `ModernHttpClient` doivent utiliser automatiquement le plus haut niveau de chiffrement TLS pris en charge par le serveur.

Selon le serveur que vous parlez à (en particulier s’il s’agit d’un service tiers 3e), vous devrez peut-être désactiver le secret de transfert ou sélectionnez un niveau inférieur de TLS. Consultez le [configuration des Options ATS](#Configuring-ATS-Options) section ci-dessous pour plus d’informations.

> [!IMPORTANT]
> Sécurité de Transport d’application ne s’applique pas aux applications de Xamarin à l’aide de **HTTPClient gérés implémentations**. Elle s’applique aux connexions à l’aide de CFNetwork **HTTPClient implémentations** ou **NSURLSession HTTPClient implémentations** uniquement.

### <a name="setting-the-httpclient-implementation"></a>Définition de l’implémentation de HTTPClient

Pour définir l’implémentation de HTTPClient utilisée par une application iOS, double-cliquez sur le **projet** dans le **l’Explorateur de solutions** pour ouvrir le **Options du projet**. Accédez à **Build iOS** et sélectionnez le type de client de votre choix sous le **implémentation de HttpClient** liste déroulante :

![](ats-images/client01.png "Définition des Options de Build iOS")


#### <a name="managed-handler"></a>Gestionnaire de code managé

Le gestionnaire managé est le gestionnaire HttpClient entièrement géré qui a été livré avec les versions antérieures de Xamarin.iOS et est le gestionnaire par défaut.

Avantage :

- Il est plus compatible avec Microsoft .NET et une version antérieure de Xamarin.

Inconvénient :

- Il n’est pas entièrement intégré à iOS (par exemple, il est limité à TLS 1.0).
- Il est généralement plus lente que les API natives.
- Elle nécessite plus de code géré et crée des applications plus grandes.

#### <a name="cfnetwork-handler"></a>Gestionnaire de CFNetwork

Le gestionnaire CFNetwork basé repose sur natif `CFNetwork` framework.

Avantage :

- Utilise l’API native pour améliorer les performances et l’exécutable de petite taille.
- Ajoute la prise en charge pour les standards plus récents tels que TLS 1.2.

Inconvénient :

- Requiert iOS 6 ou version ultérieure.
- Non disponible de watchOS.
- Certaines fonctionnalités de HttpClient et les options ne sont pas disponibles.

#### <a name="nsurlsession-handler"></a>Gestionnaire de NSUrlSession

Le Gestionnaire de NSUrlSession en repose sur natif `NSUrlSession` API.

Avantage :

- Utilise l’API native pour améliorer les performances et l’exécutable de petite taille.
- Ajoute la prise en charge pour les standards plus récents tels que TLS 1.2.

Inconvénient :

- Requiert iOS 7 ou version ultérieure.
- Certaines fonctionnalités de HttpClient et les options ne sont pas disponibles. 

## <a name="diagnosing-ats-issues"></a>Diagnostic des problèmes ATS

Lorsque vous tentez de vous connecter à internet, directement ou à partir d’une vue web dans iOS 9, vous pouvez obtenir une erreur dans le formulaire :

> Sécurité de Transport d’application a bloqué un texte en clair HTTP (http://www.-the-blocked-domain.com) chargement des ressources dans la mesure où il n’est pas sécurisé. Exceptions temporaires peuvent être configurées via le fichier Info.plist de votre application.

Dans iOS9, App Transport Security (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et votre application. En outre, ATS nécessite la communication à l’aide du `HTTPS` protocole et haut niveau communication API soient chiffrés à l’aide du protocole TLS version 1.2 avec secret de transfert.

Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9 et OS X 10.11 (El Capitan), toutes les connexions à l’aide de `NSURLConnection`, `CFURL` ou `NSURLSession` sera soumis à des exigences de sécurité ATS. Si vos connexions ne répondent pas à ces besoins, ils échoueront avec une exception.

Apple fournit également la [TLSTool, exemple d’application](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) qui peut être compilé (ou éventuellement transcodés à Xamarin et C#) et permet de diagnostiquer les problèmes ATS/TLS. Veuillez consulter la [hors-Opting ATS](#optout) section ci-dessous pour plus d’informations sur la façon de résoudre ce problème.


<a name="config" />

## <a name="configuring-ats-options"></a>Configuration des Options de ATS

Vous pouvez configurer plusieurs fonctionnalités de ATS en définissant les valeurs de clés spécifiques de votre application **Info.plist** fichier. Les clés suivantes sont disponibles pour le contrôle ATS (_mis en retrait pour montrer comment ils sont imbriqués_) :

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

- **NSAppTransportSecurity** (`Dictionary`) : contient toutes les clés de paramètres et valeurs pour ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) - si `YES` ATS sera désactivé pour n’importe quel domaine **pas** répertoriés dans `NSExceptionDomains`. Pour les domaines de la liste, les paramètres de sécurité spécifiés servira.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) - si `YES` permettra de pages web se charge correctement alors que la protection de la sécurité de Transport d’Apple (ATS) est toujours activée pour le reste de l’application.
- **NSExceptionDomains** (`Dictionary`)-une collection de domaines qui et les paramètres de sécurité qu’ATS doit utiliser pour un domaine donné.
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-une collection d’exceptions pour un domaine donné (par ex. `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-la version TLS minimale en tant que `TLSv1.0`, `TLSv1.1` ou `TLSv1.2` (qui est la valeur par défaut).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) - si `NO` le domaine n’a pas d’utiliser un chiffrement avec sécurité vers l’avant. La valeur par défaut est `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - si `NO` (la valeur par défaut) toutes les communications avec ce domaine doit être dans le `HTTPS` protocole.
- **NSRequiresCertificateTransparency** (`Boolean`) - si `YES` couche SSL du domaine (Secure Sockets) doit inclure les données de transparence valide. La valeur par défaut est `NO`.
- **NSIncludesSubdomains** (`Boolean`) - si `YES` ces paramètres remplacent tous les sous-domaines de ce domaine. La valeur par défaut est `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-version TLS The utilisé lorsque le domaine est un service tiers 3e en dehors du contrôle du développeur.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) - si `YES` un domaine tiers 3e nécessite le secret de transfert.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - si `YES` le ATS permettra non sécurisée de communication avec les domaines de tiers 3e.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Opt-Out d’ATS

Alors que Apple suggère fortement d’utiliser le `HTTPS` protocole et une communication sécurisée à internet en fonction d’informations, il peut arriver que ce n’est pas toujours possible. Par exemple, si vous communique avec un service web de 3ème partie ou à l’aide d’internet remis publicités dans votre application.

Si votre application Xamarin.iOS doit effectuer une demande à un domaine non sécurisé, les modifications suivantes à votre application **Info.plist** fichier désactivera les valeurs par défaut de la sécurité qu’ATS applique pour un domaine donné :

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


Si votre application doit charger et afficher le contenu web à partir de sites non sécurisées, ajoutez le code suivant à votre application **Info.plist** pour permettre le chargement pendant que la protection de la sécurité de Transport d’Apple (ATS) est toujours activée pour le reste des pages web de l’application :

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Si vous le souhaitez, vous pouvez apporter les modifications suivantes à votre application **Info.plist** fichier complètement désactiver ATS pour tous les domaines et communication internet :

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
> Si votre application nécessite une connexion à un site Web non sécurisé, vous devez **toujours** entrer le domaine en tant qu’une exception à l’aide `NSExceptionDomains` au lieu de la désactivation ATS complètement à l’aide de `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` doit uniquement être utilisé dans les situations d’urgence extrêmes.




Là encore, la désactivation ATS doit _uniquement_ être utilisé qu’en dernier recours, si la commutation pour des connexions sécurisées est indisponible ou peu pratique.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a introduit les application Transport Security (ATS) et décrit la façon il impose des communications sécurisées avec internet. Tout d’abord, nous avons abordé les modifications QU'ATS requiert pour une application Xamarin.iOS en cours d’exécution sur iOS 9. Puis nous avons abordé les options et fonctionnalités de contrôle ATS. Enfin, nous avons abordé refus ATS dans votre application Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
