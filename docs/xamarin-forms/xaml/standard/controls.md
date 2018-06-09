---
title: Contrôles XAML Standard (version préliminaire)
description: Cet article explore les contrôles XAML standard disponibles dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245565"
---
# <a name="xaml-standard-preview-controls"></a>Contrôles XAML Standard (version préliminaire)

![Preview](~/media/shared/preview.png)

Cette page répertorie les contrôles XAML Standard disponibles dans la version préliminaire, ainsi que leur contrôle Xamarin.Forms équivalent.

Il existe également une liste de contrôles qui ont des nouveaux noms de propriété et d’énumération dans XAML Standard.

## <a name="controls"></a>Contrôles

|Xamarin.Forms|XAML Standard|
|--- |--- |
|Frame|Bordure|
|Sélecteur|ComboBox|
|ActivityIndicator|Anneau de progression|
|StackLayout|StackPanel|
|Ajouter des contrôles|TextBlock|
|Entrée|TextBox|
|Basculer|Bouton bascule|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Propriétés et des énumérations

|Contrôles de Xamarin.Forms avec les propriétés mises à jour|Propriété de Xamarin.Forms ou enum|Équivalent XAML Standard|
|--- |--- |--- |
|Bouton, entrée, étiquette, DatePicker, éditeur, SearchBar, TimePicker|textColor|Foreground|
|VisualElement|BackgroundColor|Arrière-plan *|
|Sélecteur, bouton|BorderColor OutlineColor|BorderBrush|
|Bouton|BorderWidth|BorderThickness|
|Barre de progression|Progression|Value|
|Bouton, entrée, étiquette, éditeur, SearchBar, étendue, police|FontAttributesBold, italique, aucun|FontStyleItalic, Normal|
|Bouton, entrée, étiquette, éditeur, SearchBar, étendue, police|Attributs de la police|FontWeights * gras, Normal|
|InputView|Envoyer par courrier électronique KeyboardDefault, Url, nombre, téléphone, texte, conversation,|InputScopeNameValue * par défaut, Url, nombre, numéro de téléphone, texte, la conversation, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientation *|

> [!IMPORTANT]
> Éléments signalés par * ne sont pas complets dans la version préliminaire actuelle

## <a name="related-links"></a>Liens associés

- [Aperçu NuGet](https://aka.ms/xf-xamlstandard-nuget)
