---
title: Émulateur en ligne de commande
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 11/05/2018
ms.openlocfilehash: 995b0783604f752915daaa77a8362899ac61e174
ms.sourcegitcommit: f541a92b4f896474f6a5467ccff2028dafa6fee7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "50983586"
---
# <a name="command-line-emulator"></a>Émulateur en ligne de commande

## <a name="running-the-android-emulator-from-the-command-line"></a>Exécution de l’émulateur Android à partir de la ligne de commande

Pour permettre l’exécution de l’émulateur Android à partir de la ligne de commande, vous pouvez utiliser l’outil « émulateur » fourni par le kit de développement logiciel Android. Cet outil peut être utilisé pour exécuter l’émulateur à partir de Terminal sur OS X, ou à partir de l’invite de commandes sur un ordinateur Windows.

Pour lancer un émulateur Android spécifique, exécutez la commande suivante à partir du répertoire d’outils à l’emplacement du kit de développement logiciel Android (par exemple, C:\android-sdk-windows\tools) :

Sur Windows

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

Sur macOS

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

On a besoin de la taille de partition afin que l’émulateur offre suffisamment d’espace pour installer la plateforme Xamarin.Android dessus. En effet, par défaut, l’émulateur est de petite taille.

Vous trouverez plus d’informations sur les paramètres supplémentaires sur le site Android ici : [https://developer.android.com/studio/run/emulator-commandline](https://developer.android.com/studio/run/emulator-commandline)
