---
title: "Répond aux rappels de l’authentification"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 37d288cd75f232c8674aece085a78a83ce12d720
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
---
# <a name="responding-to-authentication-callbacks"></a>Répond aux rappels de l’authentification

Le lecteur d’empreintes digitales s’exécute en arrière-plan sur son propre thread, et lorsqu’elle est terminée, il va signaler les résultats de l’analyse en appelant une méthode de `FingerprintManager.AuthenticationCallback` sur le thread d’interface utilisateur. Une application Android doit fournir son propre gestionnaire qui étend cette classe abstraite, en implémentant les méthodes suivantes :

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Appelée quand il existe une erreur irrécupérable. Il est qu'une application ou un utilisateur peut faire pour corriger la situation, sauf éventuellement puis réessayez.
* **`OnAuthenticationFailed()`** &ndash; Cette méthode est appelée lorsqu’une empreinte digitale a été détectée mais ne pas reconnue par le périphérique.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Appelée quand il existe une erreur récupérable, telles que le doigt qui est passée à rapide sur le moteur d’analyse.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Appelé lorsqu’une empreinte digitale a été reconnue.

Si un `CryptoObject` a été utilisée lors de l’appel `Authenticate`, il est recommandé d’appeler `Cipher.DoFinal` dans `OnAuthenticationSuccessful`.
`DoFinal` lève une exception si le chiffrement a été falsifié ou mal initialisé, indiquant que le résultat de l’analyseur d’empreintes digitales peut avoir été falsifié en dehors de l’application.


> [!NOTE]
> Il est recommandé pour conserver le poids relativement faible de classe de rappel et de libérer la logique d’application spécifique. Les rappels doivent agir en tant que « cop de trafic « entre l’application Android et les résultats à partir du lecteur d’empreintes digitales.

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

`OnAuthenticationSucceeded` vérifie si un `Cipher` a été fournie au `FingerprintManager` lorsque `Authentication` a été appelé. Dans ce cas, le `DoFinal` méthode est appelée sur le chiffrement. Cela ferme le `Cipher`, restaurant à son état d’origine. Si un problème est survenu avec le chiffrement, puis `DoFinal` lève une exception et la tentative d’authentification doit être considérée comme ayant échoué.

Le `OnAuthenticationError` et `OnAuthenticationHelp` rappels chaque réception d’un entier qui indique que le problème a été. La section suivante décrit chacune de l’aide possibles ou les codes d’erreur. Les deux rappels finalités similaires &ndash; pour informer l’application que l’authentification par empreinte digitale a échoué. Est de quoi ils diffèrent par gravité. `OnAuthenticationHelp` est une erreur récupérable utilisateur, telles que le glissement de l’empreinte digitale trop rapide ; `OnAuthenticationError` est plus une erreur grave, par exemple un scanneur d’empreintes digitales endommagé.

Notez que `OnAuthenticationError` sera appelée lors de l’analyse de l’empreinte digitale est annulée le `CancellationSignal.Cancel()` message. Le `errMsgId` paramètre a la valeur 5 (`FingerprintState.ErrorCanceled`). Selon les besoins, une implémentation de la `AuthenticationCallbacks` peut traiter cette situation différemment des autres erreurs. 

`OnAuthenticationFailed` est appelé lorsque l’empreinte digitale a été analysé avec succès mais ne correspond pas à toutes les empreintes digitales inscrits auprès de l’appareil. 

## <a name="help-codes-and-error-message-ids"></a>Codes d’aide et l’ID de Message d’erreur 

Une liste et une description des codes d’erreur et codes d’aide peuvent être trouvées dans le [documentation du SDK Android](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) pour la classe FingerprintManager. Xamarin.Android représente ces valeurs avec la `Android.Hardware.Fingerprints.FingerprintState` enum :


-   **`AcquiredGood`** &ndash; (valeur 0) L’image d’acquis est bonne.


-   **`AcquiredImagerDirty`** &ndash; (valeur 3) L’image de l’empreinte digitale est trop bruyante en raison de moto suspectée ou détecté sur le capteur. Par exemple, il est raisonnable de retourner cet après plusieurs `AcquiredInsufficient` ou détection réelle de dépôts sur le capteur (pixels manquants, quantités, etc.). L’utilisateur est censé prendre des mesures pour nettoyer le capteur lorsque cette valeur est retournée.


-   **`AcquiredInsufficient`** &ndash; (valeur 2) L’image de l’empreinte digitale est trop bruyante traiter en raison d’une condition détectée (par exemple, l’apparence de sec) ou un capteur éventuellement modifié (voir `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (valeur 1) Seule une image d’empreinte digitale partielle a été détectée. Lors de l’inscription, l’utilisateur doit être informé sur ce qui doit se produire pour résoudre ce problème, par exemple, &ldquo;appuyez sur bien de capteur.&rdquo;



-   **`AcquiredTooFast`** &ndash; (valeur 5) L’image de l’empreinte digitale est incomplet en raison du mouvement rapide. Lorsque principalement approprié pour les capteurs de tableau linéaire, ceci peut aussi arriver si le doigt a été déplacé au cours d’acquisition. L’utilisateur doit être posée pour déplacer le doigt plus lent (linéaire) ou laissez le doigt du capteur de plus de temps.




-   **`AcquiredToSlow`** &ndash; (valeur 4) L’image de l’empreinte digitale est illisible en raison d’un manque de mouvement. Cela est plus approprié pour les capteurs de tableau linéaire qui nécessitent un mouvement de balayage.



-   **`ErrorCanceled`** &ndash; (valeur 5) L’opération a été annulée, car le capteur d’empreintes digitales n’est pas disponible. Par exemple, cela peut se produire lorsque l’utilisateur est basculé, l’appareil est verrouillé ou une autre opération en attente empêche ou la désactive.



-   **`ErrorHwUnavailable`** &ndash; (valeur 1) Le matériel n’est pas disponible. Réessayez plus tard.




-   **`ErrorLockout`** &ndash; (valeur 7) L’opération a été annulée parce que l’API est verrouillé en raison du trop grand nombre de tentatives.




-   **`ErrorNoSpace`** &ndash; (valeur 4) État d’erreur retourné pour des opérations telles que l’inscription ; l’opération ne peut pas aboutir, car il n’est pas un stockage suffisant restant pour terminer l’opération.



-   **`ErrorTimeout`** &ndash; (valeur 3) État d’erreur retourné lors de la requête actuelle a été trop longue. Cela est destiné à empêcher les programmes d’attendre indéfiniment pour le capteur d’empreintes digitales. Le délai d’attente est la plateforme et capteur spécifiques, mais il est généralement environ 30 secondes.



-   **`ErrorUnableToProcess`** &ndash; (valeur 2) État d’erreur renvoyé lorsque le capteur n’a pas pu traiter l’image actuelle.



## <a name="related-links"></a>Liens associés

- [Cipher](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
