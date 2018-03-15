---
title: "Partie 5 - stratégies de partage de Code pratique"
ms.topic: article
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4c102181a1d2c345e0376f53f1f343cbc7be5551
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>Partie 5 - stratégies de partage de Code pratique

Cette section fournit des exemples montrant comment partager du code pour les scénarios d’application courants.



## <a name="data-layer"></a>Couche données

La couche données se compose d’un moteur de stockage et des méthodes pour lire et écrire des informations. Pour la compatibilité de la performance, flexibilité et inter-plateformes le SQLite moteur de base de données est recommandé pour les applications Xamarin inter-plateformes.
Il s’exécute sur un large éventail de plateformes, y compris Windows, Android, iOS et Mac.


### <a name="sqlite"></a>SQLite

SQLite est une implémentation de base de données open source. La source et la documentation, consultez [SQLite.org](http://www.sqlite.org/). Prise en charge de SQLite est disponible sur chaque plate-forme mobile :

-  **iOS** – intégrée dans le système d’exploitation.
- **Android** – intégrées au système d’exploitation depuis Android 2.2 (API niveau 10).
- **Windows** : consultez le [SQLite pour l’extension de la plateforme Windows universelle](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).


Même avec le moteur de base de données disponible sur toutes les plateformes, les méthodes natives pour accéder à la base de données sont différents. À la fois iOS et Android offrent des API intégrées pour accéder à SQLite qui peut être utilisée à partir de Xamarin.iOS ou Xamarin.Android, toutefois, l’utilisation des méthodes natives du Kit de développement logiciel n’offre aucune possibilité de partager du code (autres que peut-être les requêtes SQL, en supposant qu’ils sont stockés sous forme de chaînes) . Pour plus d’informations sur la recherche de fonctionnalités de base de données native pour `CoreData` dans iOS ou de Android `SQLiteOpenHelper` classe ; car ces options ne sont pas inter-plateformes, elles sont dépasse le cadre de ce document.



### <a name="adonet"></a>ADO.NET

Prise en charge de Xamarin.iOS et Xamarin.Android `System.Data` et `Mono.Data.Sqlite` (consultez la Xamarin.iOS [documentation](~/ios/data-cloud/system.data.md) pour plus d’informations).
À l’aide de ces espaces de noms vous permet d’écrire du code ADO.NET qui fonctionne sur les deux plateformes. Modifier les références du projet à inclure `System.Data.dll` et `Mono.Data.Sqlite.dll` et les ajouter à l’aide d’instructions à votre code :

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

L’exemple de code suivant ne fonctionnera :

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Implémentations réelles d’ADO.NET seraient évidemment être réparties entre différentes méthodes et classes (cet exemple est uniquement à des fins de démonstration).



### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET – inter-plateformes ORM

Un ORM (ou un mappeur objet / relationnel) tente de simplifier le stockage de données modélisées dans les classes. Plutôt que manuellement l’écriture de requêtes SQL que créer des TABLEs ou le sélectionner, insérer et supprimer des données manuellement extraites à partir de champs de la classe et propriétés, un ORM ajoute une couche de code qui fait pour vous. Utiliser la réflexion pour examiner la structure de vos classes, un ORM peut automatiquement créer les tables et les colonnes qui correspondent à une classe et de génèrent des requêtes pour lire et écrire les données. Cela permet simplement envoyer et récupérer des instances d’objet à l’ORM, qui prend en charge toutes les opérations SQL sous le capot de code d’application.

SQLite-NET agit comme un simple ORM qui vous permettra d’enregistrer et récupérer vos classes dans SQLite. Elle masque la complexité de cross-platform SQLite l’accès avec une combinaison de directives de compilateur et d’autres astuces.

Fonctionnalités de SQLite-NET :

-  Les tables sont définies en ajoutant des attributs aux classes de modèle.
-  Une instance de la base de données est représentée par une sous-classe de `SQLiteConnection` , la classe principale dans la bibliothèque de SQLite-Net.
-  Données peuvent être insérées, interrogé et supprimés à l’aide d’objets. Aucune instruction SQL n’est requise (bien que vous pouvez écrire des instructions SQL si nécessaire).
-  Base de requêtes Linq peuvent être effectuées sur les collections retournées par SQLite-NET.


Le code source et la documentation de SQLite-NET est disponible à l’adresse [SQLite-Net sur github](https://github.com/praeclarum/sqlite-net) et a été implémenté dans les deux études de cas. Un exemple simple de code de SQLite-NET (à partir de la *Tasky Pro* étude de cas) est indiqué ci-dessous.

Tout d’abord, la `TodoItem` classe utilise des attributs pour définir un champ pour être une clé primaire de base de données :

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Cela permet une `TodoItem` table doit être créé avec la ligne suivante de code (et aucune instruction SQL) sur un `SQLiteConnection` instance :

```csharp
CreateTable<TodoItem> ();
```

Données de la table peuvent également être manipulées avec d’autres méthodes sur le `SQLiteConnection` (là encore, sans nécessiter d’instructions SQL) :

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Consultez le code source étude de cas pour obtenir des exemples complets.



## <a name="file-access"></a>Accès aux fichiers

Accès au fichier est certaines une partie essentielle de toute application. Exemples courants de fichiers peuvent faire partie d’une application figurent :

-  Fichiers de base de données SQLite.
-  Données générées par l’utilisateur (texte, images, audio, vidéo).
-  Données téléchargées pour la mise en cache (images, html ou PDF).




### <a name="systemio-direct-access"></a>Accès Direct de System.IO

Autorisent l’accès au système de fichiers à l’aide des classes dans Xamarin.iOS et Xamarin.Android le `System.IO` espace de noms.

Chaque plateforme a des restrictions d’accès différents qui doivent être prises en considération :

-  des applications iOS de s’exécuter dans un bac à sable avec un accès de système de fichiers très limité. Apple davantage détermine la façon dont vous devez utiliser le système de fichiers en spécifiant certains emplacements qui sont sauvegardés (et autres qui ne sont pas). Reportez-vous à la [fonctionne avec le système de fichiers dans Xamarin.iOS](~/ios/app-fundamentals/file-system.md) guide pour plus de détails.
-  Android restreint également l’accès à certains répertoires liées à l’application, mais il prend également en charge les supports externes (par exemple). Les cartes SD) et l’accès aux données partagées.
-  Windows Phone 8 (Silverlight) n’autorisent pas les accès direct aux fichiers : fichiers peuvent uniquement être manipulées à l’aide de `IsolatedStorage`.
-  Projets WinRT de Windows 8.1 et Windows 10 UWP uniquement proposent des opérations de fichier asynchrone via `Windows.Storage` API, qui diffèrent des autres plateformes.

#### <a name="example-for-ios-and-android"></a>Exemple pour iOS et Android

Voici un exemple simple qui écrit et lit un fichier texte.
À l’aide de `Environment.GetFolderPath` permet le même code pour s’exécuter sur iOS et Android, laquelle retourner un répertoire valide selon les conventions de leur système de fichiers.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Reportez-vous à la Xamarin.iOS [fonctionne avec le système de fichiers](~/ios/app-fundamentals/file-system.md) document pour plus d’informations sur les fonctionnalités de système de fichiers spécifiques à iOS. Lorsque vous écrivez le code d’accès aux fichiers d’inter-plateformes, n’oubliez pas que certains systèmes de fichiers respectent la casse et ont des séparateurs de répertoire différent. Il est conseillé de toujours utiliser la même casse pour les noms de fichiers et le `Path.Combine()` méthode lors de la construction des chemins d’accès de fichier ou répertoire.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage pour Windows 8 et Windows 10

Le *créer des applications mobiles avec Xamarin.Forms* [book](https://developer.xamarin.com/r/xamarin-forms/book/)
[chapitre 20. Async et e/s de fichier](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) inclut [exemples pour Windows 8.1 et Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

À l’aide un [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) il est possible de lire et de fichiers sur ces plateformes à l’aide de l’API prises en charge :

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Reportez-vous à la [chapitre](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) pour plus d’informations.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Stockage isolé sur Windows Phone 7 et 8 (Silverlight)

Le stockage isolé est une API commune pour enregistrer et charger des fichiers sur l’ensemble iOS, Android et plateformes plus anciennes de Windows Phone.

Il est le mécanisme par défaut pour l’accès de fichier dans Windows Phone (Silverlight) qui a été implémenté dans Xamarin.iOS et Xamarin.Android pour autoriser le code d’accès aux fichiers courants à écrire. Le `System.IO.IsolatedStorage` classe peut être référencé dans les trois plateformes dans une [projet partagé](~/cross-platform/app-fundamentals/shared-projects.md).

Reportez-vous à la [vue d’ensemble du stockage isolé pour Windows Phone](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff402541(v=vs.105).aspx) pour plus d’informations.

Les API de stockage isolé ne sont pas disponibles dans [bibliothèques de classes portables](~/cross-platform/app-fundamentals/pcl.md). Une alternative pour la bibliothèque PCL est la [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Accès aux fichiers d’inter-plateformes dans PCLs

Il existe également un Nuget de la bibliothèque de classes portables compatibles – [PCLStorage](https://www.nuget.org/packages/PCLStorage/) – qu’installations inter-plateformes fichier l’accès pour les plateformes prises en charge de Xamarin et les API de Windows plus récentes.


## <a name="network-operations"></a>Opérations réseau

Les applications mobiles plus aura un composant réseau, par exemple :

-  Télécharger des images, vidéo et audio (par exemple). miniatures, photos, musique).
-  Téléchargement de documents (par exemple). HTML, PDF).
-  Chargement des données utilisateur (par exemple, des photos ou texte).
-  L’accès à des services web ou 3e partie API (y compris SOAP, XML ou JSON).


Le .NET Framework fournit quelques classes différentes pour accéder aux ressources du réseau : `HttpClient`, `WebClient`, et `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

Le `HttpClient` classe dans le `System.Net.Http` espace de noms est disponible dans la plupart des plateformes Windows, Xamarin.Android et Xamarin.iOS. Il existe un [Microsoft HTTP Client bibliothèque Nuget](https://www.nuget.org/packages/Microsoft.Net.Http/) qui peut être utilisé pour afficher cette API dans les bibliothèques de classes portables (et Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

La `WebClient` classe fournit une API simple pour récupérer des données distantes à partir des serveurs distants.

Les opérations Windows Platofrm universelles *doit* être asynchrone, même si Xamarin.iOS et Xamarin.Android prennent en charge les opérations synchrones (qui peuvent être effectuées sur les threads d’arrière-plan).

Le code pour une simple asynchrone `WebClient` l’opération est :

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` a également `DownloadFileCompleted` et `DownloadFileAsync` pour récupérer des données binaires.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` offre plusieurs possibilités de personnalisation que `WebClient` et par conséquent nécessite plus de code à utiliser.

Le code pour une simple synchrone `HttpWebRequest` l’opération est :

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

Voici un exemple dans notre [documentation des Services Web](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability" />


### <a name="reachability"></a>Accessibilité

Les appareils mobiles fonctionnent sous diverses conditions réseau à partir de rapide Wi-Fi ou connexions de 4G sur les domaines de réception de mauvaise qualité et les liaisons de données bord lentes. Pour cette raison, il est conseillé de détecter si le réseau est disponible et le cas échéant, le type de réseau est disponible, avant de tenter de se connecter aux serveurs distants.

Actions de qu'une application mobile peut prendre dans ces situations sont les suivantes :

-  Si le réseau n’est pas disponible, informer l’utilisateur. Si elles ont désactivé manuellement une (par exemple). Mode avion ou la désactivation de Wi-Fi), puis ils peuvent résoudre le problème.
-  Si la connexion est 3G, les applications peuvent se comporter différemment (par exemple, Apple n’autorise pas les applications plus de 20 Mo à télécharger plus de 3G). Applications pourrait utiliser ces informations pour avertir l’utilisateur sur le téléchargement excessive fois lors de la récupération des fichiers volumineux.
-  Même si le réseau est disponible, il est conseillé de vérifier la connectivité avec le serveur cible avant de lancer d’autres requêtes. Cela empêchera les opérations de réseau de l’application à partir de l’expiration du délai à plusieurs reprises et permettent également à un message d’erreur plus explicites être affiché à l’utilisateur.


Il existe un [Xamarin.iOS exemple](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) disponibles (qui est basé sur Apple [exemple de code de l’accessibilité](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) afin de détecter la disponibilité du réseau.


## <a name="webservices"></a>WebServices

Consultez la documentation sur [fonctionne avec les Services Web](~/cross-platform/data-cloud/web-services/index.md), qui couvre l’accès à d’autres, des points de terminaison SOAP et WCF à l’aide de Xamarin.iOS. Il est possible de demandes de service web et de la main et analyser les réponses, toutefois, il existe des bibliothèques simplifier cela, notamment Azure, RestSharp et ServiceStack. Opérations de base WCF sont accessibles dans des applications Xamarin.

### <a name="azure"></a>Azure

Microsoft Azure est une plateforme de cloud qui fournit un large éventail de services pour les applications mobiles, y compris le stockage des données et de synchronisation et de notifications push.

Visitez [azure.microsoft.com](https://azure.microsoft.com/) essayer gratuitement.

### <a name="restsharp"></a>RestSharp

RestSharp est une bibliothèque .NET qui peut être incluse dans les applications mobiles pour fournir un client REST qui simplifie l’accès aux services web. Il vous aide à en fournissant une API simple permettant de demander des données et d’analyser la réponse reste. RestSharp peut être utile

Le [site Web de RestSharp](http://restsharp.org/) contient [documentation](https://github.com/restsharp/RestSharp/wiki) comment implémenter un client REST à l’aide de RestSharp.
RestSharp fournit des exemples de Xamarin.iOS et Xamarin.Android sur [github](https://github.com/restsharp/RestSharp/).

Il existe également un extrait de code Xamarin.iOS dans notre [documentation des Services Web](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

Contrairement à RestSharp, ServiceStack est une solution de côté serveur pour héberger un service web ainsi que d’une bibliothèque cliente qui peut être implémentée dans les applications mobiles pour accéder à ces services.

Le [ServiceStack site](http://servicestack.net/) explique l’objectif du projet et des liens vers les exemples de code et de document. Les exemples incluent une implémentation de côté serveur complète d’un service web, ainsi que diverses applications côté client qui peuvent y accéder.

Il existe un [Xamarin.iOS exemple](http://www.servicestack.net/monotouch/remote-info/) sur le site Web ServiceStack et un extrait de code dans notre [documentation des Services Web](~/cross-platform/data-cloud/web-services/index.md).


### <a name="wcf"></a>WCF

Outils Xamarin peuvent vous aider à consommer des services Windows Communication Foundation (WCF). En règle générale, Xamarin prend en charge le même sous-ensemble de côté client de WCF est fourni avec le runtime Silverlight. Cela inclut les implémentations d’encodage et au protocole courantes de WCF : texte encodé de messages SOAP sur HTTP à l’aide du protocole de transport le `BasicHttpBinding`.

En raison de la taille et la complexité de l’infrastructure WCF, il peut y avoir des implémentations de service actuels et futurs seront exclues de l’étendue de prise en charge par domaine de sous-ensemble de client de Xamarin. En outre, la prise en charge de WCF requiert l’utilisation d’outils disponibles uniquement dans un environnement Windows pour générer le proxy.

 <a name="Threading" />


## <a name="threading"></a>Thread

Réactivité de l’application est importante pour les applications mobiles – les utilisateurs attendent des applications pour charger et exécuter plus rapidement. Écran « figé » qui cesse d’accepter l’entrée d’utilisateur s’affiche pour indiquer que l’application s’est arrêté anormalement, par conséquent, il est important de ne pas bloquer le thread d’interface utilisateur avec longue des appels bloquants tels que les requêtes réseau ou les opérations locales lentes (par exemple, pour décompresser un fichier). En particulier, le processus de démarrage ne doit pas contenir de tâches longues – toutes les plateformes mobiles va arrêter une application qui prend trop de temps à se charger.

Cela signifie que votre interface utilisateur doit implémenter un indicateur de progression ou l’interface utilisateur dans le cas contraire utilisable est rapide à afficher et des tâches asynchrones pour effectuer des opérations d’arrière-plan. L’exécution de tâches en arrière-plan requiert l’utilisation de threads, ce qui signifie que les besoins de tâches en arrière-plan pour un moyen de communiquer avec le thread principal pour indiquer la progression, ou quand ils ont terminé.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>Bibliothèque parallèle de tâches

Les tâches créées avec la bibliothèque parallèle de tâches peuvent exécuter de façon asynchrone et renvoyer sur leur thread appelant, ce qui les rend très utile pour déclencher des opérations de longue sans bloquer l’interface utilisateur.

Une opération simple tâche parallèle peut ressembler à ceci :

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

La clé est `TaskScheduler.FromCurrentSynchronizationContext()` qui réutilise le SynchronizationContext.Current du thread appelant la méthode (ici, le thread principal est en cours d’exécution `MainThreadMethod`) comme un moyen pour marshaler les appels de retour pour ce thread. Cela signifie que si la méthode est appelée sur le thread d’interface utilisateur, il s’exécutera la `ContinueWith` opération sur le thread d’interface utilisateur.

Si le code démarre les tâches des autres threads, utilisez le modèle suivant pour créer une référence vers le thread d’interface utilisateur et la tâche peut toujours rappeler à celui-ci :

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>Appel sur le Thread d’interface utilisateur

Pour le code qui n’utilise pas la bibliothèque parallèle de tâches, chaque plateforme possède sa propre syntaxe pour les opérations de marshaling vers le thread d’interface utilisateur :

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** – `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** : `Device.BeginInvokeOnMainThread(action)`
-  **Windows** : `Deployment.Current.Dispatcher.BeginInvoke(action)`



Les paramètres iOS et Android syntaxe requiert une classe de 'contexte' soit disponible, ce qui signifie que le code doit passer cet objet dans une méthode nécessitant un rappel sur le thread d’interface utilisateur.

Pour effectuer des appels de thread de l’interface utilisateur dans le code partagé, suivez les [IDispatchOnUIThread exemple](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (fourni par [ @follesoe ](http://jonas.follesoe.no/)). Déclarez et un programme pour un `IDispatchOnUIThread` d’interface dans le code partagé et ensuite implémenter les classes spécifiques à la plateforme, comme illustré ici :

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Les développeurs Xamarin.Forms doivent utiliser [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) dans le code commun (les projets partagés ou PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>Plateforme et les fonctionnalités de l’appareil et une dégradation des

Autres exemples spécifiques de traitement avec des fonctionnalités différentes figurent dans la documentation des fonctionnalités de la plateforme. Il porte sur la détection des fonctionnalités différentes et dégradation progressive une application de fournir une bonne expérience utilisateur, même lorsque l’application ne peut pas fonctionner à son maximum.
