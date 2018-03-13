---
title: "Mise à jour des applications existantes de Xamarin.Forms"
description: "Procédez comme suit pour mettre à jour une application existante de Xamarin.Forms pour utiliser l’API unifiée et mettre à jour vers la version 1.3.1"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 48b8d1cf8e6242fde632bceec5d482f53037a954
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Mise à jour des applications existantes de Xamarin.Forms

_Procédez comme suit pour mettre à jour une application existante de Xamarin.Forms pour utiliser l’API unifiée et mettre à jour vers la version 1.3.1_

> [!IMPORTANT]
> Xamarin.Forms 1.3.1 étant la première version qui prend en charge l’API unifiée, l’ensemble de la solution doit être à jour pour utiliser la version la plus récente en même temps que la migration de l’application iOS à unifié. Cela signifie qu’en plus de la mise à jour le projet iOS pour la prise en charge unifié, vous devez également modifier le code dans _tous les_ les projets dans la solution.

La mise à jour est effectuée en deux étapes :

1. Migrer l’application iOS à l’API unifiée à l’aide de Visual Studio pour la build Mac dans l’outil de migration.

    - Utilisez l’outil de migration pour mettre à jour automatiquement le projet.

    - Mise à jour iOS natif API comme indiqué dans les instructions pour [mettre à jour des applications iOS](~/cross-platform/macios/unified/updating-ios-apps.md) (en particulier dans le Générateur de code personnalisé ou dépendance service).

2. Mettre à jour l’ensemble de la solution de Xamarin.Forms version 1.3.

    1. Installer le package NuGet Xamarin.Forms 1.3.1.

    2. Mise à jour la `App` classe dans le code partagé.

    3. Mise à jour le `AppDelegate` dans le projet iOS.

    4. Mise à jour le `MainActivity` dans le projet Android.

    5. Mise à jour le `MainPage` dans le projet Windows Phone.

## <a name="1-ios-app-unified-migration"></a>1. application (Unified Migration) iOS

Dans le cadre de la migration nécessite la mise à niveau de Xamarin.Forms pour la version 1.3, qui prend en charge l’API unifiée. Dans l’ordre pour les références d’assembly approprié doit être créé, nous devons d’abord mettre à jour le projet iOS pour utiliser l’API unifiée.

### <a name="migration-tool"></a>Outil de migration

Cliquez sur le projet iOS afin qu’il est sélectionné, puis choisissez **projet > migrer vers une API unifiée de Xamarin.iOS...**  et acceptez le message d’avertissement s’affiche.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Choisissez projet > migrer à l’API unifiée de Xamarin.iOS... et acceptez le message d’avertissement qui s’affiche")

Ceci va automatiquement :

 - Modifier le type de projet pour prendre en charge l’API unifiée de 64 bits.
 - Modifiez la référence de framework à **Xamarin.iOS** (en remplaçant l’ancien **monotouch** référence).
 - Modifiez les références d’espace de noms dans le code pour supprimer le `MonoTouch` préfixe.
 - Mise à jour la **csproj** fichier à utiliser les cibles de génération correct pour l’API unifiée.

**Nettoyer** et **Build** le projet pour vous assurer qu’aucun Corrigez les autres erreurs. Aucune action supplémentaire ne doit être requise. Ces étapes sont expliquées plus en détail dans les [docs de l’API unifiée](~/cross-platform/macios/unified/updating-ios-apps.md).

### <a name="update-native-ios-apis-if-required"></a>Mettre à jour iOS native API (si nécessaire)

Si vous avez ajouté le code natif (par exemple, les convertisseurs personnalisés ou les services de dépendance) iOS supplémentaires, vous devrez peut-être effectuer des correctifs supplémentaires manuelle du code. Recompilez votre application et de faire référence à la [mise à jour existant iOS instructions d’applications](~/cross-platform/macios/unified/updating-ios-apps.md) pour plus d’informations sur les modifications peuvent être nécessaires. [Ces conseils](~/cross-platform/macios/unified/updating-tips.md) sera également aider à identifier les modifications requises.

## <a name="2-xamarinforms-131-update"></a>2. Mise à jour Xamarin.Forms 1.3.1

