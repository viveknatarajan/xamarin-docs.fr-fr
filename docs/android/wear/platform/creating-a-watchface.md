---
title: Création d’un visage espion
description: Ce guide explique comment implémenter un service de face espion personnalisé pour porter Android. Vous trouverez des instructions détaillées pour la création d’un élément supprimé service de face espion numérique, suivie de davantage de code pour créer un type de style analogique espion.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c02755cc3ff5b46a5a97b6c14185794d8ad538d8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772937"
---
# <a name="creating-a-watch-face"></a>Création d’un visage espion

_Ce guide explique comment implémenter un service de face espion personnalisé pour porter Android. Vous trouverez des instructions détaillées pour la création d’un élément supprimé service de face espion numérique, suivie de davantage de code pour créer un type de style analogique espion._

## <a name="overview"></a>Vue d'ensemble 

Dans cette procédure pas à pas, un service de visage dans une base espion est créé pour illustrer l’essentiel de la création d’un visage d’espion usure Android personnalisé. Le service de face initiale Espion affiche un espion numérique simple qui affiche l’heure actuelle en heures et minutes : 

[![Cadran de la montre numérique](creating-a-watchface-images/01-initial-face.png "capture d’écran de la face espion numérique initiale")](creating-a-watchface-images/01-initial-face.png#lightbox)

Une fois cette face espion numérique est développée et testée, davantage de code est ajouté pour mettre à niveau à plus sophistiquées face espion analogique avec trois mains : 

[![Cadran de la montre analogique](creating-a-watchface-images/02-example-watchface.png "capture d’écran de la face espion analogique final")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Espion face services sont regroupés et installés dans le cadre d’une application d’usure. Dans les exemples suivants, `MainActivity` contient rien d’autre que le code à partir du modèle d’application usure afin que le service de face espion peut être empaqueté et déployé à la surveillance active dans le cadre de l’application. En effet, cette application servira exclusivement un véhicule pour obtenir le service de face espion chargé dans le périphérique d’usure (ou l’émulateur) pour le débogage et le test. 

## <a name="requirements"></a>Spécifications

Pour implémenter un service de face espion, les conditions suivantes sont requises :

-   Android 5.0 (niveau 21 d’API) ou une version ultérieure sur l’appareil d’usure ou l’émulateur.

-   Le [Xamarin Android usure prise en charge des bibliothèques](https://www.nuget.org/packages/Xamarin.Android.Wear) doit être ajouté au projet Xamarin.Android. 

Bien que Android 5.0 est l’API minimale niveau pour implémenter un service de face espion, Android 5.1 ou version ultérieure sont recommandés. Android porter les appareils exécutant Android 5.1 (API 22) ou autorisez les applications d’usure contrôler ce qui est affiché sur l’écran pendant que l’appareil est de faible puissance *ambiante* mode. Lorsque l’appareil quitte faible puissance *ambiante* mode, il se trouve dans *interactive* mode. Pour plus d’informations sur ces modes, consultez [en conservant votre application Visible](https://developer.android.com/training/wearables/apps/always-on.html). 


## <a name="start-an-app-project"></a>Démarrer un projet d’application

Créer un projet Android d’usure appelé **WatchFace** (pour plus d’informations sur la création de nouveaux projets Xamarin.Android, consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)) :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Boîte de dialogue Nouveau projet](creating-a-watchface-images/03-wear-project-vs-sml.png "sélectionner l’application d’usure dans la boîte de dialogue Nouveau projet")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Boîte de dialogue Nouveau projet](creating-a-watchface-images/03-wear-project-xs-sml.png "sélectionner l’application d’usure dans la boîte de dialogue Nouveau projet")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


Le nom du package la valeur `com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Configuration du nom du package](creating-a-watchface-images/04-package-name-vs.png "com.xamarin.watchface la valeur le nom du package")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Configuration du nom du package](creating-a-watchface-images/04-package-name-xs.png "com.xamarin.watchface la valeur le nom du package")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

En outre, faites défiler la liste et activez le **INTERNET** et **WAKE_LOCK** autorisations : 

[![Autorisations requises](creating-a-watchface-images/05-required-permissions-vs.png "autorisations activer INTERNET et WAKE_LOCK")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Définissez la version minimale Android **5.1 Android (API niveau 22)**. En outre, activez la **Internet** et **WakeLock** autorisations :

[![Autorisations requises](creating-a-watchface-images/05-required-permissions-xs.png "autorisations activer Internet et WakeLock")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Ensuite, téléchargez [preview.png](creating-a-watchface-images/preview.png) &ndash; cela sera ajouté à la **drawables** dossier plus loin dans cette procédure pas à pas.


## <a name="add-the-xamarin-android-wear-package"></a>Ajoutez le Package d’usure Xamarin Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Démarrez le Gestionnaire de Package NuGet (dans Visual Studio, cliquez sur **références** dans les **l’Explorateur de solutions** et sélectionnez **gérer les Packages NuGet...** ). Mettre à jour le projet vers la dernière version stable de **Xamarin.Android.Wear**: 

[![Gestionnaire de Package NuGet ajouter](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "ajouter le package Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Ensuite, si **Xamarin.Android.Support.v13** est installé, désinstallez-le :

[![Suppression du Gestionnaire de Package NuGet](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 de suppression")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Démarrez le Gestionnaire de Package NuGet (dans Visual Studio pour Mac, cliquez sur **Packages** dans les **volet Solution** et sélectionnez **ajouter des Packages** ). Mettre à jour le projet vers la dernière version stable de **Xamarin.Android.Wear**: 

[![Gestionnaire de Package NuGet ajouter](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "ajouter le package Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Générer et exécuter l’application sur un émulateur ou un périphérique d’usure (pour plus d’informations, consultez la [mise en route](~/android/wear/get-started/index.md) guide). Vous devez voir l’écran suivant de l’application sur le périphérique d’usure :

[![Capture d’écran de l’application](creating-a-watchface-images/08-app-screen.png "écran de l’application sur l’appareil d’usure")](creating-a-watchface-images/08-app-screen.png#lightbox)

À ce stade, l’application d’usure base n’a pas espion face fonctionnalité car elle ne fournit pas encore d’une implémentation de service face espion. Ce service sera ensuite ajouté. 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android usure implémente regarder faces via la `CanvasWatchFaceService` classe. `CanvasWatchFaceService` est dérivé de `WatchFaceService`, qui elle-même est dérivée de `WallpaperService` comme indiqué dans le diagramme suivant : 

[![Diagramme d’héritage](creating-a-watchface-images/09-inheritance-diagram-sml.png "diagramme de CanvasWatchFaceService d’héritage")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` inclut une liste imbriquée `CanvasWatchFaceService.Engine`; il instancie une `CanvasWatchFaceService.Engine` objet qui effectue le travail de dessin du cadran de la montre. `CanvasWatchFaceService.Engine` est dérivé de `WallpaperService.Engine` comme indiqué dans le diagramme ci-dessus. 

Ne pas représenté dans ce diagramme est un `Canvas` qui `CanvasWatchFaceService` utilise pour le dessin de la face espion &ndash; cela `Canvas` est transmise le `OnDraw` méthode comme décrit ci-dessous. 

Dans les sections suivantes, un service de face espion personnalisé sera créé en suivant ces étapes : 

1.  Définir une classe appelée `MyWatchFaceService` qui est dérivée de `CanvasWatchFaceService`. 

2.  Dans `MyWatchFaceService`, créez une classe imbriquée nommée `MyWatchFaceEngine` qui est dérivée de `CanvasWatchFaceService.Engine`. 

3.  Dans `MyWatchFaceService`, implémentez un `CreateEngine` méthode qui instancie `MyWatchFaceEngine` et le retourne.

4.  Dans `MyWatchFaceEngine`, implémenter la `OnCreate` méthode pour créer le style de police espion et effectuer d’autres tâches d’initialisation. 

5.  Implémentez la `OnDraw` méthode `MyWatchFaceEngine`. Cette méthode est appelée chaque fois que la face espion doit être redessiné (c'est-à-dire *invalidés*). `OnDraw` est la méthode qui dessine (et redessine) des éléments de face espion telles que l’heure, minute et seconde mains. 

6.  Implémentez la `OnTimeTick` méthode `MyWatchFaceEngine`. 
    `OnTimeTick` est appelée au moins une fois par minute (en modes ambiantes et interactifs) ou lorsque la date/heure a changé. 

Pour plus d’informations sur `CanvasWatchFaceService`, consultez le Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) documentation de l’API.
De même, [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) explique l’implémentation réelle de la face espion.


### <a name="add-the-canvaswatchfaceservice"></a>Ajouter le CanvasWatchFaceService

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ajouter un nouveau fichier appelé **MyWatchFaceService.cs** (dans Visual Studio, cliquez sur **WatchFace** dans les **l’Explorateur de solutions**, cliquez sur **Ajouter > nouvel élément...** , puis sélectionnez **classe**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ajouter un nouveau fichier appelé **MyWatchFaceService.cs** (dans Visual Studio pour Mac, cliquez sur le **WatchFace** de projet, cliquez sur **Ajouter > nouveau fichier...** , puis sélectionnez **classe vide**). 

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

`MyWatchFaceService` (dérivée de `CanvasWatchFaceService`) est le programme « principal » de la face espion. `MyWatchFaceService` implémente une seule méthode, `OnCreateEngine`, qui instancie et retourne un `MyWatchFaceEngine` objet (`MyWatchFaceEngine` est dérivée de `CanvasWatchFaceService.Engine`). L’instanciation `MyWatchFaceEngine` objet doit être retourné comme un `WallpaperService.Engine`. L’encapsulation `MyWatchFaceService` objet est passé au constructeur. 

`MyWatchFaceEngine` est l’implémentation de visage réel espion &ndash; il contienne le code qui dessine le cadran de la montre. Il gère également des événements système tels que des modifications de l’écran (modes ambiante/interactive, écran de mise hors tension, etc..). 

 
### <a name="implement-the-engine-oncreate-method"></a>Implémentez la méthode du moteur de OnCreate

Le `OnCreate` méthode initialise le cadran de la montre. Ajoutez le champ suivant à `MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

Cela `Paint` objet sera utilisé pour dessiner l’heure actuelle sur le cadran de la montre. Ensuite, ajoutez la méthode suivante à `MyWatchFaceEngine`: 

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

`OnCreate` est appelé peu après `MyWatchFaceEngine` est démarré. Il configure le `WatchFaceStyle` (ce qui contrôle la façon dont l’appareil usure interagit avec l’utilisateur) et instancie les `Paint` objet qui servira à afficher l’heure. 

L’appel à `SetWatchFaceStyle` effectue les opérations suivantes :

1.  Jeux de *mode d’aperçu* à `PeekModeShort`, ce qui entraîne des notifications apparaissent comme des cartes de petits « aperçu » sur l’affichage. 

2.  Définit la visibilité de l’arrière-plan sur `Interruptive`, ce qui entraîne l’arrière-plan d’une carte d’aperçu s’affiche brièvement si elle représente une notification par.

3.  Désactive l’heure de l’interface utilisateur du système par défaut d’être dessinée sur la face Espion afin que l’image personnalisée espion peut afficher l’heure à la place.

Pour plus d’informations sur ces et d’autres options de style de police espion, consultez le Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) documentation de l’API.

Après avoir `SetWatchFaceStyle` terminée, `OnCreate` instancie le `Paint` objet (`hoursPaint`) et définir sa couleur blanc et la taille du texte à 48 pixels ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) doit être spécifié en pixels). 


### <a name="implement-the-engine-ondraw-method"></a>Implémentez la méthode OnDraw de moteur

Le `OnDraw` méthode est sans doute le plus important `CanvasWatchFaceService.Engine` méthode &ndash; c’est la méthode que réellement dessine regarder la face des éléments tels que des chiffres et aiguilles de face. Dans l’exemple suivant, il dessine la chaîne d’heure sur le cadran de la montre.
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

Lorsque Android appelle `OnDraw`, elle passe dans un `Canvas` instance et les limites dans lesquelles l’image peut être tracée. Dans l’exemple de code ci-dessus, `DateTime` est utilisé pour calculer l’heure actuelle, en heures et minutes (au format de 12 heures). La chaîne d’heure obtenue est dessinée sur la zone de dessin à l’aide de la `Canvas.DrawText` (méthode). La chaîne sera 70 pixels sur à partir du bord gauche et 80 pixels vers le bas à partir du bord supérieur. 

Pour plus d’informations sur la `OnDraw` (méthode), consultez le Android [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) documentation de l’API.


### <a name="implement-the-engine-ontimetick-method"></a>Implémentez la méthode OnTimeTick du moteur

Android appelle régulièrement la `OnTimeTick` pour mettre à jour l’heure affichée dans le cadran de la montre. Elle est appelée au moins une fois par minute (dans les modes ambiantes et interactif), ou lorsque la date/heure ou un fuseau horaire ont été modifiés. Ajoutez la méthode suivante à `MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Cette implémentation de `OnTimeTick` appelle simplement `Invalidate`. Le `Invalidate` planifications de la méthode `OnDraw` pour redessiner le cadran de la montre. 

Pour plus d’informations sur la `OnTimeTick` (méthode), consultez le Android [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) documentation de l’API.

 
## <a name="register-the-canvaswatchfaceservice"></a>Enregistrer le CanvasWatchFaceService

`MyWatchFaceService` doit être enregistré dans le **AndroidManifest.xml** de l’application d’usure associée. Pour ce faire, ajoutez le code XML suivant à la `<application>` section : 

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

1.  Définit le `android.permission.BIND_WALLPAPER` autorisation. Cette autorisation donne l’autorisation de service espion face pour modifier le papier peint du système sur l’appareil. Notez que cette autorisation doit être définie dans le `<service>` section plutôt que dans la liste externe `<application>` section. 

2.  Définit un `watch_face` ressource. Cette ressource est un fichier XML court qui déclare un `wallpaper` ressource (ce fichier sera créé dans la section suivante). 

3.  Déclare une image drawable appelée `preview` qui sera affiché à l’écran de sélection de sélecteur espion. 

4.  Inclut un `intent-filter` pour informer Android qui `MyWatchFaceSevice` affichera un visage espion. 

Ainsi se termine le code de base `WatchFace` exemple. L’étape suivante consiste à ajouter les ressources nécessaires.

 
## <a name="add-resource-files"></a>Ajouter des fichiers de ressources

Avant de pouvoir exécuter le service de surveillance, vous devez ajouter le **watch_face** ressource et l’image d’aperçu. Commencez par créer un nouveau fichier XML à **Resources/xml/watch_face.xml** et remplacez son contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Action de génération de ce fichier **AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Action de génération](creating-a-watchface-images/10-android-resource-vs.png "ensemble build action AndroidResource")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Action de génération](creating-a-watchface-images/10-android-resource-xs.png "ensemble build action AndroidResource")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Ce fichier de ressources définit un simple `wallpaper` élément qui sera utilisé pour le cadran de la montre. 

