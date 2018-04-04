---
title: Mise en route avec les classeurs Xamarin SDK
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: bc8ae0304e5b044cc1a898820d0ac33e33dfec0d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Mise en route avec les classeurs Xamarin SDK

Ce document fournit un guide rapide pour la mise en route avec le développement d’intégration pour les classeurs Xamarin. Tout cela fonctionnera avec les classeurs Xamarin stable, mais **chargement intégrations via les packages NuGet est uniquement pris en charge dans les classeurs 1.3**, dans le canal alpha au moment de l’écriture.

# <a name="general-overview"></a>Vue d’ensemble générale

Les intégrations Xamarin classeurs sont petites bibliothèques qui utilisent la [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK pour intégrer les classeurs de Xamarin et d’un inspecteur agents pour fournir une expérience améliorée.

Il existe 3 étapes principales pour la mise en route avec le développement d’une intégration, nous allons montrer les ici.

## <a name="creating-the-integration-project"></a>Création du projet d’intégration

Bibliothèques d’intégration sont mieux développées comme des bibliothèques multiplateformes. Étant donné que vous souhaitez fournir la meilleure intégration sur tous les agents disponibles, passés et futurs, que vous souhaitez choisir un ensemble largement pris en charge des bibliothèques. Nous recommandons d’utiliser le modèle « Bibliothèque Portable » pour la prise en charge plus large :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Bibliothèque portable modèle Visual Studio pour Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bibliothèque portable modèle Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Dans Visual Studio, vous souhaiterez veillez à que sélectionner les plateformes cibles suivants de votre bibliothèque portable :

[![Bibliothèque portable plateformes Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Une fois que vous créez le projet de bibliothèque, ajouter une référence à notre `Xamarin.Workbooks.Integration` NuGet bibliothèque via le Gestionnaire de Package NuGet.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Studio NuGet Visual pour Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Que vous souhaitez supprimer la classe vide qui est créée pour vous dans le cadre du projet, vous ne sera pas être qui en ont besoin pour cela. Une fois que vous avez effectué ces étapes, vous êtes prêt à commencer à générer votre intégration.

## <a name="building-an-integration"></a>Création d’une intégration

Nous allons créer une intégration simple. Nous aime la couleur verte, donc nous allons ajouter la couleur verte en tant que représentation à chaque objet. Tout d’abord, créez une classe appelée `SampleIntegration`et le rendre implémenter notre [ `IAgentIntegration` ] [ integration-type] interface :

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Le résultat escompté est d’ajouter un [représentation](~/tools/workbooks/sdk/representations.md) pour chaque objet qui est la couleur vert. Nous verrons comment procéder à l’aide d’un fournisseur de représentation. Fournisseurs hériter la [ `RepresentationProvider` ] [ reppr] classe — pour nôtres, il suffit donc de remplacer [ `ProvideRepresentations` ] [ prrep]:

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

Nous renvoyons un [ `Color` ] [ color], intégrées dans le type de représentation de notre kit de développement logiciel.
Vous remarquerez que le type de retour est un `IEnumerable<object>` &mdash;un fournisseur de représentation peut retourner plusieurs représentations sous forme d’un objet ! Tous les fournisseurs de représentation sont appelés pour chaque objet, il est donc important de ne pas effectuer d’hypothèses concernant les objets qui sont passés à vous.

L’étape finale consiste à réellement inscrire notre fournisseur avec l’agent et indiquer où trouver le type d’intégration à des classeurs. Pour inscrire le fournisseur, ajoutez ce code à la `IntegrateWith` méthode dans la `SampleIntegration` classe créée précédemment :

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

La définition du type d’intégration est effectuée via un attribut de niveau de l’assembly. Vous pouvez le placer dans votre AssemblyInfo.cs, ou dans la même classe que votre type d’intégration pour des raisons pratiques :

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Pendant le développement, il peut s’avérer plus pratique d’utiliser [ `AddProvider` surcharges] [ addprovider] sur `RepresentationManager` qui permettent d’inscrire un rappel simple pour fournir des représentations sous forme d’à l’intérieur d’un classeur, puis déplacez ce code dans votre `RepresentationProvider` implémentation une fois que vous avez terminé. Un exemple de rendu une [ `OxyPlot` ] [ oxyplot] `PlotModel` peut ressembler à ceci :

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Ces API vous offrent un moyen rapide d’obtenir en cours d’exécution, mais nous vous recommandons de ne serait pas un ensemble de l’intégration d’expédition uniquement à l’aide de leur&mdash;ils fournissent très peu de contrôle sur la façon dont vos types sont traités par le client.

Avec la représentation sous forme d’inscrit votre intégration est prête pour la livraison !

## <a name="shipping-your-integration"></a>L’intégration de livraison

Pour expédier votre intégration, vous devez ajouter à un package NuGet.
Peuvent être livrées avec NuGet de votre bibliothèque existante, ou si vous créez un nouveau package, vous pouvez utiliser ce fichier .nuspec de modèle comme point de départ.
Vous devez remplir les sections pertinentes à l’intégration. La partie la plus importante est que tous les fichiers pour l’intégration doivent être dans un `xamarin.interactive` répertoire à la racine du package. Cela permet de rechercher facilement tous les fichiers appropriés pour votre intégration, indépendamment de si vous utilisez un package existant ou créez un nouveau.

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

puis le publier à [NuGet][nugetorg]. Une fois qu’il n’y figure, vous serez en mesure de le référencer à partir de n’importe quel classeur et voir en action. Dans la capture d’écran ci-dessous, nous avons empaqueté l’intégration de l’exemple que nous générés dans ce document et installé le package NuGet dans un classeur :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Classeur avec l’intégration](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Classeur avec l’intégration](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Notez que vous ne voyez pas les `#r` directives ou quoi que ce soit pour initialiser l’intégration, les classeurs a prise en charge tout cela pour vous en arrière-plan !

## <a name="next-steps"></a>Étapes suivantes

Découvrez notre autre documentation pour plus d’informations sur les éléments de déplacement qui composent le Kit de développement, et sur notre [exemple intégrations](~/tools/workbooks/samples/index.md) à des éléments supplémentaires à partir de l’intégration, comme la fourniture du code JavaScript personnalisé qui est exécuté dans le client de classeurs.

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
