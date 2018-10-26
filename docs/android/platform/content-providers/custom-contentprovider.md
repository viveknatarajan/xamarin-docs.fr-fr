---
title: Création d’un ContentProvider personnalisé
description: La section précédente vous a montré comment consommer des données à partir d’une implémentation de ContentProvider intégrée. Cette section explique comment générer un ContentProvider personnalisé et les utiliser ensuite ses données.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: da8aacac1f282fefb6b8d0e84cae168cf3a7148b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108013"
---
# <a name="creating-a-custom-contentprovider"></a>Création d’un ContentProvider personnalisé

_La section précédente vous a montré comment consommer des données à partir d’une implémentation de ContentProvider intégrée. Cette section explique comment générer un ContentProvider personnalisé et les utiliser ensuite ses données._

## <a name="about-contentproviders"></a>À propos de Contentprovider

Une classe de fournisseur de contenu doit hériter de `ContentProvider`. Il doit se composer d’un magasin de données internes qui est utilisé pour répondre aux requêtes et elle doit exposer des URI et les Types MIME comme constantes pour aider à code de consommation rendent les demandes valides pour les données.

### <a name="uri-authority"></a>URI (autorité)

`ContentProviders` sont accessibles dans Android à l’aide d’un Uri. Une application qui expose un `ContentProvider` définit les URI auxquels il doit répondre dans son **AndroidManifest.xml** fichier. Lorsque l’application est installée, ces URI sont enregistrés afin que les autres applications peuvent y accéder.

Mono pour Android, la classe de fournisseur de contenu doit avoir un `[ContentProvider]` attribut pour spécifier l’Uri (ou URI) qui doit être ajouté à **AndroidManifest.xml**.


### <a name="mime-type"></a>Type MIME

Le format standard pour les Types MIME est constitué de deux parties. Android `ContentProviders` utilisent généralement ces deux chaînes pour la première partie du Type MIME :

1. `vnd.android.cursor.item` &ndash; pour représenter une seule ligne, utilisez le `ContentResolver.CursorItemBaseType` constante dans le code.

1. `vnd.android.cursor.dir` &ndash; pour plusieurs lignes, utilisez la `ContentResolver.CursorDirBaseType` constante dans le code.

La deuxième partie du Type MIME est spécifique à votre application et doit utiliser une norme de DNS inversé à un `vnd.` préfixe. L’exemple de code utilise `vnd.com.xamarin.sample.Vegetables`.


### <a name="data-model-metadata"></a>Métadonnées de modèle de données

Beaucoup d’applications ont besoin construire des requêtes d’Uri pour accéder à différents types de données. L’Uri de base peut être développé pour faire référence à une table de données et peut également inclure des paramètres pour filtrer les résultats. Les colonnes et les clauses utilisées avec le curseur résultant pour afficher les données doivent également être déclarés.

Pour vous assurer que seules les requêtes Uri valides sont construits, il est courant de fournir les chaînes valides comme des valeurs constantes. Cela rend plus facile pour accéder à la `ContentProvider` car elle rend les valeurs détectable par le biais de complétion de code et empêche les fautes de frappe dans les chaînes.

Dans l’exemple précédent la `android.provider.ContactsContract` classe exposée les métadonnées pour les données de Contacts. Pour notre propre `ContentProvider` nous allons exposer simplement l’une des constantes sur la classe elle-même.


## <a name="implementation"></a>Implémentation

Il existe trois étapes pour créer et consommer un personnalisé `ContentProvider`:

1. **Créer une classe de base de données** &ndash; implémenter `SQLiteOpenHelper`.

2. **Créer un `ContentProvider` classe** &ndash; implémenter `ContentProvider` avec une instance de la base de données, les métadonnées exposées en tant que valeurs de constante et des méthodes pour accéder aux données.

3. **Accès le `ContentProvider` par le biais de son Uri** &ndash; peupler un `CursorAdapter` à l’aide de la `ContentProvider`, accessible par le biais de son Uri.

Comme indiqué précédemment, `ContentProviders` peut être consommé à partir d’applications autres qu’où elles sont définies. Dans cet exemple, les données sont consommées dans la même application, mais n’oubliez pas que les autres applications peuvent également y accéder tant qu’ils connaissent l’Uri et les informations sur le schéma (qui est généralement exposée comme des valeurs constantes).


## <a name="create-a-database"></a>Créer une base de données

La plupart des `ContentProvider` implémentations doit reposer sur un `SQLite` base de données. L’exemple de code de base de données dans **SimpleContentProvider/VegetableDatabase.cs** crée une base de données de deux colonnes très simple, comme indiqué :

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

L’implémentation de base de données elle-même n’a pas besoin d’être exposés avec des considérations particulières un `ContentProvider`, toutefois si vous avez l’intention de lier le `ContentProvider's` données à un `ListView` contrôle ensuite une colonne entier unique nommée `_id` doivent faire partie de la jeu de résultats. Consultez le [ListViews et Adapters](~/android/user-interface/layouts/list-view/index.md) document pour plus d’informations sur l’utilisation de la `ListView` contrôle.


