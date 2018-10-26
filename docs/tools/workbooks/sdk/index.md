---
title: Mise en route avec les Kit de développement de Xamarin Workbooks
description: Ce document décrit la prise en main avec le SDK de classeurs de Xamarin, qui peut être utilisé pour développer des intégrations pour Xamarin Workbooks.
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 5800e98acbff147735ae4a6125979a4b47be2367
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115365"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Mise en route avec les Kit de développement de Xamarin Workbooks

Ce document fournit un guide rapide de mise en route avec le développement des intégrations pour Xamarin Workbooks. Tout cela fonctionnera avec les classeurs Xamarin stable, mais **chargement intégrations via les packages NuGet est uniquement pris en charge dans les classeurs 1.3**, dans le canal alpha au moment de l’écriture.

## <a name="general-overview"></a>Présentation générale

Intégrations de Xamarin Workbooks sont petites bibliothèques qui utilisent le [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK à intégrer à la Xamarin Workbooks et Inspector agents afin de fournir des expériences améliorées.

Il existe 3 étapes principales pour la mise en route avec le développement d’une intégration, nous les exposerons ici.

## <a name="creating-the-integration-project"></a>Création du projet d’intégration

Bibliothèques d’intégration sont mieux développées comme des bibliothèques multiplateformes. Étant donné que vous souhaitez fournir la meilleure intégration sur tous les agents disponibles, passés et futurs, vous voudrez choisir un ensemble largement pris en charge des bibliothèques. Nous vous recommandons d’utiliser le modèle « Bibliothèque Portable » pour la prise en charge un large :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Bibliothèque portable modèle Visual Studio pour Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Bibliothèque portable modèle Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Dans Visual Studio, vous voudrez veillez à que sélectionner les plateformes cibles suivants pour votre bibliothèque portable :

[![Bibliothèque portable plateformes Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Une fois que vous créez le projet de bibliothèque, ajoutez une référence à notre `Xamarin.Workbooks.Integration` bibliothèque NuGet via le Gestionnaire de Package NuGet.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![NuGet Visual Studio pour Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Vous souhaitez supprimer la classe vide qui est créée pour vous dans le cadre du projet, vous n’être qui en ont besoin pour cela. Une fois que vous avez effectué ces étapes, vous êtes prêt à commencer à générer votre intégration.

## <a name="building-an-integration"></a>Création d’une intégration

Nous allons créer une intégration simple. Nous l’adore la couleur verte, donc nous allons ajouter la couleur verte comme une représentation à chaque objet. Tout d’abord, créez une classe appelée `SampleIntegration`et le rend à implémenter notre [ `IAgentIntegration` ] [ integration-type] interface :

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Ce que nous voulons faire est d’ajouter un [représentation](~/tools/workbooks/sdk/representations.md) pour chaque objet qui est une couleur verte. Nous allons pour cela à l’aide d’un fournisseur de représentation. Fournisseurs héritent le [ `RepresentationProvider` ] [ reppr] classe — pour la nôtre, nous devons remplacer [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Nous renvoyons un [ `Color` ] [ color], intégrées dans le type de représentation dans notre kit de développement logiciel.
Vous remarquerez que le type de retour est un `IEnumerable<object>` &mdash;un fournisseur de représentation peut renvoyer plusieurs représentations sous forme d’un objet ! Tous les fournisseurs de représentation sont appelés pour chaque objet, il est donc important de ne pas faire d’hypothèses concernant les objets sont passés à vous.

L’étape finale consiste à réellement enregistrer notre fournisseur avec l’agent et indiquer les classeurs où trouver le type d’intégration. Pour inscrire le fournisseur, ajoutez ce code à la `IntegrateWith` méthode dans la `SampleIntegration` classe que vous avez créée précédemment :

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Définition du type d’intégration s’effectue via un attribut à l’échelle de l’assembly. Vous pouvez le placer dans votre AssemblyInfo.cs, ou dans la même classe que votre type d’intégration pour des raisons pratiques :

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Pendant le développement, vous pouvez trouver plus pratique à utiliser [ `AddProvider` surcharges] [ addprovider] sur `RepresentationManager` qui vous permettent d’inscrire un rappel simple pour fournir des représentations à l’intérieur d’un classeur, puis déplacez ce code dans votre `RepresentationProvider` implémentation une fois que vous avez terminé. Un exemple de rendu une [ `OxyPlot` ] [ oxyplot] `PlotModel` peut ressembler à ceci :

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Ces API vous donnent un moyen rapide d’être rapidement opérationnel, mais nous recommandons pas un ensemble de l’intégration de livraison uniquement à l’aide de leur&mdash;ils fournissent très peu de contrôle sur la façon dont vos types sont traités par le client.

Avec la représentation inscrite, votre intégration est prête à être expédié !

## <a name="shipping-your-integration"></a>L’intégration de livraison

Pour expédier votre intégration, vous devez l’ajouter à un package NuGet.
Vous pouvez l’expédier avec NuGet de votre bibliothèque existante, ou si vous créez un nouveau package, vous pouvez utiliser ce fichier .nuspec de modèle comme point de départ.
Vous devez remplir les sections pertinentes à votre intégration. La partie la plus importante est que tous les fichiers pour l’intégration doivent être dans un `xamarin.interactive` répertoire à la racine du package. Cela nous permet de trouver facilement tous les fichiers appropriés pour l’intégration, indépendamment de si vous utilisez un package existant ou créez un.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

Une fois que vous avez créé le fichier .nuspec, vous pouvez choisir votre NuGet comme suit :

```csharp
nuget pack MyIntegration.nuspec
```

puis publiez-le à [NuGet][nugetorg]. Une fois qu’il n’y figure, vous serez en mesure de le référencer à partir de n’importe quel classeur et voir en action. Dans la capture d’écran ci-dessous, nous avons empaqueté l’intégration de l’exemple que nous avons créé dans ce document et installé le package NuGet dans un classeur :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Classeur avec l’intégration](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Classeur avec l’intégration](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Notez que vous ne voyez pas les `#r` directives ou quoi que ce soit pour initialiser l’intégration, les classeurs a pris en charge de tout cela pour vous en arrière-plan !

## <a name="next-steps"></a>Étapes suivantes

Découvrez notre autre documentation pour plus d’informations sur les parties de déplacement qui composent le Kit de développement, et notre [exemple intégrations](~/tools/workbooks/samples/index.md) pour d’autres choses à faire à partir de votre intégration, comme la fourniture de code JavaScript personnalisé qui est exécuté dans le client de classeurs.

[integration-type]: https://developer.xamarin.com/api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[xir]: https://developer.xamarin.com/api/namespace/Xamarin.Interactive.Representations/
[reppr]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
