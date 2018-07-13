---
title: L’Interface de commande de Xamarin.Forms
description: Cet article explique comment implémenter la propriété Command avec Xamarin.Forms des liaisons de données. L’interface de commande fournit une alternative à l’implémentation des commandes qui est beaucoup mieux adaptée à l’architecture MVVM.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b18d042e34146a72b488da9017648a430c9cd353
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996371"
---
# <a name="the-xamarinforms-command-interface"></a>L’Interface de commande de Xamarin.Forms

Dans l’architecture Model-View-ViewModel (MVVM), les liaisons de données sont définies entre les propriétés dans le ViewModel, ce qui est généralement une classe qui dérive de `INotifyPropertyChanged`et les propriétés dans la vue, qui est généralement le fichier XAML. Parfois, une application a des besoins qui vont au-delà de ces liaisons de propriété en demandant aux utilisateurs de lancer des commandes qui affectent un élément dans le ViewModel. Ces commandes sont généralement signalés par les clics de bouton ou votre doigt TAP et en règle générale, ils sont traités dans le fichier code-behind dans un gestionnaire pour le `Clicked` événements de la `Button` ou le `Tapped` événement d’un `TapGestureRecognizer`.

L’interface de commande fournit une alternative à l’implémentation des commandes qui est beaucoup mieux adaptée à l’architecture MVVM. Le modèle ViewModel lui-même peut contenir des commandes, qui sont des méthodes qui sont exécutées en réaction à une activité spécifique dans la vue comme une `Button` cliquez sur. Liaisons de données sont définies entre deux de ces commandes et le `Button`.

Pour permettre une liaison de données entre un `Button` ainsi qu’un ViewModel, le `Button` définit deux propriétés :

- [`Command`](xref:Xamarin.Forms.Button.Command) de type <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) de type `Object`

Pour utiliser l’interface de commande, vous définissez une liaison de données qui cible le `Command` propriété de la `Button` où la source est une propriété dans le ViewModel de type `ICommand`. Code associé qui trouve dans le ViewModel `ICommand` propriété qui est exécutée lorsque le bouton est activé. Vous pouvez définir `CommandParameter` à des données arbitraires à faire la distinction entre plusieurs boutons s’ils sont tous liés au même `ICommand` propriété dans le ViewModel.

Le `Command` et `CommandParameter` propriétés sont également définies par les classes suivantes :

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) et par conséquent, [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem), qui dérive de `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) et par conséquent, [ `ImageCell` ](xref:Xamarin.Forms.ImageCell), qui dérive de `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) définit un [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand) propriété de type `ICommand` et un [ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) propriété. Le [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) propriété du [ `ListView` ](xref:Xamarin.Forms.ListView) est également de type `ICommand`.

Toutes ces commandes peuvent être gérés au sein d’un ViewModel d’une manière qui ne dépend pas l’objet d’interface utilisateur particulier dans la vue.

## <a name="the-icommand-interface"></a>L’Interface ICommand

