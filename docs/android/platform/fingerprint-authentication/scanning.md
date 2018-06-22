---
title: L’analyse des empreintes digitales
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 2b20402fd2cd6782247fb2c62513d7aff43a95a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774159"
---
# <a name="scanning-for-fingerprints"></a>L’analyse des empreintes digitales

Maintenant que nous avons vu comment préparer une application Xamarin.Android pour utiliser l’authentification par empreinte digitale, revenons à la `FingerprintManager.Authenticate` (méthode) et de discuter de son emplacement dans l’authentification par empreinte digitale Android 6.0. Une vue d’ensemble rapide du flux de travail pour l’authentification par empreinte digitale est décrite dans cette liste :

1. Appeler `FingerprintManager.Authenticate`, en passant un `CryptoObject` et un `FingerprintManager.AuthenticationCallback` instance. Le `CryptoObject` est utilisé pour garantir que le résultat de l’authentification par empreinte digitale a été pas falsifié. 
2. Sous-classe le [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) classe. Une instance de cette classe est fournie à `FingerprintManager` lorsque empreinte digitale authentification démarre. Lorsque le lecteur d’empreintes digitales est terminée, elle appelle une des méthodes de rappel sur cette classe.
3. Écrire du code pour mettre à jour l’interface utilisateur pour informer l’utilisateur que le périphérique a démarré le lecteur d’empreintes digitales et est en attente d’intervention de l’utilisateur. 
4. Une fois terminé, le lecteur d’empreintes digitales Android renvoie des résultats à l’application en appelant une méthode sur le `FingerprintManager.AuthenticationCallback` instance qui a été fourni à l’étape précédente.
5. L’application informer l’utilisateur de résultats de l’authentification par empreinte digitale et réagir aux résultats comme il convient. 

L’extrait de code suivant est un exemple d’une méthode dans une activité qui démarre l’analyse pour détecter les empreintes :

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Examinons chacune de ces paramètres dans le `Authenticate` méthode un peu plus en détail :

* Le premier paramètre est un _crypto_ de l’objet que le lecteur d’empreintes digitales utilisera pour aider à authentifier les résultats d’une analyse d’empreintes digitales. Cet objet peut être `null`, auquel cas l’application doit approuvent que rien n’a falsifié les résultats d’empreintes digitales. Il est recommandé qu’un `CryptoObject` être instancié et fourni à la `FingerprintManager` au lieu de null. [Création d’un CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) explique en détail comment instancier un `CryptoObject` basé sur un `Cipher`.
* Le deuxième paramètre est toujours zéro. La documentation d’Android l’identifie en tant que jeu d’indicateurs et est très probablement réservée à un usage ultérieur. 
* Le troisième paramètre, `cancellationSignal` est un objet utilisé pour activer le lecteur d’empreintes digitales et annuler la demande en cours. Il s’agit d’un [CancellationSignal Android](http://developer.android.com/reference/android/os/CancellationSignal.html)et non un type à partir de .NET framework.
* Le quatrième paramètre est obligatoire et est une classe qui sous-classe le `AuthenticationCallback` classe abstraite. Méthodes de cette classe seront appelés pour signaler aux clients lorsque le `FingerprintManager` terminée et quels sont les résultats. Comme il y a beaucoup de comprendre sur l’implémentation de la `AuthenticationCallback`, il ne sera couvert dans [il est propre section](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Le cinquième paramètre est facultatif `Handler` instance. Si un `Handler` objet est fourni, le `FingerprintManager` utilisera le `Looper` à partir de cet objet lors du traitement des messages à partir du matériel d’empreintes digitales. En règle générale, il n’est pas nécessaire fournir un `Handler`, le FingerprintManager utilisera le `Looper` à partir de l’application.

## <a name="cancelling-a-fingerprint-scan"></a>L’annulation d’une analyse d’empreinte digitale

Il peut être nécessaire pour l’utilisateur (ou l’application) annuler l’analyse d’empreintes digitales après que qu’il a été lancé. Dans ce cas, vous devez appeler la [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) méthode sur le [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) qui a été fournie au `FingerprintManager.Authenticate` quand il a été appelé pour démarrer l’analyse d’empreintes digitales.

Maintenant que nous avons vu le `Authenticate` (méthode), nous allons examiner certains des paramètres plus en détail plus importants. Tout d’abord, nous allons nous intéresser à [répond aux rappels de l’authentification](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), qui traite des comment créer une sous-classe du [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), permettant à une application Android réagir à la résultats fournis par le lecteur d’empreintes digitales.




## <a name="related-links"></a>Liens associés

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
