---
title: Les paramètres de projet sont requis pour que le débogueur ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.openlocfilehash: 67c4b51a518f5c7dba6ae372dbd9206dd3ef8e9f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Les paramètres de projet sont requis pour que le débogueur ?

Dans l’ordre pour que le débogueur fonctionne comme prévu (d’atteinte de points d’arrêt, afficher les journaux de débogage, etc.), affichage d’informations développeur instrumentation et de débogage doit être activée.

Suivez ces étapes pour vérifier vos paramètres d’environnement :

## <a name="visual-studio"></a>Visual Studio
1. Ouvrir les options de projet
2. Accédez à **Générer > Avancé...** Définir les informations de débogage **complète**
3. Paramètres pour chaque plateforme :
   - Accédez à **Options Android > Options de débogage**. Graduation la **activer une Instrumentation de développeur** boîte.
   - Accédez à **Build iOS > Options de débogage**. Graduation la **activer le débogage** boîte.

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac
1. Ouvrir les options de projet
2. Accédez à **Générer > compilateur > Options générales**. Définir les informations de débogage **complète**
3. Paramètres pour chaque plateforme :
  - Accédez à **Générer > génération Android > Options de débogage**. Graduation la **activer une Instrumentation de développeur** boîte.
  - Accédez à **Générer > iOS débogage**. Graduation la **activer le débogage** boîte.

