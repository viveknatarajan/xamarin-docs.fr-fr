---
title: "Procédure pas à pas : enregistrement de l’état d’activité"
description: "Nous avons évoqué ici la théorie d’enregistrement de l’état dans le guide de l’activité du cycle de vie ; à présent, examinons un exemple."
ms.topic: article
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d8b44fb7f0e60db407271fd84899489bf8e65694
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---saving-the-activity-state"></a>Procédure pas à pas : enregistrement de l’état d’activité

_Nous avons évoqué ici la théorie d’enregistrement de l’état dans le guide de l’activité du cycle de vie ; à présent, examinons un exemple._

## <a name="activity-state-walkthrough"></a>Procédure pas à pas d’état d’activité

Ouvrez le **ActivityLifecycle_Start** projet (dans le [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) exemple), générer et exécuter. Il s’agit d’un projet très simple qui a deux activités pour montrer le cycle de vie des activités et comment les différentes méthodes de cycle de vie sont appelées. Lorsque vous démarrez l’application, l’écran de `MainActivity` s’affiche : 

[![Écran de l’activité A](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Transitions d’état d’affichage

Chaque méthode dans cet exemple est écrit dans la fenêtre de sortie d’application IDE pour indiquer l’état de l’activité. (Pour ouvrir la fenêtre Sortie dans Visual Studio, tapez **CTRL-ALT-O**; pour ouvrir la fenêtre Sortie dans Visual Studio pour Mac, cliquez sur **vue > remplit > sortie de l’Application**.)

Au premier démarrage de l’application, la fenêtre Sortie affiche les modifications d’état de *activité A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Lorsque vous cliquez sur le **démarrer activité B** bouton, nous constatons *activité A* suspendre et arrêter tout en *activité B* traverse des modifications de son état : 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

Par conséquent, *activité B* est démarré et affiché à la place de *activité A*: 

[![Écran de l’activité B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Lorsque vous cliquez sur le **précédent** bouton, *activité B* est détruit et *activité A* reprise : 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Ajout d’un compteur de clic

Ensuite, nous allons modifier l’application afin que nous avons un bouton qui compte et affiche le nombre de fois que vous cliquez dessus. Tout d’abord, nous allons ajouter un `_counter` à la variable d’instance `MainActivity`:

```csharp
int _counter = 0;
```

Ensuite, permet de modifier le **Resource/layout/Main.axml** mise en page et ajoutez un nouveau `clickButton` qui affiche le nombre de fois où l’utilisateur a cliqué sur le bouton. Résultant **Main.axml** doit ressembler à ce qui suit : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

Nous allons ajouter le code suivant à la fin de la [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) méthode dans `MainActivity` &ndash; ce code gère les événements de click le `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

Lorsque nous créons et réexécutez l’application, un nouveau bouton s’affiche qui incrémente et affiche la valeur de `_counter` sur chaque clic :

[![Ajouter le nombre de fonctions tactiles](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Toutefois, lorsque nous faire pivoter l’appareil en mode paysage, ce nombre est perdu :

[![Rotation paysage définit le nombre de remettre à zéro](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

En examinant la sortie de l’application, nous constatons que *activité A* a été suspendu, arrêté, détruit, recréé, redémarré, puis redémarrée au cours de la rotation de l’affichage portrait en mode paysage : 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Étant donné que *activité A* est détruit et recréé à nouveau lorsque l’appareil est pivoté, son état de l’instance est perdue. Ensuite, nous allons ajouter du code pour enregistrer et restaurer l’état d’instance.

### <a name="adding-code-to-preserve-instance-state"></a>Ajout d’un Code à l’état d’Instance Preserve

Nous allons ajouter une méthode à `MainActivity` pour enregistrer l’état d’instance. Avant de *activité A* est détruit, Android appelle automatiquement [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) et passe un [offre groupée](https://developer.xamarin.com/api/type/Android.OS.Bundle/) que nous pouvons utiliser pour stocker l’état d’instance. Nous allons l’utiliser pour enregistrer notre nombre de clics comme une valeur entière :

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Lorsque *activité A* est recréée et repris, Android transmet `Bundle` dans notre `OnCreate` (méthode). Nous allons ajouter du code à `OnCreate` pour restaurer le `_counter` valeur à partir du passé `Bundle`. Ajoutez le code suivant juste avant la ligne où `clickbutton` est défini : 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Générez et exécutez de nouveau l’application, puis cliquez sur le deuxième bouton plusieurs fois. Lorsque nous faire pivoter l’appareil en mode paysage, le nombre est conservé.

[![Rotation de l’écran d’indique le nombre de quatre conservé](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


Examinons la fenêtre Sortie pour voir ce qui s’est produit :
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

Avant du [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) méthode a été appelée, notre nouveau `OnSaveInstanceState` méthode a été appelée pour enregistrer le `_counter` valeur dans un `Bundle`. Android passé ce `Bundle` à nous lorsqu’elle appelée notre `OnCreate` (méthode) et nous avons pu permet de restaurer le `_counter` valeur à nouveau.


## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons utilisé notre connaissance de l’activité du cycle de vie pour conserver les données d’état. 



## <a name="related-links"></a>Liens associés

- [ActivityLifecycle (exemple)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Activité Android](https://developer.xamarin.com/api/type/Android.App.Activity/)
