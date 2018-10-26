---
title: Accès aux fichiers sur un stockage externe avec Xamarin.Android
description: Ce guide traite des accès aux fichiers sur un stockage externe dans Xamarin.Android
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: fa0ad282fedecec8f5ca4e94e7119c36ef182261
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116015"
---
# <a name="external-storage"></a>Stockage externe

Stockage externe fait référence au stockage de fichiers qui se trouve pas sur stockage interne et non exclusivement accessible à l’application est responsable du fichier. L’objectif principal de stockage externe consiste à fournir un emplacement pour placer les fichiers qui sont destinés à être partagés entre les applications ou qui sont trop volumineux pour tenir sur le stockage interne.

Stockage externe historiquement énonciation auquel une partition de disque sur un support amovible tel qu’une carte SD (également appelé _de stockage portable_). Cette distinction n’est plus aussi utile que les appareils Android ont évolué et nombre d’appareils Android ne plus en charge le stockage amovible. Au lieu de cela certains appareils alloue une partie de leur mémoire non volatile interne qui Android pour effectuer le même support amovible (fonction). Il s’agit _émulé_ stockage et est toujours considérée comme étant un stockage externe. Par ailleurs, certains appareils Android peuvent avoir plusieurs partitions de stockage externe. Par exemple, un Android tablette (en plus de son stockage interne) peut avoir émulée stockage et un ou plusieurs emplacements pour une carte SD. Toutes ces partitions sont traitées par Android en tant que stockage externe.

Sur les appareils qui ont plusieurs utilisateurs, chaque utilisateur aura un répertoire dédié sur la partition de stockage externe principal à leur stockage externe. Applications qui s’exécutent en tant qu’un utilisateur aura pas accès aux fichiers à partir d’un autre utilisateur sur l’appareil. Les fichiers pour tous les utilisateurs sont toujours en lecture et world inscriptible ; Toutefois, Android sera bac à sable chaque profil utilisateur des autres.

Lecture et écriture dans des fichiers sont presque identique dans Xamarin.Android, car il s’agit à n’importe quel autre application .NET. L’application Xamarin.Android détermine le chemin d’accès au fichier qui sera manipulé, puis utilise standard .NET idiomes pour l’accès de fichier. Étant donné que les chemins d’accès réels au stockage interne et externe peuvent varier d’un périphérique à l’autre ou à partir de la version d’Android vers la version Android, il est déconseillé de coder en dur le chemin d’accès aux fichiers. Au lieu de cela, Xamarin.Android expose les API Android natives qui vous aident à déterminer le chemin d’accès aux fichiers sur le stockage interne et externe.

Ce guide explique les concepts et les API Android qui sont spécifiques au stockage externe.

## <a name="public-and-private-files-on-external-storage"></a>Fichiers publiques et privées sur un stockage externe

Il existe deux types de fichiers une application peut conserver sur un stockage externe :

* **Privé** fichiers &ndash; fichiers privés sont des fichiers qui sont spécifiques à votre application (mais sont toujours en lecture et accessible en écriture). Android attend que les fichiers privés sont stockés dans un répertoire spécifique sur un stockage externe. Même si les fichiers sont appelées « private », ils sont toujours visibles et accessibles par d’autres applications sur l’appareil, ils ne sont pas offertes toute protection spéciale par Android.

* **Public** fichiers &ndash; ces sont des fichiers qui ne sont pas considérées comme propres à l’application et sont destinées à partager librement.

Les différences entre ces fichiers est avant tout conceptuel. Fichiers privés sont privés dans le sens qui leur sont considérés comme une partie de l’application, tandis que les fichiers publics sont tous les fichiers qui existent sur un stockage externe. Android fournit deux API différentes pour résoudre les chemins d’accès aux fichiers privés et publics, mais sinon les mêmes API .NET permettent de lire et écrire dans ces fichiers. Ce sont les mêmes API qui sont abordées dans la section sur [lecture et écriture](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage).

### <a name="private-external-files"></a>Fichiers externes privés

Fichiers externes privés sont considérés comme étant spécifique à une application (semblable aux fichiers internes), mais sont trouvent sur un stockage externe pour diverses raisons (par exemple, qui est trop grande pour le stockage interne). Comme pour les fichiers internes, ces fichiers est supprimés lors de l’application est désinstallée par l’utilisateur.

L’emplacement principal pour les fichiers externes privés est trouvé en appelant la méthode `Android.Content.Context.GetExternalFilesDir(string type)`. Cette méthode retournera un `Java.IO.File` objet qui représente le répertoire de stockage externe privé pour l’application. En passant `null` à cette méthode retourne le chemin d’accès au répertoire de stockage de l’utilisateur pour l’application. Par exemple, pour une application avec le nom du package `com.companyname.app`, le répertoire « racine » des fichiers externes privés serait :

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

Ce document fait référence au répertoire de stockage de fichiers privés sur un stockage externe comme _privé\_externe\_stockage_.


