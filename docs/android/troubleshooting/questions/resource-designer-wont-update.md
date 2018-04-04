---
title: Mon fichier Resource.designer.cs Android ne met pas à jour
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 6d20c92fbb61ab11fcfeda3e9d2f63fdfc5a6d54
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Mon fichier Resource.designer.cs Android ne met pas à jour

> [!NOTE]
> Ce problème a été résolu dans Xamarin Studio 5.1.4 et versions ultérieures. Toutefois, si le problème se produit dans Visual Studio pour Mac, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.

Un bogue dans Xamarin.Studio 5.1 endommagé précédemment fichiers .csproj en partiellement ou totalement supprimant le code xml dans le fichier .csproj. Cela entraînerait important système (par exemple, la mise à jour le Resource.designer.cs Android) de génération de parties de la Android échec. À compter de la 5.1.4 stable libérer le 15 juillet, ce bogue a été résolu ; mais dans de nombreux cas le fichier projet doit être réparé manuellement, comme décrit ci-dessous.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Deux approches possibles pour résoudre le fichier projet

### <a name="either"></a>Deux possibilités :

1) Créer un nouveau projet d’application de Xamarin.Android, définissez toutes les propriétés de projet pour faire correspondre votre ancien projet et ajouter toutes vos ressources, fichiers sources, etc. dans le projet.

### <a name="or"></a>OU

2) Faites une copie de sauvegarde du fichier .csproj de votre projet d’origine, puis ouvrez-le dans un éditeur de texte et restaurent les éléments manquants à partir d’un fichier .csproj correctement généré.

### <a name="if-this-does-not-solve-the-problem"></a>Si cela ne résout pas le problème

Après avoir essayé de créer ces éléments, vous pouvez remarquer qu’après l’ajout d’éléments et régénérer le projet, le fichier Resource.designer.cs est mise à jour, toutefois, puis vous pourrez encore fermez et rouvrez la solution pour obtenir l’exécution de code à reconnaître les nouveaux types de contenus dans Resource.designer.cs. 
