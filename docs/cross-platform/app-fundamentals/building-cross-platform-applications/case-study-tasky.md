---
title: 'Application multiplateforme Case Study : Tasky'
description: Ce document décrit comment l’exemple d’application Tasky Portable a été conçu et généré sous la forme d’une application mobile multiplateforme. Elle décrit l’application Configuration requise, interface, modèle de données, les fonctionnalités principales, implémentation et bien plus encore.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 48650445d06ad3bc7ca6d4da84c9b8837f8a0f88
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782233"
---
# <a name="cross-platform-app-case-study-tasky"></a>Application multiplateforme Case Study : Tasky

*Tasky* *Portable* est une application de la liste des tâches simples. Ce document décrit la façon dont il a été conçu et créé, en suivant les instructions de la [génération d’Applications inter-plateformes](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) document. La discussion couvre les domaines suivants :

<a name="Design_Process" />

## <a name="design-process"></a>Processus de conception

Il est conseillé de créer un de route pour ce que vous souhaitez atteindre avant de commencer à coder. Cela est particulièrement vrai pour le développement multiplateforme, où vous générez les fonctionnalités qui seront exposées de plusieurs façons. Commencer par une idée précise de ce que vous créez gagner du temps et efforts ultérieurement dans le cycle de développement.

 <a name="Requirements" />

### <a name="requirements"></a>Configuration requise

La première étape de la conception d’une application consiste à identifier les fonctionnalités souhaitées. Il peut s’agir des objectifs de niveau supérieur ou détaillées des cas d’usage. Tasky a des exigences fonctionnelles simples :

 -  Afficher la liste des tâches
 -  Ajouter, modifier et supprimer des tâches
 -  Définir le statut d’une tâche à 'done'

Vous devez envisager l’utilisation des fonctionnalités spécifiques à la plateforme.  Peut Tasky profiter de géorepérage d’iOS ou Windows Phone des vignettes dynamiques ? Même si vous n’utilisez pas les fonctionnalités spécifiques à la plateforme dans la première version, vous devez planifier pour vous assurer que votre entreprise et les couches de données peuvent prendre en charge les.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>Conception de l'interface utilisateur

