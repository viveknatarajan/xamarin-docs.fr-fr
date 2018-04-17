---
title: Fonctionnalités de la plateforme
description: Tirer parti des fonctionnalités spécifiques à une plateforme avec Xamarin.Forms
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
ms.openlocfilehash: fd46411f3662652ef26addc76f273d6071401a6f
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="platform-features"></a>Fonctionnalités de la plateforme

Xamarin.Forms est extensible et vous permet d’incorporer des fonctionnalités spécifiques à une plateforme à l’aide de [effets](~/xamarin-forms/app-fundamentals/effects/index.md), [convertisseurs personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), le [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), le [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)et bien plus encore.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

Ce guide décrit comment implémenter des documents de conception en mettant à jour des applications Android de Xamarin.Forms existantes.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[Indexation de l’application et lien profond](deep-linking.md)

Indexation de l’application permet aux applications seraient autrement être oubliées après quelques utilise pour rester en apparaissant dans les résultats de la recherche. Lien profond permet à un résultat de recherche qui contient les données d’application, généralement en accédant à une page référencée à partir d’un lien vers des applications.

## <a name="device-classdevicemd"></a>[Classe d’appareil](device.md)

Comment utiliser la `Device` classe pour créer un comportement spécifique à la plateforme dans le code partagé et l’interface utilisateur (y compris à l’aide de XAML). Couvre également `BeginInvokeOnMainThread` est essentielle lorsque vous modifiez des contrôles d’interface utilisateur à partir de threads d’arrière-plan.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

Certains styles iOS peut être effectuée **Info.plist** et `UIAppearance` API. Ce guide comprend des exemples montrant comment ajouter des fonctionnalités d’iOS 9 à l’application iOS d’une solution Xamarin.Forms, y compris la recherche Spotlight de base.

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms prend désormais en charge préliminaire pour les applications GTK #.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms prend désormais en charge préliminaire pour les applications de macOS.

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms prend désormais en charge préliminaire pour les applications Windows Presentation Foundation (WPF).

## <a name="native-formsnative-formsmd"></a>[Formulaires natifs](native-forms.md)

Formulaires natifs autorisent Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-dérivées des pages à être consommés par les projets natifs Xamarin.iOS, Xamarin.Android et plateforme Windows universelle (UWP).

## <a name="native-viewsnative-viewsindexmd"></a>[Vues natives](native-views/index.md)

Vues natives dans iOS, Android et la plateforme Windows universelle peuvent être référencées directement à partir de Xamarin.Forms. Propriétés et les gestionnaires d’événements peuvent être définies sur les vues natifs, et ils peuvent interagir avec les vues de Xamarin.Forms.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Caractéristiques de la plateforme](platform-specifics/index.md)

Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans nécessiter de convertisseurs personnalisés ou les effets.

## <a name="pluginspluginsmd"></a>[Plugins](plugins.md)

Il existe une grande variété de plug-ins d’open source sur Github, Nuget et le magasin de composants Xamarin afin de prolonger les applications de Xamarin.Forms.

## <a name="windowswindowsindexmd"></a>[Fenêtres](windows/index.md)

Xamarin.Forms ne prend en charge quatre types de projet Windows :

* Windows Phone 8 Silverlight (la d’origine plate-forme Windows pris en charge par Xamarin.Forms),
* Windows Phone 8.1 (WinRT),
* Windows 8.1 (WinRT), et
* Plateforme Windows universelle (Windows 10).

Cette section décrit les différences entre eux et comment les ajouter à une solution Xamarin.Forms existante.
