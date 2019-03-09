---
title: Générateur d’aperçu XAML de Xamarin.Forms
description: Cet article explique comment utiliser le Générateur d’aperçu XAML pour voir vos dispositions Xamarin.Forms rendues en cours de frappe. Le Générateur d’aperçu XAML est disponible dans Visual Studio 2017, Visual Studio pour Mac et Visual Studio 2019 (version préliminaire).
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: 7834b87687db8fd1a3d51a40a4657b24e46bac17
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670400"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Générateur d’aperçu XAML de Xamarin.Forms

_Consultez vos dispositions Xamarin.Forms rendues en cours de frappe !_

## <a name="requirements"></a>Spécifications

Les projets requièrent le dernier package Xamarin.Forms NuGet pour le Générateur d’aperçu XAML travailler. Aperçu des applications Android nécessite [JDK 1.8 x64](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Il existe plus d’informations dans le [notes de version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Bien démarrer

Le Générateur d’aperçu du XAML de Xamarin.Forms affiche un aperçu de spécifique à la plateforme de votre fichier XAML en temps réel, pendant que vous les modifiez.

::: zone pivot="windows"

### <a name="visual-studio"></a>Visual Studio

Le Générateur d’aperçu XAML est disponible en développant le volet d’affichage fractionné. Fractionnement de comportement de la vue par défaut peut être contrôlé à partir de la **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Dans cette boîte de dialogue, vous pouvez sélectionner la vue de document par défaut et l’orientation de fractionnement.

[![Options de générateur d’aperçu de Xamarin.Forms dans Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "options de générateur d’aperçu de Xamarin.Forms dans Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Lorsque vous ouvrez une page XAML, l’éditeur sera fractionnée selon les paramètres sélectionnés dans le **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Toutefois, le fractionnement peut être modifié pour chaque fichier dans la fenêtre Éditeur.

#### <a name="xaml-preview-controls"></a>Contrôles de version préliminaire XAML

Le haut de la fenêtre d’éditeur possède des boutons pour sélectionner le volet est en cours d’utilisation, avec le bouton du haut vers le volet de conception de commutation et le bouton bas basculer vers le volet de la source. Le bouton central intervertit l’ordre de volet.

[![Contrôle de générateur d’aperçu de Xamarin.Forms pour basculer entre la conception, source et le mode fractionné dans Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "contrôle de générateur d’aperçu de Xamarin.Forms pour basculer entre la conception, source et le mode fractionné dans Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Le bas de la fenêtre d’éditeur possède des boutons verticalement et horizontalement fractionner les volets et pour développer ou réduire le sous-volet actuel.

[![Contrôles de l’orientation de volet de générateur d’aperçu de Xamarin.Forms dans Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "contrôles de l’orientation de volet de générateur d’aperçu de Xamarin.Forms dans Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

Le **aperçu** bouton est affiché sur l’éditeur lorsque vous ouvrez une page XAML. Le volet de visualisation peut être affiché ou masqué en appuyant sur la **aperçu** bouton dans l’angle supérieur droit de n’importe quelle fenêtre de document XAML :

[![Générateur d’aperçu de Xamarin.Forms dans Visual Studio pour Mac](xaml-previewer-images/xamlp-list-sml.png "Générateur d’aperçu de Xamarin.Forms dans Visual Studio pour Mac")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end
::: zone pivot="win-dev16"

### <a name="visual-studio-2019-preview"></a>Visual Studio 2019 (version préliminaire)

Le Générateur d’aperçu XAML est disponible en développant le volet d’affichage fractionné. Fractionnement de comportement de la vue par défaut peut être contrôlé à partir de la **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Dans cette boîte de dialogue, vous pouvez sélectionner la vue de document par défaut et l’orientation de fractionnement.

[![Options de générateur d’aperçu de Xamarin.Forms dans Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "options de générateur d’aperçu de Xamarin.Forms dans Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

Lorsque vous ouvrez une page XAML, l’éditeur sera fractionnée selon les paramètres sélectionnés dans le **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Toutefois, le fractionnement peut être modifié pour chaque fichier dans la fenêtre Éditeur.

#### <a name="xaml-preview-controls"></a>Contrôles de version préliminaire XAML

Le haut de la fenêtre d’éditeur possède des boutons pour sélectionner le volet est en cours d’utilisation, avec le bouton du haut vers le volet de conception de commutation et le bouton bas basculer vers le volet de la source. Le bouton central intervertit l’ordre de volet.

[![Contrôle de générateur d’aperçu de Xamarin.Forms pour basculer entre la conception, source et le mode fractionné dans Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "contrôle de générateur d’aperçu de Xamarin.Forms pour basculer entre la conception, source et le mode fractionné dans Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

Le bas de la fenêtre d’éditeur possède des boutons verticalement et horizontalement fractionner les volets et pour développer ou réduire le sous-volet actuel.

[![Contrôles de l’orientation de volet de générateur d’aperçu de Xamarin.Forms dans Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "contrôles de l’orientation de volet de générateur d’aperçu de Xamarin.Forms dans Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>Options de générateur d’aperçu XAML

Les options en haut du volet d’aperçu sont :

* **Téléphone** – version préliminaire sur un écran de téléphone en taille réelle
* **Tablette** -version préliminaire sur un écran Tablet PC en taille réelle
* **Android** – afficher la version Android de l’écran
* **iOS** – afficher la version iOS de l’écran (*Remarque : Si vous utilisez Visual Studio sur Windows, vous devez être [associé à un Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) pour utiliser ce mode*)
* **Portrait (icône)** – utilise l’orientation portrait pour la version préliminaire
* **Paysage (icône)** – utilise paysage pour la version préliminaire

::: zone pivot="win-dev16"

## <a name="device-drop-down-menu"></a>Menu déroulant des appareils

Dans 2019 à Visual Studio (version préliminaire), vous pouvez sélectionner un appareil pour afficher un aperçu sur à partir d’une liste déroulante des appareils Android ou iOS. Cette liste déroulante remplace les options « Phone » et « Tablet PC ». Lors de l’aperçu iOS, la liste déroulante affiche les appareils iPhone et iPad. Lors de l’aperçu Android, la liste déroulante affiche différents téléphones et tablettes Android. Les deux listes incluent les tailles d’écran et résolutions d’écran.

::: zone-end

## <a name="detect-design-mode"></a>Le mode de conception de détection

La méthode statique [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) propriété peut être examinée pour déterminer si l’application s’exécute dans le Générateur d’aperçu. Cela vous permet de spécifier le code qui s’exécute uniquement lorsque l’application s’exécute dans le Générateur d’aperçu :

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

::: zone pivot="win-dev16"

## <a name="enable-design-time-rendering-for-custom-controls"></a>Activer le rendu au moment du design pour les contrôles personnalisés

Contrôles personnalisés ont de choisir de concevoir le temps de rendu qui s’affiche dans le Générateur d’aperçu, que le contrôle se trouve dans votre projet, ou est importé à partir d’une bibliothèque. Cela est possible en ajoutant le [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute) à votre classe de contrôle :

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

Un exemple de ce peut être consulté dans [classe de base de ImageCirclePlugin de James Montemagno](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs).

::: zone-end

## <a name="add-design-time-data"></a>Ajouter des données au moment du design

Certaines configurations peuvent être difficiles à visualiser sans toutes les données liées à des contrôles d’interface utilisateur. Pour rendre la plus utile de la version préliminaire, affecter des données statiques pour les contrôles par codage en dur un contexte de liaison (soit dans le code-behind ou à l’aide de XAML).

Reportez-vous à de James Montemagno [billet de blog sur l’ajout de données au moment du design](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) pour voir comment lier à un ViewModel statique dans XAML.

## <a name="troubleshooting"></a>Résolution des problèmes

Vérifiez les problèmes ci-dessous et le [Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms), si vous rencontrez des problèmes.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>Générateur d’aperçu XAML n’est pas visible, ou affiche une erreur

Procédez aux vérifications ci-dessous.

* L’Agent de concepteur doit être de configurer la première fois que vous affichez un aperçu un fichier XAML, un indicateur de progression s’affiche dans le Générateur d’aperçu, ainsi que des messages de progression, jusqu'à ce que cela soit prêt.
* Essayez de fermer et rouvrir le fichier XAML.
* Vérifiez que votre `App` classe a un constructeur sans paramètre.

::: zone pivot="win-dev16"

### <a name="custom-controls-arent-rendering"></a>Contrôles personnalisés ne sont pas rendu.

Vérifiez que votre projet est généré. Si le Générateur d’aperçu ne peut pas restituer le contrôle sans erreur, ou créateur du contrôle ne pas choisir de rendu au moment de la conception, le Générateur d’aperçu montre la classe de base du contrôle.

::: zone-end
