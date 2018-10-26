---
title: Personnalisation de liaisons
description: Vous pouvez personnaliser une liaison Xamarin.Android en modifiant les métadonnées qui contrôle le processus de liaison. Ces modifications manuelles sont souvent nécessaires pour la résolution des erreurs de build et mise en forme de l’API qui en résulte afin qu’il soit plus cohérent avec C#/.NET. Ces guides expliquent la structure de ces métadonnées, comment modifier les métadonnées et comment utiliser JavaDoc pour récupérer les noms des paramètres de méthode.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: 44bff372225ee1bf555eb3eeb34da918830980b4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102384"
---
# <a name="customizing-bindings"></a>Personnalisation de liaisons

_Vous pouvez personnaliser une liaison Xamarin.Android en modifiant les métadonnées qui contrôle le processus de liaison. Ces modifications manuelles sont souvent nécessaires pour la résolution des erreurs de build et mise en forme de l’API qui en résulte afin qu’il soit plus cohérent avec C#/.NET. Ces guides expliquent la structure de ces métadonnées, comment modifier les métadonnées et comment utiliser JavaDoc pour récupérer les noms des paramètres de méthode._


## <a name="overview"></a>Vue d'ensemble
 
Xamarin.Android automatise en grande partie le processus de liaison ; Toutefois, dans certains cas, une modification manuelle est nécessaire pour résoudre les problèmes suivants :

-   Résolution des erreurs de buiid dû à l’absence de types, des types masqués, les noms en double, problèmes de visibilité de classe et autres situations qui ne peut pas être résolues par les outils Xamarin.Android. 

-   Modification du mappage de Xamarin.Android utilise pour lier l’API Android à différents types de C# (par exemple, de nombreux développeurs préfèrent mapper Java `int` constantes à C# `enum` constantes).

-   Suppression des types inutilisés qui ne doivent pas être lié. 

-   Ajout de types qui n’ont aucun équivalent dans l’API Java sous-jacente. 

Vous pouvez rendre certaines ou toutes ces modifications en modifiant les métadonnées qui contrôle le processus de liaison.


## <a name="guides"></a>Repères

Les guides suivants décrivent les métadonnées qui contrôle le processus de liaison et expliquent comment modifier ces métadonnées pour résoudre ces problèmes :

-   [Métadonnées de liaisons Java](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) fournit une vue d’ensemble des métadonnées qui sont placées dans une liaison de Java.
    Il décrit les différentes étapes manuelles sont parfois nécessaires pour terminer une bibliothèque de liaison Java, et elle explique comment mettre en forme une API exposée par une liaison à mieux suivre les instructions de conception de .NET.

-   [Nommer les paramètres avec Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) explique comment récupérer les noms de paramètres dans un projet de liaison Java à l’aide de Javadoc produite à partir du projet Java lié.


 

