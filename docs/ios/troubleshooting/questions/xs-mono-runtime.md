---
title: Comment définir des variables d’environnement Mono Runtime pour des projets iOS dans Xamarin Studio ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417659"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Comment définir des variables d’environnement Mono Runtime pour des projets iOS dans Xamarin Studio ?

Si vous avez besoin définir des variables d’environnement de n’importe quel runtime pour Mono, ils peuvent être définies dans le **Options du projet > Exécuter > Général** page.

Remarque : Variables d’environnement Garbage Collection pour SGen (MONO\_GC\_PARAMS) jeu de cette façon servira uniquement lors du lancement de Xamarin Studio. Si vous lancez l’application à partir de l’appareil, les paramètres de Sgen seront ignorés. 

Pour définir une variable d’environnement pour une application de manière permanente, vous devez ajouter cela comme argument mtouch supplémentaires (pour toutes les configurations appropriées) :

```csharp
   --setenv=NAME=VALUE
```

Pour afficher les variables d’environnement qui peuvent être définies, reportez-vous à la page man Mono :  [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) Consultez la section intitulée : `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Définition des variables d’environnement pour un projet")
