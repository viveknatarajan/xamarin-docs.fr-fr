---
title: Restituer les contrôles personnalisés dans le Générateur d’aperçu XAML
description: Cet article décrit comment afficher vos contrôles personnalisés dans le Générateur d’aperçu XAML.
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 977c29312e0be8b92f216c224414f9bd03f8562d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60875975"
---
# <a name="render-custom-controls-in-the-xaml-previewer"></a>Restituer les contrôles personnalisés dans le Générateur d’aperçu XAML

_Contrôles personnalisés ne fonctionnent parfois pas comme prévu dans le Générateur d’aperçu XAML. Utilisez les instructions dans cet article pour comprendre les limitations de l’aperçu de vos contrôles personnalisés._

## <a name="basic-preview-mode"></a>Base mode Aperçu

Même si vous n’avez pas créé votre projet, le Générateur d’aperçu XAML affichera vos pages. Jusqu'à ce que vous générez, n’importe quel contrôle qui s’appuie sur le code-behind affiche son type de base Xamarin.Forms. Lorsque votre projet est généré, le Générateur d’aperçu XAML tente d’afficher des contrôles personnalisés avec le rendu au moment du design est activé. Si le rendu échoue, il indique le type de base Xamarin.Forms.

## <a name="enable-design-time-rendering-for-custom-controls"></a>Activer le rendu au moment du design pour les contrôles personnalisés

Si vous faites vos propres contrôles personnalisés, ou que vous utilisez des contrôles à partir d’une bibliothèque tierce, le Générateur d’aperçu peut-être les afficher correctement. Contrôles personnalisés doivent s’abonner à concevoir le temps de rendu qui s’affiche dans le Générateur d’aperçu, si vous avez écrit le contrôle ou importée à partir d’une bibliothèque. Avec les contrôles que vous avez créé, ajoutez le [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute) à la classe de votre contrôle à afficher dans le Générateur d’aperçu :

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

Utilisez [classe de base de ImageCirclePlugin de James Montemagno](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs) comme exemple.


## <a name="skiasharp-controls"></a>Contrôles de SkiaSharp

Actuellement, SkiaSharp contrôles sont uniquement pris en charge lorsque vous prévisualisez sur iOS. Ils ne sont pas restituer sur la version préliminaire Android.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="check-your-xamarinforms-version"></a>Vérifier votre version de Xamarin.Forms
Vérifiez que vous disposez au moins 3.6 Xamarin.Forms installé. Vous pouvez mettre à jour votre version de Xamarin.Forms sur NuGet.

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>Même avec `[DesignTimeVisible(true)]`, mon contrôle personnalisé n’est pas rendu correctement.
Les contrôles personnalisés qui s’appuient sur les données de code-behind ou back-end ne fonctionnent pas toujours dans le Générateur d’aperçu XAML. Vous pouvez essayer :
* Déplacer le contrôle afin qu’il n’initialise pas si [en mode Création est activé.](index.md#detect-design-mode)
* Configuration de [données au moment de la conception](design-time-data.md) pour afficher des données fictives à partir du serveur principal

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>Le Générateur d’aperçu XAML affiche l’erreur « Les contrôles personnalisés ne sont pas rendu correctement »
Essayez de nettoyage et régénérer votre projet, ou la clôture et rouvrir le fichier XAML.
