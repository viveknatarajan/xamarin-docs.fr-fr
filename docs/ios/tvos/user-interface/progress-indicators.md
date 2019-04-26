---
title: Utilisation des indicateurs de progression dans Xamarin tvOS
description: Ce document décrit comment utiliser des indicateurs de progression dans une application tvOS générées avec Xamarin. Il aborde les barres de progression et les indicateurs de l’activité.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/25/2018
ms.openlocfilehash: cbd2b2de237a5bb22d1dc0242569b96b12bca070
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61180613"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>Utilisation des indicateurs de progression dans Xamarin tvOS

_Cet article décrit la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS._

Il peut arriver lorsque votre application Xamarin.tvOS doit charger le nouveau contenu ou effectuer une opération de traitement de longue durée. Durant ces périodes, vous devez présenter un indicateur d’activité ou une barre de progression pour informer l’utilisateur que l’application est en cours d’exécution et leur donner des indications quant à la durée de la tâche en cours d’exécution.

![Exemples d’indicateurs de progression](progress-indicators-images/intro01.png "exemples d’indicateurs de progression")

## <a name="about-activity-indicators"></a>Sur les indicateurs de l’activité

Un indicateur d’activité présente sous la forme d’une roue dentée en rotation et est utilisé pour représenter une tâche d’une durée indéterminée. L’indicateur est présenté quand la tâche démarre et disparaît lorsque la tâche est terminée.

Apple a les suggestions suivantes pour l’utilisation des indicateurs de l’activité :

- **Si possible, utilisez à la place les barres de progression** - parce qu’une offre d’indicateur activité prendra de l’utilisateur, aucun commentaire dans la procédure de long du processus en cours d’exécution, utilisez toujours une barre de progression si la longueur est connue (par exemple, le nombre d’octets à télécharger dans un fichier).
- **Conserver l’indicateur animée** -les utilisateurs un indicateur d’activité stationnaire d'association à une application bloquée, donc vous devez toujours animer l’indicateur qui est affichée.
- **Décrire la tâche en cours de traitement** -simplement afficher l’indicateur d’activité par lui-même ne suffit pas ; l’utilisateur doit être informé sur le processus sur lequel ils sont en attente. Inclure une étiquette explicite (généralement une phrase unique et complet) qui définit clairement la tâche.

## <a name="about-progress-bars"></a>À propos des barres de progression

Une barre de progression se présente comme une ligne qui remplit avec la couleur pour indiquer l’état et la longueur d’une tâche fastidieuse. Barres de progression doivent toujours être utilisés lors de la longueur des tâches est connue ou peut être calculée.

Apple a les suggestions suivantes pour l’utilisation des barres de progression :

- **Créer des rapports précis progression** -barres de progression doivent toujours présenter une représentation exacte du temps requis pour effectuer une tâche. Jamais interprètent mal l’heure pour rendre l’application occupée.
- **Utilisation des durées bien définies** -barres pas ne doivent afficher qu’une tâche de longue durée est en cours de progression placer, mais l’utilisateur et indication de la part de la tâche est terminée et une estimation du temps restant.

## <a name="progress-indicators-and-storyboards"></a>Tables de montage séquentiel et les indicateurs de progression

Pour travailler avec un indicateur de progression dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application à l’aide du concepteur iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)
    
1. Dans le **panneau solutions**, double-cliquez sur le **Main.storyboard** et ouvrez-le pour le modifier.

2. Faites glisser un **indicateur d’activité** à partir de la **boîte à outils** et déposez-la sur la vue : 

    ![Un indicateur d’activité](progress-indicators-images/activity01.png "un indicateur d’activité")

3. Dans le **Widget** onglet de la **panneau Propriétés**, vous pouvez ajuster plusieurs propriétés de l’indicateur d’activité tels que son **Style**, **comportement**, et **nom**: 

    ![L’onglet du Widget d’un indicateur d’activité](progress-indicators-images/activity02.png "onglet le Widget d’un indicateur d’activité")
    
    Le **nom** détermine le nom de la propriété qui représente l’indicateur d’activité de C# code.

4. Faites glisser un **cours** à partir de la **boîte à outils** et déposez-la sur la vue : 

    ![Une vue de progression](progress-indicators-images/activity03.png "une vue de progression")

5. Dans le **Widget** onglet de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de la vue de progression comme son **Style**, **progression**(pourcentage d’achèvement), et **nom**: 

    ![L’onglet de Widget pour une vue de progression](progress-indicators-images/activity04.png "onglet le Widget pour une vue de progression")
    
    Le **nom** détermine le nom de la propriété qui représente la vue de progression dans C# code.

6. Enregistrez les modifications apportées.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le **Main.storyboard** et ouvrez-le pour le modifier.

2. Faites glisser un **indicateur d’activité** à partir de la **boîte à outils** et déposez-la sur la vue : 

    ![Un indicateur d’activité](progress-indicators-images/activity01-vs.png
    "un indicateur d’activité")

3. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de l’indicateur d’activité tels que son **Style**, **comportement**, et **nom**: 

    ![L’onglet du Widget d’un indicateur d’activité](progress-indicators-images/activity02-vs.png "onglet le Widget d’un indicateur d’activité")

    Le **nom** détermine le nom de la propriété qui représente l’indicateur d’activité de C# code.

4. Faites glisser un **cours** à partir de la **boîte à outils** et déposez-la sur la vue : 

   ![Une vue de progression](progress-indicators-images/activity03-vs.png "une vue de progression")

5. Dans le **Widget** onglet de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de la vue de progression comme son **Style**, **progression**(pourcentage d’achèvement), et **nom**: 

    ![L’onglet de Widget pour une vue de progression](progress-indicators-images/activity04-vs.png "onglet le Widget pour une vue de progression")
    
    Le **nom** détermine le nom de la propriété qui représente la vue de progression dans C# code.

6. Enregistrez les modifications apportées.

-----

Pour plus d’informations sur l’utilisation des tables de montage séquentiel, consultez notre [Hello, tvOS Guide de démarrage rapide](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Utilisation des indicateurs de l’activité

Comme indiqué ci-dessus, les indicateurs de l’activité doivent s’afficher quand votre application s’exécute un processus long de longueur indéterminé.

À tout moment, vous pouvez voir si un indicateur d’activité est animée en vérifiant son `IsAnimating` propriété. Si le `HidesWhenStopped` propriété est `true`, il se peut que le voyant d’activité sera automatiquement masqué lorsque son animation est arrêtée.

Vous pouvez utiliser le code suivant pour démarrer l’animation : 

```csharp
ActivityIndicator.StartAnimating();
```

Et le script suivant arrête l’animation :

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Ces extraits de code supposent que l’indicateur d’activité **nom** a été défini sur **ActivityIndicator** dans le **Widget** onglet du concepteur iOS.

## <a name="working-with-progress-bars"></a>Utilisation des barres de progression

Là encore, une barre de progression doit être utilisée à tout moment que votre application s’exécute une tâche à long terme d’une durée connue. 

Le `Progress` propriété est utilisée pour définir la durée de la tâche a été effectuée à partir de 0 % à 100 % (de 0,0 à 1,0). Utilisez le `ProgressTintColor` propriété pour définir la couleur de la barre de quantité terminée et le `TrackTintColor` propriété à définir la couleur d’arrière-plan (quantité inachevée).

## <a name="summary"></a>Récapitulatif

Cet article a présenté la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS.

## <a name="related-links"></a>Liens connexes

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
