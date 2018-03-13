---
title: Pages de Xamarin.Forms
description: "Xamarin.Forms Pages représentent des écrans de l’application mobile multi-plateforme."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5f979d2dbb894107d8d606ec1f41de44c294cdd3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinforms-pages"></a>Pages de Xamarin.Forms

_Xamarin.Forms Pages représentent des écrans de l’application mobile multi-plateforme._

Tous les types de page sont décrites ci-dessous dérivent la Xamarin.Forms [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) classe. Ces éléments visuels occupent tout ou partie de l’écran. A `Page` objet représente un `ViewController` dans iOS et une `Page` dans la plateforme Windows universelle. Sur Android, chaque page occupe de l’écran comme un `Activity`, mais sont des pages de Xamarin.Forms *pas* `Activity` objets.

[ ![](pages-images/pages-sml.png "Types de Page de Xamarin.Forms")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Pages

Xamarin.Forms prend en charge les types de page suivants :

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) est le type de la plus simple et le plus courant de page. Définir le [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) à une seule propriété [ `View` ](views.md) objet, qui est le plus souvent un [ `Layout` ](layouts.md) comme [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), ou [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![Exemple de ContentPage](pages-images/ContentPage.png "ContentPage exemple")](pages-images/ContentPage-Large.png#lightbox "ContentPage exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) gère deux volets d’informations. Définir le [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) propriété à une page présentant généralement une liste ou un menu. Définir le [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) propriété à une page présentant un élément sélectionné dans la page maître. Le [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) propriété détermine si la page maître ou détail est visible.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [Guide](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Exemple de MasterDetailPage](pages-images/MasterDetailPage.png "MasterDetailPage exemple")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) avec [code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| Le [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) gère la navigation parmi les autres pages à l’aide d’une architecture basée sur la pile. Lorsque vous utilisez la navigation entre les pages dans votre application, une instance de la page d’accueil doit être passée au constructeur d’un `NavigationPage` objet.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [Guide](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [exemple 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), et [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Exemple de NavigationPage](pages-images/NavigationPage.png "NavigationPage exemple")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [Page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) avec [code = derrière](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) abstraite [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) classe et permet de pages à l’aide des onglets de navigation entre les enfants. Définir le [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) propriété à une collection de pages, ou un ensemble la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) propriété à une collection d’objets de données et la [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) propriété à un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) décrivant comment chaque objet doit être représenté visuellement.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [Guide](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [exemple 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) et [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Exemple de TabbedPage](pages-images/TabbedPage.png "TabbedPage exemple")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) abstraite [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) classe et permet la navigation parmi les enfants des pages via le glissement de doigt. Définir le [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) propriété à une collection de [ `ContentPage` ](#contentPage) objets, ou un ensemble la [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) propriété à une collection d’objets de données et le [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) propriété un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) décrivant comment chaque objet doit être représenté visuellement.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [Guide](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [exemple 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) et [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Exemple de CarouselPage](pages-images/CarouselPage.png "CarouselPage exemple")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) Affiche le contenu de plein écran avec un modèle de contrôle, et est la classe de base pour [ `ContentPage` ](#contentPage).<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [Guide](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Exemple de TemplatedPage](pages-images/TemplatedPage.png "TemplatedPage exemple")](pages-images/TemplatedPage.png "TemplatedPage exemple") |
|     |     |

## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Exemple de Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