Une fois que l’application iOS a été mis à jour à l’API unifiée, le reste de la solution doit être mis à jour vers la version 1.3.1 de Xamarin.Forms. Cela concerne :

 - Mise à jour du package Xamarin.Forms NuGet dans chaque projet.
 - La modification du code pour utiliser le nouveau Xamarin.Forms `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android), et `FormsApplicationPage` les classes (Windows Phone).

Ces étapes sont expliquées ci-dessous :

### <a name="21-update-nuget-in-all-projects"></a>2.1 NuGet de mise à jour dans tous les projets

Mettre à jour de Xamarin.Forms pour 1.3.1 version préliminaire à l’aide du Gestionnaire de Package NuGet pour tous les projets dans la solution : bibliothèque PCL (le cas échéant), iOS, Android et Windows Phone. Il est recommandé que vous avez **supprimer et rajouter** le package Xamarin.Forms NuGet pour mettre à jour vers la version 1.3.

**Remarque :** Xamarin.Forms version 1.3.1 est en cours *préliminaires*. Cela signifie que vous devez sélectionner le **préliminaires** option NuGet via (une case à cocher-dans Visual Studio pour Mac) ou une-liste déroulante dans Visual Studio pour voir la dernière version préliminaire.

> [!IMPORTANT]
> Si vous utilisez Visual Studio, assurez-vous que la dernière version du Gestionnaire de Package NuGet est installée. Les versions antérieures de NuGet dans Visual Studio installera pas correctement la version unifiée de Xamarin.Forms 1.3.1. Accédez à **Outils > Extensions et mises à jour...**  , puis cliquez sur le **installé** liste pour vérifier que le **le Gestionnaire de Package NuGet pour Visual Studio** est au moins version 2.8.5. Si elle est antérieure, cliquez sur le **mises à jour** liste afin de télécharger la dernière version.

Une fois que vous avez mis à jour le package NuGet de Xamarin.Forms 1.3.1, apportez les modifications suivantes dans chaque projet à mettre à niveau vers la nouvelle `Xamarin.Forms.Application` classe.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 bibliothèque de classes portable (ou un projet partagé)

Modifier la **App.cs** fichier afin que :

 - Le `App` classe hérite désormais `Application`.
 - Le `MainPage` est définie sur la première page de contenu que vous souhaitez afficher.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Nous avons supprimé complètement le `GetMainPage` (méthode) et définir à la place la `MainPage` *propriété* sur la `Application` sous-classe.

Cette nouvelle `Application` classe de base prend également en charge la `OnStart`, `OnSleep`, et `OnResume` des remplacements afin de vous aider à gérer le cycle de vie de votre application.

Le `App` classe est ensuite transmise à un nouveau `LoadApplication` méthode dans chaque projet d’application, comme décrit ci-dessous :

### <a name="23-ios-app"></a>Applications 2.3 iOS

Modifier la **AppDelegate.cs** fichier afin que :

 - La classe hérite de `FormsApplicationDelegate` (au lieu de `UIApplicationDelegate` précédemment).
 - `LoadApplication` est appelé avec une nouvelle instance de `App`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="23-android-app"></a>2.3 application android

Modifier la **MainActivity.cs** fichier afin que :

 - La classe hérite de `FormsApplicationActivity` (au lieu de `FormsActivity` précédemment).
 - `LoadApplication` est appelé avec une nouvelle instance de `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="24-windows-phone-app"></a>2.4 Windows Phone application

Nous devons mettre à jour le **MainPage** -le code XAML et le code-behind.

