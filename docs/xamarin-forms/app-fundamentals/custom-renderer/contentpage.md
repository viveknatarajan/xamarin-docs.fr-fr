---
title: Personnalisation d’une ContentPage
description: Une ContentPage est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran. Cet article montre comment créer un convertisseur personnalisé pour la page ContentPage, permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995740"
---
# <a name="customizing-a-contentpage"></a>Personnalisation d’une ContentPage

_Une ContentPage est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran. Cet article montre comment créer un convertisseur personnalisé pour la page ContentPage, permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme._

Chaque contrôle Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Quand un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) est restitué par une application Xamarin.Forms, dans iOS le `PageRenderer` classe est instanciée, ce qui instancie à son tour native `UIViewController` contrôle. Sur la plateforme Android, le `PageRenderer` classe instancie un `ViewGroup` contrôle. Sur la plateforme de Windows universelle (UWP), le `PageRenderer` classe instancie un `FrameworkElement` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) et les contrôles natifs correspondants qui l’implémentent :

![](contentpage-images/contentpage-classes.png "Relation entre ContentPage classe et l’implémentation des contrôles natifs")

Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à la plateforme en créant un convertisseur personnalisé pour un [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_Xamarin.Forms_Page) une page de Xamarin.Forms.
1. [Consommer](#Consuming_the_Xamarin.Forms_Page) la page à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Page_Renderer_on_each_Platform) le convertisseur personnalisé pour la page sur chaque plateforme.

Chaque élément maintenant nous reviendrons à son tour, pour implémenter un `CameraPage` qui fournit une alimentation de caméra en direct et la possibilité de capturer une photo.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Création de la Page Xamarin.Forms

Un inchangée [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) peuvent être ajoutés au projet Xamarin.Forms partagé, comme indiqué dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

De même, le fichier code-behind pour le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) doit également rester inchangée, comme indiqué dans l’exemple de code suivant :

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

L’exemple de code suivant montre comment la page peut être créée en c# :

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Une instance de la `CameraPage` doit être utilisée pour afficher la caméra en direct de flux sur chaque plateforme. Personnalisation du contrôle est effectuée dans le convertisseur personnalisé, par conséquent, aucune implémentation supplémentaire n’est requise dans le `CameraPage` classe.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Utilisation de la Page Xamarin.Forms

Vide `CameraPage` doit être affichée par l’application Xamarin.Forms. Cela se produit lorsqu’un bouton sur le `MainPage` instance est activé par un clic, qui à son tour s’exécute à la `OnTakePhotoButtonClicked` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Ce code accède simplement à la `CameraPage`, sur les convertisseurs personnalisés doit personnaliser l’apparence de la page sur chaque plateforme.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Création du convertisseur de Page sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `PageRenderer` classe.
1. Remplacer le `OnElementChanged` méthode qui restitue la logique de page et d’écriture native pour personnaliser la page. Le `OnElementChanged` méthode est appelée lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur de page pour indiquer qu’il est utilisée pour restituer la page Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Il est facultatif pour fournir un convertisseur de page dans chaque projet de plateforme. Si un convertisseur de page n’est pas inscrit, puis le convertisseur par défaut pour la page sera utilisé.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que la relation entre eux :

![](contentpage-images/solution-structure.png "Responsabilités de projet de convertisseur CameraPage personnalisé")

Le `CameraPage` instance est restituée par spécifiques à la plateforme `CameraPageRenderer` classes qui dérivent de la `PageRenderer` classe pour cette plateforme. Ainsi, chaque `CameraPage` instance rendue avec un flux de la caméra en direct, comme indiqué dans les captures d’écran suivante :

![](contentpage-images/screenshots.png "CameraPage sur chaque plateforme")

Le `PageRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque la page Xamarin.Forms est créée pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms qui le convertisseur *a été* associée et l’élément Xamarin.Forms qui le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contiendra une référence à la `CameraPage` instance.

Une version substituée de la `OnElementChanged` méthode dans la `CameraPageRenderer` classe est l’endroit où effectuer la personnalisation de la page natif. Une référence à l’instance de page Xamarin.Forms qui est rendu peut être obtenue via la `Element` propriété.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type de la page Xamarin.Forms en cours d’affichage et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes décrivent l’implémentation de la `CameraPageRenderer` convertisseur personnalisé pour chaque plateforme.

### <a name="creating-the-page-renderer-on-ios"></a>Création du convertisseur de Page sur iOS

L’exemple de code suivant montre le convertisseur de page pour la plateforme iOS :

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un iOS `UIViewController` contrôle. Le flux de la caméra en direct est rendu uniquement à condition que le convertisseur n’est pas déjà attaché à un élément existant de Xamarin.Forms et la condition qu’il existe une instance de la page qui est restitué par le convertisseur personnalisé.

La page est ensuite personnalisée par une série de méthodes qui utilisent la `AVCapture` API pour fournir un flux en direct à partir de l’appareil photo et de la capacité à capturer une photo.

### <a name="creating-the-page-renderer-on-android"></a>Création du convertisseur de Page sur Android

L’exemple de code suivant montre le convertisseur de page pour la plateforme Android :

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un Android `ViewGroup` contrôle, qui est un groupe de vues. Le flux de la caméra en direct est rendu uniquement à condition que le convertisseur n’est pas déjà attaché à un élément existant de Xamarin.Forms et la condition qu’il existe une instance de la page qui est restitué par le convertisseur personnalisé.

La page est ensuite personnalisée en appelant une série de méthodes qui utilisent la `Camera` API afin de fournir le flux live à partir de l’appareil photo et de la capacité à capturer une photo, avant le `AddView` méthode est appelée pour ajouter l’appareil photo en direct l’interface utilisateur pour diffuser en continu le `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>Création du convertisseur de Page sur UWP

L’exemple de code suivant montre le convertisseur de page pour UWP :

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

L’appel à la classe de base `OnElementChanged` méthode instancie un `FrameworkElement` contrôle, sur lequel la page est affichée. Le flux de la caméra en direct est rendu uniquement à condition que le convertisseur n’est pas déjà attaché à un élément existant de Xamarin.Forms et la condition qu’il existe une instance de la page qui est restitué par le convertisseur personnalisé. La page est ensuite personnalisée en appelant une série de méthodes qui utilisent la `MediaCapture` API afin de fournir le flux live à partir de l’appareil photo et de la capacité à capturer une photo avant que la page personnalisée est ajoutée à la `Children` collection pour l’affichage.

Lorsque vous implémentez un convertisseur personnalisé qui dérive de `PageRenderer` sur UWP, le `ArrangeOverride` méthode doit également être implémentée pour organiser les contrôles de page, étant donné que le convertisseur de base ne sait pas quoi faire avec eux. Sinon, il en résulte une page vierge. Par conséquent, dans cet exemple le `ArrangeOverride` les appels de méthode le `Arrange` méthode sur le `Page` instance.

> [!NOTE]
> Il est important d’arrêter et supprimer les objets qui fournissent l’accès à l’appareil photo dans une application UWP. Cela peut interférer avec d’autres applications qui tentent d’accéder à photo l’appareil. Pour plus d’informations, consultez [afficher l’aperçu de la caméra](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur personnalisé pour le [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) page, permettant ainsi aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifique à la plateforme. Un `ContentPage` est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran.


## <a name="related-links"></a>Liens associés

- [CustomRendererContentPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
