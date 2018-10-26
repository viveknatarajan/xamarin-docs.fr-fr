---
title: Gestion de fichiers dans Xamarin.Forms
description: Gestion avec Xamarin.Forms de fichiers est possible à l’aide de code dans une bibliothèque .NET Standard, ou en utilisant des ressources incorporées.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: ddffb45b8cd8d47371e4ab57f30a467cea45b27d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117770"
---
# <a name="file-handling-in-xamarinforms"></a>Gestion de fichiers dans Xamarin.Forms

_Gestion avec Xamarin.Forms de fichiers est possible à l’aide de code dans une bibliothèque .NET Standard, ou en utilisant des ressources incorporées._

## <a name="overview"></a>Vue d'ensemble

Le code Xamarin.Forms s’exécute sur plusieurs plateformes, chacune ayant son propre système de fichiers. Auparavant, cela signifiait que que lire et écrire des fichiers plus facilement effectué à l’aide des API de fichier natif sur chaque plateforme. Vous pouvez également les ressources incorporées sont une solution plus simple pour distribuer des fichiers de données avec une application. Toutefois, avec .NET Standard 2.0, il est possible de partager du code d’accès de fichier dans les bibliothèques .NET Standard.

Pour plus d’informations sur la gestion des fichiers image, reportez-vous à la [utilisation des Images](~/xamarin-forms/user-interface/images.md) page.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Enregistrement et chargement de fichiers

Le `System.IO` classes peuvent être utilisées pour accéder au système de fichiers sur chaque plateforme. Le `File` classe vous permet de créer, supprimer et lire des fichiers et le `Directory` classe vous permet de créer, supprimer ou énumérer le contenu des répertoires. Vous pouvez également utiliser le `Stream` sous-classes, ce qui peuvent fournir un degré plus élevé de contrôle sur les opérations de fichier (par exemple, la compression ou la position de recherche au sein d’un fichier).

Un fichier texte peut être écrits en utilisant le `File.WriteAllText` méthode :

```csharp
File.WriteAllText(fileName, text);
```

Un fichier texte peut être lu à l’aide de la `File.ReadAllText` méthode :

```csharp
string text = File.ReadAllText(fileName);
```

En outre, le `File.Exists` méthode détermine si le fichier spécifié existe :

```csharp
bool doesExist = File.Exists(fileName);
```

Le chemin d’accès du fichier sur chaque plateforme peut être déterminé à partir d’une bibliothèque .NET Standard à l’aide d’une valeur de la [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) énumération en tant que premier argument de la `Environment.GetFolderPath` (méthode). Cela peut être combiné avec un nom de fichier avec le `Path.Combine` méthode :

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Ces opérations sont illustrées dans l’exemple d’application, qui inclut une page qui enregistre et charge le texte :

