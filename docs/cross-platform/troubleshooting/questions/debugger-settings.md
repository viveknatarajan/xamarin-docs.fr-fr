---
title: "Les paramètres de projet sont requis pour que le débogueur ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 6097dc8dfdff8807137ef68be86a08e4c9e23988
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
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

