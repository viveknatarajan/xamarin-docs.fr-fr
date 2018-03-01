---
title: "Classes de Base de convertisseur et des contrôles natifs"
description: "Chaque contrôle Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Cet article répertorie les classes de contrôle natif qui implémentent chaque page Xamarin.Forms, disposition, afficher et cellule et le convertisseur."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 95518d9b23db68cc972549c730deeea968512444
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Classes de Base de convertisseur et des contrôles natifs

_Chaque contrôle Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Cet article répertorie les classes de contrôle natif qui implémentent chaque page Xamarin.Forms, disposition, afficher et cellule et le convertisseur._

À l’exception de la `MapRenderer` (classe), les convertisseurs spécifiques à la plateforme sont accessibles dans les espaces de noms suivants :

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Windows Phone 8** – Xamarin.Forms.Platform.WinPhone
- **WinRT** – Xamarin.Forms.Platform.WinRT
- **Plateforme Windows universelle (UWP)** – Xamarin.Forms.Platform.UWP

La `MapRenderer` classe sont accessibles dans les espaces de noms suivants :

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Windows Phone 8** – Xamarin.Forms.Maps.WP8
- **WinRT** – Xamarin.Forms.Maps.WinRT
- **Plateforme Windows universelle (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Pages

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [Page](~/xamarin-forms/user-interface/controls/pages.md) type :

<table>
 <thead>
   <tr>
     <td><strong>Page</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8 < /strong</td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md">PageRenderer</a></td>
     <td>UIViewController</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Volet</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a></td>
     <td><p>PhoneMasterDetailRenderer (iOS-téléphone)</p><p>TabletMasterDetailPageRenderer (iOS – Tablet PC)</p><p>MasterDetailRenderer (Android)</p><p>MasterDetailPageRenderer (AppCompat Android)</p><p>MasterDetailRenderer (Windows Phone)</p><p>MasterDetailPageRenderer (WinRT)</p></td>
     <td><p>UIViewController (Phone)</p><p>UISplitViewController (Tablet PC)</p></td>
     <td>DrawerLayout (v4)</td>
     <td>DrawerLayout (v4)</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (contrôle personnalisé)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a></td>
     <td><p>NavigationRenderer (iOS et Android)</p><p>NavigationPageRenderer (AppCompat Android)</p><p>NavigationPageRenderer (Windows Phone 8 et WinRT)</p></td>
     <td>UIToolbar</td>
     <td>ViewGroup</td>
     <td>ViewGroup</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (contrôle personnalisé)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a></td>
     <td><p>TabbedRenderer (iOS et Android)</p><p>TabbedPageRenderer (AppCompat Android)</p><p>TabbedPageRenderer (Windows Phone 8 et WinRT)</p></td>
     <td>UIView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Pivot)</td>
     <td>FrameworkElement (Pivot)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a></td>
     <td>PageRenderer</td>
     <td>UIViewController</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Volet</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage<a/></td>
     <td>CarouselPageRenderer</td>
     <td>UIScrollView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Panorama)</td>
     <td>FrameworkElement (FlipView)</td>
   </tr>
 </tbody>
</table>

## <a name="layouts"></a>Mises en page

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [disposition](~/xamarin-forms/user-interface/controls/layouts.md) type :

<table>
 <thead>
   <tr>
     <td><strong>Disposition</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Frame</a></td>
     <td>FrameRenderer</td>
     <td>UIView</td>
     <td>ViewGroup</td>
     <td>Bordure</td>
     <td>Bordure</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a></td>
     <td>ScrollViewRenderer</td>
     <td>UIScrollView</td>
     <td>ScrollView</td>
     <td>Visionneuse de défilement</td>
     <td>Visionneuse de défilement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Vue</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

## <a name="views"></a>Affichages

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [vue](~/xamarin-forms/user-interface/controls/views.md) type :

