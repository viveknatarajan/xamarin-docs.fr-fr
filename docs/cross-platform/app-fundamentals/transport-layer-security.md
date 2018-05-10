---
title: Transport Layer Security (TLS) 1.2
description: L’activation de TLS 1.2 pour les projets Xamarin sur Android, iOS et Mac
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 8e27801a9feb8cf7ba1534f88479dbf7259c3e85
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

À l’aide de la dernière version de [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) est essentielle pour assurer des communications réseau d’application sont sécurisées.

> [!WARNING]
> **Avril, 2018** : en raison d’une sécurité accrue spécifications, y compris la mise en conformité PCI, principaux fournisseurs de cloud et les serveurs web sont attendus pour arrêter la prise en charge des versions TLS antérieures à 1.2.  Xamarin les projets créés dans les versions précédentes de Visual Studio par défaut à utiliser les anciennes versions de TLS.
>
> Afin de vérifier vos applications continuent à fonctionner avec ces serveurs et les services, **vous devez mettre à jour vos projets Xamarin pour utiliser les paramètres ci-dessous, puis régénérer et redéployer vos applications** à vos utilisateurs.

Projets doivent faire référence à la **System.Net.Http** assembly et être configuré comme indiqué ci-dessous.

## <a name="update-android-to-tls-12"></a>Mettre à jour Android au protocole TLS 1.2

Mise à jour la **HttpClient implémentation** et **implémentation de SSL/TLS** options pour activer la sécurité de TLS 1.2.

> [!NOTE]
> Nécessite Android 5.0 ou version ultérieure.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ces paramètres se trouvent dans **propriétés du projet > Options Android** puis en cliquant sur le **avancé** bouton :

[![Configurer le client HTTP et TLS dans Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ces paramètres se trouvent dans **Options du projet > Générer > Générer Android** onglet :

[![Configurer le client HTTP et TLS dans Visual Studio pour Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>E/s de la mise à jour de TLS 1.2

Mise à jour la **HttpClient implémentation** option pour activer la sécurité de TSL 1.2.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ce paramètre peut se trouver dans **propriétés du projet > Build iOS**:

[![Configurer le client HTTP et TLS dans Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ce paramètre peut se trouver dans **Options du projet > Build > Build iOS** onglet :

[![Configurer HttpClient dans Visual Studio pour Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>MacOS mise à jour de TLS 1.2 dans Visual Studio pour Mac

Mise à jour la **HttpClient implémentation** option **Options du projet > Générer > Mac générer** onglet pour activer la sécurité de TSL 1.2 :

[![Configurer HttpClient dans Visual Studio pour Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>Options de configuration de remplacement

Cette section traite des alternatives aux configurations prises en charge de TLS 1.2 ci-dessus.
Les développeurs d’applications doivent uniquement déterminer ces alternatives si elles comprennent les risques liés à l’aide de différents niveaux de prise en charge TLS.

### <a name="httpclient-implementation"></a>Implémentation d’un client HTTP

Les développeurs Xamarin ont toujours été en mesure d’utiliser les classes de mise en réseau natifs dans leur code, toutefois, il est également une option qui détermine quels pile réseau est utilisée par le `HttpClient` classes. Cela fournit une API .NET familiers qui présente les avantages de vitesse et de sécurité de la plateforme native.

Les options sont les suivantes :

- **Pile managée** – les fonctionnalités réseau Mono fournis, ou
- **Pile native** – différentes mise en réseau des API fournies par les plateformes sous-jacent (Android, iOS ou Mac OS).

La pile managée fournit le plus haut niveau de compatibilité avec le code .NET existant mais il peut être plus lents et entraîner de plus grande taille de l’exécutable.

Les options natives peuvent être plus rapides et avoir une meilleure sécurité (y compris le protocole TLS 1.2), mais peut ne pas fournir toutes les fonctionnalités et les options de la `HttpClient` classe.

### <a name="ssltls-implementation-android"></a>Implémentation de SSL/TLS (Android)

Options de projet Android vous permettent également de choisir quelle implémentation de SSL/TLS pour prendre en charge :

- **Gérés/mono** – TLS 1.1 sur Android
- **Native** – TLS 1.2 sur Android.

Nouveaux projets Xamarin par défaut pour l’implémentation native qui prend en charge TLS 1.2 (ce qui est recommandé pour tous les projets), toutefois, vous pouvez revenir au code managé si nécessaire pour des raisons de compatibilité.

> [!IMPORTANT]
> Le **Mono/Managed** option a été [retirés iOS et Mac](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) options du projet.
>
> L’option Native est toujours utilisée sur la plateforme iOS et Mac utilisent.

## <a name="platform-specific-details"></a>Détails spécifiques à la plateforme

Le résumé ci-dessus décrit les paramètres au niveau du projet pour l’implémentation d’un client HTTP et SSL/TLS dans les projets Xamarin. L’implémentation HttpClient peut également être définie dynamiquement dans le code. Consultez ces guides spécifique à la plateforme pour plus d’informations :

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS et Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Récapitulatif

Applications doivent utiliser la sécurité TLS (Transport Layer) 1.2 autant que possible.
Vous devez mettre à jour les paramètres dans les applications existantes en suivant les instructions dans cet article, puis régénérer et redéployer sur vos clients.

## <a name="related-links"></a>Liens associés

- [Sécurité de transport de l’application](~/ios/app-fundamentals/ats.md)
- [Environnement de Xamarin.Android](~/android/deploy-test/environment.md)
- [Cycle de Xamarin 9 (février 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Notes de publication mono 4.8 - la prise en charge TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler et WebRequestHandler expliqué](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP Client (exemple)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
