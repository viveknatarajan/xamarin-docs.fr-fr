---
title: Éditeur
description: Entrée de texte multiligne
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 317d4f140daeccc525c4267fca43e6164a8f7827
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848315"
---
# <a name="editor"></a>Éditeur

_Entrée de texte multiligne_

Le `Editor` contrôle est utilisé pour accepter l’entrée de plusieurs lignes. Cet article couvre :

- **[Personnalisation](#customization)**  &ndash; options de couleur et de clavier.
- **[L’interactivité](#interactivity)**  &ndash; les événements qui peuvent être écoutés pour fournir l’interactivité.

## <a name="customization"></a>Personnalisation

### <a name="setting-and-reading-text"></a>Définition et de lecture du texte

Le `Editor`, telles que d’autres affichages de présentation de texte, expose la `Text` propriété. Cette propriété peut être utilisée pour définir et lire le texte présenté par le `Editor`. L’exemple suivant illustre la définition du `Text` propriété en XAML :

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

### <a name="limiting-input-length"></a>Limitation de longueur d’entrée

Le [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) propriété peut être utilisée pour limiter la longueur d’entrée qui est autorisée pour le [ `Editor` ](xref:Xamarin.Forms.Editor). Cette propriété doit être définie à un entier positif :

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) propriété la valeur 0 indique qu’aucune entrée ne sera autorisée et la valeur `int.MaxValue`, qui est la valeur par défaut pour un [ `Editor` ](xref:Xamarin.Forms.Editor), indique qu’il y a aucun limite effective du nombre de caractères pouvant être entrés.

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

### <a name="enabling-and-disabling-spell-checking"></a>Activation et désactivation de la vérification orthographique

Le [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) propriété contrôle si vérification orthographique est activée. Par défaut, la propriété est définie `true`. Comme l’utilisateur entre du texte, les fautes d’orthographe sont indiquées.

Toutefois, pour certains scénarios de saisie de texte, telles que saisir un nom d’utilisateur, la correction orthographique dans fournit une expérience négative et donc doivent être désactivés en définissant le [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) propriété `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Lorsque le [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) est définie sur `false`et un clavier personnalisé n’est pas utilisé, le vérificateur d’orthographe natif sera désactivé. Toutefois, si un [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) a été ensemble qui désactive l’orthographe sur la vérification, tel que [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), le `IsSpellCheckEnabled` propriété est ignorée. Par conséquent, la propriété ne peut pas être utilisée pour activer la correction orthographique pour un `Keyboard` qui désactive explicitement.

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
