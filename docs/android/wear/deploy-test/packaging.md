---
title: Empaquetage d’applications usure
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: af96c0f8cf862b7a208beb5b91ecbb30598b09d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="packaging-wear-apps"></a>Empaquetage d’applications usure

Les applications Android usure sont empaquetées avec une application Android complète pour la distribution sur Google Play. 

## <a name="automatic-packaging"></a>Mise en package automatique

Votre application d’usure à partir de Xamarin Android 5.0, est automatiquement empaquetée en tant que ressource dans votre application de poche lorsque vous créez une référence de projet à partir du projet de poche au projet d’usure. Vous pouvez utiliser les étapes suivantes pour créer cette association : 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Si votre application d’usure n’est pas déjà partie de votre solution de poche, cliquez sur le nœud de la solution et sélectionnez **Ajouter > Ajouter un projet existant...** .

2. Accédez à la **.csproj** fichier de votre application d’usure, sélectionnez-le, puis cliquez sur **ouvrir**. Le projet d’application usure doit maintenant être visible dans votre solution de poche.

3. Cliquez sur le **références** nœud et sélectionnez **ajouter une référence**.

4. Dans le **Gestionnaire de références** boîte de dialogue, activez votre projet d’usure (cliquez pour ajouter une coche), puis cliquez sur **OK**.

5. Modifier le nom du package pour votre projet d’usure afin qu’il corresponde au nom de package de l’ordinateur de poche projet (le nom du package peut être modifié sous **Propriétés > manifeste Android**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Si votre application d’usure n’est pas déjà partie de votre solution de poche, cliquez sur le nœud de la solution et sélectionnez **Ajouter > Ajouter un projet existant...** .

2. Accédez à la **.csproj** fichier de votre application d’usure, sélectionnez-le, puis cliquez sur **ouvrir**. Le projet d’application usure doit maintenant être visible dans votre solution de poche.

3. Cliquez sur le nœud du projet de poche dans la solution et cliquez sur **modifier les références...** .

4. Dans le **modifier les références** boîte de dialogue, activez votre projet d’usure (cliquez pour ajouter une coche), puis cliquez sur **OK**.

5. Modifier le nom du package pour votre projet d’usure afin qu’il corresponde au nom de package de l’ordinateur de poche projet (le nom du package peut être modifié sous **Options du projet > Application Android**).

-----


Notez que vous obtenez un **XA5211** erreur si le nom du package de l’application d’usure ne correspond pas le nom du package de l’application de poche. Par exemple :

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Pour corriger cette erreur, modifiez le nom du package de l’application d’usure afin qu’il corresponde au nom de package de l’application de poche.

Lorsque vous cliquez sur **Générer > générer tout**, cette association déclenche emballage automatique du projet usure dans le projet principal (téléphone) de l’ordinateur de poche. L’application d’usure est automatiquement générée et incluse en tant que ressource dans l’application de poche.

L’assembly que le projet d’application usure génère n’est pas utilisé comme une référence d’assembly dans le projet de l’ordinateur de poche (Phone). Au lieu de cela, le processus de génération effectue les opérations suivantes :

-   Vérifie que le package noms correspondent. 

-   Génère du code XML et l’ajoute au projet pour l’associer à l’application d’usure portable. Par exemple : 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Ajoute l’application d’usure comme un **brutes** ressource pour le projet de poche. 


## <a name="manual-packaging"></a>Empaquetage manuel

Vous pouvez écrire des applications d’usure Android dans Xamarin.Android avant la version 5.0, mais vous devez suivre ces instructions Empaquetage manuel pour distribuer l’application : 

1. Assurez-vous que vos projets de l’ordinateur de poche (Phone) et le projet portable ont la même version numéro et nom de package.

2. Créer manuellement le projet portable comme un **version** générer.

3. Ajouter manuellement la version **. APK** à partir de l’étape (2) dans le **ressources/raw** répertoire du projet (téléphone) de l’ordinateur de poche.

4. Ajouter manuellement une ressource XML **Resources/xml/wearable_app_desc.xml** dans le projet de poche qui fait référence à portable **APK** à partir de l’étape (3) :

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Ajouter manuellement un `<meta-data />` élément au projet poche **AndroidManifest.xml** `<application>` élément fait référence à la nouvelle ressource XML :

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Consultez également le site de développement Android [instructions du manuel emballage](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

