---
title: "Pourquoi mon application iOS 9 n’échoue avec : System.Exception : Impossible de marshaler l’objet Objective-C ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: baba2526eefa1b69d47da47b73ea0bd417ecdc57
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Pourquoi mon application iOS 9 n’échoue avec : System.Exception : Impossible de marshaler l’objet Objective-C ?

Vous pouvez voir une erreur de ce formulaire :

> System.Exception : Impossible de marshaler l’objet Objective-C... Impossible de trouver une instance managée existante pour cet objet...

Modifications de l’API dans iOS 9 demander un constructeur de rappel à utiliser lors de l’appel de code non managé, comme l’API sous-jacente maintenant s’attend. Utilisez la ligne suivante pour ajouter le constructeur de rappel à la classe : 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Étapes suivantes

Pour obtenir une assistance pour nous contacter, ou si ce problème persiste même après utilisant les informations ci-dessus, consultez [les options de support sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment entrer un bogue nouveau si nécessaire. 
