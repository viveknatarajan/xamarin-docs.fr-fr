---
title: Hello, Wear
description: Créer votre première application Android Wear et exécutez-le sur un émulateur d’usure ou un appareil. Cette procédure pas à pas fournit des instructions détaillées pour la création d’un petit projet Android Wear qui gère les clics de bouton et affiche un compteur de clic sur l’appareil de l’usure. Il explique comment déboguer l’application à l’aide d’un émulateur d’usure ou un appareil Wear qui est connecté à un téléphone Android via Bluetooth. Il fournit également un ensemble de conseils de débogage pour Android Wear.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: a8e27063040ff91f72a1cbf932b1b277a5dee63d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277398"
---
# <a name="hello-wear"></a>Hello, Wear

_Créer votre première application Android Wear et exécutez-le sur un émulateur d’usure ou un appareil. Cette procédure pas à pas fournit des instructions détaillées pour la création d’un petit projet Android Wear qui gère les clics de bouton et affiche un compteur de clic sur l’appareil de l’usure. Il explique comment déboguer l’application à l’aide d’un émulateur d’usure ou un appareil Wear qui est connecté à un téléphone Android via Bluetooth. Il fournit également un ensemble de conseils de débogage pour Android Wear._

![Capture d’écran de l’application de l’usure être effectuées dans ce didacticiel](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Votre première application Wear

Suivez ces étapes pour créer votre première application Xamarin.Android usure :

### <a name="1-create-a-new-android-project"></a>1. Créer un nouveau projet Android

Créer un nouveau **Android Application Wear**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Création d’une Application Android Wear nouveau dans la boîte de dialogue Nouveau projet](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Création d’une Application Android Wear nouveau dans la boîte de dialogue Nouvelle Solution](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Ce modèle inclut automatiquement la **bibliothèque portable Android Xamarin** NuGet (et les dépendances) pour vous donner accès aux widgets d’usure spécifiques. Si vous ne voyez pas le modèle d’usure, passez en revue la [d’installation et](~/android/wear/get-started/installation.md) guide pour vérifier que vous avez installé un kit de développement logiciel Android pris en charge. 

### <a name="2-choose-the-correct-target-framework"></a>2. Choisissez le bon **Framework cible**

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Vérifiez que **minimale d’Android à cibler** a la valeur **Android 5.0 (Lollipop)** ou version ultérieure : 

[![Définition du Framework cible pour Android 5.0 dans Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Vérifiez le framework cible est défini sur **Android 5.0 (Lollipop)** ou version ultérieure :

[![Définition du Framework cible pour Android 5.0 dans Visual Studio pour Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Pour plus d’informations sur la définition du framework cible, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Modifier le **Main.axml** mise en page

Configurer la mise en page contienne un `TextView` et un `Button` pour l’exemple : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. Modifier le **MainActivity.cs** source

Ajoutez le code pour incrémenter un compteur et l’afficher chaque fois que le bouton est activé : 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. Programme d’installation un émulateur ou un périphérique

L’étape suivante est configuré d’un émulateur ou un périphérique pour déployer et exécuter l’application. Si vous n’êtes pas encore familiarisé avec le processus de déploiement et l’exécution des applications Xamarin.Android en général, consultez le [Hello, Android-démarrage rapide](~/android/get-started/hello-android/hello-android-quickstart.md).

Si vous n’avez pas un appareil Android Wear, par exemple un Smartwatch Android Wear, vous pouvez exécuter l’application sur un émulateur. Pour plus d’informations sur le débogage des applications de l’usure sur un émulateur, consultez [déboguer Android Wear sur un émulateur](~/android/wear/deploy-test/debug-on-emulator.md).

Si vous avez un appareil Android Wear, par exemple un Smartwatch Android Wear, vous pouvez exécuter l’application sur l’appareil au lieu d’utiliser un émulateur. Pour plus d’informations sur le débogage sur un appareil Wear, consultez [déboguer sur un appareil Wear](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Exécutez l’application Android Wear

L’appareil Android Wear doit apparaître dans le menu déroulant des appareils. Veillez à choisir le périphérique Android Wear correct ou AVD avant de commencer le débogage. Après avoir sélectionné l’appareil, cliquez sur le bouton lecture pour déployer l’application sur l’émulateur ou le périphérique.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![En choisissant un AVD Wear dans le menu de périphérique de Visual Studio](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![En choisissant un AVD Wear dans Visual Studio pour le menu de l’appareil Mac](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Vous pouvez voir un **une minute...**  message (ou un autre écran INTERSTITIEL) dans un premier temps : 

![Regardez l’émulateur affiche une minute...](hello-wear-images/please-wait.png)

Si vous utilisez un émulateur espion, il peut prendre un certain temps pour démarrer l’application. Lorsque vous utilisez Bluetooth, elle prend plus de temps pour déployer l’application qu’il ferait via USB. (Par exemple, il prend environ 5 minutes pour déployer cette application sur une Watch G LG qui est connecté à un téléphone Nexus 5 Bluetooth.)

Une fois que l’application se déploie correctement, l’écran de l’appareil de l’usure doit afficher un écran semblable à ce qui suit :

[![Écran initial de l’application de l’usure](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Appuyez sur la **CLICK ME !** bouton sur l’appareil Wear et voir l’incrément de nombre avec chaque appui sur :

[![Capture d’écran de porter une application après 3 clics](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Étapes suivantes

Découvrez le [Wear exemples](https://developer.xamarin.com/samples/android/Android%20Wear/) y compris des applications Android Wear avec les applications de téléphone d’accompagnement.

Lorsque vous êtes prêt à distribuer votre application, consultez [fonctionne avec l’empaquetage](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Liens associés

- [Cliquez sur application Me (exemple)](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
