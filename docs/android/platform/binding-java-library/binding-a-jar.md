---
title: Liaison d’un. JAR
description: Cette procédure pas à pas fournit des instructions détaillées pour la création d’une bibliothèque de liaisons Xamarin.Android Java à partir d’un Android. Fichier JAR.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a6cb08f19aac46ffa089914e28c732660caa52b2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-a-jar"></a>Liaison d’un. JAR

_Cette procédure pas à pas fournit des instructions détaillées pour la création d’une bibliothèque de liaisons Xamarin.Android Java à partir d’un Android. Fichier JAR._

## <a name="overview"></a>Vue d'ensemble

La Communauté Android offre de nombreuses bibliothèques Java que vous pouvez utiliser dans votre application. Ces bibliothèques Java sont souvent empaquetées dans. Format de fichier JAR (Java Archive), mais vous pouvez empaqueter un. JAR dans un *bibliothèque de liaisons Java* afin que ses fonctionnalités sont disponibles pour les applications de Xamarin.Android. L’objectif de la bibliothèque de liaisons de Java consiste à rendre les API dans le. Fichier JAR disponibles pour le code c# par le biais des wrappers de code généré automatiquement.

Les outils Xamarin peuvent générer une bibliothèque de liaisons à partir d’une ou plusieurs entrées. Fichiers JAR. La bibliothèque de liaisons (. Assembly de la DLL) contient les éléments suivants : 

-   Le contenu d’origine. Ou les fichiers JAR.

-   Managé Callable Wrappers (MCW), qui sont c# types qui encapsulent les types Java correspondant dans le. Ou les fichiers JAR.

Le code généré MCW utilise JNI (Java Native Interface) pour transférer vos appels d’API à sous-jacent. Fichier JAR. Vous pouvez créer des bibliothèques de liaisons pour tous. Fichier JAR ciblée à l’origine pour être utilisé avec Android (Notez que les outils Xamarin ne prend pas en charge la liaison de bibliothèques Android Java). Vous pouvez également choisir de générer la bibliothèque de liaisons sans inclure le contenu de la. Fichier JAR afin que la DLL a une dépendance sur le. JAR lors de l’exécution.

Dans ce guide, nous allons parcourir les principes fondamentaux de la création d’une bibliothèque de liaisons pour un seul. Fichier JAR. Illustrons par un exemple où tout se passe droit &ndash; où aucune personnalisation ou le débogage de liaisons est requis. 
[Création de métadonnées à l’aide de liaisons](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) propose un exemple d’un scénario plus avancé, où le processus de liaison n’est pas entièrement automatique et une certaine quantité d’une intervention manuelle est nécessaire. Pour une vue d’ensemble de Java bibliothèque de liaison en général (avec un exemple de code de base), consultez [liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md). 

 
## <a name="walkthrough"></a>Procédure pas à pas

