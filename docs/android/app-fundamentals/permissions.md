---
title: Autorisations de Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 39ee7f826d4c775ead679a09ce56a7c0f92b60ed
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="permissions-in-xamarinandroid"></a>Autorisations de Xamarin.Android


## <a name="overview"></a>Vue d'ensemble

Les applications Android s’exécutent dans leur propre bac à sable et pour la sécurité de raisons n’ont pas accès à certaines ressources système ou du matériel sur l’appareil. L’utilisateur doit accorder explicitement l’autorisation de l’application avant qu’elle peut utiliser ces ressources. Par exemple, une application ne peut pas accéder au GPS sur un appareil sans autorisation explicite de l’utilisateur. Android lèvera une `Java.Lang.SecurityException` si une application tente d’accéder à une ressource protégée sans autorisation.

Les autorisations sont déclarées dans le **AndroidManifest.xml** par le développeur de l’application lors du développement de l’application. Android comprend deux workflows distincts pour obtenir le consentement de l’utilisateur pour ces autorisations :
 
* Pour les applications ciblant Android 5.1 (API niveau 22) ou version antérieure, la demande d’autorisation lors de l’application a été installée. Si l’utilisateur n’a pas accordé les autorisations, l’application n’est pas installée. Une fois que l’application est installée, il n’est pas possible de révoquer les autorisations, à l’exception en désinstallant l’application.
* À compter de Android 6.0 (API niveau 23), les utilisateurs ont été donnés davantage de contrôle sur les autorisations ; Vous pouvez accorder ou révoquer des autorisations, que l’application est installée sur l’appareil. Cette capture d’écran montre les paramètres d’autorisation pour l’application Contacts de Google. Elle répertorie les différentes autorisations et permet à l’utilisateur Activer ou désactiver les autorisations :

![Exemple d’écran des autorisations](permissions-images/01-permissions-check.png) 

Les applications Android doivent vérifier au moment de l’exécution pour qu’ils possèdent l’autorisation d’accéder à une ressource protégée. Si l’application n’est pas autorisé, il doit créer des requêtes utilisant les nouvelles API fournies par le SDK Android pour l’utilisateur d’accorder les autorisations. Les autorisations sont divisées en deux catégories :

