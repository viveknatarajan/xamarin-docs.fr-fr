---
title: Résolution des problèmes d’installation d’émulateur
description: Cet article explique comment diagnostiquer et résoudre les problèmes qui peuvent se produire quand vous utilisez Android Device Manager.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733651"
---
# <a name="troubleshooting-emulator-setup-problems"></a>Résolution des problèmes d’installation d’émulateur

_Cet article explique comment diagnostiquer et résoudre les problèmes qui peuvent se produire quand vous utilisez Android Device Manager pour configurer l’émulateur Android. Pour plus d’informations sur la résolution des problèmes lors de l’exécution de l’émulateur Android, consultez [Résolution des problèmes de l’émulateur Google Android](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)._

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>Kit Android SDK dans un emplacement non standard

En règle générale, le kit Android SDK est installé à l’emplacement suivant :

**C:\\Program Files (x86)\\Android\\android-sdk**

Si le SDK n’est pas installé à cet emplacement, cette erreur peut s’afficher quand vous lancez Android Device Manager :

![Erreur d’instance du kit Android SDK](troubleshooting-images/win/01-sdk-error.png)

Pour contourner ce problème, effectuez les étapes suivantes :

1. À partir du bureau Windows, accédez à **C:\\Utilisateurs\\*nom d’utilisateur*\\AppData\\Roaming\\XamarinDeviceManager** :

    ![Emplacement du fichier journal d’Android Device Manager](troubleshooting-images/win/02-log-files.png)

2. Double-cliquez pour ouvrir l’un des fichiers journaux et recherchez le **chemin du fichier config**. Exemple :

    [![Chemin du fichier config dans le fichier journal](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. Accédez à cet emplacement et double-cliquez sur **user.config** pour l’ouvrir. 

4. Dans **user.config**, recherchez l’élément **&lt;UserSettings&gt;** et ajoutez-lui un attribut **AndroidSdkPath**. Définissez cet attribut sur le chemin d’installation du kit Android SDK sur votre ordinateur et enregistrez le fichier. Par exemple, l’élément **&lt;UserSettings&gt;** se présente de la manière suivante si le kit Android SDK a été installé dans **C:\\Programmes\\Android\\SDK** :
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Après avoir apporté cette modification à **user.config**, vous devriez pouvoir lancer Android Device Manager.

## <a name="snapshot-disables-wifi-on-android-oreo"></a>La capture instantanée désactive le Wi-Fi sur Android Oreo

Si vous avez un AVD configuré pour Android Oreo avec un accès Wi-Fi simulé, le redémarrage de l’AVD après une capture instantanée peut désactiver l’accès Wi-Fi.

Pour éviter ce problème,

1. Sélectionnez l’AVD dans Android Device Manager.

2. Dans le menu des options supplémentaires, cliquez sur **Révéler dans l’Explorateur**.

3. Accédez à **capture instantanée > default_boot**.

4. Supprimer le fichier **snapshot.pb** :

    ![Emplacement du fichier snapshot.pb](troubleshooting-images/win/05-delete-snapshot.png)

5. Redémarrez l'AVD. 

Une fois ces modifications apportées, l’AVD sera redémarré dans un état qui permet au Wi-Fi de fonctionner à nouveau.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>La capture instantanée désactive le Wi-Fi sur Android Oreo

Si vous avez un AVD configuré pour Android Oreo avec un accès Wi-Fi simulé, le redémarrage de l’AVD après une capture instantanée peut désactiver l’accès Wi-Fi.

Pour éviter ce problème,

1. Sélectionnez l’AVD dans Android Device Manager.

2. Dans le menu des options supplémentaires, cliquez sur **Révéler dans le Finder**.

3. Accédez à **capture instantanée > default_boot**.

4. Supprimer le fichier **snapshot.pb** :

    [![Emplacement du fichier snapshot.pb](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. Redémarrez l'AVD. 

Une fois ces modifications apportées, l’AVD sera redémarré dans un état qui permet au Wi-Fi de fonctionner à nouveau.


-----

## <a name="generating-a-bug-report"></a>Génération d’un rapport de bogues

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Si vous rencontrez un problème avec Android Device Manager qui ne peut pas être résolu à l’aide des conseils de résolution des problèmes ci-dessus, veuillez soumettre un rapport de bogue en cliquant sur la barre de titre et en sélectionnant **Générer un rapport de bogue** :

[![Emplacement de l’élément de menu pour soumettre un rapport de bogues](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Si vous rencontrez un problème avec Android Device Manager qui ne peut pas être résolu à l’aide des conseils de dépannage ci-dessus, veuillez remplir un rapport de bogues en cliquant sur **Aide > Générer un rapport de bogues** :

[![Emplacement de l’élément de menu pour soumettre un rapport de bogues](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
