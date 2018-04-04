---
title: À l’aide de SQLite.NET
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2018
ms.openlocfilehash: 8d68df2c29afe828482da7c5747b30dc5d30a5de
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="using-sqlitenet"></a>À l’aide de SQLite.NET

La bibliothèque SQLite.NET qui vous recommande de Xamarin est une base ORM qui vous permet de stocker et récupérer des objets dans la base de données SQLite local sur un appareil iOS.
ORM signifie mappage objet relationnel – une API qui vous permet d’enregistrer et de récupérer les « objets » à partir d’une base de données sans avoir à écrire des instructions SQL.

<a name="Usage"/>

## <a name="usage"></a>Utilisation

Ajouter le [package NuGet de bibliothèque PCL SQLite.net](https://www.nuget.org/packages/sqlite-net-pcl/), à votre projet - il prend en charge un éventail de plateformes, y compris iOS, Android et Windows.

  [![](using-sqlite-orm-images/image1a-sml.png "Package NuGet de SQLite.NET")](using-sqlite-orm-images/image1a.png#lightbox)

Une fois que vous avez la bibliothèque SQLite.NET disponible, suivez ces trois étapes pour l’utiliser pour accéder à une base de données :


1. **Ajouter un à l’aide instruction** -ajouter l’instruction suivante pour les fichiers c# où l’accès aux données est requise :

        using SQLite;

1. **Créer une base de données vide** -une référence de base de données peut être créée en passant le chemin d’accès du constructeur de classe SQLiteConnection. Vous n’avez pas besoin de vérifier si le fichier existe déjà, il sera créé automatiquement si nécessaire, sinon le fichier existant de la base de données s’ouvre.

        var db = new SQLiteConnection (dbPath);

    La variable dbPath doit être déterminée selon les règles décrites précédemment dans ce document.

1. **Enregistrer les données** - une fois que vous avez créé un objet SQLiteConnection, de base de données, les commandes sont exécutées en appelant ses méthodes, telles que CREATE TABLE et insérer comme suit :

        db.CreateTable<Stock> ();
        db.Insert (newStock); // after creating the newStock object

1. **Récupérer des données** - pour récupérer un objet (ou une liste d’objets) utilisez la syntaxe suivante :

        var stock = db.Get<Stock>(5); // primary key id of 5
        var stockList = db.Table<Stock>();

## <a name="basic-data-access-sample"></a>Exemple d’accès aux données de base

Le *DataAccess_Basic* exemple de code pour ce document ressemble à ceci lorsque vous exécutez sur iOS. Le code montre comment effectuer des opérations simples SQLite.NET et affiche les résultats sous forme de texte dans la fenêtre principale de l’application.

**iOS**

 ![](using-sqlite-orm-images/image2.png "exemple de SQLite.NET iOS")

L’exemple de code suivant montre une interaction de base de données entière à l’aide de la bibliothèque SQLite.NET pour encapsuler l’accès de base de données sous-jacente. Il montre :

1.  Création du fichier de base de données
1.  Insertion des données en créant des objets et enregistrez-les
1.  Interrogation des données


Vous devez inclure ces espaces de noms :

```csharp
using SQLite; // from the github SQLite.cs class
```

Cela nécessite que vous avez ajouté SQLite à votre projet, mise en évidence [ici](#Usage). Notez que la table de base de données SQLite est définie en ajoutant des attributs à une classe (le `Stock` classe) au lieu d’une commande CREATE TABLE.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

À l’aide de la `[Table]` attribut sans spécifier un paramètre de nom de table entraîne la table de base de données sous-jacente ont le même nom que la classe (dans ce cas, « Action »). Le nom de la table réelle est important si vous écrivez des requêtes SQL directement sur la base de données plutôt que d’utilisez les méthodes d’accès aux données ORM. De même la `[Column("_id")]` attribut est facultatif et si absent une colonne sera ajouté à la table avec le même nom que la propriété de la classe.

## <a name="sqlite-attributes"></a>Attributs de SQLite

Attributs courants que vous pouvez appliquer à vos classes pour contrôler la façon dont elles sont stockées dans la base de données sous-jacente sont les suivantes :

-  **[PrimaryKey]**  – Cet attribut peut être appliqué à une propriété entière pour le forcer à être la clé primaire de la table sous-jacente. Les clés primaires composites ne sont pas pris en charge.
-  **[AutoIncrement]**  – Cet attribut entraîne la valeur d’une propriété entière à incrémentation automatique pour chaque nouvel objet inséré dans la base de données
-  **[Column(name)]**  – En fournissant le paramètre facultatif `name` paramètre remplace la valeur par défaut du nom de la colonne de base de données sous-jacente (qui est identique à la propriété).
-  **[Table(name)]**  – Marque la classe comme pouvant être stockées dans une table de SQLite sous-jacente. En spécifiant le paramètre de nom facultatif remplace la valeur par défaut du nom de la table de base de données sous-jacente (qui est le même que le nom de classe).
-  **[MaxLength(value)]**  – Restreignent la longueur d’une propriété de texte, lors de la tentative d’insertion d’une base de données. Consommation du code doit effectuer cette vérification avant l’insertion de l’objet que cet attribut est uniquement « coché » lors de l’insertion d’une base de données ou l’opération de mise à jour est tentée.
-  **[Ignorer]**  – Provoque un SQLite.NET pour ignorer cette propriété. Cela est particulièrement utile pour les propriétés qui ont un type qui ne peut pas être stocké dans la base de données ou les propriétés que les collections du modèle qui ne peut pas être résolues automatiquement être SQLite.
-  **[Unique]**  – Permet de s’assurer que les valeurs dans la colonne de base de données sous-jacente sont uniques.


La plupart de ces attributs sont facultatifs, SQLite utilise les valeurs par défaut pour les noms de table et de colonne. Vous devez toujours spécifier une clé primaire de type entier afin que les requêtes de sélection et la suppression peuvent être effectuées efficacement sur vos données.

## <a name="more-complex-queries"></a>Requêtes plus complexes.

Les méthodes suivantes sur `SQLiteConnection` peut être utilisé pour effectuer d’autres opérations de données :

-  **Insérer** – ajoute un nouvel objet à la base de données.
-  **Obtenir<T>**  – tente de récupérer un objet à l’aide de la clé primaire.
-  **Table<T>**  – renvoie tous les objets de la table.
-  **Supprimer** : supprime un objet à l’aide de sa clé primaire.
-  **Requête<T>**  -exécuter une requête SQL qui retourne un nombre de lignes (sous forme d’objets).
-  **Exécutez** – Utilisez cette méthode (et non `Query` ) lorsque vous ne prévoyez pas les lignes à partir de l’instruction SQL (par exemple, les instructions INSERT, UPDATE et DELETE).


### <a name="getting-an-object-by-the-primary-key"></a>Obtention d’un objet à la clé primaire

SQLite.Net fournit la méthode Get pour extraire un seul objet en fonction de sa clé primaire.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Sélection d’un objet à l’aide de Linq

Prend en charge les méthodes qui retournent des collections IEnumerable<T> afin de pouvoir utiliser Linq pour interroger ou de trier le contenu d’une table. Le code suivant montre un exemple d’utilisation de Linq pour filtrer toutes les entrées qui commencent par la lettre « A » :

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Sélection d’un objet à l’aide de SQL

Même si SQLite.Net peut fournir basée sur un objet d’accès à vos données, parfois, vous devrez peut-être faire une requête plus complexe que ce qui permet de Linq (ou vous devrez peut-être améliorer les performances). Vous pouvez utiliser des commandes SQL avec la méthode de requête, comme indiqué ici :

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> Lors de l’écriture d’instructions SQL directement, vous créez une dépendance sur les noms des tables et des colonnes dans votre base de données qui ont été générés à partir de vos classes et leurs attributs. Si vous modifiez ces noms dans votre code vous devez mettre à jour toutes les instructions SQL écrites manuellement.

### <a name="deleting-an-object"></a>Suppression d’un objet

La clé primaire est utilisée pour supprimer la ligne, comme illustré ici :

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Vous pouvez vérifier le `rowcount` pour vérifier le nombre de lignes affecté (supprimé dans ce cas).

## <a name="using-sqlitenet-with-multiple-threads"></a>À l’aide de SQLite.NET avec plusieurs Threads

SQLite prend en charge trois modes de threads : *seul thread*, *multithread*, et *sérialisé*. Si vous souhaitez accéder à la base de données à partir de plusieurs threads sans aucune restriction, vous pouvez configurer SQLite à utiliser le **sérialisé** en mode de thread. Il est important de définir ce mode tôt dans votre application (par exemple, au début de la `OnCreate` méthode).

Pour modifier le mode de thread, appelez `SqliteConnection.SetConfig`. Par exemple, cette ligne de code configure SQLite pour **sérialisé** mode :

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```


## <a name="related-links"></a>Liens associés

- [DataAccess Basic (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avancé (exemple)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS recettes de données](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Accès aux données de Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
