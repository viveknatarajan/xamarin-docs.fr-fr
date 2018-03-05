---
title: Configuration du kit Android SDK
description: "Visual Studio inclut un Gestionnaire Android SDK, qui remplace le Gestionnaire du kit SDK autonome de Google. Ce guide explique comment utiliser le Gestionnaire Android SDK pour télécharger les outils, les plateformes et autres composants du kit Android SDK dont vous avez besoin pour développer des applications Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/24/2018
ms.openlocfilehash: 66ed0965f1ed01be129f22996e50bdea89a95287
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="android-sdk-setup"></a>Configuration du kit Android SDK

_Visual Studio inclut un Gestionnaire Android SDK, qui remplace le Gestionnaire SDK autonome de Google. Ce guide explique comment utiliser le Gestionnaire SDK pour télécharger les outils, les plateformes et d’autres composants d’Android SDK dont vous avez besoin pour développer des applications Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ce guide explique comment installer et utiliser le Gestionnaire Xamarin Android SDK pour Visual Studio sur Windows (ou [pour Mac](?tabs=vsmac)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ce guide explique comment installer et utiliser le Gestionnaire Xamarin Android SDK pour Visual Studio pour Mac (ou [sur Windows](?tabs=vswin)).

> [!NOTE]
> **Remarque :** Ce guide s’applique uniquement à Visual Studio 2017 et Visual Studio pour Mac.  

-----

Le Gestionnaire Xamarin Android SDK vous permet de télécharger les derniers composants Android dont vous avez besoin pour développer votre application Xamarin.Android.
Il remplace le Gestionnaire du kit SDK autonome de Google, qui a été déprécié.

Pourquoi voudriez-vous utiliser le Gestionnaire Xamarin Android SDK plutôt que le Gestionnaire du kit SDK qui est inclus avec le kit Android SDK ? Dans la version 25.2.3 du package Android SDK Tools, Google a introduit un nouvel outil de gestion du kit Android SDK. Ce nouvel outil, **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)**, est un utilitaire de ligne de commande qui remplace le Gestionnaire de l’interface utilisateur autonome du kit Android SDK. Par conséquent, si vous effectuez une mise à jour vers SDK Tools 26.0.1 (requis pour Android 8.0) ou une version supérieure et que vous souhaitez continuer de gérer le kit Android SDK via une interface utilisateur, vous devez utiliser le Gestionnaire Xamarin Android SDK.

<a name="requirements" />

## <a name="requirements"></a>Configuration requise

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour utiliser le Gestionnaire Xamarin Android SDK, vous avez besoin des éléments suivants :

- Visual Studio 2017 Community Edition ou version supérieure. Visual Studio 2017 version 15.5 ou version ultérieure est requis.

- Xamarin pour Visual Studio version 4.5.0 ou version ultérieure. 

Le Gestionnaire Xamarin Android SDK n’est pas compatible avec Visual Studio
2015. Les utilisateurs de Visual Studio 2015 doivent utiliser les outils du Gestionnaire du kit SDK fournis par Google dans le kit Android SDK.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

-   Visual Studio pour Mac 7.0.0.3146 ou version ultérieure.

-----

Le Gestionnaire Xamarin Android SDK requiert également le Kit de développement Java (qui est installé automatiquement avec Xamarin.Android).
Xamarin.Android utilise [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), qui est nécessaire si vous développez des applications pour le niveau d’API 24 ou des niveaux supérieurs (JDK 8 prend également en charge les niveaux d’API antérieurs au niveau 24). Vous pouvez continuer d’utiliser [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) si vous développez des applications spécifiquement pour le niveau d’API 23 ou des niveaux antérieurs.

> [!IMPORTANT]
> **Remarque :** Xamarin.Android ne prend pas en charge JDK 9.


<a name="installation" />

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>Installation

Le Gestionnaire Xamarin SDK peut être ajouté à Visual Studio 2017 au moment de l’installation. Lorsque vous installez Visual Studio, cliquez sur **Composants individuels** et accédez à la section **Activités de développement**.
Cochez la case **Xamarin SDK Manager**, si elle ne l’est pas déjà :

![Activation de Xamarin SDK Manager à partir de Composants individuels](android-sdk-images/win/01-sdk-manager-install.png)

Si vous avez déjà installé Visual Studio 2017, consultez [Modifier Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio) pour savoir comment modifier Visual Studio, puis suivez la procédure ci-dessus pour activer le Gestionnaire Xamarin SDK. Si vous êtes invité à mettre à jour le Gestionnaire du kit SDK, vous pouvez utiliser cette procédure pour installer le Gestionnaire Xamarin SDK.

Lorsque vous cliquez sur **Outils > Android > Gestionnaire Android SDK** (comme expliqué ci-dessous), le Gestionnaire Xamarin Android SDK est lancé à la place du Gestionnaire Google Android SDK. Si vous utilisez une version antérieure du kit Android SDK qui prend en charge le Gestionnaire Android SDK autonome de Google, l’installation du Gestionnaire Xamarin Android SDK ne crée pas de conflit &ndash; vous pouvez toujours lancer le Gestionnaire du kit SDK autonome de Google en dehors de Visual Studio pour gérer le kit Android SDK.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
 
