---
title: Personnalisation d’un ContentPage
description: Un ContentPage est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran. Cet article explique comment créer un convertisseur personnalisé pour la page ContentPage, permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0f4de4594e8abb8d0ee03690e5829193c51a3736
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-contentpage"></a>Personnalisation d’un ContentPage

_Un ContentPage est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran. Cet article explique comment créer un convertisseur personnalisé pour la page ContentPage, permettant aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme._

Chaque contrôle Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Lorsqu’un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) est restitué par une application de Xamarin.Forms, dans iOS le `PageRenderer` classe est instanciée, qui instancie ensuite natif `UIViewController` contrôle. Sur la plateforme Android, le `PageRenderer` classe instancie un `ViewGroup` contrôle. Sur la plate-forme de Windows universelle (UWP), le `PageRenderer` classe instancie un `FrameworkElement` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles de Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) et les contrôles natifs correspondants qui l’implémentent :

![](contentpage-images/contentpage-classes.png "Relation entre ContentPage classe et l’implémentation des contrôles natifs")

Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à une plateforme en créant un convertisseur personnalisé pour un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) sur chaque plateforme. Le processus pour cette opération est la suivante :

1. [Créer](#Creating_the_Xamarin.Forms_Page) une page de Xamarin.Forms.
1. [Consommer](#Consuming_the_Xamarin.Forms_Page) la page à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Page_Renderer_on_each_Platform) le convertisseur personnalisé pour la page sur chaque plateforme.

Chaque élément présent nous reviendrons à son tour, pour implémenter un `CameraPage` qui fournit une caméra en flux direct et permet de capturer une photo.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Création de la Page de Xamarin.Forms

Un non modifiés [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) peuvent être ajoutés au projet partagé Xamarin.Forms, comme indiqué dans l’exemple de code XAML suivant :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

De même, le fichier code-behind pour la [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) doit également rester inchangée, comme indiqué dans l’exemple de code suivant :

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

Une instance de la `CameraPage` servira à afficher la caméra en flux sur chaque plateforme. Personnalisation du contrôle est effectuée dans le convertisseur personnalisé, aucune implémentation supplémentaire est requis dans le `CameraPage` classe.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Utilisation de la Page de Xamarin.Forms

Vide `CameraPage` doit être affiché par l’application de Xamarin.Forms. Cela se produit lorsqu’un bouton sur le `MainPage` instance drainée, qui exécute à son tour le `OnTakePhotoButtonClicked` méthode, comme indiqué dans l’exemple de code suivant :

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Ce code renvoie simplement à la `CameraPage`, sur les convertisseurs personnalisés doit personnaliser l’apparence de la page sur chaque plateforme.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Création du convertisseur de Page sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `PageRenderer` classe.
1. Remplacer la `OnElementChanged` méthode qui restitue la logique de page et d’écriture native pour personnaliser la page. Le `OnElementChanged` méthode est appelée lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur de page pour spécifier qu’il sera utilisé pour restituer la page Xamarin.Forms. Cet attribut est utilisé pour inscrire le convertisseur personnalisé avec Xamarin.Forms.

> [!NOTE]
> Il est facultatif pour fournir un convertisseur de page dans chaque projet de plateforme. Si un convertisseur de page n’est pas inscrit, le convertisseur par défaut de la page d’être utilisé.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que la relation entre eux :

![](contentpage-images/solution-structure.png "Responsabilités de projet convertisseur CameraPage personnalisé")

Le `CameraPage` instance est restituée par spécifique à la plateforme `CameraPageRenderer` classes qui dérivent de la `PageRenderer` classe pour cette plateforme. Ainsi, chaque `CameraPage` instance rendue avec un flux de la caméra en direct, comme indiqué dans les captures d’écran suivants :

![](contentpage-images/screenshots.png "CameraPage sur chaque plateforme")

Le `PageRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lors de la création de la page de Xamarin.Forms pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms que le convertisseur *a été* associée et l’élément Xamarin.Forms que le convertisseur *est* attaché, respectivement. Dans l’exemple d’application le `OldElement` propriété sera `null` et `NewElement` propriété contient une référence à la `CameraPage` instance.

Une version substituée de la `OnElementChanged` méthode dans la `CameraPageRenderer` classe est l’endroit où pour effectuer la personnalisation de la page natif. Une référence à l’instance de page Xamarin.Forms qui est rendue peut être obtenue via le `Element` propriété.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type de la page Xamarin.Forms rendue et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes traitent de l’implémentation de la `CameraPageRenderer` un convertisseur personnalisé pour chaque plateforme.

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
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un iOS `UIViewController` contrôle. Le flux de la caméra en direct est uniquement affiché à condition que le convertisseur n’est pas déjà attaché à un élément existant de Xamarin.Forms et la condition qu’il existe une instance de la page qui est restitué par le convertisseur personnalisé.

La page personnalisée puis par une série de méthodes qui utilisent la `AVCapture` API pour fournir un flux dynamique à partir de l’appareil photo et permet de capturer une photo.

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
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

L’appel à la classe de base `OnElementChanged` méthode instancie un Android `ViewGroup` contrôle, qui est un groupe de vues. Le flux de la caméra en direct est uniquement affiché à condition que le convertisseur n’est pas déjà attaché à un élément existant de Xamarin.Forms et la condition qu’il existe une instance de la page qui est restitué par le convertisseur personnalisé.

Personnalisé en appelant une série de méthodes qui utilisent la page de la `Camera` API pour fournir le flux live à partir de l’appareil photo et permet de capturer une photo, avant le `AddView` méthode est appelée pour ajouter la caméra en direct l’interface utilisateur pour diffuser en continu le `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>Création du convertisseur de Page sur la plateforme Windows universelle

L’exemple de code suivant montre le convertisseur de page pour la plateforme Windows universelle :

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

L’appel à la classe de base `OnElementChanged` méthode instancie un `FrameworkElement` (contrôle), sur lequel la page est rendue. Le flux de la caméra en direct est uniquement affiché à condition que le convertisseur n’est pas déjà attaché à un élément existant de Xamarin.Forms et la condition qu’il existe une instance de la page qui est restitué par le convertisseur personnalisé. La page personnalisée puis en appelant une série de méthodes qui utilisent la `MediaCapture` API pour fournir le flux live à partir de l’appareil photo et permet de capturer une photo avant que la page personnalisée est ajoutée à la `Children` collection pour l’affichage.

Lorsque vous implémentez un convertisseur personnalisé qui dérive de `PageRenderer` sur la plateforme Windows universelle, le `ArrangeOverride` méthode doit également être implémentée pour organiser les contrôles de la page, étant donné que le convertisseur de base ne sait pas comment procéder avec eux. Sinon, il en résulte une page vierge. Par conséquent, dans cet exemple la `ArrangeOverride` les appels de méthode le `Arrange` méthode sur le `Page` instance.

> [!NOTE]
> Il est important arrêter et supprimer les objets qui fournissent l’accès à l’appareil photo dans une application UWP. Cela peut interférer avec d’autres applications qui tentent d’accéder à photo l’appareil. Pour plus d’informations, consultez [afficher l’aperçu de l’appareil photo](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur personnalisé pour le [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) page, permettant ainsi aux développeurs de substituer le rendu natif par défaut avec leur propres personnalisation spécifiques à la plateforme. A `ContentPage` est un élément visuel qui affiche une vue unique et occupe la majeure partie de l’écran.


## <a name="related-links"></a>Liens associés

- [CustomRendererContentPage (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
