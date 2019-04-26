---
title: Création d’un cadran de montre pour Android Wear 1.0
description: Ce guide explique comment implémenter un service de visage espion personnalisé pour Android Wear de 1.0. Instructions détaillées sont fournies pour la création d’un élément supprimé service de visage espion numérique, suivie de davantage de code pour créer un style analogique cadran de la montre.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/23/2018
ms.openlocfilehash: 067a39838fbfe3f1b33ac0d30b5069366b11e407
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61287071"
---
# <a name="creating-a-watch-face"></a>Création d’un cadran de montre

_Ce guide explique comment implémenter un service de visage espion personnalisé pour Android Wear de 1.0. Instructions détaillées sont fournies pour la création d’un élément supprimé service de visage espion numérique, suivie de davantage de code pour créer un style analogique cadran de la montre._

## <a name="overview"></a>Vue d'ensemble

Dans cette procédure pas à pas, un service de visage espion de base est créé pour illustrer l’essentiel de la création d’un cadran de montre Android 1.0 Wear personnalisé.
Le service de face initiale Espion affiche un espion numérique simple qui affiche l’heure actuelle en heures et minutes :

[![Numérique cadran de montre](creating-a-watchface-images/01-initial-face.png "capture d’écran de l’exemple de la face espion numérique initiale")](creating-a-watchface-images/01-initial-face.png#lightbox)

Après que cette numérique cadran de montre est développé et testé, davantage de code est ajouté pour mettre à niveau vers une plus sophistiquées analogique cadran de montre des trois mains :

[![Analogique cadran de montre](creating-a-watchface-images/02-example-watchface.png "capture d’écran de l’exemple de la face espion analogique finale")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Regardez face services sont regroupés et installés dans le cadre d’une application Wear de 1.0. Dans les exemples suivants, `MainActivity` ne contient rien de plus que le code à partir du modèle d’application 1.0 Wear afin que le service de face espion permettre être empaqueté et déployé vers l’espion active dans le cadre de l’application. En effet, cette application servira purement un véhicule pour obtenir le service de face espion chargé dans l’appareil de 1.0 Wear (ou l’émulateur) pour déboguer et tester.

## <a name="requirements"></a>Configuration requise

Pour implémenter un service de visage espion, les conditions suivantes sont requises :

-   Android 5.0 (niveau 21 d’API) ou version ultérieure sur l’appareil Wear ou l’émulateur.

-   Le [Xamarin Android Wear prise en charge des bibliothèques](https://www.nuget.org/packages/Xamarin.Android.Wear) doit être ajouté au projet Xamarin.Android.

Bien que Android 5.0 est l’API minimale niveau pour implémenter un service de visage espion, Android 5.1 ou version ultérieure est recommandée. Android Wear les appareils exécutant Android 5.1 (API 22) ou autorisez les applications d’usure contrôler ce qui est affiché sur l’écran pendant que l’appareil est en basse puissance *ambiante* mode. Lorsque l’appareil quitte faible puissance *ambiante* mode, il se trouve dans *interactive* mode. Pour plus d’informations sur ces modes, consultez [en conservant votre application Visible](https://developer.android.com/training/wearables/apps/always-on.html).


## <a name="start-an-app-project"></a>Démarrer un projet d’application

Créer un nouveau projet Android 1.0 Wear nommé **WatchFace** (pour plus d’informations sur la création de nouveaux projets Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)) :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Boîte de dialogue Nouveau projet](creating-a-watchface-images/03-wear-project-vs-sml.png "sélectionner une application Wear dans la boîte de dialogue Nouveau projet")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Boîte de dialogue Nouveau projet](creating-a-watchface-images/03-wear-project-xs-sml.png "sélectionner une application Wear dans la boîte de dialogue Nouveau projet")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


La valeur est le nom du package `com.xamarin.watchface`:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Configuration du nom du package](creating-a-watchface-images/04-package-name-vs.png "valeur com.xamarin.watchface est le nom du package")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Configuration du nom du package](creating-a-watchface-images/04-package-name-xs.png "valeur com.xamarin.watchface est le nom du package")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

En outre, faites défiler vers le bas et activez le **INTERNET** et **WAKE_LOCK** autorisations :

