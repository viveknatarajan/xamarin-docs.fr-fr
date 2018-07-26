---
title: Progression et les indicateurs d’activité dans Xamarin.iOS
description: Ce document explique comment utiliser des indicateurs de progression et d’activité dans Xamarin.iOS. Il décrit comment les utiliser à la fois par programmation et avec un storyboard.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 24ad47bd37693eccf38033159acef72a9d4942be
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242177"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Progression et les indicateurs d’activité dans Xamarin.iOS

Il est probable que votre application aura à effectuer de longues tâches telles que le chargement ou le traitement des données et que ce délai peut entraîner un délai de mise à jour de votre interface utilisateur en cours d’exécution. Pendant ce temps, vous devez toujours utiliser un indicateur de progression pour rassurer l’utilisateur que le système est occupé à faire le travail. Cela donne le contrôle utilisateur que l’application fonctionne sur leur demande, il n’attend pas de leur avis, et peut fournir un moyen de détaillant exactement la durée pendant laquelle il devra attendre.

iOS fournit deux méthodes principales pour fournir cette indication de la progression dans votre application : indicateurs d’activité (y compris un spécifique _réseau_ indicateur d’activité) et les barres de progression.

## <a name="activity-indicator"></a>Indicateur d’activité

Indicateurs de l’activité doivent être indiqués lorsque votre application s’exécute un processus long, mais vous ne connaissez pas la durée exacte de que la tâche nécessitera.

Apple a les suggestions suivantes pour l’utilisation des indicateurs de l’activité :

- **Si Possible, utilisez les barres de progression plutôt** - parce qu’un indicateur d’activité ne permet à l’utilisateur aucun commentaire quant à la durée pendant laquelle le processus en cours d’exécution prendra, utilisez toujours une barre de progression si la longueur est connaissez (par exemple, le nombre d’octets à télécharger dans un fichier).
- **Conserver l’indicateur animée** -les utilisateurs un indicateur d’activité stationnaire d'association à une application bloquée donc vous devez toujours avoir l’indicateur animé pendant qu’il est affiché.
- **Décrire la tâche en cours de traitement** -affichage uniquement l’indicateur d’activité par lui-même ne suffit pas, l’utilisateur doit être informé sur le processus qu’ils sont en attente sur. Inclure une étiquette explicite (généralement une phrase unique et complet) qui définit clairement la tâche.

### <a name="implementing-an-activity-indicator"></a>Implémentation d’un indicateur d’activité

Un indicateur d’activité est implémenté via le [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) classe pour indiquer qu’un `UIActivity` est en cours.

### <a name="activity-indicators-and-storyboards"></a>Storyboards et des indicateurs de l’activité

Si vous utilisez le concepteur iOS pour créer votre interface utilisateur, l’indicateur d’activité peuvent être ajouté à votre disposition à partir de la boîte à outils. Les propriétés suivantes peuvent être ajustées à partir du panneau Propriétés :

![Panneau Propriétés](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Gestion du comportement d’indicateur d’activité

Utilisez le `StartAnimating()` et `StopAnimating()` méthodes pour démarrer et arrêter l’animation d’indicateur d’activité.

Définir le `HidesWhenStopped` propriété `true` à disparaître le voyant d’activité après `StopAnimating()` a été appelée. Il est défini sur `true` par défaut. À tout moment, vous pouvez voir si l’indicateur d’activité s’exécute son animation de rotation en vérifiant la `IsAnimating` propriété. 


### <a name="managing-activity-indicator-appearances"></a>La gestion des apparences d’indicateur d’activité

Le `UIActivityIndicatorViewStyle` énumération peut être passée en tant que paramètre lors de l’instanciation de l’indicateur d’activité. Vous pouvez l’utiliser pour définir le style visuel `Gray`, `White`, ou `WhiteLarge`, par exemple :

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Vous pouvez remplacer la couleur fournie par `UIActivityIndicatorViewStyle` en définissant le `Color` propriété.

## <a name="progress-bar"></a>ProgressBar

Une barre de progression se présente comme une ligne qui remplit avec la couleur pour indiquer l’état et la longueur d’une tâche fastidieuse. Barres de progression doit toujours être utilisés lors de la longueur des tâches est de savoir ou peut être calculée.

Apple a les suggestions suivantes pour l’utilisation des barres de progression :

- **Rapport de progression avec précision** -barres de progression doit toujours être une représentation exacte du temps requis pour effectuer une tâche. Jamais interprètent mal l’heure pour rendre l’application occupée.
- **Utilisation des durées de Well-Defined** -barre de progression ne doit pas uniquement afficher qu’une tâche de longue durée se placer, mais l’utilisateur et indication de la part de la tâche est terminée et une estimation du temps restant.

### <a name="implementing-an-progress-bar"></a>Implémentation d’une barre de progression

Une barre de progression est créée en instanciant un [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Barres de progression et les tables de montage séquentiel

Vous pouvez également ajouter une barre de progression à votre interface utilisateur lors de l’utilisation du concepteur iOS. Recherchez **cours** dans le **boîte à outils** et faites-le glisser vers votre affichage.

Les propriétés suivantes peuvent être ajustées pour le panneau Propriétés :

![Panneau Propriétés](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Gestion du comportement de la barre de progression

La progression de la barre de peut être définie initialement à l’aide de la `Progress` propriété :

```csharp
ProgressBar.Progress = 0f;
```

La progression peut être ajustée à l’aide de la `SetProgress` méthode et en passant une valeur booléenne déclaration si vous voulez que la modification animée ou non.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Pour plus d’informations sur l’utilisation de la barre de progression, reportez-vous à la [création de rapports de progression](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) Recipe (Recette) et le [UICatalog tvOS exemple](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

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



