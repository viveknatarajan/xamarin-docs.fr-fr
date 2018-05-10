---
title: Pourquoi Visual Studio n’inclut pas mon projet de bibliothèque référencée dans ma build ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 8a35c8e0cd03d5ff07405647b08275ef9ca3758d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Pourquoi Visual Studio n’inclut pas mon projet de bibliothèque référencée dans ma build

Visual Studio utilise le **Configuration Manager** pour déterminer les projets dans une solution sont automatiquement inclus dans une configuration de build ou un déploiement donnée.

Certains modèles qui sont générées avec un projet de bibliothèque référencée disposent déjà de la bibliothèque référencée incluse dans la configuration ; mais sinon il doit être défini manuellement.

## <a name="how-to-use-the-configuration-manager"></a>Comment utiliser le Gestionnaire de Configuration

1. Ouvrez **Générer > Gestionnaire de Configuration**
2. Sélectionnez la configuration pour personnaliser, par exemple, **déboguer | iPhone**
3. Activez les cases à cocher pour les projets que vous souhaitez inclure.

> [!NOTE]
> Grisé zones sont gérées automatiquement et n’avez pas besoin de toutes les modifications.

Capture de ces étapes : [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
