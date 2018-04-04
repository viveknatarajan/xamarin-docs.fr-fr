---
title: Compression des dispositions
description: Compression de la mise en page supprime les mises en page spécifiées à partir de l’arborescence d’éléments visuels dans le but d’améliorer les performances de rendu de page. Cet article explique comment activer la compression de la disposition et les avantages qu’elles peuvent apporter.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: 9c698d539ab671ee2a033ae5943a46e0cc870f76
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="layout-compression"></a>Compression des dispositions

_Compression de la mise en page supprime les mises en page spécifiées à partir de l’arborescence d’éléments visuels dans le but d’améliorer les performances de rendu de page. Cet article explique comment activer la compression de la disposition et les avantages qu’elles peuvent apporter._

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms effectue la disposition à l’aide de deux séries d’appels de méthode récursive :

- Mise en page commence en haut de l’arborescence d’éléments visuels avec une page, et il se poursuit dans toutes les branches de l’arborescence visuelle pour englober tous les éléments visuels sur une page. Éléments parents à d’autres éléments sont chargés de dimensionnement et de positionnement de leurs enfants par rapport à eux-mêmes.
- Invalidation est le processus par lequel une modification dans un élément sur une page déclenche un nouveau cycle de mise en page. Éléments sont considérés comme non valides quand ils n’ont plus la bonne taille ou la position. Chaque élément dans l’arborescence d’éléments visuels qui a des enfants est averti chaque fois qu’un de ses enfants change de taille. Par conséquent, une modification de la taille d’un élément dans l’arborescence d’éléments visuels peut entraîner des modifications ripple haut dans l’arborescence.

Pour plus d’informations sur comment Xamarin.Forms effectue la mise en page, consultez [création d’une disposition personnalisée](~/xamarin-forms/user-interface/layouts/custom.md).

Le résultat du processus de mise en page est une hiérarchie de contrôles natifs. Toutefois, cette hiérarchie inclut des convertisseurs de conteneur supplémentaires et de wrappers de convertisseurs de plateforme plus gonfler la hiérarchie d’imbrication. Plus le niveau d’imbrication, plus la quantité de travail que Xamarin.Forms doit effectuer pour afficher une page. Pour des dispositions complexes, la hiérarchie de la vue peut être approfondie et grandes, avec plusieurs niveaux d’imbrication.

Par exemple, considérez le bouton suivant de l’exemple d’application pour la connexion à Facebook :

![](layout-compression-images/facebook-button.png "Bouton de Facebook")

Ce bouton est spécifié comme un contrôle personnalisé avec la hiérarchie de vue XAML suivant :

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

La hiérarchie de l’affichage imbriqué résultante peut être examinée avec [Xamarin inspecteur](~/tools/inspector/index.md). Sur Android, la hiérarchie imbriquée contient les 17 vues :

![](layout-compression-images/no-compression.png "Afficher la hiérarchie pour le bouton de Facebook")

La compression de disposition, qui est disponible pour les applications de Xamarin.Forms sur la plateforme iOS et Android utilisent, vise à aplanir la vue en supprimant des dispositions spécifiées à partir de l’arborescence d’éléments visuels, ce qui peut améliorer les performances de rendu de page d’imbrication. Le gain de performances est remis varie selon la complexité d’une page, la version du système d’exploitation utilisé et l’appareil sur lequel l’application est en cours d’exécution. Toutefois, les gains en termes de performances les plus importants seront visibles sur les appareils les plus anciens.

> [!NOTE]
> Alors que cet article se concentre sur les résultats de l’application de la compression de disposition sur Android, il s’applique également à iOS.

## <a name="layout-compression"></a>Compression des dispositions

En XAML, la compression de page peut être activée en définissant le `CompressedLayout.IsHeadless` propriété attachée `true` sur une classe de disposition :

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Sinon, elle peut être activée en c# en spécifiant l’instance mise en forme en tant que premier argument de la `CompressedLayout.SetIsHeadless` méthode :

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Étant donné que la compression de la mise en page supprime une mise en page de l’arborescence d’éléments visuels, il ne convient pas pour les dispositions qui ont une apparence visuelle ou qui obtiennent la saisie tactile. Par conséquent, les dispositions qui ensemble [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) propriétés (telles que [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/), [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/), [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) et [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) ou qui acceptent des mouvements, ne sont pas des candidats pour la mise en page compression. Toutefois, l’activation de la compression de disposition sur une mise en page qui définit les propriétés d’apparence visuelle ou qui accepte les mouvements, pas entraîne une erreur de build ou d’exécution. Au lieu de cela, la compression de page est appliquée et propriétés d’apparence et de reconnaissance de mouvement, en mode silencieux échouera.

Pour le bouton de Facebook, la compression de page peut être activée sur les classes de trois disposition :

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

Sur Android, il en résulte dans une hiérarchie imbriquée de vue des 14 vues :

![](layout-compression-images/layout-compression.png "Afficher la hiérarchie pour bouton Facebook avec la Compression de la mise en page")

Par rapport à la hiérarchie d’origine de l’affichage imbriqué de 17 vues, cela représente une réduction du nombre de vues de 17 %. Cette réduction peut sembler non significatif, la réduction de la vue sur une page entière peut être plus importante.

### <a name="fast-renderers"></a>Renderers rapides

Convertisseurs rapides réduisent les coûts de rendu des contrôles Xamarin.Forms sur Android et de complications par la mise à plat de la hiérarchie résultante de la vue native. Cela améliore encore les performances en créant moins d’objets, qui à son tour des résultats dans une arborescence d’éléments visuels moins complexe et moins de mémoire. Pour plus d’informations sur les convertisseurs rapides, consultez [convertisseurs rapide](~/xamarin-forms/internals/fast-renderers.md).

Pour le bouton de Facebook dans l’exemple d’application, combinaison de compression de la mise en page et convertisseurs rapides génère une hiérarchie de l’affichage imbriqué de 8 vues :

![](layout-compression-images/layout-compression-with-fast-renderers.png "Afficher la hiérarchie pour bouton Facebook avec Compression de la mise en page et les convertisseurs rapides")

Par rapport à la hiérarchie d’origine de l’affichage imbriqué de 17 vues, cela représente une réduction de 52 %.

L’exemple d’application contient une page extraite à partir d’une application réelle. Sans la compression de la mise en page et les convertisseurs rapides, la page génère une hiérarchie imbriquée de vue de 130 vues sur Android. L’activation de convertisseurs rapides et la compression de la mise en page sur les classes de mise en page appropriée permet de réduire la hiérarchie imbriquée à 70 affichages, une réduction de 46 %.

## <a name="summary"></a>Récapitulatif

Compression de la mise en page supprime les mises en page spécifiées à partir de l’arborescence d’éléments visuels dans le but d’améliorer les performances de rendu de page. Les avantages en matière de performances de cette technique varient selon la complexité d’une page, la version du système d’exploitation utilisé et l’appareil sur lequel l’application est en cours d’exécution. Toutefois, les gains en termes de performances les plus importants seront visibles sur les appareils les plus anciens.


## <a name="related-links"></a>Liens associés

- [Création d’une disposition personnalisée](~/xamarin-forms/user-interface/layouts/custom.md)
- [Convertisseurs rapides](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
