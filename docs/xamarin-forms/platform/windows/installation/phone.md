---
title: Ajout d’une application du Windows Phone
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 55bd4bdcfde4c91ad5c9b94bef486207466e135d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="adding-a-windows-phone-app"></a>Ajout d’une application du Windows Phone


Tout d’abord, si vous avez utilisé le modèle de bibliothèque de classes portables Xamarin.Forms [mettre à jour le profil](~/xamarin-forms/platform/windows/installation/index.md), puis suivez les instructions ci-dessous :

1. **avec le bouton droit sur la solution > Ajouter > Nouveau projet...**  et ajoutez un **application vide (Windows Phone)**

  ![](phone-images/add-wp81.png "Ajouter la boîte de dialogue Nouveau projet")

2. **avec le bouton droit sur le projet nouvellement créé > Gérer les Packages NuGet...**  et ajoutez le **Xamarin.Forms** package.

3. **avec le bouton droit sur le projet > Ajouter > référence** et créer une référence de projet au projet d’application partagée Xamarin.Forms.

  ![](phone-images/addref.png "Boîte de dialogue Gestionnaire de références")

4. Modifier **App.xaml.cs** à inclure le `Init()` appel de méthode, dans le `OnLaunched` méthode autour de la ligne 67 :

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Modifier **MainPage.xaml** -modifier l’élément racine `<Page` à `<forms:WindowsPhonePage` *et* définir le `xmlns:forms` qu’il utilise :

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . Modifier **MainPage.xaml.cs** pour supprimer la `: PhonePage` spécificateur d’héritage pour le nom de classe.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . Toujours dans **MainPage.xaml.cs**, ajoutez le `LoadApplication` appeler dans le `MainPage` constructeur (autour de la ligne 28) pour démarrer votre application Xamarin.Forms :

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8. Double-cliquez sur **Package.appxmanifest** pour définir ces fonctionnalités sont souvent requises :

  * Internet (client & serveur)

9 . Enfin, ajoutez toutes les ressources locales (par exemple). fichiers image) à partir de projets plateforme existants qui sont requis.

