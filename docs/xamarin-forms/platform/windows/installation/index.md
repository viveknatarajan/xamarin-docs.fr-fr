---
title: Projets d’installation Windows
description: Les solutions Xamarin.Forms plus anciennes (ou celles créées sur macOS) n’aura pas les projets de plateforme Windows universelle, et par conséquent, cet article explique comment ajouter un nouveau projet UWP à une solution Xamarin.Forms existante.
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: b0f06cf15d3a3ec7eae4742d5d037e233be46d08
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855183"
---
# <a name="setup-windows-projects"></a>Projets d’installation Windows

_Ajout de nouveaux projets de Windows à une solution Xamarin.Forms existante_

Les solutions Xamarin.Forms plus anciennes (ou celles créées sur macOS) n’aura pas les projets d’application Universal Windows Platform (UWP). Par conséquent, vous devez ajouter manuellement un projet UWP pour générer une application Windows 10 (UWP).

## <a name="add-a-universal-windows-platform-app"></a>Ajoutez un Windows Universal application de plateforme

**Visual Studio 2019** sur **Windows 10** est recommandé de créer des applications UWP. Pour plus d’informations sur la plateforme Windows universelle, consultez [Introduction à la plateforme Windows universelle](/windows/uwp/get-started/universal-application-platform-guide/).

UWP est disponible dans Xamarin.Forms 2.1 et versions ultérieures, et Xamarin.Forms.Maps est pris en charge dans Xamarin.Forms 2.2 et versions ultérieures.

Vérifier le <a href="#troubleshooting">dépannage</a> section pour obtenir des conseils utiles.

Suivez ces instructions pour ajouter une application UWP qui s’exécutera sur les téléphones, tablettes et ordinateurs de bureau Windows 10 :

 1 . Avec le bouton droit sur la solution et sélectionnez **Ajouter > Nouveau projet...**  et ajoutez un **application vide (Windows universel)** projet :

  ![](universal-images/add-wu.png "Ajouter la boîte de dialogue Nouveau projet")

 2 . Dans le **nouveau projet de plateforme Windows universelle** boîte de dialogue, sélectionnez les versions minimale et cible de Windows 10 et que l’application s’exécutera sur :

  ![](universal-images/target-version.png "Nouvelle boîte de dialogue projet de plateforme Windows universelle")

 3 . Avec le bouton droit sur le projet UWP et sélectionnez **gérer les Packages NuGet...**  et ajoutez le **Xamarin.Forms** package. Assurez-vous que les autres projets dans la solution sont également mises à jour vers la même version du package Xamarin.Forms.

 4 . Assurez-vous que le nouveau projet UWP sera généré dans le **Générer > Gestionnaire de Configuration** fenêtre (cela probablement ne se sont produites par défaut). Graduation la **Build** et **déployer** boîtes pour le projet universel :

  [![](universal-images/configuration-sml.png "Fenêtre du Gestionnaire de configuration")](universal-images/configuration.png#lightbox "fenêtre du Gestionnaire de Configuration")

 5 . Avec le bouton droit sur le projet, puis sélectionnez **Ajouter > référence** et créer une référence au projet d’application Xamarin.Forms (.NET Standard ou projet partagé).

  ![](universal-images/addref-sml.png "Boîte de dialogue Gestionnaire de références")

 6 . Dans le projet UWP, modifiez **App.xaml.cs** pour inclure le `Init` appel de méthode à l’intérieur de la `OnLaunched` méthode autour de la ligne 52 :

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . Dans le projet UWP, modifiez **MainPage.xaml** en supprimant le `Grid` contenus dans le `Page` élément.

 8 . Dans **MainPage.xaml**, ajoutez un nouveau `xmlns` entrée pour `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . Dans **MainPage.xaml**, modifier la racine `<Page` élément à `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . Dans le projet UWP, modifiez **MainPage.xaml.cs** pour supprimer la `: Page` spécificateur d’héritage pour le nom de classe (dans la mesure où elle héritera de `WindowsPage` en raison de la modification apportée à l’étape précédente) :

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . Dans **MainPage.xaml.cs**, ajoutez le `LoadApplication` appeler dans le `MainPage` constructeur pour démarrer l’application Xamarin.Forms :

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

12 . Ajouter des ressources locales (par exemple). fichiers image) à partir des projets de plateforme existants qui sont nécessaires.

## <a name="troubleshooting"></a>Résolution des problèmes

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>« Exception d’appel de cible » lors de l’utilisation de « Compilation avec la chaîne d’outils Native .NET »

Si votre application UWP fait référence à plusieurs assemblys (par exemple les bibliothèques de contrôles de tiers, ou votre application elle-même est divisée en plusieurs bibliothèques), Xamarin.Forms peut être impossible de charger des objets à partir de ces assemblys (tels que les convertisseurs personnalisés).

Cela peut se produire lorsque vous utilisez le **compiler avec la chaîne d’outils .NET Native** qui est une option pour les applications UWP dans le **Propriétés > Générer > Général** fenêtre pour le projet.

Vous pouvez résoudre ce problème à l’aide d’une surcharge spécifique à UWP de la `Forms.Init` appeler dans **App.xaml.cs** comme indiqué dans le code ci-dessous (vous devez remplacer `ClassInOtherAssembly` avec une classe réelle fait référence à votre code) :

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Ajoutez une entrée pour chaque assembly que vous avez ajoutés en tant que référence dans l’Explorateur de solutions, soit via une référence directe ou d’un package NuGet.

#### <a name="dependency-services-and-net-native-compilation"></a>Compilation Native des Services de dépendance et .NET

Les versions Release à l’aide de compilation .NET Native peuvent échouer résoudre des services de dépendance qui sont définis en dehors de l’exécutable d’application principale (par exemple, comme dans un projet distinct ou une bibliothèque).

Utilisez le `DependencyService.Register<T>()` méthode pour inscrire manuellement les classes de service de dépendance. En fonction de l’exemple ci-dessus, ajoutez la méthode register comme ceci :

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
