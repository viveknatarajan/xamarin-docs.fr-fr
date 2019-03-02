---
title: Effectuer la navigation dans une application Xamarin.Forms de plusieurs page
description: Cet article explique comment activer l’application à page unique, capable de stocker une seule note, dans une application comportant plusieurs page, capable de stocker plusieurs notes.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: f3c5ec1c6d37c74fd9b4fb4980c68a59c9864c05
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57197366"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>Effectuer la Navigation dans une Application Xamarin.Forms de plusieurs pages

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)

Dans ce démarrage rapide, vous allez apprendre comment :

- Ajouter des pages supplémentaires à une solution Xamarin.Forms.
- Effectuer la navigation entre les pages.
- Utilisez la liaison de données pour synchroniser les données entre les éléments d’interface utilisateur et de leur source de données.

Le démarrage rapide vous montre comment activer une application de Xamarin.Forms multiplateforme page unique, capable de stocker une seule note, dans une application comportant plusieurs page, capable de stocker plusieurs notes. L’application finale est indiquée ci-dessous :

[![](multi-page-images/screenshots1-sml.png "Page de commentaires")](multi-page-images/screenshots1.png#lightbox "Page de commentaires")
[![](multi-page-images/screenshots2-sml.png "Notez Page entrée")](multi-page-images/screenshots2.png#lightbox "Remarque Page d’entrée")

### <a name="prerequisites"></a>Prérequis

Vous devez mener à bien le [démarrage rapide précédent](single-page.md) avant de tenter de ce démarrage rapide. Vous pouvez également télécharger le [précédent exemple de démarrage rapide](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/) et l’utiliser comme point de départ pour ce démarrage rapide.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Mettre à jour l’application avec Visual Studio

1. Lancez Visual Studio. Sur la page de démarrage, cliquez sur **ouvrir un projet / Solution**, puis, dans le **ouvrir un projet** boîte de dialogue Sélectionnez le fichier de solution pour le projet de Notes :

    ![](multi-page-images/vs/open-solution.png "Ouvrir un projet")

2. Dans **l’Explorateur de solutions**, avec le bouton droit sur le **Notes** de projet et sélectionnez **Ajouter > Nouveau dossier**:

    ![](multi-page-images/vs/add-new-item.png "Ajouter un nouvel élément")

3. Dans **l’Explorateur de solutions**, nommez le nouveau dossier **modèles**:

    ![](multi-page-images/vs/name-folder.png "Dossier de modèles")

4. Dans **l’Explorateur de solutions**, sélectionnez le **modèles** dossier, avec le bouton droit et sélectionnez **Ajouter > nouvel élément...** :

    ![](multi-page-images/vs/add-new-models-file.png "Ajouter un nouveau fichier")

5. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **Visual C# éléments > classe**, nommez le nouveau fichier **Remarque**, puis cliquez sur le **ajouter** bouton :

    ![](multi-page-images/vs/add-note-class.png "Ajouter une classe de Remarque")

    Cela ajoutera une classe nommée **Remarque** à la **modèles** dossier de la **Notes** projet.

6. Dans **Note.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Cette classe définit un `Note` modèle qui stockera les données relatives à chaque note dans l’application.    

    Enregistrez les modifications apportées à **Note.cs** en appuyant sur **CTRL + S**et fermez le fichier.

7. Dans **l’Explorateur de solutions**, avec le bouton droit sur le **Notes** de projet et sélectionnez **Ajouter > nouvel élément...** . Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **Visual C# éléments > Xamarin.Forms > Page de contenu**, nommez le nouveau fichier **NoteEntryPage**, puis cliquez sur le  **Ajouter** bouton :

    ![](multi-page-images/vs/add-note-entry-page.png "Ajouter une Xamarin.Forms ContentPage")

    Cela ajoutera une nouvelle page nommée **NoteEntryPage** dans le dossier racine du projet. Cette page sera la deuxième page de l’application.

8. Dans **NoteEntryPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Ce code définit de manière déclarative l’interface utilisateur pour la page, ce qui se compose d’un [ `Editor` ](xref:Xamarin.Forms.Editor) pour l’entrée de texte et deux [ `Button` ](xref:Xamarin.Forms.Button) instances qui dirigent l’application à enregistrer ou supprimer un fichier. Les deux `Button` instances sont disposées horizontalement dans une [ `Grid` ](xref:Xamarin.Forms.Grid), avec le `Editor` et `Grid` en cours disposées verticalement dans une [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). En outre, le `Editor` utilise la liaison de données à lier à la `Text` propriété de la `Note` modèle. Pour plus d’informations sur la liaison de données, consultez [liaison de données](deepdive.md#data-binding) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

      Enregistrez les modifications apportées à **NoteEntryPage.xaml** en appuyant sur **CTRL + S**et fermez le fichier.

9. Dans **NoteEntryPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      Ce code stocke un `Note` instance, ce qui représente une seule note, dans le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la page. Lorsque le **enregistrer** [ `Button` ](xref:Xamarin.Forms.Button) est enfoncé le `OnSaveButtonClicked` Gestionnaire d’événements est exécuté, soit enregistre le contenu de la `Editor` dans un nouveau fichier avec un nom de fichier généré de manière aléatoire, ou à un fichier existant si la note est en cours de mise à jour. Dans les deux cas, le fichier est stocké dans le dossier de données d’application locale pour l’application. Puis la méthode revient à la page précédente. Lorsque le **supprimer** `Button` est enfoncé le `OnDeleteButtonClicked` Gestionnaire d’événements est exécuté, ce qui supprime le fichier, si elle existe et revient à la page précédente. Pour plus d’informations sur la navigation, consultez [Navigation](deepdive.md#navigation) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

      Enregistrez les modifications apportées à **NoteEntryPage.xaml.cs** en appuyant sur **CTRL + S**et fermez le fichier.

      > [!WARNING]
      > Une tentative de génération de l’application à ce stade provoquera des erreurs qui seront résolus dans les étapes suivantes.

10. Dans **l’Explorateur de solutions**, avec le bouton droit sur le **Notes** de projet et sélectionnez **Ajouter > nouvel élément...** . Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **Visual C# éléments > Xamarin.Forms > Page de contenu**, nommez le nouveau fichier **NotesPage**, puis cliquez sur le **ajouter**  bouton.

      Cela ajoutera une page nommée **NotesPage** dans le dossier racine du projet. Cette page sera la page racine de l’application.

11. Dans **NotesPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur pour la page, ce qui se compose d’un [ `ListView` ](xref:Xamarin.Forms.ListView) et un [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem). Le `ListView` utilise des données de liaison pour afficher toutes les notes sont récupérées par l’application, puis en sélectionnant une note permet d’accéder à la `NoteEntryPage` où la note peut être modifiée. Ou bien, une note peut être créée en appuyant sur la `ToolbarItem`. Pour plus d’informations sur la liaison de données, consultez [liaison de données](deepdive.md#data-binding) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

    Enregistrez les modifications apportées à **NotesPage.xaml** en appuyant sur **CTRL + S**et fermez le fichier.

12. Dans **NotesPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Ce code définit les fonctionnalités de la `NotesPage`. Lorsque la page s’affiche, le `OnAppearing` méthode est exécutée, qui remplit le [ `ListView` ](xref:Xamarin.Forms.ListView) avec toutes les remarques qui ont été récupérées à partir du dossier de données d’application locale. Lorsque le [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) est enfoncé le `OnNoteAddedClicked` Gestionnaire d’événements est exécuté. Cette méthode accède à la `NoteEntryPage`, ce qui affecte le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la `NoteEntryPage` vers un nouveau `Note` instance. Lorsqu’un élément dans le `ListView` est sélectionné le `OnListViewItemSelected` Gestionnaire d’événements est exécuté. Cette méthode accède à la `NoteEntryPage`, ce qui affecte le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la `NoteEntryPage` au `Note` instance. Pour plus d’informations sur la navigation, consultez [Navigation](deepdive.md#navigation) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

    Enregistrez les modifications apportées à **NotesPage.xaml.cs** en appuyant sur **CTRL + S**et fermez le fichier.

    > [!WARNING]
    > Une tentative de génération de l’application à ce stade provoquera des erreurs qui seront résolus dans les étapes suivantes.

13. Dans **l’Explorateur de solutions**, double-cliquez sur **App.xaml.cs** pour l’ouvrir. Puis remplacez le code existant par le code suivant :

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            ...
        }
    }
    ```

    Ce code ajoute une déclaration d’espace de noms pour le `System.IO` espace de noms et ajoute une déclaration pour une analyse statique `FolderPath` propriété de type `string`. Le `FolderPath` propriété sert à stocker le chemin d’accès sur le périphérique de stockage de noter que les données. En outre, le code initialise le `FolderPath` propriété dans le `App` constructeur et l’initialise le [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) propriété soit un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) qui héberge un instance de `NotesPage`. Pour plus d’informations sur la navigation, consultez [Navigation](deepdive.md#navigation) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

    Enregistrez les modifications apportées à **App.xaml.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

14. Dans **l’Explorateur de solutions**, dans le **Notes** du projet, avec le bouton droit **MainPage.xaml**, puis sélectionnez **supprimer**. Dans la boîte de dialogue qui s’affiche appuyez sur le **OK** bouton pour supprimer le fichier à partir de votre disque dur.

    Cette opération supprime une page qui n’est plus utilisée.

15. Générez et exécutez le projet sur chaque plateforme. Pour plus d’informations, consultez [création le démarrage rapide](single-page.md#building-the-quickstart).

    Sur le **NotesPage** appuyez sur le **+** bouton pour accéder à la **NoteEntryPage** et entrez une note. Une fois que l’enregistrement de la note de l’application permet d’accéder à la **NotesPage**.

    Entrez le nombre de notes, de longueur variable, pour observer le comportement de l’application.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mettre à jour l’application avec Visual Studio pour Mac

1. Lancez Visual Studio pour Mac. Sur la page de démarrage, cliquez sur **ouvrir...** et dans la boîte de dialogue, sélectionnez le fichier de solution pour le projet de Notes :

    ![](multi-page-images/vsmac/open-solution.png "Ouvrir une solution")

2. Dans le **panneau solutions**, sélectionnez le **Notes** projet, avec le bouton droit et sélectionnez **Ajouter > Nouveau dossier**:

    ![](multi-page-images/vsmac/add-new-folder.png "Ajouter un nouveau dossier")

3. Dans le **panneau solutions**, nommez le nouveau dossier **modèles**:

    ![](multi-page-images/vsmac/name-folder.png "Dossier de modèles")

4. Dans le **panneau solutions**, sélectionnez le **modèles** dossier, avec le bouton droit et sélectionnez **Ajouter > nouveau fichier...** :

    ![](multi-page-images/vsmac/add-new-models-file.png "Ajouter un nouveau fichier")

5. Dans le **nouveau fichier** boîte de dialogue, sélectionnez **général > classe vide**, nommez le nouveau fichier **Remarque**, puis cliquez sur le **New** bouton :

    ![](multi-page-images/vsmac/add-note-class.png "Ajouter une classe de Remarque")

    Cela ajoutera une classe nommée **Remarque** à la **modèles** dossier de la **Notes** projet.

6. Dans **Note.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    Cette classe définit un `Note` modèle qui stockera les données relatives à chaque note dans l’application.

    Enregistrez les modifications apportées à **Note.cs** en choisissant **fichier > Enregistrer** (ou en appuyant sur  **&#8984; + S**) et fermez le fichier.

7. Dans le **panneau solutions**, sélectionnez le **Notes** projet, avec le bouton droit et sélectionnez **Ajouter > nouveau fichier...** . Dans le **nouveau fichier** boîte de dialogue, sélectionnez **Forms > Forms ContentPage XAML**, nommez le nouveau fichier **NoteEntryPage**, puis cliquez sur le **New** bouton :

    ![](multi-page-images/vsmac/add-note-entry-page.png "Ajouter une Xamarin.Forms ContentPage")

    Cela ajoutera une nouvelle page nommée **NoteEntryPage** dans le dossier racine du projet. Cette page sera la deuxième page de l’application.

8. Dans **NoteEntryPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      Ce code définit de manière déclarative l’interface utilisateur pour la page, ce qui se compose d’un [ `Editor` ](xref:Xamarin.Forms.Editor) pour l’entrée de texte et deux [ `Button` ](xref:Xamarin.Forms.Button) instances qui dirigent l’application à enregistrer ou supprimer un fichier. Les deux `Button` instances sont disposées horizontalement dans une [ `Grid` ](xref:Xamarin.Forms.Grid), avec le `Editor` et `Grid` en cours disposées verticalement dans une [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). En outre, le `Editor` utilise la liaison de données à lier à la `Text` propriété de la `Note` modèle. Pour plus d’informations sur la liaison de données, consultez [liaison de données](deepdive.md#data-binding) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

      Enregistrez les modifications apportées à **NoteEntryPage.xaml** en choisissant **fichier > Enregistrer** (ou en appuyant sur  **&#8984; + S**) et fermez le fichier.

9. Dans **NoteEntryPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      Ce code stocke un `Note` instance, ce qui représente une seule note, dans le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la page. Lorsque le **enregistrer** [ `Button` ](xref:Xamarin.Forms.Button) est enfoncé le `OnSaveButtonClicked` Gestionnaire d’événements est exécuté, soit enregistre le contenu de la `Editor` dans un nouveau fichier avec un nom de fichier généré de manière aléatoire, ou à un fichier existant si la note est en cours de mise à jour. Dans les deux cas, le fichier est stocké dans le dossier de données d’application locale pour l’application. Puis la méthode revient à la page précédente. Lorsque le **supprimer** `Button` est enfoncé le `OnDeleteButtonClicked` Gestionnaire d’événements est exécuté, ce qui supprime le fichier, si elle existe et revient à la page précédente. Pour plus d’informations sur la navigation, consultez [Navigation](deepdive.md#navigation) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

      Enregistrez les modifications apportées à **NoteEntryPage.xaml.cs** en choisissant **fichier > Enregistrer** (ou en appuyant sur  **&#8984; + S**) et fermez le fichier.

      > [!WARNING]
      > Une tentative de génération de l’application à ce stade provoquera des erreurs qui seront résolus dans les étapes suivantes.

10. Dans le **panneau solutions**, sélectionnez le **Notes** projet, avec le bouton droit et sélectionnez **Ajouter > nouveau fichier...** . Dans le **nouveau fichier** boîte de dialogue, sélectionnez **Forms > Forms ContentPage XAML**, nommez le nouveau fichier **NotesPage**, puis cliquez sur le **New** bouton.

      Cela ajoutera une page nommée **NotesPage** dans le dossier racine du projet. Cette page sera la page racine de l’application.

11. Dans **NotesPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    Ce code définit de manière déclarative l’interface utilisateur pour la page, ce qui se compose d’un [ `ListView` ](xref:Xamarin.Forms.ListView) et un [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem). Le `ListView` utilise des données de liaison pour afficher toutes les notes sont récupérées par l’application, puis en sélectionnant une note permet d’accéder à la `NoteEntryPage` où la note peut être modifiée. Ou bien, une note peut être créée en appuyant sur la `ToolbarItem`. Pour plus d’informations sur la liaison de données, consultez [liaison de données](deepdive.md#data-binding) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

    Enregistrez les modifications apportées à **NotesPage.xaml** en choisissant **fichier > Enregistrer** (ou en appuyant sur  **&#8984; + S**) et fermez le fichier.

12. Dans **NotesPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    Ce code définit les fonctionnalités de la `NotesPage`. Lorsque la page s’affiche, le `OnAppearing` méthode est exécutée, qui remplit le [ `ListView` ](xref:Xamarin.Forms.ListView) avec toutes les remarques qui ont été récupérées à partir du dossier de données d’application locale. Lorsque le [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) est enfoncé le `OnNoteAddedClicked` Gestionnaire d’événements est exécuté. Cette méthode accède à la `NoteEntryPage`, ce qui affecte le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la `NoteEntryPage` vers un nouveau `Note` instance. Lorsqu’un élément dans le `ListView` est sélectionné le `OnListViewItemSelected` Gestionnaire d’événements est exécuté. Cette méthode accède à la `NoteEntryPage`, ce qui affecte le [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de la `NoteEntryPage` au `Note` instance. Pour plus d’informations sur la navigation, consultez [Navigation](deepdive.md#navigation) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

    Enregistrez les modifications apportées à **NotesPage.xaml.cs** en choisissant **fichier > Enregistrer** (ou en appuyant sur  **&#8984; + S**) et fermez le fichier.

    > [!WARNING]
    > Une tentative de génération de l’application à ce stade provoquera des erreurs qui seront résolus dans les étapes suivantes.

13. Dans le **panneau solutions**, double-cliquez sur **App.xaml.cs** pour l’ouvrir. Puis remplacez le code existant par le code suivant :

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            ...
        }
    }
    ```

    Ce code ajoute une déclaration d’espace de noms pour le `System.IO` espace de noms et ajoute une déclaration pour une analyse statique `FolderPath` propriété de type `string`. Le `FolderPath` propriété sert à stocker le chemin d’accès sur le périphérique de stockage de noter que les données. En outre, le code initialise le `FolderPath` propriété dans le `App` constructeur et l’initialise le [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) propriété soit un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) qui héberge un instance de `NotesPage`. Pour plus d’informations sur la navigation, consultez [Navigation](deepdive.md#navigation) dans le [présentation approfondie du Guide de démarrage rapide Xamarin.Forms](deepdive.md).

    Enregistrez les modifications apportées à **App.xaml.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

14. Dans le **panneau solutions**, dans le **Notes** du projet, avec le bouton droit **MainPage.xaml**, puis sélectionnez **supprimer**. Dans la boîte de dialogue qui s’affiche appuyez sur le **supprimer** bouton pour supprimer le fichier à partir de votre disque dur.

    Cette opération supprime une page qui n’est plus utilisée.

15. Générez et exécutez le projet sur chaque plateforme. Pour plus d’informations, consultez [création le démarrage rapide](single-page.md#building-the-quickstart).

    Sur le **NotesPage** appuyez sur le **+** bouton pour accéder à la **NoteEntryPage** et entrez une note. Une fois que l’enregistrement de la note de l’application permet d’accéder à la **NotesPage**.

    Entrez le nombre de notes, de longueur variable, pour observer le comportement de l’application.

::: zone-end

## <a name="next-steps"></a>Étapes suivantes

Dans ce démarrage rapide, vous avez appris comment :

- Ajouter des pages supplémentaires à une solution Xamarin.Forms.
- Effectuer la navigation entre les pages.
- Utilisez la liaison de données pour synchroniser les données entre les éléments d’interface utilisateur et de leur source de données.

Pour modifier l’application afin qu’il stocke ses données dans une base de données locale SQLite.NET, continuer au démarrage rapide suivant.

> [!div class="nextstepaction"]
> [Next](database.md)

## <a name="related-links"></a>Liens connexes

- [Notes (exemple)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)
- [Immersion dans Xamarin.Forms-démarrage rapide](deepdive.md)
