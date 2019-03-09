---
title: Création d’un CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1305a8a1f39d34b5e91e478a769750911afb2b3e
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669635"
---
# <a name="creating-a-cryptoobject"></a>Création d’un CryptoObject

L’intégrité des résultats de l’authentification par empreinte digitale est importante pour une application &ndash; il est la façon dont l’application connaît l’identité de l’utilisateur. Il est théoriquement possible pour les programmes malveillants par des tiers d’intercepter et de falsifier les résultats retournés par le lecteur d’empreintes digitales. Cette section décrit une technique permettant de préserver la validité des résultats par empreinte digitale. 

Le `FingerprintManager.CryptoObject` est un wrapper autour de l’API de chiffrement de Java et est utilisé par le `FingerprintManager` pour protéger l’intégrité de la demande d’authentification. En règle générale, un `Javax.Crypto.Cipher` objet est le mécanisme de chiffrement les résultats de la lecteur d’empreintes digitales. Le `Cipher` objet lui-même utilisera une clé qui est créée par l’application à l’aide de l’API de magasin de clés Android.

Pour comprendre comment toutes ces classes fonctionnent ensemble, commençons par examiner le code suivant qui montre comment créer un `CryptoObject`et puis expliquent plus en détail :

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

L’exemple de code crée un nouveau `Cipher` pour chaque `CryptoObject`, à l’aide d’une clé qui a été créée par l’application. La clé est identifiée par le `KEY_NAME` variable qui a été définie au début de la `CryptoObjectHelper` classe. La méthode `GetKey` essayer et de récupérer la clé à l’aide de la APIs Keystore Android. Si la clé n’existe pas, la méthode `CreateKey` créera une nouvelle clé pour l’application.

Le chiffrement est instancié avec un appel à `Cipher.GetInstance`, avec un _transformation_ (valeur de chaîne qui indique le chiffrement comment chiffrer et déchiffrer des données). L’appel à `Cipher.Init` se termine l’initialisation du chiffrement en fournissant une clé à partir de l’application. 

Il est important de savoir qu’il existe certaines situations où Android peut invalider la clé : 

* Une nouvelle empreinte digitale a été inscrit avec l’appareil.
* Il n’y a aucune empreintes inscrits auprès de l’appareil.
* L’utilisateur a désactivé l’écran de verrouillage.
* L’utilisateur a modifié le verrouillage d’écran (le type de la screenlock ou le code confidentiel/du modèle utilisé).

Dans ce cas, `Cipher.Init` lèvera une [ `KeyPermanentlyInvalidatedException` ](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). L’exemple de code ci-dessus sera intercepter cette exception, supprimez la clé, puis créez-en un.

La section suivante explique comment créer la clé et le stocker sur l’appareil.

## <a name="creating-a-secret-key"></a>Création d’une clé secrète

Le `CryptoObjectHelper` classe utilise le Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) pour créer une clé et le stocker sur l’appareil. Le `KeyGenerator` classe peut créer la clé, mais qu’elle doit certaines métadonnées sur le type de clé à créer. Ces informations sont fournies par une instance de la [ `KeyGenParameterSpec` ](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) classe. 

Un `KeyGenerator` est instancié à l’aide de la `GetInstance` méthode de fabrique. L’exemple de code utilise le [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) en tant que l’algorithme de chiffrement. AES scinder les données en blocs de taille fixe et le chiffrer chacun de ces blocs.

Ensuite, un `KeyGenParameterSpec` est créé à l’aide de la `KeyGenParameterSpec.Builder`. Le `KeyGenParameterSpec.Builder` encapsule les informations suivantes sur la clé qui doit être créé :

* Le nom de la clé.
* La clé doit être valide pour le chiffrement et le déchiffrement.
* Dans l’exemple de code la `BLOCK_MODE` a la valeur _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), ce qui signifie que chaque bloc est fonction XOR avec le bloc précédent (création de dépendances entre chaque bloc). 
* Le `CryptoObjectHelper` utilise [ _Public Key Cryptography Standard #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) pour générer les octets qui remplira les blocs pour vous assurer qu’ils sont tous de la même taille .
* `SetUserAuthenticationRequired(true)` signifie que l’authentification utilisateur est requise avant de la clé peut être utilisée.

Une fois le `KeyGenParameterSpec` est créée, elle est utilisée pour initialiser le `KeyGenerator`, qui sera générer une clé et le stocker en toute sécurité sur l’appareil. 

## <a name="using-the-cryptoobjecthelper"></a>À l’aide de la CryptoObjectHelper

Maintenant que l’exemple de code a encapsulé une grande partie de la logique pour la création d’un `CryptoWrapper` dans le `CryptoObjectHelper` classe, nous allons revoir le code à partir du début de ce guide et utiliser le `CryptoObjectHelper` pour créer le chiffrement et de démarrer un lecteur d’empreintes digitales : 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Maintenant que nous avons vu comment créer un `CryptoObject`, permet de passer à voir comment la `FingerprintManager.AuthenticationCallbacks` sont utilisées pour transférer les résultats du service de scanneur d’empreinte digitale à une application Android.



## <a name="related-links"></a>Liens associés

- [Cipher](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
