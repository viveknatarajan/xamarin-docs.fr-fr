---
title: "Émulateur en ligne de commande"
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 01ae4e1477ff5a05a5690ef24ed266b73f862748
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
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

Vous trouverez plus d’informations sur les paramètres supplémentaires sur le site Android [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
