---
title: Autorisations dans Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 204dd903586164691d068a956e741c406df10b36
ms.sourcegitcommit: 9492e417f739772bf264f5944d6bae056e130480
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53746867"
---
# <a name="permissions-in-xamarinandroid"></a>Autorisations dans Xamarin.Android


## <a name="overview"></a>Vue d'ensemble

Les applications Android qui s’exécutent dans leur propre sandbox et pour la sécurité raisons n’ont pas accès à certaines ressources système ou du matériel sur l’appareil. L’utilisateur doit accorder explicitement l’autorisation à l’application avant qu’elle peut utiliser ces ressources. Par exemple, une application ne peut pas accéder au GPS sur un appareil sans autorisation explicite de l’utilisateur. Android lèvera une `Java.Lang.SecurityException` si une application tente d’accéder à une ressource protégée sans autorisation.

Les autorisations sont déclarées dans le **AndroidManifest.xml** par le développeur d’applications quand l’application est développée. Android a deux workflows distincts pour obtenir le consentement de l’utilisateur pour ces autorisations :
 
* Pour les applications ciblant Android 5.1 (API niveau 22) ou un niveau inférieur, la demande d’autorisation lors de l’application a été installée. Si l’utilisateur n’a pas accordé les autorisations, l’application n’est pas installée. Une fois que l’application est installée, il n’existe aucun moyen de révoquer les autorisations à l’exception en désinstallant l’application.
* À compter de Android 6.0 (niveau d’API 23), les utilisateurs ont été donnés davantage de contrôle sur les autorisations ; Vous pouvez accorder ou révoquer des autorisations, que l’application est installée sur l’appareil. Cette capture d’écran montre les paramètres d’autorisation pour l’application Contacts de Google. Il répertorie les différentes autorisations et permet à l’utilisateur Activer ou désactiver les autorisations :

![Exemple d’écran autorisations](permissions-images/01-permissions-check.png) 

Applications Android doivent vérifier au moment de l’exécution pour voir s’ils ont l’autorisation d’accéder à une ressource protégée. Si l’application n’est pas autorisé, il doit être demandes à l’aide des nouvelles API fournies par le SDK Android pour l’utilisateur pour accorder les autorisations. Les autorisations sont divisées en deux catégories :

