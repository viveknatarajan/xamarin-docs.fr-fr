---
title: Utilisation des indicateurs de progression
description: "Cet article décrit la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c021550e17cf8206d59102856a11c72000ad06aa
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-progress-indicators"></a>Utilisation des indicateurs de progression

_Cet article décrit la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS._


Il peut arriver lorsque votre application Xamarin.tvOS doit charger le contenu nouveau ou effectuer une opération de traitement de longue durée. Durant ces périodes, vous devez présenter un indicateur d’activité ou barre de progression pour informer l’utilisateur que l’application est en cours d’exécution et leur donner une indication quant à la durée de la tâche en cours d’exécution.

[![](progress-indicators-images/intro01.png "Indicateurs de progression de l’exemple")](progress-indicators-images/intro01.png#lightbox)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>À propos des indicateurs d’activité

Un indicateur d’activité présente sous la forme d’une représentant une roue dentée tournant visuellement et est utilisé pour représenter une tâche d’une durée indéterminée. L’indicateur est présenté quand la tâche démarre et disparaît lors de la tâche est terminée.

Apple a les suggestions suivantes pour travailler avec des indicateurs d’activité :

- **Si Possible, utilisez les barres de progression plutôt** - car un indicateur d’activité permet à l’utilisateur sans commentaires concernant la durée pendant laquelle le processus en cours d’exécution prend, utilisez toujours une barre de progression si la longueur est connaissez (par exemple, le nombre d’octets à télécharger dans un fichier).
- **Conserver l’indicateur animée** -les utilisateurs un indicateur d’activité stationnaire d'association à une application bloquée donc vous devez toujours l’indicateur animé pendant qu’il est affiché.
- **Décrire la tâche en cours de traitement** -simplement afficher le voyant d’activité par lui-même ne suffit pas, l’utilisateur doit être informé sur le processus qu’ils sont en attente sur. Inclure une étiquette explicite (généralement une phrase unique et complet) qui définit clairement la tâche.

<a name="Summary" />

## <a name="about-progress-bars"></a>À propos des barres de progression

Une barre de progression se présente sous la forme d’une ligne qui remplit avec la couleur pour indiquer l’état et la longueur d’une tâche de longue durée. Barres de progression doit toujours être utilisés lors de la longueur des tâches est connu, ou peut être calculée.

Apple a les suggestions suivantes pour travailler avec des barres de progression :

- **Signaler la progression précisément** -barres de progression doit toujours être une représentation exacte du temps nécessaire pour effectuer une tâche. Jamais interprètent mal l’heure pour rendre l’application occupée.
- **Utilisation des durées de Well-Defined** -barre de progression ne doit pas uniquement afficher qu’une tâche de longue durée se placer, mais l’utilisateur et indication de la part de la tâche est terminée et une estimation du temps restant.

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>Plans conceptuels et les indicateurs de progression

Pour travailler avec l’indicateur de progression dans une application Xamarin.tvOS, le plus simple consiste à ajouter à l’interface utilisateur de l’application d’utiliser le concepteur iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)
    
1. Dans le **Solution remplissage**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **voyant d’activité** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](progress-indicators-images/activity01.png "Un indicateur d’activité")](progress-indicators-images/activity01.png#lightbox)
1. Dans le **onglet Widget** de la **propriétés remplissage**, vous pouvez ajuster plusieurs propriétés de l’indicateur d’activité tels que son **Style** et **comportement**: 

    [![](progress-indicators-images/activity02.png "L’onglet du Widget ")](progress-indicators-images/activity02.png#lightbox)
1. Faites glisser un **cours** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](progress-indicators-images/activity03.png "Une vue de la progression")](progress-indicators-images/activity03.png#lightbox)
1. Dans le **onglet Widget** de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de la vue de la progression, tels que son **Style** et **progression**(pourcentage achevé) : 

    [![](progress-indicators-images/activity04.png "L’onglet du Widget")](progress-indicators-images/activity04.png#lightbox)
1. Enfin, affectez **noms** aux contrôles afin que vous pouvez répondre à leur en code c#. Exemple : 

    [![](progress-indicators-images/activity05.png "Attribuez un nom")](progress-indicators-images/activity05.png#lightbox)
1. Enregistrez les modifications apportées.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Dans le **l’Explorateur de solutions**, double-cliquez sur le `Main.storyboard` de fichier et l’ouvre pour modification.
1. Faites glisser un **voyant d’activité** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](progress-indicators-images/activity01-vs.png "Un indicateur d’activité")](progress-indicators-images/activity01-vs.png#lightbox)
1. Dans le **onglet Widget** de la **l’Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de l’indicateur d’activité tels que son **Style** et **decomportement**: 

    [![](progress-indicators-images/activity02-vs.png "L’onglet du Widget")](progress-indicators-images/activity02-vs.png#lightbox)
1. Faites glisser un **cours** à partir de la **boîte à outils** et déposez-la sur la vue : 

    [![](progress-indicators-images/activity03-vs.png "Une vue de la progression")](progress-indicators-images/activity03-vs.png#lightbox)
1. Dans le **onglet Widget** de la **Explorateur de propriétés**, vous pouvez ajuster plusieurs propriétés de la vue de la progression, tels que son **Style** et **progression**(pourcentage achevé) : 

    [![](progress-indicators-images/activity04-vs.png "L’onglet du Widget")](progress-indicators-images/activity04-vs.png#lightbox)
1. Enfin, affectez **noms** aux contrôles afin que vous pouvez répondre à leur en code c#. Exemple : 

    [![](progress-indicators-images/activity05-vs.png "Attribuez un nom")](progress-indicators-images/activity05-vs.png#lightbox)
1. Enregistrez les modifications apportées.

-----

Pour plus d’informations sur l’utilisation des plans conceptuels, veuillez consulter notre [Hello, Guide de démarrage rapide de tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>Utilisation des indicateurs de l’activité

Comme indiqué ci-dessus, les indicateurs d’activité doivent être affichées lorsque votre application s’exécute un processus long, mais vous ne connaissez pas la longueur exacte de temps que sont affectées.

À tout moment, vous pouvez voir si l’indicateur de l’activité s’exécute son animation tournant en vérifiant la `IsAnimating` propriété. Si le `HidesWhenStopped` propriété est `true`, le voyant d’activité sera automatiquement masqué lorsque son animation est arrêtée.

Vous pouvez utiliser le code suivant pour lancer l’animation : 

```csharp
ActivityIndicator.StartAnimating();
```

Et le script suivant arrête l’animation :

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>Utilisation des barres de progression

Là encore, une barre de progression doit être utilisée à tout moment, que votre application exécute une tâche de longue durée d’une durée de connaître. 

Le `Progress` propriété est utilisée pour définir la durée de la tâche qui a été effectuée à partir de 0 % et 100 % (entre 0.0 et 1.0). Utilisez le `ProgressTintColor` propriété pour définir la couleur de la barre de la quantité terminée et le `TrackTintColor` propriété à définir la couleur d’arrière-plan (montant inachevée).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la conception et l’utilisation des indicateurs de progression à l’intérieur d’une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
