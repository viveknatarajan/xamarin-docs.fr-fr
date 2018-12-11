---
title: Guide de l’authentification par empreinte digitale
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 535eabe07cb4f4d36e6a6f918b5717efcc99185d
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898824"
---
# <a name="fingerprint-authentication-guidance"></a>Guide de l’authentification par empreinte digitale

## <a name="fingerprint-authentication-guidance"></a>Guide de l’authentification par empreinte digitale

Maintenant que nous avons vu les concepts et l’authentification de l’empreinte digitale API entourant Android 6.0, nous allons aborder certains conseils généraux pour l’utilisation des API d’empreintes digitales.

1. **Utiliser les API de compatibilité de bibliothèque de prise en charge Android v4** &ndash; vous simplifier le code d’application en supprimant la vérification de l’API à partir du code et une application pour cibler la plus probable de périphériques.
2. **Fournir des Alternatives à l’authentification par empreinte digitale** &ndash; authentification par empreinte digitale est un excellent moyen rapide pour une application authentifier un utilisateur, toutefois, il ne peut pas être supposé qu’il sera toujours fonctionner ou être disponible. Il est possible que le lecteur d’empreintes digitales peut échouer, le filtre peut-être être modifié, l’utilisateur ne peut pas avoir configuré l’appareil pour utiliser l’authentification par empreinte digitale ou depuis les empreintes ont disparu. Il est également possible que l’utilisateur ne pouvez pas utiliser l’authentification par empreinte digitale avec votre application. Pour ces raisons, une application Android doit fournir un processus d’authentification autre telles que le nom d’utilisateur et mot de passe.
3. **Utilisez l’icône d’empreinte digitale de Google** &ndash; toutes les applications doivent utiliser la même icône empreinte fournie par Google. L’utilisation d’une icône standard facilite pour les utilisateurs à reconnaître dans lequel l’authentification par empreinte digitale est utilisée dans les applications Android : 
    
    ![Icône de l’empreinte digitale Android](summary-images/ic-fp-40px.png)
    
4. **Notifier l’utilisateur** &ndash; une application doit afficher une sorte de notification à l’utilisateur que le lecteur d’empreintes digitales est actif et en attente de lite touch ou un balayage. 

## <a name="summary"></a>Récapitulatif

Authentification par empreinte digitale est un excellent moyen pour permettre à une application Xamarin.Android vérifier rapidement les utilisateurs, ce qui facilite pour les utilisateurs à interagir avec des fonctionnalités sensibles, telles que les achats dans l’application. Ce guide a abordé les concepts et le code requis pour incorporer l’empreinte digitale de Android 6.0 de l’API dans votre application Xamarin.Android.

Tout d’abord, nous avons abordé l’empreinte digitale de l’API proprement dites, `FingerprintManager` (et `FingerprintManagerCompat`). Nous avons examiné comment la `FingerprintManager.AuthenticationCallbacks` classe abstraite doit être étendue par une application et utilisée comme intermédiaire entre le matériel de l’empreinte digitale et de l’application elle-même. Puis nous avons examiné comment vérifier l’intégrité des résultats de scanneur d’empreinte digitale à l’aide de Java `Cipher` objet. Enfin, dont nous avons parlé un peu sur les tests en décrivant comment inscrire une empreinte digitale sur un appareil et en utilisant **adb** pour simuler un passage par empreinte digitale sur un émulateur. 

Si vous n’avez pas déjà fait, vous devez examiner le [exemple d’application](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) qui accompagne ce guide. Le [exemple de boîte de dialogue d’empreintes digitales](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) a été porté à partir de Java vers Xamarin.Android et fournit un autre exemple sur la façon d’ajouter l’authentification par empreinte digitale à une application Android.



## <a name="related-links"></a>Liens associés

- [Empreinte digitale Guide exemple d’application](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Exemple de boîte de dialogue d’empreinte digitale](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Icône de l’empreinte digitale](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