Le <xref:System.Windows.Input.ICommand> interface ne fait pas partie de Xamarin.Forms. Il est défini à la place dans le [System.Windows.Input](xref:System.Windows.Input) espace de noms et se compose de deux méthodes et un événement :

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Pour utiliser l’interface de commande, votre ViewModel contient les propriétés du type `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

Le ViewModel doit également faire référence à une classe qui implémente le `ICommand` interface. Cette classe sera décrite sous peu. Dans la vue, le `Command` propriété d’un `Button` est lié à cette propriété :

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Lorsque l’utilisateur appuie sur le `Button`, le `Button` appelle le `Execute` méthode dans le `ICommand` objet lié à son `Command` propriété. C’est la partie la plus simple de l’interface de commande.

Le `CanExecute` méthode est plus complexe. Lorsque la liaison est tout d’abord définie sur le `Command` propriété de la `Button`, et lorsque la liaison de données est modifiée d’une certaine façon, le `Button` appelle le `CanExecute` méthode dans le `ICommand` objet. Si `CanExecute` retourne `false`, puis le `Button` se désactive. Cela indique que la commande particulière est actuellement pas disponible ou non valide.

Le `Button` attache également un gestionnaire sur le `CanExecuteChanged` événement de `ICommand`. L’événement est déclenché à partir de dans le ViewModel. Lorsque cet événement est déclenché, le `Button` appels `CanExecute` à nouveau. Le `Button` lui-même permet si `CanExecute` retourne `true` et se désactive si `CanExecute` retourne `false`.

> [!IMPORTANT]
> N’utilisez pas le `IsEnabled` propriété du `Button` si vous utilisez l’interface de commande.  

## <a name="the-command-class"></a>La classe de commande

Lorsque votre ViewModel définit une propriété de type `ICommand`, le ViewModel doit également contenir ou faire référence à une classe qui implémente le `ICommand` interface. Cette classe doit contenir ou faire référence à la `Execute` et `CanExecute` méthodes et les incendies le `CanExecuteChanged` événement chaque fois que le `CanExecute` méthode peut retourner une valeur différente.

Vous pouvez écrire une telle classe vous-même, ou vous pouvez utiliser une classe de quelqu'un d’autre a écrit. Étant donné que `ICommand` fait partie de Microsoft Windows, il a été utilisé pendant des années avec les applications MVVM de Windows. À l’aide d’une classe de Windows qui implémente `ICommand` vous permet de partager vos ViewModels entre les applications Windows et les applications Xamarin.Forms.

Si ViewModels entre Windows et Xamarin.Forms de partage n’est pas un problème, vous pouvez utiliser la [ `Command` ](xref:Xamarin.Forms.Command) ou [ `Command<T>` ](xref:Xamarin.Forms.Command`1) classe inclus dans Xamarin.Forms pour implémenter le `ICommand`interface. Ces classes vous permettent de spécifier les corps de la `Execute` et `CanExecute` méthodes dans les constructeurs de classe. Utilisez `Command<T>` lorsque vous utilisez le `CommandParameter` propriété faire la distinction entre plusieurs vues lié à la même `ICommand` propriété et le plus simple `Command` classe lorsque cela n’est pas une exigence.

## <a name="basic-commanding"></a>Exécution de commandes de base

Le **personne entrée** page dans le [ **démonstrations de liaison de données** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) de programme illustre des commandes simples, implémentées dans un ViewModel.

Le `PersonViewModel` définit trois propriétés nommées `Name`, `Age`, et `Skills` qui définissent une personne. Cette classe effectue *pas* contenir aucun `ICommand` propriétés :

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Le `PersonCollectionViewModel` illustré ci-dessous crée des objets de type `PersonViewModel` et autorise l’utilisateur à renseigner les données. Pour ce faire, la classe définit les propriétés `IsEditing` de type `bool` et `PersonEdit` de type `PersonViewModel`. En outre, la classe définit trois propriétés de type `ICommand` et une propriété nommée `Persons` de type `IList<PersonViewModel>`:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Cette liste abrégée n’inclut pas de constructeur de la classe, là où les trois propriétés de type `ICommand` sont définies, ce qui s’affichera sous peu. Notez que les modifications apportées aux trois propriétés de type `ICommand` et `Persons` propriété n’entraînent pas `PropertyChanged` événements déclenchés. Ces propriétés sont toutes définies lors de la création de la classe et ne changent pas par la suite.

Avant d’examiner le constructeur de la `PersonCollectionViewModel` class, examinons le fichier XAML pour le **personne entrée** programme. Ce fichier contient un `Grid` avec son `BindingContext` propriété définie sur le `PersonCollectionViewModel`. Le `Grid` contient un `Button` avec le texte **New** avec son `Command` propriété liée à la `NewCommand` un formulaire d’entrée avec les propriétés de propriété dans le ViewModel, liée à la `IsEditing` propriété, en tant que ainsi que les propriétés de `PersonViewModel`, et deux boutons plus lié à la `SubmitCommand` et `CancelCommand` propriétés du ViewModel. La dernière `ListView` affiche la collection de personnes déjà entré :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">

            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}"
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal"
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

