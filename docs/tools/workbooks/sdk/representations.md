---
title: "Représentations sous forme de classeurs de Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a311bace159a450dc27e15baa8ef1c260a90c36e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="representations-in-xamarin-workbooks"></a>Représentations sous forme de classeurs de Xamarin

## <a name="representations"></a>représentations sous forme de

Dans une session de classeur ou d’inspecteur, le code qui s’exécute et produit un résultat (par exemple, une méthode retournant une valeur ou le résultat d’une expression) est traité via le pipeline de la représentation sous forme de l’agent. Tous les objets, à l’exception des primitives telles que des entiers, apparaîtront pour produire des graphiques interactifs membre et passent via un processus pour fournir des représentations sous forme autre que le client peut rendre plus riche. Objets de toute taille et la profondeur sont pris en charge en toute sécurité (y compris les cycles et les énumérables infinies) en raison de la communication à distance et la réflexion différée et interactif.

Les classeurs Xamarin fournit quelques types communs à tous les agents et les clients qui autorisent les élaborés de résultats. [`Color`][xir-color] est un exemple d’un tel type, où l’agent est chargée de convertir par exemple sur iOS, `CGColor` ou `UIColor` d’objets en un `Xamarin.Interactive.Representations.Color` objet.

En plus des représentations communes, l’intégration du SDK fournit des API pour la sérialisation personnalisée représentations sous forme de l’agent et le rendu des représentations dans le client.

## <a name="external-representations"></a>Représentations externes

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] Fournit la capacité d’enregistrer un [ `RepresentationProvider` ] [ repp], qu’une intégration doit implémenter pour convertir un objet arbitraire à un formulaire indépendant pour effectuer le rendu. Ces formes agnostiques doivent implémenter la [ `ISerializableObject` ] [ serobj] interface.

Mise en œuvre le `ISerializableObject` interface ajoute une méthode de sérialisation qui contrôle précisément la manière dont les objets sont sérialisés. Le `Serialize` méthode attend qu’un développeur spécifierez exactement quelles propriétés sont à sérialiser, et ce que sera le nom final. Examinez le `Person` de l’objet dans notre [`KitchenSink` exemple] [exemple], nous pouvons voir comment cela fonctionne :

```csharp
public sealed class Person : ISerializableObject
{
    public string Name { get; }

    // Rest of the code is omitted…

    void ISerializableObject.Serialize (ObjectSerializer serializer)
        => serializer.Property (nameof (Name), Name);
}
```

Si nous voulons fournir un sur-ensemble ou un sous-ensemble des propriétés de l’objet d’origine, nous pouvons faire avec `Serialize`. Par exemple, nous pouvons procéder comme suit pour fournir un précalculé `Age` propriété sur `Person`:

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
> API qui produisent `ISerializableObject` objets directement n’avez pas besoin d’être gérés par un `RepresentationProvider`. Si l’objet que vous voulez afficher est **pas** un `ISerializableObject`, vous pouvez gérer les encapsulant dans votre `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Une représentation sous forme de rendu

Les convertisseurs sont implémentées dans JavaScript et auront accès à la version de l’objet représenté par JavaScript `ISerializableObject`. La copie JavaScript aura également une `$type` propriété qui indique le nom du type .NET de type chaîne.

Nous vous recommandons d’utiliser TypeScript pour le code d’intégration de client, qui compile naturellement à la vanille JavaScript. Dans les deux cas, le SDK fournit [typages] [ typings] qui peut être référencé directement par TypeScript ou pour donner simplement manuellement si vous écrivez vanille JavaScript est préféré.

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

Ici, `PersonRenderer` implémente la `Renderer` interface. Consultez le [typages] [ typings] pour plus d’informations.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
