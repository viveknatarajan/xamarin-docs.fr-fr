---
title: Entrée de Xamarin.Forms
description: Cet article explique comment utiliser la classe d’entrée de Xamarin.Forms pour accepter la ligne de texte unique ou une entrée de mot de passe dans une application.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998229"
---
# <a name="xamarinforms-entry"></a>Entrée de Xamarin.Forms

_Texte à ligne unique ou entrée de mot de passe_

Xamarin.Forms `Entry` est utilisé pour l’entrée de texte à ligne unique. Le `Entry`, comme le `Editor` afficher, prend en charge plusieurs types de clavier. En outre, le `Entry` peut être utilisé comme un champ de mot de passe.

## <a name="display-customization"></a>Personnalisation de l’affichage

### <a name="setting-and-reading-text"></a>Définition et de lecture du texte

Le `Entry`, telles que d’autres affichages de présentation de texte, expose le `Text` propriété. Cette propriété peut être utilisée pour définir et lire le texte présenté par le `Entry`. L’exemple suivant montre comment définir le `Text` propriété dans XAML :

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
> La largeur d’un `Entry` peut être définie en configurant ses `WidthRequest` propriété. Ne dépendent pas de la largeur d’un `Entry` qui est défini selon la valeur de son `Text` propriété.

### <a name="limiting-input-length"></a>Limiter la longueur d’entrée

Le [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) propriété peut être utilisée pour limiter la longueur d’entrée est autorisée pour le [ `Entry` ](xref:Xamarin.Forms.Entry). Cette propriété doit être définie à un entier positif :

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

Un [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) propriété la valeur 0 indique qu’aucune entrée n’est autorisée et la valeur `int.MaxValue`, qui est la valeur par défaut pour un [ `Entry` ](xref:Xamarin.Forms.Entry), indique qu’il y a aucune limite effective sur le nombre de caractères qui peuvent être entrés.

### <a name="keyboards"></a>Claviers

Le clavier qui s’affiche lorsque les utilisateurs interagissent avec un `Entry` peut être définie par programme via le `Keyboard` propriété.

Les options pour le type de clavier sont :

- **Par défaut** &ndash; du clavier par défaut
- **Chat** &ndash; utilisé pour aussi & les endroits où emoji sont utiles
- **E-mail** &ndash; utilisé lors de la saisie d’adresses de messagerie
- **Numérique** &ndash; utilisé lors de la saisie de numéros
- **Téléphone** &ndash; utilisé lors de la saisie de numéros de téléphone
- **URL** &ndash; utilisé pour entrer des chemins de fichiers et des adresses web

Il existe un [exemple de chaque clavier](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) dans notre section de recettes.

### <a name="enabling-and-disabling-spell-checking"></a>Activation et désactivation de la vérification orthographique

Le [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) propriété contrôle si vérification orthographique est activée. Par défaut, la propriété est définie `true`. Comme l’utilisateur entre du texte, les fautes d’orthographe sont indiquées.

Toutefois, dans certains scénarios d’entrée de texte, comme la saisie d’un nom d’utilisateur, la vérification orthographique permet une expérience négative et donc doivent être désactivées en définissant le [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) propriété `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Lorsque le [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) propriété est définie sur `false`et un clavier personnalisé n’est pas utilisé, le vérificateur orthographique natif va être désactivé. Toutefois, si un [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) a été ensemble qui désactive orthographique vérification, tel que [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), le `IsSpellCheckEnabled` propriété est ignorée. Par conséquent, la propriété ne peut pas être utilisée pour activer la vérification orthographique pour un `Keyboard` qui désactive explicitement.

### <a name="placeholders"></a>Espaces réservés

`Entry` peut être défini pour afficher le texte d’espace réservé quand il ne stocke pas les entrées d’utilisateur. Dans la pratique, cela se produit souvent dans les formulaires afin de clarifier le contenu qui est approprié pour un champ donné. Couleur de texte d’espace réservé ne peut pas être personnalisé et doit être le même quel que soit le `TextColor` paramètre. Si votre conception fait appel à une couleur de l’espace réservé personnalisé, vous devrez revenir à un [convertisseur personnalisé](). Ce qui suit crée un `Entry` avec « Username » comme espace réservé dans XAML :

```xaml
<Entry Placeholder="Username" />
```

En C# :

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Exemple d’entrée d’espace réservé")

### <a name="password-fields"></a>Champs de mot de passe

`Entry` fournit le `IsPassword` propriété. Lorsque `IsPassword` est `true`, le contenu du champ s’affiche sous forme de cercles noir :

Dans XAML :

```xaml
<Entry IsPassword="true" />
```

En C# :

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Exemple d’entrée IsPassword")

Espaces réservés peuvent être utilisés avec les instances de `Entry` qui sont configurés en tant que champs de mot de passe :

Dans XAML :

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

En C# :

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Entrée IsPassword et exemple d’espace réservé")


### <a name="colors"></a>Couleurs

Entrée peut être définie pour utiliser un arrière-plan personnalisé et les couleurs de texte via les propriétés pouvant être liées suivantes :

- **TextColor** &ndash; définit la couleur du texte.
- **BackgroundColor** &ndash; définit la couleur affichée derrière le texte.

Une attention particulière est nécessaire pour garantir que les couleurs seront utilisables sur chaque plateforme. Étant donné que chaque plateforme a des valeurs par défaut différentes pour les couleurs de texte et d’arrière-plan, vous devrez souvent définissez à la fois si vous définissez une.

Utilisez le code suivant pour définir la couleur du texte d’une entrée :

Dans XAML :

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

Veillez à vous assurer que les couleurs d’arrière-plan et le texte que vous choisissez sont utilisables sur chaque plateforme et n’assombrissent pas n’importe quel texte d’espace réservé.

## <a name="events-and-interactivity"></a>Événements et interactivité

Entrée expose deux événements :

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash; déclenché lorsque le texte modifié dans l’entrée. Fournit le texte avant et après la modification.
- [Terminé](xref:Xamarin.Forms.Entry.Completed) &ndash; déclenché lorsque l’utilisateur s’est terminée entrée en appuyant sur la touche Retour sur le clavier.

### <a name="completed"></a>Terminé

Le `Completed` événement est utilisé pour réagir à la fin d’une interaction avec une entrée. `Completed` est déclenché lorsque l’utilisateur termine l’entrée avec un champ en entrant la touche Retour sur le clavier. Le gestionnaire pour l’événement est un gestionnaire d’événements générique, en prenant l’expéditeur et `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Il est possible de s’abonner à l’événement terminé dans XAML :

```xaml
<Entry Completed="Entry_Completed" />
```

et c# :

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

Le `TextChanged` événement est utilisé pour réagir à une modification dans le contenu d’un champ.

`TextChanged` est déclenché chaque fois que le `Text` de la `Entry` modifications. Le gestionnaire pour l’événement prend une instance de `TextChangedEventArgs`. `TextChangedEventArgs` fournit l’accès aux valeurs anciennes et nouvelles de la `Entry` `Text` via la `OldTextValue` et `NewTextValue` propriétés :

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Le `TextChanged` peut être abonné à l’événement dans XAML :

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

et c# :

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Liens associés

- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Entrée API](xref:Xamarin.Forms.Entry)
