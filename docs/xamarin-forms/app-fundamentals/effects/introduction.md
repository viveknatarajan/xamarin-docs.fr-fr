---
title: Introduction aux effets
description: Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour les styles de petites modifications. Cet article fournit une introduction aux effets décrit la limite entre les convertisseurs personnalisés et les effets et décrit la classe PlatformEffect.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997376"
---
# <a name="introduction-to-effects"></a>Introduction aux effets

_Les effets permettent les contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour les styles de petites modifications. Cet article fournit une introduction aux effets décrit la limite entre les convertisseurs personnalisés et les effets et décrit la classe PlatformEffect._

Xamarin.Forms [Pages, des dispositions et des contrôles](~/xamarin-forms/user-interface/controls/index.md) présente une API commune pour décrire des interfaces utilisateur mobiles multiplateformes. Chaque page, la disposition et le contrôle sont restitué différemment sur chaque plateforme à l’aide un `Renderer` classe qui crée à son tour un contrôle natif (correspondant à la représentation sous forme de Xamarin.Forms), elle organise sur l’écran et ajoute le comportement spécifié dans le partage code.

Les développeurs peuvent implémenter leurs propres classes `Renderer` pour personnaliser l’apparence et/ou le comportement d’un contrôle. Toutefois, en implémentant une classe de convertisseur personnalisé pour effectuer une personnalisation de contrôle simple est souvent une réponse activable. Effets simplifient ce processus, ce qui permet des contrôles natifs sur chaque plateforme à personnaliser plus facilement.

Les effets sont créés dans des projets spécifiques à la plateforme en sous-classant la `PlatformEffect` contrôle, puis les effets sont consommés en les attachant à un contrôle approprié dans une bibliothèque Xamarin.Forms .NET Standard ou d’un projet de bibliothèque partagée.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Pourquoi utiliser un effet sur un convertisseur personnalisé ?

Effets simplifient la personnalisation d’un contrôle sont réutilisables et peuvent être paramétrées afin de décupler réutilisation.

Tout ce qui peut être obtenue avec un effet peut également être obtenu avec un convertisseur personnalisé. Toutefois, les convertisseurs personnalisés offrent davantage de flexibilité et de possibilités de personnalisation que les effets. Les instructions suivantes répertorient les circonstances dans lesquelles choisir un effet sur un convertisseur personnalisé :

- Un effet est recommandé lors de la modification des propriétés d’un contrôle spécifique à la plateforme permettent d’obtenir le résultat souhaité.
- Un convertisseur personnalisé est requis lorsqu’il est nécessaire de substituer les méthodes d’un contrôle spécifique à la plateforme.
- Un convertisseur personnalisé est requis lorsqu’il est nécessaire de remplacer le contrôle spécifique à la plateforme qui implémente un contrôle Xamarin.Forms.

## <a name="subclassing-the-platformeffect-class"></a>Sous-classement de la classe PlatformEffect

Le tableau suivant répertorie l’espace de noms pour le `PlatformEffect` classe sur chaque plateforme et les types de ses propriétés :

|Plateforme|Espace de noms|Conteneur|Contrôle|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|Vue|
|Plateforme Windows universelle (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Chaque spécifiques à la plateforme `PlatformEffect` classe expose les propriétés suivantes :

- `Container` – font référence au contrôle spécifiques à la plateforme qui est utilisé pour implémenter la mise en page.
- `Control` – font référence au contrôle spécifiques à la plateforme qui est utilisé pour implémenter le contrôle de Xamarin.Forms.
- `Element` – font référence au contrôle Xamarin.Forms qui est rendu.

Effets n’ont pas de type d’informations sur le conteneur, le contrôle ou élément, qu'ils sont attachés à, car ils peuvent être attachés à n’importe quel élément. Par conséquent, lorsqu’un effet est attaché à un élément qu’il ne prend pas en charge il doit connaître une dégradation normale ou lever une exception. Toutefois, le `Container`, `Control`, et `Element` propriétés pouvant être castées en type d’implémentation. Pour plus d’informations sur ces types de voir [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Chaque spécifiques à la plateforme `PlatformEffect` classe expose les méthodes suivantes, qui doivent être substituées pour implémenter un effet :

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) : appelée quand un effet est attaché à un contrôle Xamarin.Forms. Une version substituée de cette méthode, dans chaque classe de l’effet de la plateforme spécifique, est l’endroit à effectuer la personnalisation du contrôle, ainsi que la gestion des exceptions dans le cas où l’effet ne peut pas être appliqué au contrôle Xamarin.Forms spécifié.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – appelée lorsqu’un effet est détaché d’un contrôle Xamarin.Forms. Une version substituée de cette méthode, dans chaque classe de l’effet de spécifique à la plateforme, est le lieu d’effectuer tout nettoyage effet telles que d’annuler l’inscription d’un gestionnaire d’événements.

En outre, le `PlatformEffect` expose le [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) (méthode), qui peut également être remplacée. Cette méthode est appelée lorsqu’une propriété de l’élément a changé. Une version substituée de cette méthode, dans chaque classe de l’effet de spécifique à la plateforme, est le lieu pour répondre aux modifications de propriété pouvant être liée sur le contrôle de Xamarin.Forms. Une vérification de la propriété est modifiée doit toujours être effectuée, comme ce remplacement peut être appelé plusieurs fois.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
