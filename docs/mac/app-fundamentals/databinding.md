---
title: Liaison de données et la clé-valeur de codage dans Xamarin.Mac
description: Cet article couvre l’utilisation de la clé-valeur de codage et observer pour permettre la liaison de données pour les éléments d’interface utilisateur dans le Générateur de Xcode Interface clé-valeur.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 88567e47f488a94fcf7334584a678c9689b83306
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792136"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Liaison de données et la clé-valeur de codage dans Xamarin.Mac

_Cet article couvre l’utilisation de la clé-valeur de codage et observer pour permettre la liaison de données pour les éléments d’interface utilisateur dans le Générateur de Xcode Interface clé-valeur._

## <a name="overview"></a>Vue d'ensemble

Lorsque vous travaillez avec c# et .NET dans une application Xamarin.Mac, vous avez accès aux techniques de liaison de données et de codage de la même clé-valeur qui un développeur travaillant dans *Objective-C* et *Xcode* est. Xamarin.Mac s’intègre directement avec Xcode, vous pouvez utiliser de Xcode _Interface Générateur_ pour lier des données avec les éléments d’interface utilisateur au lieu d’écrire du code.

À l’aide de codage et liaison de données techniques dans votre application Xamarin.Mac clé-valeur, vous pouvez réduire considérablement la quantité de code que vous devez écrire et conserver pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage de vos données de sauvegarde (_modèle de données_) à partir de votre premier plan mettre fin à l’Interface utilisateur (_Model-View-Controller_), début au plus facile à gérer, une application plus souple conception.