Voici comment cela fonctionne : l’utilisateur appuie première sur le **New** bouton. Cela active le formulaire d’entrée, mais désactive le **New** bouton. L’utilisateur saisit ensuite un nom, l’âge et compétences. À tout moment pendant la modification, l’utilisateur peut appuyer sur la **Annuler** bouton de repartir de zéro. Uniquement quand un nom et un âge valid ont été entrées sont la **envoyer** bouton est activé. En appuyant sur cette **envoyer** bouton transfère la personne à la collection affichée par le `ListView`. Après que le **Annuler** ou **envoyer** bouton est enfoncé, le formulaire d’entrée est désactivé et le **New** bouton est activé à nouveau.

L’écran iOS à gauche montre la disposition avant un âge valid est entré. Afficher des écrans de l’Android et UWP le **envoyer** bouton est activé après avoir défini un âge :

[![Entrée de la personne](commanding-images/personentry-small.png "personne entrée")](commanding-images/personentry-large.png#lightbox "entrée à la personne")

Le programme n’a pas de toute installation pour la modification des entrées existantes et n’enregistre pas les entrées lorsque vous quittez la page.

Toute la logique pour le **New**, **envoyer**, et **Annuler** boutons sont gérée dans `PersonCollectionViewModel` dans les définitions de la `NewCommand`, `SubmitCommand`, et `CancelCommand` propriétés. Le constructeur de la `PersonCollectionViewModel` définit ces trois propriétés aux objets de type `Command`.  

Un [constructeur](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) de la `Command` classe vous permet de passer des arguments de type `Action` et `Func<bool>` correspondant à la `Execute` et `CanExecute` méthodes. Il est plus facile de définir ces actions et des fonctions en tant que fonctions de lambda directement dans le `Command` constructeur. Voici la définition de la `Command` de l’objet pour le `NewCommand` propriété :

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }

    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Lorsque l’utilisateur clique sur le **New** bouton, le `execute` fonction passé à la `Command` constructeur est exécuté. Cette action crée une `PersonViewModel` d’objet, définit un gestionnaire sur l’objet `PropertyChanged` définit des événements, `IsEditing` à `true`et appelle le `RefreshCanExecutes` méthode définie après le constructeur.

Outre la mise en œuvre le `ICommand` interface, le `Command` classe définit également une méthode nommée `ChangeCanExecute`. Votre ViewModel doit appeler `ChangeCanExecute` pour un `ICommand` propriété chaque fois que quelque chose produit qui peut changer la valeur de retour de la `CanExecute` (méthode). Un appel à `ChangeCanExecute` provoque la `Command` classe afin de déclencher le `CanExecuteChanged` (méthode). Le `Button` a joint un gestionnaire pour cet événement et répond en appelant `CanExecute` à nouveau, puis activation lui-même en fonction de la valeur de retour de cette méthode.

Lorsque le `execute` méthode de `NewCommand` appels `RefreshCanExecutes`, le `NewCommand` propriété reçoit un appel à `ChangeCanExecute`et le `Button` appels le `canExecute` (méthode), qui retourne maintenant `false` , car le `IsEditing`propriété est désormais `true`.

Le `PropertyChanged` gestionnaire pour la nouvelle `PersonViewModel` object appelle le `ChangeCanExecute` méthode de `SubmitCommand`. Voici comment cette propriété de la commande est implémentée :


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null &&
                       PersonEdit.Name != null &&
                       PersonEdit.Name.Length > 1 &&
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

Le `canExecute` fonctionner pour `SubmitCommand` est appelée chaque fois qu’il existe une propriété modifiée dans le `PersonViewModel` de l’objet en cours de modification. Elle retourne `true` uniquement lorsque le `Name` propriété est au moins un caractère de long, et `Age` est supérieur à 0. À ce stade, le **envoyer** bouton devient disponible.

Le `execute` fonctionner pour **envoyer** supprime le Gestionnaire de modification de propriété à partir de la `PersonViewModel`, ajoute l’objet à le `Persons` collection et renvoie tout en conditions initiales.

