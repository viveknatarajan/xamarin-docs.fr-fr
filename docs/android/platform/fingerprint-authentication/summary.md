---
title: "Guide de l’authentification par empreinte digitale"
ms.topic: article
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20defea58ec0c09becd5a3cec1961806cb0acc1d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="fingerprint-authentication-guidance"></a>Guide de l’authentification par empreinte digitale

## <a name="fingerprint-authentication-guidance"></a>Guide de l’authentification par empreinte digitale

Maintenant que nous avons vu les concepts et API entourant Android 6.0 d’empreintes digitales d’authentification, examinons certains conseils généraux de l’utilisation des API d’empreintes digitales.

1. **Utiliser les API de compatibilité de bibliothèque de prise en charge Android v4** &ndash; vous simplifier le code d’application en supprimant la vérification de l’API à partir du code et une application pour cibler le plus possible de périphériques.
2. **Fournir des Alternatives à l’authentification par empreinte digitale** &ndash; l’authentification par empreinte digitale est un excellent moyen rapide d’une application authentifier un utilisateur, toutefois, il ne peut pas être supposé qu’il sera toujours fonctionner ou être disponibles. Il est possible que le lecteur d’empreintes digitales risquent d’échouer, l’objectif est peut-être être modifié, l’utilisateur ne peut pas ont configuré le périphérique pour utiliser l’authentification par empreinte digitale ou depuis les empreintes ont disparu. Il est également possible que l’utilisateur ne souhaite pas utiliser l’authentification par empreinte digitale avec votre application. Pour ces raisons, une application Android doit fournir un processus d’authentification autre telles que le nom d’utilisateur et mot de passe.
3. **Icône d’empreinte digitale de Google** &ndash; toutes les applications doivent utiliser la même icône empreinte fournie par Google. L’utilisation d’une icône standard facilite le travail des utilisateurs Android à reconnaître dans lequel l’authentification par empreinte digitale est utilisée dans les applications : 
    
    ![Icône d’empreintes digitales Android](summary-images/ic-fp-40px.png)
    
4. **Notifier l’utilisateur** &ndash; une application doit afficher un type de notification à l’utilisateur que le lecteur d’empreintes digitales est actif et en attente d’un tactile ou un balayage. 

## <a name="summary"></a>Récapitulatif

L’authentification par empreinte digitale est un excellent moyen de permettre à une application Xamarin.Android vérifier rapidement les utilisateurs, ce qui facilite les utilisateurs à interagir avec des fonctionnalités sensibles, telles que les achats dans l’application. Ce guide décrit les concepts et le code requis pour incorporer l’empreinte Android 6.0 de l’API dans votre application Xamarin.Android.

Tout d’abord, nous l’avons vu l’empreinte de l’API eux-mêmes, `FingerprintManager` (et `FingerprintManagerCompat`). Nous avons examiné comment la `FingerprintManager.AuthenticationCallbacks` classe abstraite doit être étendue par une application et utilisée comme intermédiaire entre le matériel d’empreintes digitales et l’application elle-même. Ensuite, nous avons examiné comment vérifier l’intégrité des résultats d’analyseur par empreinte digitale à l’aide de Java `Cipher` objet. Enfin, nous fonctionnaient sur les tests en décrivant comment inscrire une empreinte digitale sur un appareil et en utilisant **adb** pour simuler un balayage de l’empreinte digitale sur un émulateur. 

Si vous n’avez pas déjà fait, vous devez examiner le [exemple d’application](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) qui accompagne le présent guide. Le [exemple de boîte de dialogue d’empreintes digitales](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) a été porté à partir de Java pour Xamarin.Android et fournit un autre exemple de l’ajout de l’authentification par empreinte digitale à une application Android.



## <a name="related-links"></a>Liens associés

- [Exemple d’application de Guide par empreinte digitale](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Exemple de boîte de dialogue d’empreinte digitale](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Icône d’empreinte digitale](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
