---
title: "Hello, Android - Démarrage rapide"
description: "Dans ce guide en deux parties, vous allez créer votre première application Xamarin.Android (à l’aide de Visual Studio ou de Visual Studio pour Mac), et approfondir votre compréhension des principes fondamentaux du développement d’applications Android avec Xamarin. Vous y découvrirez les différents outils, concepts et étapes qui sont nécessaires à la création et au déploiement d’une application Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 2ec01091314df64070cafb570f01e54634759c77
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="hello-android-quickstart"></a>Hello, Android - Démarrage rapide

_Dans ce guide en deux parties, vous allez créer votre première application Xamarin.Android (à l’aide de Visual Studio ou de Visual Studio pour Mac), et approfondir votre compréhension des principes fondamentaux du développement d’applications Android avec Xamarin. Vous y découvrirez les différents outils, concepts et étapes qui sont nécessaires à la création et au déploiement d’une application Xamarin.Android._

## <a name="hello-android-quickstart"></a>Hello, Android - Démarrage rapide

Dans cette procédure pas à pas, vous allez créer une application qui permet de convertir un numéro de téléphone alphanumérique (entré par l’utilisateur) en un numéro de téléphone numérique, puis de l’afficher à l’écran. L’application finale ressemble à ceci :

[![Capture d’écran de l’application une fois terminée](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png)


## <a name="requirements"></a>Configuration requise

Pour effectuer cette procédure pas à pas, vous avez besoin des éléments suivants :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 ou ultérieur

-   Visual Studio 2015 Professional ou ultérieur

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

-   La dernière version de Visual Studio pour Mac

-   OS X Yosemite ou ultérieur

-----

Cette procédure pas à pas part du principe que la dernière version de Xamarin.Android est installée, et qu’elle est actuellement exécutée sur la plateforme que vous avez choisie. Pour savoir comment installer Xamarin.Android, reportez-vous aux [Guides d’installation de Xamarin.Android](~/android/get-started/installation/index.md).
Avant de commencer, téléchargez et décompressez le fichier contenant les [icônes et écrans de lancement de l’application Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true).

## <a name="configuring-emulators"></a>Configuration des émulateurs

Si vous utilisez l’émulateur du SDK Android de Google, nous vous recommandons de le configurer de manière à utiliser l’accélération matérielle. Pour obtenir des instructions concernant la configuration de l’accélération matérielle, consultez [Accélération matérielle](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Si vous utilisez l’émulateur Android de Visual Studio, Hyper-V doit être activé sur votre ordinateur. Pour plus d’informations sur la configuration de l’émulateur Android de Visual Studio, consultez [Configuration système requise pour l’émulateur Visual Studio pour Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

-----

## <a name="walkthrough"></a>Procédure pas à pas

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Démarrez Visual Studio.  Pour créer un projet, cliquez sur **Fichier >Nouveau > Projet**.

Dans la boîte de dialogue **Nouveau projet**, cliquez sur le modèle **Application vide (Android)**.
Nommez le nouveau projet `Phoneword`. Cliquez sur **OK** pour créer le projet :

[![Le nouveau projet se nomme Phoneword](hello-android-quickstart-images/vs/02-new-project-name-sml.png)](hello-android-quickstart-images/vs/02-new-project-name.png)

### <a name="creating-the-layout"></a>Création de la disposition

Une fois le projet créé, développez le dossier **Ressources**, puis le dossier **Disposition** dans l’**Explorateur de solutions**.
Double-cliquez sur **Main.axml** pour l’ouvrir dans Android Designer. Voici le fichier de disposition pour l’écran de l’application :

[![Ouverture du fichier Main.axml](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png)

Dans la **boîte à outils** (la zone située à gauche), entrez `text` dans le champ de recherche, puis faites glisser un widget **Text (Large)** dans l’aire de conception (la zone située au centre) :

[![Ajout du widget Text (Large)](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png)

Après avoir sélectionné le contrôle **Text (Large)** dans l’aire de conception, accédez au volet **Propriétés** pour remplacer la propriété `text` du widget **Text (Large)** par la propriété `Enter a Phoneword:`, comme indiqué ici :

[![Définition des propriétés du widget Text (Large)](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png)

Faites glisser un widget **Plain Text** de la **boîte à outils** vers l’aire de conception, puis placez-le sous le widget **Text (Large)** :

[![Ajout du widget Plain Text](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png)

Après avoir sélectionné le widget **Plain Text** dans l’aire de conception, accédez au volet **Propriétés** pour remplacer la propriété `id` du widget **Plain Text** par la propriété `@+id/PhoneNumberText`, et la propriété `text` par la propriété `1-855-XAMARIN` :

[![Définition des propriétés du widget Plain Text](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png)

Faites glisser un **bouton** de la **boîte à outils** vers l’aire de conception, puis placez-le sous le widget **Plain Text** :

[![Glissement du widget Button vers l’aire de conception](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png)

Après avoir sélectionné le widget **Button** dans l’aire de conception, accédez au volet **Propriétés** pour remplacer la propriété `id` du widget **Button** par la propriété `@+id/TranslateButton`, et la propriété `text` par la propriété `Translate` :

[![Définition des propriétés du widget Button](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png)

Faites glisser le widget **TextView** de la **boîte à outils** vers l’aire de conception, puis placez-le sous le widget **Button**. Définissez la propriété `id` du widget **TextView** sur `@+id/TranslatedPhoneWord`, et remplacez la valeur `text` par une chaîne vide :

[![Définition des propriétés du widget TextView](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png)    

Enregistrez votre travail en appuyant sur **CTRL + S**.

### <a name="writing-translation-code"></a>Écriture de code de traduction

L’étape suivante consiste à ajouter du code pour convertir des numéros de téléphone alphanumériques en numéros de téléphone numériques. Ajoutez un fichier au projet en cliquant sur le projet **Phoneword** dans l**l’Explorateur de solutions**, puis en sélectionnant **Ajouter > Nouvel élément...** , comme indiqué ci-dessous :

[![Ajout d’un nouvel élément](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png)

Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Visual C# > Code**, puis nommez le nouveau fichier de code **PhoneTranslator.cs** :

[![Ajout du fichier PhoneTranslator.cs](hello-android-quickstart-images/vs/14-add-class-sml.png)](hello-android-quickstart-images/vs/14-add-class.png)

Cette opération crée une nouvelle classe C# vide. Insérez le code suivant dans ce fichier :

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                }
                // otherwise we've skipped a non-numeric char
            }
            return newNumber.ToString();
        }
        static bool Contains (this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }
        static int? TranslateToNumber(char c)
        {
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Enregistrez les modifications apportées au fichier **PhoneTranslator.cs** en cliquant sur **Fichier > Enregistrer** (ou en appuyant sur **CTRL + S**), puis fermez le fichier.

### <a name="wiring-up-the-interface"></a>Câblage de l’interface

L’étape suivante consiste à ajouter du code pour structurer l’interface utilisateur en insérant du code de stockage dans la classe `MainActivity`. Commencez par configurer le bouton **Translate**. Dans la classe `MainActivity`, recherchez la méthode `OnCreate`. L’étape suivante consiste à ajouter le code du bouton à l’intérieur de `OnCreate`, sous les appels `base.OnCreate(bundle)` et `SetContentView
(Resource.Layout.Main)`. Tout d’abord, modifiez le code du modèle pour que la méthode `OnCreate` ressemble à ceci :

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // New code will go here
        }
    }
}
```

Récupérez une référence aux contrôles qui ont été créés dans le fichier de disposition via Android Designer. Ajoutez le code suivant dans la méthode `OnCreate`, immédiatement après l’appel à `SetContentView` :

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Ajoutez le code de réponse à l’actionnement du bouton **Translate**.
Ajoutez le code suivant à la méthode `OnCreate` (après les lignes ajoutées à l’étape précédente) :

```csharp
// Add code to translate number
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    string translatedNumber = Core.PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Enregistrez votre travail en sélectionnant **Fichier > Enregistrer tout** (ou en appuyant sur **CTRL-MAJ- S**). Ensuite, générez l’application en sélectionnant **Générer > Regénérer la solution** (ou en appuyant sur **CTRL-MAJ-B**). 

En cas d’erreurs, examinez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que l’application soit générée. S’il s’agit d’une erreur de génération, telle que _(Ressource) n’existe pas dans le contexte actuel_, vérifiez que le nom de l’espace de noms dans **MainActivity.cs** correspond bien au nom du projet (`Phoneword`), puis regénérez entièrement la solution. Si vous obtenez encore des erreurs de génération, vérifiez que vous avez installé les dernières mises à jour Xamarin.Android.

### <a name="setting-the-label-and-app-icon"></a>Définition du libellé et de l’icône de l’application

Votre application doit maintenant être opérationnelle : il est temps de lui ajouter la touche finale ! Dans **MainActivity.cs**, modifiez le `Label` de `MainActivity`. Le `Label` est ce qu’Android affiche en haut de l’écran pour permettre aux utilisateurs de savoir où ils se trouvent dans l’application.
En haut de la classe `MainActivity`, remplacez `Label` par `Phone Word`, comme indiqué ici :

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

À présent, nous allons définir l’icône de l’application. Par défaut, Visual Studio fournit une icône pour le projet. Nous allons supprimer ces fichiers de la solution et les remplacer par une autre icône. Développez le dossier **Ressources** dans le **Panneau Solutions**. Notez qu’il y a cinq dossiers dont le nom est précédé de **mipmap-**, et que chacun de ces dossiers contient un fichier **Icon.png** :

[![Dossiers mipmap- et fichiers Icon.png](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png)

Vous devez supprimer tous les fichiers d’icône du projet. Cliquez avec le bouton droit sur chaque fichier **Icon.png**, puis sélectionnez **Supprimer** dans le menu contextuel :
   
[![Suppression des fichiers Icon.png par défaut](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png)
   
Dans la boîte de dialogue, cliquez sur le bouton **Supprimer**.

Ensuite, téléchargez et décompressez les [icônes de l’application Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Ce fichier .zip contient les icônes de l’application. Toutes les icônes sont visuellement identiques. Toutefois, chacune a une résolution différente, adaptée à la densité d’écran des différents appareils sur lesquels l’application peut être utilisée.  Ces fichiers d’icône doivent être copiés dans le projet Xamarin.Android. Dans Visual Studio, dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **mipmap-hdpi**, puis sélectionnez **Ajouter > Élément existant** :

[![Ajout de fichiers](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png)

Dans la boîte de dialogue de sélection, accédez au dossier non décompressé qui contient les icônes d’application Xamarin, puis ouvrez le dossier **mipmap-hdpi**. Sélectionnez **Icon.png**, puis cliquez sur **Ajouter**.

Répétez ces étapes pour chaque dossier **mipmap-**, jusqu’à ce que le contenu des dossiers d’icônes d’application Xamarin **mipmap-** soit copié vers les dossiers **mipmap-** correspondants dans le projet **Phoneword**.

Une fois que toutes les icônes sont copiées dans le projet Xamarin.Android, ouvrez la boîte de dialogue **Options du projet** en cliquant avec le bouton droit sur le projet dans le **Panneau Solutions**. Sélectionnez **Générer > Application Android**, puis sélectionnez **@mipmap/icon** à partir de la zone de liste déroulante **Icône de l’application** :

[![Définition de l’icône du projet](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png)

### <a name="running-the-app"></a>Exécution de l’application

Enfin, testez l’application en l’exécutant sur un émulateur ou un appareil Android, et en convertissant un numéro de téléphone alphanumérique :

[![Capture d’écran de l’application une fois terminée](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lancez Visual Studio pour Mac à partir du dossier **Applications** ou à partir de **Spotlight**. 

Pour créer un projet, cliquez sur **Nouvelle solution...**.

Dans la boîte de dialogue **Choisir un modèle pour votre nouveau projet**, cliquez sur **Android > Application**, puis sélectionnez le modèle **Application Android**. Cliquez sur **Suivant**.

[![Sélection du modèle Application Android](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png)

Dans la boîte de dialogue **Configurer votre application Android**, attribuez à la nouvelle application le nom de `Phoneword`, puis cliquez sur **Suivant**.

[![Configuration de l’application Android](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png)

Dans la boîte de dialogue **Configurer votre application Android**, conservez le nom de la solution et du projet (`Phoneword`), puis cliquez sur **Créer** pour créer le projet.

### <a name="creating-the-layout"></a>Création de la disposition

Une fois le projet créé, développez le dossier **Ressources**, puis le dossier **Disposition** dans le **Panneau Solutions**.
Double-cliquez sur **Main.axml** pour l’ouvrir dans Android Designer. Voici le fichier de disposition de l’écran lorsqu’il est affiché dans Android Designer :

[![Ouverture du fichier Main.axml](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png)

Sélectionnez le **bouton** **Hello World, Click Me!** dans l’aire de conception, puis supprimez-le en appuyant sur la touche **Suppr**. 

Dans la **boîte à outils** (la zone située à droite), entrez `text` dans le champ de recherche, puis faites glisser un widget **Text (Large)** dans l’aire de conception (la zone située au centre) :

[![Ajout du widget Text (Large)](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png)

Après avoir sélectionné le widget **Text (Large)** dans l’aire de conception, accédez au panneau **Propriétés** pour remplacer la propriété `Text` du widget **Text (Large)** par la propriété `Enter a Phoneword:`, comme indiqué ci-dessous :

[![Définition des propriétés du widget Text (Large)](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png)

Ensuite, faites glisser un widget **Plain Text** de la **boîte à outils** vers l’aire de conception, puis placez-le sous le widget **Text (Large)**. Notez que vous pouvez utiliser le champ de recherche pour localiser les widgets plus facilement à l’aide de leur nom :

[![Ajout du widget Plain Text](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png)

Après avoir sélectionné le widget **Plain Text** dans l’aire de conception, accédez au panneau **Propriétés** pour remplacer la propriété `Id` du widget **Plain Text** par la propriété `@+id/PhoneNumberText`, et la propriété `Text` par la propriété `1-855-XAMARIN` :

[![Définition des propriétés du widget Plain Text](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png)

Faites glisser un **bouton** de la **boîte à outils** vers l’aire de conception, puis placez-le sous le widget **Plain Text** :

[![Ajout d’un bouton](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png)

Après avoir sélectionné le widget **Button** dans l’aire de conception, accédez au panneau **Propriétés** pour remplacer la propriété `Id` du widget **Button** par la propriété `@+id/TranslateButton`, et la propriété `Text` par la propriété `Translate` :

[![Configuration du bouton Translate](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png)

Faites glisser le widget **TextView** de la **boîte à outils** vers l’aire de conception, puis placez-le sous le widget **Button**. Après avoir sélectionné **TextView**, définissez la propriété `id` du widget **TextView** sur `@+id/TranslatedPhoneWord`, et remplacez la valeur `text` par une chaîne vide :

[![Définition des propriétés du widget TextView](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png)    

Enregistrez votre travail en appuyant sur **&#8984; + S**.

### <a name="writing-translation-code"></a>Écriture de code de traduction

À présent, ajoutez du code pour convertir des numéros de téléphone alphanumériques en numéros de téléphone numériques. Pour ajouter un fichier au projet, cliquez sur l’icône représentant une roue dentée en regard du projet **Phoneword** dans le **Panneau Solutions**, puis sélectionnez **Ajouter > Nouveau fichier...** :

[![Ajouter un nouveau fichier au projet](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png)

Dans la boîte de dialogue **Nouveau fichier**, sélectionnez **Général > Classe vide**, nommez le nouveau fichier **PhoneTranslator**, puis cliquez sur **Nouveau**. Cette opération crée une nouvelle classe C# vide.

Supprimez tout le code de modèle qui se trouve dans la nouvelle classe, puis remplacez-le par le code suivant :

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                }
                // otherwise we've skipped a non-numeric char
            }
            return newNumber.ToString();
        }
        static bool Contains (this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }
        static int? TranslateToNumber(char c)
        {
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Enregistrez les modifications apportées au fichier **PhoneTranslator.cs** en sélectionnant **Fichier > Enregistrer** (ou en appuyant sur **&#8984; + S**), puis fermez le fichier. Vérifiez qu’aucune erreur de compilation ne s’est produite lorsque vous avez regénéré la solution.

### <a name="wiring-up-the-interface"></a>Câblage de l’interface

L’étape suivante consiste à ajouter du code pour structurer l’interface utilisateur en ajoutant du code de stockage dans la classe `MainActivity`.
Dans le **Panneau Solutions**, double-cliquez sur **MainActivity.cs** pour l’ouvrir.

Commencez par ajouter un gestionnaire d’événements au bouton **Translate**. Dans la classe `MainActivity`, recherchez la méthode `OnCreate`. Ajoutez le code du bouton à l’intérieur de `OnCreate`, sous les appels `base.OnCreate(bundle)` et `SetContentView (Resource.Layout.Main)`. Supprimez le code de gestion du bouton du modèle pour que la méthode `OnCreate` ressemble à ceci :

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Our code will go here
        }
    }
}
```

Ensuite, vous avez besoin d’une référence aux contrôles qui ont été créés dans le fichier de disposition via Android Designer. Ajoutez le code suivant dans la méthode `OnCreate`, immédiatement après l’appel à `SetContentView` :

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Ajoutez le code de réponse à l’actionnement du bouton **Translate** en insérant le code suivant dans la méthode `OnCreate` (après les lignes ajoutées à l’étape précédente) :

```csharp
// Add code to translate number
string translatedNumber = string.Empty;

translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Enregistrez votre travail, puis générez l’application en sélectionnant **Générer > Générer tout** (ou en appuyant sur **&#8984; + B**). Si l’application est compilée, vous recevez un message de confirmation en haut de la fenêtre Visual Studio pour Mac :

En cas d’erreurs, examinez les étapes précédentes et corrigez les erreurs éventuelles jusqu’à ce que l’application soit générée. S’il s’agit d’une erreur de génération, telle que _(Ressource) n’existe pas dans le contexte actuel_, vérifiez que le nom de l’espace de noms dans **MainActivity.cs** correspond bien au nom du projet (`Phoneword`), puis regénérez entièrement la solution. Si vous obtenez encore des erreurs de génération, vérifiez que vous avez installé les dernières mises à jour Xamarin.Android et Visual Studio pour Mac.

### <a name="setting-the-label-and-app-icon"></a>Définition du libellé et de l’icône de l’application

Votre application doit maintenant être opérationnelle : il est temps de lui ajouter la touche finale ! Commencez par modifier le `Label` pour `MainActivity`.
Le `Label` est ce qu’Android affiche en haut de l’écran pour permettre aux utilisateurs de savoir où ils se trouvent dans l’application. En haut de la classe `MainActivity`, remplacez `Label` par `Phone Word`, comme indiqué ici :

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

À présent, nous allons définir l’icône de l’application. Par défaut, Visual Studio pour Mac fournit une icône pour le projet. Nous allons supprimer ces fichiers de la solution et les remplacer par une autre icône. Développez le dossier **Ressources** dans le **Panneau Solutions**. Notez qu’il y a cinq dossiers dont le nom est précédé de **mipmap-**, et que chacun de ces dossiers contient un fichier **Icon.png** :

[![Dossiers mipmap- et fichiers Icon.png](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png)

Vous devez supprimer tous les fichiers d’icône du projet. Cliquez avec le bouton droit sur chaque fichier **Icon.png**, puis sélectionnez **Supprimer** dans le menu contextuel :

[![Suppression des fichiers Icon.png par défaut](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png)

Dans la boîte de dialogue, cliquez sur le bouton **Supprimer**.

Ensuite, téléchargez et décompressez les [icônes de l’application Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Ce fichier .zip contient les icônes de l’application. Toutes les icônes sont visuellement identiques. Toutefois, chacune a une résolution différente, adaptée à la densité d’écran des différents appareils sur lesquels l’application peut être utilisée.  Ces fichiers d’icône doivent être copiés dans le projet Xamarin.Android. Dans Visual Studio pour Mac, dans le **Panneau Solutions**, cliquez avec le bouton droit sur le dossier **mipmap-hdpi**, puis sélectionnez **Ajouter > Ajouter des fichiers** :

[![Ajout de fichiers](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png)

Dans la boîte de dialogue de sélection, accédez au dossier non décompressé qui contient les icônes d’application Xamarin, puis ouvrez le dossier **mipmap-hdpi**. Sélectionnez **Icon.png**, puis cliquez sur **Ouvrir**.

Dans la boîte de dialogue **Ajouter un fichier à un dossier**, sélectionnez **Copier le fichier dans le répertoire**, puis cliquez sur **OK** :

[![Boîte de dialogue Copier le fichier dans le répertoire](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png)

Répétez ces étapes pour chaque dossier **mipmap-**, jusqu’à ce que le contenu des dossiers d’icônes d’application Xamarin **mipmap-** soit copié vers les dossiers **mipmap-** correspondants dans le projet **Phoneword**.

Une fois que toutes les icônes sont copiées dans le projet Xamarin.Android, ouvrez la boîte de dialogue **Options du projet** en cliquant avec le bouton droit sur le projet dans le **Panneau Solutions**. Sélectionnez **Générer > Application Android**, puis sélectionnez **@mipmap/icon** à partir de la zone de liste déroulante **Icône de l’application** :

[![Définition de l’icône du projet](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png)

### <a name="running-the-app"></a>Exécution de l’application

Enfin, testez l’application en l’exécutant sur un émulateur ou un appareil Android, et en convertissant un numéro de téléphone alphanumérique :

[![Capture d’écran de l’application une fois terminée](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png)

-----

Félicitations, vous avez terminé votre première application Xamarin.Android !
Nous allons maintenant étudier en détail les outils et les compétences que nous venons d’utiliser. À suivre : [Hello, Android - En profondeur](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>Liens associés

- [Icônes de l’application Xamarin Android (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (exemple)](https://developer.xamarin.com/samples/monodroid/Phoneword)
