---
title: Analyse des empreintes digitales
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
ms.openlocfilehash: 372fe4c7844448e7fb3cbc768f16feb3a5cc7791
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671440"
---
# <a name="scanning-for-fingerprints"></a>Analyse des empreintes digitales

Maintenant que nous avons vu comment préparer une application Xamarin.Android à utiliser l’authentification par empreinte digitale, nous allons revenir à la `FingerprintManager.Authenticate` (méthode) et discutez de la place l’authentification par empreinte digitale Android 6.0. Une vue d’ensemble rapide du flux de travail pour l’authentification par empreinte digitale est décrite dans cette liste :

1. Appeler `FingerprintManager.Authenticate`, en passant un `CryptoObject` et un `FingerprintManager.AuthenticationCallback` instance. Le `CryptoObject` permet de s’assurer que le résultat de l’authentification par empreinte digitale n’était pas falsifié. 
2. Sous-classe la [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) classe. Une instance de cette classe est fournie à `FingerprintManager` lorsque l’empreinte digitale authentification démarre. Lorsque le lecteur d’empreintes digitales est terminé, il appelle une des méthodes de rappel sur cette classe.
3. Écrire du code pour mettre à jour de l’interface utilisateur pour informer l’utilisateur que l’appareil a démarré le lecteur d’empreintes digitales et est en attente pour l’interaction utilisateur. 
4. Lorsque le lecteur d’empreintes digitales est terminé, Android renvoie des résultats à l’application en appelant une méthode sur le `FingerprintManager.AuthenticationCallback` instance qui a été fourni à l’étape précédente.
5. L’application informe l’utilisateur des résultats de l’authentification par empreinte digitale et réagir aux résultats comme il convient. 

L’extrait de code suivant est un exemple d’une méthode dans une activité qui démarre l’analyse des empreintes digitales :

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Examinons chacun de ces paramètres dans le `Authenticate` méthode un peu plus en détail :

* Le premier paramètre est un _crypto_ que le lecteur d’empreintes digitales utilisera pour aider à authentifier les résultats d’une analyse d’empreintes digitales de l’objet. Cet objet peut être `null`, auquel cas l’application doit aveuglément confiance que rien n’a altéré les résultats de l’empreinte digitale. Il est recommandé qu’un `CryptoObject` être instancié et fourni à la `FingerprintManager` plutôt que null. [Création d’un CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) explique en détail comment instancier un `CryptoObject` selon un `Cipher`.
* Le deuxième paramètre est toujours zéro. La documentation Android Cela identifie en tant que jeu d’indicateurs et est très probablement réservée pour une utilisation ultérieure. 
* Le troisième paramètre, `cancellationSignal` est un objet utilisé pour désactiver le lecteur d’empreintes digitales et annuler la requête actuelle. Il s’agit d’un [CancellationSignal Android](https://developer.android.com/reference/android/os/CancellationSignal.html)et non un type à partir de .NET framework.
* Le quatrième paramètre est obligatoire et est une classe qui sous-classe la `AuthenticationCallback` classe abstraite. Méthodes sur cette classe seront appelées pour signaler aux clients lorsque le `FingerprintManager` est terminé et quels sont les résultats. Comme il y a beaucoup à comprendre concernant l’implémentation de la `AuthenticationCallback`, il sera abordée dans [il est propre section](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Le cinquième paramètre est facultatif `Handler` instance. Si un `Handler` objet est fourni, le `FingerprintManager` utilisera le `Looper` à partir de cet objet lors du traitement des messages à partir du matériel de l’empreinte digitale. En règle générale, une dispense de fournir un `Handler`, le FingerprintManager utilisera le `Looper` à partir de l’application.

## <a name="cancelling-a-fingerprint-scan"></a>L’annulation d’une analyse de l’empreinte digitale

Il peut être nécessaire pour l’utilisateur (ou l’application) annuler l’analyse de l’empreinte digitale de fois qu’elle a été lancée. Dans ce cas, vous devez appeler la [ `IsCancelled` ](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) méthode sur le [ `CancellationSignal` ](https://developer.android.com/reference/android/os/CancellationSignal.html) qui a été fourni au `FingerprintManager.Authenticate` quand il a été appelé pour démarrer l’analyse des empreintes digitales.

Maintenant que nous avons vu le `Authenticate` (méthode), nous allons examiner certains des paramètres plus en détail plus importants. Tout d’abord, nous allons étudier [répond aux rappels d’authentification](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), qui explique comment créer une sous-classe la [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), permettant à une application Android réagir à la résultats fournis par le lecteur d’empreintes digitales.




## <a name="related-links"></a>Liens associés

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