Si vous ne le n'avez pas encore fait, téléchargez [preview.png](creating-a-watchface-images/preview.png). Installez-le à **Resources/drawable/preview.png**. Veillez à ajouter ce fichier pour le `WatchFace` projet. Cette image d’aperçu s’affiche à l’utilisateur dans le sélecteur de face espion sur l’appareil d’usure. Pour créer une image d’aperçu de votre propre image espion, vous pouvez prendre une capture d’écran de la face espion pendant son exécution. (Pour plus d’informations sur l’obtention des captures d’écran des appareils d’usure, consultez [prend des captures d’écran](~/android/wear/deploy-test/debug-on-device.md#screenshots)). 


## <a name="try-it"></a>Essayez !

Générez et déployez l’application sur le périphérique d’usure. Vous devez voir l’écran de l’application usure apparaissent comme avant. Procédez comme suit pour activer le nouveau visage de surveillance : 

1.  Faites défiler vers la droite jusqu'à ce que l’arrière-plan de l’écran de surveillance. 

2.  Appuyez sur et contenir n’importe où sur l’arrière-plan de l’écran de deux secondes.

3.  Effectuez un balayage de gauche à droite pour parcourir les faces espion différents. 

4.  Sélectionnez le **Xamarin exemple** regarder face (affiché sur la droite) : 

    [![Sélecteur de Watchface](creating-a-watchface-images/11-watchface-picker.png "balayez pour localiser le cadran de la montre des exemples de Xamarin")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Appuyez sur la **Xamarin exemple** regarder face pour le sélectionner. 

Cela modifie la face espion de l’appareil d’usure pour utiliser le service de face espion personnalisé implémenté jusqu'à présent : 

[![Cadran de la montre numérique](creating-a-watchface-images/12-digital-watchface.png "espion de numérique personnalisée en cours d’exécution sur l’appareil d’usure")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Il s’agit un visage espion relativement brute, car l’implémentation de l’application est donc minime (par exemple, il n’inclut pas un arrière-plan de face espion et il n’appelle pas `Paint` lissage des méthodes pour améliorer l’apparence). Toutefois, il implémente la fonctionnalité simple qui est requise pour créer un type personnalisé espion. 

Dans la section suivante, cette face espion est mis à niveau vers une implémentation plus sophistiquée. 

 
## <a name="upgrading-the-watch-face"></a>La mise à niveau de la Face espion

Dans le reste de cette procédure pas à pas, `MyWatchFaceService` est mis à niveau pour afficher un type de style analogique espion et il est étendu pour prendre en charge davantage de fonctionnalités. Les fonctionnalités suivantes vont être ajoutées pour créer le cadran de la mise à niveau montre : 

1.  Indique l’heure avec analogique heure, minute et seconde mains.

2.  Réagit aux modifications de la visibilité.

3.  Traite les modifications apportées entre mode ambiant et le mode interactif. 

4.  Lit les propriétés de l’appareil d’usure sous-jacent.

5.  Met automatiquement à jour l’heure quand un changement de fuseau horaire a lieu. 

Avant d’implémenter les modifications de code ci-dessous, vous devez télécharger [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), le décompresser et déplacer les fichiers .png décompressé **drawable/ressources** (remplacer la précédente **preview.png**). Ajouter les nouveaux fichiers .png le `WatchFace` projet.


### <a name="update-engine-features"></a>Fonctionnalités du moteur de mise à jour

L’étape suivante est mise à niveau **MyWatchFaceService.cs** à une implémentation qui dessine un visage analogique espion et prend en charge les nouvelles fonctionnalités. Remplacez le contenu de **MyWatchFaceService.cs** avec la version analogique du code espion face dans [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (vous pouvez couper et coller l’existants de cette source de  **MyWatchFaceService.cs**). 

Cette version de **MyWatchFaceService.cs** ajoute plus de code pour les méthodes existantes et inclut des méthodes substituées pour ajouter des fonctionnalités supplémentaires. Les sections suivantes fournissent une visite guidée du code source.

#### <a name="oncreate"></a>OnCreate

La mise à jour **OnCreate** méthode configure le style de police espion comme avant, mais il comprend quelques étapes supplémentaires : 

1.  Définit l’image d’arrière-plan le **xamarin_background** ressource qui réside dans **Resources/drawable-hdpi/xamarin_background.png**. 

2.  Initialise `Paint` objets pour le dessin de la main de l’heure, minute disponible et aiguille.

3.  Initialise un `Paint` objet pour le dessin des graduations heure autour du bord de la face espion. 

4.  Crée un minuteur qui appelle le `Invalidate` (méthode) (renouvellement) afin que l’aiguille sera redessinée à chaque seconde. Notez que cette minuterie est nécessaire car `OnTimeTick` appelle `Invalidate` qu’une seule fois par minute.

Cet exemple n'inclut qu’un seul **xamarin_background.png** image ; Toutefois, vous souhaitez créer une image d’arrière-plan différente pour chaque densité d’écran qui prend en charge cadran de votre montre personnalisé. 

#### <a name="ondraw"></a>OnDraw

La mise à jour **OnDraw** méthode dessine un visage de style analogique espion en procédant comme suit : 

1.  Obtient l’heure actuelle, qui est conservé dans un `time` objet. 

2.  Détermine les limites de la surface de dessin et de son centre.

3.  Dessine l’arrière-plan, ajusté en fonction de l’appareil lors de l’arrière-plan est dessiné.

4.  Dessine douze *graduations* autour de l’image de l’horloge (correspondant aux heures sur le cadran). 

5.  Calcule l’angle, la rotation et la longueur de chaque aiguille espion.

6.  Dessine chaque disponible sur l’aire de surveillance. Notez que l’aiguille n’est pas dessiné Si la surveillance est en mode ambiant. 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Cette méthode est appelée pour informer `MyWatchFaceEngine` sur les propriétés de l’appareil d’usure (comme faible ambiants bits et dans l’avancement de protection). Dans `MyWatchFaceEngine`, cette méthode vérifie uniquement pour le mode ambiant de bits de faible (en mode de faible ambiante, l’écran prend en charge moins de bits pour chaque couleur). 

Pour plus d’informations sur cette méthode, consultez la Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) documentation de l’API.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Cette méthode est appelée lorsque l’appareil d’usure entre ou quitte le mode ambiant. Dans la `MyWatchFaceEngine` implémentation, la face espion désactive l’anticrénelage lorsqu’il est en mode ambiant. 

Pour plus d’informations sur cette méthode, consultez la Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) documentation de l’API.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Cette méthode est appelée chaque fois que l’observation devient visible ou masqué. Dans `MyWatchFaceEngine`, cette méthode inscrit/annule l’inscription du récepteur de fuseau horaire (décrite ci-dessous), en fonction de l’état de visibilité. 

Pour plus d’informations sur cette méthode, consultez la Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) documentation de l’API.

 
### <a name="time-zone-feature"></a>Fonctionnalité de fuseau horaire

