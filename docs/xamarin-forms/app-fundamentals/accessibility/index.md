---
title: Accessibilité de Xamarin.Forms
description: Création d’une application accessible garantit que l’application est utilisable par des personnes qui approchent de l’interface utilisateur avec un éventail de besoins et les expériences.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: ac0ffbdce6b0c55e8ad9d774d80e3d9b8bf84089
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116444"
---
# <a name="xamarinforms-accessibility"></a>Accessibilité de Xamarin.Forms

_Création d’une application accessible garantit que l’application est utilisable par des personnes qui approchent de l’interface utilisateur avec un éventail de besoins et les expériences._

Rendre une application Xamarin.Forms accessibles signifie réfléchir à la disposition et la conception de nombreux éléments d’interface utilisateur. Pour obtenir des instructions sur les problèmes à prendre en compte, consultez le [liste de contrôle d’accessibilité](~/cross-platform/app-fundamentals/accessibility.md). Grand nombre de problèmes d’accessibilité telles que les grandes polices et les paramètres de couleur et le contraste appropriés peut déjà être adressé en APIs Xamarin.Forms.

Le [accessibilité Android](~/android/app-fundamentals/accessibility.md) et [iOS accessibilité](~/ios/app-fundamentals/accessibility.md) guides contiennent les détails des API natives exposées par Xamarin et le [guide d’accessibilité UWP sur MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) explique le approche natif sur cette plateforme. Ces API est utilisées pour implémenter entièrement les applications accessibles sur chaque plateforme.

Xamarin.Forms ne dispose pas actuellement *intégrés* prennent en charge pour toutes les API disponibles sur chacune des plateformes sous-jacentes d’accessibilité. Toutefois, il prend en charge définition des propriétés automation sur les éléments d’interface utilisateur pour prendre en charge des outils de l’assistance des lecteur et de navigation d’écran, qui est une des parties plus importantes de la création d’applications accessibles. Pour plus d’informations, consultez [propriétés Automation](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md).

Les applications Xamarin.Forms peuvent également avoir l’ordre de tabulation de contrôles spécifié. Pour plus d’informations, consultez [Navigation au clavier](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md).

Autres API d’accessibilité (tel que [PostNotification sur iOS](~/ios/app-fundamentals/accessibility.md)) peut être mieux adaptée à un [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) ou [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) implémentation. Ils ne sont pas couverts dans ce guide.

## <a name="testing-accessibility"></a>Test de l’accessibilité

Les applications Xamarin.Forms généralement plusieurs plateformes ciblent, ce qui signifie que les fonctionnalités d’accessibilité en fonction de la plateforme de test. Suivez ces liens pour apprendre à tester l’accessibilité sur chaque plateforme :

- [**iOS test**](~/ios/app-fundamentals/accessibility.md)
- [**Test Android**](~/android/app-fundamentals/accessibility.md)
- [**AccScope Windows (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>Liens associés

- [Accessibilité d’inter-plateformes](~/cross-platform/app-fundamentals/accessibility.md)
- [Propriétés de l’Automation](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [Accessibilité du clavier](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
