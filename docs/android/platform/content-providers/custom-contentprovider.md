---
title: "Création d’un ContentProvider personnalisé"
description: "La section précédente vous a montré comment consommer des données à partir d’une implémentation ContentProvider intégrée. Cette section explique comment générer un ContentProvider personnalisé et ensuite utiliser ses données."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 66b956eddc48699c6fd61e9cb52a7fbc3fa70a51
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-custom-contentprovider"></a>Création d’un ContentProvider personnalisé

_La section précédente vous a montré comment consommer des données à partir d’une implémentation ContentProvider intégrée. Cette section explique comment générer un ContentProvider personnalisé et ensuite utiliser ses données._

## <a name="about-contentproviders"></a>À propos de ContentProviders

Une classe de fournisseur de contenu doit hériter de `ContentProvider`. Il doit être composé d’un magasin de données internes qui est utilisé pour répondre aux requêtes et elle doit exposer des URI et les Types MIME comme constantes pour aider à la consommation du code effectuent des demandes valides pour les données.

### <a name="uri-authority"></a>URI (autorité)

`ContentProviders` sont accessibles dans Android à l’aide d’un Uri. Une application qui expose un `ContentProvider` définit les URI auxquels il doit répondre dans son **AndroidManifest.xml** fichier. Lorsque l’application est installée, ces URI sont enregistrés afin que les autres applications peuvent y accéder.

En Mono pour Android, la classe de fournisseur de contenu doit avoir un `[ContentProvider]` attribut pour spécifier l’Uri (ou URI) qui doit être ajouté à **AndroidManifest.xml**.

<a name="Mime_Type" />

### <a name="mime-type"></a>Type MIME

Le format par défaut pour les Types MIME se compose de deux parties. Android `ContentProviders` utilisent généralement des ces deux chaînes pour la première partie du Type MIME :

1. `vnd.android.cursor.item` &ndash; pour représenter une seule ligne, utilisez le `ContentResolver.CursorItemBaseType` constante dans le code.

1. `vnd.android.cursor.dir` &ndash; pour plusieurs lignes, utilisez la `ContentResolver.CursorDirBaseType` constante dans le code.

La deuxième partie du Type MIME est spécifique à votre application et doit utiliser une norme DNS inverse avec un `vnd.` préfixe. L’exemple de code utilise `vnd.com.xamarin.sample.Vegetables`.

<a name="Data_Model_Metadata" />

### <a name="data-model-metadata"></a>Métadonnées de modèle de données

Applications consommatrices amené à créer des requêtes d’Uri pour accéder à différents types de données. L’Uri de base peut être développé pour faire référence à une table de données et peut-être également inclure des paramètres pour filtrer les résultats. Les colonnes et les clauses utilisées avec le curseur résultant pour afficher des données doivent également être déclarées.

Pour vous assurer que seules les requêtes Uri valides sont construits, il est d’usage pour fournir les chaînes valides en tant que valeurs de constante. Cela facilite l’accès le `ContentProvider` , car elle rend les valeurs détectable via l’exécution de code et empêche les fautes de frappe dans les chaînes.

Dans l’exemple précédent la `android.provider.ContactsContract` classe exposée les métadonnées pour les données de Contacts. Pour notre propre `ContentProvider` nous expose uniquement les constantes sur la classe elle-même.

<a name="Implementation" />

## <a name="implementation"></a>Implémentation

Il existe trois étapes de création et utilisation d’une personnalisée `ContentProvider`:

1. **Créer une classe de base de données** &ndash; implémenter `SQLiteOpenHelper`.

2. **Créer un `ContentProvider` classe** &ndash; implémentez `ContentProvider` avec une instance de la base de données, les métadonnées exposées en tant que valeurs de constante et des méthodes pour accéder aux données.

3. **Accès le `ContentProvider` via son Uri** &ndash; Populate une `CursorAdapter` à l’aide de la `ContentProvider`, accessible via son Uri.

Comme indiqué précédemment, `ContentProviders` peuvent être utilisés à partir d’applications autres qu’où ils sont définis. Dans cet exemple, les données sont consommées dans la même application, mais n’oubliez pas que les autres applications peuvent également y accéder tant qu’ils connaissent l’Uri et les informations sur le schéma (qui est généralement exposé en tant que valeurs de constante).

<a name="Create_a_database" />

## <a name="create-a-database"></a>Créer une base de données

La plupart des `ContentProvider` implémentations doit reposer sur un `SQLite` base de données. L’exemple de code de base de données dans **SimpleContentProvider/VegetableDatabase.cs** crée une base de données très simple de deux colonnes, comme indiqué :

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

Lui-même l’implémentation de base de données n’a pas besoin de considérations spéciales à exposer avec un `ContentProvider`, toutefois si vous souhaitez lier le `ContentProvider's` données à un `ListView` contrôle ensuite une colonne d’entiers unique nommée `_id` doit faire partie de la jeu de résultats. Consultez le [ListViews et adaptateurs](~/android/user-interface/layouts/list-view/index.md) document pour plus d’informations sur l’utilisation de la `ListView` contrôle.

<a name="Create_the_ContentProvider" />

## <a name="create-the-contentprovider"></a>Créer le ContentProvider

Le reste de cette section donne des instructions détaillées sur la façon dont le **SimpleContentProvider/VegetableProvider.cs** exemple de classe a été généré.

<a name="Initialize_the_Database" />

### <a name="initialize-the-database"></a>Initialiser la base de données

La première étape consiste à sous-classer `ContentProvider` et ajouter la base de données qu’il utilisera.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Le reste du code forment l’implémentation du fournisseur de contenu réel qui permet aux données d’être découverts et interrogé.

