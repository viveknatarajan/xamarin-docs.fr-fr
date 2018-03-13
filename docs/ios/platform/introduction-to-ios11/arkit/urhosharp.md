---
title: "À l’aide de ARKit avec UrhoSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 94963e92f8372316a982bbe38f1fb653d38b2a3b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="using-arkit-with-urhosharp"></a>À l’aide de ARKit avec UrhoSharp

Avec l’introduction de [ARKit](https://developer.apple.com/arkit/), Apple a apportées aux développeurs de créer des applications de réalité augmentée. ARKit peut effectuer le suivi de la position exacte de votre appareil et détecter des surfaces différents dans le monde, et il revient ensuite au développeur de fusionner les données issues des ARKit dans votre code.

[UrhoSharp](~/graphics-games/urhosharp/index.md) fournit une API 3D complète et facile à utiliser qui vous permet de créer des applications 3D.   Ces deux éléments peuvent être fusionnés entre eux, ARKit pour fournir les informations physiques sur le monde et Urho pour afficher les résultats.

Cette page explique comment se connecter à ces deux mondes afin de créer des applications de grande réalité augmentée.


## <a name="the-basics"></a>Principes de base

Le résultat escompté est présent contenu 3D par-dessus le monde par l’iPhone.   L’idée est pour fusionner le contenu provenant de la caméra du téléphone avec le contenu 3D et lorsque l’utilisateur du téléphone déplace autour de l’espace pour vous assurer que l’objet 3D se comportent comme elles fait partie de cet espace - il suffit d’ancrage des objets dans ce monde.

![Figure animée dans ARKit](urhosharp-images/image1.gif)


Nous allons utiliser la bibliothèque Urho pour charger les ressources 3D et de les placer dans le monde, et nous allons utiliser ARKit pour obtenir le flux vidéo en provenance de l’appareil photo, ainsi que l’emplacement du téléphone dans le monde.   Lorsque l’utilisateur déplace avec son téléphone, nous allons utiliser les modifications dans l’emplacement pour mettre à jour le système de coordonnées qui affiche le moteur Urho.

Ainsi, lorsque vous placez un objet dans l’espace 3D et de l’utilisateur se déplace, l’emplacement de l’objet 3D reflète le lieu et l’emplacement où il a été placé.

## <a name="setting-up-your-application"></a>La configuration de votre application

### <a name="ios-application-launch"></a>e/s de l’Application de lancement

Votre application iOS a besoin créer et lancer votre contenu 3D, pour ce faire, vous créez une implémentation d’une sous-classe de la [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) et fournir votre code de programme d’installation en substituant le `Start` (méthode).  Il s’agit où votre scène est remplie avec des données, l’événement gestionnaires sont le programme d’installation et ainsi de suite.

Nous avons introduit un `Urho.ArkitApp` classe qui sous-classe `Urho.Application` et sur son `Start` méthode fait l’essentiel.   Vous devez faire votre Urho existant est de l’application de modifier la classe de base pour être de type `Urho.ArkitApp` et vous avez une application qui exécutera votre scène urho dans le monde.

### <a name="the-arkitapp-class"></a>La classe ArkitApp

Cette classe fournit un ensemble de valeurs par défaut pratiques, à la fois une scène avec certains objets clés, ainsi que le traitement des événements de ARKit qu’ils sont remis par le système d’exploitation.

Le programme d’installation a lieu le `Start` méthode virtuelle.   Lorsque vous substituez cette méthode dans votre sous-classe, vous devez vous assurer de chaîne à votre parent à l’aide de `base.Start()` sur votre propre implémentation.

Le `Start` méthode configure la scène, la fenêtre d’affichage, appareil photo et une lumière directionnelle et met en évidence ceux en tant que propriétés publiques :

- un [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) pour stocker vos objets,
- un directionnelle [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) avec ombres et dont l’emplacement est disponible via le `LightNode` propriété
- un [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) dont les composants sont mis à jour lorsque ARKit offre une mise à jour à l’application et
- un [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) affichage des résultats.


### <a name="your-code"></a>Votre code

Vous devez ensuite sous-classe le `ArkitApp` classe et substituer la `Start` (méthode).   La première chose que votre méthode doit faire est de chaîne jusqu'à la `ArkitApp.Start` en appelant `base.Start()`.  Après cela, vous pouvez utiliser un de l’installation de propriétés par ArkitApp pour ajouter vos objets à la scène, personnaliser les témoins lumineux, les ombres ou les événements que vous souhaitez gérer.

L’exemple de ARKit/UrhoSharp charge un caractère animé avec des textures et lit l’animation, avec l’implémentation suivante :

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

Et c’est vraiment que vous avez à faire à ce stade pour que votre contenu 3D affiché en réalité augmentée.

Urho utilise des formats personnalisés pour les modèles 3D et les animations, vous devez exporter vos éléments multimédias dans ce format.   Vous pouvez utiliser des outils comme le [Urho3D Blender Add-in](https://github.com/reattiva/Urho3D-Blender) et [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) qui peut convertir ces ressources à partir des formats courants tels que DBX, DAE, OBJ, Blend, max. 3D dans le format requis par Urho.

Pour en savoir plus sur la création d’applications 3D à l’aide de Urho, visitez le [présentation UrhoSharp](~/graphics-games/urhosharp/introduction.md) guide.

## <a name="arkitapp-in-depth"></a>ArkitApp en profondeur

> [!NOTE]
> Cette section s’adresse aux développeurs qui souhaitent personnaliser l’expérience par défaut de UrhoSharp et ARKit ou pour obtenir une meilleure idée sur le fonctionne de l’intégration.   Il n’est pas nécessaire de lire cette section.

L’API ARKit est très simple, vous créez et configurez un [ARSession](https://developer.apple.com/documentation/arkit/arsession) de l’objet qui ensuite commencer à remettre [ARFrame](https://developer.apple.com/documentation/arkit/arframe) objets.   Ces plans contiennent à la fois l’image capturée par l’appareil photo, ainsi que la position réelle estimée de l’appareil.

Nous être composer les images fournies par l’appareil photo nous avec notre contenu 3D et ajuster l’appareil photo dans UrhoSharp pour faire correspondre les risques dans l’emplacement de l’appareil et la position.

Le diagramme suivant illustre ce qui se déroule le `ArkitApp` classe :

[![Diagramme de classes et d’écrans dans le ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Les images de rendu

Le concept est simple, en combinant la vidéo issues de l’appareil photo avec notre graphique 3D pour produire l’image combiné.     Nous obtiendra une série de ces images capturées dans la séquence, et nous sera associer cette entrée avec la scène Urho.

Pour ce faire, la plus simple consiste à insérer un [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) dans le principal [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Il s’agit d’un ensemble de commandes qui sont effectuées pour dessiner une image.  Cette commande remplit la fenêtre d’affichage avec une texture que nous lui passer.    Nous avons définis sur la première image qui est le processus, et la définition réelle est effectuée dans th **ARRenderPath.xml** fichier qui est chargé à ce stade.

Toutefois, nous posent deux problèmes mélange de ces deux mondes :


1. Sur iOS, les Textures GPU doit avoir une résolution correspondant à une puissance de deux, mais les images que vous pouvez obtenir à partir de l’appareil photo n’ont pas de résolution sont une puissance de deux, par exemple : 1280 x 720.
2. Les images sont encodés dans [YUV](https://en.wikipedia.org/wiki/YUV) format, représenté par deux images - luminance et couleur.

Les frames YUV se présentent sous deux des résolutions différentes.  une image de 1280 x 720 représentant luminance (en fait une image de nuances de gris) et beaucoup plus petite 640 x 360 pour le composant chrominance :

![Image montrant la combinaison Y et les composants UV](urhosharp-images/image3.png)


Pour dessiner une image complète de couleur à l’aide d’OpenGL ES, il faut écrire un nuanceur de petits prenant luminance (composant Y) et chrominance (UV plans) à partir d’emplacements de la texture.  Dans UrhoSharp, ils ont des noms - « sDiffMap » et « sNormalMap » et les convertir en format RVB :

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

Pour restituer la texture qui n’a pas une puissance de deux résolution, nous devons définir Texture2D avec les paramètres suivants :

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Ainsi, il est possible rendre les images capturées en tant qu’arrière-plan et restituer des mutant inquiétant comme cela de toutes les scènes au-dessus de lui.

### <a name="adjusting-the-camera"></a>Ajustement de l’appareil photo

Le `ARFrame` les objets contiennent également la position de l’appareil estimé.  Nous avons maintenant nous devons déplacer jeu caméra ARFrame - avant ARKit il n’était pas un hologrammes belle affaire à suivre orientation du périphérique (restauration, tangage et du lacet) et rendu un hologramme épinglé au-dessus de la vidéo -, mais si vous déplacez un peu votre appareil - dérive.

Cela se produit parce que les capteurs intégrés tels que gyroscope ne sont pas en mesure de suivre des mouvements, ils peuvent uniquement l’accélération.  Analyses ARKit chaque fonctionnalité de frame et extraits pointe pour effectuer le suivi et est donc en mesure de proposer un transforment la matrice de rotation et déplacement des données.

Il s’agit par exemple, comment nous pouvons obtenir la position actuelle :

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Nous utilisons `-row.Z` car ARKit utilise un système de coordonnées droitier.


### <a name="plane-detection"></a>Détection de plan

ARKit est en mesure de détecter des plans horizontaux et cette capacité vous permet d’interagir avec le monde réel, par exemple, nous pouvons transmettre le mutant sur une table réelle ou un étage. Pour ce faire, le plus simple consiste à utiliser HitTest (méthode) (raycasting). Il convertit les coordonnées d’écran (0,5, 0,5 est le centre) dans les coordonnées de monde réel (0, 0 ; 0 est l’emplacement de l’image de la première).

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

maintenant, nous pouvons transmettre le mutant sur une surface horizontale en fonction de l’emplacement dans l’écran d’appareil que nous appuyez sur :

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

![Animée figure la modification des plans en tant que vue déplace](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Éclairage réaliste

Selon les conditions d’éclairage du monde réel, la scène virtuelle doit être plus clair ou sombre pour mieux correspondre à son environnement. ARFrame contient une propriété LightEstimate que nous pouvons utiliser pour ajuster la lumière ambiante Urho, cette opération s’effectue comme suit :


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Au-delà d’iOS - HoloLens

UrhoSharp [s’exécute sur tous les principaux systèmes d’exploitation](~/graphics-games/urhosharp/platform/index.md), vous pouvez réutiliser votre code existant ailleurs.

HoloLens est une des plateformes plus intéressantes, qu'il s’exécute.   Cela signifie que vous pouvez basculer facilement entre iOS et HoloLens pour créer des applications augmentée la réalité impressionnant à l’aide de UrhoSharp.

Vous pouvez trouver la source de MutantDemo à [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Liens associés

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (avec UrhoSharp) (exemple)](https://github.com/EgorBo/ARKitXamarinDemo)
