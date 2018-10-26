---
title: Introduction aux convertisseurs personnalisés
description: Cet article fournit une introduction aux convertisseurs personnalisés et décrit le processus de création d’un convertisseur personnalisé.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 2b2b5726f4ca28ae37f027a700abdd688aa0b1d7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108429"
---
# <a name="introduction-to-custom-renderers"></a>Introduction aux convertisseurs personnalisés

_Renderers personnalisés fournissent une approche puissante permettant de personnaliser l’apparence et le comportement des contrôles Xamarin.Forms. Elles peuvent servir pour les styles de petites modifications ou sophistiquée disposition spécifique à la plateforme et personnalisation du comportement. Cet article fournit une introduction aux convertisseurs personnalisés et décrit le processus de création d’un convertisseur personnalisé._

Xamarin.Forms [Pages, des dispositions et des contrôles](~/xamarin-forms/user-interface/controls/index.md) présenter une API commune pour décrire des interfaces utilisateur mobiles multiplateformes. Chaque page, la disposition et le contrôle sont restitué différemment sur chaque plateforme, à l’aide un `Renderer` classe qui crée à son tour un contrôle natif (correspondant à la représentation sous forme de Xamarin.Forms), elle organise sur l’écran et ajoute le comportement spécifié dans le code partagé.

Les développeurs peuvent implémenter leurs propres classes `Renderer` pour personnaliser l’apparence et/ou le comportement d’un contrôle. Renderers personnalisés pour un type donné peuvent être ajoutés au projet d’une application pour personnaliser le contrôle au même endroit tout en autorisant le comportement par défaut sur d’autres plateformes ; ou différents convertisseurs personnalisés peuvent être ajoutés à chaque projet d’application pour créer une apparence différente sur iOS, Android et la plateforme universelle Windows (UWP). Toutefois, en implémentant une classe de convertisseur personnalisé pour effectuer une personnalisation de contrôle simple est souvent une réponse activable. Effets simplifient ce processus et sont généralement utilisés pour les styles de petites modifications. Pour plus d’informations, consultez [Effets](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Examen pourquoi personnalisé convertisseurs sont nécessaires

Modification de l’apparence d’un contrôle Xamarin.Forms, sans utiliser un convertisseur personnalisé, est un processus en deux étapes qui implique la création d’un contrôle personnalisé par l’intermédiaire de sous-classement et d’utilisation du contrôle personnalisé à la place le contrôle d’origine. L’exemple de code suivant montre un exemple de sous-classement le `Entry` contrôle :

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

Le `MyEntry` contrôle est un `Entry` contrôlent où le `BackgroundColor` est défini sur gris et peuvent être référencées dans Xaml en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur l’élément de contrôle. Le code suivant montre l’exemple comment la `MyEntry` contrôle personnalisé peut être consommé par un `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être quelconque. Toutefois, le `namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour référencer le contrôle personnalisé.

> [!NOTE]
> Définir le `xmlns` est beaucoup plus simple dans les projets de bibliothèque .NET Standard que les projets partagés. Une bibliothèque .NET Standard est compilée dans un assembly, il est facile de déterminer ce qu’il le `assembly=CustomRenderer` valeur doit être. Lorsque vous utilisez des projets partagés, toutes les ressources partagées (y compris le XAML) sont compilés dans chacun des projets de références, ce qui signifie que si l’iOS, Android et UWP projets ont leurs propres *noms d’assemblys* il est impossible d’écrire le `xmlns` déclaration, car la valeur doit être différent pour chaque application. Contrôles personnalisés dans XAML pour les projets partagés nécessitera chaque projet d’application à être configuré avec le même nom d’assembly.

Le `MyEntry` contrôle personnalisé est ensuite rendu sur chaque plateforme, avec un arrière-plan gris, comme indiqué dans les captures d’écran suivante :

![](introduction-images/screenshots.png "Contrôle personnalisé de MyEntry sur chaque plateforme")

Modification de la couleur d’arrière-plan du contrôle sur chaque plateforme a été accomplie purement par le biais de sous-classe le contrôle. Toutefois, cette technique est limitée qu’elle permet d’obtenir comme il n’est pas possible de tirer parti des améliorations spécifiques à la plateforme et les personnalisations. Lorsqu’ils sont requis, les convertisseurs personnalisés doivent être implémentés.

## <a name="creating-a-custom-renderer-class"></a>Création d’une classe de convertisseur personnalisé

Le processus de création d’une classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la classe de convertisseur qui restitue le contrôle natif.
1. Substituez la méthode qui restitue le contrôle natif et écrire une logique afin de personnaliser le contrôle. Souvent, le `OnElementChanged` méthode est utilisée pour restituer le contrôle natif, ce qui est appelé lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera être utilisé pour restituer le contrôle Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Pour la plupart des éléments de Xamarin.Forms, il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, le convertisseur par défaut pour la classe de base du contrôle sera être utilisé. Toutefois, les convertisseurs personnalisés sont nécessaires dans chaque projet de plateforme lors du rendu d’un [vue](xref:Xamarin.Forms.View) ou [ViewCell](xref:Xamarin.Forms.ViewCell) élément.

Les rubriques de cette série fournira des démonstrations et des explications de ce processus pour différents éléments de Xamarin.Forms.

## <a name="troubleshooting"></a>Résolution des problèmes

Si un contrôle personnalisé est contenu dans un projet de bibliothèque .NET Standard qui a été ajouté à la solution (autrement dit, pas la bibliothèque .NET Standard créée par Visual Studio pour le modèle de projet d’application de Xamarin.Forms Mac/Visual Studio), une exception peut se produire dans iOS quand tente d’accéder au contrôle personnalisé. Si ce problème se produit peut être résolu en créant une référence au contrôle personnalisé à partir de la `AppDelegate` classe :

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Cela force le compilateur doit reconnaître le `ClassInPCL` le type à sa résolution. Vous pouvez également le `Preserve` attribut peut être ajouté à la `AppDelegate` classe pour obtenir le même résultat :

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Cette opération crée une référence à la `ClassInPCL` type, indiquant qu’il n’est nécessaire lors de l’exécution. Pour plus d’informations, consultez [Code préservation](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Récapitulatif

Cet article propose une introduction aux convertisseurs personnalisés et a décrit le processus de création d’un convertisseur personnalisé. Renderers personnalisés fournissent une approche puissante permettant de personnaliser l’apparence et le comportement des contrôles Xamarin.Forms. Elles peuvent servir pour les styles de petites modifications ou sophistiquée disposition spécifique à la plateforme et personnalisation du comportement.


## <a name="related-links"></a>Liens associés

- [Effets](~/xamarin-forms/app-fundamentals/effects/index.md)
