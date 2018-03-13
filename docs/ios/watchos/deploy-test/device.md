---
title: Test sur les appareils espion
description: "Déploiement d’applications à tester sur votre Apple Watch"
ms.topic: article
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 211f8c7066e86dac3a8351b913da0185093dcb70
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="testing-on-watch-devices"></a>Test sur les appareils espion

Une fois que vous avez suivi la [les étapes de déploiement](~/ios/watchos/deploy-test/index.md) pour créer des ID d’application et des groupes d’applications (si nécessaire), suivez les instructions de cette page pour :

- [Configurer vos périphériques](#devices) dans le centre de développement Apple, et
- [Créer des profils de configuration de développement](#profiles), puis
- [Déployer et tester](#testing) sur une Apple Watch.

<a name="devices" />

## <a name="devices"></a>Appareils

Test des applications iOS sur un iPhone réel ou un iPad exige toujours le périphérique à enregistrer sur le centre de développement. La liste des appareils ressemble à ceci (cliquez sur le signe plus  **+**  pour ajouter un nouvel appareil) :

![](device-images/devices-sml.png "La liste des appareils ressemble à ceci")

Observations ne sont pas différentes : vous devez maintenant ajouter votre appareil Apple Watch avant de déployer des applications à celui-ci. Recherche la surveillance à l’aide de UDID **Xcode** (**Windows > appareils** liste). Lorsque le téléphone apparié est connecté les informations de la surveillance affichera également :

[![](device-images/xcode-devices-sml.png "Informations de surveillance appariés")](device-images/xcode-devices.png#lightbox)

Lorsque vous connaissez l’observation UDID, ajoutez-le à la liste des périphériques dans le centre de développement :

![](device-images/devices-watch-sml.png "De la surveillance UDID dans la liste des appareils")

Une fois que l’appareil espion a été ajouté, vérifiez qu’il est sélectionné dans n’importe quel nouveau ou existant de développement ou des profils de configuration ad-hoc que vous créez :

![](device-images/devices-provisioning.png "Liste des périphériques disponibles")

N’oubliez pas si vous modifiez un profil de configuration existant pour télécharger et réinstaller le programme.

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Développement de profils de configuration

Pour générer pour le test sur votre appareil, vous devez créer un **le profil d’approvisionnement développement** pour chaque ID d’application dans votre solution.

Si vous disposez d’un ID d’application, de caractère générique *qu’un profil de configuration sera nécessaire*; mais si vous avez un ID d’application distinct pour chaque projet, vous devez avoir un profil de configuration pour chaque ID d’application :

![](device-images/provisioningprofile-development.png "Le profil de configuration de développement")

Une fois que vous avez créé les trois profils, elles apparaissent dans la liste. N’oubliez pas de télécharger et installer chacun d’eux :

![](device-images/provisioningprofiles.png "Les profils de configuration de développement disponibles")

Vous pouvez vérifier le profil de configuration dans le **Options du projet** en sélectionnant le **Générer > iOS signature d’offre groupée** écran et en sélectionnant le **version** ou **Déboguer iPhone** configuration.

Le **profil de préparation** liste affiche tous les profils de correspondance, vous devez voir les profils de correspondance que vous avez créé dans cette liste déroulante :

![](device-images/options-selectprofile.png "La liste des profils de configuration")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Test sur un périphérique espion

Une fois que vous avez configuré votre appareil, les ID d’application et les profils de configuration, vous êtes prêt à tester.

1. Assurez-vous que votre iPhone est branché et la surveillance est déjà associée avec l’iPhone.

2. Vérifiez la configuration est définie sur **version** ou **déboguer**.

3. Assurez-vous que le périphérique connecté iPhone est sélectionné dans la liste cible.

4. Avec le bouton droit sur le projet d’application iOS (pas l’espion ou l’extension) et choisissez **définir comme projet de démarrage**.

5. Cliquez sur le **exécuter** bouton (ou choisissez un **Démarrer** option à partir de la **exécuter** menu).

6. La solution sera générée et l’application iOS sera déployée sur l’iPhone.
  Si l’application iOS ou la configuration des extensions d’espion n’est pas définie correctement, le déploiement pour l’iPhone échoue.

7. Si le déploiement est terminé avec succès, l’iPhone tente automatiquement envoyer l’application de surveillance pour la surveillance appariée. Icône de l’application s’affiche sur l’écran de surveillance avec un cercle *installation* indicateur de progression.

8. Si l’application de la surveillance est correctement installée, l’icône reste sur l’écran de surveillance - touch pour commencer à tester votre application !


## <a name="troubleshooting"></a>Résolution des problèmes

Si une erreur se produit lors de l’utilisation du déploiement la **vue > remplit > journal d’appareil** pour plus d’informations sur l’erreur. Des erreurs et leurs causes sont répertoriées ci-dessous :

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Erreur MT3001 : Ne peut pas AOA l’assembly

Cela peut se produire lors de la génération en mode débogage pour déployer sur un appareil Apple Watch.

Pour *temporairement* contourner ce problème, désactivez **générations incrémentielles** dans l’Extension Watch **Options du projet > Générer > watchOS Build** fenêtre :

[![](device-images/disable-incremental-sml.png "La case à cocher générations incrémentielles")](device-images/disable-incremental.png#lightbox)

Ce problème sera corrigé dans une version ultérieure, après quoi les générations incrémentielles peuvent être réactivées pour tirer parti des durées de génération plus rapides.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Surveiller le Qu'application ne parvient pas à démarrer lors du débogage sur l’appareil

Lorsqu’une tentative de débogage d’une application de surveillance sur une unité physique, uniquement l’icône & le compteur de chargement s’affichent (et finalement délai d’attente). Cela sera résolu dans une version ultérieure. une solution de contournement consiste à exécuter une version Release (qui n’autorise pas le débogage).


### <a name="invalid-application-executable-or-application-verification-failed"></a>Fichier exécutable d’Application non valide ou de vérification d’Application a échoué

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Alerte exécutable d’Application non valide")

Si ces messages apparaissent *sur l’écran de surveillance* une fois que l’application a tenté d’installer, il peut y avoir deux problèmes :

- L’appareil espion lui-même n’a pas été ajouté en tant que périphérique sur le centre de développement Apple. Suivez les instructions pour [configurer correctement les appareils](#devices).

- Les profils de configuration de développement utilisés pour le test n’ont pas de l’appareil espion inclus ; ou, après l’ajout de la surveillance pour les profils de configuration qu’ils n’ont pas été téléchargées de nouveau et réinstallés. Suivez les instructions pour [configurer les profils de configuration correctement](#profiles).

- Si le **iOS périphérique journal** contient `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` puis Espion l’application **Info.plist** a incorrect **MinimumOSVersion** valeur.
  Il doit s’agir **8.2** : Si vous avez installé 6.3 Xcode vous devrez peut-être modifier manuellement la source à insérer affectez-lui 8.2.

- L’application de surveillance **Entitlements.plist** incorrecte a un droit activé (par exemple, des groupes d’applications) qu’il ne doit pas posséder de.

- L’application de surveillance **ID d’application** incorrectement a un droit activé (par exemple, des groupes d’applications) dans le centre de développement, il ne doit pas avoir.



### <a name="install-never-finished"></a>N’installez jamais terminé

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Cette erreur peut indiquer des clés inutiles (et non valides) dans l’application de surveillance **Info.plist** fichier. Vous devez inclure pas de clés destinés à l’extension d’application ou Espion iOS dans l’application de surveillance.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>« en attente pour le débogueur pour la connexion »

Si le **sortie de l’Application** fenêtre se bloque en affichant

```csharp
waiting for debugger to connect
```

Vérifiez si un de le NuGets qui ont été incluses dans votre projet est une dépendance **Microsoft.Bcl.Build**. Il est automatiquement ajouté avec certaines bibliothèques publiée par Microsoft, y compris le courant [Microsoft Http Client Libraries](http://www.nuget.org/packages/Microsoft.Net.Http/).

Le **Microsoft.Bcl.Build.targets** fichier est ajouté à la **.csproj** peut interférer avec l’empaquetage des extensions iOS durant le déploiement. Vous pouvez suivre le [bogue](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
Une solution de contournement possible consiste à modifier le fichier .csproj et déplacer manuellement les **Microsoft.Bcl.Build.targets** pour être le dernier élément.

