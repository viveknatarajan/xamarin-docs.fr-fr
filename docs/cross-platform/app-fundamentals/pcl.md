---
title: Introduction aux bibliothèques de classes portables (PCL)
description: Cet article présente les projets de bibliothèque de classes Portable (PCL) et montre comment créer et consommer les projets de bibliothèque de classes portable dans Visual Studio pour Mac et Visual Studio.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 221ee49e282b3b038d03f659d238336710283a66
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61229923"
---
# <a name="portable-class-libraries-pcl"></a>Bibliothèques de classes portables (PCL)

> [!TIP]
> Bibliothèques de classes portables (PCL) sont considérées comme déconseillées dans les dernières versions de Visual Studio.
> Bien que vous pouvez toujours ouvrir, modifier et compiler des bibliothèques de classes portables, pour les nouveaux projets, il est recommandé d’utiliser [bibliothèques .NET Standard](~/cross-platform/app-fundamentals/net-standard.md) pour accéder à une plus grande surface d’API.

Un composant clé de la création d’applications multiplateformes est la possibilité de partager du code entre divers projets propres à la plateforme. Toutefois, ceci est compliqué par le fait que les différentes plateformes utilisent souvent un ensemble différent de sous-élément de la bibliothèque de classes de Base (BCL) .NET et par conséquent sont en fait, générés à un profil de bibliothèque .NET Core différent. Cela signifie que chaque plateforme peut uniquement utiliser des bibliothèques de classes qui sont ciblés sur le même profil afin qu’ils apparaîtraient pour exiger des projets bibliothèque de classes distinct pour chaque plateforme.

Il existe trois principales approches de partage de code qui résolvent ce problème : **projets .NET Standard**, **les projets d’actifs partagés**, et **projets de bibliothèque de classes Portable (PCL)**.

- **Projets .NET standard** sont la meilleure approche pour partager du code .NET, en savoir plus sur [projets .NET Standard et Xamarin](~/cross-platform/app-fundamentals/net-standard.md).
- **Projets d’actifs partagés** utiliser un seul ensemble de fichiers et offre un moyen simple et rapide permettant de partager du code et plus généralement, dans une solution emploie des directives de compilation conditionnelle pour spécifier des chemins de code pour diverses plateformes qui va l’utiliser (pour plus d’informations d’informations, consultez le [article de projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)).
- **Bibliothèque de classes portable** projets ciblent des profils spécifiques qui prennent en charge un ensemble connu de classes/fonctionnalités de la BCL. Toutefois, à la bibliothèque de classes portable l’inconvénient est qu’elles nécessitent souvent d’effort supplémentaire architectural pour séparer le code spécifique de profil dans leurs propres bibliothèques.

Cette page explique comment créer un **PCL** projet qui cible un profil spécifique, ce qui peut ensuite être référencé par plusieurs projets spécifiques à la plateforme.

## <a name="what-is-a-portable-class-library"></a>Qu’est une bibliothèque de classes Portable ?

Lorsque vous créez un projet d’Application ou un projet de bibliothèque, la DLL résultante est limitée à travailler sur la plateforme spécifique, pour qu'il est créé. Cela vous empêche d’écrire un assembly pour une application Windows, puis l’utilisation de nouveau sur Xamarin.iOS et Xamarin.Android.

Toutefois, lorsque vous créez une bibliothèque de classes Portable, vous pouvez choisir une combinaison des plateformes que vous souhaitez que votre code s’exécutent sur. Les choix de compatibilité que vous effectuez lors de la création d’une bibliothèque de classes Portable sont traduites en un identificateur « Profil », qui décrit quelles plateformes prend en charge de la bibliothèque.

Le tableau ci-dessous présente certaines des fonctionnalités qui varient selon la plateforme .NET. Pour écrire un assembly de bibliothèque de classes portable qui est peut s’exécuter sur des plateformes/appareils spécifiques, vous simplement choisir la prise en charge est requis lorsque vous créez le projet.

