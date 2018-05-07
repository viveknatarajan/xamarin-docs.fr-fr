---
title: Émulateur Android de Visual Studio
description: Ce guide explique comment configurer et utiliser l’émulateur Android de Visual Studio pour développer des applications Xamarin.Android dans Visual Studio 2015.
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: 29e35d0dee614d28eed08fbe8799fc74c5ad1eba
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="visual-studio-android-emulator"></a>Émulateur Android de Visual Studio

_Ce guide explique comment configurer et utiliser l’émulateur Android de Visual Studio pour développer des applications Xamarin.Android dans Visual Studio 2015._

## <a name="visual-studio-android-emulator-overview"></a>Présentation de l’émulateur Android de Visual Studio

Microsoft Visual Studio 2015 comprend un émulateur Android qui peut être utilisé comme cible pour le débogage d’une application Xamarin.Android : l’*émulateur Visual Studio pour Android*. Cet émulateur utilise les fonctionnalités Hyper-V de votre ordinateur de développement, ce qui permet un lancement plus rapide et un temps d’exécution accéléré par rapport à l’émulateur par défaut qui est fourni dans le kit Android SDK. L’émulateur Visual Studio pour Android peut être utilisé comme alternative à l’émulateur par défaut du kit Android SDK lors du développement d’une application Xamarin.Android.

> [!NOTE]
> L’émulateur Android de Visual Studio est uniquement compatible avec Visual Studio 2015 &ndash; il ne fonctionne pas avec Visual Studio 2017.

Ce guide explique comment lancer l’émulateur Android de Microsoft à partir de Visual Studio pour tester votre application et décrit les différentes fonctionnalités disponibles dans cet émulateur. Vous allez apprendre comment sélectionner des *profils d’appareil* (similaires aux définitions d’appareil dans l’émulateur par défaut du kit Android SDK) pour simuler différents types d’appareils Android. Enfin, une section Résolution des problèmes explique les pièges courants et les solutions de contournement.

## <a name="requirements"></a>Configuration requise

