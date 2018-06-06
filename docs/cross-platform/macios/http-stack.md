---
title: HttpClient et sélecteur d’implémentation de SSL/TLS pour iOS/macOS
description: La pile de client HTTP et SSL/TLS le sélecteur d’implémentation détermine l’implémentation de client HTTP et SSL/TLS qui sera utilisée par votre application iOS, tvOS ou macOS de Xamarin.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 9de2c97933bd33111a751be51e06dffe09794f15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782266"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient et sélecteur d’implémentation de SSL/TLS pour iOS/macOS

Le **HttpClient implémentation sélecteur** pour Xamarin.iOS, Xamarin.tvOS et Xamarin.Mac les contrôles qui `HttpClient` implémentation à utiliser. Vous pouvez basculer vers une implémentation qui utilise des transports natives iOS, tvOS ou macOS (`NSUrlSession` ou `CFNetwork`, selon le système d’exploitation). L’avantage est binaires de TLS 1.2-prise en charge, plus petits et plus rapide télécharge ; l’inconvénient est qu’elle requiert la boucle d’événements doit être exécuté pour les opérations asynchrones à exécuter.

Projets doivent faire référence à la **System.Net.Http** assembly.

> [!WARNING]
> **Avril, 2018** : en raison d’une sécurité accrue spécifications, y compris la mise en conformité PCI, principaux fournisseurs de cloud et les serveurs web sont attendus pour arrêter la prise en charge des versions TLS antérieures à 1.2.  Xamarin les projets créés dans les versions précédentes de Visual Studio par défaut à utiliser les anciennes versions de TLS.
>
> Afin de vérifier vos applications continuent à fonctionner avec ces serveurs et les services, **vous devez mettre à jour vos projets Xamarin avec la `NSUrlSession` configuration illustré ci-dessous, puis régénérer et redéployer vos applications** à vos utilisateurs.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>Sélection d’une pile HttpClient

Pour ajuster la `HttpClient` utilisé par votre application :

1. Double-cliquez sur le **nom du projet** dans les **l’Explorateur de solutions** pour ouvrir les Options de projet.
2. Basculez vers le **générer** paramètres de votre projet (par exemple, **Build iOS** pour une application Xamarin.iOS).
3. À partir de la **HttpClient implémentation** liste déroulante, sélectionnez le `HttpClient` type comme une des valeurs suivantes : **NSUrlSession** (recommandé), **CFNetwork**, ou  **Gérés**.

