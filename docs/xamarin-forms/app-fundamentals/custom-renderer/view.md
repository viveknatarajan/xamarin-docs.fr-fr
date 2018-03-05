---
title: "Implémentation d’une vue"
description: "Xamarin.Forms les contrôles d’interface utilisateur personnalisé doivent dériver de la classe de vue, qui est utilisée pour placer des mises en page et des contrôles sur l’écran. Cet article explique comment créer un convertisseur personnalisé pour un contrôle personnalisé Xamarin.Forms qui est utilisé pour afficher un flux vidéo de la version d’évaluation à partir photo de l’appareil."
ms.topic: article
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d5f9f86447886e2cea46a6317d05506cdbed90bb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="implementing-a-view"></a>Implémentation d’une vue

_Xamarin.Forms les contrôles d’interface utilisateur personnalisé doivent dériver de la classe de vue, qui est utilisée pour placer des mises en page et des contrôles sur l’écran. Cet article explique comment créer un convertisseur personnalisé pour un contrôle personnalisé Xamarin.Forms qui est utilisé pour afficher un flux vidéo de la version d’évaluation à partir photo de l’appareil._

Chaque vue Xamarin.Forms a un convertisseur qui l’accompagne pour chaque plateforme qui crée une instance d’un contrôle natif. Lorsqu’un [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) est restitué par une application Xamarin.Forms dans iOS, le `ViewRenderer` classe est instanciée, qui instancie ensuite natif `UIView` contrôle. Sur la plateforme Android, le `ViewRenderer` classe instancie natif `View` contrôle. Sur Windows Phone et la plateforme Windows universelle (UWP), le `ViewRenderer` classe instancie natif `FrameworkElement` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles de Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) et les contrôles natifs correspondants qui l’implémentent :

![](view-images/view-classes.png "Relation entre la classe d’affichage et son implémentation des Classes natives")

Le processus de rendu peut être utilisé pour implémenter les personnalisations spécifiques à une plateforme en créant un convertisseur personnalisé pour un [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sur chaque plateforme. Le processus pour cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Control) un contrôle personnalisé de Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Control) le contrôle personnalisé à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour le contrôle sur chaque plateforme.

Chaque élément présent nous reviendrons à son tour, pour implémenter un `CameraPreview` convertisseur qui affiche un flux vidéo de la version d’évaluation à partir photo de l’appareil. Appuyez sur le flux vidéo arrêter et le démarrer.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Création du contrôle personnalisé

Un contrôle personnalisé peut être créé en sous-classant le [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) de classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

Le `CameraPreview` contrôle personnalisé est créé dans le projet de bibliothèque (PCL) de classes portable et définit l’API pour le contrôle. Le contrôle personnalisé expose un `Camera` propriété qui est utilisée pour contrôler si le flux vidéo doit être affiché à partir de l’avant ou arrière appareil photo sur l’appareil. Si aucune valeur n’est pas spécifiée pour le `Camera` propriété lorsque le contrôle est créé, la valeur par défaut à la spécification de l’appareil photo arrière.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Utilisation du contrôle personnalisé

Le `CameraPreview` contrôle personnalisé qui peut être référencé dans XAML dans le projet de bibliothèque de classes portables par déclarer un espace de noms pour son emplacement et à l’aide du préfixe d’espace de noms sur l’élément de contrôle personnalisé. Le code suivant montre d’exemple comment la `CameraPreview` contrôle personnalisé peut être consommé par une page XAML :

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour référencer le contrôle personnalisé.