[![Autorisations requises](creating-a-watchface-images/05-required-permissions-vs.png "autorisations activer INTERNET et WAKE_LOCK")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

La valeur est la version minimale d’Android **Android 5.1 (API niveau 22)**.
En outre, activer la **Internet** et **WakeLock** autorisations :

[![Autorisations requises](creating-a-watchface-images/05-required-permissions-xs.png "autorisations activer Internet et WakeLock")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Ensuite, téléchargez [preview.png](creating-a-watchface-images/preview.png) &ndash; celui-ci sera ajouté à la **drawables** dossier plus loin dans cette procédure pas à pas.


## <a name="add-the-xamarinandroid-wear-package"></a>Ajoutez le Package de l’usure de Xamarin.Android

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Démarrez le Gestionnaire de Package NuGet (dans Visual Studio, cliquez sur **références** dans le **l’Explorateur de solutions** et sélectionnez **gérer les Packages NuGet...** ). Mettre à jour le projet vers la dernière version stable de **Xamarin.Android.Wear**:

[![Gestionnaire de Package NuGet ajouter](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "ajouter le paquet Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Ensuite, si **Xamarin.Android.Support.v13** est installé, désinstallez-le :

[![Suppression du Gestionnaire de Package NuGet](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 supprimer")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Démarrez le Gestionnaire de Package NuGet (dans Visual Studio pour Mac, cliquez sur **Packages** dans le **volet Solution** et sélectionnez **ajouter des Packages...** ). Mettre à jour le projet vers la dernière version stable de **Xamarin.Android.Wear**:

[![Gestionnaire de Package NuGet ajouter](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "ajouter le paquet Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Générer et exécuter l’application sur un appareil Wear ou un émulateur (pour plus d’informations sur la procédure à suivre, consultez le [mise en route](~/android/wear/get-started/index.md) guide). Vous devez voir l’écran suivant de l’application sur l’appareil Wear :

[![Capture d’écran de l’application](creating-a-watchface-images/08-app-screen.png "écran de l’application sur un appareil Wear")](creating-a-watchface-images/08-app-screen.png#lightbox)

À ce stade, l’application Wear base n’a pas d’espion face fonctionnalité, car il ne fournit pas encore d’une implémentation de service de visage espion. Ce service sera ajouté ensuite.


## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android Wear implémente regarder visages via la `CanvasWatchFaceService` classe. `CanvasWatchFaceService` est dérivé de `WatchFaceService`, qui elle-même est dérivée de `WallpaperService` comme indiqué dans le diagramme suivant :

[![Diagramme d’héritage](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService diagramme d’héritage")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` inclut une liste imbriquée `CanvasWatchFaceService.Engine`; il instancie un `CanvasWatchFaceService.Engine` objet qui effectue le travail de dessin du cadran de montre. `CanvasWatchFaceService.Engine` est dérivé de `WallpaperService.Engine` comme indiqué dans le diagramme ci-dessus.

Ne pas illustré dans ce diagramme est un `Canvas` qui `CanvasWatchFaceService` utilise pour dessiner le cadran de montre &ndash; cela `Canvas` est transmis le `OnDraw` méthode comme décrit ci-dessous.

Dans les sections suivantes, un service de visage espion personnalisé sera créé en suivant ces étapes :

1.  Définir une classe appelée `MyWatchFaceService` qui est dérivée de `CanvasWatchFaceService`.

2.  Dans `MyWatchFaceService`, créez une classe imbriquée appelée `MyWatchFaceEngine` qui est dérivée de `CanvasWatchFaceService.Engine`.

3.  Dans `MyWatchFaceService`, implémenter un `CreateEngine` méthode qui instancie `MyWatchFaceEngine` et le retourne.

4.  Dans `MyWatchFaceEngine`, implémenter la `OnCreate` méthode pour créer le style de police espion et effectuer d’autres tâches d’initialisation.

5.  Implémentez le `OnDraw` méthode de `MyWatchFaceEngine`. Cette méthode est appelée chaque fois que le cadran de montre doit être redessiné (par exemple, *invalidé*). `OnDraw` est la méthode qui dessine (et redessine) espion face éléments tels que l’heure, minute et secondes mains.

6.  Implémentez le `OnTimeTick` méthode de `MyWatchFaceEngine`.
    `OnTimeTick` est appelée au moins une fois par minute (dans les modes ambiants et interactifs) ou quand la date/heure a changé.

Pour plus d’informations sur `CanvasWatchFaceService`, consultez le Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) documentation de l’API.
De même, [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) explique l’implémentation réelle du cadran de montre.


### <a name="add-the-canvaswatchfaceservice"></a>Ajouter le CanvasWatchFaceService

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Ajoutez un nouveau fichier appelé **MyWatchFaceService.cs** (dans Visual Studio, cliquez sur **WatchFace** dans le **l’Explorateur de solutions**, cliquez sur **Ajouter > nouvel élément...** , puis sélectionnez **classe**).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Ajoutez un nouveau fichier appelé **MyWatchFaceService.cs** (dans Visual Studio pour Mac, cliquez sur le **WatchFace** du projet, cliquez sur **Ajouter > nouveau fichier...** , puis sélectionnez **classe vide**).

-----

Remplacez le contenu de ce fichier par le code suivant :

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (dérivée de `CanvasWatchFaceService`) est le programme « principal » du cadran de montre. `MyWatchFaceService` implémente une seule méthode, `OnCreateEngine`, qui instancie et retourne un `MyWatchFaceEngine` objet (`MyWatchFaceEngine` est dérivée de `CanvasWatchFaceService.Engine`). L’instancié `MyWatchFaceEngine` objet doit être retourné comme un `WallpaperService.Engine`. L’encapsulation `MyWatchFaceService` objet est passé au constructeur.

`MyWatchFaceEngine` est l’implémentation de visage réel espion &ndash; il contient le code utilisé pour dessiner le cadran de montre. Il gère également les événements système tels que des modifications de l’écran (modes ambiante/interactifs, écran mise hors tension, etc..).


### <a name="implement-the-engine-oncreate-method"></a>Implémentez la méthode OnCreate de moteur

Le `OnCreate` méthode initialise le cadran de montre. Ajoutez le champ suivant à `MyWatchFaceEngine`:

```csharp
Paint hoursPaint;
```

Cela `Paint` objet doit être utilisé pour dessiner l’heure actuelle sur le cadran de montre. Ensuite, ajoutez la méthode suivante à `MyWatchFaceEngine`:

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` est appelé peu de temps après `MyWatchFaceEngine` est démarré. Il configure le `WatchFaceStyle` (ce qui contrôle la façon dont l’appareil Wear interagit avec l’utilisateur) et instancie les `Paint` objet qui sera utilisé pour afficher l’heure.

L’appel à `SetWatchFaceStyle` effectue les opérations suivantes :

1.  Jeux *en mode aperçu* à `PeekModeShort`, ce qui entraîne des notifications apparaissent sous forme de cartes de petits « aperçu » sur l’affichage.

2.  Définit la visibilité de l’arrière-plan `Interruptive`, ce qui entraîne l’arrière-plan d’une carte d’aperçu à indiquer que brièvement si elle représente une notification sans interruption.

3.  Désactive l’heure de l’interface utilisateur du système par défaut d’être dessinés sur le cadran de montre que la face de suivi personnalisé puisse l’afficher l’heure à la place.

Pour plus d’informations sur celles-ci et d’autres options de style de visage espion, consultez le Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) documentation de l’API.

Après avoir `SetWatchFaceStyle` se termine, `OnCreate` instancie le `Paint` objet (`hoursPaint`) et définir sa couleur blanc et sa taille du texte à 48 pixels ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) doit être spécifié en pixels).


### <a name="implement-the-engine-ondraw-method"></a>Implémentez la méthode OnDraw de moteur

Le `OnDraw` méthode est sans doute le plus important `CanvasWatchFaceService.Engine` méthode &ndash; c’est la méthode que réellement dessine regardez face des éléments tels que des chiffres et les mains de face d’horloge.
Dans l’exemple suivant, il dessine une chaîne d’heure sur le cadran de montre.
Ajoutez la méthode suivante à `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

Lorsque Android appelle `OnDraw`, il passe un `Canvas` instance et les limites dans lesquelles la face peut être dessinée. Dans l’exemple de code ci-dessus, `DateTime` est utilisée pour calculer l’heure actuelle en heures et minutes (au format de 12 heures). La chaîne d’heure qui en résulte est dessinée sur la zone de dessin à l’aide de la `Canvas.DrawText` (méthode). La chaîne apparaissent 70 pixels sur à partir du bord gauche et 80 pixels vers le bas entre le bord supérieur.

Pour plus d’informations sur la `OnDraw` (méthode), consultez le Android [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) documentation de l’API.


### <a name="implement-the-engine-ontimetick-method"></a>Implémentez la méthode OnTimeTick de moteur

Android appelle périodiquement la `OnTimeTick` méthode pour mettre à jour l’heure affichée par le cadran de montre. Il est appelé au moins une fois par minute (dans les modes ambiantes et interactif), ou lorsque la date/heure ou le fuseau horaire ont été modifiés. Ajoutez la méthode suivante à `MyWatchFaceEngine`:

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Cette implémentation de `OnTimeTick` appelle simplement `Invalidate`. Le `Invalidate` planifications de la méthode `OnDraw` pour redessiner le cadran de montre.

Pour plus d’informations sur la `OnTimeTick` (méthode), consultez le Android [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) documentation de l’API.


## <a name="register-the-canvaswatchfaceservice"></a>Inscrire le CanvasWatchFaceService

`MyWatchFaceService` doit être enregistré dans le **AndroidManifest.xml** de l’application de l’usure associée. Pour ce faire, ajoutez le code XML suivant à la `<application>` section :

```xml
<service
    android:name="watchface.MyWatchFaceService"
    android:label="Xamarin Sample"
    android:allowEmbedded="true"
    android:taskAffinity=""
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data
        android:name="android.service.wallpaper"
        android:resource="@xml/watch_face" />
    <meta-data
        android:name="com.google.android.wearable.watchface.preview"
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

Ce code XML effectue les opérations suivantes :

1.  Définit le `android.permission.BIND_WALLPAPER` autorisation. Cette autorisation donne l’autorisation de service de visage espion pour modifier le papier peint du système sur l’appareil. Notez que cette autorisation doit être définie dans le `<service>` section plutôt que dans la liste externe `<application>` section.

2.  Définit un `watch_face` ressource. Cette ressource est un fichier XML court qui déclare un `wallpaper` ressource (ce fichier sera créé dans la section suivante).

3.  Déclare une image drawable appelée `preview` qui sera affiché à l’écran de sélection de sélecteur espion.

4.  Inclut un `intent-filter` pour informer Android qui `MyWatchFaceService` affichent un cadran de montre.

Qui termine le code de base `WatchFace` exemple. L’étape suivante consiste à ajouter les ressources nécessaires.


## <a name="add-resource-files"></a>Ajouter des fichiers de ressources

Avant de pouvoir exécuter le service de suivi, vous devez ajouter le **watch_face** ressource et l’image d’aperçu. Tout d’abord, créez un fichier XML à **Resources/xml/watch_face.xml** et remplacez son contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Définir l’action de génération de ce fichier **AndroidResource**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Action de génération](creating-a-watchface-images/10-android-resource-vs.png "définir build action à AndroidResource")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Action de génération](creating-a-watchface-images/10-android-resource-xs.png "définir build action à AndroidResource")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Ce fichier de ressources définit un simple `wallpaper` élément qui sera utilisé pour le cadran de montre.

Si vous ne le n'avez pas encore fait, téléchargez [preview.png](creating-a-watchface-images/preview.png).
Installer à l’adresse **Resources/drawable/preview.png**. Veillez à ajouter ce fichier pour le `WatchFace` projet. Cette image d’aperçu s’affiche à l’utilisateur dans le sélecteur de visage espion sur l’appareil de l’usure. Pour créer une image d’aperçu pour votre propre cadran de montre, vous pouvez prendre une capture d’écran du cadran de montre pendant son exécution. (Pour plus d’informations sur l’obtention des captures d’écran à partir d’appareils de l’usure, consultez [prendre des captures d’écran](~/android/wear/deploy-test/debug-on-device.md#screenshots)).


## <a name="try-it"></a>Essaie !

Générer et déployer l’application sur l’appareil de l’usure. Vous devez voir l’écran de l’application Wear apparaissent comme avant. Procédez comme suit pour activer le nouveau visage d’espion :

1.  Balayez vers la droite jusqu'à ce que vous voyiez l’arrière-plan de l’écran de surveillance.

2.  Touch et contenir n’importe où sur l’arrière-plan de l’écran pendant deux secondes.

3.  Effectuez un balayage de gauche à droite pour parcourir les visages espion différents.

4.  Sélectionnez le **exemple Xamarin** cadran de montre (illustré à droite) :

    [![Sélecteur de Watchface](creating-a-watchface-images/11-watchface-picker.png "balayez pour localiser le cadran de montre des exemples de Xamarin")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Appuyez sur la **exemple Xamarin** cadran de montre pour le sélectionner.

Cela modifie le cadran de montre de l’appareil Wear à utiliser le service de visage espion personnalisé implémenté jusqu'à présent :

[![Numérique cadran de montre](creating-a-watchface-images/12-digital-watchface.png "espion de numérique personnalisé en cours d’exécution sur un appareil Wear")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Il s’agit un assez rudimentaire cadran de montre, car l’implémentation de l’application est donc minimale (par exemple, il n’inclut pas un arrière-plan de visage espion et il n’appelle pas `Paint` lissage des méthodes pour améliorer l’aspect).
Toutefois, il implémente la fonctionnalité réduite à qui est nécessaire pour créer un personnalisé cadran de montre.

Dans la section suivante, ce cadran de montre est mis à niveau vers une implémentation plus sophistiquée.


## <a name="upgrading-the-watch-face"></a>La mise à niveau le cadran de montre

Dans le reste de cette procédure pas à pas, `MyWatchFaceService` est mis à niveau pour afficher une face de style analogique espion et il est étendu pour prendre en charge davantage de fonctionnalités. Les fonctionnalités suivantes vont être ajoutées pour créer la mise à niveau cadran de montre :

1.  Indique l’heure avec analogique heure, minute et secondes mains.

2.  Réagit aux modifications de la visibilité.

3.  Répond aux changements entre mode ambiant et le mode interactif.

4.  Lit les propriétés de l’appareil Wear sous-jacent.

5.  Met automatiquement à jour l’heure quand un changement de fuseau horaire a lieu.

Avant d’implémenter les modifications de code ci-dessous, téléchargez [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), décompressez-le et déplacer les fichiers .png décompressé **ressources/drawable** (remplacer le précédent **preview.png**). Ajoutez les nouveaux fichiers .png pour le `WatchFace` projet.


### <a name="update-engine-features"></a>Fonctionnalités du moteur de mise à jour

L’étape suivante est mise à niveau **MyWatchFaceService.cs** à une implémentation qui dessine une cadran analogique montre et prend en charge de nouvelles fonctionnalités. Remplacez le contenu de **MyWatchFaceService.cs** avec la version analogique du code de la face espion [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (vous pouvez couper et coller cette source d’existant  **MyWatchFaceService.cs**).

Cette version de **MyWatchFaceService.cs** ajoute plus de code pour les méthodes existantes et inclut des méthodes substituées pour ajouter des fonctionnalités supplémentaires. Les sections suivantes fournissent une visite guidée du code source.

#### <a name="oncreate"></a>OnCreate

La mise à jour **OnCreate** méthode configure le style de police espion comme avant, mais il inclut quelques étapes supplémentaires :

1.  Définit l’image d’arrière-plan le **xamarin_background** ressource qui se trouve dans **Resources/drawable-hdpi/xamarin_background.png**.

2.  Initialise `Paint` objets permettant de dessiner la main de l’heure, minute disponible et seconde aiguille.

3.  Initialise un `Paint` objet pour dessiner les graduations heure autour du bord de la face d’espion.

4.  Crée une minuterie qui appelle le `Invalidate` (méthode) (renouvellement) afin que la seconde aiguille est redessinée chaque seconde. Notez que cette minuterie est nécessaire, car `OnTimeTick` appels `Invalidate` qu’une seule fois par minute.

Cet exemple n'inclut qu’un seul **xamarin_background.png** image ; Toutefois, vous souhaiterez peut-être créer une image d’arrière-plan différente pour chaque densité d’écran personnalisé cadran de votre montre prendra en charge.

#### <a name="ondraw"></a>OnDraw

La mise à jour **OnDraw** méthode dessine une face de style analogique espion en procédant comme suit :

1.  Obtient l’heure actuelle, qui est conservé dans un `time` objet.

2.  Détermine les limites de la surface de dessin et de son centre.

3.  Dessine l’arrière-plan, à l’échelle pour s’ajuster à l’appareil lorsque l’arrière-plan est dessinée.

4.  Dessine douze *graduations* autour de la face de l’horloge (correspondant aux heures cadran de l’horloge).

5.  Calcule l’angle, la rotation et la longueur de chaque aiguille d’espion.

6.  Dessine chaque main sur la surface d’espion. Notez que la seconde aiguille n’est pas dessinée si la surveillance est en mode ambiant.


#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Cette méthode est appelée pour informer `MyWatchFaceEngine` sur les propriétés de l’appareil Wear (par exemple, le mode ambiant bits faible et le phénomène de protection). Dans `MyWatchFaceEngine`, cette méthode vérifie uniquement pour le mode ambiant de bits faible (en mode ambiante bit faible, l’écran prend en charge moins de bits pour chaque couleur).

Pour plus d’informations sur cette méthode, consultez la Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) documentation de l’API.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Cette méthode est appelée quand l’appareil Wear entre ou quitte le mode ambiant. Dans le `MyWatchFaceEngine` implémentation, le cadran de montre désactive l’anticrénelage lorsqu’il est en mode ambiant.

Pour plus d’informations sur cette méthode, consultez la Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) documentation de l’API.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Cette méthode est appelée chaque fois que l’observation devient visible ou masqué. Dans `MyWatchFaceEngine`, cette méthode inscrit/annule l’inscription du récepteur de fuseau horaire (décrit ci-dessous) en fonction de l’état de visibilité.

Pour plus d’informations sur cette méthode, consultez la Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) documentation de l’API.


### <a name="time-zone-feature"></a>Fonctionnalité de fuseau horaire

La nouvelle **MyWatchFaceService.cs** inclut également une fonctionnalité pour mettre à jour l’heure actuelle, chaque fois que l’heure de la zone modifications (par exemple, pendant un déplacement sur plusieurs fuseaux horaires). À la fin du **MyWatchFaceService.cs**, une modification de la zone de temps `BroadcastReceiver` est définie qui gère des objets d’intention changé de fuseau horaire :

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

Le `RegisterTimezoneReceiver` et `UnregisterTimezoneReceiver` méthodes sont appelées par le `OnVisibilityChanged` (méthode).
`UnregisterTimezoneReceiver` est appelée lorsque l’état de visibilité de la face espion est modifié à masqué. Lorsque le cadran de montre est visible à nouveau, `RegisterTimezoneReceiver` est appelée (voir la `OnVisibilityChanged` méthode).

Le moteur `RegisterTimezoneReceiver` méthode déclare un gestionnaire pour ce récepteur de fuseau horaire `Receive` événement ; ce gestionnaire met à jour le `time` objet avec la nouvelle heure de chaque fois qu’un fuseau horaire est dépassé :

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Un filtre d’intention est créé et enregistré pour le récepteur de fuseau horaire :

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

Le `UnregisterTimezoneReceiver` méthode annule l’inscription du récepteur de fuseau horaire :

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Exécutez l’amélioration cadran de montre

Générez et déployez l’application à l’appareil Wear à nouveau. Sélectionnez le cadran de montre dans le sélecteur de visage espion comme avant. La version préliminaire dans le sélecteur d’espion est indiquée sur la gauche, et le nouveau visage d’espion est indiqué à droite :

[![Analogique cadran de montre](creating-a-watchface-images/13-analog-watchface.png "amélioré analogique face dans le sélecteur et sur l’appareil")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

Dans cette capture d’écran, la seconde aiguille correspond à un déplacement une fois par seconde. Lorsque vous exécutez ce code sur un appareil Wear, la seconde aiguille disparaît lorsque la surveillance entre en mode ambiant.


## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, un watchface Android 1.0 Wear personnalisé a été implémenté et testé. Le `CanvasWatchFaceService` et `CanvasWatchFaceService.Engine` classes ont été introduites, et les méthodes de la classe de moteur essentielles ont été implémentées pour créer un cadran numérique simple de montre. Cette implémentation a été mis à jour avec davantage de fonctionnalités pour créer une cadran analogique montre, et des méthodes supplémentaires ont été implémentées pour traiter les modifications de visibilité, le mode ambiant et différences dans les propriétés de l’appareil. Enfin, un récepteur de diffusion de fuseau horaire a été implémenté afin que l’espion met automatiquement à jour le temps si un fuseau horaire est dépassé.


## <a name="related-links"></a>Liens associés

- [Création d’espion visages](https://developer.android.com/training/wearables/watch-faces/index.html)
- [Exemple de WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
