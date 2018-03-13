---
title: Hello, usure
description: "Créer votre première application Android d’usure et l’exécuter sur un périphérique ou un émulateur d’usure. Cette procédure pas à pas fournit des instructions détaillées pour la création d’un petit projet usure Android qui gère les clics de bouton et affiche un compteur de clic sur le périphérique d’usure. Il explique comment déboguer l’application à l’aide d’un émulateur d’usure ou un périphérique d’usure est connecté via Bluetooth sur un téléphone Android. Il fournit également un ensemble de conseils de débogage pour porter Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8eed2d6b825a6e6dd7e956bf901246b9a630081a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="hello-wear"></a>Hello, usure

_Créer votre première application Android d’usure et l’exécuter sur un périphérique ou un émulateur d’usure. Cette procédure pas à pas fournit des instructions détaillées pour la création d’un petit projet usure Android qui gère les clics de bouton et affiche un compteur de clic sur le périphérique d’usure. Il explique comment déboguer l’application à l’aide d’un émulateur d’usure ou un périphérique d’usure est connecté via Bluetooth sur un téléphone Android. Il fournit également un ensemble de conseils de débogage pour porter Android._

![Capture d’écran de l’application d’usure à effectuer dans ce didacticiel](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>Votre première application usure

Suivez ces étapes pour créer votre première application Xamarin.Android usure :

### <a name="1-create-a-new-android-project"></a>1. Créez un projet Android

Créer un nouveau **Android Application usure**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Création d’une nouvelle Application de porter Android dans la boîte de dialogue Nouveau projet](hello-wear-images/vs/new-solution-sml.png)](hello-wear-images/vs/new-solution.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Création d’une nouvelle Application de porter Android dans la boîte de dialogue Nouvelle Solution](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Ce modèle inclut automatiquement la **Xamarin de bibliothèque portable Android** NuGet (et dépendances) afin de vous aurez accès aux widgets d’usure spécifiques. Si vous ne voyez pas le modèle d’usure, passez en revue les [d’installation et de](~/android/wear/get-started/installation.md) guide pour vérifier que vous avez installé un kit de développement logiciel Android pris en charge. 

### <a name="2-choose-the-correct-target-framework"></a>2. Choisir la bonne **Framework cible**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Vérifiez que **Minimum Android à cibler** a la valeur **Android 5.0 (Lollipop)** ou version ultérieure : 

[![Définir le Framework cible pour Android 5.0 dans Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Vérifiez le framework cible est défini sur **Android 5.0 (Lollipop)** ou version ultérieure :

[![Définir le Framework cible pour Android 5.0 dans Visual Studio pour Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Pour plus d’informations sur la configuration de l’infrastructure cible, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Modifier la **Main.axml** mise en page

Configurez la mise en page contienne un `TextView` et un `Button` pour l’exemple : 

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

### <a name="4-edit-the-mainactivitycs-source"></a>4. Modifier la **MainActivity.cs** source

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

L’étape suivante est configuré un émulateur ou un périphérique pour déployer et exécuter l’application. Si vous n’êtes pas encore familiarisé avec le processus de déploiement et l’exécution des applications de Xamarin.Android en général, consultez le [Hello, Android Quickstart](~/android/get-started/hello-android/hello-android-quickstart.md).

Si vous n’avez pas un appareil Android d’usure comme un Smartwatch usure Android, vous pouvez exécuter l’application sur un émulateur. Pour plus d’informations sur le débogage des applications d’usure sur un émulateur, consultez [déboguer Android porter sur un émulateur](~/android/wear/deploy-test/debug-on-emulator.md).

Si vous avez un appareil Android d’usure comme un Smartwatch usure Android, vous pouvez exécuter l’application sur l’appareil au lieu d’utiliser un émulateur. Pour plus d’informations sur le débogage sur un périphérique d’usure, consultez [déboguer sur un appareil usure](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Exécuter l’application Android usure

L’appareil Android d’usure doit apparaître dans le menu déroulant de périphérique. Veillez à choisir le périphérique Android d’usure ou AVD avant de commencer le débogage. Après avoir sélectionné l’appareil, cliquez sur le bouton lecture pour déployer l’application sur l’émulateur ou le périphérique.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![En choisissant un AVD porter dans le menu de périphérique Visual Studio](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![En choisissant un AVD usure dans Visual Studio pour le menu du périphérique Mac](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Vous pouvez voir un **simplement quelques minutes...**  écran du message (ou certains autres spot) dans un premier temps : 

![Surveiller l’émulateur affiche simplement une minute...](hello-wear-images/please-wait.png)

Si vous utilisez un émulateur espion, il peut prendre un certain temps pour pouvoir démarrer l’application. Lorsque vous utilisez Bluetooth, prend plus de temps pour déployer l’application qu’en mode USB. (Par exemple, il prend environ cinq minutes pour déployer cette application sur un Watch G LG qui est connecté à Bluetooth sur un téléphone Nexus 5.)

Après que l’application a été déployé avec succès, l’écran de l’appareil d’usure doit afficher un écran comme suit :

[![Écran initial de l’application d’usure](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Appuyez sur la **CLICK ME !** bouton sur le périphérique d’usure et voir l’incrément de nombre avec chaque drainage :

[![Capture d’écran de porter une application après 3 clics](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Étapes suivantes

Extraire le [exemples d’usure](https://developer.xamarin.com/samples/android/Android%20Wear/) notamment porter Android des applications avec les applications de téléphone mobile.

Lorsque vous êtes prêt à distribuer votre application, consultez [fonctionne avec l’empaquetage](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Liens associés

- [Cliquez sur Me application (exemple)](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
