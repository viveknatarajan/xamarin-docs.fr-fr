---
title: Éditeur
description: Entrée de texte multiligne
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: dc83defcb3eb69cf53c205793ce77029c0947c2f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="editor"></a>Éditeur

_Entrée de texte multiligne_

Le `Editor` contrôle est utilisé pour accepter l’entrée de plusieurs lignes. Cet article couvre :

- **[Personnalisation](#Customization)**  &ndash; options de couleur et de clavier.
- **[L’interactivité](#Interactivity)**  &ndash; les événements qui peuvent être écoutés pour fournir l’interactivité.

## <a name="customization"></a>Personnalisation

### <a name="setting-and-reading-text"></a>Définition et de lecture du texte

Éditeur, comme les autres affichages de présentation de texte, expose la `Text` propriété. `Text` peut être utilisé pour définir et lire le texte présenté par le `Editor`. L’exemple suivant illustre la définition du texte en XAML :

```xaml
<Editor Text="I am an Editor" />
```

En C# :

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Pour lire le texte, accéder à la `Text` propriété en c# :

```csharp
var text = MyEditor.Text;
```

### <a name="keyboards"></a>Claviers

Le clavier qui s’affiche lorsque les utilisateurs interagissent avec un `Editor` peut être définie par programme via le [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) propriété.

Les options pour le type de clavier sont :

- **Par défaut** &ndash; du clavier par défaut
- **Chat** &ndash; utilisé pour texto & endroits où emoji sont utiles
- **Messagerie** &ndash; utilisé lors de la saisie des adresses de messagerie
- **Numérique** &ndash; utilisé lors de la saisie de nombres
- **Téléphone** &ndash; utilisé lors de la saisie des numéros de téléphone
- **URL** &ndash; utilisé pour entrer les chemins d’accès de fichier et les adresses web

Il existe un [exemple de chaque clavier](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) dans notre section de recettes.

### <a name="colors"></a>Couleurs

`Editor` peut être défini pour utiliser une couleur d’arrière-plan personnalisée via le `BackgroundColor` propriété. Une attention particulière est nécessaire pour s’assurer que les couleurs sera utilisables sur chaque plateforme. Étant donné que chaque plateforme a des valeurs par défaut différentes pour la couleur du texte, vous devrez peut-être définir une couleur d’arrière-plan personnalisée pour chaque plateforme. Consultez [utilisation de la plateforme Tweaks](~/xamarin-forms/platform/device.md) pour plus d’informations sur l’optimisation de l’interface utilisateur pour chaque plateforme.

En C# :

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

En XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Éditeur de BackgroundColor exemple")

Assurez-vous que les couleurs d’arrière-plan et de texte que vous choisissez sont utilisables sur chaque plateforme et ne pas masquer tout texte d’espace réservé.

## <a name="interactivity"></a>Interactivité

`Editor` expose deux événements :

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; déclenché lorsque le texte modifié dans l’éditeur. Fournit le texte avant et après la modification.
- [Terminé](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; déclenché lorsque l’utilisateur a terminé d’entrée en appuyant sur la touche Retour sur le clavier.

### <a name="completed"></a>Terminé

Le `Completed` événement permet de réagir à la fin d’une interaction avec un `Editor`. `Completed` est déclenché lorsque l’utilisateur met fin à l’entrée avec un champ en saisissant la clé de retournée sur le clavier. Le gestionnaire pour l’événement est un gestionnaire d’événements générique, en prenant l’expéditeur et `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Il est possible de s’abonner à l’événement terminé dans le code et XAML :

En C# :

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

En XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

Le `TextChanged` événement permet de réagir à une modification dans le contenu d’un champ.

`TextChanged` est déclenché chaque fois que le `Text` de la `Editor` modifications. Le gestionnaire pour l’événement prend une instance de `TextChangedEventArgs`. `TextChangedEventArgs` fournit l’accès aux valeurs anciennes et nouvelles de la `Editor` `Text` via la `OldTextValue` et `NewTextValue` propriétés :

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Il est possible de s’abonner à l’événement terminé dans le code et XAML :

En code :

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

En XAML :

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>Liens associés

- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API de l’éditeur](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
