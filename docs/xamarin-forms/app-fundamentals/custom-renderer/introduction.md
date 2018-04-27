---
title: Introduction aux convertisseurs personnalisés
description: Les convertisseurs personnalisés fournissent une approche puissante pour personnaliser l’apparence et le comportement des contrôles de Xamarin.Forms. Ils peuvent être utilisés pour les modifications de conception de styles de petite ou sophistiquée spécifique à la plateforme disposition et personnalisation du comportement. Cet article fournit une introduction aux convertisseurs personnalisés et décrit le processus de création d’un convertisseur personnalisé.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 43b021b158bbb815ab8d27c393f54e0775599940
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
---
# <a name="introduction-to-custom-renderers"></a>Introduction aux convertisseurs personnalisés

_Les convertisseurs personnalisés fournissent une approche puissante pour personnaliser l’apparence et le comportement des contrôles de Xamarin.Forms. Ils peuvent être utilisés pour les modifications de conception de styles de petite ou sophistiquée spécifique à la plateforme disposition et personnalisation du comportement. Cet article fournit une introduction aux convertisseurs personnalisés et décrit le processus de création d’un convertisseur personnalisé._

Xamarin.Forms [contrôles, dispositions et les Pages](~/xamarin-forms/user-interface/controls/index.md) présenter une API commune pour décrire des interfaces utilisateur de mobile multiplateforme. Chaque page, la mise en page et le contrôle sont restitué différemment sur chaque plateforme, à l’aide un `Renderer` classe qui crée ensuite un contrôle natif (correspondant à la représentation sous forme de Xamarin.Forms), réorganise dans l’écran et ajoute le comportement spécifié dans le code partagé.

Les développeurs peuvent implémenter leurs propres classes `Renderer` pour personnaliser l’apparence et/ou le comportement d’un contrôle. Les convertisseurs personnalisés pour un type donné peuvent être ajoutés au projet d’une application pour personnaliser le contrôle dans un seul emplacement tout en autorisant le comportement par défaut sur d’autres plateformes ; ou différents convertisseurs personnalisés peuvent être ajoutés à chaque projet d’application pour créer une apparence différente sur iOS, Android et la plateforme Windows universelle (UWP). Toutefois, en implémentant une classe de convertisseur personnalisé pour effectuer une personnalisation de contrôle simple est souvent une réponse activable. Effets simplifient ce processus et sont généralement utilisés pour le style de petites modifications. Pour plus d’informations, consultez [Effets](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Examen pourquoi personnalisé convertisseurs sont nécessaires

Modification de l’apparence d’un contrôle Xamarin.Forms, sans l’aide d’un convertisseur personnalisé, est un processus en deux étapes qui implique la création d’un contrôle personnalisé via sous-classement et puis consommer le contrôle personnalisé à la place le contrôle d’origine. L’exemple de code suivant montre un exemple de sous-classement le `Entry` contrôle :

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

Le `MyEntry` contrôle est un `Entry` contrôler l’utilisation de la `BackgroundColor` est définie sur gris et peuvent être référencées dans Xaml en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur l’élément de contrôle. Le code suivant montre d’exemple comment la `MyEntry` contrôle personnalisé peut être consommé par un `ContentPage`:

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

Le `local` préfixe d’espace de noms peut être quoi que ce soit. Toutefois, le `namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour référencer le contrôle personnalisé.

> [!NOTE]
> Définir le `xmlns` est beaucoup plus simple dans PCLs que les projets partagés. Une bibliothèque PCL est compilée dans un assembly, il est facile de déterminer la `assembly=CustomRenderer` la valeur doit être. Lors de l’utilisation de projets partagés, tous les composants partagés (y compris le code XAML) sont compilés dans les projets de référence, ce qui signifie que si l’iOS, Android et UWP projets ont leurs propres *les noms d’assemblys* il est impossible d’écrire le `xmlns` déclaration, car la valeur doit être différent pour chaque application. Contrôles personnalisés dans XAML pour les projets partagés nécessitera chaque projet d’application soit configuré avec le même nom d’assembly.

Le `MyEntry` contrôle personnalisé est ensuite rendu sur chaque plateforme, avec un arrière-plan en gris, comme indiqué dans les captures d’écran suivants :

![](introduction-images/screenshots.png "Contrôle personnalisé de MyEntry sur chaque plateforme")

Modification de la couleur d’arrière-plan du contrôle sur chaque plateforme a été accomplie exclusivement par le biais de sous-classement du contrôle. Toutefois, cette technique est limitée qu’il peut atteindre car il n’est pas possible de tirer parti des améliorations spécifiques à la plateforme et de personnalisations. Lorsqu’ils sont requis, les convertisseurs personnalisés doivent être implémentées.

## <a name="creating-a-custom-renderer-class"></a>Création d’une classe de convertisseur personnalisé

Le processus de création d’une classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la classe de convertisseur qui restitue le contrôle natif.
1. Substituez la méthode qui restitue le contrôle natif et écrire une logique afin de personnaliser le contrôle. Souvent, le `OnElementChanged` méthode est utilisée pour restituer le contrôle natif, ce qui est appelé lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera utilisé pour restituer le contrôle de Xamarin.Forms. Cet attribut est utilisé pour inscrire le convertisseur personnalisé avec Xamarin.Forms.

> [!NOTE]
> Pour la plupart des éléments de Xamarin.Forms, il est facultatif fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, puis le convertisseur par défaut pour la classe de base du contrôle sera utilisé. Toutefois, les convertisseurs personnalisés sont nécessaires dans chaque projet de plateforme lors du rendu d’un [vue](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ou [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) élément.

Les rubriques de cette série fournissent des démonstrations et des explications de ce processus pour les différents éléments de Xamarin.Forms.

## <a name="troubleshooting"></a>Résolution des problèmes

Si un contrôle personnalisé est contenu dans un projet de bibliothèque de classes portables qui a été ajouté à la solution (autrement dit, pas la bibliothèque PCL créée par Visual Studio pour le modèle de projet application de Xamarin.Forms Mac/Visual Studio), une exception peut se produire dans iOS lorsque vous tentez d’accéder au contrôle personnalisé. Si ce problème se produit qu’il peut être résolu en créant une référence au contrôle personnalisé à partir de la `AppDelegate` classe :

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Cela force le compilateur de reconnaître le `ClassInPCL` type à résoudre. Vous pouvez également le `Preserve` attribut peut être ajouté à la `AppDelegate` classe pour obtenir le même résultat :

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Cette opération crée une référence à la `ClassInPCL` type, indiquant qu’il est nécessaire lors de l’exécution. Pour plus d’informations, consultez [Code conserver](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Récapitulatif

Cet article a fourni une introduction aux convertisseurs personnalisés et a décrit le processus de création d’un convertisseur personnalisé. Les convertisseurs personnalisés fournissent une approche puissante pour personnaliser l’apparence et le comportement des contrôles de Xamarin.Forms. Ils peuvent être utilisés pour les modifications de conception de styles de petite ou sophistiquée spécifique à la plateforme disposition et personnalisation du comportement.


## <a name="related-links"></a>Liens associés

- [Effets](~/xamarin-forms/app-fundamentals/effects/index.md)
