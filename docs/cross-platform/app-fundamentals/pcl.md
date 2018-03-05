---
title: "Introduction aux bibliothèques de classes portables"
description: "Cet article présente les projets de bibliothèque de classes Portable (PCL) et Guide de création et utilisation des projets de bibliothèque de classes portables dans Visual Studio pour Mac et Visual Studio."
ms.topic: article
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e3701960f246a8f627d991edf244656b5fd8958e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-portable-class-libraries"></a>Introduction aux bibliothèques de classes portables

_Cet article présente les projets de bibliothèque de classes Portable (PCL) et Guide de création et utilisation des projets de bibliothèque de classes portables dans Visual Studio pour Mac et Visual Studio._


Un composant essentiel de la création des applications multiplateformes est la capacité à partager du code entre les différents projets spécifiques à la plateforme. Toutefois, cela est compliqué par le fait que différentes plateformes utilisent souvent un ensemble différent de sous-système de la bibliothèque de classes de Base (BCL) .NET et par conséquent sont réellement générés dans un autre profil de bibliothèque de noyaux de .NET. Cela signifie que chaque plate-forme peut uniquement utiliser les bibliothèques de classes qui sont ciblés sur le même profil, afin d’exiger des projets de bibliothèque de classes distinct pour chaque plateforme apparition.

Il existe trois méthodes principales pour le partage de code permettant de traiter ce problème : **projets .NET Standard**, **des projets de bibliothèque de classes Portable (PCL)**, et **projets d’actifs partagés**.

- **Projets .NET standard** [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).
-  **Bibliothèque de classes portables** projets ciblent des profils spécifiques qui prennent en charge un jeu connu de fonctions/classes de BCL. Toutefois, l’inconvénient de la bibliothèque de classes portables est qu’elles nécessitent souvent effort supplémentaire architectural pour séparer le code spécifique de profil dans leurs propres bibliothèques. Pour plus d’informations sur ces deux approches, consultez la [Sharing Code Options guide](~/cross-platform/app-fundamentals/code-sharing.md) .
-  **Projets d’actifs partagés** utiliser un seul ensemble de fichiers et offre un moyen rapide et simple permettant de partager du code au sein d’une solution et généralement utilise les directives de compilation conditionnelle pour spécifier les chemins de code pour les différentes plateformes qui utiliseront (pour plus d’informations d’informations, consultez la [article du partage des projets](~/cross-platform/app-fundamentals/shared-projects.md) et [configuration d’un guide de Solution de Cross-Platform Xamarin](~/cross-platform/app-fundamentals/code-sharing.md) ).


Cette page explique comment créer un **PCL** projet qui cible un profil spécifique, qui peut ensuite être référencé par plusieurs projets spécifiques à la plateforme.

## <a name="requirements"></a>Configuration requise

Les projets de bibliothèque portables sont automatiquement activés dans Visual Studio pour Mac sur macOS et sont intégrés à Visual Studio 2013 et versions ultérieures.


## <a name="what-is-a-portable-class-library"></a>Qu’est une bibliothèque de classes Portable ?

Lorsque vous créez un projet d’Application ou un projet de bibliothèque, la DLL résultante est limitée à travailler sur la plateforme spécifique, qu'il est créé. Cela vous empêche de l’écriture d’un assembly pour une application Windows, puis son utilisation nouveau Xamarin.iOS et Xamarin.Android.

Toutefois, lorsque vous créez une bibliothèque de classes portables, vous pouvez choisir une combinaison des plateformes que vous souhaitez que votre code s’exécute. Les choix de compatibilité que vous effectuez lors de la création d’une bibliothèque de classes Portable sont traduites en un identificateur de « Profil », qui décrit les plateformes sur lesquelles prend en charge de la bibliothèque.

Le tableau ci-dessous répertorie certaines des fonctionnalités qui varient selon la plateforme .NET. Pour écrire un assembly de bibliothèque PCL est garanti pour s’exécuter sur les plateformes d’appareils spécifiques/vous choisissez simplement la prise en charge est requis lorsque vous créez le projet.

<table border="1" cellpadding="1" cellspacing="1">
  <tbody>
    <tr>
      <td>
Fonctionnalité </td>
      <td>
.NET Framework </td>
      <td>
Applications UWP </td>
      <td>
Silverlight </td>
      <td>
Windows Phone </td>
      <td>
Xamarin </td>
    </tr>
    <tr>
      <td>
