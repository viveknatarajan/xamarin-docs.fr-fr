---
title: Visual Basic.NET dans Xamarin iOS et Android
description: Cette procédure pas à pas montre comment créer des applications natives Xamarin.iOS et Xamarin.Android qui utilisent une logique métier écrite en Visual Basic.NET.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 55c08c1bd0d82f8458e0c07b4202b6275600ff38
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671102"
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET dans Xamarin iOS et Android

Le [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) exemple d’application montre comment code Visual Basic compilé dans une bibliothèque de classes Portable peut être utilisé avec Xamarin. Voici quelques captures d’écran des applications qui en résulte en cours d’exécution sur iOS, Android et Windows Phone :

 [![](native-apps-images/image5.png "une application générée avec Visual Basic en cours d’exécution les téléphones iOS, Android et Windows")](native-apps-images/image5.png#lightbox)

IOS, Android et Windows Phone tous les projets dans l’exemple sont écrits dans C#. L’interface utilisateur pour chaque application est générée avec les technologies natives (Storyboards, Xml et Xaml, respectivement), tandis que le `TodoItem` management est fournie par la bibliothèque de classes Portable Visual Basic à l’aide un `IXmlStorage` implémentation fournie par le projet natif.

## <a name="sample-walkthrough"></a>Procédure pas à pas d'exemple

Ce guide décrit comment Visual Basic a été implémenté dans le [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) exemple Xamarin pour iOS et Android.

> [!NOTE]
> Passez en revue les instructions de [Portable Visual Basic.NET](index.md) avant de continuer avec ce guide.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Les bibliothèques de classes portables Visual Basic peuvent être créés uniquement dans Visual Studio.
L’exemple de bibliothèque contient les principes fondamentaux de notre application dans quatre fichiers Visual Basic :

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

Étant donné que les comportements d’accès de fichier varient considérablement entre les plateformes, les bibliothèques de classes portables ne fournissent pas `System.IO` API dans n’importe quel profil de stockage de fichiers. Cela signifie que si vous souhaitez interagir directement avec le système de fichiers dans notre code portable, nous devons effectuer un rappel vers notre projets natifs sur chaque plateforme.  En écrivant de notre code de Visual Basic par rapport à une interface simple qui peut être implémentée dans C# sur chaque plateforme, nous pouvons avoir partageable code Visual Basic qui a toujours accès au système de fichiers.

L’exemple de code utilise cette interface très simple qui contient seulement deux méthodes : pour lire et écrire un fichier Xml sérialisé.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android et Windows Phone des implémentations de cette interface seront affichera plus loin dans le guide.

### <a name="todoitemvb"></a>TodoItem.vb

Cette classe contient l’objet métier utilisé dans l’application. Il doit être définie dans Visual Basic et avec iOS, Android et Windows Phone de projets partagés qui sont écrits en C#.

La définition de classe est indiquée ici :

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

L’exemple utilise la sérialisation XML et la désérialisation pour charger et enregistrer les objets TodoItem.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

La classe de gestionnaire présente l’API pour le code portable. Il fournit des opérations CRUD de base pour la `TodoItem` classe, mais aucune implémentation de ces opérations.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

Le constructeur prend une instance de IXmlStorage en tant que paramètre. Ainsi, chaque plateforme pour fournir sa propre implémentation de travail tout en laissant le code portable décrivent d’autres fonctionnalités qui peuvent être partagées.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

La classe de référentiel contient la logique de gestion de la liste des objets TodoItem. Le code complet est indiqué ci-dessous, la logique existe principalement pour gérer une valeur d’ID unique sur les TodoItems comme ils sont ajoutés et supprimés de la collection.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> [!NOTE]
> Ce code est un exemple d’un mécanisme de stockage de données très basique.
> Il est fourni pour illustrer comment une bibliothèque de classes Portable utiliser pour coder une interface pour accéder aux fonctionnalités spécifiques à la plateforme (dans ce cas, le chargement et enregistrement d’un fichier Xml). Il elle pas destinée à être une alternative de base de données de qualité de production.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Android et les projets d’Application Windows Phone

Cette section contient les implémentations spécifiques à la plateforme pour l’interface IXmlStorage et montre comment il est utilisé dans chaque application. Les projets d’application sont écrits C#.

### <a name="ios-and-android-ixmlstorage"></a>iOS et Android IXmlStorage

Xamarin.iOS et Xamarin.Android fournissent complète `System.IO` fonctionnalités afin de pouvoir facilement charger et enregistrer le fichier Xml à l’aide de la classe suivante :

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

Dans l’application iOS le `TodoItemManager` et `XmlStorageImplementation` sont créés dans le **AppDelegate.cs** fichier comme indiqué dans cet extrait de code. Les quatre premières lignes sont à la construction le chemin d’accès au fichier dans lequel les données seront stockées ; les deux dernières lignes indiquent les deux classes en cours d’instanciation.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Dans l’application Android le `TodoItemManager` et `XmlStorageImplementation` sont créés dans le **Application.cs** fichier comme indiqué dans cet extrait de code. Les trois premières lignes sont à la construction le chemin d’accès au fichier dans lequel les données seront stockées ; les deux dernières lignes indiquent les deux classes en cours d’instanciation.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Le reste du code d’application est principalement concerné avec l’interface utilisateur et que vous utilisez le `TaskMgr` classe pour charger et enregistrer `TodoItem` classes.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone ne fournit pas de l’accès complet au système de fichiers de l’appareil, exposition à la place de l’API d’IsolatedStorage. Le `IXmlStorage` implémentation pour Windows Phone se présente comme suit :

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

Le `TodoItemManager` et `XmlStorageImplementation` sont créés dans le **App.xaml.cs** fichier comme indiqué dans cet extrait de code.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Le reste de l’application Windows Phone se compose de Xaml et C# pour créer l’interface utilisateur et utiliser le `TodoMgr` classe pour charger et enregistrer `TodoItem` objets.

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic PCL dans Visual Studio pour Mac

Visual Studio pour Mac ne prend pas en charge le langage Visual Basic : Impossible de créer ou de compiler les projets Visual Basic avec Visual Studio pour Mac.

Visual Studio pour la prise en charge de Mac pour les bibliothèques de classes portables signifie qu’il puisse référencer les assemblys de bibliothèque de classes portable qui ont été créés à partir de Visual Basic.

Cette section explique comment compiler un assembly de bibliothèque de classes portable dans Visual Studio, puis assurez-vous qu’il sera stocké dans un système de contrôle de version et référencé par d’autres projets.

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Conserver la sortie de la bibliothèque de classes portable à partir de Visual Studio

Par défaut, la plupart des systèmes de contrôle de version (y compris TFS et Git) sera configuré pour ignorer le **/bin/** directory, ce qui signifie que l’assembly compilé de la bibliothèque de classes portable n’est pas stocké. Cela signifie que vous devez copier manuellement dans tous les ordinateurs exécutant Visual Studio pour Mac ajouter une référence à celui-ci.

Pour garantir que votre système de contrôle de version peut stocker la sortie d’assembly de bibliothèque de classes portable, vous pouvez créer un script de post-build qui copie dans la racine du projet. Cette étape post-build permet de vous assurer de l’assembly peut être facilement ajouté au contrôle de code source et partagée avec d’autres projets.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Avec le bouton droit sur le projet et choisissez le **Propriétés > événements de Build** section.

2. Ajouter un _post-build_ script copie la DLL de sortie à partir de ce projet dans le répertoire racine du projet (qui est en dehors de **/bin/**). Selon votre configuration de contrôle de version, la DLL doit maintenant être en mesure d’être ajouté au contrôle de code source.

  [![](native-apps-images/image6-vs-sml.png "Événements de build post script de build pour copier la DLL VB")](native-apps-images/image6-vs.png#lightbox)

#### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Avec le bouton droit sur le projet et choisissez **Propriétés > Compiler** , puis vérifiez toutes les Configurations est sélectionnée dans la zone de peigne en haut à gauche. Cliquez sur le **événements de Build...**  situé en bas à droite.

    [![](native-apps-images/image6.png "La section de compilation de propriétés de projet")](native-apps-images/image6.png#lightbox)

1.  Ajouter un script de post-build qui copie la DLL de sortie à partir de ce projet dans le répertoire racine du projet (qui est en dehors de **/bin/** ). Selon votre configuration de contrôle de version, la DLL doit maintenant être en mesure d’être ajouté au contrôle de code source.

    [![](native-apps-images/image7.png "Fenêtre des événements de build")](native-apps-images/image7.png#lightbox)

#### <a name="all-versions"></a>Toutes les Versions

Prochaine fois que vous générez le projet, l’assembly de bibliothèque de classes Portable est copié à la racine du projet, et lorsque vous vérifiez dans/validation/push vos modifications de la DLL sera stocké (afin qu’il peut être téléchargé sur un Mac avec Visual Studio pour Mac).

  [![](native-apps-images/image8-sml.png "Emplacement du fichier d’assembly de base Visual de sortie")](native-apps-images/image8.png#lightbox)


Cet assembly peut ensuite être ajouté aux projets Xamarin dans Visual Studio pour Mac, même si le langage Visual Basic lui-même n’est pas pris en charge dans Xamarin iOS ou Android projets.

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Faisant référence à la bibliothèque de classes portable dans Visual Studio pour Mac

Étant donné que Xamarin ne prend pas en charge Visual Basic Il est impossible de charger le projet de bibliothèque de classes portable (ni l’application Windows Phone) comme indiqué dans cette capture d’écran :

 [![](native-apps-images/image9.png "Visual Studio pour la solution de Mac")](native-apps-images/image9.png#lightbox)

Nous pouvons toujours inclure le DLL d’assembly de bibliothèque de classes portable Visual Basic dans les projets Xamarin.iOS et Xamarin.Android :

1.  Avec le bouton droit sur le **références** nœud et sélectionnez **modifier les références...**

    [![](native-apps-images/image10.png "Menu de références de projet Edition")](native-apps-images/image10.png#lightbox)

1.  Sélectionnez le **.Net Assembly** onglet et accédez à la DLL de sortie dans le répertoire du projet Visual Basic. Même si Visual Studio pour Mac ne peut pas ouvrir le projet, tous les fichiers doivent être il à partir du contrôle de code source. Cliquez sur **ajouter** puis **OK** pour ajouter cet assembly pour les applications iOS et Android.

    [![](native-apps-images/image11-sml.png "Cliquez sur Ajouter puis sur OK pour ajouter cet assembly pour les applications iOS et Android")](native-apps-images/image11.png#lightbox)

1.  Les applications iOS et Android peuvent maintenant inclure la logique d’application fournie par la bibliothèque de classes Portable de Visual Basic. Cette capture d’écran montre une application iOS qui fait référence à la bibliothèque PCL Visual Basic et comporte du code qui utilise la fonctionnalité à partir de cette bibliothèque.

    [![](native-apps-images/image12-sml.png "Modifier les références d’ajouter la fenêtre d’assembly .NET")](native-apps-images/image12.png#lightbox)


Si des modifications sont apportées au projet Visual Basic dans Visual Studio, n’oubliez pas de générer le projet, la DLL d’assembly résultant dans le contrôle de code source et ensuite extraire ce nouveau fichier DLL du contrôle de source sur votre Mac afin que Visual Studio pour Mac génère contiennent la dernière version fonctionnalité.


## <a name="summary"></a>Récapitulatif

Cet article a montré comment utiliser le code Visual Basic dans les applications Xamarin à l’aide de Visual Studio et les bibliothèques de classes portables. Même si Xamarin ne prend pas en charge directement les Visual Basic, compilation Visual Basic dans une bibliothèque de classes portable permet au code écrit avec Visual Basic à inclure dans les applications iOS et Android.

## <a name="related-links"></a>Liens associés

- [TaskyPortableVB (exemple)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [Le développement multiplateforme avec le Kit de développement .NET Framework (Microsoft)](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
