---
title: Authentification par empreinte digitale
description: Ce guide explique comment ajouter une authentification par empreinte digitale, introduite dans Android 6.0, à une application Xamarin.Android.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1b28b16dfd92ef3a31201ef2e86681a425a58ab8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="fingerprint-authentication"></a>Authentification par empreinte digitale

_Ce guide explique comment ajouter une authentification par empreinte digitale, introduite dans Android 6.0, à une application Xamarin.Android._


## <a name="fingerprint-authentication-overview"></a>Vue d’ensemble de l’authentification par empreinte digitale

L’arrivée d’empreintes digitales, sur les appareils Android fournit aux applications une alternative à la méthode traditionnelle de nom d’utilisateur/mot de passe de l’authentification utilisateur. L’utilisation d’empreintes digitales pour authentifier un utilisateur permet à une application d’incorporer la sécurité est moins importun que d’un nom d’utilisateur et un mot de passe.

Les APIs FingerprintManager cibler des appareils avec un lecteur d’empreintes digitales et API de niveau 23 (Android 6.0) sont en cours d’exécution ou une version ultérieure. Les API sont trouvent dans le `Android.Hardware.Fingerprints` espace de noms. La bibliothèque de prise en charge Android v4 fournit des versions de l’empreinte digitale API destiné à des versions antérieures d’Android. La compatibilité d’API se trouvent dans le `Android.Support.v4.Hardware.Fingerprint` espace de noms sont distribués via le [package NuGet de Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

Le [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (et son équivalent de la bibliothèque de prise en charge, [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) est la classe principale pour l’utilisation de l’empreinte de l’analyse matérielle. Cette classe est un wrapper de SDK Android autour du service de niveau système qui gère les interactions avec le matériel lui-même. Il est chargé pour lancer le lecteur d’empreintes digitales et répondre aux commentaires de l’analyseur. Cette classe possède une interface assez simple avec seulement trois membres :

* **`Authenticate`** &ndash; Cette méthode initialiser le scanneur de matériel et démarrer le service en arrière-plan, en attente de l’utilisateur à analyser leurs empreintes digitales.
* **`EnrolledFingerprints`** &ndash; Cette propriété retourne `true` si l’utilisateur a inscrit un ou plusieurs des empreintes avec l’appareil.
* **`HardwareDetected`** &ndash; Cette propriété est utilisée pour déterminer si le périphérique prend en charge l’analyse d’empreintes digitales.

Le `FingerprintManager.Authenticate` méthode est utilisée par une application Android pour démarrer le lecteur d’empreintes digitales. L’extrait de code suivant est un exemple de l’appel à l’aide de la compatibilité de la bibliothèque de prise en charge des API :

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

Ce guide explique comment utiliser le `FingerprintManager` API pour améliorer une application Android d’empreintes digitales. Il explique comment instancier et créer un `CryptoObject` pour aider à sécuriser les résultats à partir du lecteur d’empreintes digitales. Nous allons examiner la façon dont une application doit sous-classe `FingerprintManager.AuthenticationCallback` et répondre aux commentaires dans le lecteur d’empreintes digitales. Enfin, nous verrons comment inscrire une empreinte digitale sur un émulateur ou un appareil Android et comment utiliser **adb** pour simuler une analyse d’empreintes digitales.

## <a name="requirements"></a>Spécifications

L’authentification par empreinte digitale nécessite Android 6.0 (API niveau 23) ou supérieur et un appareil avec un lecteur d’empreintes digitales. 

Une empreinte doit être déjà inscrite avec l’appareil pour chaque utilisateur qui doit être authentifié. Cela implique la configuration d’un verrouillage d’écran qui utilise un mot de passe, le code confidentiel, modèle de balayage ou la reconnaissance des visages. Il est possible de simuler certaines des fonctionnalités de l’authentification par empreinte digitale dans un émulateur Android.  Pour plus d’informations sur ces deux sujets, consultez le [l’inscription d’une empreinte digitale](enrolling-fingerprint.md) section. 






## <a name="related-links"></a>Liens associés

- [Exemple d’application de Guide par empreinte digitale](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [Exemple de boîte de dialogue d’empreinte digitale](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Demande d’autorisations lors de l’exécution](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [API d’empreintes digitales et les paiements (vidéo)](https://youtu.be/VOn7VrTRlA4)