Principal </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
    </tr>
    <tr>
      <td>
LINQ </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
    </tr>
    <tr>
      <td>
IQueryable </td>
       <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
7.5 + </td>
      <td>
Y </td>
    </tr>
    <tr>
      <td>
Sérialisation </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
Y </td>
    </tr>
    <tr>
      <td>
Annotations de données </td>
      <td>
4.0.3 + </td>
      <td>
Y </td>
      <td>
Y </td>
      <td>
      </td>
      <td>
Y </td>
    </tr>
  </tbody>
</table>

La colonne Xamarin reflète le fait que Xamarin.iOS et Xamarin.Android prend en charge tous les profils inclus dans Visual Studio 2013 et versions ultérieures, et la disponibilité des fonctionnalités dans toutes les bibliothèques que vous créez est uniquement limitée par les autres plateformes que vous choisissez de prise en charge.

Cela inclut les profils qui sont des combinaisons de :

-  .NET 4 ou .NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  Applications UWP

Vous pouvez en savoir plus sur les fonctionnalités sur les différents profils [du site Web Microsoft](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx) et consultez un autre membre de la Communauté [profil PCL](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) qui inclut la prise en charge les informations de framework et d’autres remarques.



Création d’une bibliothèque de classes portables pour partager le code a un nombre d’avantages et inconvénients par rapport à l’autre liaison de fichiers :


**Avantages**

1. Partage de code centralisée – écrire et tester le code dans un projet unique qui peut être consommé par d’autres applications ou de bibliothèques.
1. Opérations de refactorisation affecte tout le code chargé dans la solution (la bibliothèque de classes portables et les projets spécifiques à la plateforme).
1. Le projet de bibliothèque de classes portables peut être facilement référencé par d’autres projets dans une solution ou l’assembly de sortie peut être partagée pour d’autres personnes à référencer dans leurs solutions.


**Inconvénients**

1. Étant donné que la même bibliothèque de classes Portable est partagée entre plusieurs applications, les bibliothèques spécifiques à la plateforme ne peut être référencés (par exemple). Community.CsharpSqlite.WP7).
1. Le sous-ensemble de la bibliothèque de classes portables ne peut pas inclure les classes qui seraient normalement disponibles dans MonoTouch et Mono pour Android (par exemple, DllImport ou System.IO.File).



Dans une certaine mesure, les deux inconvénients peuvent être contournées à l’aide du modèle de fournisseur ou Injection de dépendances coder l’implémentation réelle dans les projets de plateforme par rapport à une classe d’interface ou de base qui est défini dans la bibliothèque de classes Portable.



Ce diagramme illustre l’architecture d’une application multiplateforme à l’aide d’une bibliothèque de classes Portable pour partager du code, mais également à l’aide de l’Injection de dépendances à passer dans les fonctionnalités dépend de la plateforme :



[![](pcl-images/image1.png "Ce diagramme illustre l’architecture d’une application multiplateforme à l’aide d’une bibliothèque de classes Portable pour partager du code, mais également à l’aide de l’Injection de dépendances à passer dans les fonctionnalités dépend de la plateforme")](pcl-images/image1.png)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Procédure pas à pas Visual Studio pour Mac


Cette section vous montre comment créer et utiliser une bibliothèque de classes Portable à l’aide de Visual Studio pour Mac. Consultez la section exemple de bibliothèque de classes portables pour une implémentation complète.



### <a name="creating-a-pcl"></a>Création d’une bibliothèque de classes portables


Ajout d’une bibliothèque de classes Portable à votre solution est très similaire à l’ajout d’un projet de bibliothèque standard.


1. Dans la boîte de dialogue Nouveau projet, sélectionnez le `Multiplatform > Library > Portable Library` option


    ![](pcl-images/image2.png "Multiplateforme > bibliothèque > bibliothèque Portable")


1. Création d’une bibliothèque de classes portables dans Visual Studio pour Mac, il est automatiquement configuré avec un profil qui fonctionne pour Xamarin.iOS et Xamarin.Android. Le projet de bibliothèque PCL s’affiche comme indiqué dans cette capture d’écran :

    ![](pcl-images/image3.png "Projet de bibliothèque PCL s’affichent comme illustré dans cette capture d’écran")

La bibliothèque PCL est maintenant prête pour le code à ajouter. Il peut également être référencée par d’autres projets (projets d’Application, les projets de bibliothèque et même d’autres projets de bibliothèque PCL).



