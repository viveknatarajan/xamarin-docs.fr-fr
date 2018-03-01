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
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>Contrôles XAML Standard (version préliminaire)

![Preview](~/media/shared/preview.png)

Cette page répertorie les contrôles XAML Standard disponibles dans la version préliminaire, ainsi que leur contrôle Xamarin.Forms équivalent.

Il existe également une liste de contrôles qui ont des nouveaux noms de propriété et d’énumération dans XAML Standard.

## <a name="controls"></a>Contrôles

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>XAML Standard</th></tr>
  <tr><td>Frame</td><td>Bordure</td></tr>
  <tr><td>Sélecteur</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>Anneau de progression</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Ajouter des contrôles</td><td>TextBlock</td></tr>
  <tr><td>Entrée</td><td>TextBox</td></tr>
  <tr><td>Basculer</td><td>Bouton bascule</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>Propriétés et des énumérations

<table>
  <tr><th>Xamarin.Forms<br/>Contrôles avec les propriétés mises à jour</th><th>Xamarin.Forms<br/>Propriété ou Enum</th><th>XAML Standard<br/>Équivalent</th></tr>
  <tr><td>Button, Entry, Label, DatePicker, Editor, SearchBar, TimePicker</td><td>TextColor</td><td>Foreground</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Arrière-plan *</i></td></tr>
  <tr><td>Sélecteur, bouton</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Bouton</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>Barre de progression</td><td>Progression</td><td>Value</td></tr>
  <tr><td>Button, Entry, Label, Editor, SearchBar, Span, Font</td><td>Attributs de la police<br/>Gras, italique, aucun</td><td>FontStyle<br/>Italique, Normal</td></tr>
  <tr><td>Button, Entry, Label, Editor, SearchBar, Span, Font</td><td>Attributs de la police</td><td><i>FontWeights *</i><br/>Gras, Normal</td></tr>
  <tr><td>InputView</td><td>Clavier<br/>Par défaut, les Url, nombre, téléphone, texte, la conversation, par courrier électronique</td><td><i>InputScopeNameValue *</i><br/>Default, Url, Number, TelephoneNumber, Text, Chat, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Orientation *</i></td></tr>
</table>

> [!IMPORTANT]
> Éléments signalés par * ne sont pas complets dans la version préliminaire actuelle


## <a name="related-links"></a>Liens associés

- [Aperçu NuGet](https://aka.ms/xf-xamlstandard-nuget)
