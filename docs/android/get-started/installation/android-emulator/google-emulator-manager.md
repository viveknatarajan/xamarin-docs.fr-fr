---
title: "Gestionnaire d’émulateur Google"
description: "Guide pratique pour créer et gérer des appareils virtuels Android à l’aide du Gestionnaire d’émulateur Google"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1e90ebe55dbec9fdcc57b9eaab99e614fdc5cbbc
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="google-emulator-manager"></a>Gestionnaire d’émulateur Google

Après avoir vérifié que l’accélération matérielle est activée (comme décrit dans [Accélération matérielle de l’émulateur Android](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), l’étape suivante consiste à créer des appareils virtuels en vue de tester et de déboguer votre application. Vous pouvez utiliser le Gestionnaire d’émulateur Google hérité (également appelé *Gestionnaire d’appareils virtuels Android*) afin de créer des appareils virtuels qui seront utilisés par l’émulateur du kit Android SDK.

> [!NOTE]
> Si vous ciblez Android 8.0 Oreo, vous devez utiliser le [Gestionnaire d’appareils Android Xamarin](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) pour créer et configurer des appareils virtuels.


## <a name="installing-system-images"></a>Installation des images système

En fonction du ou des niveau(x) d’API Android que vous voulez cibler, vous devez télécharger et installer les images système spécifiques à ces niveaux d’API qui sont utilisées par l’émulateur du kit Android SDK. Pour chaque niveau d’API Android, il existe un ensemble d’images système **x86** que vous devez télécharger et installer pour créer des appareils virtuels.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour installer les images système nécessaires, démarrez le Gestionnaire du kit Android SDK (**Outils > Android > Gestionnaire du kit Android SDK**) et accédez au(x) niveau(x) d’API que vous souhaitez prendre en charge. Pour chaque niveau d’API, cochez la case en regard des images système suivantes :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour installer les images système nécessaires, démarrez le Gestionnaire du kit Android SDK (**Outils > Android > Gestionnaire du kit Android SDK**) et accédez au(x) niveau(x) d’API que vous souhaitez prendre en charge. Pour chaque niveau d’API, cochez la case en regard des images système suivantes :

-----

-   **Intel x86 Atom System Image**
-   **Google APIs Intel x86 Atom System Image**

Cette dernière image système ajoute les API Google (par exemple, API Google Maps à l’appareil virtuel). 

Dans la capture d’écran suivante, les images **Intel x86 Atom** seront installées et vous pourrez ainsi créer des appareils virtuels exécutant Android 6.0 :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sélection des images système Android 6.0 x86 pour l’émulateur Android](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sélection des images système Android 6.0 x86 pour l’émulateur Android](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png#lightbox)

-----

Si vous développez des applications 64 bits, installez plutôt les images système suivantes :

-   **Image système Intel x86 Atom_64**
-   **Google APIs Intel x86 Atom_64 System Image**

Vous pouvez utiliser ces images système 64 bits pour exécuter des applications 32 bits. Toutefois, l’image système **Intel x86 Atom System Image** 32 bits s’exécute légèrement plus rapidement dans l’émulateur du kit Android SDK.

Si vous développez des applications pour Android Wear, installez les images système suivantes :

-   **Android Wear Intel x86 Atom System Image**
-   **Google APIs Intel x86 Atom System Image**

Une fois ces images système installés, vous pouvez créer des appareils virtuels Android **x86** en sélectionnant le niveau d’API et le processeur/ABI appropriés lors de la configuration de l’appareil virtuel (comme décrit ci-dessous).


## <a name="configuring-virtual-devices"></a>Configuration d’appareils virtuels

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Les appareils virtuels sont configurés via le **Gestionnaire d’émulateur Android** (également appelé _Android Virtual Device Manager_ ou _AVD Manager_). Pour lancer le Gestionnaire d’émulateur Android à partir de Visual Studio, cliquez sur l’icône **Gestionnaire d’émulateur Android** dans la barre d’outils :

[![Emplacement de l’icône AVD](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png#lightbox)

Vous pouvez également lancer le Gestionnaire d’émulateur Android à partir de la barre de menus en sélectionnant **Outils > Android >Gestionnaire d’émulateur Android** :

[![Emplacement de l’élément de menu Gestionnaire d’émulateur Android](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png#lightbox)

La boîte de dialogue **Android Virtual Device (AVD) Manager** affiche la liste des appareils virtuels Android existants :

[![Android Virtual Device Manager](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Les appareils virtuels sont configurés via le **Gestionnaire d’émulateur Android** (également appelé _Android Virtual Device Manager_ ou _AVD Manager_). 

Vous pouvez lancer le Gestionnaire d’émulateur Android à partir de la barre de menus en sélectionnant **Outils > Gestionnaire d’émulateur Android** :

[![Emplacement de l’élément de menu Gestionnaire d’émulateur Android](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png#lightbox)

La boîte de dialogue **Android Virtual Device (AVD) Manager** affiche la liste des appareils virtuels Android existants :

[![Android Virtual Device Manager](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png#lightbox)

-----

Vous pouvez créer de nouvelles images d’appareil virtuel avec des caractéristiques d’appareil et des niveaux d’API différents. La section suivante explique comment créer des définitions d’appareil et des appareils virtuels personnalisés.


### <a name="creating-a-custom-device-definition"></a>Création d’une définition d’appareil personnalisée

Pour créer une définition d’appareil personnalisée, cliquez sur **Create** dans la boîte de dialogue **Android Virtual Device (AVD) Manager**. La boîte de dialogue **Create new Android Virtual Device (AVD)** s’ouvre :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Définition d’appareil personnalisée reposant sur Nexus 6](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Définition d’appareil personnalisée reposant sur Nexus 6](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png#lightbox)

-----

Configurez les options suivantes dans cette boîte de dialogue :

-   **AVD Name** &ndash; Nom unique de votre définition d’appareil. Dans la capture d’écran ci-dessus, le nom est **MyNexus**. Notez que le nom d’AVD ne peut pas contenir d’espaces &ndash;. Le bouton **OK** est désactivé si vous essayez d’utiliser des espaces dans le nom d’AVD.

-   **Device** &ndash; Sélectionnez le profil matériel que vous souhaitez émuler (par exemple, **Nexus 5** ou **Nexus 6**).

-   **Target** &ndash; Sélectionnez le niveau d’API Android de l’appareil virtuel. Ce paramètre doit être supérieur ou égal à la Version Android minimale de votre application.

-   **CPU/ABI** &ndash; Sélectionnez **Google API Intel Atom (x86)** pour que les API Google soient disponibles dans votre définition d’appareil.

-   **Skin** &ndash; Sélectionnez l’apparence de l’appareil virtuel. Dans la capture d’écran ci-dessus, l’apparence **HVGA** est sélectionnée (la capture d’écran de l’émulateur à la fin de cet article est un exemple d’apparence **HVGA**).

-   **Memory Options** &ndash; En général, le paramètre de mémoire vive par défaut est trop élevé et, sous Windows, génère un message d’avertissement indiquant que l’**émulation de mémoire vive supérieure à 768 Mo peut échouer**. Pour la plupart des utilisateurs, nous recommandons de définir la mémoire vive à 768 Mo (comme illustré dans la capture d’écran ci-dessus). Des valeurs de mémoire vive élevées peuvent ralentir l’émulateur.

-   **Use Host GPU** &ndash; Avec cette option, l’émulateur utilise l’unité centrale graphique (GPU, Graphical Processing Unit) de l’ordinateur hôte pour effectuer les opérations graphiques. Nous vous recommandons d’activer cette option pour optimiser les performances de l’émulateur. Pour plus d’informations sur la section **Emulation Option**, consultez [À quoi servent les options d’émulation Snapshot et Use Host GPU ?](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for)


Vous pouvez conserver les valeurs par défaut pour les options restantes. Lorsque vous êtes prêt, cliquez sur **OK** pour créer le nouvel appareil virtuel. Les résultats de la nouvelle configuration d’appareil virtuel sont détaillés dans la boîte de dialogue ci-dessous :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Boîte de dialogue de résultats après création d’un nouvel AVD](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Boîte de dialogue de résultats après création d’un nouvel AVD](google-emulator-manager-images/mac/07-create-results.png)

-----

Pour obtenir une explication détaillée des propriétés de configuration de cette boîte de dialogue, consultez [Propriétés de profil matériel](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
Après avoir cliqué sur **OK**, la nouvelle configuration d’appareil s’affiche dans la liste des appareils virtuels Android existants. Dans la capture d’écran suivante, **MyNexus** a été ajouté à la liste :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus ajouté à la liste des appareils](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![MyNexus ajouté à la liste des appareils](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png#lightbox)

-----

Le nouvel appareil virtuel personnalisé est également ajouté au menu déroulant des appareils :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus ajouté au menu déroulant des appareils](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![MyNexus ajouté au menu déroulant des appareils](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png#lightbox)

-----



### <a name="cloning-a-device-definition"></a>Clonage d’une définition d'appareil

Il est possible de sélectionner une définition d’appareil existante et de la *cloner* pour créer une nouvelle définition d’appareil personnalisée. Cette stratégie est particulièrement adaptée s’il existe une définition d’appareil qui ne demande que quelques ajustements mineurs pour répondre à vos besoins. L’onglet **Device Definitions** de la boîte de dialogue **Android Virtual Device (AVD) Manager** répertorie toutes les définitions d’appareil disponibles :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Liste de définitions d’appareil disponibles](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Liste de définitions d’appareil disponibles](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png#lightbox)

-----

Les appareils préconfigurés dans cette liste ne peuvent pas être modifiés. Seuls les appareils virtuels créés par l’utilisateur peuvent l’être. Il est possible de créer une nouvelle définition d’appareil à partir d’une définition d’appareil préconfigurée en sélectionnant la définition d’appareil, puis en cliquant sur **Clone**. Par exemple, lorsque vous sélectionnez la définition **Nexus 5** et cliquez sur **Clone**, la boîte de dialogue suivante s’affiche :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Boîte de dialogue Clone Device](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Boîte de dialogue Clone Device](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png#lightbox)

-----

Dans la capture d’écran suivante, le nom est remplacé par **Nexus 5 Custom** et les paramètres de l’appareil sont modifiés pour créer une nouvelle définition d’appareil personnalisée :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![AVD Nexus 5 personnalisé](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![AVD Nexus 5 personnalisé](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png#lightbox)

-----

Lorsque vous cliquez sur **Clone Device**, une nouvelle définition d’appareil est créée et s’affiche dans la liste **Device Definitions** :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5 Custom apparaît comme nouvelle définition d’appareil par l’utilisateur](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Nexus 5 Custom apparaît comme nouvelle définition d’appareil par l’utilisateur](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png#lightbox)

-----

Notez que chaque définition d’appareil créée par l’utilisateur s’affiche avec une icône verte, comme illustré ci-dessus. Cette nouvelle définition d’appareil peut servir à créer un nouvel AVD en sélectionnant la définition et en cliquant **Create AVD**. La boîte de dialogue **Create new Android Virtual Device (AVD)** s’affiche. Dans l’exemple suivant, le nom **AVD\_for\_Nexus\_5\_Custom** a été généré automatiquement pour le nouvel appareil virtuel :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Créer un AVD à partir de la définition d’appareil Nexus 5 Custom User](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Créer un AVD à partir de la définition d’appareil Nexus 5 Custom User](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png#lightbox)

-----

Après avoir cliqué sur **OK**, la nouvelle configuration d’appareil personnalisée s’affiche dans la liste des appareils virtuels Android existants. Elle est de plus ajoutée au menu déroulant des appareils :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nouvel AVD personnalisé ajouté au menu déroulant des appareils](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Nouvel AVD personnalisé ajouté au menu déroulant des appareils](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png#lightbox)

-----

