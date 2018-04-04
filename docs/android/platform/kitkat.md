---
title: Fonctionnalités de KitKat
description: Android 4.4 (KitKat) est fournie avec une multitude de fonctionnalités pour les utilisateurs et les développeurs. Ce guide met en surbrillance plusieurs de ces fonctionnalités et fournit des exemples de code et les détails d’implémentation pour vous aider à tirer le meilleur parti KitKat.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 3c3eafc8dc18113080dd6c906025556292c43e1c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="kitkat-features"></a>Fonctionnalités de KitKat

_Android 4.4 (KitKat) est fournie avec une multitude de fonctionnalités pour les utilisateurs et les développeurs. Ce guide met en surbrillance plusieurs de ces fonctionnalités et fournit des exemples de code et les détails d’implémentation pour vous aider à tirer le meilleur parti KitKat._

## <a name="overview"></a>Vue d'ensemble

Android 4.4 (API niveau 19), également appelé « KitKat », a été publiée en 2013 à liaison tardive. KitKat offre une variété de nouvelles fonctionnalités et améliorations, notamment :

-  [Expérience utilisateur](#user_experience) &ndash; facile animations de transition framework, barres d’état et la navigation translucides, et le mode plein écran immersif aider à créer une meilleure expérience de l’utilisateur.

-  [Contenu de l’utilisateur](#user_content) &ndash; gestion des fichiers utilisateur simplifiée avec l’infrastructure d’accès de stockage ; impression d’images, des sites web et autres contenus est plus facile avec l’impression améliorée API.

-  [Matériel](#hardware) &ndash; activer n’importe quelle application sur une carte NFC avec NFC émulation de la carte sur l’hôte, d’exécuter les capteurs de faible puissance avec le `SensorManager` .

-  [Outils de développement](#developer_tools) &ndash; capture les applications en cours avec le client Android Debug Bridge, disponible dans le cadre du SDK Android.


Ce guide fournit des conseils pour migrer une application existante de Xamarin.Android KitKat, ainsi que d’une vue d’ensemble de KitKat pour les développeurs de Xamarin.Android.

## <a name="requirements"></a>Spécifications

Pour développer des applications de Xamarin.Android à l’aide de KitKat, vous devez *Xamarin.Android 4.11.0* ou supérieur et Android 4.4 (API niveau 19) installé via le Gestionnaire de SDK Android, comme illustré par la capture d’écran suivante :

[![Sélection d’Android 4.4 dans le Gestionnaire du SDK Android](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Migration de votre application vers KitKat

Cette section fournit certains éléments de premier-réponse pour les applications existantes de transition pour Android 4.4.

### <a name="check-system-version"></a>Vérifiez la Version du système

Si une application doit être compatible avec les versions antérieures d’Android, veillez à encapsuler tout code KitKat spécifique dans un contrôle de version système, comme illustré dans l’exemple de code ci-dessous :

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Le traitement par lot d’alarme

Android utilise les services de l’alarme à sortir de veille une application en arrière-plan à un moment donné. KitKat cela prend un peu plus loin par alarmes pour conserver la puissance de traitement par lot. Cela signifie que, au lieu de faire sortir de veille chaque application à un moment exact, KitKat préfère regrouper plusieurs applications qui sont inscrits pour l’éveil pendant le même intervalle de temps et les sortir de veille en même temps.
Pour indiquer à Android à une application de sortir de veille pendant un intervalle de temps spécifié, appelez `SetWindow` sur la [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/), en passant la valeur minimale et une durée maximale, en millisecondes, qui peuvent s’écouler avant que l’application quitte le mode veille et l’opération à effectuer à la mise en éveil.
Le code suivant fournit un exemple d’une application qui doit être mis entre une demi-heure et une heure pour la durée de que la fenêtre est définie :

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Pour continuer la mise en éveil d’une application à un moment exact, utilisez `SetExact`, en passant l’heure exacte à laquelle l’application doit être quitte le mode veille et l’opération à effectuer :

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat n’est plus vous permet de définir une alarme extensible exacte. Les applications qui utilisent [ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/) et nécessitent des alarmes exactes fonctionne devez maintenant va déclencher chaque alarme manuellement.

### <a name="external-storage"></a>Stockage externe

Stockage externe est désormais divisé en deux types - stockage unique pour votre application, ainsi que les données partagées par plusieurs applications. Lecture et en écriture vers un emplacement spécifique de votre application sur un stockage externe nécessitent aucune autorisation particulière. Interaction avec des données sur le stockage partagé maintenant requiert le `READ_EXTERNAL_STORAGE` ou `WRITE_EXTERNAL_STORAGE` autorisation. Les deux types peuvent être classés comme tels :

-  Si vous obtenez un chemin d’accès du fichier ou répertoire en appelant une méthode sur `Context` , par exemple, [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/) ou [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - votre application ne nécessite aucune autorisation supplémentaire.

-  Si vous obtenez un chemin d’accès du fichier ou le répertoire par l’accès à une propriété ou l’appel d’une méthode sur `Environment` , tel que [ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/) ou [ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) , votre application nécessite la `READ_EXTERNAL_STORAGE` ou `WRITE_EXTERNAL_STORAGE` autorisation.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` implique le `READ_EXTERNAL_STORAGE` autorisation, donc vous devez toujours définir une autorisation.

### <a name="sms-consolidation"></a>Consolidation de SMS

KitKat simplifie la messagerie de l’utilisateur en regroupant tous les contenus SMS dans l’application d’une valeur par défaut sélectionnée par l’utilisateur. Le développeur est chargé pour rendre l’application pouvant être sélectionnées en tant que l’application de messagerie par défaut et comporte de manière appropriée dans le code et dans la vie si l’application n’est pas sélectionnée. Pour plus d’informations sur la transition de votre application SMS pour KitKat, reportez-vous à la [mise en route votre SMS applications prêt pour KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) guide à partir de Google.

### <a name="webview-apps"></a>Applications WebView

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) a reçu un changement dans KitKat. La plus importante modification est ajoutée à la sécurité pour le chargement de contenu dans un `WebView`. La plupart des applications qui ciblent des versions d’API antérieures doit fonctionner comme prévu, test des applications qui utilisent la `WebView` classe est fortement recommandé. Pour plus d’informations sur les APIs WebView affectés, reportez-vous à la Android [migration vers WebView Android 4.4](http://developer.android.com/guide/webapps/migrating.html) documentation.

<a name="user_experience" />

## <a name="user-experience"></a>Expérience utilisateur

KitKat est fourni avec plusieurs nouvelles API pour améliorer l’expérience utilisateur, y compris la nouvelle infrastructure de transition pour la gestion des animations de propriété et une option de l’interface utilisateur translucide pour le thème. Ces modifications sont abordées ci-dessous.

### <a name="transition-framework"></a>Infrastructure de transition

L’infrastructure de transition rend les animations plus facile à implémenter. KitKat vous permet d’effectuer une animation de la propriété simple avec une seule ligne de code ou personnaliser à l’aide de transitions *scènes*.

#### <a name="simple-property-animation"></a>Animation de la propriété simple

La nouvelle bibliothèque de Transitions Android simplifie le code derrière les animations de propriété. L’infrastructure permet d’exécuter des animations simples avec un minimum de code. Par exemple, le code suivant utilise des exemples [ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/) pour animer les afficher et masquer un `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

L’exemple ci-dessus utilise l’infrastructure de transition pour créer une transition par défaut entre les valeurs de propriété variables automatique. Étant donné que l’animation est gérée par une seule ligne de code, vous pouvez facilement apporter ce compatible avec les versions antérieures d’Android en encapsulant la `BeginDelayedTransition` appeler dans un contrôle de version du système. Consultez le [migration de votre application à KitKat](#Migrating_Your_App_to_KitKat) section pour plus d’informations.

La capture d’écran ci-dessous montre l’application avant que l’animation :

[![Capture d’écran de l’application avant le démarrage de l’animation](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

La capture d’écran ci-dessous montre l’application après l’animation :

[![Capture d’écran de l’application une fois l’animation terminée](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Vous pouvez obtenir davantage de contrôle sur la transition des séquences qui sont traitées dans la section suivante.

#### <a name="android-scenes"></a>Scènes Android

[Scènes](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) ont été introduites dans le cadre de l’infrastructure de transition pour donner le développeur contrôle accru des animations. Scènes créer une zone dynamique dans l’interface utilisateur : vous spécifiez un conteneur et plusieurs versions ou « en arrière-plan », pour le contenu XML à l’intérieur du conteneur, et Android ne le reste du travail pour animer les transitions entre en arrière-plan. Android en arrière-plan vous permettre de créer des animations complexes avec un minimum de travail sur le côté de développement.

L’élément d’interface statique hébergeant le contenu dynamique est appelé un *conteneur* ou *scène base*. L’exemple ci-dessous utilise le concepteur Android pour créer un `RelativeLayout` appelée `container`:

[![À l’aide du concepteur Android pour créer un conteneur RelativeLayout](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

La mise en page de l’exemple définit également un bouton appelé `sceneButton` sous le `container`. Ce bouton déclenche la transition.

Le contenu dynamique à l’intérieur du conteneur nécessite deux dispositions Android nouvelle. Ces dispositions spécifient uniquement le code *à l’intérieur de* le conteneur.
L’exemple de code ci-dessous définit une disposition appelée *Scene1* contenant deux champs de texte lors de la lecture « Kit » et « Kat » respectivement, et une deuxième disposition appelé *Scene2* qui contient les mêmes champs texte inversés. Le code XML est la suivante :

 **Scene1.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

L’exemple ci-dessus utilise `merge` pour raccourcir le code de vue et de simplifier la hiérarchie de la vue. Vous pouvez en savoir plus sur `merge` dispositions [ici](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Une scène est créée en appelant [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/), en passant l’objet de conteneur, l’ID de ressource de fichier de disposition de la scène et en cours `Context`, comme illustré dans l’exemple de code ci-dessous :

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Cliquez sur le bouton bascule entre les deux séquences, Android réalise une animation avec les valeurs de transition par défaut :

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

La capture d’écran ci-dessous illustre la scène avant que l’animation :

[![Capture d’écran de l’application avant le démarrage de l’animation](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

La capture d’écran ci-dessous illustre la scène après l’animation :

[![Capture d’écran de l’application une fois l’animation terminée](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> Il existe un [bogue connu](https://code.google.com/p/android/issues/detail?id=62450) dans les Transitions Android bibliothèque qui provoque des scènes créée à l’aide de `GetSceneForLayout` pour l’arrêter quand un utilisateur accède à une activité, la deuxième fois. Décrit une solution de contournement java [ici](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Transitions personnalisées dans les scènes

Une transition personnalisée peut être définie dans un fichier de ressources xml dans le `transition` répertoire sous `Resources`, comme illustré par la capture d’écran ci-dessous :

[![Emplacement du fichier transition.xml sous le répertoire de ressources/transition](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

L’exemple de code suivant définit une transition qui réalise une animation de 5 secondes et utilise le [dépassement interpolateur](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

La transition est créée dans l’activité à l’aide de la [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/), comme illustré par le code ci-dessous :

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

La nouvelle transition est ensuite ajoutée à la `Go` appel qui démarre l’animation :

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Interface utilisateur translucide

KitKat vous renforcez votre contrôle sur les thèmes votre application avec des barres d’état et la navigation transclucent facultatif. Vous pouvez modifier la transparence d’éléments d’interface utilisateur système dans le même fichier XML que vous utilisez pour définir votre thème Android. KitKat présente les propriétés suivantes :

-  `windowTranslucentStatus` -Lorsque la valeur true, rend la barre d’état supérieur translucide.

-  `windowTranslucentNavigation` -Lorsque la valeur true, rend la barre de navigation inférieure translucide.

-  `fitsSystemWindows` -La définition de la barre supérieure ou inférieure à transcluent décale contenu sous les éléments d’interface utilisateur transparents par défaut. Si cette propriété `true` est un moyen simple pour empêcher que le contenu qui se chevauchent avec les éléments d’interface utilisateur système translucide.


Le code suivant définit un thème avec translucides barres d’état et la navigation :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

La capture d’écran ci-dessous montre le thème ci-dessus avec état translucide et barres de navigation :

[![Capture d’écran de l’exemple d’application avec des barres d’état et la navigation translucides](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>Contenu de l’utilisateur

### <a name="storage-access-framework"></a>Infrastructure d’accès au stockage

L’infrastructure de l’accès de stockage (SAF) est une nouvelle façon pour les utilisateurs à interagir avec le contenu stocké, tels que des images, vidéos et documents. Au lieu de présenter aux utilisateurs une boîte de dialogue pour choisir une application de gérer le contenu, KitKat ouvre une nouvelle interface utilisateur qui permet aux utilisateurs d’accéder à leurs données dans un emplacement d’agrégation. Une fois que le contenu a été choisi, l’utilisateur retourne à l’application qui a demandé le contenu et l’expérience de l’application pourront se poursuivre.

Cette modification nécessite deux actions sur le côté du développeur : tout d’abord, les applications qui nécessitent le contenu des fournisseurs doivent être mis à jour vers une nouvelle façon de reqesting contenu. Ensuite, applications qui écrivent des données dans un `ContentProvider` doivent être modifiés pour utiliser la nouvelle infrastructure. Les deux scénarios dépendent de la nouvelle [ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) API.

#### <a name="documentsprovider"></a>DocumentsProvider

Dans KitKat, les interactions avec `ContentProviders` sont abstraits avec la `DocumentsProvider` classe. Cela signifie que SAF ne tient pas compte où les données sont physiquement, tant qu’il est accessible via la `DocumentsProvider` API. Fournisseurs locales, cloud services et périphériques de stockage externes utilisent tous la même interface et sont traitée de la même façon, fournir à l’utilisateur et le développeur d’un seul emplacement à interagir avec le contenu de l’utilisateur.

Cette section explique comment charger et enregistrer le contenu avec l’infrastructure d’accès de stockage.

#### <a name="request-content-from-a-provider"></a>Demander du contenu à partir d’un fournisseur

Nous pouvons KitKat que vous voulez choisir le contenu à l’aide de l’UI SAF avec la `ActionOpenDocument` intention, ce qui signifie que vous voulez vous connecter à tous les fournisseurs de contenu disponibles sur l’appareil. Vous pouvez ajouter une partie du filtrage à cette intention en spécifiant `CategoryOpenable`, ce qui signifie que seul le contenu qui peut être ouvert (autrement dit, accessible et utilisable du contenu) est retourné. KitKat autorise également le filtrage du contenu avec le `MimeType`. Par exemple, le code suivant filtre les résultats image en spécifiant l’image `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Appel de `StartActivityForResult` lance la SAF UI, dont l’utilisateur peut ensuite Parcourir pour choisir une image :

[![Capture d’écran de l’exemple d’une application à l’aide de l’infrastructure d’accès de stockage pour naviguer vers une image](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

Une fois que l’utilisateur a choisi une image, `OnActivityResult` retourne le `Android.Net.Uri` du fichier choisi. L’exemple de code suivant affiche la sélection d’image de l’utilisateur :

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>Écriture du contenu à un fournisseur

En plus du chargement du contenu à partir de l’UI SAF, KitKat permet également de vous enregistrer le contenu dans les `ContentProvider` qui implémente le `DocumentProvider` API. L’enregistrement de contenu utilise un `Intent` avec `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

L’exemple de code ci-dessus charge la UI SAF, l’utilisateur vous permettant de modifier le nom de fichier et sélectionnez un répertoire pour héberger le nouveau fichier :

[![Capture d’écran de l’utilisateur modifie le nom de fichier à NewDoc dans le répertoire téléchargements](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Lorsque l’utilisateur appuie sur **enregistrer**, `OnActivityResult` passé le `Android.Net.Uri` du fichier qui vient d’être créé, qui est accessible avec `data.Data`. L’uri peut être utilisé pour transmettre des données dans le nouveau fichier :

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Notez que [ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri)) retourne un `System.IO.Stream`, de sorte que tout le processus de diffusion en continu peuvent être écrits dans .NET.

Pour plus d’informations sur le chargement, création et modification de contenu avec l’infrastructure d’accès de stockage, consultez le [documentation d’Android pour l’infrastructure d’accès de stockage](http://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Impression

Contenu de l’impression est simplifié dans KitKat avec l’introduction de la [Services d’impression](https://developer.xamarin.com/api/namespace/Android.PrintServices/) et `PrintManager`. KitKat est également la première version d’API pour tirer pleinement parti de la [API des services d’impression de Cloud de Google](https://developers.google.com/cloud-print/) à l’aide de la [application de Google Cloud impression](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
La plupart des périphériques qui sont fournis avec KitKat automatiquement télécharger l’application de Google Cloud imprimer et [plug-in du Service HP impression](https://play.google.com/store/apps/details?id=com.hp.android.printservice)lorsque leur première connexion au réseau sans fil. Un utilisateur peut vérifier les paramètres d’impression de son appareil en accédant à **Paramètres > système > impression**:

[![Capture d’écran de l’écran de paramètres d’impression](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> Bien que les API d’impression sont configurés pour fonctionner avec Google Cloud d’impression par défaut, Android permet toujours aux développeurs de préparer le contenu d’impression à l’aide des nouvelles API et les envoyer à d’autres applications pour gérer l’impression.



#### <a name="printing-html-content"></a>Contenu HTML de l’impression

KitKat crée automatiquement un [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) pour un affichage web avec `WebView.CreatePrintDocumentAdapter`. Contenu de l’impression web est un effort coordonné entre un [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) qui attend que le contenu HTML à charger et vous permet de l’activité pour faire de l’option d’impression disponibles dans le menu options et Actvity, ce qui attend que l’utilisateur de Sélectionnez les options d’impression et les appels `Print`sur la `PrintManager`. Cette section décrit la configuration de base requise pour imprimer à l’écran du contenu HTML.

Notez que le chargement et l’impression du contenu web nécessitent l’autorisation d’Internet :

[![Définition des autorisations Internet dans les options de l’application](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Élément de Menu Imprimer

L’option d’impression apparaît généralement dans l’activité [menu options](http://developer.android.com/guide/topics/ui/menus.html#options-menu).
Le menu options permet aux utilisateurs d’effectuer des actions sur une activité. Il est dans l’angle supérieur droit de l’écran et ressemble à ceci :

[![Capture d’écran d’affichés d’élément de menu d’imprimer dans l’angle supérieur droit de l’écran](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


Éléments de menu supplémentaires peuvent être définies dans le *menu*répertoire sous *ressources*. Le code ci-dessous définit un menu exemple appelé [impression](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Interaction avec le menu options de l’activité se produit via le `OnCreateOptionsMenu` et `OnOptionsItemSelected` méthodes.
`OnCreateOptionsMenu` est l’endroit pour ajouter de nouveaux éléments de menu, comme l’option d’impression, à partir de la *menu* répertoire des ressources.
`OnOptionsItemSelected` écoute de l’utilisateur en sélectionnant l’option d’impression à partir du menu et commence à imprimer :

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

Le code ci-dessus définit également une variable appelée `dataLoaded` pour effectuer le suivi de l’état du contenu HTML. Le `WebViewClient` définira cette variable sur « True » lorsque tout le contenu a été chargé, afin que l’activité sache pour ajouter l’élément de menu Imprimer dans le menu d’options.

##### <a name="webviewclient"></a>WebViewClient

Le travail de le `WebViewClient` consiste à vérifier les données dans le `WebView` est complètement chargé avant que l’option d’impression s’affiche dans le menu, avec le `OnPageFinished` (méthode). `OnPageFinished` attend la fin du chargement du contenu web et indique l’activité de recréer son menu options avec `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` définit également la `dataLoaded` valeur `true`, de sorte que `OnCreateOptionsMenu` pouvez recréer le menu avec l’option d’impression en place.

##### <a name="printmanager"></a>PrintManager

L’exemple de code suivant imprime le contenu d’un `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` prend comme arguments : un nom pour le travail d’impression (« MyWebPage » dans cet exemple), un [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) qui génère le document d’impression à partir du contenu, et [ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null` dans le exemple ci-dessus). Vous pouvez spécifier `PrintAttributes` afin de disposer de contenu sur la page imprimée, bien que les attributs par défaut doivent gérer la plupart des scénarios.

Appel de `Print` se charge de l’interface utilisateur d’impression, qui répertorie les options pour le travail d’impression. L’interface utilisateur permet aux utilisateurs de l’impression ou de l’enregistrement du contenu HTML à un document PDF, comme illustré dans les captures d’écran ci-dessous :

[![Capture d’écran de PrintHtmlActivity afficher le menu Imprimer](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Capture d’écran de PrintHtmlActivity affichage de l’enregistrer en tant que menu PDF](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>Matériel

KitKat ajoute plusieurs API pour prendre en compte les nouvelles fonctionnalités de l’appareil. Le plus notable de ceux-ci sont émulation de la carte sur l’hôte et le nouveau `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Émulation de carte hôte dans NFC

Émulation de carte sur l’hôte (HCE) permet aux applications se comportent comme des cartes de NFC ou lecteurs de carte NFC sans se baser sur l’élément de sécuriser les propriétaires du transporteur. Avant de configurer HCE, vérifiez HCE est disponible sur l’appareil avec `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE requiert que les deux la fonctionnalité HCE et `Nfc` autorisation être inscrite auprès de l’application `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Définition de l’autorisation de NFC dans les options de l’application](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Pour fonctionner, HCE doit être en mesure d’exécuter en arrière-plan, et il doit démarrer lorsque l’utilisateur effectue une transaction NFC, même si l’application à l’aide de HCE n’est pas en cours d’exécution. Pour ce faire, nous pouvons écrire le code HCE comme un `Service`. Un Service HCE implémente la `HostApduService` interface, qui implémente les méthodes suivantes :

-  *ProcessCommandApdu* -un données unité APDU (Application Protocol) est envoyé entre le lecteur NFC et le Service HCE. Cette méthode consomme un ADPU à partir du lecteur et retourne une unité de données dans la réponse.

-  *OnDeactivated* - le `HostAdpuService` est désactivé lorsque le Service HCE est ne communiquent plus avec le lecteur NFC.


Un Service HCE doit également être inscrit avec le manifeste de l’application et décorée avec les autorisations appropriées, filtre intention et métadonnées. Le code suivant est un exemple d’un `HostApduService` inscrit avec le manifeste Android à l’aide de la `Service` attribut (pour plus d’informations sur les attributs, reportez-vous à la Xamarin [fonctionne avec le manifeste Android](~/android/platform/android-manifest.md) guide) :

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Le Service ci-dessus fournit un moyen pour le lecteur NFC interagir avec l’application, mais le lecteur NFC n’a encore aucun moyen de savoir si ce Service émule la carte NFC qu’il doit analyser. Pour identifier le Service au lecteur NFC, nous pouvons affecter le Service unique *ID de l’Application (aide)*. Nous spécifier une aide, ainsi que d’autres métadonnées sur le Service HCE, dans un fichier de ressources xml enregistré avec le `MetaData` attribut (voir l’exemple de code ci-dessus). Ce fichier de ressource spécifie un ou plusieurs filtres aide - chaînes d’identificateur unique au format hexadécimal qui correspondent aux outils d’un ou plusieurs lecteurs NFC :

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

En plus des filtres d’aide, le fichier de ressources xml fournit également une description de la direction de l’utilisateur du Service HCE, spécifie un groupe d’aide (application de paiement par rapport à « autre ») et, dans le cas d’une demande de paiement, une bannière de point de distribution 260 x 96 à afficher à l’utilisateur.

Le programme d’installation présentée ci-dessus fournit les blocs de construction de base pour une application émule une carte NFC. NFC lui-même nécessite plusieurs étapes supplémentaires et autres tests pour configurer. Pour plus d’informations sur l’émulation de la carte sur l’hôte, reportez-vous à la [portail de documentation Android](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Pour plus d’informations sur l’utilisation de NFC avec Xamarin, passez en revue les [exemples de Xamarin NFC](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Capteurs

KitKat fournit l’accès aux capteurs du périphérique via un [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/).
Le `SensorManager` permet le système d’exploitation planifier la remise des informations de capteur à une application par lots, en conservant la durée des batteries.

KitKat est également fourni avec deux nouveaux types de capteur pour le suivi des étapes de l’utilisateur. Elles sont basées sur l’accéléromètre et incluent :

-  *StepDetector* -application est notifiée/quitte le mode veille lorsque l’utilisateur effectue une étape et le détecteur fournit une valeur d’heure pour laquelle l’étape s’est produite.

-  *StepCounter* -conserve une trace du nombre d’étapes de l’utilisateur dans la mesure où le capteur a été inscrit *jusqu’au prochain redémarrage du périphérique*.

La capture d’écran ci-dessous illustre le compteur de l’étape en action :

[![Capture d’écran de l’application de SensorsActivity afficher un compteur de l’étape](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Vous pouvez créer un `SensorManager` en appelant `GetSystemService(SensorService)` et de la conversion du résultat qu’un `SensorManager`. Pour utiliser le compteur de l’étape, appelez `GetDeafultSensor` sur la `SensorManager`. Vous pouvez inscrire le capteur et écouter les modifications dans le nombre d’étape à l’aide de la [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/) de l’interface, comme illustré dans l’exemple de code ci-dessous :

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` est appelée si le nombre d’étape met à jour lorsque l’application est au premier plan. Si l’application passe à l’arrière-plan ou de l’appareil est en veille, `OnSensorChanged` ne sera pas appelée ; Toutefois, les étapes continueront à compter jusqu'à ce que `UnregisterListener` est appelée.

N’oubliez pas que *la valeur du nombre étape est cumulable pour toutes les applications qui inscrivent le capteur*. Cela signifie que même si vous désinstallez et réinstallez votre application et initialisez le `count` variable à 0 au démarrage de l’application, la valeur indiquée par le capteur restera le nombre total d’étapes effectuée alors que le capteur a été enregistré, que ce soit par votre application ou un autre. Vous pouvez empêcher votre application à partir de l’ajout au compteur étape en appelant `UnregisterListener` sur la `SensorManager`, comme illustré par le code ci-dessous :

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Le redémarrage de l’appareil réinitialise le nombre de l’étape 0. Votre application nécessite un code supplémentaire pour vous assurer qu’il signale un décompte précis pour l’application, indépendamment des autres applications utilisant le capteur ou l’état de l’appareil.


> [!NOTE]
> Lors de l’API pour la détection de l’étape et en prenant en compte est fourni avec KitKat, le capteur validation pas tous les téléphones. Vous pouvez vérifier si le capteur est disponible en exécutant `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, ou vérifiez que la valeur retournée de `GetDefaultSensor` n’est pas `null`.


<a name="developer_tools" />

## <a name="developer-tools"></a>Outils de développement

### <a name="screen-recording"></a>Enregistrement à l’écran

KitKat inclut un nouvel écran d’enregistrement afin que les développeurs puissent enregistrer des applications en action. Enregistrement à l’écran est disponible via le [Android Debug Bridge ADB](http://developer.android.com/tools/help/adb.html) client, ce qui peut être téléchargé en tant que partie du SDK Android.

Pour enregistrer votre écran, connectez votre appareil ; Ensuite, recherchez votre installation du SDK Android, accédez à la **outils de plateforme** répertoire et exécutez le **adb** client :

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

La commande ci-dessus enregistre une vidéo de 3 minutes par défaut à la résolution par défaut de 4 Mb/s. Pour modifier la longueur, ajoutez le *--délai* indicateur.
Pour modifier la résolution, ajouter le *--débit* indicateur. La commande suivante enregistre une vidéo de longue durée de la minute à 8 :

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Vous pouvez trouver votre vidéo sur votre appareil, il apparaît dans votre galerie lors de l’enregistrement est terminé.


## <a name="other-kitkat-additions"></a>Autres ajouts effectués KitKat

Outre les modifications décrites ci-dessus, KitKat vous permet de :

-  *Utilisez le mode plein écran* -KitKat introduit un nouveau [mode immersif](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) pour parcourir le contenu, jeux et exécutant d’autres applications qui peuvent tirer parti d’une expérience de plein écran.

-  *Personnaliser les Notifications* -obtenir des informations supplémentaires concernant les notifications système avec le [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) . Cela vous permet de présenter les informations d’une manière différente à l’intérieur de votre application.

-  *Mise en miroir de ressources Drawable* -ressources Drawable ont un nouveau [ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored) attribut qui indique au système de créer une version mise en miroir pour les images nécessitant une rotation pour les dispositions de gauche à droite.

-  *Suspendre des Animations* -suspendre et reprendre des animations créées avec le [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/) classe.

-  *Dynamiquement modification de texte en lecture* -désigner les parties de l’interface utilisateur qui mettent à jour dynamiquement par un nouveau texte en tant que « région dynamique » avec le nouveau [ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) d’attribut pour le nouveau texte sera lue automatiquement en mode d’accessibilité.

-  *Améliorer l’expérience Audio* -Vérifiez effectue le suivi plus fort avec le [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) , rechercher les heures de pointe et RMS d’un flux audio avec le [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/) classe et obtenir des informations à partir d’un [timestamp audio](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) pour aider à synchronisation audio-vidéo.

-  *Synchronisation ContentResolver personnalisé intervalles* -KitKat ajoute une certaine variabilité à l’heure à laquelle une demande de synchronisation est effectuée. Synchronisation d’un `ContentResolver` à heure personnalisé ou intervalle en appelant `ContentResolver.RequestSync` et en passant un `SyncRequest`.

-  *Distinguer entre les contrôleurs* -KitKat dans, les contrôleurs sont affectés des identificateurs entier unique qui est accessible par le biais de l’appareil `ControllerNumber` propriété. Cela simplifie l’indiquent les uns des autres lecteurs dans un jeu.

-  *Contrôle à distance* -avec peu de modifications sur le matériel et logiciel côté KitKat vous permet d’activer un périphérique équipé d’un émetteur infrarouge dans un contrôle à distance à l’aide de la `ConsumerIrService`et d’interagir avec les périphériques avec la nouvelle [ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) API.

Pour plus d’informations sur les modifications de l’API ci-dessus, reportez-vous à la Google [Android 4.4 API](http://developer.android.com/about/versions/android-4.4.html) vue d’ensemble.


## <a name="summary"></a>Récapitulatif

Cet article a introduit des nouvelles API disponibles dans Android 4.4 (API niveau 19) et couvert les meilleures pratiques lors de la transition d’une application à KitKat. Informatique de rencontrer des modifications indiquées pour les API affectant utilisateur, y compris le *framework de transition* et les nouvelles options de *thèmes*. Ensuite, il a introduit la *Framework de l’accès au stockage* et `DocumentsProvider` classe, ainsi que la nouvelle *impression API*. Il exploré *l’émulation de carte hôte NFC* et comment travailler avec *capteurs de faible puissance*, y compris deux nouveaux capteurs pour le suivi des étapes de l’utilisateur. Enfin, il démontré capture démonstrations en temps réel des applications avec *enregistrement à l’écran*et une liste détaillée des modifications de l’API de KitKat et d’ajouts.


## <a name="related-links"></a>Liens associés

- [KitKat Sample](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 API](http://developer.android.com/about/versions/android-4.4.html)
- [KitKat Android](http://developer.android.com/about/versions/kitkat.html)