[![Choisissez HttpClient implémentation gérée, CFNetwork ou NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Prise en charge de TLS 1.2 de la `NSUrlSession` option est recommandée.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

Le `NSURLSession`-en fonction de gestionnaire est basée sur natif `NSURLSession` framework disponible dans iOS 7 et les versions ultérieures. 
**Il s’agit du paramètre recommandé.**

#### <a name="pros"></a>Professionnels de l'

- Il utilise des API natives pour améliorer les performances et la plus petite taille de l’exécutable.
- Prise en charge les normes les plus récentes telles que TLS 1.2.

#### <a name="cons"></a>Inconvénients

- Nécessite iOS 7 ou version ultérieure.
- Certains `HttpClient` fonctionnalités/options ne sont pas disponibles.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

Le `CFNetwork`-en fonction de gestionnaire est basée sur natif `CFNetwork` framework disponible dans iOS 6 et versions ultérieures.

#### <a name="pros"></a>Professionnels de l'

- Il utilise des API natives pour améliorer les performances et la plus petite taille de l’exécutable.
- Prise en charge de nouvelles normes telles que TLS 1.2.

#### <a name="cons"></a>Inconvénients

- Nécessite iOS 6 ou version ultérieure.
- Non disponible sur watchOS.
- Certaines fonctionnalités de client HTTP/options ne sont pas disponibles.

<a name="Managed" />

### <a name="managed"></a>Managé

Le gestionnaire managé est le Gestionnaire de HttpClient entièrement géré qui a été livré avec une version précédente de Xamarin.

#### <a name="pros"></a>Professionnels de l'

- Il a la fonctionnalité la plus compatible avec Microsoft .NET et des versions antérieures de Xamarin.

#### <a name="cons"></a>Inconvénients

- Il n’est pas entièrement intégré à ces systèmes d’exploitation Apple et est limité à TLS 1.0. Il ne seront peut-être pas en mesure de se connecter à sécuriser les serveurs web ou de services de cloud computing dans le futur.
- Il est généralement beaucoup plus lentes à des éléments tels que le chiffrement que les API natives.
- Elle nécessite plus le code managé, créant ainsi une application plus volumineuse distribuable.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Définition par programme HttpMessageHandler

En plus de la configuration à l’échelle du projet indiquée ci-dessus, vous pouvez également instancier un `HttpClient` et injecter souhaité `HttpMessageHandler` via le constructeur, comme illustré dans ces extraits de code :

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Il est ainsi possible d’utiliser une autre `HttpMessageHandler` à partir de ce qui est déclaré dans le **Options du projet** boîte de dialogue.

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation"></a>Implémentation de SSL/TLS

SSL (Secure Socket Layer) et son successeur, TLS (Transport Layer Security), prennent en charge HTTP et d’autres connexions réseau via `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS ou de Xamarin.Mac `System.Net.Security.SslStream` implémentation appelle implémentation de SSL/TLS native d’Apple au lieu d’utiliser l’implémentation managée fournie par Mono. Implémentation de native d’Apple prend en charge TLS 1.2.

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>Sécurité de Transport de l’application

Apple _sécurité du Transport application_ (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur principal de l’application) et de votre application. ATS garantit que toutes les communications internet sont conformes pour sécuriser la connexion meilleures pratiques, empêchant ainsi la divulgation accidentelle d’informations sensibles directement par le biais de votre application ou une bibliothèque qu’il utilise.

Étant donné que les couches d’application est activée par défaut dans les applications générées pour iOS 9, tvOS 9 et OS X 10.11 (El Capitan) et les versions ultérieures, toutes les connexions à l’aide de `NSUrlConnection`, `CFUrl` ou `NSUrlSession` sera soumis à des exigences de sécurité de couches d’application. Si vos connexions ne répondent pas à ces exigences, il échoue avec une exception.

En fonction de vos sélections HttpClient pile et l’implémentation de SSL/TLS, vous devrez peut-être apporter des modifications à votre application pour fonctionner correctement avec les couches d’application.

Pour plus d’informations sur les couches d’application, consultez notre [guide de sécurité de Transport application](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Problèmes connus

Cette section décrit les problèmes connus avec prise en charge TLS dans Xamarin.iOS.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Échec du chargement avec l’erreur « Valeur demandée AppleTLS n’a pas été trouvé » du projet

Xamarin.iOS 9.8 introduit de nouveaux paramètres de contenu le **.csproj** fichier pour une application Xamarin.iOS. Ces modifications peuvent provoquer des problèmes lors de l’ouverture du projet avec les versions antérieures de Xamarin.iOS. La capture d’écran suivante est un exemple de message d’erreur qui peut-être s’afficher dans ce scénario :

![Capture d’écran de l’erreur lors de la tentative de chargement du projet, demandé hérité valeur introuvable](http-stack-images/tlserror-xs.png)

Cette erreur est due à l’introduction de le `MtouchTlsProvider` paramètre dans le fichier projet Xamarin.iOS 9.8. Si elle n’est pas possible de mettre à jour vers Xamarin.iOS 9.8 (ou version ultérieure), le travail contourner ce problème consiste à modifier manuellement le **.csproj** de fichiers d’application, supprimez le `MtouchTlsprovider` élément, puis enregistrez le fichier projet modifié.

L’extrait de code suivant est un exemple de ce que le `MtouchTlsProvider` peut ressembler à ce paramètre comme à l’intérieur d’un **.csproj** fichier :

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Liens associés

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Sécurité de transport de l’application](~/ios/app-fundamentals/ats.md)
