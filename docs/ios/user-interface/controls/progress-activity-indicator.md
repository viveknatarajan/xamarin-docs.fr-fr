---
title: Progression et les indicateurs d’activité dans Xamarin.iOS
description: Ce document explique comment utiliser des indicateurs de progression et l’activité dans Xamarin.iOS. Il décrit comment les utiliser par programme et avec un plan conceptuel.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 27ee788ec40bfd158dbc0d9926245b166e2954a9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790053"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Progression et les indicateurs d’activité dans Xamarin.iOS

Il est probable que votre application devra effectuer longue en cours d’exécution des tâches telles que le chargement ou le traitement des données et que ce délai peut entraîner un retard dans la mise à jour de votre interface utilisateur. Pendant ce temps, vous devez toujours utiliser un indicateur de progression à expliquant l’utilisateur que le système est occupé avec le travail. Cela donne le contrôle utilisateur que l’application fonctionne sur leur demande, il n’attend pas leur entrée, et peut fournir un moyen de détaillant qu’ils ont exactement combien de temps d’attente.

iOS offre deux méthodes principales pour fournir cette indication de la progression de votre application : indicateurs d’activité (y compris un spécifique _réseau_ voyant d’activité) et les barres de progression.

## <a name="activity-indicator"></a>Indicateur d’activité

Indicateurs d’activité doivent être affichées lorsque votre application s’exécute un processus long, mais vous ne connaissez pas la longueur exacte de temps que sont affectées.

Apple a les suggestions suivantes pour travailler avec des indicateurs d’activité :

- **Si Possible, utilisez les barres de progression plutôt** - car un indicateur d’activité permet à l’utilisateur sans commentaires concernant la durée pendant laquelle le processus en cours d’exécution prend, utilisez toujours une barre de progression si la longueur est connaissez (par exemple, le nombre d’octets à télécharger dans un fichier).
- **Conserver l’indicateur animée** -les utilisateurs un indicateur d’activité stationnaire d'association à une application bloquée donc vous devez toujours l’indicateur animé pendant qu’il est affiché.
- **Décrire la tâche en cours de traitement** -simplement afficher le voyant d’activité par lui-même ne suffit pas, l’utilisateur doit être informé sur le processus qu’ils sont en attente sur. Inclure une étiquette explicite (généralement une phrase unique et complet) qui définit clairement la tâche.

### <a name="implementing-an-activity-indicator"></a>Implémentation d’un indicateur d’activité

Un indicateur d’activité est implémenté via le [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) classe pour indiquer qu’un `UIActivity` est en cours.

### <a name="activity-indicators-and-storyboards"></a>Plans conceptuels et les indicateurs d’activité

Si vous utilisez le concepteur iOS pour créer votre interface utilisateur, le voyant d’activité peuvent être ajouté à votre disposition à partir de la boîte à outils. Les propriétés suivantes peuvent être ajustées à partir de la zone de propriétés :

![Remplissage des propriétés](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Gestion du comportement d’indicateur d’activité

Utilisez le `StartAnimating()` et `StopAnimating()` méthodes pour démarrer et arrêter l’animation indicateur de l’activité.

Définir le `HidesWhenStopped` propriété `true` à disparaître après le voyant d’activité `StopAnimating()` a été appelée. Cette option est définie `true` par défaut. À tout moment, vous pouvez voir si l’indicateur de l’activité s’exécute son animation tournant en vérifiant la `IsAnimating` propriété. 


### <a name="managing-activity-indicator-appearances"></a>La gestion des apparences d’indicateur d’activité

Le `UIActivityIndicatorViewStyle` énumération peut être passée en tant que paramètre lors de l’instanciation de l’indicateur d’activité. Vous pouvez l’utiliser pour définir le style visuel à `Gray`, `White`, ou `WhiteLarge`, par exemple :

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Vous pouvez remplacer la couleur fournie par `UIActivityIndicatorViewStyle` en définissant le `Color` propriété.

## <a name="progress-bar"></a>ProgressBar

Une barre de progression se présente sous la forme d’une ligne qui remplit avec la couleur pour indiquer l’état et la longueur d’une tâche de longue durée. Barres de progression doit toujours être utilisés lors de la longueur des tâches est connu, ou peut être calculée.

Apple a les suggestions suivantes pour travailler avec des barres de progression :

- **Signaler la progression précisément** -barres de progression doit toujours être une représentation exacte du temps nécessaire pour effectuer une tâche. Jamais interprètent mal l’heure pour rendre l’application occupée.
- **Utilisation des durées de Well-Defined** -barre de progression ne doit pas uniquement afficher qu’une tâche de longue durée se placer, mais l’utilisateur et indication de la part de la tâche est terminée et une estimation du temps restant.

### <a name="implementing-an-progress-bar"></a>Implémentation d’une barre de progression

Une barre de progression est créée en instanciant une [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Barres de progression et les animations

Vous pouvez également ajouter une barre de progression à votre interface utilisateur lorsque vous utilisez le concepteur iOS. Recherchez **cours** dans les **boîte à outils** et faites-le glisser vers votre affichage.

Les propriétés suivantes peuvent être ajustées pour la zone de propriétés :

![Remplissage des propriétés](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Gestion du comportement de la barre de progression

La progression de la barre peut être définie au départ à l’aide de la `Progress` propriété :

```csharp
ProgressBar.Progress = 0f;
```

La progression peut être ajustée à l’aide de la `SetProgress` méthode et en passant une valeur booléenne déclaration si vous voulez que la modification animée ou non.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Pour plus d’informations sur l’utilisation de la barre de progression, reportez-vous à la [création de rapports de progression](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS) recette et le [UICatalog tvOS exemple](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>La gestion d’apparence de la barre de progression

Similaire à un indicateur d’activité, le `UIProgressViewStyle` énumération peut être passée en tant que paramètre lors de l’instanciation de la barre de progression.

La progression et l’Image de suivi et la couleur de teinte peuvent être ajustée en utilisant les propriétés suivantes :

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



