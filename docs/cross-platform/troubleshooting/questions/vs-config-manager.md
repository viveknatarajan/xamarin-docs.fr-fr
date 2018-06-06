---
title: Pourquoi Visual Studio n’inclut pas mon projet de bibliothèque référencée dans ma build ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782220"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Pourquoi Visual Studio n’inclut pas mon projet de bibliothèque référencée dans ma build ?

Visual Studio utilise le **Configuration Manager** pour déterminer les projets dans une solution sont automatiquement inclus dans une configuration de build ou un déploiement donnée.

Certains modèles qui sont générées avec un projet de bibliothèque référencée disposent déjà de la bibliothèque référencée incluse dans la configuration ; mais sinon il doit être défini manuellement.

## <a name="how-to-use-the-configuration-manager"></a>Comment utiliser le Gestionnaire de Configuration

1. Ouvrez **Générer > Gestionnaire de Configuration**
2. Sélectionnez la configuration pour personnaliser, par exemple, **déboguer | iPhone**
3. Activez les cases à cocher pour les projets que vous souhaitez inclure.

> [!NOTE]
> Grisé zones sont gérées automatiquement et n’avez pas besoin de toutes les modifications.

Capture de ces étapes : [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
