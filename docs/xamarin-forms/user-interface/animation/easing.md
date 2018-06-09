---
title: Fonctions d’accélération dans Xamarin.Forms
description: Xamarin.Forms inclut une classe d’accélération qui vous permet de spécifier une fonction de transfert qui contrôle comment les animations accélérer ou ralentissent qu’ils s’exécutent. Cet article explique comment utiliser les fonctions d’accélération prédéfinies et comment créer des fonctions d’accélération personnalisées.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 9398a1b9cf4e5f6fd18f2213a7cf55e9cbb93ef0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243140"
---
# <a name="easing-functions-in-xamarinforms"></a>Fonctions d’accélération dans Xamarin.Forms

_Xamarin.Forms inclut une classe d’accélération qui vous permet de spécifier une fonction de transfert qui contrôle comment les animations accélérer ou ralentissent qu’ils s’exécutent. Cet article explique comment utiliser les fonctions d’accélération prédéfinies et comment créer des fonctions d’accélération personnalisées._


Le [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) classe définit un nombre de fonctions d’accélération qui peuvent être consommées par des animations :

- Le [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) fonction d’accélération rebondit l’animation au début.
- Le [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) fonction d’accélération rebondit l’animation à la fin.
- Le [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) fonction d’accélération lentement accélère l’animation.
- Le [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) fonction d’accélération accélère l’animation au début et ralentit l’animation à la fin.
- Le [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) fonction d’accélération rapidement ralentit l’animation.
- Le [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) fonction d’accélération utilise une vitesse constante et est fonction d’accélération par défaut.
- Le [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) fonction d’accélération sans heurts accélère l’animation.
- Le [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) fonction d’accélération sans heurts accélère l’animation au début et sans heurts ralentit l’animation à la fin.
- Le [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) fonction d’accélération sans heurts ralentit l’animation.
- Le [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) fonction d’accélération provoque l’animation accélérer très rapidement vers la fin.
- Le [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) fonction d’accélération d’entraîne décélération rapidement vers la fin de l’animation.

Le `In` et `Out` suffixes indiquent si l’effet fournie par la fonction d’accélération est visible au début de l’animation, à la fin, ou les deux.

En outre, les fonctions d’accélération personnalisées peuvent être créées. Pour plus d’informations, consultez [fonctions d’accélération personnalisée](#customeasing).

## <a name="consuming-an-easing-function"></a>Utilisation d’une fonction d’accélération

Les méthodes d’extension de l’animation dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) classe autorise une fonction d’accélération spécifier que le paramètre de méthode finale, comme illustré dans l’exemple de code suivant :

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

En spécifiant une fonction d’accélération à une animation, la vitesse d’animation devient non linéaire et a pour effet fournie par la fonction d’accélération. Omission d’une fonction d’accélération lors de la création d’une animation provoque l’animation d’utiliser la valeur par défaut [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) fonction, ce qui produit une vitesse linéaire d’accélération.

Pour plus d’informations sur les méthodes d’extension de l’animation dans le [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) de classe, consultez [Animations simples](~/xamarin-forms/user-interface/animation/simple.md). Fonctions d’accélération peuvent également être utilisées par le [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) classe. Pour plus d’informations, consultez [personnalisé Animations](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Fonctions d’accélération de personnalisée

Il existe trois approches principales pour la création d’une fonction d’accélération personnalisée :

1. Créer une méthode qui prend un `double` argument et retourne un `double` résultat.
1. Créer un `Func<double, double>`.
1. Spécifiez la fonction d’accélération en tant qu’argument à la [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) constructeur.

Dans les trois cas, la fonction d’accélération personnalisée renvoie 0 pour un argument de 0 et 1 pour un argument de 1. Toutefois, toute valeur peut être renvoyée entre les valeurs d’argument de 0 et 1. Chaque approche maintenant nous reviendrons à son tour.

### <a name="custom-easing-method"></a>Custom accélération (méthode)

Une fonction d’accélération personnalisée peut être définie comme une méthode qui prend un `double` argument et retourne un `double` résultat, comme illustré dans l’exemple de code suivant :

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

Le `CustomEase` méthode tronque la valeur entrante aux valeurs 0, 0,2, 0,4, 0,6, 0,8 et 1. Par conséquent, le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance est traduite en sauts discrets, plutôt que sans problème.

### <a name="custom-easing-func"></a>Personnaliser la fonction d’accélération

Une fonction d’accélération personnalisée peut également être définie comme un `Func<double, double>`, comme illustré dans l’exemple de code suivant :

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

Le `CustomEase` `Func` représente une fonction d’accélération qui commence rapide, ralentit et inverse des cours, puis inverse du cours à nouveau pour accélérer rapidement vers la fin. Par conséquent, tandis que le mouvement global de la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance est vers le bas, il annule également temporairement cours milieu de l’animation.

### <a name="custom-easing-constructor"></a>Custom accélération constructeur

Une fonction d’accélération personnalisée peut également être définie comme l’argument de la [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) constructeur, comme illustré dans l’exemple de code suivant :

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

La fonction d’accélération personnalisée est spécifiée comme un argument de fonction lambda le [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) constructeur et utilise le `Math.Cos` méthode pour créer un effet lente qui est bloqué par le `Math.Exp` (méthode). Par conséquent, le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) instance est traduite afin qu’il apparaisse à supprimer à son emplacement de repos final.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment consommer des fonctions d’accélération prédéfinies et comment créer des fonctions d’accélération personnalisées. Xamarin.Forms inclut une [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) classe qui vous permet de spécifier une fonction de transfert qui contrôle comment animations accélérer ou ralentir qu’ils s’exécutent.



## <a name="related-links"></a>Liens associés

- [Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)
- [Fonctions d’accélération (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Accélération](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