-----

<a name="sdk-manager" /> 
 
## <a name="sdk-manager"></a>Gestionnaire du kit SDK 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour démarrer le Gestionnaire du kit SDK dans Visual Studio, cliquez sur **Outils > Android  > Gestionnaire Android SDK** :

[![Emplacement de l’élément de menu Gestionnaire Android SDK](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png)

Le **Gestionnaire Xamarin Android SDK** s’ouvre dans l’écran **Kits Android SDK et outils**. Cet écran comporte deux onglets &ndash; **Plateformes** et **Outils** :

[![Capture d’écran du Gestionnaire Android SDK ouvert sous l’onglet Plateformes](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png)

L’écran **Kits Android SDK et outils** est décrit plus en détail dans les sections suivantes.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour démarrer le Gestionnaire du kit SDK dans Visual Studio pour Mac, cliquez sur **Outils > SDK Manager** :
 
![Emplacement de l’élément de menu SDK Manager](android-sdk-images/mac/sdkmanager-01.png )

Le **Gestionnaire Android SDK** s’ouvre dans la **fenêtre Préférences**, qui contient trois onglets, **Plateformes**, **Outils**et **Emplacements** :

![Capture d’écran du Gestionnaire Android SDK ouvert sous l’onglet Plateformes](android-sdk-images/mac/sdkmanager-02.png)

Les onglets du Gestionnaire Xamarin Android SDK sont décrits dans les sections suivantes.

-----


<a name="sdkmanager-locations" /> 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Emplacement du kit Android SDK

L’emplacement du kit Android SDK est configuré en haut de l’écran **Kits Android SDK et outils**, comme illustré dans l’image précédente. Cet emplacement doit être configuré correctement pour que les onglets **Plateformes** et **Outils** fonctionnent correctement. Vous devrez peut-être définir l’emplacement du kit Android SDK pour une ou plusieurs des raisons suivantes :

1. Le Gestionnaire Xamarin SDK n’a pas réussi à trouver le kit Android SDK. 

2. Vous avez installé le kit Android SDK à un autre emplacement (qui n’est pas l’emplacement par défaut). 

Pour définir l’emplacement du kit Android SDK, cliquez sur le bouton &hellip; situé complètement à droite du champ **Emplacement du kit Android SDK**. La boîte de dialogue **Rechercher un dossier** qui permet de naviguer vers l’emplacement du kit Android SDK s’ouvre. Dans la capture d’écran suivante, le kit Android SDK sous **Program Files (x86)\\Android** est sélectionné :

![Capture d’écran de la boîte de dialogue Windows Rechercher un dossier ayant permis de trouver sdk android](android-sdk-images/win/05-browse-for-folder.png)

Lorsque vous cliquez sur **OK**, le Gestionnaire Xamarin Android SDK gère le kit Android SDK qui est installé à l’emplacement sélectionné.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

### <a name="locations-tab"></a>Onglet Emplacements

L’onglet **Emplacements** comporte trois paramètres permettant de configurer l’emplacement du kit Android SDK, du kit Android NDK et du kit JDK (Java SDK). Ces emplacements doivent être configurés correctement pour que les onglets **Plateformes** et **Outils** fonctionnent correctement.

Au démarrage , le Gestionnaire du kit SDK détermine automatiquement le chemin de chaque package installé et indique qu’il a été **trouvé** en affichant une coche verte en regard du chemin :

![Capture d’écran de l’onglet Emplacements](android-sdk-images/mac/sdkmanager-03.png)

Cliquez sur le bouton **Rétablir les valeurs par défaut** pour faire en sorte que le Gestionnaire du kit SDK recherche le kit SDK, le kit NDK et le kit JDK à leur emplacement par défaut. 

En général, vous utilisez l’onglet **Emplacements** pour modifier l’emplacement du kit Android SDK et/ou du kit Java JDK. Vous n’avez pas besoin d’installer le kit NDK pour développer des applications Xamarin.Android. Le kit NDK est utilisé uniquement lorsque vous avez besoin de développer des parties de votre application à l’aide de langages en code natif, comme C et C++.

-----

<a name="sdkmanager-tools" /> 

### <a name="tools-tab"></a>Onglet Outils

L’onglet **Outils** affiche une liste d’_outils_ et de _bonus_. Utilisez cet onglet pour installer les outils du kit Android SDK, les outils de plateforme et les outils de génération.
Vous pouvez également installer l’émulateur Android, le débogueur de bas niveau (LLDB), le kit NDK, l’accélération HAXM et les bibliothèques Google Play.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Par exemple, pour télécharger le package Émulateur Android de Google, cochez la case en regard d’**Émulateur Android** et cliquez sur le bouton **Appliquer les modifications** :

[![Installation de l’émulateur Android à partir de l’onglet Outils](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Par exemple, pour télécharger le package Émulateur Android de Google, cochez la case en regard d’**Émulateur Android** et cliquez sur le bouton **Installer les mises à jour** :

![Installation de l’émulateur Android à partir de l’onglet Outils](android-sdk-images/mac/sdkmanager-08.png)

-----


Une boîte de dialogue peut s’afficher avec le message : _Certains composants peuvent être mis à jour. Voulez-vous les consulter et les mettre à jour maintenant ?_ Cliquez sur **Oui**. Ensuite, une boîte de dialogue Acceptation de la licence s’affiche :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Écran Acceptation de la licence](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Écran Acceptation de la licence](android-sdk-images/mac/sdkmanager-09.png)

-----

Cliquez sur **Accepter** si vous acceptez les Conditions générales. En bas de la fenêtre, une barre de progression indique l’avancement du téléchargement et de l’installation. Une fois l’installation terminée, l’onglet **Outils** indique que les outils et bonus sélectionnés sont installés.


<a name="xvs-platforms" /> 

### <a name="platforms-tab"></a>Onglet Plateformes

L’onglet **Plateformes** affiche une liste des versions de kit SDK avec d’autres ressources (comme des images système) pour chaque plateforme.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Capture d’écran du volet Plateformes](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Capture d’écran du volet Plateformes](android-sdk-images/mac/sdkmanager-11.png)

-----

Cet écran répertorie la version Android (comme **Android 7.0**), le nom de code (**Nougat**), le niveau d’API (tel que **24**) et l’état (**Installé** si la plateforme est installée). Vous utilisez l’onglet **Plateformes** afin d’installer des composants pour le niveau d’API Android que vous souhaitez cibler (pour plus d’informations sur les versions Android et les niveaux d’API, consultez [Présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md)).

Si tous les composants d’une plateforme sont installés, une coche apparaît en regard du nom de la plateforme. Si tous les composants d’une plateforme ne sont pas installés, la case de cette plateforme est noircie. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Vous pouvez développer une plateforme pour voir ses composants (et ceux qui sont installés) en cliquant sur le signe **+** à gauche de la plateforme.
Cliquez sur **-** pour réduire la liste de composants d’une plateforme.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Vous pouvez développer une plateforme pour voir ses composants (et ceux qui sont installés) en cliquant sur la **flèche** à gauche de la plateforme.
Cliquez sur la **flèche vers le bas** pour réduire la liste de composants d’une plateforme.

-----

Pour ajouter une autre plateforme au kit SDK, cliquez sur la case en regard de la plateforme jusqu'à ce qu’une coche apparaisse afin d’installer l’ensemble de ses composants, puis cliquez sur **Appliquer les modifications** :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Exemple d’ajout des composants Android 7.1 Nougat au kit Android SDK](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Exemple d’ajout des composants Android 4.4 au kit Android SDK](android-sdk-images/mac/sdkmanager-12.png)

-----

Pour installer uniquement le kit SDK, cliquez une fois sur la case en regard de la plateforme. Vous pouvez ensuite sélectionner les composants individuels dont vous avez besoin :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Exemple d’ajout de certains composants d’Android 7.1](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Exemple d’ajout de certains composants d’Android 4.4](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Notez que le nombre de composants à installer s’affiche en regard du bouton **Appliquer les modifications**. Dans l’exemple ci-dessus, six composants sont prêts à être installés. Après avoir cliqué sur le bouton **Appliquer les modifications**, l’écran **Acceptation de la licence** s’affiche :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Notez que le nombre de composants à installer s’affiche en regard du bouton **Appliquer les modifications**. Après avoir cliqué sur le bouton **Appliquer les modifications**, l’écran **Acceptation de la licence** s’affiche :

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Boîte de dialogue Acceptation de la licence de l’onglet Plateforme](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Boîte de dialogue Acceptation de la licence de l’onglet Plateforme](android-sdk-images/mac/sdkmanager-14.png)

-----

Cliquez sur **Accepter** si vous acceptez les Conditions générales. Cette boîte de dialogue peut s’afficher plusieurs fois si plusieurs composants sont installés. En bas de la fenêtre, une barre de progression indique l’avancement du téléchargement et de l’installation. Lorsque le processus de téléchargement et d’installation est terminé (cela peut prendre plusieurs minutes, selon le nombre de composants qui doivent être téléchargés), les composants ajoutés sont marqués avec une coche et répertoriés comme **Installé**.

Vous êtes maintenant prêt à développer votre application pour le tout dernier niveau d’API Android.


<a name="summary" />
 
## <a name="summary"></a>Récapitulatif

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dans ce guide, vous avez appris comment installer et utiliser le Gestionnaire Xamarin Android SDK pour Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans ce guide, vous avez appris comment installer et utiliser le Gestionnaire Xamarin Android SDK pour Visual Studio pour Mac.

-----


## <a name="related-links"></a>Liens associés

- [Modifications des outils Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Présentation des niveaux d’API d’Android](~/android/app-fundamentals/android-api-levels.md)
- [Notes de publication de SDK Tools (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
