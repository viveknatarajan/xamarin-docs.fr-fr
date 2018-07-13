---
title: Xamarin.Forms - Démarrage rapide
description: Cet article explique comment créer une application qui convertit un numéro de téléphone alphanumérique (entré par l’utilisateur) en numéro de téléphone numérique, puis compose le numéro.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 5b5f8c80e49d66ed3bd8b008c975d1cfeda93ed4
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38832382"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms - Démarrage rapide

Cette procédure pas à pas illustre comment créer une application qui convertit un numéro de téléphone alphanumérique (entré par l’utilisateur) en numéro de téléphone numérique, puis compose le numéro. L’application finale est indiquée ci-dessous :

[![](quickstart-images/intro-app-examples-sml.png "Application Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Application Phoneword")

Créez l’application Phoneword comme suit :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dans l’écran **Démarrer**, lancez Visual Studio. La page de démarrage s’ouvre :

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Dans Visual Studio, cliquez sur **Créer un projet...** pour créer un projet :

    ![](quickstart-images/vs/new-solution.png "Nouveau projet")

3. Dans la boîte de dialogue **Nouveau projet**, cliquez sur **Multiplateforme**, sélectionnez le modèle **Application mobile (Xamarin.Forms)**, définissez le Nom **Phoneword**, choisissez l’emplacement du projet et cliquez sur le bouton **OK** :

    ![](quickstart-images/vs/new-project.w157.png "Modèles de projet multiplateforme")

    > [!NOTE]
    > Si la solution ne porte pas le nom **Phoneword**, de nombreuses erreurs de build se produiront.

4. Dans la boîte de dialogue **Nouvelle application multiplateforme**, cliquez sur **Application vide**, sélectionnez **.NET Standard** comme stratégie de partage de code et cliquez sur le bouton **OK** :

    ![](quickstart-images/vs/new-app.png "Nouvelle application multiplateforme")

5. Dans l’**Explorateur de solutions**, dans le projet **Phoneword**, double-cliquez sur **MainPage.xaml** pour l’ouvrir :

    ![](quickstart-images/vs/open-mainpage-xaml.png "Ouvrir MainPage.xaml")

6. Dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code définit de manière déclarative l’interface utilisateur de la page :

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Enregistrez les modifications apportées à **MainPage.xaml** en appuyant sur **Ctrl+S** et fermez le fichier.

7. Dans l’**Explorateur de solutions**, développez **MainPage.xaml**, puis double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir :

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Ouvrir MainPage.xaml.cs")

8. Dans **MainPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Les méthodes `OnTranslate` et `OnCall` sont exécutées en réponse aux clics sur les boutons **Traduire** et **Appeler** dans l’interface utilisateur, respectivement :

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Une tentative de génération de l’application à ce stade provoque des erreurs qui seront corrigées plus tard.

    Enregistrez les modifications apportées à **MainPage.xaml.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

9. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword**, puis sélectionnez **Ajouter > Nouvel élément...**  :

    ![](quickstart-images/vs/add-new-item.png "Ajouter un nouvel élément")

10. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Visual C# > Code > Classe**, nommez le nouveau fichier **PhoneTranslator**, puis cliquez sur le bouton **Ajouter** :

    ![](quickstart-images/vs/add-translator-class.w157.png "Ajouter une nouvelle classe")

11. Dans **PhoneTranslator.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code convertit un mot composé sur le clavier du téléphone en numéro de téléphone :

    ```csharp
    using System.Text;

    namespace Phoneword
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneTranslator.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

12. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword**, puis sélectionnez **Ajouter > Nouvel élément...**  :

    ![](quickstart-images/vs/add-new-item.png "Ajouter un nouvel élément")

13. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Visual C# > Code > Interface**, nommez le nouveau fichier **IDialer**, puis cliquez sur le bouton **Ajouter** :

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Add New Interface") (Ajouter une nouvelle interface)

14. Dans **IDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code définit une méthode `Dial` qui doit être implémentée sur chaque plateforme pour composer un numéro de téléphone converti :

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    Enregistrez les modifications apportées à **IDialer.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

    > [!NOTE]
    > Le code commun pour l’application est maintenant complet. Le code du numéroteur téléphonique spécifique à la plateforme sera désormais implémenté comme [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword.iOS**, puis sélectionnez **Ajouter > Nouvel élément...**  :

    ![](quickstart-images/vs/add-new-item-ios.png "Ajouter un nouvel élément")

16. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Apple > Code > Classe**, nommez le nouveau fichier **PhoneDialer**, puis cliquez sur le bouton **Ajouter** :

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Ajouter une nouvelle classe")

17. Dans **PhoneDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code crée la méthode <code>Dial</code> qui sera utilisée sur la plateforme iOS pour composer un numéro de téléphone converti :

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneDialer.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

18. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword.Android**, puis sélectionnez **Ajouter > Nouvel élément...**  :

    ![](quickstart-images/vs/add-new-item-android.png "Ajouter un nouvel élément")

19. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Visual C# > Android > Classe**, nommez le nouveau fichier **PhoneDialer**, puis cliquez sur le bouton **Ajouter** :

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Ajouter une nouvelle classe")

20. Dans **PhoneDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code crée la méthode `Dial` qui sera utilisée sur la plateforme Android pour composer un numéro de téléphone converti :

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionDial);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneDialer.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

21. Dans **l’Explorateur de solutions**, dans le projet **Phoneword.Android**, double-cliquez sur **MainActivity.cs** pour l’ouvrir, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);
                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **MainActivity.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

22. Dans l’**Explorateur de solutions**, dans le projet **Phoneword.Android**, double-cliquez sur **Propriétés** et sélectionnez l’onglet **Manifeste Android** :

    ![](quickstart-images/vs/android-manifest.png "Ouvrir le manifeste Android")

23. Dans la section **Autorisations nécessaires**, activez l’autorisation **CALL_PHONE**. Cela autorise l’application à effectuer un appel téléphonique :

    ![](quickstart-images/vs/android-manifest-changed.png "Enable CallPhone Permission") (Activer l’autorisation CallPhone)

    Enregistrez les modifications apportées au manifeste en appuyant sur **Ctrl+S** et fermez le fichier.

24. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword.UWP**, puis sélectionnez **Ajouter > Nouvel élément...**  :

    ![](quickstart-images/vs/add-new-item-uwp.png "Ajouter un nouvel élément")

25. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Visual C# > Code > Classe**, nommez le nouveau fichier **PhoneDialer**, puis cliquez sur le bouton **Ajouter** :

    ![](quickstart-images/vs/new-phone-dialer-uwp.w157.png "Ajouter une nouvelle classe")

26. Dans **PhoneDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code crée la méthode `Dial` ainsi que les méthodes d’assistance qui seront utilisées sur la plateforme Windows universelle pour composer un numéro de téléphone converti :

    ```csharp
    using Phoneword.UWP;
    using System;
    using System.Threading.Tasks;
    using Windows.ApplicationModel.Calls;
    using Windows.UI.Popups;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.UWP
    {
        public class PhoneDialer : IDialer
        {
            bool dialled = false;

            public bool Dial(string number)
            {
                DialNumber(number);
                return dialled;
            }

            async Task DialNumber(string number)
            {
                var phoneLine = await GetDefaultPhoneLineAsync();
                if (phoneLine != null)
                {
                    phoneLine.Dial(number, number);
                    dialled = true;
                }
                else
                {
                    var dialog = new MessageDialog("No line found to place the call");
                    await dialog.ShowAsync();
                    dialled = false;
                }
            }

            async Task<PhoneLine> GetDefaultPhoneLineAsync()
            {
                var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                var lineId = await phoneCallStore.GetDefaultLineAsync();
                return await PhoneLine.FromIdAsync(lineId);
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneDialer.cs** en appuyant sur **Ctrl+S** et fermez le fichier.

27. Dans l’**Explorateur de solutions**, dans le projet **Phoneword.UWP**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**  :

    ![](quickstart-images/vs/uwp-add-reference.png "Ajouter une référence")

28. Dans la boîte de dialogue **Gestionnaire de références**, sélectionnez **Windows universel > Extensions > Windows Mobile Extensions for UWP** (Extensions Windows Mobile pour UWP) et cliquez sur le bouton **OK** :

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Ajouter des extensions Windows Mobile pour UWP")

29. Dans l’**Explorateur de solutions**, dans le projet **Phoneword.UWP**, double-cliquez sur **Package.appxmanifest** :

    ![](quickstart-images/vs/uwp-manifest.png "Ouvrir le manifeste UWP")

30. Dans la page **Fonctionnalités**, activez la fonctionnalité **Appel téléphonique**. Cela autorise l’application à effectuer un appel téléphonique :

    ![](quickstart-images/vs/uwp-manifest-changed.png "Activer la fonctionnalité Appel téléphonique")

    Enregistrez les modifications apportées au manifeste en appuyant sur **Ctrl+S** et fermez le fichier.

31. Dans Visual Studio, sélectionnez l’élément de menu **Générer > Générer la solution** (ou appuyez sur **Ctrl+Maj+B**). L’application est générée et un message de réussite s’affiche dans la barre d’état Visual Studio :

    ![](quickstart-images/vs/build-successful.png "Génération réussie")

    En cas d’erreurs, répétez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que l’application soit générée.

32. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword.UWP**, puis sélectionnez **Définir comme projet de démarrage** :

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Définir comme projet de démarrage")

33. Dans la barre d’outils Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application :

    ![](quickstart-images/vs/start.png "Barre d’outils Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Application Phoneword UWP")

34. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **Phoneword.Android**, puis sélectionnez **Définir comme projet de démarrage**.
35. Dans la barre d’outils de Visual Studio, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans un émulateur Android.
36. Si vous possédez un appareil iOS et répondez à la configuration requise Mac pour le développement Xamarin.Forms, utilisez une technique similaire pour déployer l’application sur l’appareil iOS. Vous pouvez aussi déployer l’application sur le [simulateur distant iOS](~/tools/ios-simulator.md).

    Remarque : Les appels téléphoniques ne sont pas pris en charge sur tous les simulateurs.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac et, dans la page de démarrage, cliquez sur **Nouveau projet...** pour créer un projet :

    ![](quickstart-images/xs/new-solution.png "Nouvelle solution")

2. Dans la boîte de dialogue **Choisir un modèle pour votre nouveau projet**, cliquez sur **Multiplateforme > Application**, sélectionnez le modèle **Application Forms vide**, puis cliquez sur le bouton **Suivant** :

    ![](quickstart-images/xs/choose-template.png "Choisir un modèle")

3. Dans la boîte de dialogue **Configurer une application Forms vide**, nommez la nouvelle application **Phoneword**, vérifiez que la case d'option **Utiliser .NET Standard** est sélectionnée, puis cliquez sur le bouton **Suivant** :

    ![](quickstart-images/xs/configure-app.png "Configurer l’application Formulaires")

4. Dans la boîte de dialogue **Configurer une nouvelle application Forms vide**, conservez **Phoneword** comme nom de la solution et du projet, choisissez l’emplacement du projet, puis cliquez sur le bouton **Créer** pour créer le projet :

    ![](quickstart-images/xs/configure-project.png "Configurer le projet Formulaires")

    > [!NOTE]
    > Si la solution et le projet ne portent pas le nom **Phoneword**, de nombreuses erreurs de build se produiront.

5. Dans le **Panneau Solutions**, double-cliquez sur **MainPage.xaml** pour l’ouvrir :

    ![](quickstart-images/xs/open-mainpage-xaml.png "Ouvrir MainPage.xaml")

6. Dans **MainPage.xaml**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code définit de manière déclarative l’interface utilisateur de la page :

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Enregistrez les modifications apportées à **MainPage.xaml** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

7. Dans le **Panneau Solutions**, double-cliquez sur **MainPage.xaml.cs** pour l’ouvrir :

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Ouvrir MainPage.xaml.cs")

8. Dans **MainPage.xaml.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Les méthodes `OnTranslate` et `OnCall` sont exécutées en réponse aux clics sur les boutons **Traduire** et **Appeler** dans l’interface utilisateur, respectivement :

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Une tentative de génération de l’application à ce stade provoque des erreurs qui seront corrigées plus tard.

    Enregistrez les modifications apportées à **MainPage.xaml.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

9. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword**, cliquez avec le bouton droit et sélectionnez **Ajouter > Nouveau fichier...**  :

    ![](quickstart-images/xs/add-new-translator-file.png "Ajouter un nouveau fichier")

10. Dans la boîte de dialogue **Nouveau fichier**, sélectionnez **Général > Classe vide**, nommez le nouveau fichier **PhoneTranslator**, puis cliquez sur le bouton **Nouveau** :

    ![](quickstart-images/xs/add-translator-class.png "Ajouter une nouvelle classe")

11. Dans **PhoneTranslator.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code convertit un mot composé sur le clavier du téléphone en numéro de téléphone :

    ```csharp
    using System.Text;

    namespace Phoneword
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneTranslator.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

12. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword**, cliquez avec le bouton droit et sélectionnez **Ajouter > Nouveau fichier...**  :

    ![](quickstart-images/xs/add-new-interface.png "Ajouter un nouveau fichier")

13. Dans la boîte de dialogue **Nouveau fichier**, sélectionnez **Général > Interface vide**, nommez le nouveau fichier **IDialer**, puis cliquez sur le bouton **Nouveau** :

    ![](quickstart-images/xs/add-idialer-interface.png "Add New Interface") (Ajouter une nouvelle interface)

14. Dans **IDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code définit une méthode `Dial` qui doit être implémentée sur chaque plateforme pour composer un numéro de téléphone converti :

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Enregistrez les modifications apportées à **IDialer.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

    > [!NOTE]
    > Le code commun pour l’application est maintenant complet. Le code du numéroteur téléphonique spécifique à la plateforme sera désormais implémenté comme [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword.iOS**, cliquez avec le bouton droit et sélectionnez **Ajouter > Nouveau fichier...**  :

    ![](quickstart-images/xs/add-new-file-ios.png "Ajouter un nouveau fichier")

16. Dans la boîte de dialogue **Nouveau fichier**, sélectionnez **Général > Classe vide**, nommez le nouveau fichier **PhoneDialer**, puis cliquez sur le bouton **Nouveau** :

    ![](quickstart-images/xs/new-phonedialer-ios.png "Ajouter une nouvelle classe")

17. Dans **PhoneDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code crée la méthode `Dial` qui sera utilisée sur la plateforme iOS pour composer un numéro de téléphone converti :

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneDialer.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

18. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword.Droid**, cliquez avec le bouton droit et sélectionnez **Ajouter > Nouveau fichier...**  :

    ![](quickstart-images/xs/add-new-file-android.png "Ajouter un nouveau fichier")

19. Dans la boîte de dialogue **Nouveau fichier**, sélectionnez **Général > Classe vide**, nommez le nouveau fichier **PhoneDialer**, puis cliquez sur le bouton **Nouveau** :

    ![](quickstart-images/xs/new-phonedialer-android.png "Ajouter une nouvelle classe")

20. Dans **PhoneDialer.cs**, supprimez tout le code du modèle et remplacez-le par le code suivant. Ce code crée la méthode `Dial` qui sera utilisée sur la plateforme Android pour composer un numéro de téléphone converti :

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionDial);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **PhoneDialer.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

21. Dans le **Panneau Solutions**, dans le projet **Phoneword.Droid**, double-cliquez sur **MainActivity.cs** pour l’ouvrir, supprimez tout le code du modèle et remplacez-le par le code suivant :

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);
                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }
    ```

    Enregistrez les modifications apportées à **MainActivity.cs** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

22. Dans le **Panneau Solutions**, développez le dossier **Propriétés** et double-cliquez sur le fichier **AndroidManifest.xml** :

    ![](quickstart-images/xs/android-manifest.png "Ouvrir le manifeste Android")

23. Dans la section **Autorisations nécessaires**, activez l’autorisation **CallPhone**. Cela autorise l’application à effectuer un appel téléphonique :

    ![](quickstart-images/xs/android-manifest-changed.png "Enable CallPhone Permission") (Activer l’autorisation CallPhone)

    Enregistrez les modifications apportées à **AndroidManifest.xml** en choisissant **Fichier > Enregistrer** (ou en appuyant sur **&#8984;+S**) et fermez le fichier.

24. Dans Visual Studio pour Mac, sélectionnez l’élément de menu **Générer > Générer tout** (ou appuyez sur **&#8984;+B**). L’application est générée et un message de réussite s’affiche dans la barre d’outils Visual Studio pour Mac.

    ![](quickstart-images/xs/build-successful.png "Génération réussie")

25. En cas d’erreurs, répétez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que l’application soit générée.
26. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans le simulateur iOS :

    ![](quickstart-images/xs/start.png "Barre d’outils Visual Studio pour Mac")
    ![](quickstart-images/xs/phoneword-result-ios.png "Simulateur iOS")

    Remarque : Les appels téléphoniques ne sont pas pris en charge dans le simulateur iOS.

27. Dans le **Panneau Solutions**, sélectionnez le projet **Phoneword.Droid**, cliquez avec le bouton droit et sélectionnez **Définir comme projet de démarrage** :

    ![](quickstart-images/xs/set-startup-project.png "Définir comme projet de démarrage")

28. Dans la barre d’outils Visual Studio pour Mac, appuyez sur le bouton **Démarrer** (le bouton triangulaire qui ressemble à un bouton Lire) pour lancer l’application dans un émulateur Android :

    ![](quickstart-images/xs/phoneword-result-android.png "Émulateur Android")

    Remarque : Les appels téléphoniques ne sont pas pris en charge dans les émulateurs Android.

-----

Félicitations ! Vous avez terminé une application Xamarin.Forms. La [rubrique suivante](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) de ce guide passe en revue les étapes qui ont été effectuées dans cette procédure pas à pas pour acquérir les notions de base du développement d’applications avec Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [Accès aux fonctionnalités natives avec DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
