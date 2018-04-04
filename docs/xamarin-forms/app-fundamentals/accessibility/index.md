---
title: Accessibilité
description: Création d’une application accessible garantit que l’application est utilisable par des personnes qui approchent de l’interface utilisateur avec une plage des besoins et des expériences.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4fb151b9664df7236d2c22ed54db09bf7bc65b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility"></a>Accessibilité

_Création d’une application accessible garantit que l’application est utilisable par des personnes qui approchent de l’interface utilisateur avec une plage des besoins et des expériences._

Rendre une application Xamarin.Forms accessibles signifie réfléchir à la disposition et la conception de nombreux éléments d’interface utilisateur. Pour connaître les instructions sur les problèmes à prendre en compte la [liste de vérification d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md). Grand nombre de problèmes d’accessibilité tels que des polices de grande taille et les paramètres de couleur et le contraste appropriés peut déjà être adressé en Xamarin.Forms APIs.

Le [accessibilité Android](~/android/app-fundamentals/accessibility.md) et [iOS accessibilité](~/ios/app-fundamentals/accessibility.md) guides contiennent des détails des API natives exposées par Xamarin et le [guide d’accessibilité UWP sur MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) explique le approche natif sur cette plateforme. Ces API sont utilisées pour implémenter les applications accessibles sur chaque plateforme.

Xamarin.Forms ne dispose pas actuellement *intégrés* prend en charge pour toutes les API disponibles sur chacune des plateformes sous-jacentes d’accessibilité. Toutefois, il prend en charge les valeurs du paramètre d’accessibilité sur les éléments d’interface utilisateur pour prendre en charge des outils de l’assistance des lecteur et de navigation d’écran, qui est une des parties plus importantes de la création d’applications accessibles. Pour plus d’informations, consultez [les valeurs du paramètre d’accessibilité sur les éléments d’Interface utilisateur](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md).

Autres API d’accessibilité (tels que [PostNotification sur iOS](~/ios/app-fundamentals/accessibility.md)) peuvent être mieux adaptés à un [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) ou [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) implémentation. Elles ne sont pas traitées dans ce guide.

## <a name="testing-accessibility"></a>Test de l’accessibilité

En général, les applications de Xamarin.Forms ciblent plusieurs plateformes, ce qui signifie que les fonctionnalités d’accessibilité en fonction de la plateforme de test. Suivez ces liens pour en savoir plus sur l’accessibilité sur chaque plateforme de test :

- [**iOS test**](~/ios/app-fundamentals/accessibility.md)
- [**Test Android**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [Définition des valeurs d’accessibilité sur les éléments d’interface utilisateur](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
