---
title: Pile de HttpClient et sélecteur d’implémentation de SSL/TLS pour Android
description: Les sélecteurs de pile du client HTTP et l’implémentation de SSL/TLS déterminent l’implémentation du client HTTP et SSL/TLS qui sera utilisée par vos applications Xamarin.Android.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 2bc9b2a454b306f0794ef3704daa7e0fe6d04ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Pile de HttpClient et sélecteur d’implémentation de SSL/TLS pour Android

_Les sélecteurs de pile du client HTTP et l’implémentation de SSL/TLS déterminent l’implémentation du client HTTP et SSL/TLS qui sera utilisée par vos applications Xamarin.Android._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Android fournit deux zones de liste déroulante qui contrôlent les paramètres TLS pour une application Android. Une zone de liste déroulante qui identifiera `HttpMessageHandler` utilisera lors de l’instanciation d’un `HttpClient` de l’objet, tandis que l’autre identifie quelle implémentation de TLS sera utilisée par les demandes web.

> [!NOTE]
> Projets doivent faire référence à la **System.Net.Http** assembly.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Les paramètres de la pile HttpClient sont trouvent dans les Options de projet pour un projet Xamarin.Android. Cliquez sur le **Options Android** onglet, puis cliquez sur le **Options avancées** bouton. Cette action affiche la **avancée Android** boîte de dialogue qui comprend des zones de liste déroulante, une pour l’implémentation du client HTTP et pour l’implémentation de SSL/TLS :


