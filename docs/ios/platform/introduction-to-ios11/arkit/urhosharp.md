---
title: À l’aide de ARKit avec UrhoSharp dans Xamarin.iOS
description: Ce document décrit comment configurer une application ARKit dans Xamarin.iOS, puis examine le mode de rendu des cadres, comment ajuster l’appareil photo, comment détecter des plans, comment travailler avec éclairage et bien plus encore. Il aborde également UrhoSharp et écrire du code pour HoloLens.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/01/2017
ms.openlocfilehash: b02ecc8a40f6ff8a1862d50202439d369003a53d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61262434"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>À l’aide de ARKit avec UrhoSharp dans Xamarin.iOS

Avec l’introduction de [ARKit](https://developer.apple.com/arkit/), Apple a permis aux développeurs de créer des applications de réalité augmentée. ARKit peut effectuer le suivi de la position exacte de votre appareil et détecter les différentes surfaces dans le monde, et il revient ensuite au développeur de fusionner les données provenant de ARKit dans votre code.

[UrhoSharp](~/graphics-games/urhosharp/index.md) fournit une API 3D complète et facile à utiliser qui vous permettent de créer des applications 3D.   Ces deux éléments peuvent être fusionnés entre eux, ARKit pour fournir les informations physiques sur le monde et Urho pour afficher les résultats.

Cette page explique comment connecter ces deux mondes afin de créer des applications de réalité augmentée excellent.


## <a name="the-basics"></a>Principes de base

Nous voulons est présentent du contenu 3D par-dessus le monde entier par l’iPhone/iPad.   L’idée consiste à fusionner le contenu en provenance de photo l’appareil avec le contenu 3D et que l’utilisateur de l’appareil ne se déplace la salle pour vous assurer que l’objet 3D se comportent comme elles fait partie de cet espace - cela par ancrage les objets dans ce monde.

![Figure animée dans ARKit](urhosharp-images/image1.gif)


Nous allons utiliser la bibliothèque Urho pour charger nos composants 3D et les placer dans le monde, et nous allons utiliser ARKit pour obtenir le flux vidéo en provenance de l’appareil photo, ainsi que l’emplacement du téléphone dans le monde.   Quand l’utilisateur se déplace avec son téléphone, nous allons utiliser les modifications dans l’emplacement pour mettre à jour le système de coordonnées qui affiche le moteur Urho.

De cette façon, lorsque vous placez un objet dans l’espace 3D et de l’utilisateur se déplace, l’emplacement de l’objet 3D reflète le lieu et l’emplacement où il a été placé.

## <a name="setting-up-your-application"></a>La configuration de votre application

### <a name="ios-application-launch"></a>iOS lancement d’Application

Votre application iOS a besoin créer et lancer votre contenu 3D, pour cela, vous devez créer une implémentation d’une sous-classe de la [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) et fournir votre code de programme d’installation en substituant le `Start` (méthode).  Il s’agit où votre scène est remplie avec des données, l’événement gestionnaires sont le programme d’installation et ainsi de suite.

Nous avons introduit un `Urho.ArkitApp` classe qui sous-classe `Urho.Application` et sur son `Start` méthode fait l’essentiel.   Il vous suffit pour votre application est de Urho existant Modifier la classe de base pour être de type `Urho.ArkitApp` et vous disposez d’une application qui exécutera votre scène urho dans le monde.

### <a name="the-arkitapp-class"></a>La classe ArkitApp

Cette classe fournit un ensemble de modèles par défaut pratiques, les deux une scène avec certains objets clés, ainsi que le traitement des événements de ARKit qu’ils sont remis par le système d’exploitation.

Le programme d’installation a lieu le `Start` méthode virtuelle.   Lorsque vous substituez cette méthode dans votre sous-classe, vous devez vous assurer à la chaîne à votre parent à l’aide de `base.Start()` sur votre propre implémentation.

Le `Start` méthode configure de la scène, fenêtre d’affichage, appareil photo et une lumière directionnelle et l’expose en tant que propriétés publiques :

- un [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) pour stocker vos objets,
- un directionnelles [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) avec les ombres et dont l’emplacement est disponible via le `LightNode` propriété
- un [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) dont les composants sont mis à jour lorsque ARKit offre une mise à jour à l’application et
- un [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) affichage des résultats.


### <a name="your-code"></a>Votre code

Vous devez ensuite sous-classe la `ArkitApp` classe et substituer les `Start` (méthode).   La première chose que votre méthode doit faire est de chaîne jusqu'à la `ArkitApp.Start` en appelant `base.Start()`.  Après cela, vous pouvez utiliser un de l’installation de propriétés par ArkitApp pour ajouter vos objets à la scène, personnaliser les lumières, les ombres ou les événements que vous souhaitez gérer.

L’exemple ARKit/UrhoSharp charge un caractère animé avec des textures et lit l’animation, avec l’implémentation suivante :

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

Et c’est vraiment tout ce que vous avez à faire à ce stade pour que votre contenu 3D affiché en réalité augmentée.

Urho utilise des formats personnalisés pour des modèles 3D et les animations, vous devez exporter vos ressources dans ce format.   Vous pouvez utiliser des outils tels que le [Urho3D Blender Add-in](https://github.com/reattiva/Urho3D-Blender) et [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) pouvant convertir ces ressources à partir des formats courants tels que DBX, DAE, OBJ, Blend, 3D-Max dans le format requis par Urho.

Pour en savoir plus sur la création d’applications 3D à l’aide de Urho, visitez le [présentation UrhoSharp](~/graphics-games/urhosharp/introduction.md) guide.

## <a name="arkitapp-in-depth"></a>ArkitApp en profondeur

> [!NOTE]
> Cette section est destinée aux développeurs qui souhaitent personnaliser l’expérience par défaut de UrhoSharp et ARKit ou pour obtenir une meilleure idée sur le fonctionne de l’intégration.   Il n’est pas nécessaire de lire cette section.

L’API ARKit est assez simple, vous créez et configurez un [ARSession](https://developer.apple.com/documentation/arkit/arsession) de l’objet qui, puis commencez à livrer [ARFrame](https://developer.apple.com/documentation/arkit/arframe) objets.   Ils contiennent à la fois l’image capturée par l’appareil photo, ainsi que la position réelle estimée de l’appareil.

Nous être composer les images fournies par l’appareil photo nous avec notre contenu 3D et ajuster l’appareil photo dans UrhoSharp pour faire correspondre les risques dans l’emplacement de l’appareil et la position.

Le diagramme suivant illustre ce qui se déroule le `ArkitApp` classe :

[![Diagramme de classes et d’écrans dans le ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Les images de rendu

Le concept est simple, combiner la vidéo provenant de l’appareil photo avec notre graphique 3D pour produire l’image combinée.     Nous obtiendra une série de ces images capturées dans la séquence, et nous sera associer cette entrée avec la scène Urho.

La façon la plus simple pour cela consiste à insérer un [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) dans la main [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Il s’agit d’un ensemble de commandes qui sont effectuées pour dessiner une image unique.  Cette commande remplira la fenêtre d’affichage avec une texture que nous lui passer.    A été paramétré sur la première image qui est le processus et la définition réelle s’effectue dans th **ARRenderPath.xml** fichier est chargé à ce stade.

Toutefois, se posent à mélanger ces deux mondes deux problèmes :


1. Sur iOS, les Textures de GPU doit avoir une résolution est une puissance de deux, mais les images qui seront obtenus à partir de l’appareil photo ne disposant pas de résolution qui sont une puissance de deux, par exemple : 1280 x 720.
2. Les images sont encodés sous [YUV](https://en.wikipedia.org/wiki/YUV) format, représenté par deux images - luminance et chrominance.

Les trames YUV se présentent sous deux différentes résolutions.  une image de 1280 x 720 représentant beaucoup plus petits 640 x 360 pour le composant de chrominance et la luminance (essentiellement une image de nuances de gris) :

![Image illustrant combinant Y et les composants UV](urhosharp-images/image3.png)


Pour dessiner une image complète de couleur à l’aide d’OpenGL ES, nous devons écrire un nuanceur de petites qui prend la luminance (composant Y) et chrominance (UV plans) à partir d’emplacements de la texture.  Dans UrhoSharp qu’ils ont des noms - « sDiffMap » et « sNormalMap » et les convertissent au format RVB :

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

Pour afficher la texture qui n’a pas une puissance de résolution de deux, nous devons définir Texture2D avec les paramètres suivants :

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Par conséquent, nous ne pouvons restituer des images capturées en tant qu’arrière-plan et restituer des mutantes ça effrayant de toutes les scènes au-dessus de lui.

### <a name="adjusting-the-camera"></a>Réglage de l’appareil photo

Le `ARFrame` objets contiennent également la position de l’appareil estimé.  Nous prenons maintenant nous devons déplacer jeu caméra ARFrame - avant ARKit il n’était pas un hologrammes très important d’orientation de l’appareil track (rouleau, pitch et lacet) et rendu un hologramme épinglé sur la vidéo - mais si vous déplacez un peu votre appareil - dérive.

Cela se produit parce que les capteurs intégrés tels que gyroscope ne sont pas en mesure d’effectuer le suivi des mouvements, ils peuvent uniquement l’accélération.  Analyses ARKit chaque fonctionnalité de frame et extraits de pointe pour effectuer le suivi et est donc en mesure de nous donner une liste précise une matrice contenant des données de mouvement et la rotation de transformation.

Il s’agit par exemple, comment nous pouvons obtenir la position actuelle :

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Nous utilisons `-row.Z` parce que ARKit utilise un système de coordonnées droitier.


### <a name="plane-detection"></a>Détection de plan

ARKit est en mesure de détecter des plans horizontaux et cette capacité vous permet d’interagir avec le monde réel, par exemple, nous pouvons placer le mutantes sur une table réelle ou un étage. Pour ce faire, le plus simple consiste à utiliser la méthode HitTest (raycasting). Il convertit les coordonnées d’écran (0,5, 0,5 correspond au centre) dans les coordonnées de monde réel (0 ; 0 ; 0 est l’emplacement du premier frame).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

Nous pouvons maintenant placer le mutantes sur une surface horizontale selon leur emplacement sur l’écran d’appareil que nous appuyez sur :

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![Figure animée modification des plans en tant que vue déplacements](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Éclairage réaliste

Selon les conditions d’éclairage du monde réel, la scène virtuelle doit être plus clair ou sombre pour mieux correspondre à son environnement. ARFrame contient une propriété LightEstimate que nous pouvons utiliser pour ajuster la lumière ambiante Urho, cette opération s’effectue comme suit :

```csharp
var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
var zone = Scene.GetComponent<Zone>();
zone.AmbientColor = Color.White * ambientIntensity;
```

### <a name="beyond-ios---hololens"></a>Au-delà d’iOS - HoloLens

UrhoSharp [s’exécute sur tous les principaux systèmes d’exploitation](~/graphics-games/urhosharp/platform/index.md), de sorte que vous pouvez réutiliser votre code existant dans un autre emplacement.

HoloLens est une des plateformes plus intéressant et sur qu'elle s’exécute.   Cela signifie que vous pouvez facilement basculer entre iOS et HoloLens pour créer des applications de réalité augmentée awesome utilisation de UrhoSharp.

Vous pouvez trouver la source de MutantDemo à [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Liens associés

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (avec UrhoSharp) (exemple)](https://github.com/EgorBo/ARKitXamarinDemo)
