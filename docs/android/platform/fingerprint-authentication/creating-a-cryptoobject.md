---
title: Création d’un CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f7a8ab7a43c0a3258cf6e737b0d235cbe7a1c747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-cryptoobject"></a>Création d’un CryptoObject

L’intégrité des résultats de l’authentification par empreinte digitale est importante pour une application &ndash; elle est la façon dont l’application connaît l’identité de l’utilisateur. Il est théoriquement possible pour les programmes malveillants de tiers d’intercepter et de falsifier les résultats retournés par le lecteur d’empreintes digitales. Cette section traite de l’une des techniques permettant de conserver la validité des résultats d’empreintes digitales. 

Le `FingerprintManager.CryptoObject` est un wrapper autour de l’API de chiffrement de Java et est utilisé par le `FingerprintManager` afin de protéger l’intégrité de la demande d’authentification. En règle générale, un `Javax.Crypto.Cipher` objet est un mécanisme pour chiffrer les résultats de l’analyseur d’empreintes digitales. Le `Cipher` objet lui-même utilisera une clé qui est créée par l’application à l’aide de l’API de magasin de clés Android.

Pour comprendre le fonctionnement toutes ces classes, commençons par examiner le code suivant qui montre comment créer un `CryptoObject`et puis expliquent plus en détail :

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

L’exemple de code crée un nouveau `Cipher` pour chaque `CryptoObject`, à l’aide d’une clé qui a été créée par l’application. La clé est identifiée par le `KEY_NAME` variable qui a été définie au début de la `CryptoObjectHelper` classe. La méthode `GetKey` opération de récupérer la clé à l’aide de la APIs Keystore Android. Si la clé n’existe pas, la méthode `CreateKey` créera une nouvelle clé pour l’application.

Le chiffrement est instancié avec un appel à `Cipher.GetInstance`, avec un _transformation_ (une valeur de chaîne qui indique le chiffrement comment chiffrer et déchiffrer les données). L’appel à `Cipher.Init` s’achève l’initialisation du chiffrement en fournissant une clé à partir de l’application. 

Il est important de savoir qu’il existe certaines situations où Android peut invalider la clé : 

* Une nouvelle empreinte a été inscrit avec le périphérique.
* Il n’y a aucune empreintes inscrits auprès de l’appareil.
* L’utilisateur a désactivé le verrouillage d’écran.
* L’utilisateur a modifié le verrouillage d’écran (le type de la screenlock ou le code confidentiel/modèle utilisé).

Dans ce cas, `Cipher.Init` lèvera une [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). L’exemple de code ci-dessus sera intercepter cette exception, supprimer la clé, puis créez-en un.

La section suivante explique comment créer la clé et le stocker sur l’appareil.

## <a name="creating-a-secret-key"></a>Création d’une clé secrète

Le `CryptoObjectHelper` classe utilise l’Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) pour créer une clé et les stocker sur l’appareil. La `KeyGenerator` classe peut créer la clé, mais qu’elle doit certaines métadonnées sur le type de clé à créer. Ces informations sont fournies par une instance de la [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) classe. 

A `KeyGenerator` est instancié à l’aide de la `GetInstance` méthode de fabrique. L’exemple de code utilise le [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) comme algorithme de chiffrement. AES est diviser les données en blocs de taille fixe et chiffrer chacun de ces blocs.

Ensuite, un `KeyGenParameterSpec` est créé à l’aide de la `KeyGenParameterSpec.Builder`. Le `KeyGenParameterSpec.Builder` encapsule les informations suivantes sur la clé qui doit être créé :

* Le nom de la clé.
* La clé doit être valide pour le chiffrement et le déchiffrement.
* Dans l’exemple de code la `BLOCK_MODE` a la valeur _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), ce qui signifie que chaque bloc est fonction XOR avec le bloc précédent (création de dépendances entre chaque bloc). 
* Le `CryptoObjectHelper` utilise [ _Public Key Cryptography Standard #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) pour générer les octets remplira les blocs pour vous assurer qu’ils sont tous de la même taille .
* `SetUserAuthenticationRequired(true)` signifie que l’authentification utilisateur est requise avant que la clé peut être utilisée.

Une fois la `KeyGenParameterSpec` est créée, elle est utilisée pour initialiser le `KeyGenerator`, qui génère une clé et stocker de façon sécurisée sur l’appareil. 

## <a name="using-the-cryptoobjecthelper"></a>À l’aide de la CryptoObjectHelper

Maintenant que l’exemple de code a encapsulé la logique pour la création d’un `CryptoWrapper` dans les `CryptoObjectHelper` classe, nous allons modifier le code à partir du début de ce guide et d’utiliser le `CryptoObjectHelper` pour créer le chiffrement et de démarrer un lecteur d’empreintes digitales : 

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

Maintenant que nous avons vu comment créer un `CryptoObject`, permet de déplacer voir comment la `FingerprintManager.AuthenticationCallbacks` sont utilisées pour transférer les résultats du service de l’analyseur d’empreintes digitales pour une application Android.



## <a name="related-links"></a>Liens associés

- [Cipher](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
