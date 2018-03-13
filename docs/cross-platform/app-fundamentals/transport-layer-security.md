---
title: Transport Layer Security (TLS)
description: "L’activation de TLS 1.2 pour les projets Xamarin sur Android, iOS et Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: b0f205c5ab2c65f0e2a99f912f3961f12a4f2b7a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

_L’activation de TLS 1.2 pour les projets Xamarin sur Android, iOS et Mac_

À l’aide de la dernière version de [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) est essentielle pour assurer des communications réseau d’application sont sécurisées.

> [!NOTE]
> Mises à jour de Xamarin depuis [février 2017](https://releases.xamarin.com/stable-release-cycle-9/) utilisent TLS 1.2 dans les nouveaux projets par défaut.

Prise en charge de TLS 1.2 est désormais disponible dans :

* Mono 4.8 (inclut [prise en charge de TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (nécessite Android 5.0 ou version ultérieure)

Projets doivent faire référence à la **System.Net.Http** assembly. 

## <a name="updating-to-tls-12"></a>Mise à jour de TLS 1.2

Cette section décrit certaines des options de configuration de mise en réseau dans les projets de Xamarin, vous pouvez mettre à jour votre _existant_ applications pour tirer parti du protocole plus sécurisé.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ces paramètres se trouvent dans **Options du projet > Options Android** puis en cliquant sur le **avancé** bouton : 

[![Configurer le client HTTP et TLS dans Visual Studio](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
Ces paramètres se trouvent dans **propriétés du projet > Options de Build > Avancé** onglet :

[![Configurer le client HTTP et TLS dans Xamarin Studio et Visual Studio pour Mac](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png#lightbox)

-----


### <a name="httpclient-implementation"></a>Implémentation d’un client HTTP

Les développeurs Xamarin ont toujours été en mesure d’utiliser les classes de mise en réseau natifs dans leur code, toutefois, il est également une option qui détermine quels pile réseau est utilisée par le `HttpClient` classes. Cela fournit une API .NET familiers qui présente les avantages de vitesse et de sécurité de la plateforme native.

Les options sont les suivantes :

- **Pile managée** – les fonctionnalités réseau Mono fournis, ou
- **Pile native** – différentes mise en réseau des API fournies par les plateformes sous-jacent (Android, iOS ou Mac OS).

La pile managée fournit le plus haut niveau de compatibilité avec le code .NET existant mais il peut être plus lents et entraîner de plus grande taille de l’exécutable.

Les options natives peuvent être plus rapides et avoir une meilleure sécurité (y compris le protocole TLS 1.2), mais peut ne pas fournir toutes les fonctionnalités et les options de la `HttpClient` classe.


### <a name="ssltls-implementation"></a>Implémentation de SSL/TLS

Options du projet vous permettent également de choisir quelle implémentation de SSL/TLS pour prendre en charge :

- **Mono/Managed** – TLS 1.1 sur Android, TLS 1.0 sur iOS et macOS.
- **Native** – TLS 1.2 sur Android, iOS et macOS.

Nouveaux projets Xamarin par défaut pour l’implémentation native qui prend en charge TLS 1.2 (ce qui est recommandé pour tous les projets), toutefois, vous pouvez revenir au code managé si nécessaire pour des raisons de compatibilité.

> [!IMPORTANT]
> Le **Mono/Managed** option sera supprimée dans une [mise en production ultérieure](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/).
>
> L’option Native est recommandée.

## <a name="platform-specific-details"></a>Détails spécifiques à la plateforme

Le résumé ci-dessus décrit les paramètres au niveau du projet pour l’implémentation d’un client HTTP et SSL/TLS dans les projets Xamarin. L’implémentation HttpClient peut également être définie dynamiquement dans le code, et sur iOS, il existe deux options de native.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS et Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Récapitulatif

Applications doivent utiliser la sécurité TLS (Transport Layer) 1.2 autant que possible.
De nouvelles applications par défaut maintenant à cette configuration, mais vous devrez peut-être mettre à jour les paramètres dans les applications existantes en suivant les instructions dans cet article.

## <a name="related-links"></a>Liens associés

- [Sécurité de transport de l’application](~/ios/app-fundamentals/ats.md)
- [Environnement de Xamarin.Android](~/android/deploy-test/environment.md)
- [Cycle de Xamarin 9 (février 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Notes de publication mono 4.8 - la prise en charge TLS 1.2](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler et WebRequestHandler expliqué](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP Client (exemple)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
