---
title: Vues natives en XAML
description: Vues natives dans iOS, Android et la plateforme Windows universelle peuvent être référencées directement à partir des fichiers XAML de Xamarin.Forms. Propriétés et les gestionnaires d’événements peuvent être définies sur les vues natifs, et ils peuvent interagir avec les vues de Xamarin.Forms. Cet article montre comment utiliser les vues natives à partir des fichiers XAML de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: b98a2b12dc2629ae7a5f2dd2a4de5c59452a19e4
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848471"
---
# <a name="native-views-in-xaml"></a>Vues natives en XAML

_Vues natives dans iOS, Android et la plateforme Windows universelle peuvent être référencées directement à partir des fichiers XAML de Xamarin.Forms. Propriétés et les gestionnaires d’événements peuvent être définies sur les vues natifs, et ils peuvent interagir avec les vues de Xamarin.Forms. Cet article montre comment utiliser les vues natives à partir des fichiers XAML de Xamarin.Forms._

Cet article traite des sujets suivants :

- [Utilisation des vues natives](#consuming) – le processus pour l’utilisation d’un mode natif à partir de XAML.
- [À l’aide de liaisons natives](#native_bindings) – liaison vers et depuis les propriétés des vues natifs de données.
- [Passer des arguments aux vues natives](#passing_arguments) : passer des arguments aux constructeurs de la vue native et méthodes de fabrique mode natif.
- [Référence aux vues natifs à partir du code](#native_view_code) : récupération des instances du mode natif déclaré dans un fichier XAML, à partir de son fichier code-behind.
- [Sous-classement vues natives](#subclassing) – sous-classement vues natives pour définir une API XAML conviviale.  

<a name="overview" />

## <a name="overview"></a>Vue d'ensemble

Pour incorporer un mode natif dans un fichier XAML de Xamarin.Forms :

1. Ajouter un `xmlns` déclaration d’espace de noms dans le fichier XAML pour l’espace de noms qui contient la vue native.
1. Créer une instance de la vue native dans le fichier XAML.

> [!NOTE]
> XAMLC doit être désactivé pour toutes les pages XAML qui utilisent des vues natifs.

Pour faire référence à une vue native à partir d’un fichier code-behind, vous devez utiliser un projet de ressource partagé (SAP) et encapsulez le code spécifique à la plateforme avec les directives de compilation conditionnelle. Pour plus d’informations, consultez [faisant référence aux vues natif à partir de Code](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Utilisation des vues natives

L’exemple de code suivant illustre la consommation d’affichages natives pour chaque plateforme pour un Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

Ainsi que la spécification de la `clr-namespace` et `assembly` pour un espace de noms du mode natif, un `targetPlatform` doit également être spécifié. Cela doit être défini sur une des valeurs de la [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) énumération et est généralement défini sur `iOS`, `Android`, ou `Windows`. Lors de l’exécution, l’analyseur XAML ignore tous les préfixes d’espace de noms XML qui ont un `targetPlatform` qui ne correspond pas à la plateforme sur laquelle l’application est en cours d’exécution.

Chaque déclaration d’espace de noms peut être utilisée pour faire référence à toute classe ou structure à partir de l’espace de noms spécifié. Par exemple, le `ios` déclaration de l’espace de noms peut être utilisée pour faire référence à toute classe ou structure à partir de l’e/s `UIKit` espace de noms. Propriétés du mode natif peuvent être définies via XAML, mais les types de propriété et d’objet doivent correspondre. Par exemple, le `UILabel.TextColor` est définie sur `UIColor.Red` à l’aide de la `x:Static` extension de balisage et la `ios` espace de noms.

Propriétés pouvant être liées et joint des propriétés pouvant être liées peuvent également être définies sur des vues natifs à l’aide de la `Class.BindableProperty="value"` syntaxe. Chaque mode natif est encapsulée dans une plateforme spécifique `NativeViewWrapper` instance, qui dérive de la [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) classe. Définissant une propriété pouvant être liée ou un jointe propriété pouvant être liée sur une vue native de transfère la valeur de propriété au wrapper. Par exemple, une disposition horizontale centrée peut être spécifiée en définissant `View.HorizontalOptions="Center"` sur le mode natif.

> [!NOTE]
> Notez que les styles ne peut pas être utilisés avec vues natifs, car les styles peuvent cibler uniquement les propriétés qui sont sauvegardées par `BindableProperty` objets.

Constructeurs de widget Android requièrent généralement l’Android `Context` de l’objet comme argument et cela peuvent être accessible via une propriété statique dans la `MainActivity` classe. Par conséquent, lorsque vous créez un widget Android en XAML, le `Context` objet doit généralement être passé à constructeur son à l’aide de la `x:Arguments` d’attribut avec un `x:Static` extension de balisage. Pour plus d’informations, consultez [en passant les Arguments à des vues natif](#passing_arguments).

> [!NOTE]
> Notez que d’affectation de noms une vue native avec `x:Name` n’est pas possible dans un projet de bibliothèque .NET Standard ou d’un projet de ressource partagé (SAP). Cette opération génère une variable du type natif, ce qui provoque une erreur de compilation. Toutefois, les vues natives peuvent être encapsulés dans `ContentView` instances et récupérées dans le fichier code-behind, sous réserve qu’un SAP est utilisé. Pour plus d’informations, consultez [faisant référence à une vue natif à partir de Code](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Liaisons natifs

Liaison de données est utilisée pour synchroniser une interface utilisateur avec sa source de données et simplifie la façon dont une application Xamarin.Forms affiche et interagit avec ses données. Autant que l’objet source implémente la `INotifyPropertyChanged` de l’interface, les modifications dans le *source* objet sont automatiquement envoyées à la *cible* objet par l’infrastructure de liaison et modifications dans le *cible* objet peut éventuellement être appliquée sur le *source* objet.

Propriétés des vues natifs peuvent également utiliser la liaison de données. L’exemple de code suivant illustre la liaison de données à l’aide des propriétés de vues natifs :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

La page contient un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) dont [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) propriété est liée à la `NativeSwitchPageViewModel.IsSwitchOn` propriété. Le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de la page est définie sur une nouvelle instance de la `NativeSwitchPageViewModel` classe dans le fichier code-behind, avec la classe ViewModel implémentant le `INotifyPropertyChanged` interface.

La page contient également un commutateur natif pour chaque plateforme. Pour chaque commutateur native utilise un [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) liaison met à jour la valeur de la `NativeSwitchPageViewModel.IsSwitchOn` propriété. Par conséquent, lorsque le commutateur est désactivé, le `Entry` est désactivé, et lorsque le commutateur est activé, le `Entry` est activé. Les captures d’écran suivantes affichent cette fonctionnalité sur chaque plateforme :

![](xaml-images/native-switch-disabled.png "Commutateur natif désactivé")
![](xaml-images/native-switch-enabled.png "commutateur natif activé")

Liaisons bidirectionnelles sont automatiquement pris en charge si la propriété native implémente `INotifyPropertyChanged`, ou prend en charge de la clé-valeur en observant (KVO) sur iOS, ou est un `DependencyProperty` sur UWP. Toutefois, plusieurs vues natifs ne prennent en charge la notification de modification de propriété. Pour ces vues, vous pouvez spécifier une [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) valeur de propriété dans le cadre de l’expression de liaison. Cette propriété doit être définie pour le nom d’un événement dans la vue native qui signale le moment où la propriété cible est modifiée. Ensuite, lorsque la valeur du commutateur natif est modifié, le `Binding` classe est averti que l’utilisateur a modifié la valeur du commutateur et le `NativeSwitchPageViewModel.IsSwitchOn` valeur de la propriété est mise à jour.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Passage des Arguments aux vues natifs

Arguments de constructeur peuvent être passés aux vues natifs à l’aide de la `x:Arguments` d’attribut avec un `x:Static` extension de balisage. En outre, les méthodes de fabrique de mode natif (`public static` méthodes qui retournent des objets ou des valeurs du même type que la classe ou structure qui définit les méthodes) peut être appelée en spécifiant la méthode nom à l’aide de la `x:FactoryMethod` attribut et ses arguments à l’aide de la `x:Arguments` attribut.

L’exemple de code suivant illustre ces deux techniques :

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

Le [ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) méthode de fabrique est utilisé pour définir le [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) à une nouvelle propriété [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) sur iOS. Le `UIFont` nom et la taille sont spécifiés par les arguments de méthode qui sont des enfants de le `x:Arguments` attribut.

Le [ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) méthode de fabrique est utilisé pour définir le [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) à une nouvelle propriété [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) sur Android. Le `Typeface` nom de famille et de style sont spécifiées par les arguments de méthode qui sont des enfants de le `x:Arguments` attribut.

Le [ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) constructeur est utilisé pour définir le [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) propriété vers un nouveau `FontFamily` sur la plateforme Windows universelle (UWP). Le `FontFamily` nom est spécifié par l’argument de la méthode qui est un enfant de le `x:Arguments` attribut.

> [!NOTE]
> Arguments doivent être les types requis par la méthode de constructeur ou de la fabrique.

Les captures d’écran suivantes affichent le résultat de la spécification des arguments de méthode et le constructeur de fabrique pour définir la police sur différentes vues natifs :

![](xaml-images/passing-arguments.png "Définition des polices sur les vues natifs")

Pour plus d’informations sur le passage d’arguments en XAML, consultez [en passant les Arguments en XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Référence aux vues natifs à partir du Code

Il n’est pas possible de nommer un mode natif avec le `x:Name` attribut, il est possible de récupérer une instance de la vue native déclarée dans un fichier XAML à partir de son fichier code-behind dans un projet d’accès partagé, sous réserve que le mode natif est un enfant d’un [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) qui spécifie un `x:Name` valeur d’attribut. Puis, à l’intérieur des directives de compilation conditionnelle dans le fichier code-behind, vous devez :

1. Récupérer le [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) propriété valeur et un cast vers un spécifique à la plateforme `NativeViewWrapper` type.
1. Récupérer le `NativeViewWrapper.NativeElement` propriété et effectuez un cast vers le type de mode natif.

L’API native peut ensuite être appelée sur la vue native pour effectuer les opérations de votre choisies. Cette approche offre également l’avantage que plusieurs vues natifs XAML pour les différentes plateformes peuvent être des enfants du même [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). L’exemple de code suivant illustre cette technique :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

Dans l’exemple ci-dessus, les vues natives pour chaque plateforme sont des enfants [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) contrôles, avec la `x:Name` valeur d’attribut qui est utilisé pour récupérer le `ContentView` dans le code-behind :

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

Le [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) propriété est accessible pour récupérer la vue native encapsulée comme une plateforme spécifique `NativeViewWrapper` instance. Le `NativeViewWrapper.NativeElement` propriété est ensuite accessible pour récupérer la vue native comme son type natif. API de la vue native est ensuite appelée pour effectuer les opérations de votre choisies.

IOS et Android boutons natifs partagent le même `OnButtonTap` Gestionnaire d’événements, car chaque bouton natif consomme un `EventHandler` déléguer en réponse à un événement tactile. Toutefois, la plateforme Windows universelle (UWP) utilise un distinct `RoutedEventHandler`, qui à son tour consomme la `OnButtonTap` Gestionnaire d’événements dans cet exemple. Par conséquent, lorsqu’un bouton natif est activé, le `OnButtonTap` Gestionnaire d’événements s’exécute, ce qui met à l’échelle et fait pivoter le contrôle natif contenu dans le [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) nommé `contentViewTextParent`. Les captures d’écran suivantes illustrent ce qui se produisent sur chaque plateforme :

![](xaml-images/contentview.png "ContentView contenant un contrôle natif")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Sous-classement vues natives

Nombreux iOS et Android vues natives ne conviennent pas pour l’instanciation en XAML, car ils utilisent des méthodes, plutôt que des propriétés, pour configurer le contrôle. La solution à ce problème consiste à des vues de native de sous-classe de wrappers qui définissent une API XAML conviviale plus qui utilise des propriétés pour le contrôle d’installation, et qui utilise les événements indépendant de la plateforme. Les vues natives encapsulées pouvant être placées dans un projet de ressource partagé (SAP) et entourés de directives de compilation conditionnelle, ou placés dans des projets spécifiques à la plateforme et référencées à partir de XAML dans un projet de bibliothèque .NET Standard.

L’exemple de code suivant montre une page Xamarin.Forms qui consomme sous-classé vues natifs :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

La page contient un [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) qui affiche les fruits choisis par l’utilisateur à partir d’un contrôle natif. Le `Label` lie à la `SubclassedNativeControlsPageViewModel.SelectedFruit` propriété. Le [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) de la page est définie sur une nouvelle instance de la `SubclassedNativeControlsPageViewModel` classe dans le fichier code-behind, avec la classe ViewModel implémentant le `INotifyPropertyChanged` interface.

La page contient également une vue sélecteur native pour chaque plateforme. Chaque mode natif affiche la collection de fruits en liant ses `ItemSource` propriété le `SubclassedNativeControlsPageViewModel.Fruits` collection. Cela permet à l’utilisateur de choisir un fruit, comme indiqué dans les captures d’écran suivantes :

![](xaml-images/sub-classed.png "Sous-classe vues natifs")

Sur iOS et Android les sélecteurs natifs utilisent des méthodes pour configurer les contrôles. Par conséquent, ces sélecteurs doivent être sous-classé pour exposer des propriétés pour les rendre compatibles avec le XAML. Sur la plate-forme de Windows universelle (UWP), le `ComboBox` est déjà XAML conviviale et par conséquent, ne nécessitent pas le sous-classement.

### <a name="ios"></a>iOS

Les sous-classes de la mise en œuvre iOS le [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) vue et expose des propriétés et un événement qui peut être facilement utilisé à partir de XAML :

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

Le `MyUIPickerView` classe expose `ItemsSource` et `SelectedItem` propriétés et un `SelectedItemChanged` événement. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) nécessite un sous-jacent [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) modèle de données, qui est accessible par le `MyUIPickerView` propriétés et événements. Le `UIPickerViewModel` modèle de données est fourni par la `PickerModel` classe :

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

Le `PickerModel` classe fournit le stockage sous-jacent de la `MyUIPickerView` (classe), via le `Items` propriété. Chaque fois que l’élément sélectionné dans le `MyUIPickerView` modifications, la [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) méthode est exécutée, les mises à jour l’index sélectionné et se déclenche le `ItemChanged` événement. Cela garantit que le `SelectedItem` propriété retourne toujours le dernier élément sélectionné par l’utilisateur. En outre, le `PickerModel` classe substitue les méthodes qui sont utilisées pour le programme d’installation le `MyUIPickerView` instance.

### <a name="android"></a>Android

Les sous-classes de la mise en œuvre Android le [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) vue et expose des propriétés et un événement qui peut être facilement utilisé à partir de XAML :

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

Le `MySpinner` classe expose `ItemsSource` et `SelectedObject` propriétés et un `ItemSelected` événement. Les éléments affichés par le `MySpinner` classe sont fournies par le [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) associé à la vue et les éléments sont remplies dans la `Adapter` lorsque le `ItemsSource` propriété est d’abord définie. Chaque fois que l’élément sélectionné dans le `MySpinner` classe les modifications, la `OnBindableSpinnerItemSelected` Gestionnaire d’événements met à jour le `SelectedObject` propriété.

## <a name="summary"></a>Récapitulatif

Cet article vous a montré comment consommer des vues natives à partir des fichiers XAML de Xamarin.Forms. Propriétés et les gestionnaires d’événements peuvent être définies sur les vues natifs, et ils peuvent interagir avec les vues de Xamarin.Forms.


## <a name="related-links"></a>Liens associés

- [NativeSwitch (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (exemple)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Formulaires natifs](~/xamarin-forms/platform/native-forms.md)
- [Passage des Arguments en XAML](~/xamarin-forms/xaml/passing-arguments.md)