### <a name="editing-pcl-settings"></a>Modification des paramètres de la bibliothèque de classes portables


Pour afficher et modifier les paramètres de la bibliothèque de classes portables pour ce projet, cliquez sur le projet et choisissez **Options > Générer > Général** pour voir l’écran illustré ici :



[![](pcl-images/image4.png "Pour afficher et modifier les paramètres de la bibliothèque de classes portables pour ce projet, cliquez sur le projet et choisissez Options générer général pour afficher l’écran présenté ici")](pcl-images/image4.png)



Les paramètres de cet écran contrôlent les plates-formes sur lesquelles cette bibliothèque PCL particulier est compatible avec. La modification de ces options modifie le profil utilisé par cette bibliothèque de classes portables, qui à son tour contrôle les fonctionnalités qui peuvent être utilisées dans le code portable.



La modification de la `Target Framework` options met automatiquement à jour le `Current Profile`; l’écran affiche également un avertissement si des options incompatibles sont sélectionnées.



[![](pcl-images/image5.png "La modification des options de la cible de .NET Framework automatiquement le profil actuel des mises à jour l’écran affiche également un avertissement si des options incompatibles sont sélectionnées.")](pcl-images/image5.png)



Si le profil est modifié une fois que le code a déjà été ajouté à la bibliothèque PCL, il est possible que la bibliothèque n’est plus compilées si le code fait référence à des fonctionnalités qui ne font pas partie du profil qui vient d’être sélectionné.


## <a name="working-with-a-pcl"></a>Utilisation avec une PCL


Lorsque le code est écrit dans une bibliothèque PCL, Visual Studio pour l’éditeur Mac reconnaît les limitations du profil sélectionné et ajuster en conséquence les options de saisie semi-automatique. Par exemple, cette capture d’écran montre les options de saisie semi-automatique pour System.IO en utilisant le profil par défaut (Profile136) utilisé dans Visual Studio pour Mac : Notez la barre de défilement indique environ la moitié des classes disponibles sont affichée (en fait il existe 14 uniquement classes disponibles).



[![](pcl-images/image6.png "E/s en utilisant le profil par défaut Profile136 utilisée dans Visual Studio pour Mac Notez la barre de défilement indique environ la moitié des classes disponibles sont affichée en fait il sont uniquement 14 classes disponibles")](pcl-images/image6.png)



Comparer qu’avec la saisie semi-automatique dans un projet Xamarin.iOS ou Xamarin.Android – de System.IO 40 classes sont disponibles, notamment couramment utilisé des classes telles que `File` et `Directory` qui ne sont pas dans un profil de bibliothèque de classes portables.



[![](pcl-images/image7.png "Il existe des 40 classes disponibles, notamment couramment utilisés tels que des fichiers et de répertoires, les classes qui ne sont pas dans n’importe quel profil PCL")](pcl-images/image7.png)



Cela reflète un compromis sous-jacent à l’aide de la bibliothèque de classes portables : la possibilité de partager du code en toute transparence dans de nombreuses plates-formes signifie que certaines API n’est pas disponibles car ils n’ont des implémentations comparables sur toutes les plateformes possibles.



### <a name="using-pcl"></a>À l’aide de la bibliothèque de classes portables


Après avoir créé un projet de bibliothèque de classes portables, vous pouvez ajouter une référence à celui-ci à partir d’un projet d’Application ou une bibliothèque compatible de la même façon que vous ajoutez des références normalement. Dans Visual Studio pour Mac, avec le bouton droit sur le nœud Références et choisissez `Edit References…` puis basculez vers l’onglet de projets, comme indiqué :



[![](pcl-images/image8.png "Dans Visual Studio pour Mac, avec le bouton droit sur le nœud Références et choisissez Modifier les références, puis basculez vers l’onglet projets comme")](pcl-images/image8.png)



La capture d’écran suivante montre le remplissage de la Solution pour l’exemple d’application TaskyPortable, affichant la bibliothèque PCL au bas de la référence à cette bibliothèque de classes portables dans le projet Xamarin.iOS.



[![](pcl-images/image9.png "Le remplissage de la Solution pour l’exemple d’application TaskyPortable")](pcl-images/image9.png)



La sortie à partir d’une bibliothèque de classes portables (ie. l’assembly résultant DLL) peut également être ajoutée comme une référence à la plupart des projets. Cela rend PCL une façon idéale pour expédier les bibliothèques et les composants inter-plateformes.




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Procédure pas à pas Visual Studio


