---
title: Fonctionnalités de la plateforme de Xamarin.Forms
description: Ce guide explique comment tirer parti des fonctionnalités spécifiques aux plateformes dans les applications Xamarin.Forms à l’aide de diverses techniques.
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
ms.openlocfilehash: 9bac53f71178ac321dea162d346295556a8f7adb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998751"
---
# <a name="xamarinforms-platform-features"></a>Fonctionnalités de la plateforme de Xamarin.Forms

Xamarin.Forms est extensible et vous permet d’incorporer des fonctionnalités spécifiques à la plateforme à l’aide de [effets](~/xamarin-forms/app-fundamentals/effects/index.md), [renderers personnalisés](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), le [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), le [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)et bien plus encore.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

Ce guide décrit comment implémenter Material Design en mettant à jour des applications Android de Xamarin.Forms existantes.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[Indexation de l’application et lien profond](deep-linking.md)

Indexation de l’application permet aux applications seraient sinon être oubliées après quelques utilise pour rester à apparaître dans les résultats de recherche. Lien profond permet aux applications répondent à un résultat de recherche qui contient les données d’application, généralement en accédant à une page référencée à partir d’un lien ciblé.

## <a name="device-classdevicemd"></a>[Classe d’appareil](device.md)

Comment utiliser le `Device` classe pour créer un comportement spécifique à la plateforme dans le code partagé et l’interface utilisateur (y compris à l’aide de XAML). Couvre également `BeginInvokeOnMainThread` ce qui est essentiel lors de la modification des contrôles d’interface utilisateur à partir de threads d’arrière-plan.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

Certains styles iOS peut être effectuée via **Info.plist** et `UIAppearance` API. Ce guide inclut des exemples montrant comment ajouter des fonctionnalités d’iOS 9 à l’application iOS d’une solution Xamarin.Forms, y compris la recherche Spotlight de base.

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms prend désormais en charge de version préliminaire pour les applications GTK #.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms prend désormais en charge de version préliminaire pour les applications Mac OS.

## <a name="native-formsnative-formsmd"></a>[Formulaires natifs](native-forms.md)

Formulaires natifs autorisent Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-dérivée des pages à être consommés par les projets Xamarin.iOS, Xamarin.Android et Universal Windows Platform (UWP) natifs.

## <a name="native-viewsnative-viewsindexmd"></a>[Vues natives](native-views/index.md)

Vues natives à partir d’iOS, Android et la plateforme Windows universelle peuvent être référencées directement à partir de Xamarin.Forms. Propriétés et gestionnaires d’événements peuvent être définies sur les vues natives, et ils peuvent interagir avec les vues Xamarin.Forms.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Caractéristiques de la plateforme](platform-specifics/index.md)

Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans nécessiter de convertisseurs personnalisés ou des effets.

## <a name="pluginspluginsmd"></a>[Plugins](plugins.md)

Il existe un large éventail de modules open source sur Github, Nuget, ainsi que le Store du composant Xamarin pour aider à étendre les applications Xamarin.Forms.

## <a name="tizentizenmd"></a>[Tizen](tizen.md)

Tizen .NET vous permet de créer des applications .NET avec Xamarin.Forms et Tizen .NET framework.

## <a name="windowswindowsindexmd"></a>[Fenêtres](windows/index.md)

Xamarin.Forms prend en charge pour la plateforme Windows universelle (UWP) sur Windows 10. Cet article décrit comment ajouter un un projet UWP à une solution Xamarin.Forms existante.

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms prend désormais en charge de version préliminaire pour les applications Windows Presentation Foundation (WPF).