Dans la procédure suivante, nous allons créer une bibliothèque de liaisons pour [Picasso](http://square.github.io/picasso/), un Android populaires. JAR qui fournit l’image de chargement et la mise en cache. Nous allons utiliser les étapes suivantes pour lier **picasso-2.x.x.jar** pour créer un nouvel assembly .NET que nous pouvons utiliser dans un projet Xamarin.Android : 

1. Créer un nouveau projet de bibliothèque de liaisons de Java.

2. Ajouter le. Fichier JAR au projet.

3. Action de génération approprié pour le. Fichier JAR.

4. Choisissez une infrastructure cible qui le. JAR prend en charge.

5. Générez la bibliothèque de liaisons.

Une fois que nous avons créé la bibliothèque de liaisons, nous allons développer une petite application Android qui illustre notre capacité à appeler des API dans la bibliothèque de liaisons. Dans cet exemple, nous souhaitons accéder aux méthodes de **picasso-2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>Création de la bibliothèque de liaisons

Avant de commencer les étapes ci-dessous, téléchargez [picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

Commencez par créer un nouveau projet de bibliothèque de liaisons. Dans Visual Studio pour Mac ou Visual Studio, créez une Solution et sélectionnez le *bibliothèque de liaisons Android* modèle. (Les captures d’écran dans cette procédure pas à pas utilisent Visual Studio, mais Visual Studio pour Mac est très similaire). Nommez la Solution **JarBinding**: 

[![Créer le projet de bibliothèque JarBinding](binding-a-jar-images/01-new-bindings-library-sml.png)](binding-a-jar-images/01-new-bindings-library.png#lightbox)

Le modèle inclut un **fichiers JAR** dossier dans lequel vous ajoutez votre. JAR(s) pour le projet de bibliothèque de liaisons. Avec le bouton droit le **fichiers JAR** et sélectionnez **Ajouter > élément existant**: 

[![Ajouter un élément existant](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Accédez à la **picasso-2.x.x.jar** fichier téléchargé précédemment, sélectionnez-le, puis cliquez sur **ajouter**: 

[![Sélectionnez le fichier jar et cliquez sur Ajouter](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Vérifiez que le **picasso-2.x.x.jar** fichier a été ajouté au projet : 

[![JAR ajouté au projet](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Lorsque vous créez un projet de bibliothèque de liaisons de Java, vous devez spécifier si le. JAR doit être incorporé dans la bibliothèque de liaisons ou empaquetées séparément. Pour ce faire, vous spécifiez une de ces *des actions de génération*: 

-   **EmbeddedJar** &ndash; le. JAR est incorporé dans la bibliothèque de liaisons.

-   **InputJar** &ndash; le. JAR est conservé distinct à partir de la bibliothèque de liaisons.

En général, vous utilisez la **EmbeddedJar** action de génération pour que le. JAR est automatiquement fourni dans la bibliothèque de liaisons. Cette option est la plus simple &ndash; p-code Java dans le. JAR est converti en Dex bytecode et est incorporé dans votre APK (ainsi que les Wrappers gérés). Si vous souhaitez conserver le. JAR distinct à partir de la bibliothèque de liaisons, vous pouvez utiliser la **InputJar** option ; Toutefois, vous devez vous assurer que le. Fichier JAR est disponible sur le périphérique qui exécute votre application. 

Définir l’action de génération **EmbeddedJar**: 

[![Sélectionnez l’action de génération EmbeddedJar](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Ensuite, ouvrez le projet de propriétés pour configurer le *Framework cible*. Si le. JAR utilise des API Android, définir le Framework cible pour le niveau de l’API qui le. JAR attend. En règle générale, le développeur de le. Fichier JAR indique quel niveau d’API (ou niveaux) qui le. JAR est compatible avec. (Pour plus d’informations sur le paramètre Framework cible et les niveaux d’API Android en général, consultez [niveaux d’API Android compréhension](~/android/app-fundamentals/android-api-levels.md).)

Définir la cible API au niveau de votre bibliothèque de liaisons (dans cet exemple, nous utilisons le niveau API 19) : 

[![Niveau de l’API cible la valeur d’API 19](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


Enfin, générez la bibliothèque de liaisons. Bien que certains messages d’avertissement peuvent s’afficher, le projet de bibliothèque de liaisons doit se générer avec succès et produire une sortie. DLL à l’emplacement suivant : **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>À l’aide de la bibliothèque de liaisons

Pour cela utiliser. DLL dans votre application de Xamarin.Android, procédez comme suit :

1.  Ajoutez une référence à la bibliothèque de liaisons.

2.  Faire des appels dans le. JAR via les Wrappers managés. 

Dans les étapes suivantes, nous allons créer une application minimale qui utilise la bibliothèque de liaisons pour télécharger et afficher une image dans un `ImageView`; « occuper » est effectuée par le code qui se trouve dans le. Fichier JAR. 

Tout d’abord, créez une nouvelle application Xamarin.Android qui utilise la bibliothèque de liaisons. Avec le bouton droit de la Solution et sélectionnez **ajouter un nouveau projet**; nommez le nouveau projet **BindingTest**. Nous créons cette application dans la même Solution que la bibliothèque de liaisons afin de simplifier cette procédure pas à pas ; Toutefois, l’application qui utilise la bibliothèque de liaisons pourrait se trouver au lieu de cela, dans une autre Solution : 

[![Ajouter un nouveau projet BindingTest](binding-a-jar-images/07-add-new-project-sml.png)](binding-a-jar-images/07-add-new-project.png#lightbox)

Avec le bouton droit le **références** nœud de la **BindingTest** de projet et sélectionnez **ajouter une référence...** :

[![Ajouter une référence à droite](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Vérifiez le **JarBinding** projet créé précédemment et cliquez sur **OK**:

[![Sélectionnez le projet JarBinding](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Ouvrir le **références** nœud de la **BindingTest** de projet et vérifiez que le **JarBinding** référence est présente : 

[![JarBinding apparaît sous références](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Modifier la **BindingTest** mise en page (**Main.axml**) afin qu’il comporte un seul `ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

Ajoutez le code suivant `using` instruction **MainActivity.cs** &ndash; Cela permet d’accéder facilement aux méthodes de la Java `Picasso` classe qui se trouve dans la bibliothèque de liaisons :

```csharp
using Com.Squareup.Picasso;
```

Modifier la `OnCreate` méthode afin qu’elle utilise le `Picasso` classe pour charger une image à partir d’une URL et l’afficher dans le `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

Compilez et exécutez le **BindingTest** projet. L’application va démarrer, et après un court délai (en fonction des conditions de réseau), il doit télécharger et afficher une image semblable à la capture d’écran suivante :

[![Capture d’écran de BindingTest en cours d’exécution](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Félicitations ! Vous avez lié avec succès une bibliothèque Java. JAR et utilisée dans votre application Xamarin.Android.
 
 
## <a name="summary"></a>Récapitulatif

Dans cette procédure pas à pas, nous avons créé une bibliothèque de liaisons pour un tiers. Un fichier, JAR ajouté la bibliothèque de liaisons pour une application de test minimale, et puis exécutez l’application pour vérifier que le code c# peut appeler du code Java résidant dans le. Fichier JAR. 



## <a name="related-links"></a>Liens associés

- [Génération d’une bibliothèque de liaisons Java (vidéo)](https://university.xamarin.com/classes#10090)
- [Liaison d’une bibliothèque Java](~/android/platform/binding-java-library/index.md)
