---
title: 'Procédure pas à pas : enregistrement de l’état d’activité'
description: Nous avons abordé la théorie de l’enregistrement d’état dans le guide du cycle de vie activité ; maintenant, nous allons étudier un exemple.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: c8f92e55648dff469227cc3bad981ad5f6e6d0ac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122125"
---
# <a name="walkthrough---saving-the-activity-state"></a>Procédure pas à pas : enregistrement de l’état d’activité

_Nous avons abordé la théorie de l’enregistrement d’état dans le guide du cycle de vie activité ; maintenant, nous allons étudier un exemple._

## <a name="activity-state-walkthrough"></a>Procédure pas à pas état d’activité

Ouvrons le **ActivityLifecycle_Start** projet (dans le [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) exemple), générez-le et exécutez-le. Il s’agit d’un projet très simple qui a deux activités pour illustrer le cycle de vie d’activité et la façon dont les différentes méthodes de cycle de vie sont appelées. Lorsque vous démarrez l’application, l’écran de `MainActivity` s’affiche : 

[![Écran de l’activité A](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Transitions d’état d’affichage

Chaque méthode dans cet exemple écrit dans la fenêtre de sortie d’application IDE pour indiquer l’état de l’activité. (Pour ouvrir la fenêtre Sortie dans Visual Studio, tapez **CTRL-ALT-O**; pour ouvrir la fenêtre Sortie dans Visual Studio pour Mac, cliquez sur **Afficher > panneaux > sortie de l’Application**.)

Lorsque l’application commence, la fenêtre Sortie affiche les modifications d’état de *activité A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Lorsque nous cliquons sur le **démarrer activité B** bouton, nous voyons *activité A* suspendre et arrêter tout *activité B* parcourt les modifications de son état : 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

Par conséquent, *activité B* est démarré et affiché à la place de *activité A*: 

[![Écran de l’activité B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Lorsque nous cliquons sur le **retour** bouton, *activité B* est détruit et *activité A* reprise : 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Ajout d’un compteur de clic

Ensuite, nous allons modifier l’application afin que nous avons un bouton qui compte et affiche le nombre de fois que vous cliquez dessus. Tout d’abord, nous allons ajouter un `_counter` variable d’instance à `MainActivity`:

```csharp
int _counter = 0;
```

Ensuite, nous allons modifier le **Resource/layout/Main.axml** mise en page et ajoutez un nouveau `clickButton` qui affiche le nombre de fois où l’utilisateur a cliqué sur le bouton. Résultant **Main.axml** doit ressembler à ce qui suit : 

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

Nous allons ajouter le code suivant à la fin de la [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) méthode dans `MainActivity` &ndash; ce code gère les événements à partir de click le `clickButton`:

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

Lorsque nous construisons et réexécutez l’application, un nouveau bouton apparaît qui incrémente et affiche la valeur de `_counter` sur chaque clic :

[![Ajouter le nombre de fonctions tactiles](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Mais quand nous avons faites pivoter l’appareil en mode paysage, ce nombre est perdu :

[![Rotation paysage définit le nombre de remettre à zéro](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

Examen de la sortie de l’application, nous voyons que *activité A* a été suspendu, arrêté, détruit, recréé, redémarré, puis redémarrée au cours de la rotation du mode portrait au mode paysage : 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Étant donné que *activité A* est détruite, puis recréée à nouveau lorsque l’appareil est pivoté, son état de l’instance est perdue. Ensuite, nous allons ajouter le code pour enregistrer et restaurer l’état d’instance.

### <a name="adding-code-to-preserve-instance-state"></a>Ajout de Code à l’état de l’Instance Preserve

Nous allons ajouter une méthode à `MainActivity` pour enregistrer l’état d’instance. Avant de *activité A* est détruit, Android appelle automatiquement [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) et transmet un [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) que nous pouvons utiliser pour stocker l’état d’instance. Nous allons l’utiliser pour enregistrer notre nombre de clics en tant qu’entier :

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

Générer et exécuter à nouveau l’application, puis cliquez sur le deuxième bouton plusieurs fois. Quand nous avons faites pivoter l’appareil en mode paysage, le nombre est conservé !

[![Rotation de l’écran affiche le nombre de quatre conservé](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


Jetons un œil à la fenêtre de sortie pour voir ce qui est arrivé :
    
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

Avant du [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) méthode a été appelée, notre nouveau `OnSaveInstanceState` méthode a été appelée pour enregistrer le `_counter` valeur dans un `Bundle`. Android passé ce `Bundle` qui nous quand elle appelée notre `OnCreate` (méthode) et nous avons pu permet de restaurer le `_counter` valeur à où nous nous étions arrêtés.


## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons utilisé notre connaissance de l’activité du cycle de vie pour conserver les données d’état. 



## <a name="related-links"></a>Liens associés

- [ActivityLifecycle (exemple)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Cycle de vie des activités](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Activité Android](https://developer.xamarin.com/api/type/Android.App.Activity/)
