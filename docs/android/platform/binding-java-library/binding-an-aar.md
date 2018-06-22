---
title: Liaison d’un. AAR
description: Cette procédure pas à pas fournit des instructions détaillées pour la création d’une bibliothèque de liaisons Xamarin.Android Java à partir d’un Android. Fichier de AAR.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 54708a7cfd071f77968991c9fe4e52938697c9bb
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798705"
---
# <a name="binding-an-aar"></a>Liaison d’un. AAR

_Cette procédure pas à pas fournit des instructions détaillées pour la création d’une bibliothèque de liaisons Xamarin.Android Java à partir d’un Android. Fichier de AAR._


## <a name="overview"></a>Vue d'ensemble

Le *Archive Android (. AAR)* fichier est le format de fichier pour les bibliothèques Android.
Une barre d’outils. Les fichiers AAR sont un. ZIP qui contient les éléments suivants :

-   Compilé en code Java
-   ID de ressource
-   Ressources
-   Métadonnées (par exemple, les déclarations d’activité, les autorisations)

Dans ce guide, nous allons parcourir les principes fondamentaux de la création d’une bibliothèque de liaisons pour un seul. Fichier de AAR. Pour une vue d’ensemble de Java bibliothèque de liaison en général (avec un exemple de code de base), consultez [liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Un projet de liaison peut inclure uniquement un. Fichier de AAR. Si le. Dépendances AAR sur d’autres. AAR, puis ces dépendances doivent être contenus dans leur propre projet de liaison et ensuite référencé. Consultez [bogue 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>Procédure pas à pas

Nous allons créer une bibliothèque de liaisons pour un exemple de fichier d’archive Android qui a été créé dans Android Studio, [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). Cela. AAR contient un `TextCounter` classe avec des méthodes statiques qui compte le nombre de voyelles et les consonnes dans une chaîne. En outre, **textanalyzer.aar** contient une ressource d’image pour aider à afficher les résultats d’inventaire.

Nous allons utiliser les étapes suivantes pour créer une bibliothèque de liaisons à partir de la. Fichier de AAR :

1. Créer un nouveau projet de bibliothèque de liaisons de Java.

2. Ajouter un seul. Fichier AAR au projet. Un projet de liaison peut uniquement contenir une seule. AAR.

3. Action de génération approprié pour le. Fichier de AAR.

4. Choisissez une infrastructure cible qui le. AAR prend en charge.

5. Générez la bibliothèque de liaisons.

Une fois que nous avons créé la bibliothèque de liaisons, nous allons développer une petite application Android qui invite l’utilisateur pour une chaîne de texte, les appels. Méthodes AAR pour analyser le texte, extrait l’image à partir de la. AAR et affiche les résultats en même temps que l’image.

L’exemple d’application accédera à la `TextCounter` classe de **textanalyzer.aar**:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

En outre, cet exemple d’application récupère et affiche une ressource d’image qui est empaquetée dans **textanalyzer.aar**:

[![Image de singe Xamarin](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Cette ressource d’image se situe à **res/drawable/monkey.png** dans **textanalyzer.aar**.



### <a name="creating-the-bindings-library"></a>Création de la bibliothèque de liaisons

Avant de commencer les étapes ci-dessous, téléchargez l’exemple [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) fichier d’archive Android :

1.  Créer un nouveau projet de bibliothèque de liaisons, en commençant par le modèle de bibliothèque de liaisons Android. Vous pouvez utiliser Visual Studio pour Mac ou Visual Studio (les captures d’écran ci-dessous montrent Visual Studio, mais Visual Studio pour Mac est très similaire). Nommez la solution **AarBinding**:

    [![Créer le projet de AarBindings](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  Le modèle inclut un **fichiers JAR** dossier dans lequel vous ajoutez votre. AAR(s) pour le projet de bibliothèque de liaisons. Avec le bouton droit le **fichiers JAR** et sélectionnez **Ajouter > élément existant**:

    [![Ajouter un élément existant](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  Accédez à la **textanalyzer.aar** fichier téléchargé précédemment, sélectionnez-le, puis cliquez sur **ajouter**:

    [![Ajouter textanalayzer.aar](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  Vérifiez que le **textanalyzer.aar** fichier a été ajouté au projet :

    [![Le fichier textanalyzer.aar a été ajouté.](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  Action de génération pour **textanalyzer.aar** à `LibraryProjectZip`. Dans Visual Studio pour Mac, cliquez sur **textanalyzer.aar** pour définir l’Action de génération. Dans Visual Studio, l’Action de génération peut être définie dans le **propriétés** volet) :

    [![Définition de l’action de génération textanalyzer.aar à LibraryProjectZip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  Ouvrez le projet de propriétés pour configurer le *Framework cible*. Si le. AAR utilise des API Android, définir le Framework cible pour le niveau de l’API qui le. AAR attend. (Pour plus d’informations sur le paramètre Framework cible et les niveaux d’API Android en général, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md).)

    Définir la niveau de l’API de la cible de votre bibliothèque de liaisons. Dans cet exemple, nous sont libres d’utiliser la dernière plateforme API niveau (API 23), car notre **textanalyzer** n’a pas de dépendance sur les API Android :

    [![Définition du niveau de la cible sur API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  Générez la bibliothèque de liaisons. Le projet de bibliothèque de liaisons doit se générer avec succès et produire une sortie. DLL à l’emplacement suivant : **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>À l’aide de la bibliothèque de liaisons

Pour cela utiliser. DLL dans votre application de Xamarin.Android, vous devez d’abord ajouter une référence à la bibliothèque de liaisons. Utilisez les étapes suivantes :

1.  Nous créons cette application dans la même Solution que la bibliothèque de liaisons pour simplifier cette procédure pas à pas. (L’application qui utilise la bibliothèque de liaisons peut également se trouver dans une autre Solution.) Créer une application Xamarin.Android : avec le bouton droit de la Solution et sélectionnez **ajouter un nouveau projet**. Nommez le nouveau projet **BindingTest**:

    [![Créer le projet BindingTest](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  Avec le bouton droit le **références** nœud de la **BindingTest** de projet et sélectionnez **ajouter une référence...** :

    [![Cliquez sur Ajouter une référence](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  Sélectionnez le **AarBinding** projet créé précédemment et cliquez sur **OK**:

    [![Archiver le projet de liaison AAR](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  Ouvrez le **références** nœud de la **BindingTest** projet pour vérifier que le **AarBinding** référence est présente :

    [![AarBinding est répertorié sous références](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Si vous souhaitez afficher le contenu du projet de bibliothèque de liaison, vous pouvez double-cliquer sur la référence pour l’ouvrir dans le **Explorateur d’objets**. Vous pouvez voir le contenu mappé de la `Com.Xamarin.Textcounter` espace de noms (mappé à partir de Java `com.xamarin.textanalyzezr` package) et vous pouvez afficher les membres de la `TextCounter` classe :

[![Affichage de l’Explorateur d’objets](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

La capture d’écran ci-dessus met en surbrillance les deux `TextAnalyzer` méthodes qui appelle l’exemple d’application : `NumConsonants` (qui encapsule le Java sous-jacente `numConsonants` méthode), et `NumVowels` (qui encapsule le Java sous-jacente `numVowels` méthode).



### <a name="accessing-aar-types"></a>L’accès à. Types de AAR

Après avoir ajouté une référence à votre application qui pointe vers la bibliothèque de liaison, vous pouvez accéder à des types Java dans le. AAR comme vous accéderiez à des types c# (grâce aux wrappers c#). Le code d’application c# peut appeler `TextAnalyzer` méthodes, comme illustré dans cet exemple :

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

Dans l’exemple ci-dessus, nous appelons des méthodes statiques la `TextCounter` classe. Toutefois, vous pouvez également instancier des classes et appeler les méthodes d’instance. Par exemple, si votre. AAR encapsule une classe appelée `Employee` dont la méthode d’instance `buildFullName`, vous pouvez instancier `MyClass` et l’utiliser comme indiqué ici :

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Les étapes suivantes ajoutent le code à l’application afin qu’il invite l’utilisateur pour le texte, utilise `TextCounter` pour analyser le texte, puis affiche les résultats.

Remplacez le **BindingTest** mise en page (**Main.axml**) avec le code XML suivant. Cette disposition a un `EditText` pour l’entrée de texte et deux boutons de l’initialisation des nombres VOYELLE et consonne :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Remplacez le contenu de **MainActivity.cs** avec le code suivant. Comme indiqué dans cet exemple, l’appel de gestionnaires d’événements bouton encapsulé `TextCounter` méthodes qui se trouvent dans le. Toasts AAR et utilisez pour afficher les résultats. Notez le `using` instruction pour l’espace de noms de la bibliothèque liée (dans ce cas, `Com.Xamarin.Textcounter`) :

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Compilez et exécutez le **BindingTest** projet. L’application démarre et présenter la capture d’écran de gauche (le `EditText` est initialisé avec du texte, mais vous pouvez cliquer dessus pour le modifier). Quand vous appuyez sur **nombre voyelles**, une notification toast affiche le nombre des voyelles comme indiqué sur la droite :

[![Captures d’écran de BindingTest en cours d’exécution](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Essayez en appuyant sur la **nombre CONSONNES** bouton. En outre, vous pouvez modifier la ligne de texte et appuyez sur ces boutons pour tester différentes VOYELLE et compte les consonnes.



### <a name="accessing-aar-resources"></a>L’accès à. Ressources AAR

L’outils des fusions de Xamarin le **R** les données à partir de la. AAR dans votre application **ressource** classe. Par conséquent, vous pouvez accéder. Ressources AAR à partir de la mise en page (et à partir de code-behind) dans la même façon que vous accéderiez à des ressources qui sont dans le **ressources** chemin d’accès de votre projet.

Pour accéder à une ressource d’image, vous utilisez la **Resource.Drawable** nom de l’image est compressé dans le. AAR. Par exemple, vous pouvez référencer **image.png** dans le. Les fichiers AAR à l’aide de `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Vous pouvez également accéder dispositions de ressource qui se trouvent dans le. AAR. Pour ce faire, vous utilisez la **Resource.Layout** nom de la mise en page empaqueté dans le. AAR. Par exemple :

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Le **textanalyzer.aar** exemple contient un fichier image qui se situe à **res/drawable/monkey.png**. Nous allons accéder à cette ressource de l’image et l’utiliser dans notre exemple d’application :

Modifier la **BindingTest** mise en page (**Main.axml**) et ajoutez une `ImageView` à la fin de la `LinearLayout` conteneur. Cela `ImageView` affiche l’image à **@drawable/monkey**; cette image sera chargée à partir de la section de ressources de **textanalyzer.aar**:

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Compilez et exécutez le **BindingTest** projet. L’application démarre et présenter la capture d’écran de gauche &ndash; quand vous appuyez sur **nombre CONSONNES**, les résultats s’affichent comme illustré sur la droite :

[![BindingTest affichage conformes décompte](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Félicitations ! Vous avez lié avec succès une bibliothèque Java. AAR !



## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons créé une bibliothèque de liaisons pour un. Les fichiers AAR, ajouté la bibliothèque de liaisons pour une application de test minimale et exécution de l’application pour vérifier que le code c# peut appeler du code Java résidant dans le. Fichier de AAR.
En outre, nous avons étendu l’application pour accéder à et afficher une ressource d’image qui réside dans le. Fichier de AAR.


## <a name="related-links"></a>Liens associés

- [Génération d’une bibliothèque de liaisons Java (vidéo)](https://university.xamarin.com/classes#10090)
- [Liaison d’un fichier .JAR](~/android/platform/binding-java-library/binding-a-jar.md)
- [Liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md)
- [AarBinding (exemple)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [Projet de 44573-un bogue ne peut pas lier plusieurs fichiers .aar](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
