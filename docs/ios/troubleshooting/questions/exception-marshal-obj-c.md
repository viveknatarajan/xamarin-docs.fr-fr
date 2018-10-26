---
title: 'Pourquoi mon application iOS 9 échoue-t-elle avec : System.Exception : Impossible de marshaler l’objet Objective-C ?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: dfe1918d271946eb96d1f57d32c533a075f6d0bd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103359"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Pourquoi mon application iOS 9 échoue-t-elle avec : System.Exception : Impossible de marshaler l’objet Objective-C ?

Vous pouvez rencontrer une erreur de ce formulaire :

> System.Exception : Impossible de marshaler l’objet Objective-C... Une instance gérée existante est introuvable pour cet objet...

Modifications de l’API dans iOS 9 demander un constructeur de rappel à utiliser lors de l’appel de code non managé, comme l’API sous-jacente maintenant il attend. Utilisez la ligne suivante pour ajouter le constructeur de rappel à la classe : 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Étapes suivantes

Pour obtenir une assistance supplémentaire, pour nous contacter, ou si le reste de ce problème même après utilisant les informations ci-dessus, veuillez consulter [quelles options de prise en charge sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment signaler un bogue nouveau si nécessaire. 
