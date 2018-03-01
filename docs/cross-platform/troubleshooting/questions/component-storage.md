---
title: "Les composants de stockage sur mon ordinateur ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Les composants de stockage sur mon ordinateur ?

Chaque fois que vous installez un composant Xamarin dans un projet d’application, il est placé dans deux emplacements :

1. Dans un dossier de composants à la racine de votre dossier de Solution. Si vous supprimez le composant à partir de tous les projets de la solution, il sera sont supprimé à partir de ce dossier.

2. Une copie est également stockée dans les emplacements suivants :
    - Windows : `%LocalAppData%\Xamarin\Cache\Components`
    - Mac : `~/Library/Caches/Xamarin/Components`

Par conséquent, pour supprimer un composant de votre système, supprimez-le de vos projets et des solutions et à partir du dossier de cache ci-dessus.