Démarrez avec une conception de haut niveau qui peut être implémentée sur les plateformes cibles. Veiller aux contraintes d’interface utilisateur Remarque spécifique à la plateforme. Par exemple, un `TabBarController` dans iOS peuvent afficher plus de cinq boutons, tandis que l’équivalent de Windows Phone peut afficher jusqu'à quatre.
Dessinez le flux d’écran à l’aide de l’outil de votre choix (livre s’applique).

 [![](case-study-tasky-images/taskydesign.png "Tracer le flux d’écran à l’aide de l’outil de vos travaux de papier de choix")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>Modèle de données

Le fait de savoir quelles données doivent être stockées pour déterminer quel mécanisme de persistance à utiliser. Consultez [accès aux données de Cross-Platform](~/cross-platform/app-fundamentals/index.md) pour plus d’informations sur les mécanismes de stockage disponible et à vos besoins entre eux. Pour ce projet, nous utiliserons SQLite.NET.

Tasky doit stocker les trois propriétés pour chaque « TaskItem » :

 -  **Nom** : chaîne
 -  **Notes** : chaîne
 -  **Fait** : booléen

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>Fonctionnalité de base

Envisagez de l’API de l’interface utilisateur doit consommer pour satisfaire les exigences. Une liste de tâches nécessite les fonctions suivantes :

 -   **Répertorier toutes les tâches** : permet d’afficher la liste de l’écran principal de toutes les tâches disponibles
 -  **Obtenir une tâche** – lorsqu’une ligne de tâche est affectée.
 -  **Enregistrer une tâche** : lorsqu’une tâche est modifiée.
 -  **Supprimer une tâche** : lors de la suppression d’une tâche
 -  **Créer une tâche vide** – lorsqu’une tâche est créée.

Pour obtenir la réutilisation de code, cette API doit être implémentée qu’une seule fois dans le *bibliothèque de classes portables*.

 <a name="Implementation" />

### <a name="implementation"></a>Implémentation

Une fois la conception de l’application a été acceptée, pensez comment elle peut être implémentée comme une application multiplateforme. Cela devient l’architecture de l’application. Suivant les instructions fournies dans le [génération d’Applications inter-plateformes](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) document, le code d’application doit être rompu vers le bas dans les sections suivantes :

 -   **Le Code commun de** – un projet commun qui contient le code réutilisables pour stocker les données de tâche qui expose une classe de modèle et une API pour gérer l’enregistrement et le chargement des données.
 -   **Code spécifique à la plateforme** – projets spécifiques à une plateforme qui implémentent une interface utilisateur native pour chaque système d’exploitation, en utilisant le code commun comme serveur principal.

 [![](case-study-tasky-images/taskypro-architecture.png "Projets spécifiques à la plateforme implémentent une interface utilisateur native pour chaque système d’exploitation, en utilisant le code commun en tant que le serveur principal")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Ces deux parties sont décrits dans les sections suivantes.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>Code commun de (PCL)

Tasky Portable utilise la stratégie de la bibliothèque de classes portables pour le partage de code. Consultez le [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) document pour obtenir une description des options de partage de code.

Tout le code commun, y compris la couche d’accès aux données, le code de base de données et des contrats, est placé dans le projet de bibliothèque.

Le projet bibliothèque de classes portables est illustré ci-dessous. Tout le code dans la bibliothèque portable est compatible avec chaque plateforme ciblée. Lors du déploiement, chaque application native fait référence cette bibliothèque.

![](case-study-tasky-images/portable-project.png "Lors du déploiement, chaque application native fait référence à cette bibliothèque")

Le diagramme de classes ci-dessous montre les classes regroupées par couche. La `SQLiteConnection` classe est un code réutilisable à partir du package de Sqlite-NET. Le reste des classes sont le code personnalisé pour Tasky. Le `TaskItemManager` et `TaskItem` classes représentent l’API est exposée aux applications spécifiques à la plateforme.

 [![](case-study-tasky-images/classdiagram-core.png "Les classes TaskItemManager et TaskItem représentent l’API est exposée aux applications spécifiques à la plateforme")](case-study-tasky-images/classdiagram-core.png#lightbox)

À l’aide des espaces de noms pour séparer les couches permet de gérer les références entre chaque couche. Les projets spécifiques à la plateforme ne doit inclure un `using` instruction pour la couche métier. La couche d’accès aux données et la couche de données doivent être encapsulé par l’API qui est exposé par `TaskItemManager` dans la couche métier.

 <a name="References" />

### <a name="references"></a>Références

Bibliothèques de classes portables doivent être utilisables sur plusieurs plateformes, chacun avec des niveaux de prise en charge des fonctionnalités de plateforme et le framework. Par conséquent, des limitations sont sur lequel les packages et bibliothèques framework peuvent être utilisés. Par exemple, Xamarin.iOS ne prend pas en charge du langage c# `dynamic` (mot clé), une bibliothèque de classes portables ne pouvez pas utiliser n’importe quel package dépend de code dynamique, même si un tel code fonctionne sur Android. Visual Studio pour Mac vous empêche d’ajouter des packages incompatibles et des références, mais que vous souhaitez conserver les limitations à l’esprit afin d’éviter les mauvaises surprises ultérieurement.

Remarque : Vous verrez que vos projets de référencent les bibliothèques de framework que vous avez utilisé. Ces références sont inclus dans le cadre des modèles de projet Xamarin. Lorsque les applications sont compilées, le processus de liaison supprime code non référencé, par conséquent, même si `System.Xml` a été référencée, elle ne pas être inclus dans l’application finale, car nous n’utilisons pas toutes les fonctions Xml.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>Couche de données (DL)

La couche données contient le code qui effectue le stockage physique de données : si pour une base de données, fichiers plats ou un autre mécanisme. La couche données Tasky se compose de deux parties : la bibliothèque de SQLite-NET et le code personnalisé est ajouté à le connecter.

Tasky s’appuie sur le package nuget de Sqlite-net (publié par Frank Kreuger) pour incorporer le code de SQLite-NET qui fournit une interface de base de données de mappage relationnel objet (ORM). Le `TaskItemDatabase` hérite de la classe `SQLiteConnection` et ajoute le requise création, lecture, mise à jour, les méthodes de suppression (CRUD) pour lire et écrire des données dans SQLite. C’est une implémentation simple réutilisable des méthodes génériques CRUD qui peuvent être réutilisées dans d’autres projets.

Le `TaskItemDatabase` est un singleton, garantissant que tous les accès portant sur la même instance. Un verrou est utilisé pour empêcher l’accès simultané de plusieurs threads.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>SQLite sur Windows Phone

Lors de l’iOS et Android sont fournis avec SQLite en tant que partie du système d’exploitation, Windows Phone n’inclut pas d’un moteur de base de données compatible. Pour partager le code entre les trois plateformes, une version de téléphone en mode natif de Windows de SQLite est requise. Consultez [fonctionne avec une base de données locale](~/xamarin-forms/app-fundamentals/databases.md) pour plus d’informations sur la configuration de votre projet Windows Phone pour Sqlite.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>À l’aide d’une Interface pour généraliser l’accès aux données

La couche données prennent une dépendance sur `BL.Contracts.IBusinessIdentity` afin qu’elle peut implémenter des méthodes d’accès de données abstrait qui nécessitent une clé primaire. Toute classe de la couche métier qui implémente l’interface peut ensuite être conservé dans la couche données.

L’interface spécifie simplement une propriété entière en tant que la clé primaire :

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

La classe de base implémente l’interface et ajoute les attributs de SQLite-NET pour la marquer comme étant une clé primaire auto-incrémentée. N’importe quelle classe de la couche métier qui implémente cette classe de base peut ensuite être conservée dans la couche données :

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

S’agit-il d’un exemple de méthodes génériques dans la couche données qui utilisent l’interface `GetItem<T>` méthode :

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>Le verrouillage pour empêcher les accès simultanés

A [verrou](http://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) est implémenté dans le `TaskItemDatabase` classe pour empêcher l’accès simultané à la base de données. Il s’agit pour vous assurer que les accès simultanés à partir de différents threads sont sérialisé (un composant d’interface utilisateur peut essayer de lire la base de données en même temps un thread d’arrière-plan est la mise à jour). Un exemple de la façon dont le verrou est implémenté est illustré ici :

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

La plupart du code de couche données peut être réutilisée dans d’autres projets. Le code d’application spécifique uniquement dans la couche est la `CreateTable<TaskItem>` appeler dans le `TaskItemDatabase` constructeur.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>Couche d’accès aux données (DAL)

Le `TaskItemRepository` classe encapsule le mécanisme de stockage de données avec une API fortement typée qui permet `TaskItem` objets doit être créé, supprimé, récupérés et mis à jour.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>À l’aide de la Compilation conditionnelle

La classe utilise la compilation conditionnelle pour définir l’emplacement du fichier - il s’agit d’un exemple d’implémentation de la Divergence de plateforme. La propriété qui retourne le chemin d’accès est compilé en code différent sur chaque plateforme. Le code et les directives de compilateur de plateforme spécifique sont illustrés ici :

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

Selon la plateforme, le résultat sera «<app
path>/Library/TaskDB.db3 » pour iOS, «<app
path>/Documents/TaskDB.db3 » pour Android ou simplement « TaskDB.db3 » pour Windows Phone.

### <a name="business-layer-bl"></a>Couche d’entreprise (BL)

La couche métier implémente les classes du modèle et une Façade pour pouvoir les gérer.
Dans Tasky le modèle est le `TaskItem` classe et `TaskItemManager` implémente le modèle de Façade pour fournir une API de gestion `TaskItems`.

 <a name="Façade" />

#### <a name="faade"></a>Façade

 `TaskItemManager` encapsule le `DAL.TaskItemRepository` pour fournir la méthode Get, enregistrer et supprimer des méthodes qui seront référencés par l’Application et les couches d’interface utilisateur.

Logique et les règles d’entreprise sont placées ici si nécessaire, par exemple les règles de validation qui doivent être satisfaites avant l’enregistrée d’un objet.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>API pour le Code spécifique à la plateforme

Une fois que le code commun a été écrit, l’interface utilisateur doit être généré pour collecter et afficher les données exposées par celui-ci. La `TaskItemManager` classe implémente le modèle de Façade pour fournir une API simple pour le code d’application à accéder.

Le code écrit dans chaque projet spécifique à la plateforme sont généralement étroitement couplés pour le Kit de développement natif de ce périphérique et d’accès seulement le code commun à l’aide de l’API définie par le `TaskItemManager`. Cela inclut les méthodes et entreprise classes il expose, telles que `TaskItem`.

Les images ne sont pas partagés entre les plateformes mais ajoutés de manière indépendante pour chaque projet. Ceci est important, car chaque plate-forme gère les images différemment, à l’aide des noms de fichiers différents, des répertoires et des résolutions.

Les sections suivantes décrivent les détails d’implémentation de la plateforme spécifique de l’interface utilisateur Tasky.

 <a name="iOS_App" />

## <a name="ios-app"></a>Application iOS

Il n’y a que quelques classes nécessaires pour implémenter l’application iOS application Tasky à l’aide du projet de bibliothèque de classes portables commun pour stocker et récupérer des données. Le projet de Xamarin.iOS iOS complet est indiqué ci-dessous :

 ![](case-study-tasky-images/taskyios-solution.png "projet iOS est illustrée ici")

Les classes sont répertoriées dans ce diagramme, regroupé en couches.

 [![](case-study-tasky-images/classdiagram-android.png "Les classes sont répertoriées dans ce diagramme, regroupé en couches")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Références

L’application iOS fait référence aux bibliothèques de kit de développement logiciel spécifique à la plateforme – par exemple. Xamarin.iOS et MonoTouch.Dialog-1.

Il doit également faire référence à la `TaskyPortableLibrary` projet de bibliothèque PCL.
La liste des références est illustrée ici :

 ![](case-study-tasky-images/taskyios-references.png "La liste des références est illustrée ici")

La couche Application et la couche d’Interface utilisateur sont implémentées dans ce projet à l’aide de ces références.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Couche d’application (AL)

La couche d’Application contient des classes spécifiques à la plateforme nécessaires pour les objets exposés par la bibliothèque PCL à l’interface utilisateur de 'bind'. L’application iOS spécifiques dispose de deux classes pour permettre l’affichage des tâches :

 -   **EditingSource** : cette classe est utilisée pour lier des listes de tâches à l’interface utilisateur. Étant donné que `MonoTouch.Dialog` a été utilisé pour la liste des tâches, nous devons implémenter ce programme d’assistance pour activer la fonctionnalité de balayage à supprimer dans le `UITableView` . Effectuez un balayage à supprimer est courant sur iOS, mais pas sur Android ou Windows Phone, par conséquent, le projet spécifique iOS est le seul qui l’implémente.
 -   **TaskDialog** : cette classe est utilisée pour lier une seule tâche à l’interface utilisateur. Elle utilise le `MonoTouch.Dialog` API de réflexion pour « encapsuler » le `TaskItem` objet avec une classe qui contient les attributs appropriés pour permettre à l’écran d’entrée mise en forme correctement.

Le `TaskDialog` classe utilise `MonoTouch.Dialog` attributs pour créer un écran basé sur les propriétés d’une classe. La classe ressemble à ceci :

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

Notez le `OnTap` attributs nécessitent un nom de méthode : de ces méthodes doivent exister dans la classe où le `MonoTouch.Dialog.BindingContext` est créé (dans ce cas, la `HomeScreen` classe décrite dans la section suivante).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>Couche d’Interface utilisateur (IU)

La couche d’Interface utilisateur se compose des classes suivantes :

1.   **AppDelegate** – contient des appels à l’API de l’apparence de style les polices et les couleurs utilisées dans l’application. Tasky est une application simple et présentent donc pas d’autres tâches d’initialisation en cours d’exécution `FinishedLaunching` .
2.   **Écrans** – sous-classes de `UIViewController` qui définissent chaque écran et son comportement. Écrans relier l’interface utilisateur avec les classes de la couche Application et l’API commune ( `TaskItemManager` ). Dans cet exemple, les écrans sont créés dans le code, mais elles pourraient ont été conçues à l’aide Interface Builder de Xcode ou le Concepteur de plan conceptuel.
3.   **Images** – éléments visuels sont une partie importante de chaque application. Tasky a démarrage écran et l’icône des images, qui pour iOS doivent être fournis en Normal et la résolution rétine.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Écran d’accueil

L’écran d’accueil est un `MonoTouch.Dialog` écran qui affiche une liste de tâches à partir de la base de données SQLite. Il hérite de `DialogViewController` et implémente le code pour définir le `Root` contenir une collection de `TaskItem` objets pour l’affichage.

 [![](case-study-tasky-images/ios-taskylist.png "Il hérite de DialogViewController et implémente le code permettant de définir la racine pour contenir une collection d’objets TaskItem pour l’affichage")](case-study-tasky-images/ios-taskylist.png#lightbox)

Les deux méthodes principales liées à afficher et interagir avec la liste des tâches sont :

1.   **PopulateTable** – utilise la couche de métier `TaskManager.GetTasks` méthode pour récupérer une collection de `TaskItem` objets à afficher.
2.   **Sélectionné** – lorsqu’une ligne est couvertes, affiche la tâche dans un nouvel écran.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Écran de détails de tâche

Détails de la tâche est un écran d’entrée qui permet aux tâches d’être modifiées ni supprimées.

Utilise tasky `MonoTouch.Dialog`d’API de réflexion pour afficher l’écran, il est donc aucune `UIViewController` implémentation. Au lieu de cela, le `HomeScreen` classe instancie et affiche un `DialogViewController` à l’aide de la `TaskDialog` classe à partir de la couche Application.

Cette capture d’écran montre un écran vide qui montre la `Entry` attribut à définir le texte de filigrane dans le **nom** et **Notes** champs :

 [![](case-study-tasky-images/ios-taskydetail.png "Cette capture d’écran montre un écran vide qui illustre la définition du texte de filigrane dans les champs nom et les Notes de l’attribut d’entrée")](case-study-tasky-images/ios-taskydetail.png#lightbox)

Les fonctionnalités de la **détails de la tâche** écran (par exemple, l’enregistrement ou la suppression d’une tâche) doit être implémenté dans le `HomeScreen` classe, car c’est là la `MonoTouch.Dialog.BindingContext` est créé. Les éléments suivants `HomeScreen` méthodes prennent en charge de l’écran de détails de la tâche :

1.   **ShowTaskDetails** – crée un `MonoTouch.Dialog.BindingContext` pour afficher un écran. Il crée l’écran d’entrée à l’aide de la réflexion pour récupérer les noms de propriétés et les types à partir de la `TaskDialog` classe. Des informations supplémentaires, telles que le texte de filigrane pour les zones d’entrée, sont implémentées avec des attributs sur les propriétés.
2.   **SaveTask** – cette méthode est référencée dans le `TaskDialog` classe un `OnTap` attribut. Elle est appelée lorsque **enregistrer** est enfoncée et utilise un `MonoTouch.Dialog.BindingContext` pour récupérer les données entrées par l’utilisateur avant d’enregistrer les modifications apportées à l’aide de `TaskItemManager` .
3.   **DeleteTask** – cette méthode est référencée dans le `TaskDialog` classe un `OnTap` attribut. Il utilise `TaskItemManager` pour supprimer les données à l’aide de la clé primaire (propriété d’ID).

 <a name="Android_App" />

## <a name="android-app"></a>Application Android

Le projet Xamarin.Android est illustré ci-dessous :

 ![](case-study-tasky-images/taskyandroid-solution.png "Projet Android est illustrée ici")

Le diagramme de classes, avec les classes regroupées par couche :

 [![](case-study-tasky-images/classdiagram-android.png "Le diagramme de classes, avec les classes regroupées par couche")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Références

Le projet d’application Android doit faire référence à l’assembly de Xamarin.Android spécifique à la plateforme pour accéder aux classes du SDK Android.

Il doit également référencer le projet de bibliothèque PCL (par exemple). TaskyPortableLibrary) pour accéder au code de couche données et business courantes.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary pour accéder au code de couche données et business courantes")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Couche d’application (AL)

Semblable à la version d’iOS, nous avons étudié précédemment, la couche d’Application dans la version Android contient les classes spécifiques à la plate-forme requis pour les objets exposés par le noyau à l’interface utilisateur de 'bind'.

 **TaskListAdapter** : permet d’afficher une liste<T> d’objets, nous avons besoin d’implémenter un adaptateur pour afficher les objets personnalisés dans un `ListView`. L’adaptateur de contrôle de disposition est utilisée pour chaque élément dans la liste : dans ce cas le code utilise une disposition prédéfinie Android `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Interface utilisateur (IU)

Couche d’Interface utilisateur de l’application Android est une combinaison de code et le balisage XML.

 -   **Ressources/disposition** – conception implémentée en tant que fichiers AXML dispositions d’écran et la ligne de la cellule. Le AXML peut être écrit à la main ou disposé que visuellement à l’aide du Concepteur de l’interface utilisateur de Xamarin pour Android.
 -   **Ressources/Drawable** – images (icônes) et le bouton personnalisé.
 -   **Écrans** – sous-classes d’activité qui définissent chaque écran et son comportement. Relie l’interface utilisateur avec les classes de la couche Application et l’API commune (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Écran d’accueil

L’écran d’accueil se compose d’une sous-classe de l’activité `HomeScreen` et `HomeScreen.axml` fichier qui définit la disposition (position de la liste de bouton et de tâches). L’écran ressemble à ceci :

 [![](case-study-tasky-images/android-taskylist.png "L’écran ressemble à ceci")](case-study-tasky-images/android-taskylist.png#lightbox)

Le code de l’écran d’accueil définit les gestionnaires d’en cliquant sur le bouton et en cliquant sur les éléments dans la liste, ainsi que pour remplir la liste dans le `OnResume` (méthode) (afin qu’il reflète les modifications apportées dans l’écran de détails de tâche). Chargement des données à l’aide de la couche métier `TaskItemManager` et `TaskListAdapter` à partir de la couche Application.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Écran de détails de tâche

L’écran de détails de tâche est également constitué d’un `Activity` sous-classe et un fichier de mise en page AXML. La disposition détermine l’emplacement des contrôles d’entrée et de la classe c# définit le comportement pour charger et enregistrer `TaskItem` objets.

 [![](case-study-tasky-images/android-taskydetail.png "La classe définit le comportement pour charger et enregistrer les objets de l’objet TaskItem")](case-study-tasky-images/android-taskydetail.png#lightbox)

Toutes les références à la bibliothèque PCL soient effectuent via la `TaskItemManager` classe.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Application du Windows Phone
Le projet Windows Phone :

 ![](case-study-tasky-images/taskywp7-solution.png "Application du Windows Phone l’ensemble du projet Windows Phone")

Le diagramme suivant présente les classes regroupées en couches :

 [![](case-study-tasky-images/classdiagram-wp7.png "Ce diagramme présente les classes regroupées dans des couches")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>Références

Le projet spécifique à la plateforme doit référencer les bibliothèques requises spécifiques à la plateforme (tel que `Microsoft.Phone` et `System.Windows`) pour créer une application Windows Phone valide.

Il doit également référencer le projet de bibliothèque PCL (par exemple). `TaskyPortableLibrary`) pour utiliser le `TaskItem` classe et la base de données.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary d’utiliser la classe de TaskItem et de la base de données")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Couche d’application (AL)

Là encore, comme avec l’iOS et Android, la couche d’application se compose des éléments non visuel permettant de lier des données à l’interface utilisateur.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModel encapsuler des données à partir de la bibliothèque PCL ( `TaskItemManager`) et il présente de manière qui peut être utilisé par la liaison de données/XAML Silverlight. Il s’agit d’un exemple de comportement spécifique à la plateforme (comme indiqué dans le document des Applications multiplateformes).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Interface utilisateur (IU)

XAML comprend une fonctionnalité de liaison de données unique qui peut être déclarée dans le balisage et de réduire la quantité de code nécessaire pour afficher les objets suivants :

1.   **Pages** – fichiers XAML et son code-behind définissent l’interface utilisateur et référencer les ViewModel et le projet de bibliothèque de classes portables pour afficher et de collecter des données.
2.   **Images** – images d’écran, en arrière-plan et icône de démarrage sont un aspect essentiel de l’interface utilisateur.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

La classe MainPage utilise le `TaskListViewModel` pour afficher des données à l’aide des fonctionnalités de liaison de données de XAML. La page `DataContext` est définie sur le modèle d’affichage, qui est rempli de façon asynchrone. Le `{Binding}` syntaxe dans le code XAML détermine la façon dont les données sont affichées.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

Chaque tâche est affichée en liant la `TaskViewModel` pour le code XAML défini dans le TaskDetailsPage.xaml. Les données de tâche sont récupérées la `TaskItemManager` dans la couche métier.

 <a name="Results" />

## <a name="results"></a>Résultats

Les applications qui en résulte se présenter comme suit sur chaque plateforme :

 <a name="iOS" />

#### <a name="ios"></a>iOS

L’application utilise la conception de l’interface utilisateur d’e/s standard, telles que le bouton « Ajouter » est placé dans la barre de navigation et à l’aide de la fonction intégrée **plus (+)** icône. Il utilise également la valeur par défaut `UINavigationController` bouton « arrière » le comportement et prend en charge 'balayez à supprimer' dans la table.

 [![](case-study-tasky-images/ios-taskylist.png "Il utilise le comportement du bouton précédent de UINavigationController par défaut et prend en charge le balayage à supprimer dans la table d’également")](case-study-tasky-images/ios-taskylist.png#lightbox) [![](case-study-tasky-images/ios-taskylist.png "il utilise également la valeur par défaut UINavigationController sauvegarder le comportement du bouton et prend en charge le balayage à supprimer dans la table")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

L’application Android utilise les contrôles intégrés, y compris la mise en page intégré pour les lignes qui nécessitent une graduation affichée. Le comportement précédent de matériel système est pris en charge en plus d’un bouton à l’écran précédent.

 [![](case-study-tasky-images/android-taskylist.png "Le comportement précédent de matériel système est pris en charge en plus d’un bouton à l’écran précédent")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "le comportement précédent de matériel système est pris en charge à un écran bouton précédent")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

L’application Windows Phone utilise la mise en page standard, remplissage de la barre des applications au bas de l’écran au lieu d’une barre de navigation en haut.

 [![](case-study-tasky-images/wp-taskylist.png "L’application Windows Phone utilise la mise en page standard, remplissage de la barre des applications au bas de l’écran au lieu d’une barre de navigation en haut")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "de Windows Phone l’application utilise le standard mise en page, remplissage de la barre des applications au bas de l’écran au lieu d’une barre de navigation en haut")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Récapitulatif

Ce document a fourni une explication détaillée de la façon dont les principes de conception de l’application en couches ont été appliqués à une application simple pour faciliter la réutilisation de code entre les trois plateformes mobiles : iOS, Android et Windows Phone.

Elle a décrit le processus utilisé pour concevoir les couches application et décrit ce que le code &amp; fonctionnalité a été implémentée dans chaque couche.

Le code peut être téléchargé à partir de [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Liens associés

- [Génération d’Applications inter-plateformes (document principal)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky Portable exemple d’application (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