<a name="Add_Metadata_for_Consumers" />


## <a name="add-metadata-for-consumers"></a>Ajouter des métadonnées pour les consommateurs

Il existe quatre types différents de métadonnées que nous allons exposent sur la `ContentProvider` classe. Seul l’autorité est requise, le reste sont effectuées par convention.

- **Autorité** &ndash; le `ContentProvider` attribut *doit* être ajoutée à la classe afin qu’il est inscrit avec le Android lorsque l’application est installée.

- **URI** &ndash; le `CONTENT_URI` est exposé en tant que constante afin qu’il soit facile à utiliser dans le code. Il doit correspondre à l’autorité de, mais incluent le schéma et le chemin d’accès de base.

- **Types MIME** &ndash; des listes de résultats et les résultats sont traités comme des différents types de contenu, afin de nous définissons deux Types MIME pour représenter les.

- **InterfaceConsts** &ndash; fournir une valeur constante pour chaque nom de colonne de données, afin que la consommation du code peut découvrir et y faire référence sans risquer de fautes de frappe.

Ce code montre comment chacun de ces éléments est implémentée, ajout à la définition de la base de données à partir de l’étape précédente :

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

<a name="Implement_the_URI_Parsing_Helper" />

## <a name="implement-the-uri-parsing-helper"></a>Implémenter l’URI de l’analyse d’assistance

Étant donné que la consommation du code utilise des URI pour effectuer des demandes d’un `ContentProvider`, nous devons être en mesure d’analyser ces requêtes pour déterminer les données à retourner. Le `UriMatcher` classe permettent d’analyser les URI, une fois qu’il a été initialisé avec l’Uri des modèles qui le `ContentProvider` prend en charge.

Le `UriMatcher` dans l’exemple sera initialisé avec deux URI :

1. *« com.xamarin.sample.VegetableProvider/vegetables »* &ndash; demande de retour de la liste complète des légumes.

2. *« com.xamarin.sample.VegetableProvider/vegetables/\#»* &ndash; où le \# est un espace réservé pour un paramètre numérique (la `_id` de la ligne dans la base de données). Un espace réservé d’astérisque («\*») peut également être utilisé pour correspondre à un paramètre de texte.

Dans le code que nous utilisons l’une des constantes pour faire référence à des valeurs de métadonnées telles que l’autorité et de la BASE de\_chemin d’accès. Les codes de retour seront utilisés dans les méthodes qui effectuent des Uri de l’analyse, pour déterminer les données à renvoyer.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Ce code est privé tout le `ContentProvider` classe. Reportez-vous à [documentation de UriMatcher de Google](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) pour plus d’informations.

<a name="Implement_the_QueryMethod" />

## <a name="implement-the-querymethod"></a>Implémentez la QueryMethod

Le plus simple `ContentProvider` consiste à implémenter la `Query` (méthode). L’implémentation ci-dessous utilise le `UriMatcher` pour analyser la `uri` paramètre et appelez la méthode de base de données correcte. Si le `uri` contient un paramètre ID, l’entier est analysé (à l’aide de `LastPathSegment`) et utilisé dans la requête de base de données.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

Le `GetType` méthode doit également être substituée. Cette méthode peut être appelée pour déterminer le type de contenu qui sera retourné pour un Uri donné.
Cela peut indiquer l’application consommatrice de la gestion des données.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

<a name="Implement_the_Other_Overrides" />

## <a name="implement-the-other-overrides"></a>Implémentez les autres substitutions

Notre exemple n’autorise pas de modification ou de suppression de données, mais les méthodes Insert, Update et Delete doivent être implémentées par conséquent, les ajouter sans implémentation :

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Ainsi se termine le basic `ContentProvider` implémentation. Une fois que l’application a été installée, les données qu’il expose seront disponibles à la fois à l’intérieur de l’application, mais aussi à toute autre application qui connaît l’Uri pour la référencer.

<a name="Access_the_ContentProvider" />

## <a name="access-the-contentprovider"></a>Accéder à la ContentProvider

Une fois le `VegetableProvider` a été implémenté, y accéder s’effectue la même façon que le fournisseur de Contacts au début de ce document : obtenir un curseur à l’aide de l’Uri spécifié, puis utilisez un adaptateur pour accéder aux données.

<a name="Bind_a_ListView_to_a_ContentProvider" />

## <a name="bind-a-listview-to-a-contentprovider"></a>Lier un contrôle ListView à une ContentProvider

Pour remplir un `ListView` avec des données, nous utilisons l’Uri qui correspond à la liste filtrée de légumes. Dans le code, nous utilisons la valeur de constante `VegetableProvider.CONTENT_URI`, ce qui nous savons donne `com.xamarin.sample.vegetableprovider/vegetables`. Notre `VegetableProvider.Query` implémentation retourne un curseur qui peut alors être lié à la `ListView`.

Le code dans `SimpleContentProvider/HomeScreen.cs` montre combien il est simple pour afficher les données d’une `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

L’application résultante ressemble à ceci :

[![Capture d’écran de l’application de liste de légumes, Fruits, boutons, coupés fleur, légumes, ampoules, tubercules](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png)


<a name="Retrieve_a_Single_Item_from_a_ContentProvider" />

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Récupérer un élément unique à partir d’un ContentProvider

Une application consommatrice peut également accéder aux lignes uniques de données, ce qui peuvent être effectuées en construisant un autre Uri qui fait référence à une ligne spécifique (par exemple).

Utilisez `ContentResolver` directement accéder à un seul élément, en créant un Uri avec requis `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

La méthode complète ressemble à ceci :

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```


## <a name="related-links"></a>Liens associés

- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
