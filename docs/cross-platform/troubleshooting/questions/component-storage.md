---
title: Les composants de stockage sur mon ordinateur ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.openlocfilehash: a53045a6179a26b30d824976d11fd2769a84811e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Les composants de stockage sur mon ordinateur ?

Chaque fois que vous installez un composant Xamarin dans un projet d’application, il est placé dans deux emplacements :

1. Dans un dossier de composants à la racine de votre dossier de Solution. Si vous supprimez le composant à partir de tous les projets de la solution, il sera sont supprimé à partir de ce dossier.

2. Une copie est également stockée dans les emplacements suivants :
    - Windows : `%LocalAppData%\Xamarin\Cache\Components`
    - Mac : `~/Library/Caches/Xamarin/Components`

Par conséquent, pour supprimer un composant de votre système, supprimez-le de vos projets et des solutions et à partir du dossier de cache ci-dessus.
