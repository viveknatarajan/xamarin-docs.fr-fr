---
title: Fichiers
description: Gestion avec Xamarin.Forms de fichiers peuvent s’effectuer à l’aide des ressources incorporées ou de l’écriture sur le système de fichiers natif API.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 8315f1a0056c6a6f084ebfe2c29f0c0c2bb30330
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="files"></a>Fichiers

_Gestion avec Xamarin.Forms de fichiers peuvent s’effectuer à l’aide des ressources incorporées ou de l’écriture sur le système de fichiers natif API._

## <a name="overview"></a>Vue d'ensemble

Le code Xamarin.Forms s’exécute sur plusieurs plateformes, chacune ayant son propre système de fichiers. Cela signifie que la lecture et écriture de fichiers sont le plus facilement effectuée à l’aide de l’API de fichier natif sur chaque plateforme. Vous pouvez également les ressources incorporées sont une solution plus simple pour distribuer des fichiers de données avec une application.

Ce document traite le fichier suivant courantes, la gestion des scénarios :

-  [ **Fichiers incorporés comme ressources** ](#Loading_Files_Embedded_as_Resources) -fichiers peuvent être envoyés en tant que partie d’une application et chargés à l’aide de l’API de réflexion.
-  [ **Enregistrement et chargement de fichiers** ](#Loading_and_Saving_Files) -stockage accessible en écriture utilisateur peut être implémentées en mode natif et ensuite accessibles à l’aide de la `DependencyService` .


Un appel de composant tiers **PCLStorage** peut également être utilisé pour lire et écrire des fichiers de stockage accessibles utilisateur à partir du code de bibliothèque de classes portables.

Pour plus d’informations sur la gestion des fichiers image, reportez-vous à la [utilisation des Images](~/xamarin-forms/user-interface/images.md) page.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Le chargement des fichiers incorporés comme ressources

Pour incorporer un fichier dans un **PCL** assembly, créer ou ajouter un fichier et vérifiez que **Action de génération : EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Configuration incorporé action de génération ressource](files-images/vs-embeddedresource-sml.png "paramètre EmbeddedResource Buiidaction")](files-images/vs-embeddedresource.png#lightbox "paramètre EmbeddedResource génération")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Fichier texte incorporé dans la bibliothèque PCL, configuration de ressource incorporée build action](files-images/xs-embeddedresource-sml.png "paramètre EmbeddedResource Buiidaction")](files-images/xs-embeddedresource.png#lightbox "paramètre EmbeddedResource génération")

-----

`GetManifestResourceStream` est utilisé pour accéder au fichier incorporé via son **ID de ressource**. Par défaut, l’ID de ressource est précédé de l’espace de noms par défaut pour le projet, il est incorporé dans - le nom de fichier dans ce cas l’assembly est **WorkingWithFiles** et le nom de fichier est **PCLTextResource.txt**, Par conséquent, l’ID de ressource est `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

Le `text` peut ensuite être utilisée pour afficher le texte ou sinon l’utiliser dans le code. Cette capture d’écran de la [exemple d’application](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) affiche le texte affiché dans un `Label` contrôle.

 [![Fichier texte incorporé dans la bibliothèque de classes portables](files-images/pcltext-sml.png "fichier texte incorporé dans la bibliothèque de classes portables affichés dans l’application")](files-images/pcltext.png#lightbox "fichier texte incorporé dans la bibliothèque de classes portables affichés dans l’application")

Le chargement et la désérialisation d’un document XML sont tout aussi simple. Le code suivant illustre un fichier XML qui est chargé et désérialisé à partir d’une ressource, puis lié à un `ListView` pour l’affichage. Le fichier XML contient un tableau de `Monkey` objets (la classe est définie dans l’exemple de code).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![Fichier XML incorporé dans la bibliothèque PCL, affiché dans ListView](files-images/pclxml-sml.png "fichier XML incorporé dans la bibliothèque de classes portables affichés dans ListView")](files-images/pclxml.png#lightbox "fichier XML incorporé dans la bibliothèque de classes portables affichés dans ListView")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Incorporer dans les projets partagés

Les projets partagés peuvent également contenir des fichiers en tant que ressources incorporées, toutefois, étant donné que le contenu d’un projet partagé est compilé dans les projets de référence, le préfixe utilisé pour incorporé peuvent modifier les ID de ressource de fichier. Cela signifie que l’ID de ressource pour chaque fichier incorporé peut être différent pour chaque plateforme.

Il existe deux solutions à ce problème avec les projets partagés :

-  **Synchroniser les projets** -modifier les propriétés du projet pour chaque plateforme utiliser le **même** espace de noms par défaut et le nom d’assembly. Cette valeur peut ensuite être « codé en dur » comme préfixe de la ressource incorporée ID dans le projet partagé.
-  **directives de compilateur #if** -utilisez les directives du compilateur pour le préfixe d’ID de ressource correct et utiliser cette valeur pour construire dynamiquement l’ID de ressource correct.


Code qui illustre la deuxième option est indiqué ci-dessous. Directives de compilateur utilisés pour sélectionner le préfixe de ressource codé en dur (qui est normalement le même que l’espace de noms par défaut pour le projet de référence). Le `resourcePrefix` est ensuite utilisée pour créer un ID de ressource valide en concaténant avec le nom de fichier ressource incorporée.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organisation des ressources

Les exemples ci-dessus supposent que le fichier est incorporé dans la racine du projet de bibliothèque PCL, auquel cas l’ID de ressource est sous la forme **Namespace.Filename.Extension**, tel que `WorkingWithFiles.PCLTextResource.txt` et `WorkingWithFiles.iOS.SharedTextResource.txt`.

Il est possible d’organiser les ressources incorporées dans des dossiers. Lorsqu’une ressource incorporée est placée dans un dossier, le nom du dossier devient partie intégrante de l’ID de ressource (séparé par des points), afin que le format d’ID de ressource devient **Namespace.Folder.Filename.Extension**. Placer les fichiers utilisés dans l’exemple d’application dans un dossier **MonDossier** rendrait les ID de ressources correspondant `WorkingWithFiles.MyFolder.PCLTextResource.txt` et `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Débogage des ressources incorporées

Comme il est parfois difficile à comprendre pourquoi une ressource particulière n’est pas en cours de chargement, le débogage code suivant peut être ajouté temporairement à une application afin de vérifier que les ressources sont correctement configurés. Il ne produira pas connus de toutes les ressources incorporées dans l’assembly donné à la **erreurs** remplissage pour déboguer les problèmes de chargement de la ressource.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Enregistrement et chargement de fichiers

Parce que Xamarin.Forms s’exécute sur plusieurs plateformes, chacun avec son propre système de fichiers, il n’existe aucune approche unique pour charger et enregistrer les fichiers créés par l’utilisateur. Pour illustrer comment enregistrer et charger des fichiers texte que l’exemple d’application inclut un écran qui enregistre et charge une entrée utilisateur - l’écran terminé est indiqué ci-dessous :

 [![Enregistrement et chargement de texte](files-images/saveandload-sml.png "l’enregistrement et chargement de fichiers dans l’application")](files-images/saveandload.png#lightbox "l’enregistrement et chargement de fichiers dans l’application")

Chaque plateforme possède une structure de répertoire légèrement différente et des fonctionnalités de système de fichiers différent, par exemple Xamarin.iOS et Xamarin.Android prennent en charge la plupart `System.IO` fonctionnalité, mais Windows Phone prend uniquement en charge `IsolatedStorage` et [ `Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) API.

Pour contourner ce problème, l’exemple d’application définit une Interface dans la bibliothèque PCL de Xamarin.Forms pour charger et enregistrer des fichiers. Il fournit une API simple pour charger et enregistrer des fichiers texte qui est stockée sur l’appareil.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

La bibliothèque PCL code puis utilise le [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) pour obtenir une référence à une implémentation native de l’interface. Ainsi, le code portable déléguer le chargement et l’enregistrement de fichiers à une classe écrite dans chacun des projets spécifiques à la plateforme. Dans l’exemple, le **enregistrer** et **charge** boutons sont écrites comme indiqué :

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

Une implémentation puis doit être ajouté à chacun des projets spécifiques à une plateforme avant que les fichiers réellement peuvent être enregistrés et chargés.

### <a name="ios-and-android"></a>iOS et Android

L’implémentation de l’interface pour les projets Xamarin.iOS et Xamarin.Android peut être identique. Le code indiqué ci-dessous, y compris le `[assembly: Dependency (typeof (SaveAndLoad))]` attribut qui est nécessaire pour le `DependencyService` pour fonctionner.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>Plateforme Windows universelle (UWP), Windows 8.1 et Windows Phone 8.1

Ces plates-formes ont un système de fichiers différentes API – [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) : qui est utilisé pour enregistrer et charger des fichiers.
Le `ISaveAndLoad` interface peut être implémentée comme indiqué ci-dessous :

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>Enregistrement et chargement de projets partagés

Car les projets partagés prend en charge les directives de compilateur, il est possible d’inclure tout le code spécifique à la plateforme dans un seul fichier de classe situé dans le projet partagé (sans utiliser le `DependencyService`).
Un seul `SaveAndLoad` classe peut être écrite qui contient les deux implémentations ci-dessus, sélectivement compilés dans les projets de référence à l’aide de directives de compilateur comme `#if WINDOWS_PHONE`, `#if __IOS__`, et `#if __ANDROID__`.

## <a name="additional-information"></a>Informations supplémentaires

Projets bibliothèque de classes portables Xamarin.Forms peuvent également tirer parti de la [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([code &amp; documentation](https://pclstorage.codeplex.com/)) pour aider à implémenter des opérations de fichier d’une manière inter-plateformes.


## <a name="summary"></a>Récapitulatif

Ce document montre certaines opérations de fichier simple pour le chargement des ressources incorporées et l’enregistrement et chargement de texte sur l’appareil. Les développeurs peuvent implémenter leur propre API de fichier natif à l’aide de la `DependencyService`, rendant aussi complexe que nécessaire pour gérer leurs exigences de manipulation de fichier.


## <a name="related-links"></a>Liens associés

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Créer, écrire et lire un fichier (UWP)](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Classeur de fichiers](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
