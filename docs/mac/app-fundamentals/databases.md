---
title: "Bases de données"
description: "Cet article couvre l’utilisation de la clé-valeur de codage et observer pour permettre la liaison de données entre bases de données SQLite et éléments d’interface utilisateur dans le Générateur de Xcode Interface clé-valeur. Elle traite également à l’aide de l’ORM SQLite.NET pour fournir l’accès aux données de SQLite."
ms.topic: article
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 39186735749174884646a72871accfa051d2901c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="databases"></a>Bases de données

_Cet article couvre l’utilisation de la clé-valeur de codage et observer pour permettre la liaison de données entre bases de données SQLite et éléments d’interface utilisateur dans le Générateur de Xcode Interface clé-valeur. Elle traite également à l’aide de l’ORM SQLite.NET pour fournir l’accès aux données de SQLite._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès aux bases de données SQLite mêmes accessibles à une application Xamarin.iOS ou Xamarin.Android.

Dans cet article que nous aborderons deux façons d’accéder aux données de SQLite :

1. **Accès direct** - en accédant directement à une base de données SQLite, nous pouvons utiliser des données à partir de la base de données pour le codage de la clé-valeur et la liaison de données avec les éléments d’interface utilisateur créé dans le Générateur de Xcode Interface. À l’aide de codage et liaison de données techniques dans votre application Xamarin.Mac clé-valeur, vous pouvez réduire considérablement la quantité de code que vous devez écrire et conserver pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage de vos données de sauvegarde (_modèle de données_) à partir de votre premier plan mettre fin à l’Interface utilisateur (_Model-View-Controller_), début au plus facile à gérer, une application plus souple conception.
2. **Les ORM SQLite.NET** - en utilisant l’open source [SQLite.NET](http://www.sqlite.org) relation Manager ORM (Object) nous pouvons réduire considérablement la quantité de code requis pour lire et écrire des données à partir d’une base de données SQLite. Ces données peuvent ensuite être utilisées pour remplir un élément d’interface utilisateur tel qu’une vue de Table.

[![Un exemple de l’application en cours d’exécution](databases-images/intro01.png "un exemple de l’application en cours d’exécution")](databases-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de la clé-valeur de codage et de la liaison de données avec les bases de données SQLite dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Étant donné que nous allons utiliser la liaison de données et de codage de la clé-valeur, veuillez consulter le [de liaison de données et de codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) tout d’abord, comme les techniques de base et concepts seront abordées qui sera utilisé dans cette documentation et son application.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` attributs utilisé pour associer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

## <a name="direct-sqlite-access"></a>SQLite accès direct

Pour les données de SQLite vont être lié à des éléments d’interface utilisateur dans le Générateur de Xcode Interface, il est fortement recommandé que vous accéder à la base de données SQLite directement (par opposition à l’aide d’une technique comme un ORM), étant donné que vous avez un contrôle total sur la façon dont les données sont écrites et lire  à partir de la base de données.

Comme nous l’avons vu dans la [de liaison de données et de codage de la clé-valeur](~/mac/app-fundamentals/databinding.md) documentation, à l’aide de codage clé-valeur et la liaison de données techniques dans votre application Xamarin.Mac, permet de réduire considérablement la quantité de code que vous devez écrire et mettre à jour pour remplir et utiliser des éléments d’interface utilisateur. Lorsqu’elles sont combinées avec un accès direct à une base de données SQLite, il peut également réduire la quantité de code nécessaire pour lire et écrire des données dans cette base de données.

Dans cet article, nous allons modifier l’exemple d’application à partir de la liaison de données et le document de codage clé-valeur à utiliser une base de données SQLite comme source de sauvegarde pour la liaison.

### <a name="including-sqlite-database-support"></a>Prise en charge de la base de données SQLite

Avant de continuer, nous devons ajouter la prise en charge de la base de données SQLite à notre application en incluant des références aux deux. Fichiers DLL.

Effectuez ce qui suit :

1. Dans le **Solution remplissage**, avec le bouton droit sur le **références** et sélectionnez **modifier les références**.
2. Sélectionnez à la fois le **Mono.Data.Sqlite** et **System.Data** assemblys : 

    [![Ajout des références requises](databases-images/reference01.png "Ajout des références requises")](databases-images/reference01-large.png#lightbox)
3. Cliquez sur le **OK** bouton pour enregistrer vos modifications et ajoutez les références.

### <a name="modifying-the-data-model"></a>Modifier le modèle de données

Maintenant que nous avons ajouté la prise en charge pour accéder directement à une base de données SQLite notre application, nous devons modifier notre objet de modèle de données pour lire et écrire des données à partir de la base de données (ainsi que pour fournir la clé-valeur de codage et de la liaison de données). Dans le cas de notre exemple d’application, nous allons modifier le **PersonModel.cs** de classe et le rendre l’aspect suivant :

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

Examinons les modifications en détail ci-dessous.

Tout d’abord, nous avons ajouté plusieurs à l’aide des instructions qui sont requises pour utiliser SQLite et nous avons ajouté une variable pour enregistrer notre dernière connexion à la base de données SQLite :

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Nous allons utiliser cette connexion enregistrée pour enregistrer automatiquement toutes les modifications à l’enregistrement de la base de données quand l’utilisateur modifie le contenu dans l’interface utilisateur via la liaison de données :

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

Toute modification apportée à la **nom**, **profession** ou **isManager** propriétés seront envoyées à la base de données si les données n’a été enregistrées il (par exemple, si le `_conn` variable n’est pas `null`). Ensuite, examinons les méthodes que nous avons ajouté à **créer**, **mise à jour**, **charge** et **supprimer** personnes à partir de la base de données.

#### <a name="create-a-new-record"></a>Créer un nouvel enregistrement

Le code suivant a été ajouté pour créer un nouvel enregistrement dans la base de données SQLite :

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Nous utilisons un `SQLiteCommand` pour créer le nouvel enregistrement dans la base de données. Nous obtenons une nouvelle commande à partir de la `SQLiteConnection` (conn) que nous avons passé dans la méthode en appelant `CreateCommand`. Ensuite, nous avons défini l’instruction SQL pour écrire réellement le nouvel enregistrement, en fournissant des paramètres pour les valeurs réelles :

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Plus tard, nous avons défini les valeurs pour les paramètres à l’aide de la `Parameters.AddWithValue` méthode sur le `SQLiteCommand`. À l’aide de paramètres, nous nous assurons que que les valeurs (par exemple, un guillemet simple) codés correctement avant d’être envoyés à SQLite. Exemple :

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Enfin, dans la mesure où une personne peut comporter un gestionnaire et une collection d’employés qu’ils contiennent, nous sont de manière récursive appelant le `Create` méthode sur ces personnes à les enregistrer dans la base de données :

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>Mise à jour un enregistrement

Le code suivant a été ajouté pour mettre à jour un enregistrement existant dans la base de données SQLite :

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Comme **créer** ci-dessus, nous obtenons un `SQLiteCommand` à partir de `SQLiteConnection`et définissez notre SQL pour mettre à jour notre enregistrement (en fournissant les paramètres) :

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Nous renseigner les valeurs de paramètre (exemple : `command.Parameters.AddWithValue ("@COL1", ID);`) et à nouveau, appeler de façon récursive mise à jour sur tous les enfants enregistrements :

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Le chargement d’un enregistrement

Le code suivant a été ajouté pour charger un enregistrement existant à partir de la base de données SQLite :

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

Étant donné que la routine peut être appelée de manière récursive à partir d’un objet parent (par exemple, un objet de gestionnaire du chargement de leur objet employés), un code spécial a été ajouté pour gérer l’ouverture et de fermeture de la connexion à la base de données :

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

Comme toujours, nous avons défini notre SQL pour récupérer l’enregistrement et d’utiliser des paramètres :

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Enfin, nous utilisons un lecteur de données pour exécuter la requête et retourner les champs d’enregistrement (qui nous copions dans l’instance de la `PersonModel` classe) :

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

Si cette personne est un gestionnaire, nous devons également charger tous les employés (là encore, en appelant leurs `Load` (méthode)) :

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>Suppression d’un enregistrement

Le code suivant a été ajouté pour supprimer un enregistrement existant à partir de la base de données SQLite :

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

Nous proposons les instructions SQL pour supprimer l’enregistrement de gestionnaires et les enregistrements de tous les employés sous ce gestionnaire (à l’aide de paramètres) :

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Une fois que l’enregistrement a été supprimé, nous effacer l’instance actuelle de la `PersonModel` classe :

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>L’initialisation de la base de données

Avec les modifications apportées à notre modèle de données en place pour prendre en charge la lecture et en écriture à la base de données, nous devons ouvrir une connexion à la base de données et l’initialiser à la première exécution. Ajoutez le code suivant à notre **MainWindow.cs** fichier :

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
        };
        conn.Open ();
        foreach (var cmd in commands) {
            using (var c = conn.CreateCommand()) {
                c.CommandText = cmd;
                c.CommandType = CommandType.Text;
                c.ExecuteNonQuery ();
            }
        }
        conn.Close ();

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

Prenons le code ci-dessus plus approfondie. Tout d’abord, nous choisir un emplacement pour la nouvelle base de données (dans cet exemple, bureau de l’utilisateur), vérifiez si la base de données existe et si elle n’est pas, créez-le :

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Ensuite, nous établir la connexion à la base de données utilisant le chemin d’accès que nous avons créés ci-dessus :

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Puis nous créer toutes les tables SQL dans la base de données dont nous avons besoin :

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
};
conn.Open ();
foreach (var cmd in commands) {
    using (var c = conn.CreateCommand()) {
        c.CommandText = cmd;
        c.CommandType = CommandType.Text;
        c.ExecuteNonQuery ();
    }
}
conn.Close ();
```

Enfin, nous utilisons notre modèle de données (`PersonModel`) pour créer un ensemble par défaut d’enregistrements pour la base de données de la première exécution de l’application ou si la base de données est manquant :

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

Lorsque l’application démarre et ouvre la fenêtre principale, nous établir une connexion à la base de données en utilisant le code que nous avons ajouté ci-dessus :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Le chargement des données liées

Tous les composants pour accéder directement aux données liées à partir d’une base de données SQLite en place, nous pouvons charger les données dans les différentes vues qui fournit de notre application, et elle s’affichera automatiquement dans notre interface utilisateur.

#### <a name="loading-a-single-record"></a>Le chargement d’un enregistrement unique

Pour charger un seul enregistrement dont l’ID est savez, nous pouvons utiliser le code suivant :

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Le chargement de tous les enregistrements

Pour charger tous les utilisateurs, quel que soit s’ils sont un gestionnaire ou non, utilisez le code suivant :

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

Ici, nous utilisons une surcharge du constructeur pour la `PersonModel` classe à charger la personne en mémoire :

```csharp
var person = new PersonModel (_conn, childID);
```

Nous allons appeler également la classe lié aux données pour ajouter la personne à notre collection de personnes `AddPerson (person)`, ainsi que notre interface utilisateur reconnaît la modification et l’affiche :

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Chargement des enregistrements de niveau supérieur uniquement

Pour charger les gestionnaires uniquement (par exemple, pour afficher des données en mode plan), nous utilisons le code suivant :

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

La seule différence dans l’instruction SQL en (ce qui charge composée uniquement de responsables `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), mais fonctionne de la même que la section ci-dessus dans le cas contraire.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Bases de données et des zones de liste modifiable

Les contrôles de Menu disponibles pour macOS (par exemple, la zone de liste modifiable) peut être définis pour remplir la liste déroulante à partir d’une liste interne (qui permettre être prédéfinie dans le Générateur de Interface ou remplie via du code) ou en fournissant votre propre source de données externe personnalisée. Consultez [fournissant des données de contrôle Menu](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) pour plus d’informations.

Par exemple, modifier l’exemple de liaison Simple ci-dessus dans le constructeur d’Interface, ajoutez une zone de liste modifiable et l’exposer à l’aide d’une sortie nommée `EmployeeSelector`:

[![Exposition d’une prise de zone de liste modifiable](databases-images/combo01.png "exposer une prise de zone de liste déroulante")](databases-images/combo01-large.png#lightbox)

Dans le **attributs inspecteur**, vérifiez le **Autocompletes** et **utilise une Source de données** propriétés :

![Configurer les attributs de zone de liste modifiable](databases-images/combo02.png "configurer les attributs de zone de liste déroulante")

Enregistrez vos modifications et revenir à Visual Studio pour Mac à synchroniser.

#### <a name="providing-combobox-data"></a>Fournit les données de la zone de liste déroulante

Ensuite, ajoutez une nouvelle classe au projet nommé `ComboBoxDataSource` et le rendre l’aspect suivant :

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

Dans cet exemple, nous allons créer un nouveau `NSComboBoxDataSource` qui peut présenter les éléments de zone de liste déroulante à partir de n’importe quelle Source de données SQLite. Tout d’abord, nous définissons les propriétés suivantes :

- `Conn` -Obtient ou définit une connexion à la base de données SQLite.
- `TableName` -Obtient ou définit le nom de la table.
- `IDField` -Obtient ou définit le champ qui fournit l’ID unique de la Table donnée. La valeur par défaut est `ID`.
- `DisplayField` -Obtient ou définit le champ affiché dans la liste déroulante.
- `RecordCount` -Obtient le nombre d’enregistrements dans la Table donnée.

Lorsque vous créez une nouvelle instance de l’objet, nous transmettre la connexion, nom de la table, et éventuellement le champ ID et le champ d’affichage :

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

Le `GetRecordCount` méthode retourne le nombre d’enregistrements dans la Table donnée :

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

Elle est appelée à tout moment la `TableName`, `IDField` ou `DisplayField` valeur de propriété est modifiée.

Le `IDForIndex` méthode retourne l’ID unique (`IDField`) pour l’enregistrement à l’index d’élément de liste déroulante donné : 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Le `ValueForIndex` méthode retourne la valeur (`DisplayField`) pour l’élément à l’index de la liste déroulante donné :

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Le `IDForValue` méthode retourne l’ID unique (`IDField`) pour la valeur donnée (`DisplayField`) :

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

Le `ItemCount` retourne précalculé nombre d’éléments dans la liste, comme elle est calculée lorsque le `TableName`, `IDField` ou `DisplayField` propriétés sont modifiées :

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

Le `ObjectValueForItem` méthode fournit la valeur (`DisplayField`) pour l’index d’élément de liste déroulante donné :

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Notez que nous utilisons le `LIMIT` et `OFFSET` instructions dans notre commande SQLite pour limiter à l’enregistrement d’un que nous sommes nécessaires.

Le `IndexOfItem` méthode retourne l’index d’élément de liste déroulante de la valeur (`DisplayField`) donné :

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

Si la valeur ne peut pas être trouvée, le `NSRange.NotFound` valeur est renvoyée et tous les éléments sont désélectionnés dans la liste déroulante.

Le `CompletedString` méthode retourne la première valeur correspondante (`DisplayField`) pour une entrée partiellement typée :

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>Affichage des données et réponse aux événements

Pour afficher tous les éléments ensemble, vous devez modifier le `SubviewSimpleBindingController` et le rendre l’aspect suivant :

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

Le `DataSource` propriété fournit un raccourci vers le `ComboBoxDataSource` (créé ci-dessus) attaché à la zone de liste déroulante.

Le `LoadSelectedPerson` méthode charge de la personne à partir de la base de données pour l’ID Unique donné :

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

Dans la `AwakeFromNib` substitution de méthode, tout d’abord nous attacher une instance de notre Source de données de zone de liste déroulante personnalisée :

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Ensuite, nous répondre à l’utilisateur de modifier la valeur de texte de la zone de liste déroulante en recherchant l’ID unique associé (`IDField`) des données de présentation et de chargement de la personne donnée si trouvent :

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Nous avons également charger une nouvelle personne si l’utilisateur sélectionne un nouvel élément dans la liste déroulante :

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Enfin, nous remplissent automatiquement la zone de liste déroulante et de la personne affichée avec le premier élément dans la liste :

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Comme indiqué précédemment, à l’aide de la source ouverte [SQLite.NET](http://www.sqlite.org) relation Manager ORM (Object) nous pouvons réduire considérablement la quantité de code requis pour lire et écrire des données à partir d’une base de données SQLite. Cela ne peut pas être le meilleur itinéraire en cas de liaison de données en raison de plusieurs des conditions requises par clé-valeur de codage et de liaison de données sur un objet.

Selon le site Web SQLite.Net, _« SQLite est une bibliothèque de logiciels qui implémente un moteur de base de données SQL autonome, sans serveur, sans aucune configuration, transactionnels. SQLite est le moteur de base de données plus largement déployée dans le monde. Le code source de SQLite est dans le domaine public. »_

Dans les sections suivantes, nous allons montrer comment utiliser SQLite.Net pour fournir des données pour une vue de Table.

### <a name="including-the-sqlitenet-nuget"></a>Y compris SQLite.net NuGet

SQLite.NET est présenté sous forme de NuGet Package que vous incluez dans votre application. Avant que nous pouvons ajouter la prise en charge de base de données à l’aide de SQLite.NET, nous devons inclure ce package.

Procédez comme suit pour ajouter le package :

1. Dans le **Solution remplissage**, avec le bouton droit le **Packages** et sélectionnez **ajouter des Packages...**
2. Entrez `SQLite.net` dans les **zone de recherche** et sélectionnez le **sqlite-net** entrée :

    [![Ajout du package NuGet de SQLite](databases-images/nuget01.png "Ajout du package NuGet de SQLite")](databases-images/nuget01-large.png#lightbox)
3. Cliquez sur le **ajouter un Package** bouton Terminer.

### <a name="creating-the-data-model"></a>Création du modèle de données

Nous allons ajouter une nouvelle classe au projet et appeler dans `OccupationModel`. Ensuite, nous allons modifier le **OccupationModel.cs** de fichier et le rendre l’aspect suivant :

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

Tout d’abord, nous incluons SQLite.NET (`using Sqlite`), puis nous exposons plusieurs propriétés, chacun d’eux est écrit dans la base de données lors de l’enregistrement de cet enregistrement. La première propriété nous rendre comme clé primaire et affectez-lui la valeur incrémentation automatique comme suit :

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>L’initialisation de la base de données

Avec les modifications apportées à notre modèle de données en place pour prendre en charge la lecture et en écriture à la base de données, nous devons ouvrir une connexion à la base de données et l’initialiser à la première exécution. Nous allons ajouter le code suivant :

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

Tout d’abord, nous obtenir un chemin d’accès à la base de données (poste de travail de l’utilisateur dans ce cas) et si la base de données existe déjà :

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Ensuite, nous établissons une connexion à la base de données sur le chemin d’accès que nous avons créés ci-dessus :

```csharp
var conn = new SQLiteConnection (db);
```

Enfin, nous crée la table et ajouter des enregistrements par défaut :

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>Ajout d’une vue de table

Comme un exemple d’utilisation, nous allons ajouter une vue de Table à notre interface utilisateur dans le Générateur de Xcode Interface. Nous allons exposer cette Table via une prise de vue (`OccupationTable`) afin de nous pouvons accéder à via le code c# :

[![Exposition d’une prise de vue table](databases-images/table01.png "exposer une prise de vue de table")](databases-images/table01-large.png#lightbox)

Ensuite, nous allons ajouter des classes personnalisées pour remplir cette table avec des données à partir de la base de données SQLite.NET.

### <a name="creating-the-table-data-source"></a>Création de la source de données de table

Nous allons créer une Source de données personnalisée pour fournir des données pour notre table. Tout d’abord, ajoutez une nouvelle classe appelée `TableORMDatasource` et qu’elle ressemble à ce qui suit :

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

Lorsque vous créez une instance de cette classe plus tard, nous allons passer dans notre SQLite.NET ouvrir une connexion de base de données. Le `LoadOccupations` méthode interroge la base de données et copie les enregistrements trouvés dans la mémoire (à l’aide de notre `OccupationModel` modèle de données).

### <a name="creating-the-table-delegate"></a>Créer le délégué de la table

La classe finale, que nous avons besoin est un délégué personnalisé de la Table pour afficher les informations que nous avons chargé à partir de la base de données SQLite.NET. Nous allons ajouter un nouveau `TableORMDelegate` à notre projet et rendre l’aspect suivant :

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Nous utilisons ici la Source de données `Occupations` collection (que nous avons chargé à partir de la base de données SQLite.NET) pour remplir les colonnes de la table via le `GetViewForItem` substitution de méthode.

### <a name="populating-the-table"></a>Remplissage de la table

Tous les éléments en place, nous allons remplir notre table lorsqu’elle est agrandie à partir du fichier .xib en remplaçant le `AwakeFromNib` méthode et rend la recherche comme suit :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

Tout d’abord, nous avons accès à notre base de données SQLite.NET, création et remplissage s’il n’existe pas. Créez ensuite une nouvelle instance de notre Source de données de Table personnalisée, passez dans notre connexion de base de données et nous l’attacher à la Table. Enfin, nous créer une nouvelle instance de notre délégué personnalisé de la Table, passez dans notre Source de données et l’attacher à la table.

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation de la liaison de données et la clé-valeur de codage avec les bases de données SQLite dans une application Xamarin.Mac. Tout d’abord, il se présentait à exposer une classe c# pour Objective-C en utilisant la clé-valeur de codage (KVM) et clé-valeur en observant (KVO). Ensuite, il vous a montré comment utiliser une classe conforme KVO et données lier à des éléments d’interface utilisateur dans le Générateur de Xcode Interface. L’article est également décrite utilisation de données SQLite via les ORM SQLite.NET et afficher ces données dans une vue de Table.



## <a name="related-links"></a>Liens associés

- [MacDatabase (exemple)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Liaison de données et la clé-valeur de codage](~/mac/app-fundamentals/databinding.md)
- [Contrôles standard](~/mac/user-interface/standard-controls.md)
- [Vues de table](~/mac/user-interface/table-view.md)
- [Mode plan](~/mac/user-interface/outline-view.md)
- [Vues de collection](~/mac/user-interface/collection-view.md)
- [Guide de programmation de codage clé-valeur](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Introduction aux liaisons/Cocoa sujets de programmation](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Introduction à la référence de liaisons/Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
