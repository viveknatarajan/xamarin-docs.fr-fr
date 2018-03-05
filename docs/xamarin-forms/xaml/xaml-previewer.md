---
title: "Générateur d’aperçu XAML pour Xamarin.Forms"
description: "Consultez vos dispositions Xamarin.Forms rendues telle que vous tapez !"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 02/24/2017
ms.openlocfilehash: 4798da42718658096509948bae07e5e5534d5af0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-previewer-for-xamarinforms"></a>Générateur d’aperçu XAML pour Xamarin.Forms

_Consultez vos dispositions Xamarin.Forms rendues telle que vous tapez !_

## <a name="requirements"></a>Configuration requise

Les projets requièrent le dernier package Xamarin.Forms NuGet pour l’aperçu de XAML travailler. Aperçu des applications Android nécessite [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Il existe plus d’informations dans le [notes de publication](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Prise en main

### <a name="visual-studio-for-mac-on-mac"></a>Visual Studio pour Mac sur Mac

Le **aperçu** bouton peut être affiché sur l’éditeur en double-cliquant sur un fichier XAML et en sélectionnant **ouvrir avec > visionneuse XAML**. Le volet de visualisation peut ensuite être affiché ou masqué en appuyant sur la **aperçu** situé dans l’angle supérieur droit de n’importe quelle fenêtre de document XAML :

[ ![Aperçu du contrôle ListView dans Visual Studio pour Mac](xaml-previewer-images/xamlp-list-sml.png "aperçu des formulaires dans Visual Studio pour Mac")](xaml-previewer-images/xamlp-list.png "aperçu des formulaires dans Visual Studio pour Mac")

### <a name="visual-studio-on-windows"></a>Visual Studio sur Windows

Utilisez le **vue > autres fenêtres > Aperçu de Xamarin.Forms** menu dans Visual Studio pour ouvrir la fenêtre d’aperçu. Utilisez le **fenêtre > Nouveau groupe d’onglets Vertical** menu pour la positionner côte-à-côte.

[ ![Aperçu du contrôle ListView dans Visual Studio](xaml-previewer-images/xamlp-list-vs-sml.png "aperçu des formulaires dans Visual Studio")](xaml-previewer-images/xamlp-list-vs.png "aperçu des formulaires dans Visual Studio")

## <a name="xaml-preview-options"></a>Options d’aperçu XAML

Les options en haut du volet d’aperçu sont :

* **Téléphone** – rendu dans un écran de taille de téléphone
* **Tablet** – rendu dans un écran de la taille de la tablette (Notez il existe des contrôles de zoom à l’angle inférieur droit du volet)
* **Android** – indiquent la version Android de l’écran
* **iOS** – indiquent la version iOS de l’écran
* Portrait (icône) – utilise une orientation portrait pour la version d’évaluation
* Paysage (icône) – utilise paysage pour la version d’évaluation

## <a name="adding-design-time-data"></a>Ajout de données au moment du Design

Certaines configurations peuvent être difficiles à visualiser exemptes de données liées à des contrôles d’interface utilisateur. Pour que la version préliminaire plus utile, affecter des données statiques pour les contrôles en coder en dur un contexte de liaison (soit dans le code-behind ou à l’aide de XAML).

Faire de James Montemagno [billet de blog sur l’ajout de données au moment du design](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) pour apprendre à lier à un ViewModel statique en XAML.

## <a name="troubleshooting"></a>Résolution des problèmes

Vérifiez les problèmes ci-dessous et le [Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms), si vous rencontrez des problèmes.

### <a name="xaml-preview-isnt-showing"></a>Aperçu de XAML n’est pas visible.

Procédez aux vérifications ci-dessous.

* Projet doit être généré (compilée) avant d’essayer d’afficher un aperçu des fichiers XAML.
* L’Agent de concepteur doit être correct la première fois que vous affichez un aperçu d’un fichier XAML, un indicateur de progression s’affiche dans le Générateur d’aperçu, ainsi que des messages de progression, jusqu'à ce qu’il est prêt.
* Essayez de fermer et rouvrir le fichier XAML.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>XAML non valide : Le projet Android doit générés avant l’aperçu peut être créé.

Le Générateur d’aperçu XAML requiert que le projet généré avant le rendu d’une page.
Si l’erreur suivante s’affiche en haut du volet d’aperçu, générez de nouveau l’application, puis réessayez.

![Message d’erreur : projet doit être généré en premier](xaml-previewer-images/error-not-built-sml.png "message d’erreur : régénérez le projet")
