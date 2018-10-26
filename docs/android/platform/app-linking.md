---
title: La liaison application dans Android
description: Ce guide explique comment Android 6.0 prend en charge la liaison application, une technique qui permet aux applications mobiles répondre aux URL sur les sites Web. Il étudie la liaison quelle application est l’implémentation de la liaison application dans une application Android 6.0 et comment configurer un site Web pour accorder des autorisations à l’application mobile pour un domaine.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: dd4ba236df8e5993c7f7ed86393eb66ce01db595
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111263"
---
# <a name="app-linking-in-android"></a>La liaison application dans Android

_Ce guide explique comment Android 6.0 prend en charge la liaison application, une technique qui permet aux applications mobiles répondre aux URL sur les sites Web. Il étudie la liaison quelle application est l’implémentation de la liaison application dans une application Android 6.0 et comment configurer un site Web pour accorder des autorisations à l’application mobile pour un domaine._

## <a name="app-linking-overview"></a>Vue d’ensemble de la liaison d’application

Les applications mobiles n’est plus résident dans un silo &ndash; dans de nombreux cas, ils sont des composants importants de leurs entreprises, ainsi que leur site Web. Il est souhaitable pour les entreprises de connecter en toute transparence leur présence sur le web et les applications mobiles, avec des liens sur un site Web de lancement d’applications mobiles et affiche le contenu pertinent dans l’application mobile. *La liaison application* (également appelé *lien profond*) est une technique qui permet à un appareil mobile répondre à un URI et de lancer une application mobile qui correspond à cet URI.

Android gère la liaison application via le *système intent* &ndash; lorsque l’utilisateur clique sur un lien dans un navigateur mobile, le navigateur mobile envoie une intention Android va déléguer à une application inscrite. Par exemple, en cliquant sur un lien sur un site Web cuisine voulez-vous ouvrir une application mobile qui est associée à ce site Web et afficher une Recipe (Recette) spécifique à l’utilisateur. S’il existe plusieurs applications inscrit pour gérer cette intention, puis Android déclenche ce que l'on appelle un *boîte de dialogue de levée d’ambiguïté* qui demande à un utilisateur quelle application pour sélectionner l’application qui doit traiter l’intention, pour exemple :

