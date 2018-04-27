---
title: Ajout d’une application de plateforme universelle Windows universelle
description: Cet article explique comment ajouter un projet d’application UWP pour une solution Xamarin.Forms qui a été créée dans Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: 34AAA045-64B8-4FDE-BB49-3FF0B4FFA17C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: f851c1ca241be9e3c94a70b1f63135a46575d471
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/26/2018
---
# <a name="adding-a-universal-windows-platform-uwp-app"></a>Ajout d’une application de plateforme universelle Windows universelle

_Cet article explique comment ajouter un projet d’application UWP pour une solution Xamarin.Forms qui a été créée dans Visual Studio pour Mac._

Vous devez être en cours d’exécution **Visual Studio 2017** sur **Windows 10** pour générer des applications UWP. Pour plus d’informations sur la plateforme Windows universelle, consultez [Introduction à la plateforme Windows universelle](/windows/uwp/get-started/universal-application-platform-guide/).

UWP est disponible dans Xamarin.Forms 2.1 et versions ultérieures, et Xamarin.Forms.Maps est pris en charge dans Xamarin.Forms 2.2 et versions ultérieures.

Vérifiez le <a href="#troubleshooting">dépannage</a> section pour obtenir des conseils utiles.

Suivez ces instructions pour ajouter une application UWP qui s’exécutera sur les téléphones, tablettes et ordinateurs de bureau Windows 10 :

 1 . Avec le bouton droit sur la solution et sélectionnez **Ajouter > Nouveau projet...**  et ajoutez un **application vide (Windows universel)** projet :

  ![](universal-images/add-wu.png "Ajouter la boîte de dialogue Nouveau projet")

 2 . Dans le **nouveau projet de plateforme Windows universelle** boîte de dialogue, sélectionnez les versions minimale et la cible de Windows 10 et que l’application s’exécutera sur :

  ![](universal-images/target-version.png "Nouvelle boîte de dialogue projet de plateforme Windows universelle")

 3 . Avec le bouton droit sur le projet UWP, puis sélectionnez **gérer les Packages NuGet...**  et ajoutez le **Xamarin.Forms** package. Assurez-vous que les autres projets dans la solution sont également mis à jour vers la même version du package Xamarin.Forms.

 4 . Assurez-vous que le nouveau projet UWP sera généré le **Générer > Gestionnaire de Configuration** fenêtre (cela probablement ne se produire par défaut). Graduation la **générer** et **déployer** zones pour le projet d’application universelle :

  [![](universal-images/configuration-sml.png "Fenêtre Gestionnaire de configuration")](universal-images/configuration.png#lightbox "fenêtre du Gestionnaire de Configuration")

 5 . Avec le bouton droit sur le projet, puis sélectionnez **Ajouter > référence** et créer une référence au projet d’application Xamarin.Forms (PCL, .NET Standard ou projet partagé).

  ![](universal-images/addref-sml.png "Boîte de dialogue Gestionnaire de références")

 6 . Dans le projet UWP, modifiez **App.xaml.cs** à inclure le `Init` appel de méthode à l’intérieur de la `OnLaunched` méthode autour de la ligne 52 :

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . Dans le projet UWP, modifiez **MainPage.xaml** en supprimant la `Grid` contenus dans le `Page` élément.

 8. Dans **MainPage.xaml**, ajoutez un nouveau `xmlns` entrée pour `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . Dans **MainPage.xaml**, modifier la racine `<Page` élément `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10. Dans le projet UWP, modifiez **MainPage.xaml.cs** pour supprimer la `: Page` spécificateur d’héritage pour le nom de classe (dans la mesure où elle héritera de `WindowsPage` en raison de la modification apportée à l’étape précédente) :

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11. Dans **MainPage.xaml.cs**, ajoutez le `LoadApplication` appeler dans le `MainPage` constructeur pour démarrer l’application de Xamarin.Forms :

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12. Ajouter des ressources locales (par exemple). fichiers image) à partir de projets plateforme existants qui sont requis.

<a name="troubleshooting" />

## <a name="troubleshooting"></a>Résolution des problèmes

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>« Exception d’appel de cibler » lors de l’utilisation de « Compiler avec la chaîne d’outils Native .NET »

Si votre application UWP fait référence à plusieurs assemblys (par exemple les bibliothèques de contrôles de tiers, ou votre application elle-même est divisée en plusieurs bibliothèques), Xamarin.Forms peut être impossible de charger les objets à partir de ces assemblys (tels que les convertisseurs personnalisés).

Cela peut se produire lorsque vous utilisez la **compiler avec la chaîne d’outils .NET Native** qui est une option pour applications UWP dans la **Propriétés > Générer > Général** fenêtre pour le projet.

Vous pouvez résoudre ce problème à l’aide d’une surcharge UWP spécifiques de la `Forms.Init` appeler dans **App.xaml.cs** comme indiqué dans le code ci-dessous (vous devez remplacer `ClassInOtherAssembly` avec une classe réelle fait référence à votre code) :

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Ajoutez une référence à chaque assembly qui est référencé par l’application.

#### <a name="dependency-services-and-net-native-compilation"></a>Compilation en mode natif .NET et les Services de dépendance

Versions Release à l’aide de la compilation .NET Native peuvent échouer résoudre des services de dépendance qui sont définies en dehors de l’exécutable principal de l’application (par exemple, dans une bibliothèque ou un projet distinct).

Utilisez la `DependencyService.Register<T>()` méthode inscrire manuellement les classes de service de dépendance. En fonction de l’exemple ci-dessus, ajoutez la méthode register comme suit :

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
