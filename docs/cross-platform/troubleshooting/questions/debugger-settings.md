---
title: Quels sont les paramètres de projet obligatoires pour le débogueur ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 9a18c97ba227615ae42529424b5c22b5e144f5e5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357860"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Quels sont les paramètres de projet obligatoires pour le débogueur ?

Dans l’ordre pour que le débogueur fonctionne comme prévu (positionnement des points d’arrêt, afficher les journaux de débogage, etc.), affichage des informations developer instrumentation et de débogage doit être activée.

Suivez ces étapes pour vérifier vos paramètres d’environnement :

## <a name="visual-studio"></a>Visual Studio
1. Ouvrir les options du projet
2. Accédez à **Générer > Avancé...** Définir les informations de débogage **complète**
3. Paramètres pour chaque plateforme :
   - Accédez à **Options Android > Options de débogage**. Graduation la **Instrumentation du développeur activer** boîte.
   - Accédez à **débogage iOS > débogage et Instrumentation**. Graduation la **activer le débogage** boîte.

## <a name="visual-studio-for-mac"></a>Visual Studio pour Mac
1. Ouvrir les options du projet
2. Accédez à **Générer > compilateur > Options générales**. Définir les informations de débogage **complète**
3. Paramètres pour chaque plateforme :
  - Accédez à **Build > Build Android > Options de débogage**. Graduation la **Instrumentation du développeur activer** boîte.
  - Accédez à **Générer > débogage iOS**. Graduation la **activer le débogage** boîte.