Le `execute` fonctionner pour la **Annuler** bouton fait tout ce que le **envoyer** bouton effectue execept ajouter l’objet à la collection :

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            });
    }

    ···

}
```

Le `canExecute` retourne de la méthode `true` à tout moment un `PersonViewModel` est en cours de modification.

Ces techniques pourraient être adaptées à des scénarios plus complexes : une propriété dans `PersonCollectionViewModel` peut être lié à la `SelectedItem` propriété de la `ListView` pour la modification des éléments existants et un **supprimer** bouton peut être ajouté à supprimer ces éléments.

Il n’est pas nécessaire de définir le `execute` et `canExecute` méthodes en tant que fonctions lambda. Vous pouvez les écrire des méthodes privées régulières dans le ViewModel et les référencer dans le `Command` constructeurs. Toutefois, cette approche ont tendance à générer un grand nombre de méthodes qui sont référencés en une seule fois dans le ViewModel.

## <a name="using-command-parameters"></a>À l’aide des paramètres de commande

Il est parfois commode pour un ou plusieurs boutons (ou d’autres objets d’interface utilisateur) partager le même `ICommand` propriété dans le ViewModel. Dans ce cas, vous utilisez le `CommandParameter` propriété faire la distinction entre les boutons.

Vous pouvez continuer à utiliser le `Command` classe pour ces éléments `ICommand` propriétés. La classe définit un [constructeur alternatif](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})) qui accepte `execute` et `canExecute` méthodes avec des paramètres de type `Object`. Voici comment la `CommandParameter` est passé à ces méthodes.

Toutefois, lorsque vous utilisez `CommandParameter`, il est plus facile à utiliser le modèle générique [ `Command<T>` ](xref:Xamarin.Forms.Command`1) classe pour spécifier le type de l’objet défini sur `CommandParameter`. Le `execute` et `canExecute` les méthodes que vous spécifiez ont des paramètres de ce type.

Le **clavier décimal** page illustre cette technique en montrant comment implémenter un pavé numérique pour la saisie de nombres décimaux. Le `BindingContext` pour le `Grid` est un `DecimalKeypadViewModel`. Le `Entry` propriété de ce ViewModel est liée à la `Text` propriété d’un `Label`. Tous les `Button` objets sont liés à plusieurs commandes dans le ViewModel : `ClearCommand`, `BackspaceCommand`, et `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2"
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="8" />

        <Button Text="9"
                Grid.Row="2" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

Les 11 boutons pour les 10 chiffres et de la virgule décimale partagent une liaison à `DigitCommand`. Le `CommandParameter` fait la distinction entre ces boutons. La valeur est définie sur `CommandParameter` est généralement le même que le texte affiché par le bouton à l’exception de la virgule décimale, qui s’affiche avec un caractère de point central pour des raisons de clarté.

Voici le programme en action :

[![Clavier décimal](commanding-images/decimalkeyboard-small.png "clavier décimal")](commanding-images/decimalkeyboard-large.png#lightbox "clavier décimal")

Notez que le bouton de la virgule décimale dans toutes les captures de trois écran est désactivé, car le nombre entré contient déjà une virgule décimale.

Le `DecimalKeypadViewModel` définit un `Entry` propriété de type `string` (qui est la seule propriété qui déclenche un `PropertyChanged` événement) et trois propriétés de type `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Le bouton correspondant à la `ClearCommand` est toujours activée et se contente de définir l’entrée à « 0 » :

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···

    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···

}
```

Étant donné que le bouton est toujours activé, il n’est pas nécessaire de spécifier un `canExecute` argument dans le `Command` constructeur.

La logique de saisie de numéros et un retour arrière est un peu difficile, car si aucun chiffre n’ont été entrées, puis le `Entry` propriété est la chaîne « 0 ». Si l’utilisateur tape des zéros non significatifs, plus le `Entry` contient toujours un zéro. Si l’utilisateur tape un autre chiffre, ce chiffre remplace le zéro. Toutefois, si l’utilisateur tape une virgule décimale avant tout autre chiffre, puis `Entry` est la chaîne « 0 ».

Le **retour arrière** bouton est activé uniquement lorsque la longueur de l’entrée est supérieure à 1, ou si `Entry` n’est pas égal à la chaîne « 0 » :

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···

}
```

La logique pour le `execute` fonctionner pour la **retour arrière** bouton garantit que le `Entry` est au moins une chaîne « 0 ».

