---
title: Transport Layer Security (TLS) 1.2
description: Ce document décrit comment l’activation de TLS 1.2 pour les projets Xamarin.iOS, Xamarin.Android et Xamarin.Mac. Il montre comment le faire dans Visual Studio 2019 et Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 26870ae0e84a84a7b78f7766a8e134ecfc7b223e
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855001"
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

À l’aide de la dernière version de [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) est important de s’assurer des communications de réseau d’application sont sécurisées.

> [!WARNING]
> **Avril 2018** – en raison d’une sécurité accrue requises, y compris la conformité avec PCI, principaux fournisseurs de services cloud et serveurs web sont censées arrêter la prise en charge des versions TLS antérieures à 1.2.  Projets Xamarin créés dans les versions précédentes de Visual Studio par défaut à utiliser des versions antérieures de TLS.
>
> Afin de garantir à vos applications continuent de fonctionner avec ces serveurs et les services, **vous devez mettre à jour vos projets Xamarin pour utiliser les paramètres ci-dessous, puis régénérer et redéployer vos applications** à vos utilisateurs.

Projets doivent faire référence à la **System.Net.Http** assembly et être configuré comme indiqué ci-dessous.

## <a name="update-xamarinandroid-to-tls-12"></a>Mettre à jour Xamarin.Android au protocole TLS 1.2

Mise à jour le **implémentation de HttpClient** et **implémentation de SSL/TLS** options pour activer la sécurité de TLS 1.2.

> [!NOTE]
> Nécessite Android 5.0 ou version ultérieure.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ces paramètres se trouvent dans **propriétés du projet > Options Android** puis en cliquant sur le **avancé** bouton :

[![Configurer HttpClient et TLS dans Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ces paramètres se trouvent dans **Options du projet > Build > Build Android** onglet :

[![Configurer HttpClient et TLS dans Visual Studio pour Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Mettre à jour de Xamarin.iOS sur TLS 1.2

Mise à jour le **implémentation de HttpClient** option pour activer la sécurité de TSL 1.2.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ce paramètre se trouve dans **propriétés du projet > Build iOS**:

[![Configurer HttpClient et TLS dans Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ce paramètre se trouve dans **Options du projet > Build > Build iOS** onglet :

[![Configurer HttpClient dans Visual Studio pour Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Mettre à jour de Xamarin.Mac au protocole TLS 1.2

Dans Visual Studio pour Mac, pour activer TLS 1.2 dans une application Xamarin.Mac, mettez à jour le **implémentation de HttpClient** option **Options du projet > Build > Build Mac**:

[![Configurer HttpClient dans Visual Studio pour Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> La prochaine version de Xamarin.Mac (4.8) prendra uniquement en charge macOS 10.9 ou ultérieur.
> Les versions précédentes de Xamarin.Mac prenaient en charge macOS 10.7 ou ultérieur, mais l’infrastructure TLS des anciennes versions de macOS n’était pas suffisante pour prendre en charge TLS 1.2. Pour cibler macOS 10.7 ou macOS 10.8, utilisez Xamarin.Mac 4.6 ou antérieur.

## <a name="alternative-configuration-options"></a>Options de configuration de remplacement

Cette section traite des alternatives aux configurations prises en charge de TLS 1.2 ci-dessus.
Les développeurs d’applications ne devraient envisager ces alternatives que si elles comprennent les risques liés à l’aide de différents niveaux de prise en charge TLS.

### <a name="httpclient-implementation"></a>Implémentation de HttpClient

Les développeurs Xamarin ont toujours été en mesure d’utiliser les classes de mise en réseau natifs dans leur code, cependant, il est également une option qui détermine quelle pile réseau est utilisée par le `HttpClient` classes. Cela fournit une API .NET familière qui présente l’avantage de vitesse et la sécurité de la plateforme native.

Les options sont les suivantes :

- **Pile gérée** – les fonctionnalités réseau de Mono fournie, ou
- **Pile native** – diverses API fournies par les plateformes sous-jacentes (Android, iOS ou macOS) de mise en réseau.

La pile gérée fournit le plus haut niveau de compatibilité avec le code .NET existant, mais il peut être plus lent et entraîner de plus grande taille de l’exécutable.

Les options natives peuvent être plus rapides et avoir une meilleure sécurité (y compris TLS 1.2), mais ne peut pas fournir toutes les fonctionnalités et les options de la `HttpClient` classe.

### <a name="ssltls-implementation-android"></a>Implémentation de SSL/TLS (Android)

Options de projet Android vous permettent également de choisir quelle implémentation de SSL/TLS pour prendre en charge :

- **Mono/géré** – TLS 1.1 sur Android
- **Natif** – TLS 1.2 sur Android.

Par défaut des nouveaux projets de Xamarin à l’implémentation native qui prend en charge TLS 1.2 (ce qui est recommandé pour tous les projets), toutefois vous pouvez basculer vers le code managé si nécessaire pour des raisons de compatibilité.

> [!IMPORTANT]
> Le **Mono/Managed** option a été [retiré iOS et Mac](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) options du projet.
>
> L’option Native est toujours utilisée sur les plateformes iOS et Mac.

## <a name="platform-specific-details"></a>Détails spécifiques à la plateforme

Le résumé ci-dessus explique les paramètres au niveau du projet pour l’implémentation de HttpClient et SSL/TLS dans les projets Xamarin. L’implémentation de HttpClient peut également être définie de manière dynamique dans le code. Consultez ces guides spécifiques à la plateforme pour plus d’informations :

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS et Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>Récapitulatif

Applications doivent utiliser la sécurité TLS (Transport Layer) 1.2 autant que possible.
Vous devez mettre à jour les paramètres dans les applications existantes en suivant les instructions dans cet article, puis régénérez et déployez de nouveau à vos clients.

## <a name="related-links"></a>Liens connexes

- [Sécurité de transport de l’application](~/ios/app-fundamentals/ats.md)
- [Environnement de Xamarin.Android](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (février 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Notes de publication mono 4.8 - la prise en charge TLS 1.2](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler et WebRequestHandler expliqué](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](https://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](xref:CoreFoundation.CFNetwork)
- [Foundation.NSUrlConnection](xref:Foundation.NSUrlConnection)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP Client (exemple)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
