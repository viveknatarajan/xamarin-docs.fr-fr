---
title: ApiDefinitions & StructsAndEnums fichiers
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cd0d32d48d96e2f4c2c109bfb79864fa77b8268f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums fichiers

Lorsque l’objectif Sharpie a exécuté avec succès, il génère `Binding/ApiDefinitions.cs` et `Binding/StructsAndEnums.cs` fichiers.
Ces deux fichiers sont ajoutés à un projet de liaison dans Visual Studio pour Mac ou transmis directement à la `btouch` ou `bmac` outils pour produire la liaison finale.

Dans *certains* cas ces fichiers générés peuvent être tout ce que vous devez, toutefois plus souvent le développeur doit modifier manuellement ces fichiers pour résoudre les problèmes qui ne peuvent pas gérées automatiquement par l’outil (par exemple, ces indicateurs de générés avec un [ `Verify` attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Parmi les étapes suivantes :

- **Ajustement de noms**: parfois, vous devez ajuster les noms des méthodes et des classes pour faire correspondre les instructions de conception de .NET Framework.
- **Méthodes ou propriétés**: l’heuristique utilisée par un objectif Sharpie parfois sélectionnent une méthode à être transformée en une propriété. À ce stade, vous pouvez décider s’il s’agit du comportement souhaité ou non.
- **Raccorder des événements**: Impossible de lier vos classes avec vos classes de délégué et de générer automatiquement des événements pour les.
- **Raccorder les Notifications**: il n’est pas possible d’extraire le contrat pour l’API des notifications à partir des fichiers d’en-tête pure, cette opération nécessite un aller-retour vers la documentation de l’API. Si vous souhaitez que les notifications fortement typées, vous devez mettre à jour le résultat.
- **Le site d’API**: À ce stade, vous pouvez choisir fournir des constructeurs supplémentaires, ajouter des méthodes (pour autoriser la syntaxe c# initialize-sur-construction), la surcharge d’opérateur et implémenter vos propres interfaces sur le fichier de définitions supplémentaires.

Consultez le [une API de liaison](~/cross-platform/macios/binding/objective-c-libraries.md) description pour voir comment ces fichiers dans le processus de liaison, comme indiqué dans le diagramme ci-dessous :

![](apidefinitions-structsandenums-images/binding-flowchart.png "Le processus de liaison est représenté dans ce diagramme")

Reportez-vous à la [référence des Types de liaison](~/cross-platform/macios/binding/binding-types-reference.md) pour plus d’informations sur le contenu de ces fichiers.

