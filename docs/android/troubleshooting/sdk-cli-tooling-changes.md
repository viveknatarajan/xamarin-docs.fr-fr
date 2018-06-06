---
title: Modifications des outils du kit Android SDK
description: Modifications apportées à la façon dont le Kit de développement logiciel Android gère les niveaux d’API et AVDs installés.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: b0d9458238c4b3ac9ceeeb7d7ce4e2ca8b0b6de3
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732864"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Modifications des outils du kit Android SDK

_Modifications apportées à la façon dont le Kit de développement logiciel Android gère les niveaux d’API et AVDs installés._

## <a name="changes-to-android-sdk-tooling"></a>Modifications apportées aux outils du Kit de développement logiciel Android

Dans les versions récentes des outils SDK pour Android de Google a supprimé les gestionnaires AVD et Kit de développement logiciel existants en faveur de nouveaux outils d’interface CLI (Interface de ligne de commande). Le **android** programme a été supprimé et les gestionnaires de l’interface utilisateur graphique de Google (Interface utilisateur graphique) dans Visual Studio pour Mac et les versions antérieures de Visual Studio Tools pour Xamarin ne fonctionnera plus après la version 25.2.5 des outils de kit de développement logiciel Android. Par exemple, essayez d’utiliser le **android** programme via la ligne de commande génère un message d’erreur semblable à la suivante :

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Les sections suivantes expliquent comment gérer le SDK Android et les appareils virtuels Android à l’aide du Kit de développement logiciel Android 25.3.0 et versions ultérieures.

### <a name="ui-tools"></a>Outils d’interface utilisateur

Visual Studio et Visual Studio pour Mac fournissent désormais des remplacements de Xamarin pour les gestionnaires d’interface utilisateur graphique de Google supprimées :

-   Pour télécharger les outils du kit SDK Android, les plateformes et les autres composants dont vous avez besoin pour développer des applications de Xamarin.Android, utilisez le [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) plutôt que le Gestionnaire du SDK de Google hérité.

-   Pour créer et configurer des appareils virtuels Android, utilisez le [le Gestionnaire de périphériques Android](~/android/get-started/installation/android-emulator/device-manager.md) au lieu du Gestionnaire d’émulateur Google hérité.

Ces outils sont fonctionnellement équivalents à basée sur l’interface utilisateur graphique de Google gestionnaires elles remplacent.

### <a name="cli-tools"></a>Outils de l’interface CLI

Ou bien, vous pouvez utiliser les outils CLI pour gérer et mettre à jour votre émulateurs et le Kit de développement logiciel Android. Les programmes suivants composent désormais l’interface de ligne de commande pour les outils du Kit de développement logiciel Android :

#### <a name="sdkmanager"></a>sdkmanager

**Ajouté dans :** Outils kit de développement logiciel Android 25.2.3 (novembre 2016) et versions ultérieures.

Il existe un nouveau programme appelé **sdkmanager** dans les **outils/bin** dossier de votre Kit de développement logiciel Android. Cet outil est utilisé pour mettre à jour le SDK Android à la ligne de commande. Pour plus d’informations sur l’utilisation de cet outil, consultez [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Ajouté dans :** Outils kit de développement logiciel Android 25.3.0 (mars 2017) et versions ultérieures.

Il existe un nouveau programme appelé **avdmanager** dans les **outils/bin** dossier de votre Kit de développement logiciel Android. Cet outil est utilisé pour mettre à jour les AVDs pour l’émulateur Android de Google. Pour plus d’informations sur l’utilisation de cet outil, consultez [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>La rétrogradation

Vous pouvez mettre à niveau votre **Outils kit de développement logiciel Android** version en installant une version précédente du SDK Android à partir de la [site Web Android Developer](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>À l’aide de l’interface de l’ancien

Vous pouvez toujours utiliser l’interface graphique utilisateur d’origine en exécutant le **android** de programme à l’intérieur de votre **outils** dossier tant que vous êtes sur **Outils kit de développement logiciel Android** version **25.2.5**  ou inférieure.


## <a name="related-links"></a>Liens associés

- [Configuration du kit Android SDK](~/android/get-started/installation/android-sdk.md)
- [Gestionnaire de périphériques Android](~/android/get-started/installation/android-emulator/device-manager.md)
- [Présentation des niveaux d’API d’Android](~/android/app-fundamentals/android-api-levels.md)
- [Notes de publication de SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