## <a name="create-the-contentprovider"></a>Créer le ContentProvider

Le reste de cette section donne des instructions détaillées sur la façon dont le **SimpleContentProvider/VegetableProvider.cs** exemple de classe a été créé.


### <a name="initialize-the-database"></a>Initialiser la base de données

La première étape consiste à la sous-classe `ContentProvider` et ajouter la base de données qu’il utilisera.

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



## <a name="add-metadata-for-consumers"></a>Ajouter des métadonnées pour les consommateurs

Il existe quatre différents types de métadonnées que nous allons exposer sur le `ContentProvider` classe. Seul l’autorité est requise, le reste sont effectuées par convention.

- **Autorité** &ndash; le `ContentProvider` attribut *doit* être ajoutée à la classe afin qu’il est inscrit avec le Android lorsque l’application est installée.

- **URI** &ndash; le `CONTENT_URI` est exposée en tant que constante afin qu’il soit facile à utiliser dans le code. Il doit correspondre à l’autorité, mais incluent le schéma et le chemin d’accès de base.

- **Types MIME** &ndash; listes de résultats et de résultats unique sont traitées comme des différents types de contenu, nous le définissons deux Types MIME pour les représenter.

- **InterfaceConsts** &ndash; fournir une valeur constante pour chaque nom de colonne de données, afin que le code de consommation peut découvrir et y faire référence sans risquer des erreurs typographiques.

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


## <a name="implement-the-uri-parsing-helper"></a>Implémenter l’URI de l’analyse d’assistance

Étant donné que la consommation du code utilise des URI pour effectuer des demandes d’un `ContentProvider`, nous devons être en mesure d’analyser ces requêtes pour déterminer les données à retourner. Le `UriMatcher` classe peut aider à analyser des URI, une fois qu’il a été initialisé avec l’Uri des modèles qui le `ContentProvider` prend en charge.

Le `UriMatcher` dans l’exemple sera initialisé avec deux URI :

1. *« com.xamarin.sample.VegetableProvider/vegetables »* &ndash; demande de retour de la liste complète des légumes.

2. *« com.xamarin.sample.VegetableProvider/vegetables/\#»* &ndash; où le \# est un espace réservé pour un paramètre numérique (la `_id` de la ligne dans la base de données). Un espace réservé d’astérisque («\*») peut également être utilisé pour correspondre à un paramètre de texte.

Dans le code, nous utilisons les constantes pour faire référence à des valeurs de métadonnées telles que l’autorité et de la BASE de\_chemin d’accès. Les codes de retour seront utilisés dans les méthodes qui effectuent des Uri de l’analyse, pour déterminer les données à renvoyer.

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

Ce code est tout privé pour le `ContentProvider` classe. Reportez-vous à [documentation de UriMatcher de Google](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) pour plus d’informations.


## <a name="implement-the-querymethod"></a>Implémenter le QueryMethod

La plus simple `ContentProvider` consiste à implémenter le `Query` (méthode). L’implémentation ci-dessous utilise le `UriMatcher` pour analyser le `uri` paramètre et appelez la méthode de base de données correcte. Si le `uri` contient un paramètre ID, l’entier est analysé (à l’aide de `LastPathSegment`) et utilisé dans la requête de base de données.

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

Le `GetType` méthode doit également être remplacée. Cette méthode peut être appelée pour déterminer le type de contenu qui sera retourné pour un Uri donné.
Cela peut indiquer à l’application consommatrice comment gérer ces données.

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


## <a name="implement-the-other-overrides"></a>Implémenter les autres substitutions

Notre exemple n’autorise pas de modification ou de suppression de données, mais les méthodes Insert, Update et Delete doivent être implémentées donc les ajouter sans implémentation :

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

Qui termine le basic `ContentProvider` implémentation. Une fois que l’application a été installée, les données qu’il expose seront disponibles à la fois à l’intérieur de l’application, mais aussi à toute autre application qui connaît l’Uri pour la référencer.


## <a name="access-the-contentprovider"></a>Accéder à la ContentProvider

Une fois le `VegetableProvider` a été implémenté, y accéder s’effectue la même façon que le fournisseur de Contacts au début de ce document : obtenir un curseur à l’aide de l’Uri spécifié, puis utiliser un adaptateur pour accéder aux données.


## <a name="bind-a-listview-to-a-contentprovider"></a>Lier un ListView à un ContentProvider

Pour remplir un `ListView` avec des données, nous utilisons le Uri qui correspond à la liste non filtrée de légumes. Dans le code, nous utilisons la valeur de constante `VegetableProvider.CONTENT_URI`, ce qui nous savons donne `com.xamarin.sample.vegetableprovider/vegetables`. Notre `VegetableProvider.Query` implémentation retourne un curseur qui peut ensuite être lié à la `ListView`.

Le code dans `SimpleContentProvider/HomeScreen.cs` montre combien il est simple pour afficher des données à partir d’un `ContentProvider`:

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

[![Capture d’écran de l’application de liste de légumes, Fruits, boutons de fleurs frais, légumes, ampoules, tubercules](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



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

- [SimpleContentProvider (exemple)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
