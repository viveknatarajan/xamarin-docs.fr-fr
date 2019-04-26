---
title: Pourquoi Visual Studio ne contient pas mon projet de bibliothèque référencée dans ma build ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341306"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Pourquoi Visual Studio ne contient pas mon projet de bibliothèque référencée dans ma build ?

Visual Studio utilise le **Configuration Manager** pour déterminer quels projets d’une solution sont automatiquement inclus dans une configuration de build ou déploiement donnée.

Certains modèles qui sont générées avec un projet de bibliothèque référencée possède déjà la bibliothèque référencée incluse dans la configuration ; mais sinon il doit être définie manuellement.

## <a name="how-to-use-the-configuration-manager"></a>Comment utiliser le Gestionnaire de Configuration

1. Ouvrez **Générer > Configuration Manager**
2. Sélectionnez la configuration à personnaliser, par exemple, **Debug | iPhone**
3. Activez les cases à cocher pour les projets que vous souhaitez inclure.

> [!NOTE]
> Grisés zones sont gérées automatiquement et ne doit pas besoin d’être modifié.

Capture vidéo de ces étapes : [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