Le code suivant montre d’exemple comment la `CameraPreview` contrôle personnalisé peut être consommé par une page c# :

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Une instance de la `CameraPreview` contrôle personnalisé servira pour afficher le flux vidéo de la version d’évaluation à partir photo de l’appareil. À l’exception en spécifiant éventuellement une valeur pour le `Camera` propriété, la personnalisation du contrôle est effectuée dans le convertisseur personnalisé.

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour créer des contrôles de version d’évaluation spécifiques à une plateforme à la caméra.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `ViewRenderer<T1,T2>` classe qui restitue le contrôle personnalisé. Le premier argument de type doit être le contrôle personnalisé, le convertisseur est dans ce cas, `CameraPreview`. Le deuxième argument de type doit être le contrôle natif qui implémente le contrôle personnalisé.
1. Remplacer la `OnElementChanged` méthode qui restitue le contrôle et l’écriture une logique personnalisée pour le personnaliser. Cette méthode est appelée lorsque le contrôle de Xamarin.Forms correspondant est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour spécifier qu’il sera utilisé pour restituer le contrôle personnalisé Xamarin.Forms. Cet attribut est utilisé pour inscrire le convertisseur personnalisé avec Xamarin.Forms.

> [!NOTE]
> **Remarque**: pour la plupart des éléments de Xamarin.Forms, il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, puis le convertisseur par défaut pour la classe de base du contrôle sera utilisé. Toutefois, les convertisseurs personnalisés sont nécessaires dans chaque projet de plateforme lors du rendu d’un [vue](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) élément.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](view-images/solution-structure.png "Responsabilités de projet convertisseur CameraPreview personnalisé")

Le `CameraPreview` contrôle personnalisé est restitué par les classes de rendu spécifique à la plateforme, qui dérivent toutes de la `ViewRenderer` classe pour chaque plateforme. Ainsi, chaque `CameraPreview` contrôle personnalisé rendu avec des contrôles de plateforme spécifique, comme indiqué dans les captures d’écran suivants :

![](view-images/screenshots.png "CameraPreview sur chaque plateforme")

Le `ViewRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque le contrôle personnalisé Xamarin.Forms est créé pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre contienne `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms que le convertisseur *a été* associée et l’élément Xamarin.Forms que le convertisseur *est* attaché, respectivement. Dans l’exemple d’application, le `OldElement` propriété sera `null` et `NewElement` propriété contient une référence à la `CameraPreview` instance.

Une version substituée de la `OnElementChanged` , dans chaque classe de convertisseur de plateforme spécifique, est l’endroit pour effectuer l’instanciation des contrôles natifs et la personnalisation. Le `SetNativeControl` (méthode) doit être utilisée pour instancier le contrôle natif, et cette méthode attribue également la référence de contrôle pour le `Control` propriété. En outre, une référence au contrôle qui est rendue Xamarin.Forms peut être obtenue via le `Element` propriété.

