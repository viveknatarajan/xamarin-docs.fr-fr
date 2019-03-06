# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans **MainPage.xaml**, modifiez la déclaration [`Label`](xref:Xamarin.Forms.Label) pour modifier son apparence visuelle :

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Ce code définit les propriétés qui modifient l’apparence visuelle de [`Label`](xref:Xamarin.Forms.Label). La propriété [`TextColor`](xref:Xamarin.Forms.Label.TextColor) définit la couleur du texte `Button`. La propriété [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) définit la police de l’étiquette en italique et la propriété [`FontSize`](xref:Xamarin.Forms.Label.FontSize) définit la taille de police. En outre, un ornement de texte souligné est appliqué à `Label` en définissant sa propriété [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations), et le texte est centré horizontalement en définissant la propriété [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) sur [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application à l’intérieur du simulateur iOS distant ou de l’émulateur Android de votre choix. Vous remarquerez que l’apparence de [`Label`](xref:Xamarin.Forms.Label) a été modifiée :

    [![Capture d’écran d’une Label avec une apparence visuelle modifiée, sur iOS et Android](../images/change-label-appearance.png "Label avec une apparence modifiée")](../images/change-label-appearance-large.png#lightbox "Label avec une apparence modifiée")

    Pour plus d’informations sur la configuration de l’apparence de [`Label`](xref:Xamarin.Forms.Label), consultez le guide [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md) guide.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans **MainPage.xaml**, modifiez la déclaration [`Label`](xref:Xamarin.Forms.Label) pour modifier son apparence visuelle :

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Ce code définit les propriétés qui modifient l’apparence visuelle de [`Label`](xref:Xamarin.Forms.Label). La propriété [`TextColor`](xref:Xamarin.Forms.Label.TextColor) définit la couleur du texte `Button`. La propriété [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) définit la police de l’étiquette en italique et la propriété [`FontSize`](xref:Xamarin.Forms.Label.FontSize) définit la taille de police. En outre, un ornement de texte souligné est appliqué à `Label` en définissant sa propriété [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations), et le texte est centré horizontalement en définissant la propriété [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) sur [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton de lecture) pour lancer l’application dans le simulateur iOS ou dans l’émulateur Android de votre choix. Vous remarquerez que l’apparence de [`Label`](xref:Xamarin.Forms.Label) a été modifiée :

    [![Capture d’écran d’une Label avec une apparence visuelle modifiée, sur iOS et Android](../images/change-label-appearance.png "Label avec une apparence modifiée")](../images/change-label-appearance-large.png#lightbox "Label avec une apparence modifiée")

    Pour plus d’informations sur la configuration de l’apparence de [`Label`](xref:Xamarin.Forms.Label), consultez le guide [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md) guide.
