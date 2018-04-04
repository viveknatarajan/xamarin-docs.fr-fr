---
title: Styles de périphérique
description: Xamarin.Forms inclut six styles dynamiques, connus comme des styles de périphérique, dans la classe Device.Styles.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c52de431faa8e6d17b281ece87c862a49d30e886
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="device-styles"></a>Styles de périphérique

_Xamarin.Forms inclut six styles dynamiques, connus comme des styles de périphérique, dans la classe Device.Styles._

Le *périphérique* styles sont :

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

Tous les styles de six applicable uniquement aux [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances. Par exemple, un `Label` qui affiche le corps d’un paragraphe peut définir ses [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/).

L’exemple de code suivant montre comment utiliser le *périphérique* styles dans une page XAML :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Les styles de périphérique sont liés à l’utilisation de la `DynamicResource` extension de balisage. La nature dynamique des styles peut être consultée dans iOS en modifiant le **accessibilité** paramètres pour la taille du texte. L’apparence de la *périphérique* styles est différent sur chaque plateforme, comme indiqué dans les captures d’écran suivantes :

![](device-images/device-styles.png "Styles de périphérique sur chaque plateforme")

*APPAREIL* styles peuvent également être dérivés en définissant le [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) propriété le nom de clé pour le style de l’appareil. Dans l’exemple de code ci-dessus, `myBodyStyle` hérite [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/) et définit une couleur de texte accentuées. Pour plus d’informations sur l’héritage de style dynamique, consultez [dynamique l’héritage de Style](~/xamarin-forms/user-interface/styles/dynamic.md#dynamic-style-inheritance).

L’exemple de code suivant illustre la page équivalente dans c# :

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

Le [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propriété de chaque [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance est affectée à la propriété appropriée à partir de la [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) classe.

## <a name="accessibility"></a>Accessibilité

Le *périphérique* styles respectent les préférences d’accessibilité, afin de tailles de police change que les préférences d’accessibilité sont modifiées sur chaque plateforme. Par conséquent, pour prendre en charge le texte accessible, vérifiez que le *périphérique* styles sont utilisés comme base pour les styles de texte dans votre application.

Les captures d’écran suivantes illustrent les styles de périphérique sur chaque plateforme, avec la plus petite taille de police accessible :

[![](device-images/minimum-size.png "Styles de périphérique de petite taille accessible sur chaque plateforme")](device-images/minimum-size-large.png#lightbox "Styles de périphérique de petite taille Accessible sur chaque plateforme")

Les captures d’écran suivantes illustrent les styles de périphérique sur chaque plateforme, avec la plus grande taille accessible :

![](device-images/maximum-size.png "Styles de périphérique grand accessible sur chaque plateforme")

## <a name="summary"></a>Récapitulatif

Xamarin.Forms inclut six *dynamique* styles, appelés *périphérique* styles, dans le [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) classe. Tous les styles de six applicable uniquement aux [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instances.


## <a name="related-links"></a>Liens associés

- [Styles de texte](~/xamarin-forms/user-interface/text/styles.md)
- [Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamic Styles (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Utilisation de Styles (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