![Capture d’écran de l’exemple d’une boîte de dialogue de levée d’ambiguïté](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 améliore cela en utilisant la gestion de la liaison automatique. Il est possible pour Android enregistrer automatiquement une application en tant que le gestionnaire par défaut pour un URI &ndash; l’application lance automatiquement et accéder directement à l’activité pertinentes. Comment Android 6.0 décide de gérer un clic de l’URI varie selon les critères suivants :

1. **Une application existante est déjà associée à l’URI** &ndash; l’utilisateur peut-être être déjà associés à une application existante avec un URI. Dans ce cas, Android continueront à utiliser cette application.
2. **Aucune application existante n’est associée à l’URI, mais une application prise en charge est installée** &ndash; dans ce scénario, l’utilisateur n’a pas spécifié une application existante, afin d’Android utilisera l’application de prise en charge installée pour gérer la demande.
3. **Aucune application existante n’est associée à l’URI, mais de nombreuses applications de prise en charge sont installées** &ndash; , car il existe plusieurs applications qui prennent en charge de l’URI, la boîte de dialogue d’élargissement de la recherche s’affichera et l’utilisateur doit sélectionner quelle application sera gérer l’URI.

Si l’utilisateur ne dispose d’aucune applications installées qui prennent en charge de l’URI et une est installée par la suite, puis Android définira cette application en tant que le gestionnaire par défaut pour l’URI après avoir vérifié l’association avec le site Web qui est associé à l’URI.

Ce guide explique comment configurer une application Android 6.0 et comment créer et publier le fichier de liaisons d’actifs numériques pour prendre en charge la liaison application dans Android 6.0.

## <a name="requirements"></a>Configuration requise

Ce guide nécessite Xamarin.Android 6.1 et une application qui cible Android 6.0 (niveau d’API 23) ou une version ultérieure.

Application de la liaison est possible dans les versions antérieures d’Android à l’aide de la [package NuGet Rivets](https://www.nuget.org/packages/Rivets/) à partir du magasin du composant Xamarin. Le package Rivets n’est pas compatible avec la liaison par application dans Android 6.0 ; Il ne gère pas la liaison d’application Android 6.0.

## <a name="configuring-app-linking-in-android-60"></a>Configuration de la liaison d’application dans Android 6.0

Configuration des liens d’application dans Android 6.0 comprend deux étapes principales :

1. **Ajout de filtres intention un ou plusieurs pour du site Web URI** &ndash; les filtres intentionnels guident Android dans la façon de gérer un clic de l’URL dans un navigateur mobile.
2. **Publication d’un *JSON de liens actifs numériques* fichier sur le site Web** &ndash; il s’agit d’un fichier qui est téléchargé sur un site Web et est utilisé par Android pour vérifier la relation entre l’application mobile et le domaine du site Web. Sans cela, Android ne peut pas installer l’application en tant que le handle de la valeur par défaut pour les URI ; l’utilisateur doit le faire manuellement.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Configuration du filtre Intent

Il est nécessaire de configurer un filtre d’intention qui mappe un URI (ou possible un jeu d’URI) à partir d’un site Web à une activité dans une application Android. Dans Xamarin.Android, cette relation est établie par orner une activité dont le [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Le filtre intent doit déclarer les informations suivantes :

* **`Intent.ActionView`** &ndash; Cette opération inscrira le filtre intent pour répondre aux requêtes pour afficher des informations
* **`Categories`** &ndash;  Le filtre intent doit s’inscrire à la fois **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** et **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** pour pouvoir correctement gérer l’URI du web.
* **`DataScheme`** &ndash; Le filtre intent doit déclarer `http` et/ou `https`. Ces modèles sont les deux seuls valides.
* **`DataHost`** &ndash; Ceci est le domaine qui sont issus de l’URI.
* **`DataPathPrefix`** &ndash; Il s’agit d’un chemin d’accès facultatif à des ressources sur le site Web.
* **`AutoVerify`** &ndash; Le `autoVerify` attribut indique à Android pour vérifier la relation entre l’application et le site Web. Cette méthode est expliquée plus ci-dessous.

L’exemple suivant montre comment utiliser le [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) pour gérer les liens à partir de `https://www.recipe-app.com/recipes` et à partir de `http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe",
              AutoVerify=true)]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android vérifie chaque hôte qui est identifié par les filtres intentionnels par rapport au fichier de ressources numériques sur le site Web avant d’inscrire l’application en tant que le gestionnaire par défaut pour un URI. Tous les filtres intentionnels doivent passer la vérification que Android puisse établir l’application en tant que le gestionnaire par défaut.

### <a name="creating-the-digital-assets-link-file"></a>Création du fichier de liaison actifs numériques

Android 6.0 application la liaison requiert que Android vérifier l’association entre l’application et le site Web avant de définir l’application en tant que le gestionnaire par défaut pour l’URI. Cette vérification se produit lors de la première installation de l’application. Le *des liens actifs numériques* fichier est un fichier JSON qui est hébergé par le webdomain(s) pertinentes.

> [!NOTE]
> Le `android:autoVerify` attribut doit être défini par le filtre intent &ndash; sinon Android n’effectuera pas la vérification.

Le fichier est placé par l’administrateur du domaine à l’emplacement **https://domain/.well-known/assetlinks.json**.

Le fichier de ressource numérique contient les métadonnées nécessaires pour Android vérifier l’association. Un **assetlinks.json** fichier contient les paires clé-valeur suivantes :

* `namespace` &ndash; l’espace de noms de l’application Android.
* `package_name` &ndash; le nom du package de l’application Android (déclaré dans le manifeste d’application).
* `sha256_cert_fingerprints` &ndash; les empreintes SHA256 de l’application signée. Veuillez consulter le guide [recherche de votre magasin de clés Signature MD5 ou SHA1](~/android/deploy-test/signing/keystore-signature.md) pour plus d’informations sur la façon d’obtenir l’empreinte SHA1 d’une application.

L’extrait de code suivant est un exemple de **assetlinks.json** avec une seule application répertorié :

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

Il est possible d’inscrire plusieurs empreintes SHA256 pour prendre en charge différentes versions ou les builds de votre application. Ce qui suit **assetlinks.json** fichier est un exemple d’inscription de plusieurs applications :

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

Le [site Web de Google Digital Asset liens](https://developers.google.com/digital-asset-links/tools/generator) a un outil en ligne qui peut-être aider à créer et tester le fichier de ressources numériques.

### <a name="testing-app-links"></a>Test des liens de l’application

Après l’implémentation des liens de l’application, les différents éléments doivent être testés pour vous assurer qu’elles fonctionnent comme prévu.

Il est possible de confirmer que le fichier de ressources numériques est correctement formaté et hébergé à l’aide Digital Asset liens API de Google, comme illustré dans cet exemple :

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Il existe deux tests qui peuvent être effectuées pour vous assurer que les filtres intentionnels ont été configurés correctement et que l’application est définie comme le gestionnaire par défaut pour un URI :

1.  Le fichier de ressource numérique est hébergé correctement comme décrit ci-dessus. Le premier test distribue une intention qui Android doit vous rediriger vers l’application mobile. L’application Android doit lancer et afficher l’activité enregistrée pour l’URL. À une invite de commandes, tapez :

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Afficher le lien existant, gestion des stratégies pour les applications installées sur un appareil donné. La commande suivante provoque le vidage de liste des stratégies de liaison pour chaque utilisateur sur l’appareil avec les informations suivantes. À l'invite de commandes, tapez la commande suivante :

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Le nom du package de l’application.
    * **`Domain`** &ndash; Les domaines (séparés par des espaces) dont les liens web seront gérées par l’application
    * **`Status`** &ndash; Il s’agit de l’état actuel de la gestion des liens de l’application. La valeur **toujours** signifie que l’application a `android:autoVerify=true` déclaré et a réussi la vérification de système. Il est suivi par un nombre hexadécimal représentant l’Android enregistrement du système de la préférence.

    Exemple :

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Récapitulatif

Ce guide décrit comment lier l’application de works dans Android 6.0. Ensuite, elle couvrait comment configurer une application Android 6.0 pour prendre en charge et répondre aux liens de l’application. Il présente également comment tester la liaison application dans une application Android.


## <a name="related-links"></a>Liens associés

- [Recherche de votre magasin de clés Signature MD5 ou SHA1](~/android/deploy-test/signing/keystore-signature.md)
- [Activités et les intentions](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Liens de ressources numériques de Google](https://developers.google.com/digital-asset-links/)
- [Testeur et Générateur de listes d’instruction](https://developers.google.com/digital-asset-links/tools/generator)