* **Les autorisations normal** &ndash; ces autorisations qui présentent peu de risques de sécurité pour la sécurité ou de confidentialité de l’utilisateur sont. Android 6.0 accorde des autorisations normales automatiquement au moment de l’installation. Consultez la documentation d’Android pour un [la liste complète des autorisations normales](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Autorisations dangereuses** &ndash; Contrairement aux autorisations normales, autorisations dangereuses sont celles qui protègent la sécurité ou confidentialité de l’utilisateur. Ceux-ci doivent être explicitement accordées par l’utilisateur. Envoyer ou recevoir un message SMS est un exemple d’une action nécessitant une autorisation dangereuse.

> [!IMPORTANT]
> La catégorie à laquelle appartient une autorisation peut changer au fil du temps.  Il est possible qu’une autorisation qui a été classée comme une autorisation « normale » peut être élevé dans les futures niveaux d’API pour une autorisation dangereuse.

Autorisations dangereuses sont pas divisées en [ _groupes d’autorisations_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Un groupe d’autorisations conserve les autorisations qui sont liées de manière logique. Lorsque l’utilisateur accorde l’autorisation à un membre d’un groupe d’autorisations, Android accorde automatiquement l’autorisation à tous les membres de ce groupe. Par exemple, le [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) groupe d’autorisation conserve à la fois le `WRITE_EXTERNAL_STORAGE` et `READ_EXTERNAL_STORAGE` autorisations. Si l’utilisateur accorde l’autorisation de `READ_EXTERNAL_STORAGE`, puis le `WRITE_EXTERNAL_STORAGE` autorisation est accordée automatiquement en même temps.

Avant de demander une ou plusieurs autorisations, il est recommandé de fournir une logique pour la raison pour laquelle l’application requiert l’autorisation avant de demander l’autorisation. Une fois que l’utilisateur comprend la logique, l’application peut demander l’autorisation de l’utilisateur. En comprenant le raisonnement, l’utilisateur peut prendre une décision informée s’ils souhaitent accorder l’autorisation et de comprendre les conséquences s’ils ne sont pas. 

Le flux de travail entière de vérification et de la demande d’autorisations est appelé un _autorisations d’exécution_ Vérifiez et peut être résumé dans le diagramme suivant : 

[![Diagramme de flux de contrôle autorisation d’exécution](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

La bibliothèque de prise en charge Android backports certaines des nouvelles API pour les autorisations sur les versions antérieures d’Android. Ces API utilisées vérifie automatiquement la version d’Android sur l’appareil afin qu’il n’est pas nécessaire d’effectuer un contrôle de niveau API chaque fois.  

Ce document explique comment ajouter des autorisations à une application de Xamarin.Android et la manière dont les applications qui ciblent Android 6.0 (API niveau 23) ou version ultérieure doit y effectuer une vérification d’autorisation d’exécution.


> [!NOTE]
> Il est possible que les autorisations pour le matériel peuvent affecter comment l’application est filtrée par Google Play. Par exemple, si l’application requiert l’autorisation de l’appareil photo, puis Google Play n’affichent pas l’application dans la Boutique Google Play sur un appareil qui ne dispose pas d’un appareil photo installé.


<a name="requirements" />

## <a name="requirements"></a>Configuration requise

Il est fortement recommandé que les projets Xamarin.Android incluent le [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) package NuGet. Cette autorisation de backport package sera API spécifiques à des versions antérieures d’Android, en fournissant un commun de l’interface sans devoir constamment vérifier la version d’Android l’application est en cours d’exécution.


## <a name="requesting-system-permissions"></a>Demande des autorisations de système

La première étape de l’utilisation d’autorisations Android doit déclarer que les autorisations dans le Android du fichier manifeste. Cela est indispensable, quelle que soit le niveau de l’API que l’application est ciblé.

Applications qui ciblent Android 6.0 ou une version ultérieure ne peut pas supposent que parce que l’utilisateur autorisé à un moment donné dans le passé, que l’autorisation sera valide la prochaine fois. Une application qui cible Android 6.0 doit toujours effectuer une vérification d’autorisation de runtime. Les applications qui ciblent Android 5.1 ou inférieure est inutile d’effectuer une vérification de l’autorisation d’exécution.

> [!NOTE]
> Les applications doivent demander uniquement les autorisations dont ils ont besoin.


### <a name="declaring-permissions-in-the-manifest"></a>Déclarer des autorisations dans le manifeste

Les autorisations sont ajoutées à la **AndroidManifest.xml** avec la `uses-permission` élément. Par exemple, si une application est d’identifier l’emplacement de l’appareil, il requiert correctement et autorisations de l’emplacement du cours. Les deux éléments suivants sont ajoutés au manifeste : 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Il est possible de déclarer les autorisations à l’aide de la prise en charge de l’outil intégré à Visual Studio :

1. Double-cliquez sur **propriétés** dans les **l’Explorateur de solutions** et sélectionnez le **manifeste Android** onglet dans la fenêtre Propriétés :

    [![Autorisations requises dans l’onglet manifeste Android](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Si l’application n’a pas déjà un fichier AndroidManifest.xml, cliquez sur **AndroidManifest.xml non trouvé. Cliquez pour ajouter un** comme indiqué ci-dessous :

    [![Aucun message AndroidManifest.xml](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Sélectionnez les autorisations que votre application a besoin à partir de la **autorisations requises** liste et enregistrer :

    [![Exemples d’autorisations de caméra sélectionnés](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Il est possible de déclarer les autorisations à l’aide de la prise en charge de l’outil intégré à Visual Studio pour Mac :

1. Double-cliquez sur le projet dans le **Solution remplissage** et sélectionnez **Options > Générer > Application Android**:

    [![Section d’autorisations requis indiquée](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Cliquez sur le **ajouter le manifeste Android** bouton si le projet n’a pas déjà un **AndroidManifest.xml**:

    [![Manifeste de Android du projet est manquant](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Sélectionnez les autorisations que votre application a besoin à partir de la **autorisations requises** liste et cliquez sur **OK**:

    [![Exemples d’autorisations de caméra sélectionnés](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android ajoute automatiquement des autorisations au moment de la génération pour les versions Debug. Cela rend le débogage de l’application plus facile. En particulier, les deux autorisations notables sont `INTERNET` et `READ_EXTERNAL_STORAGE`. Ces autorisations définies automatiquement ne semblent pas être activée dans le **autorisations requises** liste. Toutefois, les versions Release, utilisez uniquement les autorisations qui sont définies explicitement dans le **autorisations requises** liste. 

Pour les applications qui ciblent la 5.1 Android (API niveau 22) ou version antérieure, il est devant être effectué. Les applications qui seront exécute sur Android 6.0 (API 23 niveau 23) ou version ultérieure doivent passer à la section suivante sur la façon d’effectuer les vérifications des autorisations d’exécution. 


### <a name="runtime-permission-checks-in-android-60"></a>Vérifications des autorisations d’exécution dans Android 6.0

Le `ContextCompat.CheckSelfPermission` (disponible dans la bibliothèque de prise en charge Android) de la méthode est utilisée pour vérifier si une autorisation spécifique a été accordée. Cette méthode retourne un [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) enum qui a l’une des deux valeurs :

* **`Permission.Granted`** &ndash; A reçu l’autorisation spécifiée.
* **`Permission.Denied`** &ndash; L’autorisation spécifiée n’a pas été accordée.

Cet extrait de code est un exemple de procédure de vérification de l’autorisation de l’appareil photo dans une activité : 

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

Il est recommandé d’informer l’utilisateur sur la raison pour laquelle une autorisation est nécessaire pour une application afin qu’une décision informée est possible d’accorder l’autorisation. Un exemple de ce serait une application qui prend des photos et des balises de géo-réplication les. Il est clair à l’utilisateur que l’autorisation de l’appareil photo est nécessaire, mais il peut être difficile de raison pour laquelle l’application doit également l’emplacement de l’appareil. Le raisonnement doit afficher un message pour aider l’utilisateur à comprendre la raison pour laquelle l’autorisation de l’emplacement est souhaitable et que l’autorisation de l’appareil photo est nécessaire.

Le `ActivityCompat.ShouldShowRequestPermissionRational` méthode est utilisée pour déterminer si la logique qui doit être affichée à l’utilisateur. Cette méthode retournera `true` si la logique pour une autorisation donnée doit être affichée. Cette capture d’écran montre un exemple d’un Snackbar affiché par une application qui explique pourquoi l’application a besoin de connaître l’emplacement de l’appareil :

![Raisonnement pour l’emplacement](permissions-images/07-rationale-snackbar.png) 

Si l’utilisateur accorde l’autorisation, le `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` méthode doit être appelée. Cette méthode requiert les paramètres suivants :

* **activité** &ndash; c’est l’activité qui demande les autorisations et doit être informé par Android des résultats.
* **autorisations** &ndash; une liste des autorisations qui sont demandées.
* **requestCode** &ndash; une valeur entière qui est utilisée pour faire correspondre les résultats de la demande d’autorisation à un `RequestPermissions` appeler. Cette valeur doit être supérieure à zéro.

Cet extrait de code est un exemple des deux méthodes qui ont été présentés. Tout d’abord, une vérification est effectuée pour déterminer si la logique d’autorisation doit être indiquée. Si la logique qui doit être affiché, un Snackbar s’affiche avec le raisonnement sous-jacent. Si l’utilisateur clique sur **OK** dans le Snackbar, l’application demandera les autorisations. Si l’utilisateur n’accepte pas la logique, l’application ne doit pas continuer à demander des autorisations. Si la logique qui n’est pas affichée, l’activité de demande l’autorisation :

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

`RequestPermission` peut être appelée même si l’utilisateur a déjà obtenu l’autorisation. Les appels suivants ne sont pas nécessaires, mais ils fournissent l’utilisateur la possibilité de confirmer (ou révoquer) l’autorisation. Lorsque `RequestPermission` est appelée, contrôle est remis au système d’exploitation, qui affiche une interface utilisateur permettant d’accepter les autorisations :  

![Boîte de dialogue sont](permissions-images/08-location-permission-dialog.png)

Une fois que l’utilisateur est terminé, Android retournera les résultats à l’activité via une méthode de rappel, `OnRequestPermissionResult`. Cette méthode est une partie de l’interface `ActivityCompat.IOnRequestPermissionsResultCallback` qui doit être implémentée par l’activité. Cette interface a une méthode unique, `OnRequestPermissionsResult`, qui sera appelé par Android pour informer l’activité des choix de l’utilisateur. Si l’utilisateur a l’autorisation, l’application peut continuer et utiliser la ressource protégée. Un exemple montrant comment implémenter `OnRequestPermissionResult` est indiqué ci-dessous : 

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
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
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

Ce guide décrit comment ajouter et vérifier des autorisations sur un appareil Android. Les différences de fonctionnement des autorisations entre les anciennes applications Android (API niveau < 23) et les applications Android (API niveau 22 >). Il décrit comment effectuer des vérifications d’autorisations d’exécution dans Android 6.0.


## <a name="related-links"></a>Liens associés

- [Liste des autorisations normales](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Exemple d’application Runtime autorisations](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [La gestion des autorisations dans Xamarin.Android](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
