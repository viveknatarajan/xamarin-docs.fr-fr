---
title: Gestion des appareils virtuels avec Android Device Manager
description: Cet article décrit comment utiliser Android Device Manager pour créer et configurer des appareils virtuels Android (AVD) qui émulent des appareils physiques Android. Vous pouvez utiliser ces appareils virtuels pour exécuter et tester votre application sans avoir à dépendre d’un appareil physique.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a7c1aeafd94d7e2639617cda13312ee8a09e2c94
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935326"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Gestion des appareils virtuels avec Android Device Manager

_Cet article décrit comment utiliser Android Device Manager pour créer et configurer des appareils virtuels Android (AVD) qui émulent des appareils physiques Android. Vous pouvez utiliser ces appareils virtuels pour exécuter et tester votre application sans avoir à dépendre d’un appareil physique._

## <a name="overview"></a>Vue d'ensemble

Après avoir vérifié que l’accélération matérielle était activée (comme décrit dans [Accélération matérielle de l’émulateur Android](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), l’étape suivante consiste à utiliser _Android Device Manager_ (également appelé _Xamarin Android Device Manager_) afin de créer des appareils virtuels que vous pouvez utiliser pour tester et déboguer votre application.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Cet article explique comment utiliser Android Device Manager pour créer, dupliquer, personnaliser et lancer des appareils virtuels Android.

[![Capture d’écran d’Android Device Manager sous l’onglet Appareils](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Vous utilisez Android Device Manager pour créer et configurer des _appareils virtuels Android_ (AVD) que vous exécutez dans l’[émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md).
Chaque AVD est une configuration d’émulateur qui simule un appareil Android physique. Vous pouvez ainsi exécuter et tester votre application dans diverses configurations qui simulent différents appareils Android physiques.

## <a name="requirements"></a>Configuration requise

Pour utiliser Android Device Manager, vous avez besoin des éléments suivants :

-   Visual Studio 2017 version 15.7 ou version ultérieure est nécessaire. Les éditions Visual Studio Community, Professional et Enterprise sont prises en charge.

-   Visual Studio Tools pour Xamarin version 4.9 ou ultérieure.

-   Android SDK doit être installé (voir [Installation du kit Android SDK pour Xamarin.Android](~/android/get-started/installation/android-sdk.md)) et SDK Tools version 26.1.1 ou ultérieure doit être installé comme expliqué dans la section suivante. Veillez à installer le kit Android SDK à l’emplacement suivant (s’il n’est pas déjà installé) : **C:\\Program Files (x86)\\Android\\android-sdk**.


## <a name="launching-the-device-manager"></a>Lancement du Gestionnaire d’appareils

Lancez Android Device Manager à partir du menu **Outils** en cliquant sur **Outils > Android > Android Device Manager** :

[![Lancement à partir du menu Outils](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

Si la boîte de dialogue d’erreur suivante s’affiche au lancement, consultez [Résolution des problèmes de l’émulateur Android](~/android/get-started/installation/android-emulator/troubleshooting.md) afin d’obtenir des instructions de contournement :

![Erreur d’instance du kit Android SDK](device-manager-images/win/32-sdk-error.png)

Avant de pouvoir utiliser Android Device Manager, vous devez installer Android SDK Tools version 26.1.1 ou ultérieure. Si Android SDK Tools version 26.1.1 ou ultérieure n’est pas installé, la boîte de dialogue d’erreur suivante peut s’afficher au lancement :

![Boîte de dialogue d’erreur d’instance du kit Android SDK](device-manager-images/win/02-sdk-instance-error.png)

Si cette boîte de dialogue d’erreur s’affiche, cliquez sur **Ouvrir SDK Manager** pour ouvrir Android SDK Manager. Dans Android SDK Manager, cliquez sur l’onglet **Outils** et installez les éléments suivants :

-   **Android SDK Tools 26.1.1** ou ultérieur 
-   **Android SDK Platform-Tools {27.0.1}** ou ultérieur  
-   **Android SDK Build-Tools 27.0.3** ou ultérieur

Ces packages doivent être affichés avec l’état **Installé**, comme illustré dans la capture d’écran suivante :

[![Installation d’Android SDK Tools 27.0](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

Une fois ces packages installés, vous pouvez fermer le Gestionnaire du kit SDK et relancer le Gestionnaire d’appareils Android.

## <a name="main-screen"></a>Écran principal

Lorsque vous lancez pour la première fois le Gestionnaire d’appareils Android, un écran affichant tous les appareils virtuels actuellement configurés s’affiche. Pour chaque appareil, le **Nom**, le **Système d’exploitation** (niveau d’API Android), le **Processeur**, la taille de la **Mémoire** et la résolution d’écran sont affichés :

[![Liste des appareils installés avec leurs paramètres](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Lorsque vous cliquez sur un appareil dans la liste, le bouton **Démarrer** apparaît à droite. Vous pouvez cliquer sur le bouton **Démarrer** pour lancer l’émulateur avec cet appareil virtuel :

[![Bouton Démarrer d’une image d’appareil](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

Une fois que l’émulateur a démarré avec l’appareil virtuel sélectionné, le bouton **Démarrer** se transforme en bouton **Arrêter** que vous pouvez utiliser pour arrêter l’émulateur :

[![Bouton Arrêter de l’appareil en cours d’exécution](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Nouvel appareil

Pour créer un nouvel appareil, cliquez sur le bouton **Nouveau** (situé en haut à droite de l’écran) :

[![Bouton Nouveau pour la création d’un nouvel appareil](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

Lorsque vous cliquez sur **Nouveau**, l’écran **Nouvel appareil** s’affiche :

[![Écran Nouvel appareil du Gestionnaire d’appareils](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Pour configurer un nouvel appareil dans l’écran **Nouvel appareil**, effectuez les étapes suivantes :

1. Sélectionnez un appareil physique à émuler en cliquant sur le menu déroulant **Appareil** :

    [![Menu déroulant Appareil](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. Sélectionnez une image système à utiliser avec cet appareil virtuel en cliquant sur le menu déroulant **Image système**. Les images de gestionnaire d’appareils système installées sont affichées dans la section **Installé** de ce menu. La section **Télécharger** répertorie les images de gestionnaire d’appareils système qui ne sont actuellement pas disponibles sur votre ordinateur de développement, mais qui peuvent être installées automatiquement :

    [![Menu déroulant Image système](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. Donnez un nouveau nom à l’appareil. Dans l’exemple suivant, le nouvel appareil est nommé **Nexus 5 API 25** :

    [![Nommage du nouvel appareil](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. Modifiez les propriétés nécessaires. Pour apporter des modifications aux propriétés, consultez [Modification des propriétés d’appareil virtuel Android](~/android/get-started/installation/android-emulator/device-properties.md).

5. Ajoutez toute propriété supplémentaire que vous avez besoin de définir explicitement. L’écran **Nouvel appareil** répertorie uniquement les propriétés le plus souvent modifiées, mais vous pouvez cliquer sur le menu déroulant **Ajouter une propriété** (dans l’angle inférieur gauche) pour ajouter des propriétés supplémentaires. Dans l’exemple suivant, la propriété `hw.lcd.backlight` a été ajoutée :

    [![Menu déroulant Ajouter une propriété](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. Cliquez sur le bouton **Créer** (dans l’angle inférieur droit) pour créer le nouvel appareil :

    ![Bouton Créer](device-manager-images/win/14-create-button.png)

7. Un écran **Acceptation de la licence** est susceptible de s’afficher. Si vous êtes d’accord avec les termes du contrat de licence, cliquez sur **Accepter** :

    ![Écran Acceptation de la licence](device-manager-images/win/15-license-acceptance.png)

8. Android Device Manager ajoute le nouvel appareil à la liste des appareils virtuels installés et affiche un indicateur de progression **Création en cours** pendant la création de l’appareil :

    [![Indicateur de progression de la création](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. Lorsque le processus de création est terminé, le nouvel appareil apparaît dans la liste des appareils virtuels installés avec un bouton **Démarrer**, prêt à être lancé :

   [![Appareil nouvellement créé prêt à être lancé](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>Modifier l’appareil

Pour modifier un appareil virtuel existant, sélectionnez-le, puis cliquez sur le bouton **Modifier** (situé dans le coin supérieur droit de l’écran) :

[![Bouton Modifier pour modifier un nouvel appareil](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

Le fait de cliquer sur le bouton **Modifier** lance l’Éditeur d’appareil pour l’appareil virtuel sélectionné :

[![Écran Éditeur d’appareil](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

L’écran **Éditeur d’appareil** répertorie les propriétés de l’appareil virtuel dans la première colonne et les valeurs correspondantes de chaque propriété dans la seconde colonne. Lorsque vous sélectionnez une propriété, une description détaillée de cette propriété s’affiche à droite.

Par exemple, dans la capture d’écran suivante, la propriété `hw.lcd.density` est en cours de modification et passera de **420** à **240** :

[![Exemple de modification d’un appareil](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

Après avoir apporté les modifications de configuration nécessaires, cliquez sur le bouton **Enregistrer**.
Pour plus d’informations sur la modification des propriétés des appareils virtuels, consultez [Modification des propriétés d’appareil virtuel Android](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Options supplémentaires

Des options supplémentaires pour travailler avec les appareils sont disponibles dans le menu &hellip; situé dans le coin supérieur droit) :

[![Emplacement du menu Options supplémentaires](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

Le menu Options supplémentaires contient les éléments suivants :

-   **Dupliquer et modifier** &ndash; Duplique l’appareil sélectionné et l’ouvre dans l’écran **Nouvel appareil** avec un nom unique différent. Par exemple, si vous sélectionnez **VisualStudio_android-23_x86_phone** et cliquez sur **Dupliquer et modifier**, un compteur est ajouté au nom :

    [![Écran Dupliquer et modifier](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Révéler dans l’Explorateur** &ndash; Ouvre une fenêtre de l’Explorateur Windows dans le dossier contenant les fichiers de l’appareil virtuel. Par exemple, si vous sélectionnez **Nexus 5 X API 25** et cliquez sur **Révéler dans l’Explorateur**, une fenêtre similaire à celle-ci s’ouvre :

    [![Résultat du clic sur Afficher dans le Finder](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Réinitialisation aux paramètres d’usine** &ndash; Réinitialise l’appareil aux paramètres par défaut, en effaçant toutes les modifications apportées par l’utilisateur à l’état interne de l’appareil pendant son exécution (l’instantané [Quick Boot](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot), s’il existe, est également effacé). Cette modification n’altère pas les modifications que vous apportez à l’appareil virtuel lors de la création et de la modification. Une boîte de dialogue rappelant que la réinitialisation ne peut pas être annulée s’affiche. Cliquez sur **Effacer les données utilisateur** pour confirmer la réinitialisation.

-   **Supprimer** &ndash; Supprime définitivement l’appareil virtuel sélectionné.
    Une boîte de dialogue rappelant que la suppression d’un appareil ne peut pas être annulée s’affiche. Cliquez sur **Supprimer** si vous êtes certain que vous souhaitez supprimer l’appareil.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Cet article explique comment utiliser Android Device Manager pour créer, dupliquer, personnaliser et lancer des appareils virtuels Android.

[![Capture d’écran d’Android Device Manager sous l’onglet Appareils](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Ce guide s’applique uniquement à Visual Studio pour Mac.
Xamarin Studio n’est pas compatible avec Android Device Manager.

Vous utilisez Android Device Manager pour créer et configurer des *appareils virtuels Android* (AVD) que vous exécutez dans l’[émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md).
Chaque AVD est une configuration d’émulateur qui simule un appareil Android physique. Vous pouvez ainsi exécuter et tester votre application dans diverses configurations qui simulent différents appareils Android physiques.

## <a name="requirements"></a>Configuration requise

- Visual Studio pour Mac 7.5 ou version ultérieure.

- Android SDK 8.0 (API 26) ou ultérieur doit être installé par le biais d’Android SDK Manager.


## <a name="launching-the-device-manager"></a>Lancement du Gestionnaire d’appareils

Lancez Android Device Manager en cliquant sur **Outils > Device Manager** :

[![Lancement à partir du menu Outils](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Avant de pouvoir utiliser Android Device Manager, vous devez installer Android SDK Tools version 26.0.2 ou ultérieure. Si Android SDK Tools version 26.0.2 ou ultérieure n’est pas installé, la boîte de dialogue d’erreur suivante s’affiche au lancement :

![Boîte de dialogue d’erreur d’instance du kit Android SDK](device-manager-images/mac/02-sdk-instance-error.png)

Si cette boîte de dialogue d’erreur s’affiche, cliquez sur **OK** pour ouvrir le Gestionnaire du kit Android SDK. Dans Android SDK Manager, cliquez sur l’onglet **Outils** et installez les éléments suivants :

-   **Android SDK Tools 26.0.2** ou ultérieur 
-   **Android SDK Platform-Tools 26.0.0** ou ultérieur 
-   **Android SDK Build-Tools 26.0.0** ou ultérieur

Ces packages doivent être affichés avec l’état **Installé**, comme illustré dans la capture d’écran suivante :

[![Installation d’Android SDK Tools 26.0](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>Écran principal

Lorsque vous lancez pour la première fois le Gestionnaire d’appareils Android, un écran affichant tous les appareils virtuels actuellement configurés s’affiche. Pour chaque appareil, le **Nom**, l’**Image système** (niveau d’API Android), le **Processeur**, la taille de la **Mémoire** et la résolution d’écran sont affichés :

[![Liste des appareils installés avec leurs paramètres](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Cliquez sur le bouton **Lire** pour lancer l’émulateur avec l’appareil virtuel de votre choix :

[![Bouton Démarrer d’une image d’appareil](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

Une fois que l’émulateur a démarré avec l’appareil virtuel sélectionné, le bouton **Lire** se transforme en bouton **Arrêter** que vous pouvez utiliser pour arrêter l’émulateur :

[![Bouton Arrêter de l’appareil en cours d’exécution](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>Nouvel appareil

Pour créer un nouvel appareil, cliquez sur le bouton **Nouvel appareil** (situé en haut à droite de l’écran) :

[![Bouton Nouveau pour la création d’un nouvel appareil](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

Lorsque vous cliquez sur **Nouvel appareil**, l’écran **Nouvel appareil** s’affiche :

[![Écran Nouvel appareil du Gestionnaire d’appareils](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

Pour configurer un nouvel appareil dans l’écran **Nouvel appareil**, effectuez les étapes suivantes :

1. Sélectionnez un appareil physique à émuler en cliquant sur le menu déroulant **Appareil** :

    [![Menu déroulant Appareil](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. Sélectionnez une image système à utiliser avec cet appareil virtuel en cliquant sur le menu déroulant **Image système**. Les images de gestionnaire d’appareils système installées sont affichées dans la section **Installé** de ce menu. La section **Télécharger** (si elle est présente) répertorie les images de gestionnaire d’appareils système qui ne sont actuellement pas disponibles sur votre ordinateur de développement, mais qui peuvent être installées automatiquement :

    [![Menu déroulant Image système](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Donnez un nouveau nom à l’appareil. Dans l’exemple suivant, le nouvel appareil est nommé **Nexus 5X API 25** :

    [![Nommage du nouvel appareil](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. Modifiez les propriétés nécessaires. Pour apporter des modifications aux propriétés, consultez [Modification des propriétés d’appareil virtuel Android](~/android/get-started/installation/android-emulator/device-properties.md).

5. Ajoutez toute propriété supplémentaire que vous avez besoin de définir explicitement. L’écran **Nouvel appareil** répertorie uniquement les propriétés le plus souvent modifiées, mais vous pouvez cliquer sur le menu déroulant **Ajouter une propriété** (dans l’angle inférieur gauche) pour ajouter des propriétés supplémentaires :

    [![Menu déroulant Ajouter une propriété](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Vous pouvez également cliquer sur **Personnalisé** afin de définir une nouvelle propriété pour l’appareil :

    ![Bouton Créer](device-manager-images/mac/14-custom-button.png)

7. Cliquez sur le bouton **Créer** (dans l’angle inférieur droit) pour créer le nouvel appareil :

    ![Bouton Créer](device-manager-images/mac/15-create-button.png)

8. Un écran **Acceptation de la licence** est susceptible de s’afficher. Si vous êtes d’accord avec les termes du contrat de licence, cliquez sur **Accepter**.

9. Android Device Manager ajoute le nouvel appareil à la liste des appareils virtuels installés et affiche un indicateur de progression **Création en cours** pendant la création de l’appareil :

    [![Indicateur de progression de la création](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Lorsque le processus de création est terminé, le nouvel appareil apparaît dans la liste des appareils avec un bouton **Lire**, prêt à être lancé :

   [![Appareil nouvellement créé prêt à être lancé](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>Modifier l’appareil

Pour modifier un appareil virtuel existant, sélectionnez le menu déroulant **Options supplémentaires** (icône d’engrenage) et sélectionnez **Modifier** :
 
[![Sélection du bouton Modifier pour modifier un nouvel appareil](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

Le fait de cliquer sur le bouton **Modifier** lance l’Éditeur d’appareil pour l’appareil virtuel sélectionné :
 
[![Écran Éditeur d’appareil](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

L’écran **Éditeur d’appareil** répertorie les propriétés de l’appareil virtuel dans la première colonne et les valeurs correspondantes de chaque propriété dans la seconde colonne. Lorsque vous sélectionnez une propriété, une description détaillée de cette propriété s’affiche à droite.

Par exemple, dans la capture d’écran suivante, la propriété `hw.lcd.density` est en cours de modification et passera de **320** à **240** : la propriété `hw.ramSize` passera quant à elle à **768** :
 
[![Exemple de modification d’un appareil](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

Après avoir apporté les modifications de configuration nécessaires, cliquez sur le bouton **Enregistrer**.
Pour plus d’informations sur la modification des propriétés des appareils virtuels, consultez [Modification des propriétés d’appareil virtuel Android](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Options supplémentaires

Des options supplémentaires pour travailler avec un appareil sont disponibles dans le menu déroulant situé à gauche du bouton **Lire** :

[![Emplacement du menu Options supplémentaires](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

Le menu Options supplémentaires contient les éléments suivants :

-   **Modifier** &ndash; Ouvre l’appareil sélectionné dans l’éditeur d’appareil, comme décrit précédemment.

-   **Dupliquer et modifier** &ndash; Duplique l’appareil sélectionné et l’ouvre dans l’écran **Nouvel appareil** avec un nom unique différent.
    Par exemple, si vous sélectionnez **Nexus 5X API 25** et cliquez sur **Dupliquer et modifier**, un compteur est ajouté au nom :

    [![Écran Duplicate et modifier](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Révéler dans l’Explorateur** &ndash; Ouvre une fenêtre Finder macOS dans le dossier contenant les fichiers de l’appareil virtuel. Par exemple, si vous sélectionnez **Nexus 5 X API 25** et cliquez sur **Afficher dans le Finder**, une fenêtre similaire à celle-ci s’ouvre :

    [![Résultat du clic sur Afficher dans le Finder](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Réinitialisation aux paramètres d’usine** &ndash; Réinitialise l’appareil aux paramètres par défaut, en effaçant toutes les modifications apportées par l’utilisateur à l’état interne de l’appareil pendant son exécution. Cette modification n’altère pas les modifications que vous apportez à l’appareil virtuel lors de la création et de la modification. Une boîte de dialogue rappelant que la réinitialisation ne peut pas être annulée s’affiche. Cliquez sur **Effacer les données utilisateur** pour confirmer la réinitialisation.

-   **Supprimer** &ndash; Supprime définitivement l’appareil virtuel sélectionné.
    Une boîte de dialogue rappelant que la suppression d’un appareil ne peut pas être annulée s’affiche. Cliquez sur **Supprimer** si vous êtes certain que vous souhaitez supprimer l’appareil.

-----

## <a name="troubleshooting"></a>Résolution des problèmes

Les sections suivantes expliquent comment diagnostiquer et résoudre les problèmes qui peuvent se produire quand vous utilisez Android Device Manager pour configurer les appareils virtuels.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Kit Android SDK dans un emplacement non standard

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

### <a name="snapshot-disables-wifi-on-android-oreo"></a>La capture instantanée désactive le Wi-Fi sur Android Oreo

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

### <a name="snapshot-disables-wifi-on-android-oreo"></a>La capture instantanée désactive le Wi-Fi sur Android Oreo

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

### <a name="generating-a-bug-report"></a>Génération d’un rapport de bogues

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Si vous rencontrez un problème avec Android Device Manager qui ne peut pas être résolu à l’aide des conseils de résolution des problèmes ci-dessus, veuillez soumettre un rapport de bogue en cliquant sur la barre de titre et en sélectionnant **Générer un rapport de bogue** :

[![Emplacement de l’élément de menu pour soumettre un rapport de bogues](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Si vous rencontrez un problème avec Android Device Manager qui ne peut pas être résolu à l’aide des conseils de dépannage ci-dessus, veuillez remplir un rapport de bogues en cliquant sur **Aide > Générer un rapport de bogues** :

[![Emplacement de l’élément de menu pour soumettre un rapport de bogues](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)


-----

## <a name="summary"></a>Récapitulatif

Dans ce guide, vous avez découvert Android Device Manager disponible dans Visual Studio pour Mac et Visual Studio Tools pour Xamarin. Les fonctionnalités essentielles vous ont été expliquées, comme le démarrage et l’arrêt de l’émulateur Android, la sélection d’un appareil virtuel Android (AVD) à exécuter, la création de nouveaux appareils virtuels et la manière de modifier un appareil virtuel. Ce guide a expliqué comment modifier les propriétés matérielles du profil pour une personnalisation plus poussée et a fourni des conseils de dépannage pour les problèmes courants.


## <a name="related-links"></a>Liens associés

- [Modifications des outils Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Débogage sur l’émulateur Android](~/android/deploy-test/debugging/debug-on-emulator.md)
- [Notes de publication de SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
