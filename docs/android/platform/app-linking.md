---
title: La liaison App dans Android
description: "Ce guide explique comment Android 6.0 prend en charge la liaison application, une technique qui permet de répondre aux URL sur les sites Web des applications mobiles. Il explique quelles applications liaison est l’implémentation de la liaison application dans une application Android 6.0 et comment configurer un site Web pour accorder des autorisations à l’application mobile pour un domaine."
ms.topic: article
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 78fef780728ba1c2a3b9978504058f7a386b0e7d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="app-linking-in-android"></a>La liaison App dans Android

_Ce guide explique comment Android 6.0 prend en charge la liaison application, une technique qui permet de répondre aux URL sur les sites Web des applications mobiles. Il explique quelles applications liaison est l’implémentation de la liaison application dans une application Android 6.0 et comment configurer un site Web pour accorder des autorisations à l’application mobile pour un domaine._

## <a name="app-linking-overview"></a>Vue d’ensemble de la liaison d’application

Les applications mobiles n’est plus dynamique dans un silo &ndash; dans de nombreux cas ils sont des composants importants de leur entreprise, ainsi que son site Web. Il est préférable pour les entreprises de connecter de façon transparente des applications mobiles et leur présence sur le web avec des liens sur un site Web de lancement d’applications mobiles et affiche le contenu pertinent dans l’application mobile. *La liaison application* (également appelé *lien profond*) est une technique qui permet à un appareil mobile répondre à un URI et de lancer une application mobile qui correspond à cet URI.

Android gère la liaison application via le *système intention* &ndash; lorsque l’utilisateur clique sur un lien dans un navigateur mobile, le navigateur mobile distribue une intention Android délègue à une application enregistrée. Par exemple, en cliquant sur un lien sur un site Web de cuisine serait ouvrir une application mobile qui est associée à ce site Web et afficher une recette spécifique à l’utilisateur. S’il existe plusieurs applications inscrits pour gérer cette intention, puis déclenche Android ce que l'on appelle un *boîte de dialogue ambiguïté* qui demande à un utilisateur quelle application pour sélectionner l’application qui doit gérer l’intention, pour exemple :

