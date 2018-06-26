---
title: Obtention d’un Google mappe une clé API
description: Pour obtenir une clé API mappe de Google pour l’ajout des mappages des fonctionnalités à votre application.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: 365bc56c70ef903622c3a4583a30460f907b4ec9
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935052"
---
# <a name="obtaining-a-google-maps-api-key"></a>Obtention d’un Google mappe une clé API

Pour utiliser la fonctionnalité de Google Maps dans Android, vous devez enregistrer une clé API Maps avec Google. Jusqu'à ce que vous faites cela, vous verrez seulement une grille vierge au lieu d’une carte dans vos applications. Vous devez obtenir une clé de v2 API Android de Google Maps - clés à partir de l’ancienne v1 clé API Android de Google Maps ne fonctionneront pas.

Obtention d’une clé de v2 API Maps implique les étapes suivantes :

1.  Récupérer l’empreinte SHA-1 de combinaison de touches qui permet de signer l’application.
2.  Créez un projet dans la console APIs Google.
3.  Obtention de la clé d’API.


## <a name="obtaining-your-signing-key-fingerprint"></a>Obtention de votre empreinte de clé de signature

Pour demander une clé d’API de mappages à partir de Google, vous devez connaître l’empreinte SHA-1 de combinaison de touches qui permet de signer l’application.
En règle générale, cela signifie vous devez déterminer l’empreinte SHA-1 pour le magasin de clés de débogage, et puis l’empreinte pour le magasin de clés qui est utilisé pour signer votre application pour cette version de l’algorithme SHA-1.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Par défaut, le magasin de clés qui est utilisé pour signer les versions debug d’une application peut être de Xamarin.Android trouvé à l’emplacement suivant :

**C:\\utilisateurs\\[USERNAME]\\AppData\\Local\\Xamarin\\Mono pour Android\\debug.keystore**

Vous pouvez obtenir des informations sur un magasin de clés en exécutant la commande `keytool` du JDK. Cet outil se trouve généralement dans le répertoire bin de Java :

**C:\\Program Files (x86)\\Java\\jdk [VERSION]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Par défaut, le magasin de clés qui est utilisé pour signer les versions debug d’une application peut être de Xamarin.Android trouvé à l’emplacement suivant :

**/Users/[USERNAME]/.local/Share/Xamarin/mono pour Android/debug.keystore**

Vous pouvez obtenir des informations sur un magasin de clés en exécutant la commande `keytool` du JDK. Cet outil se trouve généralement dans le répertoire bin de Java :

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


Exécutez keytool à l’aide de la commande suivante (en utilisant les chemins d’accès du fichier indiqués ci-dessus) :

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Exemple de Debug.keystore

Pour la clé de débogage par défaut (qui est automatiquement créée pour le débogage), utilisez la commande :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>Clés de production

Lorsque vous déployez une application pour Google Play, il doit être [signé avec une clé privée](~/android/deploy-test/signing/index.md).
Le `keytool` devra être exécuter avec les détails de la clé privées et l’empreinte SHA-1 résultante permet de créer une clé de Google Maps API de production. N’oubliez pas de mettre à jour le **AndroidManifest.xml** fichier avec la clé d’API de Google Maps correcte avant le déploiement.

### <a name="keytool-output"></a>Sortie de keytool

Vous devez voir quelque chose comme la sortie suivante dans la fenêtre de console :

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

Vous allez utiliser l’empreinte numérique SHA-1 (répertoriés après **SHA1**) plus loin dans ce guide.

## <a name="creating-an-api-project"></a>Création d’un projet d’API

Après avoir récupéré l’empreinte de SHA-1 du magasin de clés de signature, il est nécessaire créer un nouveau projet dans la console APIs Google (ou ajoutez le service de v2 d’API Android de Google Maps à un projet existant).