Pour exécuter l’émulateur, l’ordinateur doit répondre à la configuration requise pour exécuter Hyper-V. Hyper-V requiert une version 64 bits de Windows 8 Professionnel, Windows 8.1 Professionnel, Windows 10 Professionnel ou version supérieure. Pour plus d’informations sur la configuration requise, consultez [Configuration système requise pour l’émulateur Visual Studio pour Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

> [!NOTE]
> Vous ne pouvez pas utiliser HAXM (utilisé par l’émulateur d’Android SDK) quand Hyper-V est activé. Pour plus d’informations sur les limitations et les problèmes potentiels d’HAXM, consultez [Conflits de virtualisation d’HAXM](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts).


## <a name="running-the-emulator"></a>Exécution dans l'émulateur

Visual Studio propose plusieurs profils préconfigurés d’appareil cible dans le menu déroulant **Cible de débogage** (comme illustré dans la capture d’écran suivante). Les cibles de l’émulateur Android de Microsoft sont précédées de la mention **VS Emulator** :

[![Profils d’appareils cibles préconfigurés](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Quand Visual Studio démarre une application Xamarin.Android, l’émulateur est lancé avec l’appareil cible choisi et l’application est déployée sur l’émulateur. Un message indiquant que l’émulateur est en cours de démarrage s’affiche dans l’angle inférieur gauche de Visual Studio :

[![Démarrage de l’émulateur Visual Studio](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

Après un délai de démarrage, l’écran de l’émulateur s’affiche comme illustré ci-dessous à gauche. Faites glisser l’icône de verrou vers le haut pour déverrouiller l’appareil.
L’application Xamarin.Android doit ensuite s’exécuter dans l’émulateur comme illustré à droite :

[![Captures d’écran de l’émulateur](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

Comme avec l’émulateur par défaut du kit Android SDK, il est possible de définir des points d’arrêt dans le code, d’examiner des variables et d’afficher la pile des appels. La barre d’outils verticale à droite de l’émulateur permet d’accéder aux fonctionnalités de l’émulateur :

[![Boutons de la barre d’outils verticale](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

La fonction de chaque bouton de la barre d’outils verticale est présentée ci-dessous :

-   **Fermer** &ndash; Arrête l’application d’émulateur. Ce bouton n’est pas souvent utilisé. En règle générale, l’émulateur continue de s’exécuter après le lancement initial (pour éviter le délai de redémarrage de l’émulateur) et est fermé uniquement lorsqu’il n’est plus nécessaire.

-   **Réduire** &ndash; Laisse l’émulateur s’exécuter, mais le réduit dans la barre des tâches.

-   **Marche/Arrêt** &ndash; Simule la mise sous tension et hors tension de l’appareil. (L’émulateur continue de s’exécuter.)

-   **Entrée tactile multipoint** &ndash; Superpose plusieurs points sur l’écran de l’appareil qui font office de points d‘entrée tactile pour le pincement et le zoom.
    Le fait de faire glisser un point déplace l’autre point dans le sens opposé, pour simuler le mouvement à deux doigts.

-   **Entrée souris point unique** &ndash; Rétablit l’entrée point unique (après avoir utilisé l’entrée tactile multipoint).

-   **Faire pivoter à gauche/Faire pivoter à droite** &ndash; Permet de tester la manière dont l’application répond aux changements d’orientation. Par exemple, la première fois que le bouton *Faire pivoter à gauche* est activé, l’émulateur bascule en mode paysage. Lorsque le bouton *Faire pivoter à droite* est activé l’émulateur revient au mode portrait.

-   **Adapter à l’écran** &ndash; Effectue un zoom de la taille de l’écran de l’émulateur afin qu’elle s’adapte à l’écran de bureau.

-   **Zoom** &ndash; Applique une mise à l’échelle de l’émulateur de 33 %, 50 %, 66 %, 100 % ou un pourcentage personnalisé.


Le bouton *Outils supplémentaires* ouvre une boîte de dialogue qui affiche les fonctionnalités supplémentaires de l’émulateur :

[![Boîte de dialogue Outils supplémentaires](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


Chaque fonctionnalité supplémentaire est disponible dans une ligne d’onglets en haut de la boîte de dialogue :

-   **Accéléromètre** &ndash; Simule le mouvement de l’appareil dans un espace 3D.

-   **Emplacement** &ndash; Présente une carte qui peut être utilisée pour sélectionner et simuler un emplacement GPS. Sur cette carte, des *points de la carte* peuvent être créés pour simuler le mouvement entre des emplacements.

-   **Batterie** &ndash; Fournit un curseur permettant de simuler le niveau de batterie restant de l’appareil.

-   **Capture d’écran** &ndash; Dans cet onglet, le bouton **Capturer** permet de prendre une capture d’écran et d’afficher un aperçu instantané. Le bouton *Enregistrer* enregistre la capture d’écran.

-   **Appareil photo** &ndash; Simule la prise d’une photo via une image animée fixe, une image à partir d’un fichier ou à partir d’une webcam attachée sur votre ordinateur hôte. Il est possible de sélectionner l’appareil photo avant ou arrière.

-   **Carte SD** &ndash; L’émulateur peut mettre à disposition de l’appareil un dossier de votre ordinateur hôte en tant que carte SD.
    Lorsque l’application lit et écrit des fichiers sur la carte SD simulée, ces fichiers sont accessibles directement depuis le bureau sans utiliser la commande `adb`.

-   **Réseau** &ndash; Affiche un résumé des paramètres réseau de l’émulateur (l’émulateur réutilise la connexion réseau de l’ordinateur hôte).

Pour plus d’informations sur l’utilisation de ces fonctionnalités, consultez [Présentation de l’émulateur pour Android de Visual Studio](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/).



## <a name="configuring-device-profiles"></a>Configuration des profils d’appareil

(L’émulateur Android de Microsoft comprend un ensemble de profils d’appareil qui représentent les versions du système d’exploitation Android, les tailles d’écran et les propriétés de matériel les plus courantes des appareils Android sur le marché. En outre, ces profils d’appareil sont déjà configurés pour différentes versions du système d’exploitation Android, comme KitKat, Lollipop et Marshmallow.

Le *Gestionnaire d’émulateur* est utilisé pour installer, désinstaller et démarrer les profils d’appareil. Dans le menu **Outils**, sélectionnez **Émulateur Visual Studio pour Android...** comme illustré dans cette capture d’écran :

[![Lancement de l’émulateur à partir du menu Outils](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

Cette opération ouvre la boîte de dialogue **Device Profiles** (Profils d’appareil). Les profils installés sont mis en surbrillance en haut de la liste des profils d’appareil. Les profils qui ne sont pas installés (mais qui sont disponibles pour l’installation) sont grisés :

[![Icônes des profils d’appareil](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

Pour installer un nouveau profil, cliquez sur l’icône d’installation de profil (une flèche pointant vers le bas comme illustré dans la capture d’écran ci-dessus). Par exemple, lorsque vous cliquez sur l’icône d’installation du profil **5.7" Marshmallow (6.0.0) XHDPI Phone**, le Gestionnaire d’émulateur télécharge le profil comme indiqué ici :

[![Exemple de téléchargement de profils](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

Une fois le profil d’appareil téléchargé, il est mis en surbrillance pour indiquer que le profil a été installé avec succès. Vous pouvez cliquer sur l’icône *Afficher les détails* pour afficher le type de plateforme, l’architecture de processeur, la taille/résolution d’écran et la mémoire disponible sur l’appareil :

[![Afficher les détails du profil d’appareil](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Le profil d’appareil qui vient d’être installé est désormais disponible en tant que cible dans le menu déroulant **Cible de débogage** de Visual Studio :

[![Nouveau profil dans le menu déroulant de la cible](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

Cette liste peut être réduite en cliquant sur **Désinstaller ce profil** dans le *Gestionnaire d’émulateur* de manière à supprimer les profils d’appareil non utilisés. Notez qu’il n’existe actuellement aucun moyen de créer un profil d’appareil personnalisé dans cet émulateur.


## <a name="troubleshooting"></a>Résolution des problèmes

Cette section décrit des erreurs courantes rencontrées lors de l’utilisation de l’*émulateur Visual Studio pour Android* avec Xamarin.Android et les solutions de contournement.

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>L’émulateur ne démarre pas

Parfois, l’émulateur ne démarre pas en cas d’incompatibilité entre le processeur hôte et la machine virtuelle Hyper-V. Pour contourner ce problème, configurez Hyper-V de manière à limiter les fonctionnalités du processeur d’un ordinateur virtuel. Cela améliore la compatibilité de la machine virtuelle avec différentes versions de processeur hôte.
Pour cela, effectuez les étapes suivantes :

1.  Cliquez sur le bouton **Démarrer**, tapez **MMC**, et appuyez sur **Entrée**. Cliquez sur **Gestionnaire Hyper-V** comme illustré ici :

    [![Gestionnaire Hyper-V](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  Dans le volet **Machines virtuelles** du Gestionnaire Hyper-V, cliquez avec le bouton droit sur l’émulateur à modifier, puis cliquez sur **Paramètres...**  :

    [![Élément de menu Paramètres des machines virtuelles](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  Dans la fenêtre de paramètres, recherchez la section **Compatibilité** (sous **Matériel > Processeur**) et cochez la case **Migrer vers un ordinateur physique ayant une autre version de processeur** :

    [![Option Migrer activée](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  Cliquez sur **OK** et fermez la fenêtre Gestionnaire Hyper-V.



### <a name="app-deploys-and-starts-but-fails-immediately"></a>L’application est déployée et démarre, mais échoue immédiatement

Dans cette situation, l’émulateur démarre, l’application se déploie correctement sur l’émulateur et l’application démarre. Toutefois, elle échoue immédiatement.
Souvent, cette erreur est également due à une incompatibilité entre le processeur hôte et la machine virtuelle Hyper-V. Pour résoudre cette erreur, suivez les instructions de la section [L’émulateur ne démarre pas](#cant_connect) (ci-dessus).


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>L’émulateur s’arrête avec le message de diagnostic : **libaot-mscorlib.dll.so introuvable**

Pour résoudre cette erreur, utilisez les étapes suivantes afin de désactiver le déploiement rapide :

1.  Suivez les instructions de la section [L’émulateur ne démarre pas](#cant_connect) (ci-dessus).

2.  Double-cliquez sur les **Propriétés** du projet.

3.  Cliquez sur **Options Android** et désélectionnez **Utiliser Fast Deployment (mode débogage uniquement)**  :

    [![Option Utiliser le déploiement rapide désactivée](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>Glisser-déplacer ne fonctionne pas

Si l’*émulateur Visual Studio pour Android* est démarré en tant qu’administrateur (ou si vous le lancez à partir de Visual Studio alors que celui-ci s’exécute avec des privilèges d’administrateur), le glisser-déplacer de fichiers .APK ou.ZIP fichiers peut ne pas fonctionner. Pour contourner ce problème, exécutez *Émulateur Visual Studio pour Android* sans autorisations élevées (autrement dit, pas en tant qu’administrateur).


### <a name="other-errors"></a>Autres erreurs

Les conseils de dépannage ci-dessus concernent les problèmes les plus courants rencontrés lors de l’utilisation de l’émulateur Android de Visual Studio avec Xamarin.Android. Pour obtenir un guide plus complet de dépannage de l’émulateur Android de Visual Studio, consultez [Résolution des problèmes liés à l’émulateur Visual Studio pour Android](https://msdn.microsoft.com/en-us/library/mt228282.aspx).



## <a name="summary"></a>Récapitulatif

Cet article vous a présenté l’*émulateur Visual Studio pour Android*. Vous y avez appris comment utiliser l’émulateur pour déboguer des applications Xamarin.Android dans Visual Studio, avez découvert les fonctions des boutons de la barre d’outils verticale et avez pu avoir une bref aperçu des fonctionnalités disponibles dans la boîte de dialogue **Outils supplémentaires**. Vous avez appris comment utiliser le *Gestionnaire d’émulateur* pour installer, désinstaller et démarrer des profils d’appareil. Les problèmes les plus courants rencontrés lors de l’utilisateur de l’émulateur vous ont été expliqués dans une section *Résolution des problèmes*.


## <a name="related-links"></a>Liens associés

- [Émulateur Visual Studio pour Android](https://www.visualstudio.com/vs/msft-android-emulator/)
- [Présentation de l’émulateur pour Android de Visual Studio](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