|Fonctionnalité|.NET Framework|Applications UWP|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Principal|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7.5 +|Y|
|Sérialisation|Y|Y|Y|Y|Y|
|Annotations de données|4.0.3 +|Y|Y||Y|

La colonne Xamarin reflète le fait que Xamarin.iOS et Xamarin.Android prend en charge tous les profils fournis avec Visual Studio, et la disponibilité des fonctionnalités dans les bibliothèques que vous créez est uniquement limitée par d’autres plateformes que vous choisissez pour prendre en charge.

Cela inclut les profils qui sont des combinaisons de :

- .NET 4 ou .NET 4.5
- Silverlight 5
- Windows Phone 8
- Applications UWP

Vous trouverez plus d’informations sur les fonctionnalités des différents profils sur [du site Web Microsoft](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) et voir un autre membre de la Communauté [profil PCL Résumé](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) qui inclut pris en charge les informations de framework et d’autres remarques.

**Avantages**

1. Partage de code centralisés – écrire et tester le code dans un projet unique qui peut être consommé par d’autres bibliothèques ou les applications.
2. Opérations de refactorisation affecte tout le code chargé dans la solution (la bibliothèque de classes portables et les projets spécifiques à la plateforme).
3. Le projet de bibliothèque de classes portable peut être facilement référencé par d’autres projets dans une solution ou l’assembly de sortie peut être partagé pour d’autres personnes à référencer dans leurs solutions.

**Inconvénients**

1. Étant donné que la même bibliothèque de classes Portable est partagée entre plusieurs applications, les bibliothèques spécifiques à la plateforme ne peut être référencés (par exemple). Community.CsharpSqlite.WP7).
2. Le sous-ensemble de la bibliothèque de classes Portable peut ne pas inclure les classes qui seraient autrement disponibles dans MonoTouch et Mono pour Android (par exemple, DllImport ou System.IO.File).

> [!NOTE]
> Bibliothèques de classes portables ont été déconseillées dans la dernière version de Visual Studio, et [bibliothèques .NET Standard](net-standard.md) sont recommandées à la place.

Dans une certaine mesure, les deux inconvénients peuvent être contournées à l’aide du modèle de fournisseur ou l’Injection de dépendances de coder l’implémentation réelle dans les projets de plateforme par rapport à une classe interface ou de base qui est défini dans la bibliothèque de classes Portable.

Ce diagramme illustre l’architecture d’une application multiplateforme à l’aide d’une bibliothèque de classes portables pour partager du code, mais également à l’aide de l’Injection de dépendances à passer dans les fonctionnalités dépend de la plateforme :

