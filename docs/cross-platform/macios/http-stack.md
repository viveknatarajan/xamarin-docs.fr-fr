---
title: HttpClient et sélecteur d’implémentation de SSL/TLS pour iOS/Mac OS
description: La pile HttpClient et SSL/TLS sélecteur d’implémentation détermine l’implémentation de HttpClient et SSL/TLS qui sera utilisée par votre application de Xamarin iOS, tvOS ou macOS.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: fd48c7148aadd8d156544113e2d719295294bf40
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40250997"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>Sélecteur d’implémentation de HttpClient et SSL/TLS pour iOS/Mac OS

Le **sélecteur d’implémentation de HttpClient** pour Xamarin.iOS, Xamarin.tvOS et Xamarin.Mac les contrôles qui `HttpClient` implémentation à utiliser. Vous pouvez basculer vers une implémentation qui utilise des transports natifs iOS, tvOS ou macOS (`NSUrlSession` ou `CFNetwork`, selon le système d’exploitation). L’avantage est binaires de TLS 1.2-prise en charge, plus petits et plus rapidement télécharge ; l’inconvénient est qu’il nécessite la boucle d’événements pour exécuter des opérations asynchrones à exécuter.

Projets doivent faire référence à la **System.Net.Http** assembly.

> [!WARNING]
> **Avril 2018** – en raison d’une sécurité accrue requises, y compris la conformité avec PCI, principaux fournisseurs de services cloud et serveurs web sont censées arrêter la prise en charge des versions TLS antérieures à 1.2.  Projets Xamarin créés dans les versions précédentes de Visual Studio par défaut à utiliser des versions antérieures de TLS.
>
> Afin de garantir à vos applications continuent de fonctionner avec ces serveurs et les services, **vous devez mettre à jour vos projets Xamarin avec le `NSUrlSession` configuration illustré ci-dessous, puis régénérez et redéployez vos applications** à vos utilisateurs.

### <a name="selecting-an-httpclient-stack"></a>Sélection d’une pile HttpClient

Pour ajuster le `HttpClient` utilisé par votre application :

1. Double-cliquez sur le **nom_projet** dans le **l’Explorateur de solutions** pour ouvrir les Options de projet.
2. Basculez vers le **Build** paramètres pour votre projet (par exemple, **Build iOS** pour une application Xamarin.iOS).
3. À partir de la **implémentation de HttpClient** liste déroulante, sélectionnez le `HttpClient` tapez comme l’une des opérations suivantes : **NSUrlSession** (recommandé), **CFNetwork**, ou  **Managed**.

