---
title: Personnalisation de liaisons
description: "Vous pouvez personnaliser une liaison Xamarin.Android en modifiant les métadonnées qui contrôle le processus de liaison. Ces modifications manuelles sont souvent nécessaires pour résoudre les erreurs de build et de mise en forme de l’API résultant afin qu’il soit plus cohérent avec C# / .NET. Ces guides expliquent la structure de ces métadonnées, comment modifier les métadonnées et comment utiliser JavaDoc pour récupérer les noms des paramètres de méthode."
ms.topic: article
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: e71d497201cc2d8f2b3e2b8b252e5f963806a75b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="customizing-bindings"></a>Personnalisation de liaisons

_Vous pouvez personnaliser une liaison Xamarin.Android en modifiant les métadonnées qui contrôle le processus de liaison. Ces modifications manuelles sont souvent nécessaires pour résoudre les erreurs de build et de mise en forme de l’API résultant afin qu’il soit plus cohérent avec C# / .NET. Ces guides expliquent la structure de ces métadonnées, comment modifier les métadonnées et comment utiliser JavaDoc pour récupérer les noms des paramètres de méthode._

<a name="overview" />

## <a name="overview"></a>Vue d'ensemble
 
Xamarin.Android automatise en grande partie le processus de liaison ; Toutefois, dans certains cas, une modification manuelle est requise pour résoudre les problèmes suivants :

-   Résolution de générer des erreurs provoquées par des types, types obscurcis, des noms en double, problèmes de visibilité de classe et d’autres situations qui ne peut pas être résolues manquantes par les outils de Xamarin.Android. 

-   Modification du mappage de Xamarin.Android utilise pour lier l’API Android à différents types en c# (par exemple, de nombreux développeurs préfèrent mapper Java `int` constantes en c# `enum` constantes).

-   Suppression de types inutilisés qui n’ont pas besoin d’être lié. 

-   Ajout de types qui n’ont aucun équivalent dans l’API Java sous-jacente. 

Vous pouvez apporter tout ou partie de ces modifications en modifiant les métadonnées qui contrôle le processus de liaison.

<a name="guides" />

## <a name="guides"></a>Repères

Les guides suivants décrivent les métadonnées qui contrôle le processus de liaison et expliquent comment modifier ces métadonnées pour résoudre ces problèmes :

-   [Métadonnées de liaisons Java](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) fournit une vue d’ensemble des métadonnées qui sont placées dans une liaison de Java.
    Il décrit les différentes étapes manuelles sont parfois nécessaires pour terminer une bibliothèque de liaison Java, et elle explique comment mettre en forme une API exposée par une liaison à suivre plus en détail les instructions de conception de .NET.

-   [Nommer les paramètres avec Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) explique comment récupérer les noms de paramètre dans un projet de liaison Java à l’aide de Javadoc généré à partir du projet Java lié.


 

