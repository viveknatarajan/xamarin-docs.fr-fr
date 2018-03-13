---
title: "Entrée"
description: "Ligne de texte unique ou d’entrée de mot de passe"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 88b133a803aa74324e60a4f7610a2c0f0d31a807
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="entry"></a>Entrée

_Ligne de texte unique ou d’entrée de mot de passe_

Xamarin.Forms `Entry` est utilisé pour l’entrée de la ligne de texte unique. `Entry`, comme l’affichage de l’éditeur, prend en charge de plusieurs types de clavier. En outre, `Entry` peut être utilisé comme un champ de mot de passe.

## <a name="display-customization"></a>Personnalisation de l’affichage

### <a name="setting-and-reading-text"></a>Définition et de lecture du texte

Entrée, comme d’autres affichages de présentation de texte, expose la `Text` propriété. `Text` peut être utilisé pour définir et lire le texte présenté par le `Entry`. L’exemple suivant illustre la définition du texte en XAML :

```xaml
<Entry Text="I am an Entry" />
```

En C# :

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Pour lire le texte, accéder à la `Text` propriété en c# :

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> La largeur d’une `Entry` peut être défini en définissant son `WidthRequest` propriété. Ne dépendent pas de la largeur d’une `Entry` définie en fonction de la valeur de son `Text` propriété.

### <a name="keyboards"></a>Claviers

Le clavier qui s’affiche lorsque les utilisateurs interagissent avec un `Entry` peut être définie par programme via le `Keyboard` propriété.

Les options pour le type de clavier sont :

- **Par défaut** &ndash; du clavier par défaut
- **Chat** &ndash; utilisé pour texto & endroits où emoji sont utiles
- **Messagerie** &ndash; utilisé lors de la saisie des adresses de messagerie
- **Numérique** &ndash; utilisé lors de la saisie de nombres
- **Téléphone** &ndash; utilisé lors de la saisie des numéros de téléphone
- **URL** &ndash; utilisé pour entrer les chemins d’accès de fichier et les adresses web

Il existe un [exemple de chaque clavier](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) dans notre section de recettes.

### <a name="placeholders"></a>Espaces réservés

`Entry` peut être défini pour afficher le texte d’espace réservé quand il ne stocke pas d’entrée d’utilisateur. Dans la pratique, il est souvent dans les formulaires pour préciser le contenu qui est approprié pour un champ donné. Couleur de texte d’espace réservé ne peut pas être personnalisé et doit être le même, quel que soit le `TextColor` paramètre. Si votre conception nécessite une couleur de l’espace réservé personnalisé, vous devez revenir à un [convertisseur personnalisé](). Ce qui suit crée un `Entry` avec « Nom_utilisateur » en tant que l’espace réservé en XAML :

```xaml
<Entry Placeholder="Username" />
```

En C# :

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Exemple d’entrée d’espace réservé")

### <a name="password-fields"></a>Champs de mot de passe

`Entry` fournit le `IsPassword` propriété. Lorsque `IsPassword` est `true`, la valeur du champ s’affiche sous forme de cercles noirs :

En XAML :

```xaml
<Entry IsPassword="true" />
```

En C# :

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Exemple d’entrée IsPassword")

Espaces réservés peuvent être utilisés avec les instances de `Entry` qui sont configurés en tant que champs de mot de passe :

En XAML :

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

En C# :

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "IsPassword d’entrée et un exemple d’espace réservé")


### <a name="colors"></a>Couleurs

Peut être définie pour utiliser un arrière-plan personnalisé et des couleurs de texte via les propriétés pouvant être liées suivantes :

- **TextColor** &ndash; définit la couleur du texte.
- **BackgroundColor** &ndash; définit la couleur affichée derrière le texte.

Une attention particulière est nécessaire pour s’assurer que les couleurs sera utilisables sur chaque plateforme. Étant donné que chaque plateforme a des valeurs par défaut différentes pour les couleurs de texte et d’arrière-plan, vous devez souvent définir les deux si vous définissez une.

Utilisez le code suivant pour définir la couleur du texte d’une entrée :

En XAML :

```xaml
<Entry TextColor="Green" />
```

En C# :

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Exemple d’entrée TextColor")

Notez que l’espace réservé n’est pas affecté par le `TextColor`.

Pour définir la couleur d’arrière-plan en XAML :

```xaml
<Entry BackgroundColor="#2c3e50" />
```

En C# :

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Exemple d’entrée BackgroundColor")

Veillez à vous assurer que les couleurs d’arrière-plan et de texte que vous choisissez sont utilisables sur chaque plateforme et ne pas masquer tout texte d’espace réservé.

## <a name="events-and-interactivity"></a>Événements et interactivité

Entrée expose deux événements :

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; déclenché lorsque le texte modifié dans l’entrée. Fournit le texte avant et après la modification.
- [Terminé](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; déclenché lorsque l’utilisateur a terminé d’entrée en appuyant sur la touche Retour sur le clavier.

### <a name="completed"></a>Terminé

Le `Completed` événement permet de réagir à la fin d’une interaction avec une entrée. `Completed` est déclenché lorsque l’utilisateur met fin à l’entrée avec un champ en saisissant la clé de retournée sur le clavier. Le gestionnaire pour l’événement est un gestionnaire d’événements générique, en prenant l’expéditeur et `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Il est possible de s’abonner à l’événement terminé en XAML :

```xaml
<Entry Completed="Entry_Completed" />
```

et Visual c# :

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

Le `TextChanged` événement permet de réagir à une modification dans le contenu d’un champ.

`TextChanged` est déclenché chaque fois que le `Text` de la `Entry` modifications. Le gestionnaire pour l’événement prend une instance de `TextChangedEventArgs`. `TextChangedEventArgs` fournit l’accès aux valeurs anciennes et nouvelles de la `Entry` `Text` via la `OldTextValue` et `NewTextValue` propriétés :

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Le `TextChanged` peut être abonné à l’événement en XAML :

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

et Visual c# :

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Liens associés

- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API d’entrée](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
