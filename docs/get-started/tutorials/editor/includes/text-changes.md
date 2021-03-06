---
ms.openlocfilehash: 19afeed47f1c06c89c58dfd996d360698b19fc9c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61373368"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans **MainPage.xaml**, modifiez la déclaration [`Editor`](xref:Xamarin.Forms.Editor) pour qu’elle définisse un gestionnaire pour les événements [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) et [`Completed`](xref:Xamarin.Forms.Editor.Completed) :

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    Ce code définit l’événement [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) sur un gestionnaire d’événements nommé `OnEditorTextChanged` et l’événement [`Completed`](xref:Xamarin.Forms.Editor.Completed) sur un gestionnaire d’événements nommé `OnEditorCompleted`. Les deux gestionnaires d’événements seront créés à l’étape suivante.

1. Dans **l’Explorateur de solutions**, dans le projet **EditorTutorial**, développez **MainPage.xaml**, puis double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir. Puis, dans **MainPage.xaml.cs**, ajoutez les gestionnaires d’événements `OnEditorTextChanged` et `OnEditorCompleted` à la classe :

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    Lorsque le texte de [`Editor`](xref:Xamarin.Forms.Editor) change, la méthode `OnEditorTextChanged` s’exécute. L’argument `sender` est l’objet `Editor` chargé de déclencher l’événement `TextChanged` ; il peut être utilisé pour accéder à l’objet `Editor`. L’argument [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) fournit les anciennes et les nouvelles valeurs de texte, avant et après le changement de texte.

    Lorsque la modification est terminée, la méthode `OnEditorCompleted` s’exécute. Pour y parvenir, vous devez décentrer [`Editor`](xref:Xamarin.Forms.Editor) ou appuyer sur le bouton « Terminé » sur iOS. L’argument `sender` est l’objet `Editor` chargé de déclencher l’événement `TextChanged` ; il peut être utilisé pour accéder à l’objet `Editor`.

    > [!IMPORTANT]
    > Tout texte saisi dans [`Editor`](xref:Xamarin.Forms.Editor) est stocké dans la propriété [`Text`](xref:Xamarin.Forms.Editor.Text).

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix :

    [![Capture d’écran d’un Editor contenant du texte, sur iOS et Android](../images/text-changes.png "Editor avec texte")](../images/text-changes-large.png#lightbox "Editor avec texte")

    Définissez des points d’arrêt dans les deux gestionnaires d’événements, entrez du texte dans [`Editor`](xref:Xamarin.Forms.Editor) et observez le déclenchement de l’événement [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged). Décentrez `Editor` pour observer le déclenchement de l’événement [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    Pour plus d’informations sur les événements [`Editor`](xref:Xamarin.Forms.Editor), consultez la rubrique [Événements et interactivité](~/xamarin-forms/user-interface/text/editor.md#interactivity) du guide [Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/editor.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans **MainPage.xaml**, modifiez la déclaration [`Editor`](xref:Xamarin.Forms.Editor) pour qu’elle définisse un gestionnaire pour les événements [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) et [`Completed`](xref:Xamarin.Forms.Editor.Completed) :

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    Ce code définit l’événement [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) sur un gestionnaire d’événements nommé `OnEditorTextChanged` et l’événement [`Completed`](xref:Xamarin.Forms.Editor.Completed) sur un gestionnaire d’événements nommé `OnEditorCompleted`. Les deux gestionnaires d’événements seront créés à l’étape suivante.

1. Dans **Panneau Solutions**, dans le projet **EditorTutorial**, développez **MainPage.xaml**, puis double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir. Puis, dans **MainPage.xaml.cs**, ajoutez les gestionnaires d’événements `OnEditorTextChanged` et `OnEditorCompleted` à la classe :

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    Lorsque le texte de [`Editor`](xref:Xamarin.Forms.Editor) change, la méthode `OnEditorTextChanged` s’exécute. L’argument `sender` est l’objet `Editor` chargé de déclencher l’événement `TextChanged` ; il peut être utilisé pour accéder à l’objet `Editor`. L’argument [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) fournit les anciennes et les nouvelles valeurs de texte, avant et après le changement de texte.

    Lorsque la modification est terminée, la méthode `OnEditorCompleted` s’exécute. Pour y parvenir, vous devez décentrer [`Editor`](xref:Xamarin.Forms.Editor) ou appuyer sur le bouton « Terminé » sur iOS. L’argument `sender` est l’objet `Editor` chargé de déclencher l’événement `TextChanged` ; il peut être utilisé pour accéder à l’objet `Editor`.

    > [!IMPORTANT]
    > Tout texte saisi dans [`Editor`](xref:Xamarin.Forms.Editor) est stocké dans la propriété [`Text`](xref:Xamarin.Forms.Editor.Text).

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix :

    [![Capture d’écran d’un Editor contenant du texte, sur iOS et Android](../images/text-changes.png "Editor avec texte")](../images/text-changes-large.png#lightbox "Editor avec texte")

    Définissez des points d’arrêt dans les deux gestionnaires d’événements, entrez du texte dans [`Editor`](xref:Xamarin.Forms.Editor) et observez le déclenchement de l’événement [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged). Décentrez `Editor` pour observer le déclenchement de l’événement [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    Pour plus d’informations sur les événements [`Editor`](xref:Xamarin.Forms.Editor), consultez la rubrique [Événements et interactivité](~/xamarin-forms/user-interface/text/editor.md#interactivity) du guide [Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/editor.md).