Le paramètre `GetExternalFilesDir()` est une chaîne qui spécifie un _répertoire de l’application_. Il s’agit d’un répertoire destiné à fournir un emplacement standard pour une organisation logique des fichiers. Les valeurs de chaîne sont disponibles via les constantes sur la `Android.OS.Environment` classe :

| `Android.OS.Environment` | Répertoire |
|-|-|
| DirectoryAlarms | **_PRIVÉ\_externe\_stockage_  /alarmes** |
| DirectoryDcim | **_PRIVÉ\_EXTERNE\_STOCKAGE_/DCIM** |
| DirectoryDownloads | **_PRIVÉ\_externe\_stockage_  /télécharger** |
| DirectoryDocuments | **_PRIVÉ\_externe\_stockage_  /Documents** |
| DirectoryMovies | **_PRIVÉ\_externe\_stockage_/Movies** |
| DirectoryMusic | **_PRIVÉ\_externe\_stockage_/Music** |
| DirectoryNotifications | **_PRIVÉ\_externe\_stockage_  /notifications** |
| DirectoryPodcasts | **_PRIVÉ\_externe\_stockage_/Podcasts** |
| DirectoryRingtones | **_PRIVÉ\_externe\_stockage_/Ringtones** |
| DirectoryPictures | **_PRIVÉ\_externe\_stockage_Pictures** |

Pour les appareils qui ont plusieurs partitions de stockage externe, chaque partition aura un répertoire qui est destiné aux fichiers privés. La méthode `Android.Content.Context.GetExternalFilesDirs(string type)` renvoie un tableau de `Java.IO.Files`. Chaque objet représente un répertoire spécifique à l’application privé sur tous les périphériques de stockage de partagés/externe où l’application peut placer les fichiers qu’il détient.

> [!IMPORTANT]
> Le chemin d’accès exact dans le répertoire de stockage privé externe peut varier à partir de l’appareil à l’appareil et entre les versions d’Android. Pour cette raison, les applications ne doivent pas dur le chemin d’accès à ce répertoire de code et à la place utiliser les APIs Xamarin.Android, comme `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Fichiers externes publics

Fichiers publics sont des fichiers qui existent sur un stockage externe qui ne sont pas stockées dans le répertoire Android alloue pour les fichiers privés. Fichiers publics ne sont pas supprimés lorsque l’application est désinstallée. Applications Android doivent être autorisées avant de pouvoir lire ou écrire tous les fichiers publics. Il est possible pour les fichiers publics se trouver n’importe où sur un stockage externe, mais par convention Android suppose que les fichiers publics d’exister dans le répertoire identifié par la propriété `Android.OS.Environment.ExternalStorageDirectory`. Cette propriété retourne un `Java.IO.File` objet qui représente le répertoire principal de stockage externe. Par exemple, `Android.OS.Environment.ExternalStorageDirectory` peuvent faire référence au répertoire suivant :

```bash
/storage/emulated/0/
```

Ce document fait référence au répertoire de stockage de fichiers publics sur un stockage externe comme _PUBLIC\_externe\_stockage_.


Android prend également en charge le concept de répertoires d’application _PUBLIC\_externe\_stockage_. Ces répertoires sont exactement les mêmes que l’application diretories pour `_PRIVATE\_EXTERNAL\_STORAGE_` et sont décrits dans le tableau dans la section précédente. La méthode `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` retournera un `Java.IO.File` objet qui correspondent à un répertoire d’application publique. Le `directoryType` paramètre est un paramètre obligatoire et ne peut pas être `null`.

Par exemple, l’appel `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` renvoie une chaîne qui ressemble à :

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Le chemin d’accès exact dans le répertoire de stockage externe public peut varier à partir de l’appareil à l’appareil et entre les versions d’Android. Pour cette raison, les applications ne doivent pas dur le chemin d’accès à ce répertoire de code et à la place utiliser les APIs Xamarin.Android, comme `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Utilisation du stockage externe

Une fois qu’une application Xamarin.Android a obtenu le chemin complet vers un fichier, il doit utiliser les API .NET standard pour la création, lecture, écriture ou suppression de fichiers. Cela permet d’optimiser la quantité d’entre le code de plate-forme compatible pour une application. Toutefois, avant de tenter d’accéder à un fichier une application Xamarin.Android doit s’assurer qu’est-il possible d’accéder à ce fichier.

1. **Vérifier le stockage externe** &ndash; selon la nature du stockage externe, il est possible qu’il ne peut pas être monté et utilisable par l’application. Toutes les applications doivent vérifier l’état du stockage externe avant de tenter de l’utiliser.
2. **Effectuer une vérification d’autorisation runtime** &ndash; Android un application doit demander l’autorisation de l’utilisateur afin d’accéder à un stockage externe. Cela signifie qu’un temps d’exécution demande d’autorisation doit être effectuée avant tout accès au fichier. Le guide [autorisations dans Xamarin.Android](~/android/app-fundamentals/permissions.md) contient plus de détails sur les autorisations Android.

Chacune de ces deux tâches est décrites ci-dessous.

