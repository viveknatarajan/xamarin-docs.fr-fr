---
title: Les Classes de Base de convertisseur et contrôles natifs
description: Chaque contrôle Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Cet article répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms page, disposition, vue et la cellule et le convertisseur.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 56df2f7e6b83ddd4a5780506471cbd32a3aced40
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171948"
---
# <a name="renderer-base-classes-and-native-controls"></a>Les Classes de Base de convertisseur et contrôles natifs

_Chaque contrôle Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Cet article répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms page, disposition, vue et la cellule et le convertisseur._

À l’exception de la `MapRenderer` (classe), les convertisseurs spécifiques à la plateforme figurent dans les espaces de noms suivants :

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Plateforme Windows universelle (UWP)** – Xamarin.Forms.Platform.UWP

Le `MapRenderer` classe figurent dans les espaces de noms suivants :

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Plateforme Windows universelle (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Pages

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [Page](~/xamarin-forms/user-interface/controls/pages.md) type :

|Page|Convertisseur|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS : téléphone), TabletMasterDetailPageRenderer (iOS : tablette), MasterDetailRenderer (Android), MasterDetailPageRenderer (AppCompat Android), MasterDetailPageRenderer (UWP)|UIViewController (téléphone), UISplitViewController (tablette)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (contrôle personnalisé)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS et Android), NavigationPageRenderer (AppCompat Android), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (contrôle personnalisé)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS et Android), TabbedPageRenderer (AppCompat Android), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Dispositions

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [disposition](~/xamarin-forms/user-interface/controls/layouts.md) type :

|Mise en page|Convertisseur|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|Bordure|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|Visionneuse de défilement|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Vue|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Vue|FrameworkElement|

## <a name="views"></a>Affichages

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [vue](~/xamarin-forms/user-interface/controls/views.md) type :

|Affichages|Convertisseur|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|Barre de progression||Barre de progression|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS et Android), BoxViewRenderer (UWP)|UIView|ViewGroup||Rectangle|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Bouton|AppCompatButton|Bouton|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|Champ UITextField|EditText||Sélecteur de dates|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|Champ UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Image|
|`ImageButton`|ImageButtonRenderer|UIButton||AppCompatImageButton|Bouton|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|Affichage de liste||Affichage de liste|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|Champ UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|Barre de progression||Barre de progression|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|Localisateur||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Curseur|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Contrôle|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Basculer|SwitchCompat|Bouton bascule|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|Affichage de liste||Affichage de liste|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|Champ UITextField|EditText||Sélecteur d'heure|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>Cellules

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [cellule](~/xamarin-forms/user-interface/controls/cells.md) type :

|Cellules|Convertisseur|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell avec un champ UITextField|LinearLayout avec TextView et EditText|DataTemplate avec une zone de texte|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell avec un UISwitch|Basculer|DataTemplate avec une grille contenant un TextBlock et un ToggleSwitch|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout avec deux éléments TextViews|DataTemplate avec un StackPanel contenant deux TextBlocks|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell avec un UIImage|LinearLayout avec deux éléments TextViews et un ImageView|DataTemplate avec une grille contenant une Image et deux TextBlocks|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Vue|DataTemplate avec un ContentPresenter|

## <a name="summary"></a>Récapitulatif

Cet article a répertorié les classes de contrôle natif qui implémentent chaque Xamarin.Forms page, disposition, vue et la cellule et le convertisseur. Chaque contrôle Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif.

## <a name="related-links"></a>Liens associés

- [Renderers personnalisés (vidéo Xamarin University)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
