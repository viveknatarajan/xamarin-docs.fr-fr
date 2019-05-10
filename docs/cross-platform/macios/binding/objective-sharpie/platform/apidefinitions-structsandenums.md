---
title: ApiDefinitions & StructsAndEnums fichiers
description: Ce document décrit les fichiers ApiDefinitions.cs et StructsAndEnums.cs objectif Sharpie génère. Ces fichiers sont ensuite utilisés pour accéder au code Objective-C à partir de C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3b991f6105c6053f473b049d195aaef63cbcdd57
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977678"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums fichiers

Lorsque l’objectif Sharpie s’est exécutée correctement, il génère `Binding/ApiDefinitions.cs` et `Binding/StructsAndEnums.cs` fichiers.
Ces deux fichiers sont ajoutés à un projet de liaison dans Visual Studio pour Mac ou passées directement à la `btouch` ou `bmac` outils pour produire la liaison finale.

Dans *certains* cas ces fichiers générés peuvent suffire, mais le plus souvent le développeur devra modifier manuellement ces fichiers pour résoudre les problèmes qui ne peuvent pas être gérées automatiquement par l’outil (par exemple, ces indicateurs générés avec un [ `Verify` attribut](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Parmi les étapes suivantes :

- **Ajustement de noms**: Parfois, vous devez ajuster les noms des méthodes et des classes pour faire correspondre les instructions de conception de .NET Framework.
- **Méthodes ou propriétés**: L’heuristique utilisée par objectif Sharpie parfois utilisera une méthode soit transformé en une propriété. À ce stade, vous pouvez décider s’il s’agit du comportement souhaité ou non.
- **Raccorder des événements**: Vous pouvez lier vos classes avec vos classes de délégué et générer automatiquement des événements pour ceux.
- **Raccorder les Notifications**: Il n’est pas possible d’extraire le contrat d’API de notifications à partir des fichiers d’en-tête pure, cette opération nécessite un aller-retour vers la documentation de l’API. Si vous souhaitez que les notifications fortement typées, vous devez mettre à jour le résultat.
- **Collecte de l’API**: À ce stade, vous pouvez choisir de fournir des constructeurs supplémentaires, ajoutez les méthodes (pour permettre C# syntaxe d’initialisation de construction), opérateur de surcharge et implémenter vos propres interfaces sur le fichier de définitions supplémentaires.

Consultez le [une API de liaison](~/cross-platform/macios/binding/objective-c-libraries.md) description pour voir comment ces fichiers s’intègrent dans le processus de liaison, comme illustré dans le diagramme ci-dessous :

![](apidefinitions-structsandenums-images/binding-flowchart.png "Le processus de liaison est illustré dans ce diagramme")

Reportez-vous à la [référence des Types de liaison](~/cross-platform/macios/binding/binding-types-reference.md) pour plus d’informations sur le contenu de ces fichiers.

