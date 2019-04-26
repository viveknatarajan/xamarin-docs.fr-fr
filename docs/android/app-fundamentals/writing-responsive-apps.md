---
title: Écriture d’Applications réactives
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: a1642c4cbb790cf09d2a31e629408afc61d5b7ab
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61011366"
---
# <a name="writing-responsive-applications"></a>Écriture d’Applications réactives

Une des clés à la maintenance d’une interface graphique utilisateur réactive consiste à effectuer des tâches longues sur un thread d’arrière-plan afin de l’interface graphique utilisateur ne sont bloqué. Supposons que nous voulons calculer une valeur à afficher à l’utilisateur, mais cette valeur prend 5 secondes pour calculer :

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

Cela fonctionne, mais l’application sera « bloquer » pendant 5 secondes tandis que la valeur est calculée. Pendant ce temps, l’application ne répond pas à l’interaction de l’utilisateur. Pour contourner ce problème, nous voulons faire nos calculs sur un thread d’arrière-plan :

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

Maintenant, nous calculons la valeur sur un thread d’arrière-plan afin de notre interface graphique utilisateur reste réactive pendant le calcul. Toutefois, lorsque le calcul est terminé, notre application tombe en panne, en laissant cela dans le journal :

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Il s’agit, car vous devez mettre à jour l’interface utilisateur graphique à partir du thread d’interface utilisateur graphique. Notre code met à jour l’interface utilisateur graphique à partir du thread de pool de threads, à l’origine de l’application se bloque. Nous devons calculer notre valeur sur le thread d’arrière-plan, mais ne la mise à jour sur le thread de GUI, qui est géré avec [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

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

Ce code fonctionne comme prévu. Cette interface utilisateur reste réactive et obtient correctement mis à jour une fois le calcul comple.

Notez que cette technique n’est pas simplement utilisée pour calculer une valeur de coûteuse. Il peut être utilisé pour n’importe quelle tâche longue qui peut être effectué en arrière-plan, comme un appel de service web ou le téléchargement de données internet.