### <a name="verifying-that-external-storage-is-available"></a>Vérification que le stockage externe est disponible

La première étape avant d’écrire dans un stockage externe consiste à vérifier qu’il est accessible en lecture ou en écriture. Le `Android.OS.Environment.ExternalStorageState` propriété contient une chaîne qui identifie l’état du stockage externe. Cette propriété retourne une chaîne qui représente l’état. Cette table est une liste de la `ExternalStorageState` valeurs pouvant être renvoyées par `Environment.ExternalStorageState`:

| ExternalStorageState | Description  |
|----------------------|---|
| MediaBadRemoval      | Le média a été brusquement supprimé sans non monté correctement. |
| MediaChecking        | Le support est présent mais un disque en cours de vérification.  |
| MediaEjecting        | Média est en train d’être démonté et supprimés du cache.  |
| MediaMounted         | Média est monté et peut être lues ou écrit dans.  |
| MediaMountedReadOnly | Support est monté, mais peut uniquement être lus à partir de. |
| MediaNofs            | Média est présent mais ne contient-elle pas un système de fichiers approprié pour Android. |
| MediaRemoved         | Il n’existe aucun support présent. |
| MediaShared          | Support est présent, mais il n’est pas monté. Il est partagé via une connexion USB avec un autre appareil.|
| MediaUnknown         | L’état du média n’est pas reconnu par Android. |
| MediaUnmountable     | Le support est présent mais ne peut pas être monté par Android. |
| MediaUnmounted       | Le support est présent mais n’est pas monté. |


La plupart des applications Android devrez vérifier si un stockage externe est monté. L’extrait de code suivant montre comment vérifier que le stockage externe est monté pour l’accès en lecture seule ou accès en lecture-écriture :

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Autorisations de stockage externe

Android prend en compte l’accès au stockage externe pour être un _autorisation dangereuse_, qui nécessite généralement l’utilisateur à accorder leur autorisation pour accéder à la ressource. L’utilisateur peut révoquer cette permission à tout moment.  Cela signifie qu’un temps d’exécution demande d’autorisation doit être effectuée avant tout accès au fichier. Les applications sont automatiquement accordées des autorisations pour lire et écrire leurs propres fichiers privés. Il est possible pour les applications lire et écrire les fichiers privés qui appartiennent à d’autres applications après avoir été [autorisé](~/android/app-fundamentals/permissions.md) par l’utilisateur.

Toutes les applications Android doivent déclarer une des deux autorisations pour le stockage externe dans le **AndroidManifest.xml** . Pour identifier les autorisations, une des deux suivantes `uses-permission` doit ajouter des éléments à **AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Si l’utilisateur accorde `WRITE_EXTERNAL_STORAGE`, puis `READ_EXTERNAL_STORAGE` est également implicitement accordée. Il n’est pas nécessaire de demander des autorisations à la fois dans **AndroidManifest.xml**.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Les autorisations peuvent également être ajoutées à l’aide de la **manifeste Android** onglet de la **propriétés de la solution**:

![Explorateur de solutions - autorisations requises pour Visual Studio 2017](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Les autorisations peuvent également être ajoutées à l’aide de la **manifeste Android** onglet de la **Panneau de propriétés de solution**:

[![Panneau de solutions, des autorisations requises pour Visual Studio pour Mac](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

En règle générale, toutes les autorisations dangereuses doivent être approuvées par l’utilisateur. Les autorisations pour le stockage externe sont une anomalie dans la mesure où il existe des exceptions à cette règle, selon la version d’Android utilisée par l’application est en cours d’exécution :

![Organigramme de vérifications d’autorisations de stockage externe](./images/external-permission-check-flowchart.png)

Pour plus d’informations sur l’exécution des demandes d’autorisation d’exécution, veuillez consulter le guide [autorisations dans Xamarin.Android](~/android/app-fundamentals/permissions.md). Le **monodroid-sample** [Fichiers_locaux](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) montre également une façon d’effectuer des vérifications d’autorisation de runtime.

#### <a name="granting-and-revoking-permissions-with-adb"></a>L’octroi et la révocation d’autorisations avec ADB

Au cours du développement d’une application Android, il peut être nécessaire d’accorder et révoquer des autorisations pour tester différents flux de travail impliqués dans les vérifications d’autorisation de runtime. Il est possible de le faire à l’invite de commandes à l’aide de ADB. Les extraits de ligne de commande suivants montrent comment accorder ou révoquer des autorisations à l’aide de ADB pour une application Android est dont le nom package **com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Suppression de fichiers

Un de la norme c# API peuvent être utilisées pour supprimer un fichier à partir d’un stockage externe, tel que [ `System.IO.File.Delete` ](xref:System.IO.File.Delete*). Il est également possible d’utiliser les API Java au détriment de la portabilité du code. Exemple :

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>Liens associés

* [Exemple de fichiers locaux de Xamarin.Android sur **monodroid-samples**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Autorisations dans Xamarin.Android](~/android/app-fundamentals/permissions.md)
