---
title: "Contrôles XAML Standard (version préliminaire)"
description: "Comment commencer à Explorer l’aperçu Standard XAML dans Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: b044cb849f9a8e591a8db5907211a55f77d6e45f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
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

|Xamarin.FormsControls avec les propriétés mises à jour|Xamarin.FormsProperty ou Enum|XAML StandardEquivalent|
|--- |--- |--- |
|Button, Entry, Label, DatePicker, Editor, SearchBar, TimePicker|TextColor|Foreground|
|VisualElement|BackgroundColor|Arrière-plan *|
|Sélecteur, bouton|BorderColor, OutlineColor|BorderBrush|
|Bouton|BorderWidth|BorderThickness|
|Barre de progression|Progression|Value|
|Button, Entry, Label, Editor, SearchBar, Span, Font|FontAttributesBold, italique, aucun|FontStyleItalic, Normal|
|Button, Entry, Label, Editor, SearchBar, Span, Font|Attributs de la police|FontWeights * gras, Normal|
|InputView|Envoyer par courrier électronique KeyboardDefault, Url, nombre, téléphone, texte, conversation,|InputScopeNameValue *Default, Url, Number, TelephoneNumber, Text, Chat, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientation *|

> [!IMPORTANT]
> Éléments signalés par * ne sont pas complets dans la version préliminaire actuelle

## <a name="related-links"></a>Liens associés

- [Aperçu NuGet](https://aka.ms/xf-xamlstandard-nuget)