![Capture d’écran de l’exemple d’une boîte de dialogue d’ambiguïté](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 améliore cela en utilisant la gestion de la liaison automatique. Il est possible pour Android inscrire automatiquement une application en tant que gestionnaire par défaut pour un URI &ndash; l’application sera automatiquement lancer et accéder directement à l’activité. Android 6.0 décide comment gérer un clic de l’URI dépend des critères suivants :

1. **Une application existante est déjà associée à l’URI** &ndash; l’utilisateur peut-être être déjà associés à une application existante avec un URI. Dans ce cas, Android continue d’utiliser cette application.
2. **Aucune application existante n’est associée à l’URI, mais une application prise en charge est installée** &ndash; dans ce scénario, l’utilisateur n’a pas spécifié une application existante, afin de Android utilisera l’application installée pour gérer la demande.
3. **Aucune application existante n’est associée à l’URI, mais de nombreuses applications de prise en charge sont installées** &ndash; , car il existe plusieurs applications qui prennent en charge de l’URI, la boîte de dialogue ambiguïté s’affichera et l’utilisateur doit sélectionner quelle application gérer l’URI.

Si l’utilisateur ne possède pas les applications installées qui prennent en charge de l’URI et une est installée par la suite, puis Android définit cette application en tant que gestionnaire par défaut pour l’URI après avoir vérifié l’association avec le site Web qui est associé à l’URI.

Ce guide explique comment configurer une application Android 6.0 et comment créer et publier le fichier de liens d’élément multimédia numérique pour prendre en charge la liaison d’application dans Android 6.0.

## <a name="requirements"></a>Configuration requise

Ce guide nécessite Xamarin.Android 6.1 et une application ciblant Android 6.0 (API niveau 23) ou une version ultérieure.

Application de la liaison est possible dans les versions antérieures d’Android à l’aide de la [package NuGet de Rivets](https://www.nuget.org/packages/Rivets/) à partir du magasin de composant Xamarin. Le package de Rivets n’est pas compatible avec la liaison app dans Android 6.0 ; Il ne gère pas la liaison d’application Android 6.0.

## <a name="configuring-app-linking-in-android-60"></a>Configuration de la liaison d’application dans Android 6.0

Configuration des liaisons de l’application dans Android 6.0 comprend deux étapes principales :

1. **Ajout de filtres intention de l’un ou plusieurs pour du site Web URI** &ndash; les filtres intentionnels guident Android dans la gestion d’un clic de l’URL dans un navigateur mobile.
2. **Publication d’un *JSON de liens numériques Asset* fichier sur le site Web** &ndash; il s’agit d’un fichier qui est téléchargé vers un site Web et est utilisé par Android pour vérifier la relation entre l’application mobile et le domaine du site Web. Sans cela, Android ne peut pas installer l’application en tant que le handle de la valeur par défaut pour l’URI ; l’utilisateur doit le faire manuellement.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Configuration du filtre Intent

Il est nécessaire de configurer un filtre d’intention qui mappe un URI (ou possible un jeu d’URI) à partir d’un site Web à l’activité d’une application Android. Dans Xamarin.Android, cette relation est établie par ornementer une activité dont le [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Le filtre intention doit déclarer les informations suivantes :

* **`Intent.ActionView`** &ndash; Enregistre le filtre intention pour répondre aux requêtes pour afficher des informations
* **`Categories`** &ndash;  Le filtre intention doit enregistrer les deux  **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)**  et  **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)**  pour pouvoir correctement gérer l’URI du web.
* **`DataScheme`** &ndash; Le filtre intention doit déclarer `http` et/ou `https`. Ces modèles sont les seuls deux valides.
* **`DataHost`** &ndash; Il s’agit du domaine qui proviennent de l’URI.
* **`DataPathPrefix`** &ndash; Il s’agit d’un chemin d’accès facultatif à des ressources sur le site Web.
* **`AutoVerify`** &ndash; Le `autoVerify` attribut indique Android pour vérifier la relation entre l’application et le site Web. Cette méthode est expliquée plus ci-dessous.

L’exemple suivant montre comment utiliser le [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) afin de gérer les liens à partir de `https://www.recipe-app.com/recipes` et de `http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe"),
              AutoVerify=true]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android vérifie chaque hôte qui est identifié par les filtres intentionnels par rapport au fichier de ressources numérique sur le site Web avant d’inscrire l’application en tant que gestionnaire par défaut pour un URI. Tous les filtres d’intention doivent réussir la vérification avant Android peut établir l’application en tant que gestionnaire par défaut.

### <a name="creating-the-digital-assets-link-file"></a>Création du fichier de liaison de ressources numériques

Android 6.0 application la liaison requiert que Android vérifier l’association entre l’application et le site Web avant de définir l’application en tant que gestionnaire par défaut pour l’URI. Cette vérification se produit lors de la première installation de l’application. Le *des liens actifs numériques* fichier est un fichier JSON qui est hébergé par le webdomain(s) pertinentes.

> [!NOTE]
> Le `android:autoVerify` attribut doit être défini par le filtre intention &ndash; sinon Android n’effectuera pas la vérification.

Le fichier est placé par l’administrateur du domaine à l’emplacement **https://domain/.well-known/assetlinks.json**.

Le fichier d’élément multimédia numérique contient les métadonnées nécessaires pour Android vérifier l’association. Un **assetlinks.json** fichier contient les paires clé-valeur suivantes :

* `namespace` &ndash; l’espace de noms de l’application Android.
* `package_name` &ndash; le nom du package de l’application Android (déclaré dans le manifeste d’application).
* `sha256_cert_fingerprints` &ndash; les empreintes SHA256 de l’application signée. Veuillez consulter le guide [recherche de votre magasin de clés MD5 ou SHA1 Signature](~/android/deploy-test/signing/keystore-signature.md) pour plus d’informations sur la façon d’obtenir l’empreinte SHA1 d’une application.

L’extrait de code suivant est un exemple de **assetlinks.json** avec une seule application répertoriés :

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Il est possible d’inscrire plusieurs empreintes SHA256 pour prendre en charge différentes versions ou versions de votre application. Ce qui suit **assetlinks.json** fichier est un exemple d’inscription de plusieurs applications :

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Le [site Web de Google numérique Asset liens](https://developers.google.com/digital-asset-links/tools/generator) a un outil en ligne qui peut-être aider à créer et tester le fichier de ressources numériques.

### <a name="testing-app-links"></a>Test des liens de l’application

Après l’implémentation des liens de l’application, les différents éléments doivent être testées pour vous assurer qu’elles fonctionnent comme prévu.

Il est possible confirmer que le fichier de ressources numériques est correctement formaté et hébergé à l’aide des API de liens numériques Asset de Google, comme illustré dans cet exemple :

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Il existe deux tests qui peuvent être effectuées pour garantir que les filtres intentionnels ont été configurés correctement et que l’application est définie comme le gestionnaire par défaut pour un URI :

1.  Le fichier d’élément multimédia numérique est correctement hébergé comme décrit ci-dessus. Le premier test distribue une intention qui Android doit vous rediriger vers l’application mobile. L’application Android doit lancer et afficher l’activité enregistrée pour l’URL. Dans une invite de commandes, tapez :

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Afficher le lien existant, la gestion des stratégies pour les applications installées sur un appareil donné. La commande suivante provoque le vidage de liste des stratégies de lien pour chaque utilisateur sur l’appareil avec les informations suivantes. À l'invite de commandes, tapez la commande suivante :

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Le nom du package de l’application.
    * **`Domain`** &ndash; Les domaines (séparés par des espaces) dont les liens web seront gérées par l’application
    * **`Status`** &ndash; Il s’agit de l’état actuel de la gestion des liens de l’application. La valeur **toujours** signifie que l’application a `android:autoVerify=true` déclaré et a passé de vérification du système. Il est suivi d’un nombre hexadécimal représentant l’enregistrement du système Android de la préférence.

    Exemple :

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Récapitulatif

Ce guide décrit comment lier l’application fonctionne Android 6.0. Il couvert puis comment configurer une application Android 6.0 pour prendre en charge et répondre aux liens de l’application. Il décrit également comment tester l’application de liaison dans une application Android.


## <a name="related-links"></a>Liens associés

- [Recherche de votre magasin de clés MD5 ou SHA1 Signature](~/android/deploy-test/signing/keystore-signature.md)
- [Les activités et objectifs](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Liens actifs de Google numérique](https://developers.google.com/digital-asset-links/)
- [Testeur et Générateur de liste d’instruction](https://developers.google.com/digital-asset-links/tools/generator)