[![Un exemple de l’application en cours d’exécution](databinding-images/intro01.png "un exemple de l’application en cours d’exécution")](databinding-images/intro01-large.png#lightbox)

Dans cet article, nous aborderons les principes fondamentaux de l’utilisation de la clé-valeur de codage et de la liaison de données dans une application Xamarin.Mac. Il est fortement recommandé que vous parcourez le [Hello, Mac](~/mac/get-started/hello-mac.md) article tout d’abord, en particulier le [Introduction à Xcode et Interface Générateur](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) et [prises et Actions](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sections, telle qu’elle couvre les principaux concepts et techniques que nous utiliserons dans cet article.

Vous souhaiterez peut-être examiner la [classes exposition c# / méthodes pour Objective-C](~/mac/internals/how-it-works.md) section de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) de document, elle explique la `Register` et `Export` attributs utilisé pour associer vos classes c# pour les objets Objective-C et de l’interface utilisateur éléments.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Quelle est la clé-valeur de codage

Valeur de clé de codage (KVM) est un mécanisme pour accéder aux propriétés d’un objet indirectement, à l’aide de clés (spécialement mise en forme des chaînes) pour identifier les propriétés au lieu d’y accéder via des variables d’instance ou des méthodes d’accesseur (`get/set`). En implémentant les accesseurs conformes codage clé-valeur dans votre application Xamarin.Mac, pour accéder à d’autres fonctionnalités macOS (anciennement OS X) comme clé-valeur observation (KVO), liaison de données, données de base, les liaisons/Cocoa et scriptability.

À l’aide de codage et liaison de données techniques dans votre application Xamarin.Mac clé-valeur, vous pouvez réduire considérablement la quantité de code que vous devez écrire et conserver pour remplir et utiliser des éléments d’interface utilisateur. Vous avez également l’avantage de découplage davantage de vos données de sauvegarde (_modèle de données_) à partir de votre premier plan mettre fin à l’Interface utilisateur (_Model-View-Controller_), début au plus facile à gérer, une application plus souple conception. 

Par exemple, examinons la définition de classe suivante d’un objet conforme KVM :

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Tout d’abord, le `[Register("PersonModel")]` attribut inscrit la classe et l’expose à objectif-C. Ensuite, la classe doit hériter de `NSObject` (ou une sous-classe hérite `NSObject`), cela ajoute plusieurs méthode qui permettent d’être KVM conforme à la classe de base. Ensuite, le `[Export("Name")]` attribut expose le `Name` propriété et définit la valeur de clé qui sera utilisée ultérieurement pour accéder à la propriété via KVM et KVO techniques. 

Enfin, pour pouvoir être clé-valeur observée à la valeur de propriété, l’accesseur doit encapsuler les modifications apportées à sa valeur dans `WillChangeValue` et `DidChangeValue` les appels de méthode (en spécifiant la même clé que la `Export` attribut).  Exemple :

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Cette étape est _très_ important pour la liaison de données dans Xcode de l’Interface Générateur (comme nous le verrons plus loin dans cet article).

Pour plus d’informations, consultez le site d’Apple [Guide de programmation clé-valeur de codage](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Clés et les chemins de clés

A _clé_ est une chaîne qui identifie une propriété spécifique d’un objet. En règle générale, une clé correspond au nom d’une méthode d’accesseur dans une valeur de clé de codage objet conforme. Les clés doivent utiliser l’encodage ASCII, généralement commencer par une lettre minuscule et ne peut pas contenir un espace blanc. Étant donné l’exemple ci-dessus, `Name` serait une valeur de clé `Name` propriété de la `PersonModel` classe. La clé et le nom de la propriété qu’ils exposent ne doivent être identiques, mais dans la plupart des cas.

A _chemin d’accès de la clé_ est une chaîne de point séparés par des clés utilisées pour spécifier une hiérarchie de propriétés de l’objet à parcourir. La propriété de la première clé dans la séquence est relatif au récepteur, et chaque clé suivante est évaluée par rapport à la valeur de la propriété précédente. De la même façon vous utilisez la notation par points pour parcourir un objet et ses propriétés dans une classe c#.

Par exemple, si vous avez développé la `PersonModel` classe et ajouté `Child` propriété :

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Le chemin d’accès de clé au nom de l’enfant serait `self.Child.Name` ou simplement `Child.Name` (selon la façon dont la valeur de clé a été utilisée).

### <a name="getting-values-using-key-value-coding"></a>Obtention de valeurs à l’aide de la clé-valeur de codage

Le `ValueForKey` méthode retourne la valeur de la clé spécifiée (comme un `NSString`), relatif à l’instance de la classe KVM reçoit la demande. Par exemple, si `Person` est une instance de la `PersonModel` définie ci-dessus :

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

Cette requête renvoie la valeur de la `Name` propriété pour cette instance de `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Valeurs de paramètre à l’aide de la clé-valeur de codage

De même, la `SetValueForKey` définir la valeur de la clé spécifiée (comme un `NSString`), relatif à l’instance de la classe KVM reçoit la demande. Là encore, à l’aide d’une instance de la `PersonModel` classe, comme indiqué ci-dessous :

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Modifierait la valeur de la `Name` propriété `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Observer des modifications de valeur

À l’aide de clé-valeur en observant (KVO), vous pouvez attacher un observateur à une clé spécifique d’une classe conforme KVM et averti chaque fois que la valeur de cette clé est modifiée (à l’aide de techniques de KVM ou accéder directement à la propriété donnée dans le code C#). Exemple :

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Maintenant, à tout moment la `Name` propriété de la `Person` instance de la `PersonModel` classe est modifié, la nouvelle valeur est écrit dans la console. 

Pour plus d’informations, consultez le site d’Apple [Introduction au Guide de programmation d’observation clé-valeur](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Liaison de données

Les sections suivantes seront montrent comment vous pouvez utiliser une clé-valeur de codage et la clé-valeur, en observant la classe conforme à lier des données à des éléments d’interface utilisateur dans le Générateur de Xcode Interface, au lieu de lire et écrire des valeurs à l’aide de code c#. De cette façon, vous séparez votre _modèle de données_ à partir des vues qui sont utilisés pour les afficher, afin de la rendre Xamarin.Mac plus flexible et plus facile à gérer. Vous réduisez considérablement la quantité de code qui doit être écrit.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Définition de votre modèle de données

Avant de pouvoir lier des données dans le Générateur de Interface un élément d’interface utilisateur, vous devez disposer d’une classe conforme KVM/KVO définie dans votre application Xamarin.Mac en tant que le _modèle de données_ pour la liaison. Le modèle de données fournit toutes les données qui seront affiche dans l’Interface utilisateur et reçoit toutes les modifications des données effectués par l’utilisateur dans l’interface utilisateur pendant l’exécution de l’application.

Par exemple, si vous écrivez une application qui gérés d’un groupe d’employés, vous pouvez utiliser la classe suivante pour définir le modèle de données :

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
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
    }
}
```

La plupart des fonctionnalités de cette classe ont été traitée dans le [quelle est la valeur de clé de codage](#What_is_Key-Value_Coding) section ci-dessus. Toutefois, examinons certains éléments spécifiques et quelques ajouts qui ont été apportées pour permettre à cette classe en tant que modèle de données pour **contrôleurs de baie** et **arborescence contrôleurs** (ce qui nous utiliserons ultérieurement à des données lier **arborescences**, **mode plan** et **vues de Collection**).

En premier lieu, car un employé peut être un gestionnaire, nous avons utilisé un `NSArray` (en particulier un `NSMutableArray` donc les valeurs peuvent être modifiées) pour autoriser les employés qu’ils gérés pour être attachés à ceux-ci :

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Deux choses à noter ici :

1. Nous avons utilisé un `NSMutableArray` au lieu d’un tableau c# ou une collection, car cette opération est nécessaire pour lier des données aux contrôles de AppKit comme standard **tableaux**, **mode plan** et **Collections** .
2. Nous exposé le tableau d’employés par un cast sur un `NSArray` pour la liaison de données à des fins et modifié son c# au format de nom, `People`, à ce que la liaison de données attend, `personModelArray` sous la forme **{class_name} tableau** (Remarque que le premier caractère a été effectué en minuscules).

Ensuite, nous devons ajouter certaines spécialement nom méthodes publiques pour prendre en charge **contrôleurs de baie** et **arborescence contrôleurs**:

```csharp
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
```

Celles-ci permettent aux contrôleurs de demander et de modifier les données qu’ils affichent. Comme l’exposé `NSArray` ci-dessus, ces messages ont une convention d’affectation des noms très spécifique (ce qui diffère de C# d’affectation de noms conventions standard) :

- `addObject:` -Ajoute un objet dans le tableau.
- `insertObject:in{class_name}ArrayAtIndex:` -Où `{class_name}` est le nom de votre classe. Cette méthode insère un objet dans le tableau à partir d’un index donné.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Où `{class_name}` est le nom de votre classe. Cette méthode supprime l’objet dans le tableau à un index donné.
- `set{class_name}Array:` -Où `{class_name}` est le nom de votre classe. Cette méthode vous permet de remplacer le transport existant par une nouvelle.

À l’intérieur de ces méthodes, nous avons enveloppé des modifications dans le tableau dans `WillChangeValue` et `DidChangeValue` messages pour la conformité de KVO.

Enfin, depuis le `Icon` propriété s’appuie sur la valeur de la `isManager` modifications apportées aux propriétés, les `isManager` propriété ne soient pas répercutée dans le `Icon` pour les éléments de l’interface utilisateur de données liées (pendant KVO) :

```csharp
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
``` 

Pour corriger cela, nous utilisons le code suivant :

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Notez qu’en plus de sa propre clé, le `isManager` accesseur envoie également le `WillChangeValue` et `DidChangeValue` des messages pour le `Icon` afin qu’il s’affiche également la modification de la clé.

Nous utiliserons la `PersonModel` modèle de données dans le reste de cet article.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Liaison de données simple

Avec notre modèle de données défini, examinons un exemple simple de liaison de données dans le Générateur de Xcode Interface. Par exemple, vous allez ajouter un formulaire à notre application Xamarin.Mac qui peut être utilisée pour modifier la `PersonModel` que nous avons défini ci-dessus. Nous allons ajouter quelques champs de texte et une case à cocher pour afficher et modifier les propriétés de notre modèle.

Tout d’abord, nous allons ajouter un nouveau **View Controller** à notre **Main.storyboard** dans l’Interface Générateur de fichier et nom de sa classe `SimpleViewController`: 

[![Ajoutez un nouveau contrôleur de vue](databinding-images/simple01.png "Ajout d’un nouveau contrôleur de vue")](databinding-images/simple01-large.png#lightbox)

Revenez à Visual Studio pour Mac, modifiez le **SimpleViewController.cs** fichier (ce qui a été automatiquement ajouté à notre projet) et d’exposer une instance de la `PersonModel` que nous sera notre formulaire de liaison de données. Ajoutez le code suivant :

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

Ensuite lors du chargée de la vue, nous allons créer une instance de notre `PersonModel` et le remplir avec ce code :

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Maintenant nous avons besoin créer notre formulaire, double-cliquez sur le **Main.storyboard** fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Disposition du formulaire afin de se présenter comme suit :

[![Modification du plan conceptuel dans Xcode](databinding-images/simple02.png "modifier le plan conceptuel dans Xcode")](databinding-images/simple02-large.png#lightbox)

Lier aux données du formulaire à la `PersonModel` que nous exposée le `Person` clé, procédez comme suit :

1. Sélectionnez le **nom de l’employé** champ de texte et de basculer vers le **liaisons inspecteur**.
2. Vérifiez le **lier à** et sélectionnez **contrôleur d’affichage Simple** dans la liste déroulante. Entrez ensuite `self.Person.Name` pour le **chemin d’accès de la clé**: 

    [![Entrez le chemin de clé](databinding-images/simple03.png "entrer le chemin de clé")](databinding-images/simple03-large.png#lightbox)
3. Sélectionnez le **profession** champ de texte et vérifier les **lier à** et sélectionnez **contrôleur d’affichage Simple** dans la liste déroulante. Entrez ensuite `self.Person.Occupation` pour le **chemin d’accès de la clé**:  

    [![Entrez le chemin de clé](databinding-images/simple04.png "entrer le chemin de clé")](databinding-images/simple04-large.png#lightbox)
4. Sélectionnez le **employé est un gestionnaire de** case à cocher et vérifiez la **lier à** et sélectionnez **contrôleur d’affichage Simple** dans la liste déroulante. Entrez ensuite `self.Person.isManager` pour le **chemin d’accès de la clé**:  

    [![Entrez le chemin de clé](databinding-images/simple05.png "entrer le chemin de clé")](databinding-images/simple05-large.png#lightbox)
5. Sélectionnez le **nombre d’employés géré** champ de texte et vérifier les **lier à** et sélectionnez **contrôleur d’affichage Simple** dans la liste déroulante. Entrez ensuite `self.Person.NumberOfEmployees` pour le **chemin d’accès de la clé**:  

    [![Entrez le chemin de clé](databinding-images/simple06.png "entrer le chemin de clé")](databinding-images/simple06-large.png#lightbox)
6. Si l’employé n’est pas un gestionnaire, que vous souhaitez masquer le nombre d’employés gérés étiquette et un champ de texte.
7. Sélectionnez le **nombre d’employés géré** étiquette, développez le **masqué** turndown et vérifiez la **lier à** et sélectionnez **contrôleur d’affichage Simple** dans la liste déroulante. Entrez ensuite `self.Person.isManager` pour le **chemin d’accès de la clé**:  

    [![Entrez le chemin de clé](databinding-images/simple07.png "entrer le chemin de clé")](databinding-images/simple07-large.png#lightbox)
8. Sélectionnez `NSNegateBoolean` à partir de la **valeur transformateur** liste déroulante :  

    ![Sélection de la transformation de la clé NSNegateBoolean](databinding-images/simple08.png "en sélectionnant la transformation de la clé NSNegateBoolean")
9. Cela indique à la liaison de données que l’étiquette sera masquée si la valeur de la `isManager` propriété est `false`.
10. Répétez les étapes 7 et 8 pour le **nombre d’employés géré** champ de texte.
11. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Si vous exécutez l’application, les valeurs à partir de la `Person` propriété remplit automatiquement les notre formulaire :

[![Afficher un formulaire et rempli automatiquement](databinding-images/simple09.png "montrant un formulaire et rempli automatiquement")](databinding-images/simple09-large.png#lightbox)

Toutes les modifications apportées par les utilisateurs vers le formulaire seront réécrites à le `Person` propriété dans le contrôleur de la vue. Par exemple, si vous désélectionnez le **employé est un gestionnaire de** mises à jour le `Person` instance de notre `PersonModel` et **nombre d’employés géré** étiquette et le champ de texte sont masquées automatiquement (via liaison de données) :

[![Le masquage du nombre d’employés pour les gestionnaires de non](databinding-images/simple10.png "le masquage du nombre d’employés pour les autres personnes")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Liaison de données de vue de table

Maintenant que nous avons les principes fondamentaux de la liaison de données, examinons une tâche de liaison de données plus complexe à l’aide un _contrôleur de baie_ et la liaison de données à une vue de Table. Pour plus d’informations sur l’utilisation des vues de Table, consultez notre [tableaux](~/mac/user-interface/table-view.md) documentation.

Tout d’abord, nous allons ajouter un nouveau **View Controller** à notre **Main.storyboard** dans l’Interface Générateur de fichier et nom de sa classe `TableViewController`:

[![Ajoutez un nouveau contrôleur de vue](databinding-images/table01.png "Ajout d’un nouveau contrôleur de vue")](databinding-images/table01-large.png#lightbox)

Ensuite, permet de modifier le **TableViewController.cs** fichier (ce qui a été automatiquement ajouté à notre projet) et exposer un tableau (`NSArray`) de `PersonModel` classes que nous sera notre formulaire de liaison de données. Ajoutez le code suivant :

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
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
```

Comme nous l’avons fait sur la `PersonModel` classe ci-dessus dans le [définissant votre modèle de données](#Defining_your_Data_Model) section, nous avons exposé quatre méthodes publiques spéciale nommées afin que les contrôleur de baie et en lecture et écriture aux données à partir de la collection de `PersonModels`.

Lors du chargée de la vue, nous devons ensuite remplir notre tableau avec ce code :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Maintenant nous avons besoin créer notre Table, vue, double-cliquez sur le **Main.storyboard** fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Disposition de la table pour ressembler à la suivante :

[![Disposer d’une nouvelle vue table](databinding-images/table02.png "disposer d’une nouvelle vue de table")](databinding-images/table02-large.png#lightbox)

Nous devons ajouter une **contrôleur de baie** pour fournir des données liées à notre table, procédez comme suit :

1. Faites glisser un **tableau contrôleur** à partir de la **inspecteur de bibliothèque** sur la **éditeur de l’Interface**:  

    ![Sélection d’un contrôleur de baie à partir de la bibliothèque](databinding-images/table03.png "en sélectionnant un contrôleur de baie à partir de la bibliothèque")
2. Sélectionnez **contrôleur de baie** dans les **hiérarchie de l’Interface** et basculez vers le **inspecteur de l’attribut**:  

    [![Sélection de l’inspecteur attributs](databinding-images/table04.png "en sélectionnant l’inspecteur d’attributs")](databinding-images/table04-large.png#lightbox)
3. Entrez `PersonModel` pour le **nom de la classe**, cliquez sur le **Plus** bouton et ajoutez des clés de trois. Nommez-les `Name`, `Occupation` et `isManager`:  

    ![Ajouter les chemins d’accès de clé requis](databinding-images/table05.png "ajouter les chemins d’accès de clé requis")
4. Cela indique le contrôleur de groupe qu’il gère un tableau, et les propriétés, elle doit exposer (via les clés).
5. Basculez vers le **liaisons inspecteur** et sous **tableau contenu** sélectionnez **lier à** et **Table-View-Controller**. Entrez un **modèle de chemin de la clé** de `self.personModelArray`:  

    ![Entrez un chemin de clé](databinding-images/table06.png "entrer un chemin de clé")
6. Cela lie le contrôleur de tableau au tableau de `PersonModels` que nous exposée sur le contrôleur de la vue.

Maintenant nous avons besoin de lier le contrôleur de baie de notre Table, vue, procédez comme suit :

1. Sélectionnez la vue de Table et la **liaison inspecteur**:  

    [![Sélection de l’inspecteur de liaison](databinding-images/table07.png "en sélectionnant l’inspecteur de liaison")](databinding-images/table07-large.png#lightbox)
2. Sous le **sommaire** turndown, sélectionnez **lier à** et **contrôleur de baie**. Entrez `arrangedObjects` pour le **contrôleur clé** champ :  

    ![Définition de la clé de contrôleur](databinding-images/table08.png "définissant la clé de contrôleur")
3. Sélectionnez le **cellule d’affichage de tableau** sous le **employé** colonne. Dans le **liaisons inspecteur** sous le **valeur** turndown, sélectionnez **lier à** et **affichage de cellule de tableau**. Entrez `objectValue.Name` pour le **modèle de chemin de la clé**:  

    [![Définir le chemin d’accès de clé de modèle](databinding-images/table09.png "définissant le chemin d’accès de clé de modèle")](databinding-images/table09-large.png#lightbox)
4. `objectValue` est en cours `PersonModel` dans le tableau qui est géré par le contrôleur de tableau.
5. Sélectionnez le **cellule d’affichage de tableau** sous le **profession** colonne. Dans le **liaisons inspecteur** sous le **valeur** turndown, sélectionnez **lier à** et **affichage de cellule de tableau**. Entrez `objectValue.Occupation` pour le **modèle de chemin de la clé**:  

    [![Définir le chemin d’accès de clé de modèle](databinding-images/table10.png "définissant le chemin d’accès de clé de modèle")](databinding-images/table10-large.png#lightbox)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Si nous exécutons l’application, la table sera remplie avec notre tableau de `PersonModels`:

[![Exécution de l’application](databinding-images/table11.png "l’application en cours d’exécution")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Liaison de données de mode plan

liaison de données par rapport à un mode plan est très similaire à la liaison par rapport à une vue de Table. La principale différence est que nous utilisons un **arborescence contrôleur** au lieu d’un **contrôleur de baie de** pour fournir des données liées à la vue hiérarchique. Pour plus d’informations sur l’utilisation des vues de plan, consultez notre [mode plan](~/mac/user-interface/outline-view.md) documentation.

Tout d’abord, nous allons ajouter un nouveau **View Controller** à notre **Main.storyboard** dans l’Interface Générateur de fichier et nom de sa classe `OutlineViewController`: 

[![Ajoutez un nouveau contrôleur de vue](databinding-images/outline01.png "Ajout d’un nouveau contrôleur de vue")](databinding-images/outline01-large.png#lightbox)

Ensuite, permet de modifier le **OutlineViewController.cs** fichier (ce qui a été automatiquement ajouté à notre projet) et exposer un tableau (`NSArray`) de `PersonModel` classes que nous sera notre formulaire de liaison de données. Ajoutez le code suivant :

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
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
```

Comme nous l’avons fait sur la `PersonModel` classe ci-dessus dans le [définissant votre modèle de données](#Defining_your_Data_Model) section, nous avons exposé quatre méthodes publiques spéciale nommées afin que les données du contrôleur de l’arborescence et en lecture et écriture à partir de la collection de `PersonModels`.

Lors du chargée de la vue, nous devons ensuite remplir notre tableau avec ce code :

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Maintenant nous avons besoin créer notre mode plan, double-cliquez sur le **Main.storyboard** fichier à ouvrir pour le modifier dans le Générateur de l’Interface. Disposition de la table pour ressembler à la suivante :

[![Création de la vue hiérarchique](databinding-images/outline02.png "création le mode plan")](databinding-images/outline02-large.png#lightbox)

Nous devons ajouter une **arborescence contrôleur** pour fournir des données liées à notre plan, procédez comme suit :

1. Faites glisser un **arborescence contrôleur** à partir de la **inspecteur de bibliothèque** sur la **éditeur de l’Interface**:  

    ![Sélection d’un contrôleur de l’arborescence de la bibliothèque](databinding-images/outline03.png "en sélectionnant un contrôleur de l’arborescence de la bibliothèque")
2. Sélectionnez **arborescence contrôleur** dans les **hiérarchie de l’Interface** et basculez vers le **inspecteur de l’attribut**:  

    [![Sélection de l’inspecteur de l’attribut](databinding-images/outline04.png "en sélectionnant l’inspecteur de l’attribut")](databinding-images/outline04-large.png#lightbox)
3. Entrez `PersonModel` pour le **nom de la classe**, cliquez sur le **Plus** bouton et ajoutez des clés de trois. Nommez-les `Name`, `Occupation` et `isManager`:  

    ![Ajouter les chemins d’accès de clé requis](databinding-images/outline05.png "ajouter les chemins d’accès de clé requis")
4. Cela indique le contrôleur de l’arborescence qu’il gère un tableau, et les propriétés, elle doit exposer (via les clés).
5. Sous le **arborescence contrôleur** section, entrez `personModelArray` pour **enfants**, entrez `NumberOfEmployees` sous le **nombre** et entrez `isEmployee` sous  **Feuille**:  

    ![Définir les chemins d’accès de clés de contrôleur de l’arborescence](databinding-images/outline05.png "définissant les chemins d’accès de clés de contrôleur d’arborescence")
6. Cela indique le contrôleur de l’arborescence où trouver tous les enfants de nœuds sont des nœuds enfants combien il et si le nœud actuel possède des nœuds enfants.
7. Basculez vers le **liaisons inspecteur** et sous **tableau contenu** sélectionnez **lier à** et **propriétaire du fichier**. Entrez un **modèle de chemin de la clé** de `self.personModelArray`:  

    ![Modification du chemin d’accès clé](databinding-images/outline06.png "modifier le chemin de clé")
8. Cela lie le contrôleur d’arborescence dans le tableau des `PersonModels` que nous exposée sur le contrôleur de la vue.

Maintenant nous avons besoin de lier notre mode plan pour le contrôleur de l’arborescence, procédez comme suit :

1. Sélectionnez le mode plan et en le **l’inspecteur de liaison** sélectionnez :  

    [![Sélection de l’inspecteur de liaison](databinding-images/outline07.png "en sélectionnant l’inspecteur de liaison")](databinding-images/outline07-large.png#lightbox)
2. Sous le **afficher le contenu de plan** turndown, sélectionnez **lier à** et **arborescence contrôleur**. Entrez `arrangedObjects` pour le **contrôleur clé** champ :  

    ![Définition de la clé de contrôleur](databinding-images/outline08.png "définissant la clé de contrôleur")
3. Sélectionnez le **cellule d’affichage de tableau** sous le **employé** colonne. Dans le **liaisons inspecteur** sous le **valeur** turndown, sélectionnez **lier à** et **affichage de cellule de tableau**. Entrez `objectValue.Name` pour le **modèle de chemin de la clé**:  

    [![Entrez le chemin d’accès de clé de modèle](databinding-images/outline09.png "entrer le chemin d’accès de clé de modèle")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` est en cours `PersonModel` dans le tableau qui est géré par le contrôleur de l’arborescence.
5. Sélectionnez le **cellule d’affichage de tableau** sous le **profession** colonne. Dans le **liaisons inspecteur** sous le **valeur** turndown, sélectionnez **lier à** et **affichage de cellule de tableau**. Entrez `objectValue.Occupation` pour le **modèle de chemin de la clé**:  

    [![Entrez le chemin d’accès de clé de modèle](databinding-images/outline10.png "entrer le chemin d’accès de clé de modèle")](databinding-images/outline10-large.png#lightbox)
6. Enregistrez vos modifications et revenir à Visual Studio pour Mac pour la synchronisation avec Xcode.

Si nous exécutons l’application, le plan est rempli avec notre tableau de `PersonModels`:

[![Exécution de l’application](databinding-images/outline11.png "l’application en cours d’exécution")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Liaison de données de vue de collection

Liaison de données avec une vue de Collection est très similaire à la liaison avec une vue de la Table, tel qu’un contrôleur de baie est utilisé pour fournir des données pour la collection. Étant donné que la vue de collection n’a pas un format d’affichage prédéfini, un travail supplémentaire est requis pour fournir des commentaires d’interaction utilisateur et pour effectuer le suivi de la sélection de l’utilisateur.

> [!IMPORTANT]
> En raison d’un problème dans Xcode 7 et macOS 10.11 (et supérieur), les vues de Collection ne peuvent pas être utilisé à l’intérieur d’un fichier de plan conceptuel (.storyboard). Par conséquent, vous devez continuer à utiliser les fichiers .xib pour définir vos vues de Collection pour vos applications Xamarin.Mac. Veuillez consulter notre [vues de Collection](~/mac/user-interface/collection-view.md) documentation pour plus d’informations.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
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
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Débogage natif se bloque

Si vous effectuez une erreur dans vos liaisons de données peut provoquer un _Crash Native_ dans du code non managé et empêcher votre application Xamarin.Mac échouer avec une `SIGABRT` erreur :

[![Exemple de boîte de dialogue incident natif](databinding-images/debug01.png "exemple de boîte de dialogue incident natif")](databinding-images/debug01-large.png#lightbox)

En général, il existe quatre causes principales des pannes natifs lors de la liaison de données :

1. Votre modèle de données n’hérite pas de `NSObject` ou une sous-classe de `NSObject`.
2. Vous n’avez pas exposé à Objective-C à l’aide de votre propriété le `[Export("key-name")]` attribut.
3. Vous imbriquez pas les modifications apportées à la valeur de l’accesseur dans `WillChangeValue` et `DidChangeValue` les appels de méthode (en spécifiant la même clé que la `Export` attribut).
4. Vous disposez d’une clé incorrecte ou erreur de saisie le **l’inspecteur de liaison** dans le constructeur d’Interface.

### <a name="decoding-a-crash"></a>Le décodage d’un incident

Nous allons entraîne un blocage natif dans notre liaison de données nous pouvons afficher comment localiser et corriger. Dans le générateur Interface, nous allons modifier notre liaison de la première étiquette dans l’exemple de la vue de Collection à partir de `Name` à `Title`:

[![Modification de la clé de liaison](databinding-images/debug02.png "modification de la clé de liaison")](databinding-images/debug02-large.png#lightbox)

Nous allons enregistrer la modification, revenez à Visual Studio pour Mac pour la synchronisation avec Xcode et exécuter votre application. Lorsque la vue de Collection est affichée, l’application sera bientôt se bloquer avec une `SIGABRT` erreur (comme indiqué dans le **sortie de l’Application** dans Visual Studio pour Mac) dans la mesure où le `PersonModel` n’expose pas une propriété avec la clé `Title`:

[![Exemple d’une erreur de liaison](databinding-images/debug03.png "exemple d’une erreur de liaison")](databinding-images/debug03-large.png#lightbox)

Si nous allons faire défiler vers la partie supérieure de l’erreur dans le **sortie de l’Application** nous pouvons voir la clé à résoudre le problème :

[![Recherche le problème dans le journal des erreurs](databinding-images/debug04.png "recherche le problème dans le journal des erreurs")](databinding-images/debug04-large.png#lightbox)

Cette ligne nous indique que la clé `Title` n’existe pas sur l’objet que nous créons une liaison. Si nous modifions la liaison pour revenir `Name` dans le générateur Interface, enregistrer, la synchronisation, reconstruire et exécuté, l’application s’exécutera comme prévu, sans problème.

## <a name="summary"></a>Récapitulatif

Cet article a pris une présentation détaillée sur l’utilisation de la liaison de données et la clé-valeur de codage dans une application Xamarin.Mac. Tout d’abord, il se présentait à exposer une classe c# pour Objective-C en utilisant la clé-valeur de codage (KVM) et clé-valeur en observant (KVO). Ensuite, il vous a montré comment utiliser une classe conforme KVO et données lier à des éléments d’interface utilisateur dans le Générateur de Xcode Interface. Enfin, il a montré la liaison de données complexes à l’aide **contrôleurs de baie** et **arborescence contrôleurs**.


## <a name="related-links"></a>Liens associés

- [Table de montage séquentiel MacDatabinding (exemple)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (exemple)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Contrôles standard](~/mac/user-interface/standard-controls.md)
- [Vues de table](~/mac/user-interface/table-view.md)
- [Mode plan](~/mac/user-interface/outline-view.md)
- [Vues de collection](~/mac/user-interface/collection-view.md)
- [Guide de programmation de codage clé-valeur](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Introduction au Guide de programmation clé-valeur en observant](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Introduction aux liaisons/Cocoa sujets de programmation](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Introduction à la référence de liaisons/Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS indications de l’Interface humaine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