1. Dans un navigateur, accédez à la [API de Console des développeurs Google & tableau de bord Services](https://console.developers.google.com/apis/dashboard/) et cliquez sur **sélectionnez un projet**. Cliquez sur un nom de projet ou créez-en un en cliquant sur **nouveau projet**:

   [![Bouton de Google Developer Console créer un projet](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Si vous avez créé un nouveau projet, entrez le nom du projet dans le **nouveau projet** boîte de dialogue qui s’affiche. Cette boîte de dialogue sera fabriquer un ID unique du projet qui est basé sur le nom de votre projet. Ensuite, cliquez sur le **créer** bouton comme indiqué dans cet exemple :

   [![Nouveau projet est nommé XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Après environ une minute, le projet est créé et vous accédez à la **tableau de bord** page du projet. À partir de là, cliquez sur **activez les API et les SERVICES**:

   [![En cliquant sur les API Android de Google Maps dans la section de la bibliothèque](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. À partir de la **bibliothèque d’API** , cliquez sur **SDK de mappages pour Android**. Dans la page suivante, cliquez sur **activer** pour activer le service pour ce projet :

   [![En cliquant sur le bouton Activer dans la section du tableau de bord](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

À ce stade, le projet d’API a été créé et l’API Android de Google Maps v2 a été ajouté à ce dernier. Toutefois, vous ne pouvez pas utiliser cette API dans votre projet jusqu'à ce que vous créez des informations d’identification pour celle-ci. La section suivante explique comment créer une clé d’API et de la liste blanche une application Xamarin.Android afin qu’il est autorisé à utiliser cette clé.

## <a name="obtaining-the-api-key"></a>Obtention de la clé d’API

Après le **Console des développeurs Google** projet d’API a été créé, il est nécessaire de créer une clé d’API Android. Applications de Xamarin.Android doivent avoir une clé d’API avant de pouvoir accéder à des API de mappage Android v2.

1. Dans le **SDK de mappages pour Android** page qui s’affiche (après avoir cliqué sur **activer** à l’étape précédente), accédez à la **informations d’identification** onglet et cliquez sur le **créer informations d’identification** bouton :

   [![Mappe le Kit de développement logiciel pour le message d’informations d’identification Android](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Cliquez sur **clé API**:

   [![Ajouter des informations d’identification à votre boîte de dialogue de projet](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Une fois que l’utilisateur clique sur ce bouton, la clé API est générée. Ensuite, il est nécessaire de restreindre cette clé pour seulement votre application peut appeler des API avec cette clé. Cliquez sur **RESTRICT clé**:

   [![En cliquant sur restreindre la clé dans la page informations d’identification](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Modifier la **nom** champ **clé API 1** à un nom qui vous aideront à mémoriser la clé qui est utilisée pour (**XamarinMapsDemoKey** est utilisé dans cet exemple). Ensuite, cliquez sur le **des applications Android** case :

   [![Sélection des applications Android sur la page informations d’identification](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Pour ajouter l’empreinte numérique SHA-1, cliquez sur **+ ajouter un nom de package et les empreintes digitales**:

   [![En cliquant sur Ajouter le nom de package et une empreinte digitale](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Entrez le nom du package de votre application et l’empreinte numérique du certificat SHA-1 (obtenus via `keytool` comme expliqué plus haut dans ce guide). Dans l’exemple suivant, le package nom `XamarinMapsDemo` est entré, suivie de l’empreinte numérique du certificat SHA-1 obtenu à partir de **debug.keystore**:

   [![Nom du package entré est com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Notez que, afin que votre APK accéder aux mappages de Google, vous devez inclure les empreintes de SHA-1 et noms pour chaque magasin de clés (debug et release) que vous utilisez pour signer votre APK du package. Par exemple, si vous utilisez un ordinateur pour le débogage et un autre ordinateur pour générer l’APK de version, vous devez inclure l’empreinte numérique du certificat SHA-1 à partir du magasin de clés de débogage du premier ordinateur et l’empreinte numérique du certificat SHA-1 dans le magasin de clés de version de le deuxième ordinateur. Cliquez sur **+ ajouter un nom de package et les empreintes digitales** pour ajouter un autre nom de package et les empreintes digitales, comme illustré dans cet exemple :

   [![L’ajout d’une autre empreinte digitale crée un autre certificat SHA-1](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Cliquez sur le bouton **Save (Enregistrer)** pour enregistrer vos changements. Ensuite, vous revenez à la liste de vos clés API. Si vous avez d’autres clés de l’API que vous avez créé précédemment, ils sont également répertoriés ici. Dans cet exemple, une seule clé API (créée à l’étape précédente) est répertoriée :

   [![XamarinMapsDemoKey est indiqué dans la liste de clés d’API](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Connecter le projet à un compte facturable

À compter de juin, 11 2018, la clé d’API ne fonctionne pas si le projet n’est pas connecté à un compte facturable (même si le service est toujours disponible pour les applications mobiles).

1. Cliquez sur le bouton de menu hamburger et sélectionnez le **facturation** page :

   [![Sélection de la section de facturation du menu hamburger](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Lier le projet à un compte de facturation en cliquant sur **lier un compte de facturation** suivie **créer un compte de facturation** dans la fenêtre contextuelle affichée (si vous n’avez pas un compte, vous serez guidé pour créer un nouveau) :

   [![Projet de liaison pour le compte de facturation](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Ajout de la clé à votre projet

Enfin, ajoutez cette clé API pour le **AndroidManifest.XML** fichier de votre application Xamarin.Android. Dans l’exemple suivant, `YOUR_API_KEY` est remplacé par la clé API générée dans les étapes précédentes :

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>Liens associés

- [Console des API Google](https://code.google.com/apis/console/)
- [La clé d’API Google Maps](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
