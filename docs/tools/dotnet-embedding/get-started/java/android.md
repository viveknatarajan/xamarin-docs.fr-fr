---
title: Bien démarrer avec Android
description: Ce document décrit la prise en main à l’aide de l’incorporation de .NET avec Android. Il aborde l’installation de .NET incorporation, création d’un projet de bibliothèque Android, à l’aide de la sortie générée dans un projet Android Studio et d’autres considérations.
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: e60892edfcf73f3e7cada8923e16bcc1be2c203e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121384"
---
# <a name="getting-started-with-android"></a>Bien démarrer avec Android

Outre les exigences à partir de la [mise en route avec Java](~/tools/dotnet-embedding/get-started/java/index.md) guide, vous devez également :

* [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) ou version ultérieure
* [Android Studio 3.x](https://developer.android.com/studio/index.html) avec Java 1.8

Une vue d’ensemble, nous allons :

* Créer un C# projet de bibliothèque Android
* Installer l’incorporation de .NET via NuGet
* Exécutez l’incorporation de .NET sur l’assembly de bibliothèque Android
* Utiliser le fichier AAR généré dans un projet Java dans Android Studio

## <a name="create-an-android-library-project"></a>Créer un projet de bibliothèque Android

Ouvrez Visual Studio pour Windows ou Mac, créez un projet de bibliothèque de classes Android, nommez-le **hello de csharp**et enregistrez-le à **~/Projects/hello-from-csharp** ou **%USERPROFILE%\ Projects\hello de csharp**.

Ajoutez une nouvelle activité Android nommé **HelloActivity.cs**, suivie d’une disposition Android à **Resource/layout/hello.axml**.

Ajouter un nouveau `TextView` à votre disposition, puis remplacez le texte à quelque chose de plus agréable.

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

> [!NOTE]
> N’oubliez pas que le `[Register]` attribut. Pour plus d’informations, consultez [Limitations](#current-limitations-on-android).

Générez le projet. L’assembly résultant sera enregistré dans `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Installation d’incorporation de .NET à partir de NuGet

Suivez ces [instructions](~/tools/dotnet-embedding/get-started/install/install.md) pour installer et configurer .NET incorporation pour votre projet.

L’appel de commande, vous devez configurer ressemblera à ceci :

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Utiliser la sortie générée dans un projet Android Studio

1. Ouvrez Android Studio et créez un nouveau projet avec un **activité vide**.
2. Avec le bouton droit sur votre **application** module et choisissez **New > Module**.
3. Sélectionnez **importation. JAR /. Le Package AAR**.
4. Permet de localiser le navigateur directory **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** et cliquez sur **Terminer**.

![Importer AAR dans Android Studio](android-images/androidstudioimport.png)

Cette commande copie le fichier AAR dans un nouveau module nommé **hello_from_csharp**.

![Projet Android Studio](android-images/androidstudioproject.png)

Pour utiliser le nouveau module à partir de votre **application**, avec le bouton droit et choisissez **ouvrir les paramètres du Module**. Sur le **dépendances** onglet, ajoutez une nouvelle **Module dépendance** et choisissez **: hello_from_csharp**.

![Dépendances d’Android Studio](android-images/androidstudiodependencies.png)

Dans votre activité, ajoutez un nouveau `onResume` (méthode) et utilisez le code suivant pour lancer le C# activité :

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

### <a name="assembly-compression-important"></a>Compression de l’assembly (*IMPORTANT*)

Une modification supplémentaire est requise pour l’incorporation de .NET dans votre projet Android Studio.

Ouvrez votre application **build.gradle** fichier, puis ajoutez la modification suivante :

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin.Android charge actuellement les assemblys .NET directement à partir de l’APK, mais elle nécessite les assemblys ne soient ne pas compressés.

Si vous n’avez pas cette configuration, l’application sera bloque au lancement et imprimer quelque chose comme ceci dans la console :

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Exécuter l'application

Lors du lancement de votre application :

![Hello de C# exemple en cours d’exécution dans l’émulateur](android-images/hello-from-csharp-android.png)

Notez que s’est-il passé ici :

* Nous avons un C# (classe), `HelloActivity`, que les sous-classes Java
* Nous avons des fichiers de ressources Android
* Nous avons utilisé à partir de Java dans Android Studio

Pour cet exemple fonctionne, tous les éléments suivants sont configurés dans le fichier APK final :

* Xamarin.Android est configuré sur le démarrage de l’application
* Les assemblys .NET inclus dans   **/assemblys de ressources**
* **AndroidManifest.xml** modifications pour votre C# activités, etc.
* Des ressources Android à partir de bibliothèques .NET
* [Les Wrappers RCW Android](~/android/platform/java-integration/android-callable-wrappers.md) pour toute `Java.Lang.Object` sous-classe

Si vous recherchez une procédure pas à pas supplémentaires, consultez la vidéo suivante, qui illustre l’incorporation de Petzold [démonstration de la peinture avec les doigts](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) dans un projet Android Studio :

[![Embeddinator-4000 pour Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>À l’aide de Java 1.8

À compter de la rédaction de cet article, la meilleure option consiste à utiliser Android Studio 3.0 ([télécharger ici](https://developer.android.com/studio/index.html)).

Pour activer Java 1.8 dans votre module application **build.gradle** fichier :

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Vous pouvez également examinons une [projet de test Android Studio](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) pour plus d’informations. 

Si vous voulez utiliser stable de 2.3.x Android Studio, vous devrez activer la chaîne d’outils Jack déconseillée :

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

À ce stade, si vous créez une sous-classe `Java.Lang.Object`, Xamarin.Android génère le stub de Java (Android Callable Wrapper) au lieu d’incorporer de .NET. Pour cette raison, vous devez suivre les mêmes règles pour l’exportation C# à Java en tant que Xamarin.Android. Exemple :

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

* `[Register]` est requise pour mapper à un nom de package Java souhaité
* `[Export]` est requis pour rendre une méthode visible à Java

Nous pouvons utiliser `ViewSubclass` dans Java comme suit :

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

En savoir plus sur [intégration Java avec Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Plusieurs assemblys

Incorporation d’un assembly unique est simple. Toutefois, il est plus probable que vous aurez plusieurs C# assembly. Nombre de fois où vous aurez dépendances sur les packages NuGet telles que les bibliothèques de prise en charge Android ou les Services Google Play qui compliquer encore davantage les choses.

Cela entraîne un dilemme, étant donné que l’incorporation de .NET doit inclure les nombreux types de fichiers dans la dernière AAR telles que :

* Ressources Android
* Ressources Android
* Bibliothèques natives Android
* Code source java Android

Vous sûrement ne souhaitez pas inclure ces fichiers à partir de la bibliothèque de prise en charge Android ou Google Play Services dans votre AAR mais préférez utiliser la version officielle à partir de Google dans Android Studio.

Voici l’approche recommandée :

* Passer de l’incorporation de .NET n’importe quel assembly que vous possédez (avez pas la source) et à appeler à partir de Java
* Passer de l’incorporation de .NET tout assembly dont vous avez besoin d’Android, des bibliothèques natives ou des ressources à partir de
* Ajouter les dépendances Java comme Android prend en charge la bibliothèque ou Google Play Services dans Android Studio

Par conséquent, votre commande peut être :

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Vous devez exclure quoi que ce soit à partir de NuGet, sauf si vous découvrez qu’il contient des ressources Android, ressources, etc. que vous aurez besoin dans votre projet Android Studio. Vous pouvez également omettre les dépendances que vous n’avez pas besoin d’appeler à partir de Java et l’éditeur de liens _doit_ incluent les parties de votre bibliothèque qui sont nécessaires.

Pour ajouter toutes les dépendances Java nécessaires dans Android Studio, votre **build.gradle** fichier peut ressembler à :

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Informations supplémentaires

* [Rappels sur Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Recherche Android préliminaire](~/tools/dotnet-embedding/android/index.md)
* [Limitations de l’incorporation de .NET](~/tools/dotnet-embedding/limitations.md)
* [Contribution au projet open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Codes d’erreur et descriptions](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Liens connexes

- [Exemple de météo (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
