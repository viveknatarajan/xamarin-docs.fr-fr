---
title: Contrôles XAML Standard (version préliminaire)
description: Cet article explore les contrôles XAML standard disponibles dans Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
ms.openlocfilehash: b9bf0e1ba14f4e8584bfd8492776ac7c8668df87
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61175176"
---
# <a name="xaml-standard-preview-controls"></a>Contrôles XAML Standard (version préliminaire)

![Preview](~/media/shared/preview.png)

Cette page répertorie les contrôles XAML Standard disponibles dans la version préliminaire, ainsi que leur équivalent contrôle Xamarin.Forms.

Il existe également une liste des contrôles qui ont des nouveaux noms de propriété et d’énumération dans XAML Standard.

## <a name="controls"></a>Contrôles

|Xamarin.Forms|XAML Standard|
|--- |--- |
|Frame|Bordure|
|Sélecteur|ComboBox|
|ActivityIndicator|Anneau de progression|
|StackLayout|StackPanel|
|Etiquette|TextBlock|
|Entrée|TextBox|
|Basculer|Bouton bascule|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Propriétés et des énumérations

|Contrôles Xamarin.Forms avec les propriétés mises à jour|Propriété de Xamarin.Forms ou enum|Équivalent XAML Standard|
|--- |--- |--- |
|Bouton, entrée, étiquette, DatePicker, éditeur, SearchBar, TimePicker|TextColor|Foreground|
|VisualElement|BackgroundColor|Arrière-plan *|
|Sélecteur, bouton|BorderColor, OutlineColor|BorderBrush|
|Bouton|BorderWidth|BorderThickness|
|Barre de progression|Progression|Value|
|Bouton, entrée, étiquette, éditeur, SearchBar, étendue, police|FontAttributesBold, Italic, None|FontStyleItalic, Normal|
|Bouton, entrée, étiquette, éditeur, SearchBar, étendue, police|FontAttributes|FontWeights *Bold, Normal|
|InputView|KeyboardDefault, Url, nombre, téléphone, texte, Chat, envoyer par courrier électronique|InputScopeNameValue * par défaut, Url, nombre, TelephoneNumber, texte, Chat, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientation *|

> [!IMPORTANT]
> Éléments signalés par * ne sont pas complets dans la version préliminaire actuelle

## <a name="related-links"></a>Liens associés

- [Aperçu NuGet](https://aka.ms/xf-xamlstandard-nuget)