[![Enregistrement et chargement de texte](files-images/saveandload-sml.png "l’enregistrement et chargement de fichiers dans l’application")](files-images/saveandload.png#lightbox "l’enregistrement et chargement de fichiers dans l’application")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Le chargement des fichiers incorporés comme ressources

Pour incorporer un fichier dans un **.NET Standard** assembly, créer ou ajouter un fichier et vérifiez que **Action de génération : EmbeddedResource**.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Configuration incorporé action de génération ressource](files-images/vs-embeddedresource-sml.png "paramètre EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "paramètre EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Fichier texte incorporé dans la bibliothèque de classes portable, configuration d’action de génération de ressource incorporée](files-images/xs-embeddedresource-sml.png "paramètre EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "paramètre EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` est utilisé pour accéder au fichier incorporé en utilisant son **ID de ressource**. Par défaut, l’ID de ressource est préfixé avec l’espace de noms par défaut pour le projet, il est incorporé dans - le nom de fichier dans ce cas l’assembly est **WorkingWithFiles** et le nom de fichier est **PCLTextResource.txt**, Par conséquent, l’ID de ressource est `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

Le `text` variable peut ensuite être utilisée pour afficher le texte ou sinon l’utiliser dans le code. Cette capture d’écran de la [exemple d’application](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) affiche le texte restitué dans un `Label` contrôle.

 [![Fichier texte incorporé dans la bibliothèque de classes portable](files-images/pcltext-sml.png "fichier texte incorporé dans la bibliothèque de classes portable affiché dans l’application")](files-images/pcltext.png#lightbox "fichier texte incorporé dans la bibliothèque de classes portable affiché dans l’application")

Le chargement et la désérialisation d’un document XML sont tout aussi simple. Le code suivant montre un fichier XML qui est chargé et désérialisé à partir d’une ressource, puis lié à un `ListView` pour l’affichage. Le fichier XML contient un tableau de `Monkey` objets (la classe est définie dans l’exemple de code).

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

 [![Fichier XML incorporé dans la bibliothèque de classes portable, affiché dans ListView](files-images/pclxml-sml.png "fichier XML incorporé dans la bibliothèque de classes portable affichés dans ListView")](files-images/pclxml.png#lightbox "fichier XML incorporé dans la bibliothèque de classes portable affichés dans ListView")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>L’incorporation dans les projets partagés

Projets partagés peuvent également contenir des fichiers en tant que ressources incorporées, toutefois, étant donné que le contenu d’un projet partagé est compilé dans les projets de références, la ressource de fichier Qu'id peuvent changer incorporée dans le préfixe utilisé pour. Cela signifie que l’ID de ressource pour chaque fichier incorporé peut être différente pour chaque plateforme.

Il existe deux solutions à ce problème avec les projets partagés :

-  **Synchroniser les projets** -modifier les propriétés du projet pour chaque plateforme pour utiliser le **même** espace de noms par défaut et le nom d’assembly. Cette valeur peut ensuite être « codée en dur » comme préfixe de ressource incorporée ID dans le projet partagé.
-  **directives de compilateur #if** -utiliser des directives de compilateur pour définir le préfixe d’ID de ressource correct et utilise cette valeur pour construire de façon dynamique l’ID de ressource correct.


Code illustrant la deuxième option est indiqué ci-dessous. Directives de compilateur sont utilisés pour sélectionner le préfixe de ressource codé en dur (qui est normalement le même que l’espace de noms par défaut pour le projet de référence). Le `resourcePrefix` variable est ensuite utilisée pour créer un ID de ressource valide en concaténant le nom de fichier de ressource incorporée.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organisation des ressources

Les exemples ci-dessus supposent que le fichier est incorporé dans la racine du projet bibliothèque .NET Standard, auquel cas l’ID de ressource est au format **Namespace.Filename.Extension**, tel que `WorkingWithFiles.PCLTextResource.txt` et `WorkingWithFiles.iOS.SharedTextResource.txt`.

Il est possible organiser les ressources incorporées dans des dossiers. Lorsqu’une ressource incorporée est placée dans un dossier, le nom du dossier devient partie intégrante de l’ID de ressource (séparé par des points), afin que le format d’ID de ressource devient **Namespace.Folder.Filename.Extension**. Placer les fichiers utilisés dans l’exemple d’application dans un dossier **MyFolder** rendrait les ID de ressource correspondante `WorkingWithFiles.MyFolder.PCLTextResource.txt` et `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Débogage des ressources incorporées

Car il est parfois difficile de comprendre pourquoi une ressource particulière n’est pas en cours de chargement, le débogage code suivant peut être temporairement ajouté à une application pour vous aider à vérifier que les ressources sont correctement configurées. Il génère en sortie connues de toutes les ressources incorporées dans l’assembly donné à la **erreurs** pad pour aider à déboguer les problèmes de chargement des ressources.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>Récapitulatif

Cet article vous a montré certaines opérations de fichier simple pour enregistrer et charger le texte sur l’appareil et pour charger des ressources incorporées. Avec .NET Standard 2.0, il est possible de partager du code d’accès de fichier dans les bibliothèques .NET Standard.

## <a name="related-links"></a>Liens associés

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Utilisation avec le système de fichiers dans Xamarin.iOS](~/ios/app-fundamentals/file-system.md)

