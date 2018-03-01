---
title: "Caractéristiques de la plateforme"
description: "Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: e1598241e64e6ed3a77fec3803e7fa4d94ee7433
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="platform-specifics"></a>Caractéristiques de la plateforme

_Caractéristiques de la plateforme vous permettent de vous permet d’utiliser la fonctionnalité est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou les effets._

Les fonctionnalités suivantes de la plateforme spécifique sont intégrée à Xamarin.Forms :

<table>
  <thead>
    <tr>
      <td><strong>iOS</strong></td>
      <td><strong>Android</strong></td>
      <td><strong>Fenêtres</strong></td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur">VisualElement.BlurEffect</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode">Application.WindowSoftInputModeAdjust</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement">Page.ToolbarPlacement</a></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title">NavigationPage.PrefersLargeTitles</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll">ListView.IsFastScrollEnabled</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar">MasterDetailPage.CollapsedPaneWidth and MasterDetailPage.CollapseStyle</a></td>
    </tr>    
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout">Page.UseSafeArea</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging">TabbedPage.IsSwipePagingEnabled</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar">NavigationPage.IsNavigationBarTranslucent</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation">Elevation.Elevation</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode">NavigationPage.StatusBarTextColorMode</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events">Application.SendDisappearingEventOnPause, Application.SendAppearingEventOnResume et Application.ShouldPreserveKeyboardOnResume</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size">Entry.AdjustsFontSizeToFitWidth</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode">Picker.UpdateMode</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility">Page.PrefersStatusBarHidden et Page.PreferredStatusBarUpdateAnimation</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches">ScrollView.ShouldDelayContentTouches</a></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

Le processus pour consommer un spécifique à la plateforme via XAML ou via une API de code fluent est comme suit :

1. Ajouter un `xmlns` déclaration ou `using` directive pour le [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) espace de noms.
1. Ajouter un `xmlns` déclaration ou `using` directive pour l’espace de noms qui contient les fonctionnalités spécifiques à la plateforme :
    1. Sur iOS, il s’agit du [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) espace de noms.
    1. Sur Android, il s’agit du [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) espace de noms. Pour Android AppCompat, il s’agit du [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) espace de noms.
    1. Sur la plateforme Windows universelle, il s’agit du [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) espace de noms.
1. Appliquer la spécifique à la plateforme à partir de XAML ou à partir de code avec la `On<T>` API fluent. La valeur de `T` peut être le [ `iOS` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/), [ `Android` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/), ou [ `Windows` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/) types à partir de la [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) espace de noms.

> [!NOTE]
> Notez que la tentative de consommer un spécifique à la plateforme sur une plateforme où il n’est pas disponible ne provoque pas une erreur. Au lieu de cela, le code s’exécute sans plateforme spécifiques à l’application.

Caractéristiques de la plateforme consommée via la `On<T>` fluent code retour API [ `IPlatformElementConfiguration` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/) objets. Cela permet à plusieurs caractéristiques de plateforme à appeler sur le même objet avec la méthode en cascade.

Pour plus d’informations sur les spécificités de la plateforme, consultez [caractéristiques de plateforme consommation](~/xamarin-forms/platform/platform-specifics/consuming/index.md) et [plate-forme de création-spécificités](~/xamarin-forms/platform/platform-specifics/creating.md).


## <a name="related-links"></a>Liens associés

- [Utilisation des caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Caractéristiques de la plateforme de création](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
