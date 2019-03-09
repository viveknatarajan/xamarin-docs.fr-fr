---
title: Test sur les appareils Apple Watch
description: Ce document décrit comment déployer une application watchOS générée avec Xamarin pour le test sur une Apple Watch réelle. Il traite des appareils, profils, de test, de configuration et fournit des conseils de dépannage.
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 9c15e9205b96a02caa182e47b71c6d36c8bff1aa
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671050"
---
# <a name="testing-on-apple-watch-devices"></a>Test sur les appareils Apple Watch

Une fois que vous avez suivi le [étapes de déploiement](~/ios/watchos/deploy-test/index.md) pour créer des ID d’application et des groupes d’applications (si nécessaire), utilisez les instructions de cette page pour :

- [Configurer vos appareils](#devices) dans le centre de développement Apple, et
- [Créer des profils de provisionnement de développement](#profiles), puis
- [Déployer et tester](#testing) sur une Apple Watch.

<a name="devices" />

## <a name="devices"></a>Appareils

Test des applications iOS sur un véritable iPhone ou iPad a toujours nécessité l’appareil soit inscrit sur le centre de développement. La liste des appareils ressemble à ceci (cliquez sur le signe **+** pour ajouter un nouvel appareil) :

![](device-images/devices-sml.png "La liste des appareils ressemble à ceci")

Observations ne sont pas différentes : vous devez maintenant ajouter votre appareil Apple Watch avant de déployer des applications à ce dernier. Rechercher à l’aide de UDID l’espion **Xcode** (**Windows > appareils** liste). Lorsque le téléphone jumelé est connecté les informations de l’observation affichera également :

[![](device-images/xcode-devices-sml.png "Informations une Watch")](device-images/xcode-devices.png#lightbox)

Lorsque vous connaissez l’espion UDID, ajoutez-le à la liste des appareils dans le centre de développement :

![](device-images/devices-watch-sml.png "L’espion UDID dans la liste des appareils")

Une fois que l’appareil Watch a été ajouté, vérifiez qu’il est sélectionné dans n’importe quel de développement de nouveau ou existant ou vous créez des profils d’approvisionnement ad hoc :

![](device-images/devices-provisioning.png "Liste des appareils disponibles")

N’oubliez pas si vous modifiez un profil de configuration existant pour télécharger et réinstaller !

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Profils de provisionnement de développement

Pour générer des tests sur votre appareil, vous devez créer un **profil de provisionnement de développement** pour chaque ID d’application dans votre solution.

Si vous avez un ID d’application, de caractère générique *qu’un seul profil de provisionnement devront*; Toutefois, si vous avez un ID d’application distinct pour chaque projet vous aurez besoin un profil de provisionnement pour chaque ID d’application :

![](device-images/provisioningprofile-development.png "Le profil de provisionnement de développement")

Une fois que vous avez créé les trois profils, ceux-ci s’affichent dans la liste. N’oubliez pas de télécharger et installer chacun d’eux :

![](device-images/provisioningprofiles.png "Les profils de provisionnement de développement disponibles")

Vous pouvez vérifier le profil d’approvisionnement dans le **Options du projet** en sélectionnant le **Générer > signature du Bundle iOS** écran et en sélectionnant le **version** ou **Déboguer iPhone** configuration.

Le **profil de provisionnement** liste affiche tous les profils correspondants : vous devez voir les profils correspondants que vous avez créée dans cette liste déroulante :

![](device-images/options-selectprofile.png "La liste des profils de provisionnement")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Tests sur un appareil espion

Une fois que vous avez configuré votre appareil, les ID d’application et les profils de provisionnement, vous êtes prêt à tester.

1. Assurez-vous que votre iPhone est branché, et l’observation est déjà associée à l’iPhone.

2. Vérifiez la configuration est définie sur **version** ou **déboguer**.

3. Assurez-vous que le périphérique connecté iPhone est sélectionné dans la liste cible.

4. Avec le bouton droit sur le projet d’application iOS (pas le suivi ou l’extension) et choisissez **définir comme projet de démarrage**.

5. Cliquez sur le **exécuter** bouton (ou choisissez un **Démarrer** option à partir de la **exécuter** menu).

6. La solution est générée et l’application iOS sera déployée sur l’iPhone.
  Si l’application iOS ou l’approvisionnement de l’extension watch n’est pas définie correctement déploiement pour l’iPhone échouera.

7. Si le déploiement terminé, l’iPhone tentera automatiquement envoyer l’application Apple watch à l’une Watch. Icône de votre application s’affiche sur l’écran de surveillance avec un cercle *installation* indicateur de progression.

8. Si l’application watch est installée avec succès, l’icône reste sur l’écran watch - touch pour commencer à tester votre application !


## <a name="troubleshooting"></a>Résolution des problèmes

Si une erreur se produit lors de l’utilisation de déploiement le **Affichage > blocs > journal de l’appareil** pour voir plus d’informations sur l’erreur. Certaines erreurs et leurs causes sont répertoriées ci-dessous :

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Erreur MT3001 : Impossible de pas AOT l’assembly

Cela peut se produire lors de la génération en mode débogage pour le déployer sur un appareil Apple Watch.

Pour *temporairement* contourner ce problème, désactivez **Builds incrémentielles** dans l’Extension Watch **Options du projet > Build > Build watchOS** fenêtre :

[![](device-images/disable-incremental-sml.png "La case à cocher Builds incrémentielles")](device-images/disable-incremental.png#lightbox)

Ce problème sera résolu dans une version ultérieure, après laquelle les builds incrémentielles peuvent être réactivées pour tirer parti des durées de génération plus rapides.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Regardez le Qu'application ne parvient pas à démarrer lors du débogage sur l’appareil

Quand une tentative de débogage d’une application watch sur un appareil physique, uniquement l’icône et le compteur de chargement s’affichent (et finalement délai d’attente). Cela sera résolu dans une version ultérieure. une solution de contournement consiste à exécuter une version Release (qui ne permet pas de débogage).


### <a name="invalid-application-executable-or-application-verification-failed"></a>Exécutable d’Application non valide ou de la vérification d’Application a échoué

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Alerte d’exécutable de l’Application non valide")

Si ces messages apparaissent *sur l’écran espion* après l’application a tenté d’installer, il peut y avoir un ou deux problèmes :

- L’appareil espion lui-même n’a pas été ajouté en tant qu’appareil sur le centre de développement Apple. Suivez les instructions pour [configurer correctement des appareils](#devices).

- Les profils de provisionnement de développement utilisés pour le test ne disposait pas de l’appareil espion inclus ; ou, une fois que la surveillance a été ajoutée pour les profils de provisionnement qu’ils n’ont pas été téléchargées de nouveau et réinstallés. Suivez les instructions pour [configurer les profils de provisionnement correctement](#profiles).

- Si le **iOS journal de l’appareil** contient `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` puis regardez l’application **Info.plist** a incorrecte **MinimumOSVersion** valeur.
  Cette valeur doit être **8.2** : Si vous avez installé 6.3 Xcode vous devrez peut-être modifier manuellement la source à insérer affectez-lui 8.2.

- L’application Watch **Entitlements.plist** incorrectement a un droit activé (par exemple, des groupes d’applications) qu’il ne doit pas avoir.

- L’application Watch **ID d’application** incorrectement a un droit activé (par exemple, des groupes d’applications) dans le centre de développement qui ne doit lui être.



### <a name="install-never-finished"></a>N’installez jamais terminé

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Cette erreur peut indiquer des clés inutiles (et non valides) dans l’application Watch **Info.plist** fichier. Vous devez inclure pas de clés destinés à l’extension d’application ou regarder iOS dans l’application Watch.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>« en attente pour la connexion du débogueur »

Si le **sortie de l’Application** fenêtre se bloque en affichant

```csharp
waiting for debugger to connect
```

Vérifiez si un des packages NuGet qui ont été incluses dans votre projet a une dépendance **Microsoft.Bcl.Build**. Cela est ajoutée automatiquement avec certaines bibliothèques publiée par Microsoft, y compris le fameux [Microsoft Http Client Libraries](https://www.nuget.org/packages/Microsoft.Net.Http/).

Le **Microsoft.Bcl.Build.targets** fichier est ajouté à la **.csproj** peut interférer avec l’empaquetage des extensions iOS lors du déploiement. Vous pouvez suivre la [bogue](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
Une solution de contournement possible consiste à modifier le fichier .csproj et déplacer manuellement le **Microsoft.Bcl.Build.targets** pour être le dernier élément.