Le `DigitCommand` propriété est liée aux 11 boutons, chacun d’eux s’identifie avec le `CommandParameter` propriété. Le `DigitCommand` peut être définie sur une instance de la mise à jour `Command` classe, mais plus facile à utiliser le `Command<T>` classe générique. Lorsque vous utilisez l’interface de commande avec XAML, le `CommandParameter` propriétés sont généralement des chaînes, et qui est le type de l’argument générique. Le `execute` et `canExecute` puis, les fonctions ont des arguments de type `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···

}
```

Le `execute` méthode ajoute l’argument de chaîne pour le `Entry` propriété. Toutefois, si le résultat commence par un zéro (mais pas d’un zéro et de virgule décimale) puis que zéro initial doit être supprimé à l’aide de la `Substring` (fonction).

Le `canExecute` retourne de la méthode `false` uniquement si l’argument est la virgule décimale (indiquant que la virgule décimale est enfoncée) et `Entry` contient déjà une virgule décimale.

Tous les `execute` appel de méthodes `RefreshCanExecutes`, qui appelle ensuite `ChangeCanExecute` pour les deux `DigitCommand` et `ClearCommand`. Cela garantit que la virgule décimale et les boutons de retour arrière sont activées ou désactivées en fonction de la séquence de chiffres entrés en cours.

## <a name="adding-commands-to-existing-views"></a>Ajout de commandes à des vues existantes

Si vous souhaitez utiliser l’interface de commande avec des vues qui ne prennent en charge, il est possible d’utiliser un comportement de Xamarin.Forms qui convertit un événement dans une ligne de commande. Cette opération est décrite dans l’article [ **réutilisable EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchrone des commandes des Menus de Navigation

Exécution de commandes est pratique pour implémenter des menus de navigation, tel que celui de la [ **démonstrations de liaison de données** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) programme lui-même. Voici une partie de **MainPage.xaml**:


```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

Lors de l’utilisation des commandes avec XAML, `CommandParameter` propriétés sont généralement définies en chaînes. Dans ce cas, toutefois, une extension de balisage XAML est utilisée afin que le `CommandParameter` est de type `System.Type`.

Chaque `Command` propriété est liée à une propriété nommée `NavigateCommand`. Que la propriété est définie dans le fichier code-behind, **MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Le constructeur affecte la `NavigateCommand` propriété à un `execute` méthode qui instancie le `System.Type` paramètre, puis accède à ce dernier. Étant donné que le `PushAsync` appel nécessite un `await` opérateur, la `execute` méthode doit être marquée comme asynchrone. Pour cela, avec la `async` mot clé avant la liste de paramètres.

Le constructeur définit également la `BindingContext` de la page pour lui-même afin que les liaisons de référencent le `NavigateCommand` dans cette classe.

L’ordre du code dans ce constructeur fait une différence : le `InitializeComponent` appel entraîne le XAML à analyser, mais à ce moment-là, la liaison à une propriété nommée `NavigateCommand` ne peut pas être résolu car `BindingContext` est défini sur `null`. Si le `BindingContext` est défini dans le constructeur *avant* `NavigateCommand` est définie, la liaison peut être résolu lorsque `BindingContext` est définie, mais à ce moment-là, `NavigateCommand` est toujours `null`. Paramètre `NavigateCommand` après `BindingContext` n’aura aucun effet sur la liaison, car une modification apportée à `NavigateCommand` ne déclenche pas une `PropertyChanged` événements et la liaison ne sait pas ça `NavigateCommand` est désormais valide.

Définition des deux valeurs `NavigateCommand` et `BindingContext` (dans n’importe quel ordre) avant l’appel à `InitializeComponent` fonctionnera, car les deux composants de la liaison sont définies lorsque l’analyseur XAML rencontre la définition de liaison.

Liaisons de données peuvent parfois être difficile, mais comme vous l’avez vu dans cette série d’articles, ils sont puissantes et polyvalentes et vous aider à organiser votre code en séparant la logique sous-jacente à partir de l’interface utilisateur.



## <a name="related-links"></a>Liens associés

- [Démonstrations de liaison de données (exemple)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Chapitre de liaison de données à partir du livre de Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