Modifier la **MainPage.xaml** fichier afin que :

 - L’élément racine XAML doit être `winPhone:FormsApplicationPage`.
 - Le `xmlns:phone` doit être *modifié* à `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Voici un exemple de mise à jour - vous ne devez modifier ces éléments (le reste des attributs doit rester le même) :

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Modifier la **MainPage.xaml.cs** fichier afin que :

 - La classe hérite de `FormsApplicationPage` (au lieu de `PhoneApplicationPage` précédemment).
 - `LoadApplication` est appelé avec une nouvelle instance de la Xamarin.Forms `App` classe. Vous devrez peut-être qualifier entièrement cette référence, car Windows Phone a son propre `App` classe déjà défini.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>Résolution des problèmes

Parfois, vous verrez une erreur similaire à celui-ci après la mise à jour le package Xamarin.Forms NuGet. Il se produit lors de la mise à jour de NuGet ne supprime pas complètement les références aux anciennes versions de votre **csproj** fichiers.

>VOTRE\_PROJECT.csproj : erreur : ce projet fait référence à des packages NuGet qui sont manquants sur cet ordinateur. Activer la restauration des packages NuGet pour les télécharger.  Pour plus d’informations, consultez http://go.microsoft.com/fwlink/?LinkID=322105. Le fichier manquant est... /.. /packages/Xamarin.Forms.1.2.3.6257/build/portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (VOTRE\_PROJET)

Pour résoudre ces erreurs, ouvrez le **csproj** dans un éditeur de texte et recherchez `<Target` les éléments qui font référence à des versions antérieures de Xamarin.Forms, par exemple l’élément indiqué ci-dessous. Vous devez supprimer manuellement cet élément tout entier à partir de la **csproj** de fichier et enregistrez les modifications.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Le projet doit se générer avec succès une fois ces références anciennes sont supprimées.

## <a name="considerations"></a>Éléments à prendre en considération

Les considérations suivantes doivent être prises en compte lors de la conversion d’un projet existant de Xamarin.Forms à partir de l’API classique à la nouvelle API unifiée si cette application s’appuie sur un ou plusieurs composants ou le NuGet Package.

### <a name="components"></a>Composants

Tout composant que vous avez inclus dans votre application également devront être mis à jour à l’API unifiée ou que vous obtenez un conflit lorsque vous essayez de compiler. Pour les composants inclus, remplacez la version actuelle par une nouvelle version à partir du magasin de composant Xamarin qui prend en charge l’API unifiée et effectuer un nettoyage de build. Tout composant qui n’a pas encore été converti par l’auteur, affiche un 32 bits d’avertissement uniquement dans le magasin de composants.

### <a name="nuget-support"></a>Prise en charge par NuGet

Pendant que nous avons contribué modifications NuGet pour travailler avec la prise en charge de l’API unifiée, il n’a pas été une nouvelle version de NuGet, donc nous examinons l’évaluation de l’obtention de NuGet pour reconnaître les nouvelles API.

En attendant, tout comme les composants, vous devez passer n’importe quel NuGet Package que vous avez inclus dans votre projet vers une version qui prend en charge les API unifiée et effectuer un nettoyage de build par la suite.

> [!IMPORTANT]
> **Remarque :** si vous avez une erreur dans le formulaire _« erreur 3 ne peut pas inclure 'monotouch.dll' et 'Xamarin.iOS.dll' dans le même projet Xamarin.iOS - 'Xamarin.iOS.dll' est référencé explicitement, alors que 'monotouch.dll' est référencé par ' xxx, Version = 0.0.000, Culture = neutral, PublicKeyToken = null' «_ après la conversion de votre application à l’API unifiée, il est généralement parce qu’un composant ou un NuGet Package dans le projet n’a pas été mis à jour à l’API unifiée. Vous devez supprimer le composant existant/NuGet, mettre à jour vers une version qui prend en charge les API unifiée, puis effectuez un nettoyage de build.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>L’activation de 64 bits génère des applications de Xamarin.iOS

Pour une application mobile Xamarin.iOS qui a été convertie à l’API unifiée, le développeur doit toujours autorisent la création de l’application pour les ordinateurs 64 bits à partir des Options de l’application. Consultez le **activation 64 génère de Xamarin.iOS applications bits** de la [considérations relatives à la plate-forme 32/64 bits](~/cross-platform/macios/32-and-64/index.md#enable-64) génère des documents pour obtenir des instructions détaillées sur l’activation de 64 bits.

## <a name="summary"></a>Récapitulatif

L’application Xamarin.Forms doit être mis à jour vers la version 1.3.1 et l’application iOS migrés à l’API unifiée (qui prend en charge les architectures 64 bits sur la plateforme iOS).

Comme indiqué précédemment, si votre application Xamarin.Forms inclut le code natif telles que les convertisseurs personnalisés ou des services de dépendance, puis ils ont également besoin de mise à jour pour utiliser les nouveaux Types de [introduit dans l’API unifiée](~/cross-platform/macios/index.md).

## <a name="related-links"></a>Liens associés

- [Mise à jour des applications iOS](~/cross-platform/macios/unified/updating-apps.md)
- [Mise à jour des applications iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Utilisation de types natifs dans des applications multiplateformes](~/cross-platform/macios/native-types-cross-platform.md)
- [Conseils de mise à jour](~/cross-platform/macios/unified/updating-tips.md)
- [Différences d’API unifiée de vs classiques](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
