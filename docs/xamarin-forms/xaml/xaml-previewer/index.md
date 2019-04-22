---
title: Générateur d’aperçu XAML de Xamarin.Forms
description: Cet article explique comment utiliser le Générateur d’aperçu XAML pour voir vos dispositions Xamarin.Forms rendues en cours de frappe. Le Générateur d’aperçu XAML est disponible dans Visual Studio 2019 et Visual Studio 2019 pour Mac.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: db243a9c8dcb25f51bc7926a7aa239531e9c24f6
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58859009"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Générateur d’aperçu XAML de Xamarin.Forms

_Consultez vos dispositions Xamarin.Forms rendues en cours de frappe_

## <a name="overview"></a>Vue d'ensemble

Le Générateur d’aperçu XAML vous montre à quoi votre page XAML de Xamarin.Forms ressemblera sur iOS et Android. Lorsque vous apportez des modifications à votre XAML, vous verrez leur effet immédiatement en même temps que votre code. Le Générateur d’aperçu XAML est disponible dans Visual Studio et Visual Studio pour Mac.

## <a name="getting-started"></a>Bien démarrer

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

Vous pouvez ouvrir le Générateur d’aperçu XAML en cliquant sur les flèches dans le volet d’affichage de fractionnement. Si vous souhaitez modifier la valeur par défaut fractionne le comportement de la vue, utilisez le **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Dans cette boîte de dialogue, vous pouvez sélectionner la vue de document par défaut et l’orientation de fractionnement.

[![Options de générateur d’aperçu de Xamarin.Forms dans Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "options de générateur d’aperçu de Xamarin.Forms dans Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Lorsque vous ouvrez un fichier XAML, l’éditeur s’ouvre en taille réelle ou suivant pour le Générateur d’aperçu, selon les paramètres sélectionnés dans le **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Toutefois, le fractionnement peut être modifié pour chaque fichier dans la fenêtre Éditeur.

#### <a name="xaml-preview-controls"></a>Contrôles de version préliminaire XAML

Choisissez si vous souhaitez voir votre code, le Générateur d’aperçu XAML, ou à la fois en sélectionnant ces boutons sur le fractionnement le volet d’affichage. Le bouton central permute ce que le Générateur d’aperçu de côté et votre code se trouvent sur :

[![Contrôle de générateur d’aperçu de Xamarin.Forms pour basculer entre la conception, source et le mode fractionné dans Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "contrôle de générateur d’aperçu de Xamarin.Forms pour basculer entre la conception, source et le mode fractionné dans Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Vous pouvez choisir si l’écran est divisée verticalement ou horizontalement, ou réduire un seul volet complètement :

[![Contrôles de l’orientation de volet de générateur d’aperçu de Xamarin.Forms dans Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "contrôles de l’orientation de volet de générateur d’aperçu de Xamarin.Forms dans Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

Le **aperçu** bouton est affiché sur l’éditeur lorsque vous ouvrez une page XAML. Afficher ou masquer le Générateur d’aperçu en appuyant sur la **aperçu** bouton dans l’angle supérieur droit de n’importe quelle fenêtre de document XAML :

[![Générateur d’aperçu de Xamarin.Forms dans Visual Studio pour Mac](xaml-previewer-images/xamlp-list-sml.png "Générateur d’aperçu de Xamarin.Forms dans Visual Studio pour Mac")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>Options de générateur d’aperçu XAML

Les options en haut du volet d’aperçu sont :

* **Android** – afficher la version Android de l’écran
* **iOS** – afficher la version iOS de l’écran (*Remarque : Si vous utilisez Visual Studio sur Windows, vous devez être [associé à un Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) pour utiliser ce mode*)
* **APPAREIL** -liste déroulante des appareils Android ou iOS, y compris la taille d’écran et de résolution
* **Portrait (icône)** – utilise l’orientation portrait pour la version préliminaire
* **Paysage (icône)** – utilise paysage pour la version préliminaire

## <a name="detect-design-mode"></a>Le mode de conception de détection

La méthode statique [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) propriété vous indique si l’application est en cours d’exécution dans le Générateur d’aperçu. À l’utiliser, vous pouvez spécifier le code qui s’exécute uniquement lorsque l’application est ou n’est pas en cours d’exécution dans le Générateur d’aperçu :

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}

if (!DesignMode.IsDesignModeEnabled)
{
  // Don't run in the Previewer  
}
```

Cette propriété est utile si vous initialisez une bibliothèque dans votre constructeur de la page qui ne parvient pas à exécuter au moment du design.

## <a name="troubleshooting"></a>Résolution des problèmes

Vérifiez les problèmes ci-dessous et le [Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms), si le Générateur d’aperçu ne fonctionne pas.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>Générateur d’aperçu XAML n’est pas visible, ou affiche une erreur

* Il peut prendre un certain temps pour le Générateur d’aperçu Démarrer, vous verrez l’initialisation de « rendu » jusqu'à ce qu’il est prêt.
* Essayez de fermer et rouvrir le fichier XAML.
* Vérifiez que votre `App` classe a un constructeur sans paramètre.
* Vérifier votre version de Xamarin.Forms - il doit être au moins Xamarin.Forms 3.6. Vous pouvez mettre à jour vers la dernière version de Xamarin.Forms via NuGet.
* Vérification de votre installation JDK - aperçu Android nécessite au moins [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Essayez un habillage initialisé classes dans la page C# code-behind dans `if (!DesignMode.IsDesignModeEnabled)`.

### <a name="custom-controls-arent-rendering"></a>Contrôles personnalisés ne sont pas rendu.

Essayez de générer votre projet. Le Générateur d’aperçu montre la classe de base du contrôle en cas d’échec restituer le contrôle, ou si le créateur du contrôle choisi à la sortie de rendu au moment du design. Pour plus d’informations, consultez [restituer les contrôles personnalisés dans le Générateur d’aperçu XAML](render-custom-controls.md).
