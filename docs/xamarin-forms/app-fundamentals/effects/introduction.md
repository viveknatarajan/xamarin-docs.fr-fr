---
title: Introduction aux effets
description: "Autoriser les effets des contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour le style de petites modifications. Cet article fournit une introduction aux effets, décrit la limite entre les convertisseurs personnalisés et les effets et décrit la classe PlatformEffect."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b66f47ecb8f955f6558df6fff18af92a7a8b97cf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-effects"></a>Introduction aux effets

_Autoriser les effets des contrôles natifs sur chaque plateforme pour être personnalisées et sont généralement utilisés pour le style de petites modifications. Cet article fournit une introduction aux effets, décrit la limite entre les convertisseurs personnalisés et les effets et décrit la classe PlatformEffect._

Xamarin.Forms [contrôles, dispositions et les Pages](~/xamarin-forms/user-interface/controls/index.md) présente une API commune pour décrire des interfaces utilisateur de mobile multiplateforme. Chaque page, la mise en page et le contrôle sont restitué différemment sur chaque plateforme en utilisant un `Renderer` classe qui crée ensuite un contrôle natif (correspondant à la représentation sous forme de Xamarin.Forms), réorganise dans l’écran et ajoute le comportement spécifié dans le partagé code.

Les développeurs peuvent implémenter leurs propres classes `Renderer` pour personnaliser l’apparence et/ou le comportement d’un contrôle. Toutefois, en implémentant une classe de convertisseur personnalisé pour effectuer une personnalisation de contrôle simple est souvent une réponse activable. Effets de simplifient ce processus, ce qui permet des contrôles natifs sur chaque plateforme à personnaliser plus facilement.

Effets sont créés dans des projets spécifiques à la plateforme en sous-classant le `PlatformEffect` contrôle, puis les effets sont consommés par les joindre à un contrôle approprié dans un projet bibliothèque de classes Portable Xamarin.Forms (PCL) ou de la bibliothèque partagée.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Pourquoi utiliser un effet sur un convertisseur personnalisé ?

Effets simplifient la personnalisation d’un contrôle sont réutilisables et peuvent être paramétrées pour augmenter encore plus la réutilisation.

Tout ce qui peut être obtenue avec un effet peut être obtenu avec un convertisseur personnalisé. Toutefois, les convertisseurs personnalisés offrent plus de souplesse et de personnalisation à effets. Les instructions suivantes répertorient les circonstances dans lesquelles choisir un effet sur un convertisseur personnalisé :

- Un effet est recommandé lors de la modification des propriétés d’un contrôle spécifique à la plateforme permettra d’obtenir le résultat souhaité.
- Un convertisseur personnalisé est requis lorsqu’il est nécessaire de substituer les méthodes d’un contrôle spécifique à la plateforme.
- Un convertisseur personnalisé est requis lorsqu’il est nécessaire pour remplacer le contrôle spécifique à la plateforme qui implémente un contrôle de Xamarin.Forms.

## <a name="subclassing-the-platformeffect-class"></a>Le sous-classement de la classe PlatformEffect

Le tableau suivant répertorie l’espace de noms pour la `PlatformEffect` classe sur chaque plateforme et les types de ses propriétés :

<table>
 <thead>
   <tr>
     <td><strong>Plateforme</strong></td>
     <td><strong>Namespace</strong></td>
     <td><strong>conteneur</strong></td>
     <td><strong>Control</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td>iOS</a></td>
     <td>Xamarin.Forms.Platform.iOS</td>
     <td>UIView</td>
     <td>UIView</td>
   </tr>
   <tr>
     <td>Android</a></td>
     <td>Xamarin.Forms.Platform.Android</td>
     <td>ViewGroup</td>
     <td>Vue</td>
   </tr>
   <tr>
     <td>Windows Phone 8.1</a></td>
     <td>Xamarin.Forms.Platform.WinRT</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td>Plateforme Windows universelle (UWP)</a></td>
     <td>Xamarin.Forms.Platform.UWP</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

Chaque spécifique à la plateforme `PlatformEffect` classe expose les propriétés suivantes :

- `Container` – référence le contrôle spécifique à la plateforme qui est utilisé pour implémenter la mise en page.
- `Control` – référence le contrôle spécifique à la plateforme qui est utilisé pour implémenter le contrôle de Xamarin.Forms.
- `Element` – fait référence au contrôle Xamarin.Forms qui est rendu.

Effets n’ont pas de type d’informations sur le conteneur, le contrôle ou l’élément, qu'ils sont attachés à, car ils peuvent être attachés à tout élément. Par conséquent, lorsqu’un effet est attaché à un élément qu’il ne prend pas en charge il doit offrir un traitement réduit ou lever une exception. Toutefois, le `Container`, `Control`, et `Element` propriétés peuvent être converties en type d’implémentation. Pour plus d’informations sur ces types de voir [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Chaque spécifique à la plateforme `PlatformEffect` classe expose les méthodes suivantes, qui doivent être substituées pour implémenter un effet :

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) – appelée lorsqu’un effet est attaché à un contrôle de Xamarin.Forms. Une version substituée de cette méthode, dans chaque classe d’effet de la plateforme spécifique, est l’endroit pour effectuer la personnalisation du contrôle, ainsi que la gestion des exceptions au cas où l’effet ne peut pas être appliqué au contrôle Xamarin.Forms spécifié.
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) – appelée lorsqu’un effet est détaché d’un contrôle de Xamarin.Forms. Une version substituée de cette méthode, dans chaque classe spécifique à la plateforme effet, est le lieu d’effectuer tout nettoyage effet tels qu’annuler l’inscription d’un gestionnaire d’événements.

En outre, le `PlatformEffect` expose le [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/) (méthode), qui peut également être remplacée. Cette méthode est appelée lorsqu’une propriété de l’élément a changé. Une version substituée de cette méthode, dans chaque classe spécifique à la plateforme effet, est l’endroit pour répondre aux modifications de propriété pouvant être liée sur le contrôle de Xamarin.Forms. Une vérification de la propriété est modifiée doit toujours être rendue, comme ce remplacement peut être appelé plusieurs fois.


## <a name="related-links"></a>Liens associés

- [Renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