[![](pcl-images/image1.png "Ce diagramme illustre l’architecture d’une application multiplateforme à l’aide d’une bibliothèque de classes portables pour partager du code, mais également à l’aide de l’Injection de dépendances à passer dans les fonctionnalités dépend de la plateforme")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio pour Mac procédure pas à pas

Cette section décrit comment créer et utiliser une bibliothèque de classes Portable à l’aide de Visual Studio pour Mac. Consultez la section exemple de bibliothèque de classes portable pour une implémentation complète.

### <a name="creating-a-pcl"></a>Création d’une bibliothèque de classes portable

Ajout d’une bibliothèque de classes Portable à votre solution est très similaire à l’ajout d’un projet de bibliothèque standard.

1. Dans le **nouveau projet** boîte de dialogue Sélectionnez le **multiplateforme > bibliothèque > bibliothèque Portable** option :

    ![Créer un nouveau projet de bibliothèque de classes portable](pcl-images/image2.png)

1. Création d’une bibliothèque de classes portable dans Visual Studio pour Mac, il est automatiquement configuré avec un profil qui fonctionne pour Xamarin.iOS et Xamarin.Android. Le projet de bibliothèque de classes portable s’affiche comme indiqué dans cette capture d’écran :

    ![Projet de bibliothèque de classes portable dans le panneau solution](pcl-images/image3.png)

La bibliothèque PCL est maintenant prête pour le code à ajouter. Il peut également être référencé par d’autres projets (projets d’Application, les projets de bibliothèque et même d’autres projets de bibliothèque de classes portable).

### <a name="editing-pcl-settings"></a>Modification des paramètres de la bibliothèque de classes portable

Pour afficher et modifier les paramètres de la bibliothèque de classes portable pour ce projet, cliquez sur le projet et choisissez **Options > Build > Général** pour voir l’écran illustré ici :

[![Options de projet de bibliothèque de classes portable pour définir le profil](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

Cliquez sur **modification...**  pour modifier le profil cible pour cette bibliothèque de classes portable.

Si le profil est modifié une fois que le code a déjà été ajouté à la bibliothèque de classes portable, il est possible que la bibliothèque n’est plus compilées si le code référence des fonctionnalités qui ne font pas partie du profil qui vient d’être sélectionné.

## <a name="working-with-a-pcl"></a>Travaillez sur une bibliothèque de classes portable

Lorsque le code est écrit dans une bibliothèque PCL, l’éditeur Visual Studio pour Mac reconnaîtra les limitations du profil sélectionné et ajuster en conséquence les options de saisie semi-automatique. Par exemple, cette capture d’écran montre les options de saisie semi-automatique de System.IO en utilisant le profil par défaut (Profile136) utilisé dans Visual Studio pour Mac : Notez que la barre de défilement indique environ la moitié des classes disponibles sont affichée (en fait il existe 14 uniquement classes disponibles).

[![Liste IntelliSense des 14 classes dans la classe System.IO d’une bibliothèque de classes portable](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Comparer que, avec la saisie semi-automatique dans un projet Xamarin.iOS ou Xamarin.Android – de System.IO, il existe des 40 classes disponibles, notamment couramment utilisé des classes comme `File` et `Directory` qui ne sont pas dans n’importe quel profil PCL.

[![Liste IntelliSense des 40 classes dans l’espace de noms System.IO de .NET Framework](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

Cela reflète le compromis sous-jacent d’à l’aide de la bibliothèque de classes portable : la possibilité de partager du code en toute transparence dans de nombreuses plates-formes signifie que certaines API n’est pas disponibles car ils n’ont des implémentations comparables sur toutes les plateformes possible.

### <a name="using-pcl"></a>À l’aide de la bibliothèque de classes portable

Une fois un projet de bibliothèque de classes portable a été créé, vous pouvez ajouter une référence à celui-ci à partir de n’importe quel projet d’Application ou une bibliothèque compatible de la même façon que vous ajoutez généralement des références. Dans Visual Studio pour Mac, avec le bouton droit sur le nœud Références et choisissez **modifier les références...**  puis basculez vers le **projets** onglet comme indiqué :

[![Ajoutez une référence à une bibliothèque de classes portable via l’option Modifier les références](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

La capture d’écran suivante montre le panneau Solution pour l’exemple d’application TaskyPortable, affichant la bibliothèque PCL en bas et une référence à cette bibliothèque PCL dans le projet Xamarin.iOS.

[![Projet de bibliothèque de classes portable TaskyPortable exemple solution montrant](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

La sortie à partir d’une bibliothèque de classes portable (ie. l’assembly résultant DLL) peut également être ajouté comme une référence à la plupart des projets. Bibliothèque de classes portable ainsi un moyen idéal de livrer des bibliothèques et les composants inter-plateformes.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Procédure pas à pas de Visual Studio

Cette section décrit comment créer et utiliser une bibliothèque de classes Portable à l’aide de Visual Studio. Consultez la section exemple de bibliothèque de classes portable pour une implémentation complète.

### <a name="creating-a-pcl"></a>Création d’une bibliothèque de classes portable

Ajout d’une bibliothèque de classes portable à votre solution dans Visual Studio est légèrement différent à l’ajout d’un projet standard :

1. Dans le **ajouter un nouveau projet** s’affiche, sélectionnez le **bibliothèque de classes (Portable héritée)** option. Notez que la description de droite indique que ce type de projet a été déconseillé.

    [![Nouvelle fenêtre de projet pour créer une bibliothèque de classes portable](pcl-images/image10-sml.png "bibliothèque de classes Portable")](pcl-images/image10.png#lightbox)

2. Visual Studio vous invite immédiatement avec la boîte de dialogue suivante afin que le profil peut être configuré.
 Cochez les plateformes que vous devez prendre en charge et appuyez sur OK.

    [![Sélectionnez les plateformes cibles pour la bibliothèque](pcl-images/image11-sml.png "Cochez les plateformes que vous devez prendre en charge et appuyez sur OK")](pcl-images/image11.png#lightbox)

3. Le projet de bibliothèque de classes portable s’affiche comme indiqué dans l’Explorateur de solutions &ndash; le texte **(Portable)** s’affiche en regard du nom de projet pour indiquer qu’il est une bibliothèque de classes portable :

    ![NET Framework définies par le profil de bibliothèque de classes portable](pcl-images/image12.png "NET Framework définies par le profil de bibliothèque de classes portable")

La bibliothèque PCL est maintenant prête pour le code à ajouter. Il peut également être référencé par d’autres projets (projets d’application, les projets de bibliothèque et même d’autres projets de bibliothèque de classes portable).

### <a name="editing-pcl-settings"></a>Modification des paramètres de la bibliothèque de classes portable

Les paramètres de la bibliothèque de classes portable peuvent être affichées et modifiées en effectuant un clic droit sur le projet et en choisissant **Propriétés > bibliothèque** , comme illustré dans cette capture d’écran :

[![Modifier les plateformes cibles](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

Si le profil est modifié une fois que le code a déjà été ajouté à la bibliothèque de classes portable, il est possible que la bibliothèque n’est plus compilées si le code référence des fonctionnalités qui ne font pas partie du profil qui vient d’être sélectionné.

> [!TIP]
> Il existe également un message indiquant que **. NETStandard est la méthode recommandée pour partager du code**. Il s’agit d’une indication qui lors de bibliothèques de classes portables sont toujours pris en charge, il est recommandé de mettre à niveau vers .NET Standard.

### <a name="working-with-a-pcl"></a>Travaillez sur une bibliothèque de classes portable

Lorsque le code est écrit dans une bibliothèque PCL, Visual Studio reconnaît les limitations du profil sélectionné et ajuster en conséquence les options Intellisense. Par exemple, cette capture d’écran montre les options de saisie semi-automatique de System.IO en utilisant le profil par défaut (Profile136) : Notez que la barre de défilement indique environ la moitié des classes disponibles sont affichée (en fait il existe uniquement des 14 classes disponible).

[![Réduit le nombre de classes d’e/s disponibles dans une bibliothèque de classes portable](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

Comparer que, avec la saisie semi-automatique dans un projet standard – de System.IO, il existe des 40 classes disponibles, notamment couramment utilisé des classes comme `File` et `Directory` qui ne sont pas dans n’importe quel profil PCL.

[![Plusieurs classes d’e/s plus disponibles dans le .NET Framework](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

Cela reflète le compromis sous-jacent d’à l’aide de la bibliothèque de classes portable : la possibilité de partager du code en toute transparence dans de nombreuses plates-formes signifie que certaines API n’est pas disponibles car ils n’ont des implémentations comparables sur toutes les plateformes possible.

> [!TIP]
> .NET standard 2.0 représente une quantité plus grande surface d’API que les bibliothèques de classes portables, y compris l’espace de noms System.IO. Pour les nouveaux projets, .NET Standard est préférable aux bibliothèques de classes portables.

### <a name="using-pcl"></a>À l’aide de la bibliothèque de classes portable

Une fois un projet de bibliothèque de classes portable a été créé, vous pouvez ajouter une référence à celui-ci à partir de n’importe quel projet d’Application ou une bibliothèque compatible de la même façon que vous ajoutez généralement des références. Dans Visual Studio, avec le bouton droit sur le nœud Références et choisissez `Add Reference...` puis basculez vers le **Solution > projets** onglet comme indiqué :

[![Ajoutez une référence à une bibliothèque de classes portable via l’onglet Ajouter les projets de référence](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

La capture d’écran suivante montre le volet de la Solution pour l’exemple d’application TaskyPortable, affichant la bibliothèque PCL en bas et une référence à cette bibliothèque PCL dans le projet Xamarin.iOS.

[![Exemple de solution TaskyPortable montrant une bibliothèque bibliothèque de classes portable](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

La sortie à partir d’une bibliothèque de classes portable (ie. l’assembly résultant DLL) peut également être ajouté comme une référence à la plupart des projets.
Bibliothèque de classes portable ainsi un moyen idéal de livrer des bibliothèques et les composants inter-plateformes.

-----

## <a name="pcl-example"></a>Exemple de bibliothèque de classes portable

Le [TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) exemple d’application montre comment une bibliothèque de classes Portable peut être utilisée avec Xamarin.
Voici quelques captures d’écran des applications qui en résulte en cours d’exécution sur iOS et Android :

[![](pcl-images/image18.png "Voici quelques captures d’écran des applications qui en résulte en cours d’exécution sur iOS, Android et Windows Phone")](pcl-images/image18.png#lightbox)

Il partage un nombre de classes de données et la logique qui sont purement portable code, et il montre également comment intégrer les exigences spécifiques à la plateforme à l’aide de l’Injection de dépendances pour l’implémentation de base de données SQLite.

Voici la structure de solution (dans Visual Studio pour Mac et Visual Studio respectivement) :

[![](pcl-images/image19.png "La structure de solution est illustrée ici dans Visual Studio pour Mac et Visual Studio respectivement")](pcl-images/image19.png#lightbox)

Étant donné que le code de SQLite-NET a des spécifiques à la plateforme (pour travailler avec les implémentations de SQLite sur chaque système d’exploitation) pour la démonstration à des fins il a été refactorisée en abstraite classe qui peut être compilée dans une bibliothèque de classes Portable, et le code réel implémenté en tant que sous-classe dans les projets iOS et Android.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

La bibliothèque de classes Portable est limitée dans les fonctionnalités de .NET, il peut prendre en charge. Comme il est compilé pour s’exécuter sur plusieurs plateformes, il ne peut pas être utiliser `[DllImport]` des fonctionnalités utilisées dans SQLite-NET. Au lieu de cela SQLite-NET est implémenté comme une classe abstraite et puis référencé par le reste du code partagé. Vous trouverez ci-dessous un extrait de l’API abstraite :

```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```

Le reste du code partagé utilise la classe abstraite pour « stocker » et « récupérer » des objets de la base de données. Dans toute application qui utilise cette classe abstraite, nous devons passer dans une implémentation complète qui fournit les fonctionnalités de base de données réelle.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid et TaskyiOS

IOS et projets d’application Android contient l’interface utilisateur et tout autre code spécifique à la plateforme utilisée pour le code partagé dans la bibliothèque PCL AutoEventWireup.

Ces projets contiennent également une implémentation de la base de données abstraite API qui fonctionne sur cette plateforme. Sur iOS et Android le Sqlite moteur de base de données est intégré au système d’exploitation, afin que l’implémentation peut utiliser `[DllImport]` comme indiqué pour fournir l’implémentation concrète de la connectivité de base de données. Voici un extrait du code d’implémentation spécifique à la plateforme :

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

L’implémentation complète peut être constatée dans l’exemple de code.

## <a name="summary"></a>Récapitulatif

Cet article a brièvement abordé les avantages et les pièges de bibliothèques de classes portables, vous a montré comment créer et consommer des bibliothèques de classes portables à partir de Visual Studio pour Mac et Visual Studio. et enfin introduit une exemple complet d’application – TaskyPortable – qui montre une bibliothèque de classes portable en action.

## <a name="related-links"></a>Liens connexes

- [TaskyPortable (exemple)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Création d’applications multiplateformes](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)
- [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md)
- [Le développement multiplateforme avec le Kit de développement .NET Framework (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
