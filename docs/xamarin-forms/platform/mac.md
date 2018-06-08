---
title: Programme d’installation de Mac plateforme
description: Xamarin.Forms prend désormais en charge d’aperçu pour la plateforme Mac
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: c3a2c36463b2934254c54f3f2250ee253d57798b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848222"
---
# <a name="mac-platform-setup"></a>Programme d’installation de Mac plateforme

![Preview](~/media/shared/preview.png)

Avant de commencer, créez (ou utiliser un existant) Xamarin.Forms projet.
Vous pouvez ajouter uniquement les applications Mac à l’aide de Visual Studio pour Mac.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Ajout d’un projet macOS de Xamarin.Forms, par [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Ajout d’une application Mac

Suivez ces instructions pour ajouter une application Mac qui s’exécutera sur macOS Sierra et Mac OS X El Capitan :

1. Dans Visual Studio pour Mac, avec le bouton droit sur la solution Xamarin.Forms et choisissez **Ajouter > Ajouter un nouveau projet...**

2. Dans le **nouveau projet** fenêtre choisissez **Mac > application >/Cocoa application** et appuyez sur **suivant**.

3. Type une **nom de l’application** (et éventuellement choisir un autre nom pour l’élément d’ancrage), puis appuyez sur **suivant**.

4. Passez en revue la configuration et appuyez sur **créer**. Ces étapes sont illustrées dans ci-dessous :

  ![Instructions animées montrant comment ajouter une application/Cocoa](mac-images/add-macos-proj.gif)

5. Dans le projet Mac, cliquez sur **Packages > ajouter des Packages en cours...**  pour ajouter le [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Vous devez également mettre à jour les autres projets vers cette version.

6. Dans le projet Mac, cliquez sur **références** et ajoutez une référence au projet Xamarin.Forms (projet de bibliothèque de projet partagé ou .NET Standard).

  ![Ajoutez une référence au projet de code partagé Xamarin.Forms](mac-images/references-sml.png)

7. Mise à jour **Main.cs** pour initialiser le `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Mise à jour `AppDelegate` pour initialiser Xamarin.Forms, créer une fenêtre et charger l’application Xamarin.Forms (n’oubliez pas de définir un `Title`). _Si vous avez d’autres dépendances qui doivent être initialisés, le faire ici également._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Double-cliquez sur **Main.storyboard** à modifier dans Xcode. Sélectionnez le **fenêtre** et _Décochez_ le **est un contrôleur initiale** case à cocher (c’est parce que le code ci-dessus crée une fenêtre) :

  [![Désactivez la case à cocher est un contrôleur Initial dans Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  Vous pouvez modifier le système de menus dans la table de montage séquentiel pour supprimer les éléments indésirables.

10. Enfin, ajoutez toutes les ressources locales (par exemple). fichiers image) à partir de projets plateforme existants qui sont requis.

11. Le projet Mac doit-elle désormais exécuter votre code Xamarin.Forms sur macOS !

## <a name="next-steps"></a>Étapes suivantes

### <a name="styling"></a>Styles

Avec les dernières modifications apportées à `OnPlatform` vous pouvez maintenant cibler n’importe quel nombre de plateformes. Qui inclut macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Notez vous pouvez également deux sur les plateformes comme suit : `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Position et la taille de fenêtre

Vous pouvez ajuster la taille initiale et l’emplacement de la fenêtre de la `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Problèmes connus

Il s’agit d’une version préliminaire, vous devez donc vous attendre pas permet à tous les éléments est prêts pour la production. Voici quelques éléments que vous pouvez rencontrer lorsque vous ajoutez macOS à vos projets :

### <a name="not-all-nugets-are-ready-for-macos"></a>NuGets pas tous prêts à être macOS

Les packages doivent cibler « xamarinmac20 » pour travailler dans un projet macOS. Vous découvrirez peut-être que certaines des bibliothèques que vous utilisez ne gèrent pas encore macOS.

Dans ce cas, vous devez envoyer une demande au chargé de maintenance du projet pour l’ajouter. Jusqu'à ce qu’ils ont prise en charge, vous devrez peut-être chercher d’autres solutions.

### <a name="missing-xamarinforms-features"></a>Fonctionnalités Xamarin.Forms manquantes

Pas toutes les fonctionnalités de Xamarin.Forms sont terminées dans cette version préliminaire ; Voici une liste de certaines des fonctionnalités qui ne sont pas encore implémentée :

* Pied de page
* Image – h
* UnevenRows de ListView – ScrollTo, prendre en charge, l’actualisation, SeparatorColor, SeparatorVisibility
* MasterDetailPage – BackgroundColor
* Navigation – InsertPageBefore
* OpenGLRenderer
* Sélecteur – mise en œuvre pouvant être lié/Observable
* TabbedPage – BarBackgroundColor, BarTextColor
* TableView – UnevenRows
* ViewCell – IsEnabled, ForceUpdateSize
* WebView – la plupart des WebNavigationEvents


## <a name="related-links"></a>Liens associés

- [Xamarin.Mac](~/mac/index.yml)
