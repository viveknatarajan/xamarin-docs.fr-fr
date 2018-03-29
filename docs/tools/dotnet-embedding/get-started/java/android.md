---
title: Mise en route avec Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 713f182e3139933484d3185cc0226bf6758c23c3
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2018
---
# <a name="getting-started-with-android"></a>Mise en route avec Android


Outre les exigences de nos [mise en route avec Java](~/tools/dotnet-embedding/get-started/java/index.md) guide, vous devez également :

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) ou version ultérieure
* [Android Studio 3.x](https://developer.android.com/studio/index.html) avec Java 1.8

Comme une vue d’ensemble, nous allons :

* Créer un projet de bibliothèque Android c#
* Installer .NET incorporation via NuGet
* Exécutez Embeddinator sur l’assembly de bibliothèque Android
* Utiliser le fichier AAR généré dans un projet Java dans Android Studio

## <a name="create-an-android-library-project"></a>Créez un projet de bibliothèque Android

Ouvrez Visual Studio pour Windows ou Mac, créez un projet de bibliothèque de classes Android, nommez-le `hello-from-csharp`et enregistrez-le `~/Projects/hello-from-csharp` ou `%USERPROFILE%\Projects\hello-from-csharp`.

Ajouter une nouvelle activité Android nommé `HelloActivity.cs`, suivi d’une disposition Android à `Resource/layout/hello.axml`.

Ajouter un nouveau `TextView` à votre disposition, puis remplacez le texte à un élément agréable.

Votre source de mise en page doit ressembler à ceci :
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

Dans votre activité, assurez-vous que vous appelez `SetContentView` avec votre nouvelle disposition :
```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```
*Remarque : n’oubliez pas le `[Register]` d’attribut, consultez les détails sous Limitations*

Générez le projet, l’assembly résultant sera enregistré dans `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>L’installation de .NET incorporation de NuGet

Choisissez **Ajouter > ajouter des Packages NuGet...**  et installer `Embeddinator-4000` à partir du Gestionnaire de package NuGet : ![Gestionnaire de Package NuGet](android-images/visualstudionuget.png)

Ceci installe `Embeddinator-4000.exe` dans le `packages/Embeddinator-4000/tools` active.

## <a name="run-embeddinator-4000"></a>Exécutez Embeddinator-4000.

Nous allons ajouter une étape post-build pour exécuter Embeddinator et créer un fichier AAR natif pour l’assembly de projet de bibliothèque Android.

Dans Visual Studio pour Mac, accédez à _Options du projet > Générer > commandes personnalisées_ et ajoutez un _après génération_ étape.

Le programme d’installation le commnd suivant :
```
mono '${SolutionDir}/packages/Embeddinator-4000.0.2.0.80/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

> [!NOTE]
> Remarque : veillez à utiliser le numéro de version que vous avez installé à partir de NuGet

Si vous souhaitez effectuer un développement continu sur le projet c#, vous pouvez également ajouter une commande personnalisée pour nettoyer le `output` répertoire avant d’exécuter Embeddinator :
```
rm -Rf '${SolutionDir}/output/'
```

![Action de génération personnalisée](android-images/visualstudiocustombuild.png)

Le fichier AAR Android figurera dans `~/Projects/hello-from-csharp/output/hello_from_csharp.aar`. _Remarque : des traits d’union sont remplacés, car Java ne pas en charge dans les noms de package._

### <a name="running-net-embedding-on-windows"></a>.NET en cours d’exécution, l’intégration sur Windows

Nous va installer essentiellement la même chose, mais les menus de Visual Studio sont légèrement différentes sur Windows. Les commandes d’environnement sont également légèrement différents.

Accédez à **Options du projet > événements de Build** et entrez le texte suivant dans le **ligne de commande d’événement post-build** zone :
```
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

Par exemple la capture d’écran suivante :

![Embeddinator sur Windows](android-images/visualstudiowindows.png)

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Utilisez la sortie générée dans un projet Android Studio

1. Ouvrez Android Studio et créez un nouveau projet avec un **activité vide**.
2. Avec le bouton droit sur votre `app` module et choisissez **Nouveau > Module**.
3. Sélectionnez **importation. JAR /. Package de AAR**.
4. Permet de localiser l’Explorateur de répertoires `~/Projects/hello-from-csharp/output/hello_from_csharp.aar` et cliquez sur **Terminer**.

![Importation AAR dans Android Studio](android-images/androidstudioimport.png)

Le fichier AAR est copié dans un nouveau module nommé `hello_from_csharp`.

![Projet Android Studio](android-images/androidstudioproject.png)

Pour utiliser le nouveau module à partir de votre `app`, avec le bouton droit et choisissez **ouvrir les paramètres du Module**. Sur le **dépendances** , ajoutez un nouvel onglet de **Module dépendance** et choisissez `:hello_from_csharp`.

![Dépendances de Android Studio](android-images/androidstudiodependencies.png)

Dans votre activité, ajoutez un nouveau `onResume` (méthode) et utilisez le code suivant pour lancer notre activité c# :

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Compression de l’assembly *IMPORTANT*

Une modification supplémentaire est requise pour l’incorporation de .NET dans votre projet Android Studio.

Ouvrez votre application `build.gradle` et ajoutez la modification suivante :
```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```
Xamarin.Android charge actuellement les assemblys .NET directement à partir de APK, mais elle nécessite des assemblys ne pas compressé.

Si vous n’avez pas de ce programme d’installation, l’application sera bloque au lancement et afficher quelque chose comme ceci sur la console :

```csharp
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Exécuter l'application

Après le lancement de votre application :

![Bonjour à partir de l’exemple de code c# en cours d’exécution dans l’émulateur](android-images/hello-from-csharp-android.png)

Notez que s’est-il passé ici :

* Nous disposons d’une classe c# `HelloActivity`, que les sous-classes Java
* Nous avons les fichiers de ressources Android
* Nous avons utilisé à partir de Java dans Android Studio

Par conséquent, pour cet exemple fonctionne, tous les éléments suivants sont le programme d’installation dans APK finale :

* Xamarin.Android est configuré sur le démarrage de l’application
* Inclus dans des assemblys .NET `assets/assemblies`
* `AndroidManifest.xml` apporter des modifications à vos activités c#, un etc.
* Android des ressources à partir de bibliothèques .NET
* [Les Wrappers RCW Android](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/android_callable_wrappers/) pour toute `Java.Lang.Object` sous-classe

Si vous recherchez une procédure pas à pas supplémentaires, consultez cette vidéo de l’incorporation de Petzold [démonstration de peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) dans un Studio Android projet ici :

[![Embeddinator-4000 pour Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>À l’aide de Java 1.8

À compter de la rédaction de cet article, la meilleure solution consiste à utiliser Android Studio 3.0 ([télécharger ici](https://developer.android.com/studio/index.html)).

Pour activer Java 1.8 dans votre module d’application `build.gradle` fichier :
```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
Vous pouvez également examiner notre projet de test Android Studio [ici](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) pour plus d’informations.

Si vous souhaitez utiliser stable de 2.3.x Android Studio, vous devez activer la chaîne d’outils prise déconseillée :
```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Limitations actuelles sur Android

Maintenant si vous créez une sous-classe `Java.Lang.Object`, Xamarin.Android génère le stub de Java (Android Callable Wrapper) au lieu de Embeddinator.

Par conséquent, vous devez suivre les mêmes règles pour l’exportation c# pour Java en tant que Xamarin.Android.

Par exemple en c# :
```csharp
    [Register("mono.embeddinator.android.ViewSubclass")]
    public class ViewSubclass : TextView
    {
        public ViewSubclass(Context context) : base(context) { }

        [Export("apply")]
        public void Apply(string text)
        {
            Text = text;
        }
    }
```

* `[Register]` est requis pour mapper à un nom de package de Java souhaité
* `[Export]` est nécessaire pour qu’une méthode visible pour Java

Nous pouvons utiliser `ViewSubclass` dans Java comme suit :
```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

En savoir plus sur [intégration Java avec Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Plusieurs assemblys

Incorporation d’un assembly unique est simple. Toutefois, il est beaucoup plus susceptible de qu'avoir plus de c# un assembly. Nombre de fois où vous aurez dépendances sur les packages NuGet telles que les bibliothèques Android prennent en charge les Services Google Play qui davantage compliquer les choses.

Cela provoque un dilemme, puisque Embeddinator doit inclure de nombreux types de fichiers dans la dernière AAR telles que :
* Ressources Android
* Ressources Android
* Bibliothèques natives Android
* Source java Android

Vous très probablement ne souhaitez pas inclure ces fichiers à partir de la bibliothèque de prise en charge Android ou les Services Google Play dans votre AAR, mais utilisez plutôt la version officielle de Google dans Android Studio.

Voici l’approche recommandée :
* Passer Embeddinator tous les assemblys que vous possédez (avoir source) et souhaitez appeler à partir de Java
* Passer Embeddinator tout assembly dont vous avez besoin d’Android, des bibliothèques natives ou des ressources à partir de
* Ajouter des dépendances de Java, telles que le Android prend en charge la bibliothèque ou les Services Google Play dans Android Studio

Par conséquent, votre commande peut être :
```
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```
Vous ne devez exclure quoi que ce soit à partir de NuGet, sauf si vous découvrez qu’il contient des ressources Android, les ressources, etc. dont vous aurez besoin dans votre projet Android Studio. Vous pouvez également omettre les dépendances que vous n’avez pas besoin d’appeler à partir de Java et de l’éditeur de liens _doit_ incluent les parties de votre bibliothèque qui sont nécessaires.

Pour ajouter des dépendances de Java nécessaires dans Android Studio, votre `build.gradle` fichier peut ressembler à :
```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>informations supplémentaires

* [Rappels sur Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Recherche Android préliminaire](~/tools/dotnet-embedding/android/index.md)
* [Limitations de Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Contribue au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)


## <a name="related-links"></a>Liens associés

- [Exemple Weather (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
