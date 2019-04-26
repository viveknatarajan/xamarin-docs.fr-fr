---
title: Réponse aux rappels d’authentification
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: c720a30a59eea8f1ed74033da8d1c045a1fb9109
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023481"
---
# <a name="responding-to-authentication-callbacks"></a>Réponse aux rappels d’authentification

Le lecteur d’empreintes digitales s’exécute en arrière-plan sur son propre thread, et lorsqu’il est terminé, il signalera les résultats de l’analyse en appelant une méthode de `FingerprintManager.AuthenticationCallback` sur le thread d’interface utilisateur. Une application Android doit fournir son propre gestionnaire qui étend cette classe abstraite, qui implémente toutes les méthodes suivantes :

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Appelée quand il existe une erreur irrécupérable. Il en est rien de plus qu'une application ou un utilisateur peut faire pour corriger la situation sauf éventuellement essayer à nouveau.
* **`OnAuthenticationFailed()`** &ndash; Cette méthode est appelée lorsqu’une empreinte digitale a été détectée mais n’est ne pas reconnue par l’appareil.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Appelée quand il existe une erreur récupérable, telles que le doigt est passée à rapide sur le scanneur.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Il s’agit quand une empreinte digitale a été reconnue.

Si un `CryptoObject` a été utilisé lors de l’appel `Authenticate`, il est recommandé d’appeler `Cipher.DoFinal` dans `OnAuthenticationSuccessful`.
`DoFinal` lève une exception si le chiffrement a été falsifié ou mal initialisé, indiquant que le résultat de lecteur d’empreintes digitales a peut-être été falsifié en dehors de l’application.


> [!NOTE]
> Il est recommandé de conserver le rappel classe relativement légère et gratuits spécifique de logique d’application. Les rappels doivent agir en tant que « cop de trafic » entre l’application Android et les résultats à partir du lecteur d’empreintes digitales.

## <a name="a-sample-authentication-callback-handler"></a>Un exemple de gestionnaire de rappel d’authentification

La classe suivante est un exemple d’un nombre minimal `FingerprintManager.AuthenticationCallback` implémentation : 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` vérifie si un `Cipher` a été fourni au `FingerprintManager` lorsque `Authentication` a été appelée. Dans ce cas, le `DoFinal` méthode est appelée sur le chiffrement. Cette méthode ferme le `Cipher`, restaurant à son état d’origine. Si un problème est survenu avec le chiffrement, puis `DoFinal` lève une exception et la tentative d’authentification doit être considérée comme ayant échoué.

Le `OnAuthenticationError` et `OnAuthenticationHelp` rappels chaque réception d’un entier qui indique quel était le problème. La section suivante explique chaque aide possible ou des codes d’erreur. Les deux rappels servent des objectifs similaires &ndash; pour informer l’application que l’authentification par empreinte digitale a échoué. Quoi ils diffèrent est la gravité. `OnAuthenticationHelp` est une erreur récupérable utilisateur, telles que passant l’empreinte digitale trop rapide ; `OnAuthenticationError` est plus une erreur grave, comme un lecteur d’empreintes digitales endommagé.

Notez que `OnAuthenticationError` sera appelé lorsque l’analyse de l’empreinte digitale est annulé via la `CancellationSignal.Cancel()` message. Le `errMsgId` paramètre a la valeur 5 (`FingerprintState.ErrorCanceled`). Selon les besoins, une implémentation de la `AuthenticationCallbacks` peut traiter cette situation différemment des autres erreurs. 

`OnAuthenticationFailed` est appelé lorsque l’empreinte digitale a été analysé avec succès mais ne correspond pas aux empreintes digitales inscrits auprès de l’appareil. 

## <a name="help-codes-and-error-message-ids"></a>Aide de Codes et les ID de Message d’erreur 

Une liste et une description des codes d’erreur et des codes d’aide peuvent se trouver dans le [documentation du SDK Android](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) pour la classe FingerprintManager. Xamarin.Android représente ces valeurs avec le `Android.Hardware.Fingerprints.FingerprintState` enum :


-   **`AcquiredGood`** &ndash; (valeur 0) L’image d’acquis est bonne.


-   **`AcquiredImagerDirty`** &ndash; (valeur 3) L’image de l’empreinte digitale est trop bruyante en raison de saletés suspectée ou détectée sur le capteur. Par exemple, il est raisonnable de revenir à cette après plusieurs `AcquiredInsufficient` ou détection réelle de saletés sur le capteur (pixels manquants, groupes, etc.). L’utilisateur doit intervenir pour nettoyer le capteur lorsque celui-ci est renvoyé.


-   **`AcquiredInsufficient`** &ndash; (valeur 2) L’image de l’empreinte digitale est trop bruyante traiter en raison d’une condition détectée (par exemple, l’apparence de sec) ou un capteur éventuellement modifié (voir `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (valeur 1) Uniquement une image de l’empreinte digitale partielle a été détectée. Lors de l’inscription, l’utilisateur doit être informé sur ce qui doit se produire pour résoudre ce problème, par exemple, &ldquo;Appuyez fermement sur le capteur.&rdquo;



-   **`AcquiredTooFast`** &ndash; (valeur 5) L’image de l’empreinte digitale est incomplète en raison de mouvement rapide. Tandis que principalement approprié pour les capteurs de tableau linéaire, cela peut également se produire si le doigt a été déplacé pendant l’acquisition de. L’utilisateur doit être invité à déplacer le doigt plus lent (linéaire) ou laissez le doigt sur le capteur plus de temps.




-   **`AcquiredToSlow`** &ndash; (la valeur 4) L’image de l’empreinte digitale était illisible en raison du manque de mouvement. Cela est plus approprié pour les capteurs de tableau linéaire qui nécessitent un mouvement de balayage.



-   **`ErrorCanceled`** &ndash; (valeur 5) L’opération a été annulée, car le capteur d’empreintes digitales n’est pas disponible. Par exemple, cela peut se produire lors de la commutation de l’utilisateur, l’appareil est verrouillé ou une autre opération en attente empêche ou la désactive.



-   **`ErrorHwUnavailable`** &ndash; (valeur 1) Le matériel n’est pas disponible. Réessayez plus tard.




-   **`ErrorLockout`** &ndash; (valeur 7) L’opération a été annulée, car l’API est verrouillé en raison du trop grand nombre de tentatives.




-   **`ErrorNoSpace`** &ndash; (la valeur 4) État d’erreur retourné pour les opérations telles que l’inscription ; l’opération ne peut pas être effectuée, car il n’est pas suffisamment de stockage restant pour terminer l’opération.



-   **`ErrorTimeout`** &ndash; (valeur 3) État d’erreur retourné lors de la requête actuelle a été trop longue. Cela vise à empêcher les programmes d’attendre indéfiniment pour le capteur d’empreintes digitales. Le délai d’expiration est la plateforme et capteur spécifiques, mais est généralement environ 30 secondes.



-   **`ErrorUnableToProcess`** &ndash; (valeur 2) État d’erreur renvoyé lorsque le capteur n’a pas pu traiter l’image actuelle.



## <a name="related-links"></a>Liens associés

- [Cipher](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