Cette section vous montre comment créer et utiliser une bibliothèque de classes Portable à l’aide de Visual Studio. Consultez la section exemple de bibliothèque de classes portables pour une implémentation complète.



### <a name="creating-a-pcl"></a>Création d’une bibliothèque de classes portables


Ajout d’une bibliothèque de classes portables à votre solution dans Visual Studio est légèrement différente à l’ajout d’un projet standard.



1. Dans l’écran Ajouter un nouveau projet, sélectionnez le `Portable Class Library` option


    ![](pcl-images/image10.png "Bibliothèque de classes portable")


1. Visual Studio vous invite immédiatement avec la boîte de dialogue suivante afin que le profil peut être configuré.
 Cochez les plateformes que vous devez prendre en charge et appuyez sur OK.


    ![](pcl-images/image11.png "Cochez les plateformes que vous devez prendre en charge et appuyez sur OK")


1. Projet de bibliothèque PCL s’affiche comme illustré dans l’Explorateur de solutions. Le nœud Références indique que la bibliothèque utilise un sous-ensemble du .NET Framework (défini par le profil de bibliothèque PCL).

    ![](pcl-images/image12.png "NET Framework définies par le profil de la bibliothèque de classes portables")

La bibliothèque PCL est maintenant prête pour le code à ajouter. Il peut également être référencée par d’autres projets (projets d’Application, les projets de bibliothèque et même d’autres projets de bibliothèque PCL).



### <a name="editing-pcl-settings"></a>Modification des paramètres de la bibliothèque de classes portables


Les paramètres de la bibliothèque de classes portables peuvent être affichées et modifiées en cliquant sur le projet et en choisissant **Propriétés > bibliothèque** , comme indiqué dans cette capture d’écran :



[![](pcl-images/image13.png "Les paramètres de la bibliothèque de classes portables peuvent être affichées et modifiées en cliquant sur le projet et en choisissant Propriétés bibliothèque, comme indiqué dans cette capture d’écran")](pcl-images/image13.png)



Si le profil est modifié une fois que le code a déjà été ajouté à la bibliothèque PCL, il est possible que la bibliothèque n’est plus compilées si le code fait référence à des fonctionnalités qui ne font pas partie du profil qui vient d’être sélectionné.



### <a name="working-with-a-pcl"></a>Utilisation avec une PCL


Lorsque le code est écrit dans une bibliothèque PCL, Visual Studio reconnaît les limitations du profil sélectionné et ajuster en conséquence les options Intellisense. Par exemple, cette capture d’écran montre les options de saisie semi-automatique pour System.IO en utilisant le profil par défaut (Profile136) – Notez la barre de défilement indique environ la moitié des classes disponibles sont affichée (en fait il existe uniquement des 14 classes).



[![](pcl-images/image14.png "E/s en utilisant le profil par défaut Profile136")](pcl-images/image14.png)



Comparer qu’avec la saisie semi-automatique dans un projet standard – de System.IO 40 classes sont disponibles, notamment couramment utilisé des classes telles que `File` et `Directory` qui ne sont pas dans un profil de bibliothèque de classes portables.



[![](pcl-images/image15.png "Saisie semi-automatique dans un projet standard")](pcl-images/image15.png)



Cela reflète un compromis sous-jacent à l’aide de la bibliothèque de classes portables : la possibilité de partager du code en toute transparence dans de nombreuses plates-formes signifie que certaines API n’est pas disponibles car ils n’ont des implémentations comparables sur toutes les plateformes possibles.



### <a name="using-pcl"></a>À l’aide de la bibliothèque de classes portables


Après avoir créé un projet de bibliothèque de classes portables, vous pouvez ajouter une référence à celui-ci à partir d’un projet d’Application ou une bibliothèque compatible de la même façon que vous ajoutez des références normalement. Dans Visual Studio, avec le bouton droit sur le nœud Références et choisissez `Add Reference...` vous basculez ensuite vers le **Solution : projets** onglet comme indiqué :



[![](pcl-images/image16.png "Onglet projets comme")](pcl-images/image16.png)



La capture d’écran suivante montre le volet de la Solution pour l’exemple d’application TaskyPortable, affichant la bibliothèque PCL au bas de la référence à cette bibliothèque de classes portables dans le projet Xamarin.iOS.



[![](pcl-images/image17.png "Le volet de la Solution pour l’exemple d’application TaskyPortable")](pcl-images/image17.png)



