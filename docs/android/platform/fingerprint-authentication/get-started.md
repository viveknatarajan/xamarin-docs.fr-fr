---
title: Prise en main de l’authentification par empreinte digitale
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 70d27ef3d7518619a246c25aac128b2fd1ed70c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764396"
---
# <a name="getting-started-with-fingerprint-authentication"></a>Prise en main de l’authentification par empreinte digitale

Pour commencer, nous allons couvrir de tout d’abord comment configurer un projet Xamarin.Android afin que l’application est en mesure d’utiliser l’authentification par empreinte :

1. Mise à jour **AndroidManifest.xml** pour déclarer les autorisations requises par les API d’empreintes digitales.
2. Obtenir une référence à la `FingerprintManager`.
3. Vérifiez que l’appareil est en mesure d’analyse d’empreintes digitales.

## <a name="requesting-permissions-in-the-application-manifest"></a>Demande d’autorisations dans l’Application de manifeste

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Une application Android doit demander le `USE_FINGERPRINT` autorisation dans le manifeste. La capture d’écran suivante illustre l’ajout de cette autorisation à l’application dans Visual Studio 2015 :

[![L’activation\_par empreinte digitale dans l’écran de manifeste Android](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Une application Android doit demander le `USE_FINGERPRINT` autorisation dans le manifeste. La capture d’écran suivante illustre l’ajout de cette autorisation à l’application dans Visual Studio pour Mac :

[![L’activation de UseFingerprint dans l’écran de l’Application Android](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Obtention d’une Instance de la FingerprintManager

Ensuite, l’application doit obtenir une instance de la `FingerprintManager` ou `FingerprintManagerCompat` classe. Pour être compatible avec les versions antérieures d’Android, une application Android doit utiliser l’API de compatibilité trouvées dans le package NuGet de v4 Android prennent en charge. L’extrait de code suivant montre comment obtenir l’objet approprié à partir du système d’exploitation : 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

Dans l’extrait de code précédent, le `context` est n’importe quel Android `Android.Content.Context`. Il s’agit généralement de l’activité qui effectue l’authentification.

## <a name="checking-for-eligibility"></a>La vérification d’éligibilité

Une application doit effectuer plusieurs vérifications pour vous assurer qu’il est possible d’utiliser l’authentification par empreinte digitale. Au total, il existe cinq conditions que l’application utilise pour vérifier l’éligibilité :  
 

**API de niveau 23** &ndash; les API d’empreintes digitales nécessitent le niveau d’API égal ou supérieur à 23. La `FingerprintManagerCompat` classe encapsule le contrôle de niveau API pour vous. Pour cette raison, il est recommandé d’utiliser le **bibliothèque de prise en charge Android v4** et `FingerprintManagerCompat`; cela en compte de l’un de ces vérifications.

**Matériel** &ndash; lorsque l’application démarre pour la première fois, il doit vérifier la présence d’un lecteur d’empreintes digitales :

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**Appareil est sécurisé** &ndash; l’utilisateur doit disposer de l’appareil sécurisé avec un verrouillage d’écran. Si l’utilisateur n’a pas sécurisé l’appareil avec un verrouillage d’écran et la sécurité est importante pour l’application, l’utilisateur doit être averti qu’un verrouillage d’écran doit être configuré. L’extrait de code suivant montre comment vérifier cette pre-requiste :

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Inscrit les empreintes digitales** &ndash; l’utilisateur doit disposer d’au moins une empreinte digitale inscrit avec le système d’exploitation. Cette vérification d’autorisation doit se produire avant chaque tentative d’authentification :

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Autorisations** &ndash; l’application doit demander l’autorisation de l’utilisateur avant d’utiliser l’application. Pour Android 5.0 et inférieure, l’utilisateur accorde l’autorisation en tant que condition de l’installation de l’application. Android 6.0 a introduit un nouveau modèle d’autorisation qui vérifie les autorisations au moment de l’exécution. Cet extrait de code est un exemple de procédure de vérification des autorisations sur Android 6.0 :

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // http://developer.android.com/training/permissions/requesting.html
}
```

Une application doit vérifier les conditions 3, 4 et 5, chaque fois qu’il souhaite utiliser l’authentification par empreinte digitale. Les deux premières conditions peuvent être vérifiées, la première fois qu’une application est exécutée sur un périphérique et les résultats enregistrés (dans partagé préférences par exemple).

Pour plus d’informations sur la façon de demander des autorisations dans Android 6.0, consultez le guide Android [demande d’autorisations au moment de l’exécution](http://developer.android.com/training/permissions/requesting.html).



## <a name="related-links"></a>Liens associés

- [Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Demande des autorisations au moment de l’exécution](http://developer.android.com/training/permissions/requesting.html)