[![Implémentation de HttpClient sélectionnables de managé, CFNetwork ou NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Prise en charge de TLS 1.2 de la `NSUrlSession` option est recommandée.

### <a name="nsurlsession"></a>NSUrlSession

Le `NSURLSession`-en fonction de gestionnaire est basé sur natif `NSURLSession` framework disponible dans iOS 7 et versions ultérieures. 
**Il s’agit du paramètre recommandé.**

#### <a name="pros"></a>Professionnels de l'

- Il utilise des API natives pour améliorer les performances et la plus petite taille de l’exécutable.
- Prise en charge les dernières normes, tels que TLS 1.2.

#### <a name="cons"></a>Inconvénients

- Requiert iOS 7 ou version ultérieure.
- Certains `HttpClient` fonctionnalités/options ne sont pas disponibles.

### <a name="cfnetwork"></a>CFNetwork

Le `CFNetwork`-en fonction de gestionnaire est basé sur natif `CFNetwork` framework disponible dans iOS 6 et versions ultérieures.

#### <a name="pros"></a>Professionnels de l'

- Il utilise des API natives pour améliorer les performances et la plus petite taille de l’exécutable.
- Prise en charge pour les standards plus récents tels que TLS 1.2.

#### <a name="cons"></a>Inconvénients

- Requiert iOS 6 ou version ultérieure.
- Non disponible sur watchOS.
- Certaines fonctionnalités de HttpClient/options ne sont pas disponibles.

### <a name="managed"></a>Managé

Le gestionnaire managé est le gestionnaire HttpClient entièrement géré qui a été livré avec une version précédente de Xamarin.

#### <a name="pros"></a>Professionnels de l'

- Il a la fonctionnalité la plus compatible avec Microsoft .NET et les versions antérieures de Xamarin.

#### <a name="cons"></a>Inconvénients

- Il n’est pas entièrement intégré avec les systèmes d’exploitation Apple et est limité à TLS 1.0. Il ne seront peut-être pas en mesure de se connecter pour sécuriser les serveurs web ou des services de cloud à l’avenir.
- Il est généralement beaucoup plus lent des services tels que le chiffrement que les API natives.
- Elle nécessite plus le code managé, créant ainsi une application plus volumineuse distribuable.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Définition par programmation HttpMessageHandler

Outre la configuration de l’échelle du projet indiquée ci-dessus, vous pouvez également instancier un `HttpClient` et injecter souhaité `HttpMessageHandler` via le constructeur, comme illustré dans ces extraits de code :

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Cela rend possible d’utiliser un autre `HttpMessageHandler` à partir de ce qui est déclaré dans le **Options du projet** boîte de dialogue.

## <a name="ssltls-implementation"></a>Implémentation de SSL/TLS

SSL (Secure Socket Layer) et son successeur, TLS (Transport Layer Security), prennent en charge pour HTTP et d’autres connexions réseau via `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS ou de Xamarin.Mac `System.Net.Security.SslStream` implémentation appellera l’implémentation de SSL/TLS native d’Apple au lieu d’utiliser l’implémentation managée fournie par Mono. Implémentation native d’Apple prend en charge TLS 1.2.

> [!WARNING]
> La prochaine version de Xamarin.Mac 4.8 sera prennent uniquement en charge macOS 10.9 ou version ultérieure.
> Les versions précédentes de Xamarin.Mac pris en charge macOS 10.7 ou une version ultérieure, mais ces versions antérieures de macOS ne disposent pas d’une infrastructure TLS suffisante pour prendre en charge TLS 1.2. Pour cibler macOS 10.7 ou macOS est 10.8, utilisez Xamarin.Mac 4.6 ou une version antérieure.

## <a name="app-transport-security"></a>Sécurité de Transport de l’application

Apple _App Transport Security_ (ATS) applique des connexions sécurisées entre les ressources internet (par exemple, le serveur de l’application back-end) et votre application. ATS garantit que toutes les communications internet sont conformes pour sécuriser la connexion meilleures pratiques, ce qui empêche la divulgation accidentelle d’informations sensibles directement par le biais de votre application ou une bibliothèque qui il consomme.

Étant donné que ATS est activé par défaut dans les applications développées pour iOS 9, tvOS 9 et OS X 10.11 (El Capitan) et les versions ultérieures, toutes les connexions à l’aide de `NSUrlConnection`, `CFUrl` ou `NSUrlSession` sera soumis à des exigences de sécurité ATS. Si vos connexions ne répondent pas à ces exigences, ils échoueront avec une exception.

En fonction de vos sélections de pile HttpClient et d’implémentation de SSL/TLS, vous devrez peut-être apporter des modifications à votre application pour fonctionner correctement avec ATS.

Pour en savoir plus sur ATS, veuillez consulter notre [guide de sécurité de Transport application](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Problèmes connus

Cette section décrit les problèmes connus avec la prise en charge TLS dans Xamarin.iOS.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Échec du chargement avec l’erreur « Valeur demandée AppleTLS n’a pas été trouvé » du projet

Xamarin.iOS 9.8 a introduit de nouveaux paramètres de contenu le **.csproj** fichier pour une application Xamarin.iOS. Ces modifications peuvent provoquer des problèmes lorsque le projet est ouvert avec les versions antérieures de Xamarin.iOS. La capture d’écran suivante est un exemple de message d’erreur qui peut-être s’afficher dans ce scénario :

![Capture d’écran de l’erreur lors du chargement du projet, demandé hérité de valeur introuvable](http-stack-images/tlserror-xs.png)

Cette erreur est due à l’introduction de la `MtouchTlsProvider` définition sur le fichier projet dans Xamarin.iOS 9.8. Si elle n’est pas possible de mettre à jour vers Xamarin.iOS 9.8 (ou version ultérieure), le travail est environ pour modifier manuellement le **.csproj** d’application de fichiers, supprimez le `MtouchTlsprovider` élément, puis enregistrez le fichier de projet modifié.

L’extrait de code suivant est un exemple de ce à quoi le `MtouchTlsProvider` paramètre peut sembler comme à l’intérieur d’un **.csproj** fichier :

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Liens connexes

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Sécurité de transport de l’application](~/ios/app-fundamentals/ats.md)