[![Options Android Visual Studio](http-stack-images/tls07-vs-sml.png)](http-stack-images/tls07-vs.png#lightbox)

## <a name="httpclient-stack-selector"></a>Sélecteur de pile de HttpClient

Cette option de projet de contrôle qui `HttpMessageHandler` implémentation sera utilisée chaque fois qu’un `HttpClient` objet est instancié. Par défaut, il s’agit managé `HttpClientHandler`.

[![Zone de liste déroulante HttpClient implémentation Android dans Visual Studio](http-stack-images/tls04-vs-sml.png)](http-stack-images/tls04-vs.png#lightbox) 


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Les paramètres de la pile HttpClient sont trouvent dans les Options de projet pour un projet Xamarin.Android. Cliquez sur le **Générer > Générer Android** paramètres, puis cliquez sur le **général** onglet :

[![Visual Studio pour Mac des Options Android](http-stack-images/tls07-xs-sml.png)](http-stack-images/tls07-xs.png#lightbox)

## <a name="httpclient-stack-selector"></a>Sélecteur de pile de HttpClient

Cette option de projet de contrôle qui `HttpMessageHandler` implémentation sera utilisée chaque fois qu’un `HttpClient` objet est instancié. Par défaut, il s’agit managé `HttpClientHandler`.

![Zone de liste déroulante HttpClient implémentation Android dans Visual Studio pour Mac](http-stack-images/tls04-xs.png )

-----

### <a name="managed-httpclienthandler"></a>Managé (HttpClientHandler)

Gestionnaire managé est le Gestionnaire de HttpClient entièrement géré qui a été livré avec les versions précédentes de Xamarin.Android.

#### <a name="pros"></a>Avantage :

- Il est le plus compatible avec Microsoft .NET (fonctions) et les anciennes versions de Xamarin.

#### <a name="cons"></a>Inconvénient :

- Il n’est pas entièrement intégré avec le système d’exploitation (par exemple). limité à TLS 1.0).
- Il est généralement beaucoup plus lent (par exemple). le chiffrement) que l’API native.
- Elle nécessite plus le code managé, créer des applications plus importantes.

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler est le nouveau gestionnaire qui délègue au lieu d’implémenter tous les éléments dans le code managé en code Java/système d’exploitation natif.

#### <a name="pros"></a>Avantage :

- Utilisez l’API native pour améliorer les performances et la plus petite taille de l’exécutable.
- Prend en charge les dernières normes, par exemple. TLS 1.2.

#### <a name="cons"></a>Inconvénient :

- Nécessite Android 5.0 ou version ultérieure.
- Certaines fonctionnalités de client HTTP/options ne sont pas disponibles.

### <a name="choosing-a-handler"></a>Choix d’un gestionnaire

Le choix entre `AndroidClientHandler` et `HttpClientHandler` dépend des besoins de votre application. `AndroidClientHandler` est un bon choix si toutes les conditions suivantes s’appliquent :

-   Vous avez besoin de que prendre en charge de TLS 1.2 +.
-   Votre application cible Android 5.0 (API 21) ou version ultérieure.
-   Vous avez besoin de TLS 1.2 + prend en charge pour `HttpClient`.
-   Vous n’avez pas besoin de TLS 1.2 + prend en charge pour `WebClient`.

`HttpClientHandler` est un bon choix si vous avez besoin de TLS 1.2 + prend en charge mais doit prendre en charge les versions antérieures à Android 5.0 Android. Il est également un bon choix si vous avez besoin de TLS 1.2 + prend en charge pour `WebClient`.

À partir de Xamarin.Android 8.3, `HttpClientHandler` valeur par défaut est ennuyeuse de SSL (`btls`) en tant que le fournisseur sous-jacent de TLS. Le fournisseur ennuyeuse SSL, TLS offre les avantages suivants :

-   Il prend en charge TLS 1.2.
-   Il prend en charge toutes les versions d’Android.
-   Il prend en charge de TLS 1.2 pour les deux `HttpClient` et `WebClient`.

L’inconvénient de l’utilisation de SSL d’ennuyeuse comme fournisseur de TLS sous-jacent n’est que cela peut augmenter la taille de APK résultant (elle ajoute environ 1 Mo de taille APK supplémentaire par ABI pris en charge).

À partir de Xamarin.Android 8.3, le fournisseur TLS par défaut est ennuyeuse de SSL (`btls`). Si vous ne souhaitez pas utiliser ennuyeuse de SSL, vous pouvez revenir à l’implémentation de SSL managée historique en définissant le `$(AndroidTlsProvider)` propriété `legacy` (pour plus d’informations sur la définition des propriétés de build, consultez [processus de génération](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>Par programmation à l’aide de `AndroidClientHandler`

Le `Xamarin.Android.Net.AndroidClientHandler` est un `HttpMessageHandler` implémentation spécifiquement pour Xamarin.Android.
Les instances de cette classe utilisent natif `java.net.URLConnection` implémentation pour toutes les connexions HTTP. Cela fournit une augmentation des performances de HTTP et de plus petite taille APK théoriquement.

Cet extrait de code est un exemple montrant comment explicitement pour une instance unique de la `HttpClient` classe :

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> L’appareil Android sous-jacente doit prendre en charge TLS 1.2 (ie. Android 5.0 et versions ultérieures)


## <a name="ssltls-implementation-build-option"></a>Option de génération de mise en œuvre de SSL/TLS

Cette option de projet contrôle sous-jacent bibliothèque TLS sera utilisé par toutes les demandes web, les deux `HttpClient` et `WebRequest`. Par défaut, TLS 1.2 est sélectionnée :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zone de liste déroulante d’implémentation de TLS/SSL dans Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Zone de liste déroulante d’implémentation de TLS/SSL dans Visual Studio pour Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Par exemple :

```csharp
var client = new HttpClient();
```

Si la valeur de l’implémentation HttpClient **managé** et la valeur de l’implémentation de TLS **natif TLS 1.2 +**, le `client` objet utiliseriez automatiquement managé `HttpClientHandler` et TLS 1.2 (fourni par la bibliothèque BoringSSL) pour les requêtes HTTP.

Toutefois, si le **HttpClient implémentation** a la valeur `AndroidHttpClient`, puis toutes les `HttpClient` objets utilisera la classe Java sous-jacente `java.net.URLConnection` et ne seront pas affectées par la **implémentation de TLS/SSL** valeur. `WebRequest` objets utiliseriez la bibliothèque BoringSSL.

## <a name="other-ways-to-control-ssltls-configuration"></a>Autres façons de contrôler la configuration de SSL/TLS

Il existe trois façons qu’une application de Xamarin.Android peut contrôler les paramètres TLS :

1. Sélectionnez la bibliothèque TLS HttpClient des relatives à l’implémentation et la valeur par défaut dans les Options de projet.
2. Par programmation à l’aide de `Xamarin.Android.Net.AndroidClientHandler`.
3. Déclarer des variables d’environnement (facultatif).

Les trois choix, l’approche recommandée consiste à utiliser les options de projet Xamarin.Android pour déclarer la valeur par défaut `HttpMessageHandler` et TLS pour l’application entière. Ensuite, si nécessaire, d’instancier `Xamarin.Android.Net.AndroidClientHandler` objets. Ces options sont décrites ci-dessus.

La troisième option &ndash; à l’aide de variables d’environnement &ndash; est expliqué ci-dessous.

### <a name="declare-environment-variables"></a>Déclarer des Variables d’environnement

Il existe deux variables d’environnement qui sont liés à l’utilisation de TLS dans Xamarin.Android :

-   `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Cette variable d’environnement déclare la valeur par défaut `HttpMessageHandler` que l’application utilisera. Par exemple :

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

-   `XA_TLS_PROVIDER` &ndash; Cette variable d’environnement déclarera bibliothèque TLS sera utilisé, soit `btls`, `legacy`, ou `default` (qui est le même que si vous omettez cette variable) :

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Cette variable d’environnement est définie en ajoutant un _fichier d’environnement_ au projet. Un fichier d’environnement est un fichier texte brut au format Unix avec une action de génération **AndroidEnvironment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Capture d’écran de l’action de génération AndroidEnvironment dans Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Capture d’écran de la AndroidEnvironment build action dans Visual Studio pour Mac.](http-stack-images/tls03-xs.png)

-----

Consultez le [Xamarin.Android environnement](~/android/deploy-test/environment.md) guide pour plus d’informations sur les variables d’environnement et de Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
