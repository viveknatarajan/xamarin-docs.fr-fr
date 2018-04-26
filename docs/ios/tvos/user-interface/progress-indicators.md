---
title: Utilisation des indicateurs de progression
description: Cet article décrit la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/25/2018
ms.openlocfilehash: d512dfddb3a6c81767f937272a4ffb1ab1a35372
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="working-with-progress-indicators"></a>Utilisation des indicateurs de progression

_Cet article décrit la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS._

Il peut arriver lorsque votre application Xamarin.tvOS doit charger le contenu nouveau ou effectuer une opération de traitement de longue durée. Durant ces périodes, vous devez présenter un indicateur d’activité ou d’une barre de progression pour informer l’utilisateur que l’application est en cours d’exécution et leur donner une indication quant à la durée de la tâche en cours d’exécution.

![Exemples d’indicateurs de progression](progress-indicators-images/intro01.png "exemples d’indicateurs de progression")

## <a name="about-activity-indicators"></a>À propos des indicateurs d’activité

Un indicateur d’activité présente sous la forme d’une représentant une roue dentée tournant et est utilisé pour représenter une tâche d’une durée indéterminée. L’indicateur est présenté quand la tâche démarre et disparaît lors de la tâche est terminée.

Apple a les suggestions suivantes pour travailler avec des indicateurs d’activité :

- **Si possible, utilisez à la place les barres de progression** - parce qu’une offre d’indicateur activité prend l’utilisateur sans commentaires la long du processus en cours d’exécution, utilisez toujours une barre de progression si la longueur est connue (par exemple, le nombre d’octets à télécharger dans un fichier).
- **Conserver l’indicateur animée** -les utilisateurs un indicateur d’arrêt d’activité d'association à une application bloquée, afin de toujours animation l’indicateur qui est affichée.
- **Décrire la tâche en cours de traitement** -simplement afficher le voyant d’activité par lui-même ne suffit pas ; l’utilisateur doit être informé sur le processus sur lequel ils sont en attente. Inclure une étiquette explicite (généralement une phrase unique et complet) qui définit clairement la tâche.

## <a name="about-progress-bars"></a>À propos des barres de progression

Une barre de progression se présente sous la forme d’une ligne qui remplit avec la couleur pour indiquer l’état et la longueur d’une tâche de longue durée. Barres de progression doivent toujours être utilisés lors de la longueur des tâches est connue ou peut être calculée.

Apple a les suggestions suivantes pour travailler avec des barres de progression :

- **Signaler la progression avec précision** -barres de progression doivent toujours présenter une représentation exacte du temps nécessaire pour effectuer une tâche. Jamais interprètent mal l’heure pour rendre l’application occupée.
- **Utilisation des durées bien définies** -barres ne doivent pas uniquement afficher qu’une tâche de longue durée est en cours de progression placer, mais l’utilisateur et indication de la part de la tâche est terminée et une estimation du temps restant.

## <a name="progress-indicators-and-storyboards"></a>Plans conceptuels et les indicateurs de progression

Pour travailler avec un indicateur de progression dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
1. Dans le **Solution remplissage**, double-cliquez sur le **Main.storyboard** de fichier et l’ouvre pour modification.

2. Faites glisser un **voyant d’activité** à partir de la **boîte à outils** et déposez-la sur la vue : 

    ![Un indicateur d’activité](progress-indicators-images/activity01.png "un indicateur d’activité")

3. Dans le **Widget** onglet de la **propriétés remplissage**, vous pouvez ajuster plusieurs propriétés de l’indicateur d’activité tels que son **Style**, **comportement**, et **nom**: 

    ![L’onglet du Widget d’un indicateur d’activité](progress-indicators-images/activity02.png "onglet le Widget d’un indicateur d’activité")
    
    Le **nom** détermine le nom de la propriété qui représente l’indicateur d’activité de code c#.

4. Faites glisser un **cours** à partir de la **boîte à outils** et déposez-la sur la vue : 

    ![Une vue de progression](progress-indicators-images/activity03.png "une vue de la progression")

5. Dans le **Widget** onglet de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de la vue de la progression, tels que son **Style**, **progression**(pourcentage achevé), et **nom**: 

    ![L’onglet Widget pour une vue de progression](progress-indicators-images/activity04.png "onglet le Widget pour une vue de la progression")
    
    Le **nom** détermine le nom de la propriété qui représente la vue de la progression dans le code c#.

6. Enregistrez les modifications apportées.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le **Main.storyboard** de fichier et l’ouvre pour modification.

2. Faites glisser un **voyant d’activité** à partir de la **boîte à outils** et déposez-la sur la vue : 

    ![Un indicateur d’activité](progress-indicators-images/activity01-vs.png
    "un indicateur d’activité")

3. Dans le **Widget** onglet de la **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de l’indicateur d’activité tels que son **Style**, **comportement**, et **nom**: 

    ![L’onglet du Widget d’un indicateur d’activité](progress-indicators-images/activity02-vs.png "onglet le Widget d’un indicateur d’activité")

    Le **nom** détermine le nom de la propriété qui représente l’indicateur d’activité de code c#.

4. Faites glisser un **cours** à partir de la **boîte à outils** et déposez-la sur la vue : 

   ![Une vue de progression](progress-indicators-images/activity03-vs.png "une vue de la progression")

5. Dans le **Widget** onglet de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de la vue de la progression, tels que son **Style**, **progression**(pourcentage achevé), et **nom**: 

    ![L’onglet Widget pour une vue de progression](progress-indicators-images/activity04-vs.png "onglet le Widget pour une vue de la progression")
    
    Le **nom** détermine le nom de la propriété qui représente la vue de la progression dans le code c#.

6. Enregistrez les modifications apportées.

-----

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Utilisation des indicateurs de l’activité

Comme indiqué ci-dessus, les indicateurs d’activité doivent être affichées lorsque votre application s’exécute un processus long de longueur indéterminée.

À tout moment, vous pouvez voir si un indicateur d’activité anime en vérifiant son `IsAnimating` propriété. Si le `HidesWhenStopped` propriété est `true`, le voyant d’activité sera automatiquement masqué lorsque son animation est arrêtée.

Vous pouvez utiliser le code suivant pour lancer l’animation : 

```csharp
ActivityIndicator.StartAnimating();
```

Et le script suivant arrête l’animation :

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Ces extraits de code supposent que le voyant d’activité **nom** a été définie sur **ActivityIndicator** dans les **Widget** onglet du concepteur iOS.

## <a name="working-with-progress-bars"></a>Utilisation des barres de progression

Là encore, une barre de progression doit être utilisée à tout moment, que votre application exécute une tâche de longue durée d’une durée. 

Le `Progress` propriété est utilisée pour définir la durée de la tâche qui a été effectuée à partir de 0 % et 100 % (entre 0.0 et 1.0). Utilisez le `ProgressTintColor` propriété pour définir la couleur de la barre de la quantité terminée et le `TrackTintColor` propriété à définir la couleur d’arrière-plan (montant inachevée).

## <a name="summary"></a>Récapitulatif

Cet article a couvert conception et utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS.

## <a name="related-links"></a>Liens connexes

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
