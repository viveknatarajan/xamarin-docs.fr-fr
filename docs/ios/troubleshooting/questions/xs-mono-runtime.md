---
title: Comment définir des variables d’environnement Mono Runtime pour les projets iOS dans Xamarin Studio ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 5f4f3a2de012d35ddca9c1fa830d599d9d5acb17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Comment définir des variables d’environnement Mono Runtime pour les projets iOS dans Xamarin Studio ?

Si vous avez besoin définir des variables d’environnement n’importe quel runtime pour Mono, elles peuvent être définies dans le **Options du projet > Exécuter > Général** page.

Remarque : Variables d’environnement le Garbage Collection pour SGen (MONO\_GC\_PARAMS) jeu de cette façon sera utilisée uniquement lors du lancement de Xamarin Studio. Si vous lancez l’application à partir de l’appareil, les paramètres de Sgen seront ignorés. 

Pour définir une variable d’environnement pour une application de manière permanente, vous devez ajouter cet élément comme argument mtouch supplémentaires (pour toutes les configurations appropriées) :

```csharp
   --setenv=NAME=VALUE
```

Pour voir les variables d’environnement qui peuvent être définies, reportez-vous à la page man Mono : [ http://docs.go-mono.com/?link=man%3amono(1) ](http://docs.go-mono.com/?link=man%3amono(1)) , consultez la section intitulée : `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Définition des variables d’environnement pour un projet")
