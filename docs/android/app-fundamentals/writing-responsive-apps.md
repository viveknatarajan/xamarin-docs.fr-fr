---
title: L’écriture d’Applications réactives
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b8c113b67b3fbfa57ca86c72e11ddeb0e4e1a9ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="writing-responsive-applications"></a>L’écriture d’Applications réactives

Une des clés pour conserver une interface graphique utilisateur réactive consiste à effectuer des tâches longues sur un thread d’arrière-plan afin de l’interface graphique utilisateur ne se bloquer. Supposons que vous souhaitez calculer une valeur à afficher à l’utilisateur, mais cette valeur est 5 secondes pour calculer :

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Cela fonctionne, mais l’application sera « se bloque » pendant 5 secondes tandis que la valeur est calculée. Pendant ce temps, l’application ne répond pas à l’intervention de l’utilisateur. Pour contourner ce problème, nous souhaitons nos calculs sur un thread d’arrière-plan :

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Maintenant nous calculons la valeur sur un thread d’arrière-plan afin de notre interface utilisateur reste réactive pendant le calcul. Toutefois, lorsque le calcul est effectué, notre application tombe en panne, en laissant cela dans le journal :

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Il s’agit, car vous devez mettre à jour l’interface utilisateur à partir du thread d’interface utilisateur graphique. Notre code met à jour l’interface utilisateur à partir du thread de pool de threads, à l’origine de l’application à se bloquer. Nous avons besoin calculer la valeur sur le thread d’arrière-plan, mais ne la mise à jour sur le thread d’interface graphique utilisateur, qui est géré avec [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

Ce code fonctionne comme prévu. Cette interface utilisateur reste réactive et correctement mises à jour une fois le calcul comple.

Notez que cette technique n’est pas uniquement utilisée pour calculer une valeur coûteuse. Il peut être utilisé avec toutes les tâches longues qui peuvent être effectués en arrière-plan, comme un appel de service web ou le téléchargement des données internet.