La sortie à partir d’une bibliothèque de classes portables (ie. l’assembly résultant DLL) peut également être ajoutée comme une référence à la plupart des projets.
Cela rend PCL une façon idéale pour expédier les bibliothèques et les composants inter-plateformes.




-----



## <a name="pcl-example"></a>Exemple de bibliothèque de classes portables


Le [TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) exemple d’application montre comment une bibliothèque de classes Portable peuvent être utilisée avec Xamarin.
Voici certaines des captures d’écran des applications qui en résulte en cours d’exécution sur iOS, Android et Windows Phone :



[![](pcl-images/image18.png "Voici certaines des captures d’écran des applications qui en résulte en cours d’exécution sur iOS, Android et Windows Phone")](pcl-images/image18.png)



Il partage un nombre de classes de données et la logique du code entièrement portables, et il montre également comment incorporer des exigences spécifiques à une plateforme à l’aide de l’Injection de dépendances pour l’implémentation de base de données SQLite.




Voici la structure de la solution (dans Visual Studio pour Mac et Visual Studio respectivement) :



[![](pcl-images/image19.png "La structure de la solution est illustrée ici dans Visual Studio pour Mac et Visual Studio respectivement")](pcl-images/image19.png)



Étant donné que le code de SQLite-NET a spécifique à la plateforme (pour travailler avec les implémentations de SQLite sur chaque système d’exploitation) pour la démonstration à des fins qu’il a été refactorisé dans un résumé de la classe qui peut être compilée dans une bibliothèque de classes portables, et le code réel implémenté en tant que sous-classes dans les projets iOS et Android.



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

La bibliothèque de classes Portable est limitée dans les fonctionnalités de .NET, il peut prendre en charge. Comme il est compilé pour s’exécuter sur plusieurs plateformes, il ne peut pas être utiliser `[DllImport]` fonctionnalité est utilisée dans SQLite-NET. Au lieu de cela SQLite-NET est implémenté comme une classe abstraite et puis référencé par le reste du code partagé. Vous trouverez ci-dessous un extrait de l’API abstraite :


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


Le reste du code partagé utilise la classe abstraite pour « stocker » et « extraire » des objets à partir de la base de données. Dans toute application qui utilise cette classe abstraite, nous devons passer dans une implémentation complète qui fournit les fonctionnalités de base de données.



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid et TaskyiOS


IOS et les projets d’application Android contient l’interface utilisateur et tout autre code spécifique à la plateforme utilisée pour le code partagé dans la bibliothèque PCL câble à distance.



Ces projets contiennent également une implémentation de la base de données abstrait API qui fonctionne sur cette plateforme. Sur iOS et Android le Sqlite moteur de base de données est intégré au système d’exploitation, par conséquent, l’implémentation peut utiliser `[DllImport]` comme indiqué pour fournir l’implémentation concrète de la connectivité de base de données. Voici un extrait du code de mise en œuvre spécifiques à la plateforme :


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


L’implémentation complète peut être observée dans l’exemple de code.

### <a name="taskywinphone"></a>TaskyWinPhone


L’application Windows Phone a son interface utilisateur créé avec le XAML et contient l’autre code spécifique à la plateforme pour connecter les objets partagés avec l’interface utilisateur.



Contrairement à l’implémentation utilisée pour iOS et Android, l’application Windows Phone doit créer et utiliser une instance de la `Community.Sqlite.dll` dans le cadre de son abstraite les API de base de données. Au lieu d’utiliser `DllImport`, méthodes, telles que Open sont implémentées par rapport à l’assembly Community.Sqlite qui est référencé dans le `TaskWinPhone` projet. Un extrait est illustré ici, pour la comparaison avec iOS et Android version ci-dessus


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>Récapitulatif


Cet article a brièvement décrit les avantages et les inconvénients des bibliothèques de classes portables, vous a montré comment créer et consommer PCLs à partir de Visual Studio pour Mac et Visual Studio ; et enfin a introduit une application d’exemple complet – TaskyPortable – qui montre une bibliothèque PCL en action.


## <a name="related-links"></a>Liens associés

- [TaskyPortable (sample)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Création d’applications multiplateformes](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Projets partagés](~/cross-platform/app-fundamentals/shared-projects.md)
- [Options de partage de Code](~/cross-platform/app-fundamentals/code-sharing.md)
- [Le développement multiplateforme avec le .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
