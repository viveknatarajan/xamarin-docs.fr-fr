---
title: Modificateurs de champ XAML dans Xamarin.Forms
description: 'L’attribut d’espace de noms x : FieldModifier, spécifie le niveau d’accès pour les champs générés pour les éléments XAML nommés.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075296"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Modificateurs de champ XAML dans Xamarin.Forms

_Le `x:FieldModifier` namespace (attribut) spécifie le niveau d’accès pour les champs générés pour les éléments XAML nommés._

## <a name="overview"></a>Vue d'ensemble

Les valeurs valides de l’attribut sont :

- `Public` : Spécifie que le champ généré pour l’élément XAML est `public`.
- `NotPublic` : Spécifie que le champ généré pour l’élément XAML est `internal` à l’assembly.

Si la valeur de l’attribut n’est pas définie, le champ généré pour l’élément sera `private`.

Les conditions suivantes doivent être remplies pour un `x:FieldModifier` attribut à traiter :

- L’élément XAML de niveau supérieur doit être un `x:Class`.
- L’élément XAML actuel a un `x:Name` spécifié.

Le XAML suivant montre des exemples de définition de l’attribut :

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> Le `x:FieldModifier` attribut ne peut pas être utilisé pour spécifier le niveau d’accès d’une classe XAML.