<table>
 <thead>
   <tr>
     <td><strong>Vues</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a></td>
     <td>ActivityIndicatorRenderer</td>
     <td>UIActivityIndicator</td>
     <td>Barre de progression</td>
     <td></td>
     <td>Barre de progression</td>
     <td>Barre de progression</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a></td>
     <td><p>BoxRenderer (iOS et Android)</p><p>BoxViewRenderer (Windows Phone et WinRT)</p></td>
     <td>UIView</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Rectangle</td>
     <td>Rectangle</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Button</a></td>
     <td>ButtonRenderer</td>
     <td>UIButton</td>
     <td>Bouton</td>
     <td>AppCompatButton</td>
     <td>Bouton</td>
     <td>Bouton</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/">CarouselView</a></td>
     <td>CarouselViewRenderer</td>
     <td>UIScrollView</td>
     <td>RecyclerView</td>
     <td></td>
     <td>FlipView</td>
     <td>FlipView</td>
   </tr>   
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></td>
     <td>DatePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>Sélecteur de dates</td>
     <td>Sélecteur de dates</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Éditeur</a></td>
     <td>EditorRenderer</td>
     <td>UITextView</td>
     <td>EditText</td>
     <td></td>
     <td>TextBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/entry.md">EntryRenderer</a></td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>PhoneTextBox/PasswordBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image</a></td>
     <td>ImageRenderer</td>
     <td>UIImageView</td>
     <td>ImageView</td>
     <td></td>
     <td>Image</td>
     <td>Image</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a></td>
     <td>LabelRenderer</td>
     <td>UILabel</td>
     <td>TextView</td>
     <td></td>
     <td>TextBlock</td>
     <td>TextBlock</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/listview.md">ListViewRenderer</a></td>
     <td>UITableView</td>
     <td>Affichage de liste</td>
     <td></td>
     <td>LongListSelector</td>
     <td>Affichage de liste</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/">Map</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md">MapRenderer</a></td>
     <td>MKMapView</td>
     <td>MapView</td>
     <td></td>
     <td>Carte</td>
     <td>MapControl</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Picker</a></td>
     <td>PickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td>EditText</td>
     <td>FrameworkElement</td>
     <td>ComboBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a></td>
     <td>ProgressBarRenderer</td>
     <td>UIProgressView</td>
     <td>Barre de progression</td>
     <td></td>
     <td>Barre de progression</td>
     <td>Barre de progression</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></td>
     <td>SearchBarRenderer</td>
     <td>UISearchBar</td>
     <td>Localisateur</td>
     <td></td>
     <td>PhoneTextBox</td>
     <td><p>SearchBox (WinRT)</p><p>AutoSuggestBox (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a></td>
     <td>SliderRenderer</td>
     <td>UISlider</td>
     <td>SeekBar</td>
     <td></td>
     <td>Curseur</td>
     <td>Curseur</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a></td>
     <td>StepperRenderer</td>
     <td>UIStepper</td>
     <td>LinearLayout</td>
     <td></td>
     <td>Bordure avec un StackPanel et deux boutons</td>
     <td><p>UserControl (WinRT)</p><p>Contrôle (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a></td>
     <td>SwitchRenderer</td>
     <td>UISwitch</td>
     <td>Basculer</td>
     <td>SwitchCompat</td>
     <td>Bordure avec un ToggleSwitchButton</td>
     <td>Bouton bascule</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a></td>
     <td>TableViewRenderer</td>
     <td>UITableView</td>
     <td>Affichage de liste</td>
     <td></td>
     <td>Grille avec un TextBlock et une zone de liste</td>
     <td>Affichage de liste</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></td>
     <td>TimePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>Sélecteur d'heure</td>
     <td>Sélecteur d'heure</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a></td>
     <td>WebViewRenderer</td>
     <td>UIWebView</td>
     <td>Affichage Web</td>
     <td></td>
     <td>Navigateur web</td>
     <td>Affichage Web</td>
   </tr>
 </tbody>
</table>

## <a name="cells"></a>Cellules

Le tableau suivant répertorie les classes de contrôle natif qui implémentent chaque Xamarin.Forms et le convertisseur [cellule](~/xamarin-forms/user-interface/controls/cells.md) type :

<table>
 <thead>
   <tr>
     <td><strong>Cells</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a></td>
   <td>EntryCellRenderer</td>
   <td>UITableViewCell avec un UITextField</td>
   <td>LinearLayout avec TextView et EditText</td>
   <td>DataTemplate avec une grille contenant un TextBlock et un PhoneTextBox</td>
   <td>DataTemplate avec une zone de texte</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a></td>
   <td>SwitchCellRenderer</td>
   <td>UITableViewCell avec un UISwitch</td>
   <td>Basculer</td>
   <td>DataTemplate avec un ToggleSwitch</td>
   <td>DataTemplate avec une grille contenant un TextBlock et un ToggleSwitch</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a></td>
   <td>TextCellRenderer</td>
   <td>UITableViewCell</td>
   <td>LinearLayout avec deux éléments TextViews</td>
   <td>DataTemplate avec un bouton contenant un StackPanel avec deux TextBlocks</td>
   <td>DataTemplate avec un StackPanel contenant deux TextBlocks</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a></td>
   <td>ImageCellRenderer</td>
   <td>UITableViewCell avec un UIImage</td>
   <td>LinearLayout avec deux éléments TextViews et un ImageView</td>
   <td>DataTemplate avec un bouton qui contient une grille avec une Image et un StackPanel contenant deux TextBlocks</td>
   <td>DataTemplate avec une grille contenant une Image et deux TextBlocks</td>  
   </td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/">ViewCell</a></td>
   <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md">ViewCellRenderer</a></td>
   <td>UITableViewCell</td>
   <td>Vue</td>
   <td>DataTemplate avec un ContentPresenter</td>
   <td>DataTemplate avec un ContentPresenter</td>  
   </td>
 </tr>
 </tbody>
</table>

## <a name="summary"></a>Récapitulatif

Cet article a répertorié les classes de contrôle natif qui implémentent chaque page Xamarin.Forms, disposition, afficher et cellule et le convertisseur. Chaque contrôle Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif.



## <a name="related-links"></a>Liens associés

- [Convertisseurs personnalisés (vidéo de l’université Xamarin)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
