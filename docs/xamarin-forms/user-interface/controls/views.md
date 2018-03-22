---
title: Vues de Xamarin.Forms
description: "Xamarin.Forms vues sont les blocs de construction d’interfaces utilisateur de mobile multiplateforme."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: f7d27c5226741ec2b105633206ebaa0ac73d9a7b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="xamarinforms-views"></a>Vues de Xamarin.Forms

_Xamarin.Forms vues sont les blocs de construction d’interfaces utilisateur de mobile multiplateforme._

Les vues sont des objets d’interface utilisateur tels que des étiquettes, des boutons et des curseurs qui sont couramment appelées *contrôles* ou *widgets* dans d’autres environnements de programmation graphiques. Les vues prises en charge par Xamarin.Forms dérivent toutes de la [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) classe. Elles peuvent être divisées en plusieurs catégories :

## <a name="views-for-presentation"></a>Affichages de présentation

### <a name="label"></a>Ajouter des contrôles

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Affiche les chaînes de texte à ligne unique ou multilignes des blocs de texte, soit avec mise en forme de constante ou variable. Définir le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriété dans une chaîne pour une constante, ou ensemble la [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) propriété un [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) objet de variable mise en forme.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [Guide](~/xamarin-forms/user-interface/text/label.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Exemple d’étiquette](views-images/Label.png "exemple d’étiquette")](views-images/Label-Large.png#lightbox "exemple d’étiquette")<br /> [Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) affiche une image bitmap. Les images bitmap peuvent être téléchargées sur le Web, incorporés comme ressources dans l’ou les projets de plateforme commune ou créé à l’aide de .NET `Stream` objet.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [Guide](~/xamarin-forms/user-interface/images.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Exemple d’image](views-images/Image.png "exemple d’Image")](views-images/Image-Large.png#lightbox "exemple d’Image")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) affiche un rectangle plein coloré par le [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) propriété. `BoxView` a une demande de taille par défaut de 40 x 40. Pour d’autres tailles, affecter la [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) et [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) propriétés.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [Guide](~/xamarin-forms/user-interface/boxview.md) / [exemple 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), et [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Exemple de BoxView](views-images/BoxView.png "BoxView exemple")](views-images/BoxView-Large.png#lightbox "BoxView exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Affichage Web

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Affiche les pages Web ou du contenu, HTML, selon que le [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) est définie sur une [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) ou [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) objet.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [Guide](~/xamarin-forms/user-interface/webview.md) / [exemple 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) et [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Exemple de WebView](views-images/WebView.png "WebView exemple")](views-images/WebView-Large.png#lightbox "WebView exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) Affiche le graphique OpenGL dans les projets iOS et Android. Il n’existe aucune prise en charge pour la plateforme Windows universelle. Les projets iOS et Android requièrent une référence à la **OpenTK-1.0** assembly ou le **OpenTK** assembly de la version 1.0.0.0. `OpenGLView` Il est plus facile à utiliser dans un projet partagé. Si utilisé dans une bibliothèque PCL ou .NET Standard, puis un Service de dépendance sera également requis (comme indiqué dans l’exemple de code).<br /><br />Il s’agit de la fonctionnalité qu’aux graphiques intégrée dans Xamarin.Forms, mais une application Xamarin.Forms peut également rendre à l’aide de graphiques [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), ou [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![Exemple de OpenGLView](views-images/OpenGLView.png "OpenGLView exemple")](views-images/OpenGLView-Large.png#lightbox "OpenGLView exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) avec [code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Carte

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) affiche une carte. Le **Xamarin.Forms.Maps** le package Nuget doit être installé. Android et la plateforme Windows universelle nécessitent une clé d’autorisation de carte.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [Guide](~/xamarin-forms/user-interface/map.md) / [exemple](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Exemple de mappage](views-images/Map.png "mapper exemple")](views-images/Map-Large.png#lightbox "mapper l’exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Vues qui lancer des commandes

### <a name="button"></a>Bouton

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) est un objet rectangulaire qui affiche du texte, et qui se déclenche un [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) événement lorsqu’il est appuyé.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![Exemple de bouton](views-images/Button.png "bouton exemple")](views-images/Button-Large.png#lightbox "exemple de bouton")<br /> [Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) avec [code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) affiche une zone pour l’utilisateur à saisir une chaîne de texte et un bouton (ou une touche du clavier) qui signale l’application pour effectuer une recherche. Le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) propriété permet d’accéder au texte et le [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) événement indique que le bouton a été enfoncé.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![Exemple de SearchBar](views-images/SearchBar.png "SearchBar exemple")](views-images/SearchBar-Large.png#lightbox "SearchBar exemple")<br /> [Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) avec [code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Pour définir les valeurs des vues 

### <a name="slider"></a>Curseur

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) permet à l’utilisateur de sélectionner un `double` valeur d’une plage continue spécifiée avec la [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) et [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) propriétés.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) / [Guide](~/xamarin-forms/user-interface/slider.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Exemple de curseur](views-images/Slider.png "curseur exemple")](views-images/Slider-Large.png#lightbox "exemple de curseur")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) permet à l’utilisateur de sélectionner un `double` valeur d’une plage de valeurs incrémentielles spécifié avec la [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), et [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) propriétés.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![Exemple d’exécution pas à pas](views-images/Stepper.png "exemple d’exécution pas à pas")](views-images/Stepper-Large.png#lightbox "exemple d’exécution pas à pas")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Basculer 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) prend la forme d’un commutateur marche/arrêt pour permettre à l’utilisateur de sélectionner une valeur booléenne. Le [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) propriété est l’état du commutateur et le [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) événement est déclenché lorsque l’état change.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![Exemple de commutateur](views-images/Switch.png "basculer exemple")](views-images/Stepper-Large.png#lightbox "basculer l’exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>Sélecteur de dates

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) permet à l’utilisateur de sélectionner une date avec le sélecteur de dates de plateforme. Définir une plage de dates autorisées avec la [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) et [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) propriétés. Le [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) propriété correspond à la date sélectionnée et le [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) événement est déclenché lorsque cette propriété est modifiée.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [Guide](~/xamarin-forms/user-interface/datepicker.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Exemple de sélecteur de dates](views-images/DatePicker.png "DatePicker exemple")](views-images/DatePicker-Large.png#lightbox "exemple de sélecteur de dates")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>Sélecteur d'heure

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) permet à l’utilisateur à sélectionner une fois avec le sélecteur de temps de plateforme. Le [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) propriété est l’heure sélectionnée. Une application peut surveiller les modifications dans le `Time` propriété en installant un gestionnaire pour le [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) événement.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![Exemple de TimePicker](views-images/TimePicker.png "TimePicker exemple")](views-images/TimePicker-Large.png#lightbox "TimePicker exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Vues de modification de texte

Ces deux classes dérivées de la [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) (classe), qui définit le [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) propriété.

<a name="entry" />

### <a name="entry"></a>Entrée

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) permet à l’utilisateur à entrer et de modifier une seule ligne de texte. Le texte est disponible en tant que le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) propriété et le [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) et [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) les événements sont déclenchés lorsque les modifications de texte ou de l’utilisateur signale la fin en appuyant sur la touche ENTRÉE.<br /><br />Utilisez un [ `Editor` ](#editor) pour entrer et de modifier plusieurs lignes de texte.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [Guide](~/xamarin-forms/user-interface/text/entry.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Exemple d’entrée](views-images/Entry.png "exemple d’entrée")](views-images/Entry-Large.png#lightbox "exemple d’entrée")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Éditeur

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) permet à l’utilisateur à entrer et de modifier plusieurs lignes de texte. Le texte est disponible en tant que le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) propriété et le [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) et [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) les événements sont déclenchés lorsque les modifications de texte ou de l’utilisateur signale la fin.<br /><br />Utilisez un [ `Entry` ](#entry) vue pour la saisie et la modification d’une seule ligne de texte.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [Guide](~/xamarin-forms/user-interface/text/editor.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Exemple d’entrée](views-images/Editor.png "exemple d’éditeur")](views-images/Editor-Large.png#lightbox "exemple d’éditeur")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Vues pour indiquer l’activité

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) utilise une animation pour montrer que l’application est engagée dans une activité de longue durée sans donner toute indication de la progression. Le [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) propriété contrôle l’animation.<br /><br />Si la progression de l’activité est connue, utilisez un [ `ProgressBar` ](#progressbar) à la place.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![Exemple de ActivityIndicator](views-images/ActivityIndicator.png "ActivityIndicator exemple")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>Barre de progression

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) utilise une animation pour afficher la progression que via une activité de longue durée de l’application. Définir le [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) propriété pour les valeurs comprises entre 0 et 1 pour indiquer la progression.<br /><br />Si la progression de l’activité n’est pas connue, utilisez un [ `ActivityIndicator` ](#activityindicator) à la place.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![Exemple de barre de progression](views-images/ProgressBar.png "ProgressBar exemple")](views-images/ProgressBar-Large.png#lightbox "exemple de barre de progression")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) avec [code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Vues qui affichent des collections

### <a name="picker"></a>Sélecteur

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) affiche un élément sélectionné dans une liste de chaînes de texte et permet la sélection de cet élément lorsque l’utilisateur clique sur la vue. Définir le [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) propriété à une liste de chaînes, ou le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) propriété à une collection d’objets. Le [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) événement est déclenché lorsqu’un élément est sélectionné.<br /><br />Le `Picker` affiche la liste des éléments uniquement lorsqu’il est sélectionné. Utilisez un [ `ListView` ](#listView) ou [ `TableView` ](#tableView) pour une liste déroulante qui reste sur la page.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [Guide](~/xamarin-forms/user-interface/picker/index.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Exemple de sélecteur](views-images/Picker.png "exemple de sélecteur")](views-images/Picker-Large.png#lightbox "exemple de sélecteur")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) avec [code-behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>Affichage de liste

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) dérive de [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) et affiche une liste déroulante d’éléments de données pouvant être sélectionnées. Définir le [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) propriété à une collection d’objets et ensemble la [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) propriété un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) objet décrivant la façon dont les éléments sont à mettre en forme. Le [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) événement signale qu’une sélection a été effectuée, qui est disponible en tant que le [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) propriété.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [Guide](~/xamarin-forms/user-interface/listview/index.md) / [exemple](https://developer.xamarin.com/samples/WorkingWithListview) | [![Exemple de ListView](views-images/ListView.png "ListView exemple")](views-images/ListView-Large.png#lightbox "exemple de ListView")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Affiche la liste des lignes de type [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) avec les en-têtes et les en-têtes facultatifs. Définir le [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) propriété à un objet de type [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)et ajoutez [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) pour que les objets `TableRoot`. Chaque `TableSection` est une collection de `Cell` objets.<br /><br />[Documentation de l’API](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [Guide](~/xamarin-forms/user-interface/tableview.md) / [exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Exemple de TableView](views-images/TableView.png "TableView exemple")](views-images/TableView-Large.png#lightbox "TableView exemple")<br />[Code c# pour cette page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [page XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Exemple de Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
