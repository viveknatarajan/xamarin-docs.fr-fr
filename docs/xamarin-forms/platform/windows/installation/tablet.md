---
title: Ajout d’une application Windows
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0d2ef44896c9352776443c2fec318d40d27d7539
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="adding-a-windows-app"></a>Ajout d’une application Windows


1. **avec le bouton droit sur la solution > Ajouter > Nouveau projet...**  et ajoutez un **application vide (Windows)**

 ![](tablet-images/add-wu.png "Ajouter la boîte de dialogue Nouveau projet")

2. **avec le bouton droit sur le projet > Gérer les Packages NuGet...**  et ajoutez le **Xamarin.Forms** package.

3. **avec le bouton droit sur le projet > Ajouter > référence** et créer une référence de projet au projet d’application partagée Xamarin.Forms.

  ![](tablet-images/addref.png "Boîte de dialogue Gestionnaire de références")

4. Modifier **App.xaml.cs** à inclure le `Init()` appel de méthode à l’intérieur de la `OnLaunched` autour de la ligne 65 (méthode)

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Modifier **MainPage.xaml** -modifier l’élément racine `<Page` à `<forms:WindowsPage` *et* définir le `xmlns:forms` qu’il utilise :

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . Modifier **MainPage.xaml.cs** pour supprimer la `: Page` spécificateur d’héritage pour le nom de classe.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . Toujours dans **MainPage.xaml.cs**, ajoutez le `LoadApplication` appeler dans le `MainPage` constructeur (autour de la ligne 28) pour démarrer votre application Xamarin.Forms :

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8. Double-cliquez sur **Package.appxmanifest** pour définir ces fonctionnalités sont souvent requises :

  Ensemble de fonctionnalités :

  * Internet (client)
  * Emplacement

9 . Enfin, ajoutez toutes les ressources locales (par exemple). fichiers image) à partir de projets plateforme existants qui sont requis.

