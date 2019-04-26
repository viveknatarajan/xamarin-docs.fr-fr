---
title: Représentations dans Xamarin Workbooks
description: Ce document décrit le pipeline de représentation sous forme de Xamarin Workbooks, ce qui permet le rendu des résultats riches pour tout code qui retourne une valeur.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: d9aafbe13e06875b6577a4d2308e419932fd1589
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382111"
---
# <a name="representations-in-xamarin-workbooks"></a>Représentations dans Xamarin Workbooks

## <a name="representations"></a>Représentations sous forme de

Dans une session de classeur ou d’inspecteur, le code est exécuté et produit un résultat (par exemple, une méthode retournant une valeur ou le résultat d’une expression) qui est traité via le pipeline de représentation de l’agent. Tous les objets, à l’exception de primitives tels que les entiers, apparaîtront pour produire des graphes de membre interactive et un processus pour fournir des représentations autre que le client peut rendre plus riche. Objets de toute taille et la profondeur sont pris en charge en toute sécurité (y compris les cycles et les énumérables infinie) en raison de la communication à distance et différée et interactive de la réflexion.

Xamarin Workbooks fournit quelques types communs à tous les agents et les clients qui permettent de rendu riche des résultats. [`Color`][xir-color] est un exemple d’un tel type, où par exemple sur iOS, l’agent est responsable de la conversion `CGColor` ou `UIColor` des objets dans un `Xamarin.Interactive.Representations.Color` objet.

En plus des représentations communes, l’intégration du SDK fournit des API pour la sérialisation personnalisées représentations dans l’agent et le rendu des représentations dans le client.

## <a name="external-representations"></a>Représentations externes

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] offre la possibilité d’inscrire un [ `RepresentationProvider` ] [ repp], lequel une intégration doit implémenter pour convertir à partir d’un objet arbitraire à un formulaire indépendant pour effectuer le rendu. Ces formes agnostiques doivent implémenter le [ `ISerializableObject` ] [ serobj] interface.

Implémentation de la `ISerializableObject` interface ajoute une méthode Serialize qui contrôle avec précision la façon dont les objets sont sérialisés. Le `Serialize` méthode attend qu’un développeur spécifierez exactement quelles propriétés sont à sérialiser, et quel sera le nom final. Examinez le `Person` de l’objet dans notre [`KitchenSink` exemple] [exemple], nous pouvons voir comment cela fonctionne :

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Si nous souhaitions offrir un sur-ensemble ou un sous-ensemble de propriétés à partir de l’objet d’origine, nous pouvons le faire avec `Serialize`. Par exemple, nous pouvons procéder comme suit pour fournir un précalculé `Age` propriété sur `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> Les API qui produisent `ISerializableObject` objets directement n’avez pas besoin d’être gérés par un `RepresentationProvider`. Si l’objet que vous souhaitez afficher est **pas** un `ISerializableObject`, vous pouvez gérer les encapsulant dans votre `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Une représentation sous forme de rendu

Convertisseurs sont implémentées dans JavaScript et auront accès à une version JavaScript de l’objet représenté par le biais de `ISerializableObject`. La copie de JavaScript aura également un `$type` propriété qui indique le nom de type .NET de type chaîne.

Nous vous recommandons d’utiliser TypeScript pour le code d’intégration de client, qui bien entendu se compile en vanilla JavaScript. Dans les deux cas, le SDK fournit [typages] [ typings] qui peut être référencé directement par TypeScript ou simplement appelé manuellement si vous écrivez vanille JavaScript est préféré.

Le point d’intégration principale pour le rendu est `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Ici, `PersonRenderer` implémente le `Renderer` interface. Consultez le [typages] [ typings] pour plus d’informations.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
