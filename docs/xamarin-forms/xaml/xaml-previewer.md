---
title: Générateur d’aperçu XAML de Xamarin.Forms
description: Cet article explique comment utiliser le Générateur d’aperçu XAML pour voir vos dispositions Xamarin.Forms rendues en cours de frappe. Le Générateur d’aperçu XAML est disponible dans Visual Studio 2017 et Visual Studio pour Mac.
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: def7a7a3bdd9e165252c5ad1928b89ec654e5d74
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108080"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Générateur d’aperçu XAML de Xamarin.Forms

_Consultez vos dispositions Xamarin.Forms rendues en cours de frappe !_

## <a name="requirements"></a>Configuration requise

Les projets requièrent le dernier package Xamarin.Forms NuGet pour le Générateur d’aperçu XAML travailler. Aperçu des applications Android nécessite [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Il existe plus d’informations dans le [notes de version](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Prise en main

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Le Générateur d’aperçu XAML est activé par défaut et peut être contrôlé à partir de la **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Dans cette boîte de dialogue, vous pouvez sélectionner la vue de document par défaut et l’orientation de fractionnement.

[![Aperçu du contrôle ListView dans Visual Studio](xaml-previewer-images/xamlp-options-vs.png "Options de générateur d’aperçu de formulaires dans Visual Studio")](xaml-previewer-images/xamlp-options-vs.png#lightbox "Options de générateur d’aperçu de formulaires dans Visual Studio")

Ouverture d’une page XAML fractionne l’éditeur lorsque selon les paramètres sélectionnés dans le **Outils > Options > Xamarin > Générateur d’aperçu de formulaires** boîte de dialogue. Toutefois, ces préférences sont modifiables dans la fenêtre d’éditeur.

## <a name="xaml-preview-controls"></a>Contrôles de version préliminaire XAML

Le haut de la fenêtre d’éditeur possède des boutons pour sélectionner le volet est en cours d’utilisation, avec le bouton du haut vers le volet de conception de commutation et le bouton bas basculer vers le volet de la source. Le bouton central intervertit l’ordre de volet.

[![Aperçu du contrôle ListView dans Visual Studio](xaml-previewer-images/xamlp-controls-vs.png "contrôle de volet de générateur d’aperçu de formulaires dans Visual Studio")](xaml-previewer-images/xamlp-controls-vs.png#lightbox "contrôle de volet de générateur d’aperçu de formulaires dans Visual Studio")

Le bas de la fenêtre d’éditeur possède des boutons verticalement et horizontalement fractionner les volets et pour développer ou réduire le sous-volet actuel.

[![Aperçu du contrôle ListView dans Visual Studio](xaml-previewer-images/xamlp-controls2-vs.png "contrôle de volet de générateur d’aperçu de formulaires dans Visual Studio")](xaml-previewer-images/xamlp-controls2-vs.png#lightbox "contrôle de volet de générateur d’aperçu de formulaires dans Visual Studio")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Le **aperçu** bouton est affiché sur l’éditeur lorsque vous ouvrez une page XAML. Le volet de visualisation peut être affiché ou masqué en appuyant sur la **aperçu** bouton dans l’angle supérieur droit de n’importe quelle fenêtre de document XAML :

[![Aperçu du contrôle ListView dans Visual Studio pour Mac](xaml-previewer-images/xamlp-list-sml.png "Générateur d’aperçu de formulaires dans Visual Studio pour Mac")](xaml-previewer-images/xamlp-list.png#lightbox "Générateur d’aperçu de formulaires dans Visual Studio pour Mac")

-----

## <a name="xaml-preview-options"></a>Options d’aperçu XAML

Les options en haut du volet d’aperçu sont :

* **Téléphone** – s’affichent dans un écran de téléphone-taille
* **Tablette** – s’affichent dans un écran de la taille de la tablette (Notez il existe des contrôles de zoom en bas à droite du volet)
* **Android** – afficher la version Android de l’écran
* **iOS** – afficher la version iOS de l’écran
* Portrait (icône) : utilise l’orientation portrait pour la version préliminaire
* Paysage (icône) – utilise paysage pour la version préliminaire

## <a name="adding-design-time-data"></a>Ajout de données au moment du Design

Certaines configurations peuvent être difficiles à visualiser sans toutes les données liées à des contrôles d’interface utilisateur. Pour rendre la plus utile de la version préliminaire, affecter des données statiques pour les contrôles par codage en dur un contexte de liaison (soit dans le code-behind ou à l’aide de XAML).

Reportez-vous à de James Montemagno [billet de blog sur l’ajout de données au moment du design](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) pour voir comment lier à un ViewModel statique dans XAML.

## <a name="detecting-design-mode"></a>Détection du Mode de conception

La méthode statique [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) propriété peut être examinée pour déterminer si l’application s’exécute dans le Générateur d’aperçu. Cela vous permet de spécifier le code qui s’exécute uniquement lorsque l’application s’exécute dans le Générateur d’aperçu :

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>Résolution des problèmes

Vérifiez les problèmes ci-dessous et le [Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms), si vous rencontrez des problèmes.

### <a name="xaml-preview-isnt-showing"></a>Aperçu de XAML n’est pas visible.

Procédez aux vérifications ci-dessous.

* Projet doit être généré (compilé) avant de tenter d’afficher un aperçu des fichiers XAML.
* L’Agent de concepteur doit être de configurer la première fois que vous affichez un aperçu un fichier XAML, un indicateur de progression s’affiche dans le Générateur d’aperçu, ainsi que des messages de progression, jusqu'à ce que cela soit prêt.
* Essayez de fermer et rouvrir le fichier XAML.
* Vérifiez que votre `App` classe a un constructeur sans paramètre.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>XAML non valide : Le projet Android doit avant de pouvoir créer un aperçu

Le Générateur d’aperçu XAML nécessite que le projet généré avant le rendu d’une page.
Si l’erreur ci-dessous s’affiche en haut du volet d’aperçu, générez de nouveau l’application et réessayez.

![Message d’erreur : projet doit être généré en premier](xaml-previewer-images/error-not-built-sml.png "message d’erreur : régénérer le projet")
