---
title: Débogage des applications Xamarin.iOS avec Xcode
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514706"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Débogage des applications Xamarin.iOS avec Xcode

Il peut y avoir des scénarios où vous souhaitez utiliser Xcode pour déboguer certaines parties de votre application Xamarin.iOS. Bien que vous ne pourrez pas déboguer le code .NET qu’il contient, vous serez toujours en mesure de déboguer le code natif et d’utiliser certaines des visualiseurs natifs dans Xcode.

## <a name="walkthrough"></a>Procédure pas à pas

Il n’existe aucune prise en charge intégrée pour le débogage de Xcode dans Visual Studio pour Mac, vous pouvez utiliser les étapes suivantes pour y parvenir :

1. Créer une application iOS de Xcode avec l’ID de bundle de même que celui de votre application Xamarin.
   
    - Vous pouvez trouver l’identificateur de bundle de votre projet Xamarin.iOS en ouvrant le **Info.plist** fichier :

        ![Modification du fichier Info.plist](debugging-with-xcode-images/vsmac-infoplist.png "Info.list de modification")

    - Dans Xcode, vous définissez l’identificateur de bundle lors de la création de votre projet ou en sélectionnant votre cible dans le projet :

        ![Définition de l’identificateur de Bundle dans Xcode](debugging-with-xcode-images/xcode-bundle.png "définissant l’identificateur de Bundle dans Xcode")

2. Modifier le projet Xcode à attendre pour le lancement au lieu de lancer l’application automatiquement :

    - Ouvrir le **modifier le volet schéma** en sélectionnant **produit > schéma > modifier le modèle de** ou à l’aide de la **cmd⌘ + <** raccourci clavier.

    - Sélectionnez le **exécuter** schéma et dans le droit volet vous devez voir **lancer** options. Sélectionnez **attendre exécutable à lancer** et cliquez sur **fermer**.

        ![Attendez que l’exécutable à lancer](debugging-with-xcode-images/xcode-schemes.png "attendre exécutable à lancer")

3. Exécutez le projet Xcode.

    Cela installe l’application Xcode factice sur votre appareil, mais il lancera pas.

4. Exécutez l’application Xamarin.

    Xcode doit être attaché à l’application Xamarin lors du lancement.

### <a name="caveats"></a>Avertissements

Vous devrez peut-être apporter une petite modification à l’application de Xamarin.iOS chaque fois que vous lancez. Sinon, Visual Studio pour Mac détecte que l’application n’a pas besoin être généré *et* est déjà installé, et il ne le réinstaller sur l’application factice Xcode.

## <a name="alternative---using-lldb"></a>Autre possibilité : à l’aide de lldb

Si vous êtes familiarisé avec l’utilisation **lldb** à partir de la ligne de commande, il existe une solution beaucoup plus simple.

Sur l’interpréteur de commandes, tapez la commande suivante :

```bash
touch ~/.mtouch-launch-with-lldb
```

Vous obtiendrez des instructions le **sortie de l’Application** fenêtre sur quoi faire, mais en fait, lorsque vous exécutez votre application, vous serez en mesure d’utiliser **lldb** à partir de la ligne de commande pour déboguer votre application.
