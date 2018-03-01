---
title: "Erreur d’exécution : le fichier d’assembly mscorlib.dll est introuvable ou ne peut pas être chargé"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: fd5ba59841142306df7c65e97e0f16ff5654d5b2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>Erreur d’exécution : le fichier d’assembly mscorlib.dll est introuvable ou ne peut pas être chargé

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

Ce problème se produit lorsque le *masqué* `.monotouch-32` et `.monotouch-64` dossiers sont absents de la `.xcarchive` pour la signature / création IPA, déclenchement de l’erreur d’exécution.

