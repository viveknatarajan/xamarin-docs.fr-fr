---
title: Thème sombre de Xamarin.Forms
description: Cet article explique comment utiliser le thème sombre Xamarin.Forms dans une application.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61186338"
---
# <a name="xamarinforms-dark-theme"></a>Thème sombre de Xamarin.Forms

![](~/media/shared/preview.png "Cette API est actuellement en version préliminaire")

> [!NOTE]
> Thèmes nécessitent la version 2.3 de Xamarin.Forms. Vérifier le [des conseils de dépannage](~/xamarin-forms/user-interface/themes/index.md) si des erreurs se produisent.

Pour utiliser le thème foncé :

## <a name="1-add-nuget-packages"></a>1. Ajout de packages Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Ajouter au dictionnaire de ressources

Dans le **App.xaml** fichier ajouter un nouveau personnalisé `xmlns` pour le thème, puis vérifiez les ressources du thème sont fusionnées avec le dictionnaire de ressources de l’application.
Vous trouverez ci-dessous un exemple de fichier XAML :

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Charger des classes de thème

Suivez ce [étape du dépannage](~/xamarin-forms/user-interface/themes/index.md) et ajoutez le code requis dans les projets d’application Android et iOS.

## <a name="4-use-styleclass"></a>4. Utiliser StyleClass

Voici un exemple de boutons et des étiquettes dans le thème foncé, ainsi que le balisage qui produit les.

[![](dark-images/dark-theme-sml.png "Boutons et des étiquettes dans le thème sombre")](dark-images/dark-theme.png#lightbox "boutons et des étiquettes dans le thème sombre")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

Le [la liste complète des classes intégrées](~/xamarin-forms/user-interface/themes/index.md) montre quels styles sont disponibles pour les contrôles communs.