La nouvelle **MyWatchFaceService.cs** inclut également les fonctionnalités pour mettre à jour l’heure actuelle à chaque fois que l’heure de la zone des modifications (par exemple, pendant un déplacement sur plusieurs fuseaux horaires). Vers la fin de **MyWatchFaceService.cs**, une modification de la zone `BroadcastReceiver` est définie qui gère les objets d’intention changé de fuseau horaire : 

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
`UnregisterTimezoneReceiver` est appelé lorsque l’état de visibilité de la face espion est modifié à masqué. Lorsque la face espion est visible, `RegisterTimezoneReceiver` est appelé (consultez la `OnVisibilityChanged` méthode). 

Le moteur `RegisterTimezoneReceiver` méthode déclare un gestionnaire pour ce récepteur de fuseau horaire `Receive` événement ; ce gestionnaire met à jour le `time` objet avec la nouvelle heure de chaque fois qu’un fuseau horaire est dépassé : 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Un filtre d’intention est créé et inscrit pour le récepteur de fuseau horaire :

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

Le `UnregisterTimezoneReceiver` méthode annule l’inscription du récepteur de fuseau horaire :

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Exécuter la Face espion améliorée

Générez et déployez l’application à l’appareil d’usure à nouveau. Sélectionnez la face espion dans le sélecteur de face d’espion comme avant. La version d’évaluation dans le sélecteur d’espion est indiquée sur la gauche, et le nouveau visage espion est affiché à droite :

[![Cadran de la montre analogique](creating-a-watchface-images/13-analog-watchface.png "améliorée face analogique dans le sélecteur et sur l’appareil")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

Dans cette capture d’écran, l’aiguille se déplacent une fois par seconde. Lorsque vous exécutez ce code sur un appareil usure, l’aiguille disparaît lors de la surveillance en mode ambiante.

 
## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, un watchface usure Android personnalisé a été implémentée et testée. Le `CanvasWatchFaceService` et `CanvasWatchFaceService.Engine` classes ont été introduites, et les méthodes essentielles de la classe du moteur ont été implémentées pour créer un type simple espion numérique. Cette implémentation a été mis à jour avec davantage de fonctionnalités pour créer une image analogique espion et des méthodes supplémentaires ont été implémentées pour traiter des modifications de la visibilité, au mode ambiant et les différences dans les propriétés de l’appareil. Enfin, un récepteur de diffusion de fuseau horaire a été implémenté pour que la surveillance met automatiquement à jour l’heure, si un fuseau horaire est dépassé. 


## <a name="related-links"></a>Liens associés

- [Création de face espion](https://developer.android.com/training/wearables/watch-faces/index.html)
- [Exemple de WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