* **Les autorisations normal** &ndash; il s’agit des autorisations qui présentent peu risque de sécurité pour la sécurité ou de confidentialité de l’utilisateur. Android 6.0 accorde des autorisations normales automatiquement au moment de l’installation. Veuillez consulter la documentation Android pour un [la liste complète des autorisations normales](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Autorisations dangereuses** &ndash; Contrairement aux autorisations normales, les autorisations dangereuses sont celles qui protègent la sécurité ou confidentialité de l’utilisateur. Ils doivent être explicitement accordées par l’utilisateur. Envoyer ou recevoir un message SMS est un exemple d’une action nécessitant une autorisation dangereuse.

> [!IMPORTANT]
> La catégorie à laquelle appartient une autorisation peut changer au fil du temps.  Il est possible qu’une autorisation qui a été classée comme une autorisation « normale » peut être dans un avenir niveaux d’API élevés au rang d’une autorisation dangereuse.

Autorisations dangereuses sont sous-composant divisées en [ _groupes d’autorisation_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Un groupe d’autorisations conserve les autorisations qui sont logiquement liées. Lorsque l’utilisateur accorde l’autorisation à un membre d’un groupe d’autorisation, Android accorde automatiquement l’autorisation à tous les membres de ce groupe. Par exemple, le [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) groupe d’autorisations conserve à la fois le `WRITE_EXTERNAL_STORAGE` et `READ_EXTERNAL_STORAGE` autorisations. Si l’utilisateur accorde l’autorisation de `READ_EXTERNAL_STORAGE`, puis le `WRITE_EXTERNAL_STORAGE` autorisation est accordée automatiquement en même temps.

Avant de demander une ou plusieurs autorisations, il est recommandé de fournir une justification quant à la raison pour laquelle l’application requiert l’autorisation avant de demander l’autorisation. Une fois que l’utilisateur comprend le raisonnement, l’application peut demander l’autorisation de l’utilisateur. Comprendre le raisonnement, l’utilisateur peut prendre une décision éclairée s’ils souhaitent accorder l’autorisation et de comprendre les répercussions s’ils ne le font pas. 

Le flux de travail entière de vérification et de la demande d’autorisations est appelé un _autorisations d’exécution_ vérifier et peuvent être classés dans le diagramme suivant : 

[![Autorisation d’exécution cocher Organigramme](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

La bibliothèque de prise en charge Android backports certaines des nouvelles API pour les autorisations sur des versions antérieures d’Android. Ces API rétroportée vérifie automatiquement la version d’Android sur l’appareil afin qu’il n’est pas nécessaire d’effectuer un contrôle de niveau API chaque fois.  

Ce document explique comment ajouter des autorisations à une application Xamarin.Android et la façon dont les applications qui ciblent Android 6.0 (niveau d’API 23) ou une version ultérieure doit effectuer une vérification d’autorisation d’exécution.


> [!NOTE]
> Il est possible que les autorisations pour le matériel peuvent affecter la façon dont l’application est filtrée par Google Play. Par exemple, si l’application requiert une autorisation pour l’appareil photo, puis Google Play n’affiche pas l’application dans le Store Play Google sur un appareil qui ne dispose pas d’un appareil photo installé.


<a name="requirements" />

## <a name="requirements"></a>Spécifications

Il est fortement recommandé d’inclure dans les projets de Xamarin.Android le [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) package NuGet. Cette autorisation de rétroporter package va API spécifiques pour les versions antérieures d’Android, en fournissant un commun de l’interface sans devoir constamment vérifier la version d’Android utilisée par l’application est en cours d’exécution.


## <a name="requesting-system-permissions"></a>Demande des autorisations de système

La première étape de l’utilisation des autorisations Android consiste à déclarer que les autorisations Android du fichier manifeste. Cela doit être effectuée indépendamment du niveau d’API que l’application est ciblé.

Applications qui ciblent Android 6.0 ou version ultérieure ne peut pas supposent que comme l’utilisateur l’autorisation à un moment donné dans le passé, que l’autorisation sera valide la prochaine fois. Une application qui cible Android 6.0 doit toujours effectuer une vérification d’autorisation de runtime. Les applications qui ciblent Android 5.1 ou inférieur n’avez pas besoin d’effectuer une vérification de l’autorisation d’exécution.

> [!NOTE]
> Les applications doivent demander uniquement les autorisations dont ils ont besoin.


### <a name="declaring-permissions-in-the-manifest"></a>Déclarer des autorisations dans le manifeste

Les autorisations sont ajoutées à la **AndroidManifest.xml** avec la `uses-permission` élément. Par exemple, si une application consiste à localiser la position de l’appareil, il nécessite correctement et les autorisations de localisation du cours. Les deux éléments suivants sont ajoutés au manifeste : 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Il est possible de déclarer les autorisations à l’aide de la prise en charge de l’outil intégré à Visual Studio :

1. Double-cliquez sur **propriétés** dans le **l’Explorateur de solutions** et sélectionnez le **manifeste Android** onglet dans la fenêtre Propriétés :

    [![Autorisations requises dans l’onglet manifeste Android](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Si l’application n’a pas déjà un fichier AndroidManifest.xml, cliquez sur **AndroidManifest.xml non trouvé. Cliquez pour ajouter un** comme indiqué ci-dessous :

    [![Aucun message AndroidManifest.xml](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Sélectionnez les autorisations que votre application a besoin à partir de la **autorisations requises** répertorier et enregistrer :

    [![Exemples d’autorisations de caméra sélectionnés](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Il est possible de déclarer les autorisations à l’aide de la prise en charge de l’outil intégré à Visual Studio pour Mac :

1. Double-cliquez sur le projet dans le **panneau solutions** et sélectionnez **Options > Générer > Application Android**:

    [![Section d’autorisations requis indiquée](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Cliquez sur le **ajouter le manifeste Android** bouton si le projet n’a pas déjà un **AndroidManifest.xml**:

    [![Manifeste Android du projet est manquant](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Sélectionnez les autorisations que votre application a besoin à partir de la **autorisations requises** liste et cliquez sur **OK**:

    [![Exemples d’autorisations de caméra sélectionnés](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android ajoutera automatiquement certaines autorisations au moment de la génération pour les versions Debug. Cela rendra le débogage de l’application plus facile. En particulier, les deux autorisations notables sont `INTERNET` et `READ_EXTERNAL_STORAGE`. Ces autorisations automatiquement-set ne semblent pas être activée dans le **autorisations requises** liste. Toutefois, les versions Release, utilisez uniquement les autorisations qui sont définies explicitement dans le **autorisations requises** liste. 

Pour les applications qui ciblent Android 5.1 (API niveau 22) ou version antérieure, il n’a rien d’autre qui doit être effectuée. Les applications qui seront exécute sur Android 6.0 (API 23 niveau 23) ou une version ultérieure doivent passer à la section suivante sur la façon d’effectuer des vérifications des autorisations d’exécution. 


### <a name="runtime-permission-checks-in-android-60"></a>Autorisation d’exécution vérifie dans Android 6.0

Le `ContextCompat.CheckSelfPermission` (méthode) (disponible avec la bibliothèque de prise en charge Android) est utilisé pour vérifier si une autorisation spécifique a été accordée. Cette méthode retournera un [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) enum qui a une des deux valeurs :

* **`Permission.Granted`** &ndash; L’autorisation spécifiée a été accordée.
* **`Permission.Denied`** &ndash; L’autorisation spécifiée n’a pas été accordée.

Cet extrait de code est un exemple montrant comment vérifier l’autorisation de l’appareil photo dans une activité : 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

Il est recommandé d’informer l’utilisateur quant à la raison pour laquelle une autorisation est nécessaire pour une application afin qu’une décision éclairée est possible d’accorder l’autorisation. Un exemple serait une application qui accepte des photos et géo-tags les. Il est clair à l’utilisateur que l’autorisation de l’appareil photo est nécessaire, mais il peut être difficile de comprendre l’application doit également l’emplacement de l’appareil. Le raisonnement doit afficher un message pour aider l’utilisateur à comprendre pourquoi l’autorisation de l’emplacement est souhaitable et que l’autorisation de l’appareil photo est nécessaire.

Le `ActivityCompat.ShouldShowRequestPermissionRationale` méthode est utilisée pour déterminer si la logique qui doit être indiquée à l’utilisateur. Cette méthode retournera `true` si la logique pour une autorisation donnée doit être affichée. Cette capture d’écran montre un exemple d’un Snackbar affichée par une application qui explique pourquoi l’application a besoin de connaître l’emplacement de l’appareil :

![Raisonnement pour l’emplacement](permissions-images/07-rationale-snackbar.png) 

Si l’utilisateur accorde l’autorisation, le `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` méthode doit être appelée. Cette méthode requiert les paramètres suivants :

* **activité** &ndash; c’est l’activité qui demande les autorisations et doit être informé par Android des résultats.
* **autorisations** &ndash; une liste des autorisations qui sont demandées.
* **requestCode** &ndash; une valeur entière qui est utilisée pour faire correspondre les résultats de la demande d’autorisation à un `RequestPermissions` appeler. Cette valeur doit être supérieure à zéro.

Cet extrait de code est un exemple des deux méthodes qui ont été abordés. Tout d’abord, un contrôle est effectué pour déterminer si la logique d’autorisation doit être indiquée. Si la logique qui doit être affiché, un Snackbar s’affiche avec le raisonnement. Si l’utilisateur clique sur **OK** dans le Snackbar, puis l’application demande les autorisations. Si l’utilisateur n’accepte pas le raisonnement, l’application ne doit pas continuer à demander les autorisations. Si la logique qui n’est pas affichée, l’activité de demande l’autorisation :

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` peut être appelée même si l’utilisateur a déjà autorisé. Les appels suivants ne sont pas nécessaires, mais ils fournissent l’utilisateur avec la possibilité de vérifier (ou révoquer) l’autorisation. Lorsque `RequestPermission` est appelée, contrôle est transféré au système d’exploitation, qui affiche une interface utilisateur pour accepter les autorisations :  

![Boîte de dialogue sont](permissions-images/08-location-permission-dialog.png)

Une fois que l’utilisateur est terminé, Android retournera les résultats à l’activité via une méthode de rappel, `OnRequestPermissionResult`. Cette méthode est une partie de l’interface `ActivityCompat.IOnRequestPermissionsResultCallback` qui doit être implémenté par l’activité. Cette interface a une méthode unique, `OnRequestPermissionsResult`, qui sera appelé par Android pour informer l’activité du choix de l’utilisateur. Si l’utilisateur a accordé l’autorisation, l’application peut continuer et utiliser la ressource protégée. Un exemple montrant comment implémenter `OnRequestPermissionResult` est indiqué ci-dessous : 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permission_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  


## <a name="summary"></a>Récapitulatif

Ce guide décrit comment ajouter et vérifier des autorisations sur un appareil Android. Les différences de fonctionnement des autorisations entre les anciennes applications Android (API niveau 23 <) et de nouvelles applications Android (API niveau 22 >). Il a expliqué comment effectuer des vérifications d’autorisations d’exécution dans Android 6.0.


## <a name="related-links"></a>Liens associés

- [Liste des autorisations normales](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Exemple d’application Runtime autorisations](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Gestion des autorisations dans Xamarin.Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
