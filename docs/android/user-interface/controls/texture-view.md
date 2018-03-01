---
title: TextureView
ms.topic: article
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: 7048962a93f5bd99f4a27062ecc6cc2d5b2d3398
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="textureview"></a>TextureView

La `TextureView` classe est une vue qui utilise le rendu à accélération matérielle 2D pour activer une vidéo ou le flux de contenu OpenGL à afficher. Par exemple, la capture d’écran suivante montre le `TextureView` affichant un flux en direct à partir photo de l’appareil :

[![Capture d’écran de l’exemple d’une image en direct à partir photo de l’appareil](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png)

Contrairement à la `SurfaceView` (classe), qui peut également être utilisé pour afficher des OpenGL ou contenu vidéo, le TextureView n’est pas rendu dans une fenêtre distincte.
Par conséquent, `TextureView` est en mesure de prendre en charge les transformations de vue comme toute autre vue. Par exemple, faire pivoter un `TextureView` peut être effectuée en définissant simplement son `Rotation` propriété, sa transparence en définissant son `Alpha` propriété et ainsi de suite.

Par conséquent, avec la `TextureView` nous pouvons maintenant effectuer les opérations afficher un flux en direct à partir de l’appareil photo et la transformer, comme indiqué dans le code suivant :

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Le code ci-dessus crée un `TextureView` instance dans l’activité `OnCreate` (méthode) et définit l’activité en tant que le `TextureView`de `SurfaceTextureListener`. Soit le `SurfaceTextureListener`, l’activité implémente la `TextureView.ISurfaceTextureListener` interface. Le système appelle le `OnSurfaceTextAvailable` méthode lorsque le `SurfaceTexture` est prêt à être utilisé. Dans cette méthode, nous prenons la `SurfaceTexture` qui est transmise et affectez-lui la valeur texture d’aperçu de la caméra. Ensuite, nous libres d’effectuer des opérations normales basée sur une vue, telles que la définition du `Rotation` et `Alpha`, comme dans l’exemple ci-dessus. L’application résultante, en cours d’exécution sur un appareil, est indiquée ci-dessous :

[![Exemple de l’application en cours d’exécution sur un appareil, afficher une image](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png)

Pour utiliser le `TextureView`, l’accélération matérielle doit être activée, il sera par défaut à partir de l’API de niveau 14. Également, dans la mesure où cet exemple utilise l’appareil photo, à la fois le `android.permission.CAMERA` autorisation et la `android.hardware.camera` fonctionnalité doit être définie dans le **AndroidManifest.xml**.



## <a name="related-links"></a>Liens associés

- [TextureViewDemo (sample)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Présentation de glace Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plateforme 4.0 Android](http://developer.android.com/sdk/android-4.0.html)