Dans certains cas, le `OnElementChanged` méthode peut être appelée plusieurs fois. Par conséquent, pour éviter les fuites de mémoire, être vigilant lors de l’instanciation d’un nouveau contrôle natif. L’approche à utiliser lors de l’instanciation d’un nouveau contrôle natif dans un renderer personnalisé est indiquée dans l’exemple de code suivant :

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Un nouveau contrôle natif doit uniquement être instancié une seule fois, quand la propriété `Control` a la valeur `null`. Vous devez uniquement configurer le contrôle et vous abonner aux gestionnaires d’événements quand le renderer personnalisé est attaché à un nouvel élément Xamarin.Forms. De même, les gestionnaires d’événements auquel les clients ont été ne doivent être annulés lorsque le convertisseur est attaché à l’élément est modifié. Adoptant cette approche vous aidera à créer un convertisseur personnalisé performant qui ne connaît pas les fuites de mémoire.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle personnalisé Xamarin.Forms rendu et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes traitent de l’implémentation de chaque classe de convertisseur de personnalisé spécifique à la plateforme.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme iOS :

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Autant que la `Control` propriété est `null`, le `SetNativeControl` méthode est appelée pour instancier un nouvel `UICameraPreview` contrôle et d’attribuer une référence à celui-ci à le `Control` propriété. Le `UICameraPreview` contrôle est un contrôle personnalisé spécifique à la plateforme qui utilise le `AVCapture` API pour fournir un flux aperçu à partir de l’appareil photo. Il expose une `Tapped` événement qui est géré par le `OnCameraPreviewTapped` méthode pour arrêter et démarrer l’aperçu vidéo lorsqu’il est activé par un clic. Le `Tapped` est être abonné à l’événement lorsque le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms et désinscrit uniquement lorsque l’élément le convertisseur est attaché aux modifications.

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Android :

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Autant que la `Control` propriété est `null`, le `SetNativeControl` méthode est appelée pour instancier un nouvel `CameraPreview` contrôler et d’affecter une référence à celui-ci à le `Control` propriété. Le `CameraPreview` contrôle est un contrôle personnalisé spécifique à la plateforme qui utilise le `Camera` API pour fournir le flux d’aperçu de l’appareil photo. Le `CameraPreview` contrôle est ensuite configuré correctement, à condition que le convertisseur personnalisé est attaché à un nouvel élément de Xamarin.Forms. Cette configuration implique la création d’un nouveau natif `Camera` de l’objet pour accéder à un appareil photo de matériel spécifique et l’inscription d’un gestionnaire d’événements pour traiter les `Click` événement. À son tour ce gestionnaire arrête et démarre l’aperçu vidéo lorsqu’il est activé par un clic. Le `Click` événement est désinscrit de si l’élément Xamarin.Forms le convertisseur est associé aux modifications.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Création du convertisseur personnalisé sur Windows Phone et UWP

L’exemple de code suivant montre le convertisseur personnalisé pour Windows Phone et UWP :

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        MediaCapture mediaCapture;
        CaptureElement captureElement;
        CameraOptions cameraOptions;
        Application app;
        bool isPreviewing = false;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                ...
                captureElement = new CaptureElement ();
                captureElement.Stretch = Stretch.UniformToFill;

                InitializeAsync ();
                SetNativeControl (captureElement);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped (object sender, TappedRoutedEventArgs e)
        {
            if (isPreviewing) {
                await StopPreviewAsync ();
            } else {
                await StartPreviewAsync ();
            }
        }
        ...
    }
}
```

Autant que la `Control` propriété est `null`, un nouveau `CaptureElement` est instancié et `InitializeAsync` méthode est appelée, qui utilise le `MediaCapture` API pour fournir le flux d’aperçu de l’appareil photo. Le `SetNativeControl` méthode est alors appelée pour assigner une référence à la `CaptureElement` d’instance pour le `Control` propriété. Le `CaptureElement` contrôle expose un `Tapped` événement qui est géré par le `OnCameraPreviewTapped` méthode pour arrêter et démarrer l’aperçu vidéo lorsqu’il est activé par un clic. Le `Tapped` est être abonné à l’événement lorsque le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms et désinscrit uniquement lorsque l’élément le convertisseur est attaché aux modifications.

> [!NOTE]
> **Remarque**: il est important arrêter et supprimer les objets qui fournissent l’accès à l’appareil photo dans une application Windows Phone ou UWP. Cela peut interférer avec d’autres applications qui tentent d’accéder à photo l’appareil. Pour plus d’informations, consultez et [démarrage rapide : capture vidéo à l’aide de l’API MediaCapture](https://msdn.microsoft.com/library/windows/apps/xaml/dn642092.aspx) pour les applications Windows Runtime, et [afficher l’aperçu de l’appareil photo](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access) pour applications UWP.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur personnalisé pour un contrôle personnalisé Xamarin.Forms qui est utilisé pour afficher un flux vidéo de la version d’évaluation à partir photo de l’appareil. Xamarin.Forms les contrôles d’interface utilisateur personnalisé doivent dériver de la [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) (classe), qui est utilisé pour placer des mises en page et des contrôles sur l’écran.


## <a name="related-links"></a>Liens associés

- [CustomRendererView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
