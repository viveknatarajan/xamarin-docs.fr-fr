---
title: Empaquetage d’applications Wear
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 585c276b327a9092bdd13fa633307477017558c5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61276791"
---
# <a name="packaging-wear-apps"></a>Empaquetage d’applications Wear

Les applications Android Wear sont empaquetées avec une application Android complète pour la distribution sur Google Play. 

## <a name="automatic-packaging"></a>Mise en package automatique

Votre application Wear à compter de Xamarin Android 5.0, est automatiquement empaquetée en tant que ressource dans votre application de poche lorsque vous créez une référence de projet à partir du projet de poche au projet usure. Vous pouvez utiliser les étapes suivantes pour créer cette association : 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Si votre application Wear n’est pas déjà partie de votre solution de poche, cliquez sur le nœud de solution et sélectionnez **Ajouter > Ajouter un projet existant...** .

2. Accédez à la **.csproj** fichier de votre application Wear, sélectionnez-le, puis cliquez sur **Open**. Le projet d’application Wear doit maintenant être visible dans votre solution de poche.

3. Cliquez sur le **références** nœud et sélectionnez **ajouter une référence**.

4. Dans le **Gestionnaire de références** boîte de dialogue, d’activer votre projet d’usure (cliquez pour ajouter une case à cocher), puis cliquez sur **OK**.

5. Modifier le nom du package pour votre projet d’usure afin qu’il corresponde au nom du package du projet poche (le nom du package peut être modifié sous **Propriétés > manifeste Android**).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Si votre application Wear n’est pas déjà partie de votre solution de poche, cliquez sur le nœud de solution et sélectionnez **Ajouter > Ajouter un projet existant...** .

2. Accédez à la **.csproj** fichier de votre application Wear, sélectionnez-le, puis cliquez sur **Open**. Le projet d’application Wear doit maintenant être visible dans votre solution de poche.

3. Cliquez sur le nœud de projet de poche dans la solution et cliquez sur **modifier les références...** .

4. Dans le **modifier les références** boîte de dialogue, d’activer votre projet d’usure (cliquez pour ajouter une case à cocher), puis cliquez sur **OK**.

5. Modifier le nom du package pour votre projet d’usure afin qu’il corresponde au nom du package du projet poche (le nom du package peut être modifié sous **Options du projet > Application Android**).

-----


Notez que vous obtenez un **XA5211** erreur si le nom du package de l’application de l’usure ne correspond pas le nom du package de l’application de poche. Exemple :

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Pour corriger cette erreur, modifiez le nom du package de l’application de l’usure afin qu’il corresponde au nom de package de l’application de poche.

Lorsque vous cliquez sur **Générer > générer tout**, cette association déclenche la mise en package automatique de l’usure du projet dans le projet de périphérique portable (téléphone) principal. L’usure application est automatiquement créée et incluse en tant que ressource dans l’application de poche.

L’assembly qui génère le projet d’application Wear n’est pas utilisé comme une référence d’assembly dans le projet de périphérique portable (téléphone). Au lieu de cela, le processus de génération effectue les opérations suivantes :

-   Vérifie que le package noms correspondent. 

-   Génère du code XML et l’ajoute au projet portable pour l’associer à l’application de l’usure. Exemple : 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Ajoute l’application Wear comme un **brutes** ressource pour le projet de poche. 


## <a name="manual-packaging"></a>Empaquetage manuel

Vous pouvez écrire des applications Android Wear dans Xamarin.Android avant la version 5.0, mais vous devez suivre ces instructions d’empaquetage manuel pour distribuer l’application : 

1. Assurez-vous que votre projet portable et vos projets de poche (téléphone) ont la même version numéro et le nom package.

2. Créer manuellement le projet portable comme un **version** générer.

3. Ajouter manuellement la version **. APK** de l’étape (2) dans le **brut/ressources** répertoire du projet (téléphone) de l’ordinateur de poche.

4. Ajouter manuellement une nouvelle ressource XML **Resources/xml/wearable_app_desc.xml** dans le projet portable qui fait référence à portable **APK** à partir de l’étape (3) :

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Ajouter manuellement un `<meta-data />` élément de projet poche **AndroidManifest.xml** `<application>` élément fait référence à la nouvelle ressource XML :

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Consultez également le site développeur Android [emballage manuelle instructions](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

