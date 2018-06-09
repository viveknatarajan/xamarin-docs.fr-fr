---
title: Thème de lumière de Xamarin.Forms
description: Cet article explique comment utiliser le thème de lumière Xamarin.Forms dans une application.
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 7f40e375d653acec60f8848627234ab46fcce8de
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245245"
---
# <a name="xamarinforms-light-theme"></a>Thème de lumière de Xamarin.Forms

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

> [!NOTE]
> Thèmes requièrent la version préliminaire de Xamarin.Forms 2.3. Vérifiez le [des conseils de dépannage](~/xamarin-forms/user-interface/themes/index.md) si des erreurs se produisent.

Pour utiliser le thème clair :

## <a name="1-add-nuget-packages"></a>1. Ajouter des packages Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. Ajouter au dictionnaire de ressources

Dans le **App.xaml** fichier ajouter une nouvelle personnalisée `xmlns` pour le thème, puis vérifiez que les ressources du thème sont fusionnés avec le dictionnaire de ressources de l’application.
Vous trouverez ci-dessous un exemple de fichier XAML :

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Charger des classes de thème

Suivez cette [dépannage étape](~/xamarin-forms/user-interface/themes/index.md) et ajoutez le code requis dans les projets d’application Android iOS.

## <a name="4-use-styleclass"></a>4. Utilisez StyleClass

Voici un exemple des boutons et des étiquettes dans le thème clair, ainsi que le balisage qui produit les.

[![](light-images/light-theme-sml.png "Boutons et des étiquettes dans le thème clair")](light-images/light-theme.png#lightbox "des boutons et des étiquettes dans le thème clair")

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

Le [la liste complète des classes intégrées](~/xamarin-forms/user-interface/themes/index.md) affiche les styles disponibles pour les contrôles communs.
