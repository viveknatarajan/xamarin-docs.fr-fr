---
title: Gestionnaire d’appareils Android Xamarin
description: Le Gestionnaire d’appareils Android Xamarin, actuellement en préversion, remplace le Gestionnaire d’appareils hérité de Google. Ce guide explique comment utiliser le Gestionnaire d’appareils Android Xamarin pour créer et configurer des appareils virtuels Android qui émulent des appareils Android. Vous pouvez utiliser ces appareils virtuels pour exécuter et tester votre application sans avoir à dépendre d’un appareil physique.
ms.topic: article
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: 01fb21729e919872935fd63af28a13642a11fa4b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="xamarin-android-device-manager"></a>Gestionnaire d’appareils Android Xamarin

_Le Gestionnaire d’appareils Android Xamarin, actuellement en préversion, remplace le Gestionnaire d’appareils hérité de Google. Ce guide explique comment utiliser le Gestionnaire d’appareils Android Xamarin pour créer et configurer des appareils virtuels Android qui émulent des appareils Android. Vous pouvez utiliser ces appareils virtuels pour exécuter et tester votre application sans avoir à dépendre d’un appareil physique._

![Actuellement en préversion](~/media/shared/preview.png)

 
## <a name="overview"></a>Vue d'ensemble

Après avoir vérifié que l’accélération matérielle est activée (comme décrit dans [Accélération matérielle](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), l’étape suivante consiste à créer des appareils virtuels en vue de tester et de déboguer votre application. Vous pouvez utiliser le Gestionnaire d’appareils Android Xamarin pour créer des appareils virtuels qui seront utilisés par l’émulateur du kit Android SDK.

Pourquoi voudriez-vous utiliser le Gestionnaire d’appareils Android Xamarin à la place du [Gestionnaire d’appareils Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md) ?
À compter de la version 26.0.1 d’Android SDK Tools, Google ne prend plus en charge le Gestionnaire d’AVD et le Gestionnaire du kit SDK basés sur l’interface utilisateur au profit de ses nouveaux outils d’interface de ligne de commande. En raison de ce changement, vous devez utiliser le [Gestionnaire Xamarin SDK](~/android/get-started/installation/android-sdk.md) et le Gestionnaire d’appareils Android Xamarin lorsque vous effectuez la mise à niveau vers Android SDK Tools 26.0.1 et version ultérieure (requis pour le développement Android 8.0 Oreo).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ce guide explique comment installer et utiliser le Gestionnaire d’appareils Android Xamarin pour Visual Studio sur Windows (ou [pour Mac](?tabs=vsmac)) :

[![Capture d’écran du Gestionnaire d’appareils Android Xamarin sous l’onglet Appareils](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ce guide explique comment installer et utiliser le Gestionnaire d’appareils Android Xamarin pour Visual Studio pour Mac (ou [sur Windows](?tabs=vswin)) :

[![Capture d’écran du Gestionnaire d’appareils Android Xamarin sous l’onglet Appareils](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Ce guide s’applique uniquement à Visual Studio pour Mac.
Xamarin Studio n’est pas compatible avec le Gestionnaire d’appareils Android Xamarin.

-----

Vous utilisez le Gestionnaire d’appareils Android Xamarin pour créer et configurer des *appareils virtuels Android* (AVD) que vous exécutez dans l’[émulateur du kit Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Chaque AVD est une configuration d’émulateur qui simule un appareil Android physique. Vous pouvez ainsi exécuter et tester votre application dans diverses configurations qui simulent différents appareils Android physiques. Le Gestionnaire d’appareils Android Xamarin remplace le Gestionnaire d’appareils virtuels Android autonome de Google (qui a été déprécié).

Dans ce guide, vous allez apprendre à installer et démarrer le Gestionnaire d’appareils Android. Vous allez apprendre à créer, dupliquer, personnaliser et lancer des appareils virtuels. Ce guide explique également comment configurer les propriétés de chaque appareil virtuel (comme le niveau d’API, le processeur, la mémoire et la résolution), activer ou désactiver les capteurs simulés (comme l’accéléromètre, le GPS, le capteur d’orientation et le capteur de lumière) et configurer le type d’accélération matérielle utilisé par cet appareil virtuel.

## <a name="requirements"></a>Configuration requise

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour utiliser le Gestionnaire d’appareils Android Xamarin, vous avez besoin des éléments suivants :

-   Visual Studio 2017 version 15.5 ou version ultérieure est requis. Visual Studio Community Edition et versions supérieures sont pris en charge.

-   Xamarin pour Visual Studio version 4.8 ou version ultérieure. Pour plus d’informations sur la mise à jour de Xamarin, consultez [Modifier le canal de mise à jour](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).

-   La dernière version du [programme d’installation du Gestionnaire d’appareils Xamarin](https://go.microsoft.com/fwlink/?linkid=865528) pour Windows.

-   **Kit Android SDK** &ndash; Le kit Android SDK doit être installé (voir [Installation du kit Android SDK](~/android/get-started/installation/android-sdk.md)) et SDK Tools version 26.0 doit être installé comme expliqué dans la section suivante. Veillez à installer le kit Android SDK à l’emplacement suivant (s’il n’est pas déjà installé) : **C:\\Program Files (x86)\\Android\\android-sdk**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

-   Visual Studio pour Mac 7.4 ou version ultérieure.

-   La dernière version du [programme d’installation du Gestionnaire d’appareils Xamarin](https://go.microsoft.com/fwlink/?linkid=865527) pour macOS.

-   **Kit Android SDK** &ndash; Android SDK 8.0 (API 26) ou version ultérieure doit être installé via le Gestionnaire du kit SDK.

-----

 
## <a name="installing-the-device-manager"></a>Installation du Gestionnaire d’appareils

Effectuez les étapes suivantes pour installer le Gestionnaire d’appareils Android Xamarin :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Téléchargez le [programme d’installation du Gestionnaire d’appareils Xamarin](https://go.microsoft.com/fwlink/?linkid=865528) pour Windows.

2. Double-cliquez sur **Xamarin.DeviceManager.msi** et suivez les instructions d’installation : 

    ![Assistant Installation du Gestionnaire d’appareils Android Xamarin](xamarin-device-manager-images/win/30-installer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Téléchargez le [programme d’installation du Gestionnaire d’appareils Xamarin](https://go.microsoft.com/fwlink/?linkid=865527) pour macOS.

2. Double-cliquez sur **AndroidDevices.pkg** et suivez les instructions d’installation : 

    [![Assistant Installation du Gestionnaire d’appareils Android Xamarin](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png#lightbox)

-----

 
## <a name="launching-the-device-manager"></a>Lancement du Gestionnaire d’appareils

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dans Visual Studio 15.6 Preview 3 et versions ultérieures, vous pouvez lancer le Gestionnaire d’appareils Android Xamarin à partir du menu **Outils**. Si vous utilisez Visual Studio 15.6 Preview 3 ou version ultérieure, démarrez le Gestionnaire d’appareils en cliquant sur **Outils > Gestionnaire d’émulateur Android** :

[![Lancement à partir du menu Outils](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

Si vous utilisez une version antérieure de Visual Studio, le Gestionnaire d’appareils Android Xamarin doit être lancé à partir du menu **Démarrer** de Windows.

![Gestionnaire d’appareils Android Xamarin dans le menu Démarrer](xamarin-device-manager-images/win/31-start-menu.png)

Cliquez avec le bouton droit sur **Gestionnaire d’appareils Android Xamarin** et sélectionnez **Plus > Exécuter en tant qu’administrateur**. Si la boîte de dialogue d’erreur suivante s’affiche au lancement, consultez la section [Résolution des problèmes](#troubleshooting) afin d’obtenir des instructions de contournement :

![Erreur d’instance du kit Android SDK](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans Visual Studio pour Mac 7.6 Preview 3 (actuellement dans le canal alpha) ou version ultérieure, vous pouvez lancer le Gestionnaire d’appareils Android Xamarin en sélectionnant **Outils > Gestionnaire d’émulateur** :

[![Lancement à partir du menu Outils](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

Si vous utilisez une version antérieure de Visual Studio pour Mac, le Gestionnaire d’appareils Android Xamarin doit être lancé indépendamment. Recherchez **Appareils Android** dans le dossier **Applications** et double-cliquez dessus pour le lancer :

[![Emplacement du Gestionnaire d’appareils Android Xamarin dans le Finder](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png#lightbox)


-----

Avant de pouvoir utiliser le Gestionnaire d’appareils Android, vous devez installer Android SDK Tools version 26.0.0 ou ultérieure. Si Android SDK Tools version 26.0.0 ou ultérieure n’est pas installé, la boîte de dialogue d’erreur suivante s’affiche au lancement :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Boîte de dialogue d’erreur d’instance du kit Android SDK](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Boîte de dialogue d’erreur d’instance du kit Android SDK](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

Si cette boîte de dialogue d’erreur s’affiche, cliquez sur **OK** pour ouvrir le Gestionnaire du kit Android SDK. Dans le Gestionnaire du kit Android SDK, cliquez sur l’onglet **Outils** et installez **Android SDK Tools 26.0.2** ou version ultérieure, **Android SDK Platform-Tools 26.0.0** ou version ultérieure et  **Android SDK Build-Tools 26.0.0** (ou version ultérieure) :


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Installation d’Android SDK Tools 26.0](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

Une fois ces packages installés, vous pouvez fermer le Gestionnaire du kit SDK et relancer le Gestionnaire d’appareils Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Installation d’Android SDK Tools 26.0](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

 
## <a name="main-screen"></a>Écran principal

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lorsque vous lancez pour la première fois le Gestionnaire d’appareils Android, un écran affichant tous les appareils virtuels actuellement configurés s’affiche. Pour chaque appareil, le **Nom**, le **Système d’exploitation** (niveau d’API Android), le **Processeur**, la taille de la **Mémoire** et la résolution d’écran sont affichés :

[![Liste des appareils installés avec leurs paramètres](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lorsque vous lancez pour la première fois le Gestionnaire d’appareils Android, un écran affichant tous les appareils virtuels actuellement configurés s’affiche. Pour chaque appareil, le **Nom**, l’**Image système** (niveau d’API Android), le **Processeur**, la taille de la **Mémoire** et la résolution d’écran sont affichés :

[![Liste des appareils installés avec leurs paramètres](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lorsque vous cliquez sur un appareil dans la liste, le bouton **Démarrer** apparaît à droite. Vous pouvez cliquer sur le bouton **Démarrer** pour lancer l’émulateur avec cet appareil virtuel :

[![Bouton Démarrer d’une image d’appareil](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Cliquez sur le bouton **Lire** pour lancer l’émulateur avec l’appareil virtuel de votre choix :
 
[![Bouton Démarrer d’une image d’appareil](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Une fois que l’émulateur a démarré avec l’appareil virtuel sélectionné, le bouton **Démarrer** se transforme en bouton **Arrêter** que vous pouvez utiliser pour arrêter l’émulateur :

[![Bouton Arrêter de l’appareil en cours d’exécution](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Une fois que l’émulateur a démarré avec l’appareil virtuel sélectionné, le bouton **Lire** se transforme en bouton **Arrêter** que vous pouvez utiliser pour arrêter l’émulateur :
 
[![Bouton Arrêter de l’appareil en cours d’exécution](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)
 
-----

 
### <a name="new-device"></a>Nouvel appareil

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour créer un nouvel appareil, cliquez sur le bouton **Nouveau** (situé en haut à droite de l’écran) :

[![Bouton Nouveau pour la création d’un nouvel appareil](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour créer un nouvel appareil, cliquez sur le bouton **Nouvel appareil** (situé en haut à droite de l’écran) :
 
[![Bouton Nouveau pour la création d’un nouvel appareil](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lorsque vous cliquez sur **Nouveau**, l’écran **Nouvel appareil** s’affiche :

[![Écran Nouvel appareil du Gestionnaire d’appareils](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

Pour configurer un nouvel appareil dans l’écran **Nouvel appareil**, effectuez les étapes suivantes :

1. Sélectionnez un appareil physique à émuler en cliquant sur le menu déroulant **Appareil** :

    [![Menu déroulant Appareil](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. Sélectionnez une image système à utiliser avec cet appareil virtuel en cliquant sur le menu déroulant **Image système**. Les images système installées sont affichées dans la section **Installé** de ce menu. La section **Télécharger** répertorie les images système qui ne sont actuellement pas disponibles sur votre ordinateur de développement, mais qui peuvent être installées automatiquement :

    [![Menu déroulant Image système](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. Donnez un nouveau nom à l’appareil. Dans l’exemple suivant, le nouvel appareil est nommé **Nexus 5 API 25** :

    [![Nommage du nouvel appareil](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. Modifiez les propriétés nécessaires. Pour apporter des modifications aux propriétés, consultez [Propriétés de profil](#properties) plus loin dans ce guide.

5. Ajoutez toute propriété supplémentaire que vous avez besoin de définir explicitement. L’écran **Nouvel appareil** répertorie uniquement les propriétés le plus souvent modifiées, mais vous pouvez cliquer sur le menu déroulant **Ajouter une propriété** (dans l’angle inférieur gauche) pour ajouter des propriétés supplémentaires. Dans l’exemple suivant, la propriété `hw.lcd.backlight` a été ajoutée :

    [![Menu déroulant Ajouter une propriété](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. Cliquez sur le bouton **Créer** (dans l’angle inférieur droit) pour créer le nouvel appareil :

    ![Bouton Créer](xamarin-device-manager-images/win/14-create-button.png)

7. Un écran **Acceptation de la licence** est susceptible de s’afficher. Si vous êtes d’accord avec les termes du contrat de licence, cliquez sur **Accepter** :

    ![Écran Acceptation de la licence](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Le Gestionnaire d’appareils Android ajoute le nouvel appareil à la liste des appareils virtuels installés et affiche un indicateur de progression **Création en cours** pendant la création de l’appareil :

    [![Indicateur de progression de la création](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. Lorsque le processus de création est terminé, le nouvel appareil apparaît dans la liste des appareils virtuels installés avec un bouton **Démarrer**, prêt à être lancé :

   [![Appareil nouvellement créé prêt à être lancé](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lorsque vous cliquez sur **Nouvel appareil**, l’écran **Nouvel appareil** s’affiche :

[![Écran Nouvel appareil du Gestionnaire d’appareils](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

Pour configurer un nouvel appareil dans l’écran **Nouvel appareil**, effectuez les étapes suivantes :

1. Sélectionnez un appareil physique à émuler en cliquant sur le menu déroulant **Appareil** :

    [![Menu déroulant Appareil](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. Sélectionnez une image système à utiliser avec cet appareil virtuel en cliquant sur le menu déroulant **Image système**. Les images système installées sont affichées dans la section **Installé** de ce menu. La section **Télécharger** (si présente) répertorie les images système qui ne sont actuellement pas disponibles sur votre ordinateur de développement, mais qui peuvent être installées automatiquement :

    [![Menu déroulant Image système](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Donnez un nouveau nom à l’appareil. Dans l’exemple suivant, le nouvel appareil est nommé **Nexus 5X API 25** :

    [![Nommage du nouvel appareil](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. Modifiez les propriétés nécessaires. Pour apporter des modifications aux propriétés, consultez [Propriétés de profil](#properties) plus loin dans ce guide.

5. Ajoutez toute propriété supplémentaire que vous avez besoin de définir explicitement. L’écran **Nouvel appareil** répertorie uniquement les propriétés le plus souvent modifiées, mais vous pouvez cliquer sur le menu déroulant **Ajouter une propriété** (dans l’angle inférieur gauche) pour ajouter des propriétés supplémentaires :

    [![Menu déroulant Ajouter une propriété](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Vous pouvez également cliquer sur **Personnalisé** afin de définir une nouvelle propriété pour l’appareil :

    ![Bouton Créer](xamarin-device-manager-images/mac/14-custom-button.png)

7. Cliquez sur le bouton **Créer** (dans l’angle inférieur droit) pour créer le nouvel appareil :

    ![Bouton Créer](xamarin-device-manager-images/mac/15-create-button.png)

8. Un écran **Acceptation de la licence** est susceptible de s’afficher. Si vous êtes d’accord avec les termes du contrat de licence, cliquez sur **Accepter**.

9. Le Gestionnaire d’appareils Android ajoute le nouvel appareil à la liste des appareils virtuels installés et affiche un indicateur de progression **Création en cours** pendant la création de l’appareil :

    [![Indicateur de progression de la création](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Lorsque le processus de création est terminé, le nouvel appareil apparaît dans la liste des appareils avec un bouton **Lire**, prêt à être lancé :

   [![Appareil nouvellement créé prêt à être lancé](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />


### <a name="edit-device"></a>Modifier l’appareil

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour modifier un appareil virtuel existant, sélectionnez-le, puis cliquez sur le bouton **Modifier** (situé dans le coin supérieur droit de l’écran) :

[![Bouton Modifier pour modifier un nouvel appareil](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour modifier un appareil virtuel existant, sélectionnez le menu déroulant **Options supplémentaires** (icône d’engrenage) et sélectionnez **Modifier** :
 
[![Sélection du bouton Modifier pour modifier un nouvel appareil](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

Le fait de cliquer sur le bouton **Modifier** lance l’Éditeur d’appareil pour l’appareil virtuel sélectionné :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Écran Éditeur d’appareil](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
 
[![Écran Éditeur d’appareil](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

L’écran **Éditeur d’appareil** répertorie les propriétés de l’appareil virtuel dans la première colonne et les valeurs correspondantes de chaque propriété dans la seconde colonne. Lorsque vous sélectionnez une propriété, une description détaillée de cette propriété s’affiche à droite.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Par exemple, dans la capture d’écran suivante, la propriété `hw.lcd.density` est en cours de modification et passera de **420** à **240** :

[![Exemple de modification d’un appareil](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Par exemple, dans la capture d’écran suivante, la propriété `hw.lcd.density` est en cours de modification et passera de **320** à **240** : la propriété `hw.ramSize` passera quant à elle à **768** :
 
[![Exemple de modification d’un appareil](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

Après avoir apporté les modifications de configuration nécessaires, cliquez sur le bouton **Enregistrer**.
Pour plus d’informations sur la modification des propriétés des appareils virtuels, consultez [Propriétés de profil](#properties) plus loin dans ce guide.


 
### <a name="additional-options"></a>Options supplémentaires

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Des options supplémentaires pour travailler avec les appareils sont disponibles dans le menu &hellip; situé dans le coin supérieur droit) :

[![Emplacement du menu Options supplémentaires](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Des options supplémentaires pour travailler avec un appareil sont disponibles dans le menu déroulant situé à gauche du bouton **Lire** :

[![Emplacement du menu Options supplémentaires](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Le menu Options supplémentaires contient les éléments suivants :

-   **Dupliquer et modifier** &ndash; Duplique l’appareil sélectionné et l’ouvre dans l’écran **Nouvel appareil** avec un nom unique différent. Par exemple, si vous sélectionnez **VisualStudio_android-23_x86_phone** et cliquez sur **Dupliquer et modifier**, un compteur est ajouté au nom :

    [![Écran Dupliquer et modifier](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Révéler dans l’Explorateur** &ndash; Ouvre une fenêtre de l’Explorateur Windows dans le dossier contenant les fichiers de l’appareil virtuel. Par exemple, si vous sélectionnez **Nexus 5 X API 25** et cliquez sur **Révéler dans l’Explorateur**, une fenêtre similaire à celle-ci s’ouvre :

    [![Résultat du clic sur Révéler dans l’Explorateur](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Réinitialisation aux paramètres d’usine** &ndash; Réinitialise l’appareil aux paramètres par défaut, en effaçant toutes les modifications apportées par l’utilisateur à l’état interne de l’appareil pendant son exécution. Cette modification n’altère pas les modifications que vous apportez à l’appareil virtuel lors de la création et de la modification. Une boîte de dialogue rappelant que la réinitialisation ne peut pas être annulée s’affiche. Cliquez sur **Effacer les données utilisateur** pour confirmer la réinitialisation.

-   **Supprimer** &ndash; Supprime définitivement l’appareil virtuel sélectionné.
    Une boîte de dialogue rappelant que la suppression d’un appareil ne peut pas être annulée s’affiche. Cliquez sur **Supprimer** si vous êtes certain que vous souhaitez supprimer l’appareil.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Le menu Options supplémentaires contient les éléments suivants :

-   **Modifier** &ndash; Ouvre l’appareil sélectionné dans l’éditeur d’appareil, comme décrit précédemment dans [Modifier l’appareil](#device-edit).

-   **Dupliquer et modifier** &ndash; Duplique l’appareil sélectionné et l’ouvre dans l’écran **Nouvel appareil** avec un nom unique différent.
    Par exemple, si vous sélectionnez **Nexus 5X API 25** et cliquez sur **Dupliquer et modifier**, un compteur est ajouté au nom :

    [![Écran Duplicate et modifier](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Révéler dans l’Explorateur** &ndash; Ouvre une fenêtre Finder macOS dans le dossier contenant les fichiers de l’appareil virtuel. Par exemple, si vous sélectionnez **Nexus 5 X API 25** et cliquez sur **Afficher dans le Finder**, une fenêtre similaire à celle-ci s’ouvre :

    [![Résultat du clic sur Afficher dans le Finder](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Réinitialisation aux paramètres d’usine** &ndash; Réinitialise l’appareil aux paramètres par défaut, en effaçant toutes les modifications apportées par l’utilisateur à l’état interne de l’appareil pendant son exécution. Cette modification n’altère pas les modifications que vous apportez à l’appareil virtuel lors de la création et de la modification. Une boîte de dialogue rappelant que la réinitialisation ne peut pas être annulée s’affiche. Cliquez sur **Effacer les données utilisateur** pour confirmer la réinitialisation.

-   **Supprimer** &ndash; Supprime définitivement l’appareil virtuel sélectionné.
    Une boîte de dialogue rappelant que la suppression d’un appareil ne peut pas être annulée s’affiche. Cliquez sur **Supprimer** si vous êtes certain que vous souhaitez supprimer l’appareil.

-----

<a name="properties" />
 

## <a name="profile-properties"></a>Propriétés de profil

Les écrans **Nouvel appareil** et **Modifier l’appareil** répertorient les propriétés de l’appareil virtuel dans la première colonne et les valeurs correspondantes de chaque propriété dans la seconde colonne. Lorsque vous sélectionnez une propriété, une description détaillée de cette propriété s’affiche à droite. Vous pouvez modifier ses *propriétés de profil matériel* et ses  *propriétés d’AVD*.
Les propriétés de profil matériel (comme `hw.ramSize` et `hw.accelerometer`) décrivent les caractéristiques physiques de l’appareil émulé. Ces caractéristiques comprennent la taille de l’écran, la quantité de mémoire vive disponible et la présence ou non d’un accéléromètre. Les propriétés d’AVD spécifient le fonctionnement de l’AVD lorsqu’il s’exécute. Par exemple, les propriétés d’AVD peuvent être configurées pour spécifier la manière dont l’AVD utilise la carte graphique de votre ordinateur de développement pour le rendu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Vous pouvez modifier les propriétés en suivant les instructions ci-dessous :

-   Pour modifier une propriété booléenne, cliquez sur la coche à droite de la propriété booléenne :

    ![Modification d’une propriété booléenne](xamarin-device-manager-images/win/25-boolean-value.png)

-   Pour modifier une propriété *enum* (avec liste de choix), cliquez sur la flèche vers le bas à droite de la propriété et choisissez une nouvelle valeur.

    ![Modification d’une propriété enum](xamarin-device-manager-images/win/27-enum-value.png)

-   Pour modifier une propriété de chaîne ou d’entier, double-cliquez sur le paramètre de chaîne ou d’entier actuel dans la colonne de valeur et entrez une nouvelle valeur.

    ![Modification d’une propriété d’entier](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Vous pouvez modifier les propriétés en suivant les instructions ci-dessous :

-   Pour modifier une propriété booléenne, cliquez sur la coche à droite de la propriété booléenne :

    ![Modification d’une propriété booléenne](xamarin-device-manager-images/mac/25-boolean-value.png)

-   Pour modifier une propriété *enum* (avec liste de choix), cliquez sur le menu déroulant à droite de la propriété et choisissez une nouvelle valeur.

    ![Modification d’une propriété enum](xamarin-device-manager-images/mac/27-enum-value.png)

-   Pour modifier une propriété de chaîne ou d’entier, double-cliquez sur le paramètre de chaîne ou d’entier actuel dans la colonne de valeur et entrez une nouvelle valeur.

    ![Modification d’une propriété d’entier](xamarin-device-manager-images/mac/26-integer-value.png)


-----

Le tableau suivant présente en détail les propriétés répertoriées dans les écrans **Nouvel appareil** et **Éditeur d’appareil** :

[!include[](~/android/includes/emulator-properties.md)]

Pour plus d’informations sur ces propriétés, consultez [Propriétés de profil matériel](https://developer.android.com/studio/run/managing-avds.html#hpproperties).


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Résolution des problèmes

La section suivante décrit les problèmes courants du Gestionnaire d’appareils Android Xamarin et les solutions de contournement :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Kit Android SDK dans un emplacement non standard

En règle générale, le kit Android SDK est installé à l’emplacement suivant :

**C:\\Program Files (x86)\\Android\\android-sdk**

Si le kit SDK n’est pas installé à cet emplacement, cette erreur peut s’afficher au lancement :

![Erreur d’instance du kit Android SDK](xamarin-device-manager-images/win/32-sdk-error.png)

Pour contourner ce problème, effectuez les étapes suivantes :

1. À partir du bureau Windows, accédez à **C:\\Utilisateurs\\*nom d’utilisateur*\\AppData\\Roaming\\XamarinDeviceManager** :

    ![Emplacement du fichier journal du Gestionnaire d’appareils Xamarin](xamarin-device-manager-images/win/33-log-files.png)

2. Double-cliquez pour ouvrir l’un des fichiers journaux et recherchez le **chemin du fichier config**. Exemple :

    [![Chemin du fichier config dans le fichier journal](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. Accédez à cet emplacement et double-cliquez sur **user.config** pour l’ouvrir. 

4. Dans **user.config**, recherchez l’élément **&lt;UserSettings&gt;** et ajoutez-lui un attribut **AndroidSdkPath**. Définissez cet attribut sur le chemin d’installation du kit Android SDK sur votre ordinateur et enregistrez le fichier. Par exemple, l’élément **&lt;UserSettings&gt;** se présente de la manière suivante si le kit Android SDK a été installé dans **C:\\Programmes\\Android\\SDK** :
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Après avoir apporté cette modification à **user.config**, vous devez être en mesure de lancer le Gestionnaire d’appareils Android Xamarin.

### <a name="snapshot-disables-wifi-on-android-oreo"></a>La capture instantanée désactive le Wi-Fi sur Android Oreo

Si vous avez un AVD configuré pour Android Oreo avec un accès Wi-Fi simulé, le redémarrage de l’AVD après une capture instantanée peut désactiver l’accès Wi-Fi.

Pour éviter ce problème,

1. sélectionnez l’AVD dans le Gestionnaire de périphériques Xamarin.

2. Dans le menu des options supplémentaires, cliquez sur **Révéler dans l’Explorateur**.

3. Accédez à **capture instantanée > default_boot**.

4. Supprimer le fichier **snapshot.pb** :

    [![Emplacement du fichier snapshot.pb](xamarin-device-manager-images/win/36-delete-snapshot-sml.png)](xamarin-device-manager-images/win/36-delete-snapshot.png#lightbox)

5. Redémarrez l'AVD. 

Une fois ces modifications apportées, l’AVD sera redémarré dans un état qui permet au Wi-Fi de fonctionner à nouveau.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

### <a name="snapshot-disables-wifi-on-android-oreo"></a>La capture instantanée désactive le Wi-Fi sur Android Oreo

Si vous avez un AVD configuré pour Android Oreo avec un accès Wi-Fi simulé, le redémarrage de l’AVD après une capture instantanée peut désactiver l’accès Wi-Fi.

Pour éviter ce problème,

1. sélectionnez l’AVD dans le Gestionnaire de périphériques Xamarin.

2. Dans le menu des options supplémentaires, cliquez sur **Révéler dans le Finder**.

3. Accédez à **capture instantanée > default_boot**.

4. Supprimer le fichier **snapshot.pb** :

    [![Emplacement du fichier snapshot.pb](xamarin-device-manager-images/mac/36-delete-snapshot-sml.png)](xamarin-device-manager-images/mac/36-delete-snapshot.png#lightbox)

5. Redémarrez l'AVD. 

Une fois ces modifications apportées, l’AVD sera redémarré dans un état qui permet au Wi-Fi de fonctionner à nouveau.

-----


### <a name="generating-a-bug-report"></a>Génération d’un rapport de bogues

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Si vous rencontrez un problème avec le Gestionnaire d’appareils Android Xamarin qui ne peut pas être résolu à l’aide des conseils de résolution des problèmes ci-dessus, veuillez soumettre un rapport de bogue en cliquant sur la barre de titre et en sélectionnant **Générer un rapport de bogue** :

![Emplacement de l’élément de menu pour soumettre un rapport de bogues](xamarin-device-manager-images/win/35-bug-report.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Si vous rencontrez un problème avec le Gestionnaire de périphériques Android Xamarin qui ne peut pas être résolu à l’aide des conseils de dépannage ci-dessus, veuillez remplir un rapport de bogues en cliquant sur **Aide > Générer un rapport de bogues** :

![Emplacement de l’élément de menu pour soumettre un rapport de bogues](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>Récapitulatif

Dans ce guide, vous avez découvert le Gestionnaire d’appareils Android Xamarin disponible dans Visual Studio pour Mac et Xamarin pour Visual Studio. Les fonctionnalités essentielles vous ont été expliquées, comme le démarrage et l’arrêt de l’émulateur Android, la sélection d’un appareil virtuel Android (AVD) à exécuter, la création de nouveaux appareils virtuels et la manière de modifier un appareil virtuel. Vous avez également appris comment modifier les propriétés matérielles de profil afin de les personnaliser.


## <a name="related-links"></a>Liens associés

- [Modifications des outils Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Débogage avec l’émulateur du kit Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Notes de publication d’Android SDK Tools (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
