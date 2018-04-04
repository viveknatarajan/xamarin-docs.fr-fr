---
title: Les composants de stockage sur mon ordinateur ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Les composants de stockage sur mon ordinateur ?

Chaque fois que vous installez un composant Xamarin dans un projet d’application, il est placé dans deux emplacements :

1. Dans un dossier de composants à la racine de votre dossier de Solution. Si vous supprimez le composant à partir de tous les projets de la solution, il sera sont supprimé à partir de ce dossier.

2. Une copie est également stockée dans les emplacements suivants :
    - Windows : `%LocalAppData%\Xamarin\Cache\Components`
    - Mac : `~/Library/Caches/Xamarin/Components`

Par conséquent, pour supprimer un composant de votre système, supprimez-le de vos projets et des solutions et à partir du dossier de cache ci-dessus.
