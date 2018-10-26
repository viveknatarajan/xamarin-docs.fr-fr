---
title: Graphismes et Animation
description: Android fournit une infrastructure extrêmement riche et divers prenant en charge des graphiques 2D et des animations. Cette rubrique présente ces infrastructures et explique comment créer des graphiques personnalisés et les animations pour une utilisation dans une application Xamarin.Android.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: c49b8855bccaf2eca825096746769d7f201736c5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116886"
---
# <a name="graphics-and-animation"></a>Graphismes et Animation

_Android fournit une infrastructure extrêmement riche et divers prenant en charge des graphiques 2D et des animations. Cette rubrique présente ces infrastructures et explique comment créer des graphiques personnalisés et les animations pour une utilisation dans une application Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

En dépit d’en cours d’exécution sur les appareils qui sont traditionnellement de puissance limitée, les applications mobiles nominales la plus élevées ont souvent un sophistiquées utilisateur expérience (UX), avec des graphiques de haute qualité et animations qui fournissent une apparence intuitive, réactive et dynamique. Comme les applications mobiles Tirez un meilleur et plus sophistiquées, les utilisateurs ont commencé à attendre de plus en plus d’applications.

Heureusement pour nous, les plateformes mobiles modernes ont des infrastructures très puissants pour la création d’animations sophistiquées et des graphiques personnalisés tout en conservant la simplicité d’utilisation. Cela permet aux développeurs d’ajouter une interactivité riche avec très peu d’efforts.

Infrastructures d’API d’interface utilisateur Android à peu près peuvent être fractionnées en deux catégories : graphismes et Animation.

Graphiques sont encore divisées en différentes approches permettant d’effectuer des graphiques 2D et 3D. Graphiques 3D sont disponibles via un nombre d’infrastructures comme OpenGL ES (version mobile de OpenGL spécifique) et d’infrastructures tierces telles que MonoGame (un toolkit inter-plateformes compatible avec le Kit de ressources XNA). Bien que les graphismes 3D ne sont pas dans la portée de cet article, nous allons examiner les techniques de dessins 2D intégrés.

Android fournit deux API différentes pour créer des graphiques 2D. Un est une approche déclarative de haut niveau et l’autre une API de bas niveau par programmation :

-   **Ressources drawable** &ndash; ceux-ci servent à créer des graphiques personnalisés par programmation ou (plus généralement) en incorporant des instructions de dessin dans des fichiers XML. Ressources drawable sont généralement définies en tant que fichiers XML qui contiennent des instructions ou des actions pour Android restituer un graphique 2D. 

-   **Zone de dessin** &ndash; il s’agit d’une API de bas niveau qui implique le dessin directement sur une bitmap sous-jacent. Il fournit un contrôle précis sur ce qui est affiché. 

En plus de ces techniques graphiques 2D, Android fournit également les différentes façons de créer des animations :

-   **Animations drawable** &ndash; Android prend également en charge des animations image par image connu sous le nom *Animation Drawable*. Il s’agit de l’API d’animation la plus simple. Android charge séquentiellement et affiche les ressources Drawable dans la séquence (un peu comme un dessin animé).

-   **Afficher des Animations** &ndash; *vue Animations* sont l’animation d’origine de l’API dans Android et sont disponibles dans toutes les versions d’Android. Cette API est limitée, car elle fonctionne qu’avec les objets de vue et que vous ne pouvez effectuer des transformations simples sur ces vues.
    Animations de vue sont généralement définies dans les fichiers XML se trouvent dans le `/Resources/anim` dossier.

-   **Animations de propriété** &ndash; Android 3.0 introduit un nouvel ensemble d’animation de l’API appelée *des Animations de propriété*. Ces nouvelle API introduit un système extensible et flexible qui peut être utilisé pour animer les propriétés de n’importe quel objet, non seulement d’afficher les objets. Cette flexibilité permet des animations d’être encapsulées dans des classes distinctes qui rend plus facile de partage de code.


Animations de vue sont plus adaptées pour les applications qui doivent prendre en charge les anciens Android avant 3.0 de l’API (API niveau 11). Sinon, les applications doivent utiliser la plus récente propriété API d’Animation pour les raisons citées ci-dessus.

Toutes ces infrastructures sont viables toutefois lorsque cela est possible, préférence doit être accordée aux Animations de propriété, car il s’agit d’une API plus flexible permettant de travailler avec des options. Autorisent les Animations de propriété pour la logique d’animation d’être encapsulées dans des classes distinctes qui rend le partage de code et simplifie la maintenance du code.


## <a name="accessibility"></a>Accessibilité

Graphismes et animations permettant de rendre les applications Android attrayant et plus agréables à utiliser ; Toutefois, il est important de se rappeler que certaines interactions ont lieu via les lecteurs, les autres périphériques d’entrée, ou avec zoom assisté.
En outre, certaines interactions peuvent se produire sans les fonctionnalités audio.

Les applications sont plus utilisables dans ces situations si elles ont été conçues avec l’accessibilité à l’esprit : fournissant des conseils et assistance de navigation dans l’interface utilisateur et en vérifiant qu’il agit contenu de texte ou les descriptions pour les éléments de représentation graphique de l’interface utilisateur.

Reportez-vous à [Guide d’accessibilité de Google](http://developer.android.com/guide/topics/ui/accessibility/) pour plus d’informations sur la façon d’utiliser les API d’accessibilité d’Android.



## <a name="2d-graphics"></a>Graphiques 2D

Ressources drawable sont une technique répandue dans les applications Android. Comme avec d’autres ressources, ressources Drawable sont déclaratives &ndash; elles sont définies dans des fichiers XML. Cette approche permet une séparation claire du code à partir de ressources. Cela peut simplifier le développement et la maintenance, car il n’est pas nécessaire de modifier le code pour mettre à jour ou de modifier les graphiques dans une application Android. Toutefois, tandis que les ressources Drawable sont utiles pour nombreuses exigences graphiques simples et courantes, ils n’ont pas la puissance et contrôle de l’API de la zone de dessin.

L’autre technique, à l’aide de la [canevas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) d’objet, est très similaire aux autres infrastructures API traditionnelles telles que System.Drawing ou Core dessin d’iOS. À l’aide de l’objet Canvas fournit un contrôle optimal des graphismes 2D comment sont créés. Il convient aux situations dans lesquelles une ressource Drawable ne fonctionne pas ou qu’il sera difficile de travailler avec. Par exemple, il peut être nécessaire dessiner un contrôle de curseur personnalisée dont l’aspect change selon les calculs reliées à la valeur du curseur.

Examinons tout d’abord les ressources Drawable. Ils sont plus simples et couvrent les scénarios les plus courants de dessins personnalisés.


### <a name="drawable-resources"></a>Ressources drawable

Ressources drawable sont définies dans un fichier XML dans le répertoire `/Resources/drawable`. Contrairement à l’incorporation de JPEG ou PNG, il n’est pas nécessaire de fournir des versions spécifiques de la densité des ressources Drawable.
Lors de l’exécution, une application Android charge ces ressources et suivez les instructions contenues dans ces fichiers XML pour créer des graphiques 2D.
Android définit plusieurs types de ressources Drawable :

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; il s’agit d’un objet Drawable qui dessine une forme géométrique primitive et qui applique un ensemble limité d’effets graphiques cette forme. Ils sont très utiles pour les éléments tels que la personnalisation des boutons ou la définition de l’arrière-plan d’éléments TextViews. Nous verrons un exemple montrant comment utiliser un `ShapeDrawable` plus loin dans cet article.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; il s’agit d’une ressource Drawable qui va changer l’apparence en fonction de l’état d’un contrôle de widget /. Par exemple, un bouton peut modifier son apparence en fonction de si elle est activée ou non.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; cette ressource Drawable qui sera s’empilent plusieurs drawables une autre. Un exemple d’un *LayerDrawable* est illustré dans la capture d’écran suivante :

    ![Exemple de LayerDrawable](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; il s’agit d’un *LayerDrawable* mais avec une différence. Un *TransitionDrawable* est en mesure d’animer une couche s’affiche par dessus un autre.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; cela est très similaire à un *StateListDrawable* dans la mesure où il affiche une image selon certaines conditions. Cependant, contrairement à un *StateListDrawable*, le *LevelListDrawable* affiche une image basée sur une valeur entière. Un exemple d’un *LevelListDrawable* serait pour afficher la force d’un signal de Wi-Fi. En tant que la force des modifications de signal Wi-Fi, le drawable qui s’affiche est modifiée en conséquence.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; comme leur nom l’indique, ces Drawables fournissent une mise à l’échelle et des fonctionnalités de découpage. Le *ScaleDrawable* mise à l’échelle un autre while Drawable, le *ClipDrawable* fait correspondre un autre Drawable.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; ce Drawable appliquera encarts sur les côtés d’une autre ressource Drawable. Il est utilisé lorsqu’une vue a besoin d’un arrière-plan plus petite que les limites réelles de la vue.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; ce fichier est un ensemble d’instructions, au format XML, qui doivent être exécutées sur une bitmap réelle. Certaines actions réalisables par Android sont les limites de tramage et de l’anticrénelage. L’une des utilisations très courantes de ce est à la vignette d’une image bitmap sur l’arrière-plan d’une disposition.


#### <a name="drawable-example"></a>Exemple drawable

Examinons un exemple rapide de la création d’un graphique 2D à l’aide un `ShapeDrawable`. Un `ShapeDrawable` peut définir une des quatre formes de base : rectangle, ellipse, ligne et en anneau. Il est également possible d’appliquer des effets de base, telles que le dégradé de couleur et taille. Le code XML suivant est un `ShapeDrawable` qui peuvent se trouver dans le *AnimationsDemo* projet de complément (dans le fichier `Resources/drawable/shape_rounded_blue_rect.xml`).
Il définit un rectangle avec un arrière-plan dégradé violet et des angles arrondis :

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

Nous pouvons faire référence à cette ressource Drawable façon déclarative dans une disposition ou autres Drawable comme indiqué dans le code XML suivant :

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

Ressources drawable peuvent également être appliqués par programmation. L’extrait de code suivant montre comment définir par programmation de l’arrière-plan d’un contrôle TextView :

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Pour voir à quoi cela ressemblerait, exécutez le *AnimationsDemo* de projet et sélectionnez l’élément de forme Drawable dans le menu principal. Nous devrions voir quelque chose de similaire à la capture d’écran suivante :

![TextView avec un arrière-plan personnalisé, drawable avec un dégradé et arrondi l’angle](graphics-and-animation-images/image1.png)

Pour plus d’informations sur les éléments XML et la syntaxe des ressources Drawable, consultez [documentation de Google](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>À l’aide de l’API de dessin du canevas

Drawables sont puissantes, mais ont leurs limites. Certaines choses sont n’est pas possible ou très complexes (par exemple : application d’un filtre à une image qui a été effectuée par un appareil photo sur l’appareil). Il serait très difficile d’appliquer la réduction des yeux rouges à l’aide d’une ressource Drawable.
Au lieu de cela, l’API de la zone de dessin permet à une application d’avoir un contrôle très précis pour modifier les couleurs dans une partie spécifique de l’image de manière sélective.

Une classe est généralement utilisée avec la zone de dessin est la [Paint](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) classe. Cette classe conserve des informations de couleur et de style sur le dessin. Il est utilisé pour fournir des choses telles de couleur et de transparence.

Utilise l’API de canevas le *modèle du peintre* pour dessiner des graphiques 2D.
Opérations sont appliquées dans les couches successives sur chacun des autres. Chaque opération couvre une zone de l’image bitmap sous-jacent. Lorsque la zone chevauche une zone peinte précédemment, la peinture nouvelle sera partiellement ou complètement masquer l’ancien. Il s’agit de la même façon que fonctionne de nombreuses autres API de dessin tels que System.Drawing et graphismes de base d’iOS.

Il existe deux façons d’obtenir un `Canvas` objet. La première méthode consiste à définir un [Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) objet, puis l’instanciation d’un `Canvas` objet avec lui. Par exemple, l’extrait de code suivant crée une nouvelle zone avec une image bitmap sous-jacent :

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Le moyen d’obtenir un `Canvas` objet est en le [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) méthode de rappel qui est fourni le [vue](https://developer.xamarin.com/api/type/Android.Views.View/) classe de base. Android appelle cette méthode quand elle décide d’une vue doit se dessiner lui-même et transmet un `Canvas` objet pour la vue travailler avec.

La classe de zone de dessin expose des méthodes pour fournir par programmation les instructions de dessin. Exemple :

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; remplit bitmap de la totalité du canevas avec la peinture spécifiée.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; Dessine la forme géométrique spécifiée à l’aide de la peinture spécifiée.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; Dessine le texte sur la zone de dessin avec la couleur spécifiée. Le texte est dessiné à l’emplacement `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>Dessiner avec l’API de canevas

Nous allons voir un exemple de l’API de canevas en action. L’extrait de code suivant montre comment dessiner une vue :

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

Le code ci-dessus crée d’abord une peinture rouge et un objet de peinture verte. Il remplit le contenu de la zone de dessin en rouge et indique à la zone de dessin pour dessiner un rectangle vert est de 25 % de la largeur de la zone de dessin. Un exemple de ceci peut être vu par dans `AnimationsDemo` projet qui est inclus avec le code source pour cet article. En démarrant l’application et en sélectionnant l’élément de dessin dans le menu principal, il nous faut un écran semblable à ce qui suit :

![Écran avec peinture rouge et les objets de peinture verte](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animation

Les utilisateurs comme des choses qui se déplacent dans leurs applications. Les animations sont un excellent moyen d’améliorer l’expérience utilisateur d’une application et de faire ressortir. Les animations meilleures sont ceux que les utilisateurs ne remarquent parce qu’elles naturelles. Android fournit le trois API suivante pour les animations :

-   **Afficher l’Animation** &ndash; c’est l’API d’origine. Ces animations sont liées à une vue spécifique et peuvent effectuer des transformations simples sur le contenu de la vue. En raison de sa simplicité, cette API toujours utile pour des choses telles que les animations alphabétiques, rotations et ainsi de suite.

-   **Animation de la propriété** &ndash; des animations de propriété ont été introduites dans Android 3.0. Ils permettent à une application animer pratiquement tout élément. Animations de propriété peuvent être utilisées pour modifier n’importe quelle propriété de n’importe quel objet, même si cet objet n’est pas visible à l’écran.

-   **Animation drawable** &ndash; cela une ressource Drawable spéciale qui est utilisée pour appliquer une animation simple effet aux dispositions.

En règle générale, animation de la propriété est le système par défaut à utiliser comme il est plus flexible et offre davantage de fonctionnalités.


### <a name="view-animations"></a>Animations de vue

Les animations de vue sont limitées aux vues et peuvent effectuer uniquement les animations sur des valeurs telles que le début et de points de terminaison, de taille, de rotation et de transparence.
Ces types d’animations sont généralement appelées *interpolation animations*. Animations de la vue peuvent être définies de deux façons &ndash; par programmation dans le code ou à l’aide de fichiers XML. Fichiers XML sont la meilleure façon de déclarer des animations de la vue, car ils sont plus lisible et plus facile à gérer.

Les fichiers XML de l’animation seront stockés dans le `/Resources/anim` répertoire d’un projet Xamarin.Android. Ce fichier doit avoir un des éléments suivants comme élément racine :

-   `alpha` &ndash; Une animation de fondu ou fondu.

-   `rotate` &ndash; Une animation de rotation.

-   `scale` &ndash; Une animation de redimensionnement.

-   `translate` &ndash; Un mouvement horizontal et/ou vertical.

-   `set` &ndash; Un conteneur qui peut contenir un ou plusieurs des autres éléments de l’animation.

Par défaut, toutes les animations dans un fichier XML s’appliqueront simultanément. Pour que les animations se produisent de manière séquentielle, définissez le `android:startOffset` attribut sur l’un des éléments définis ci-dessus.

Il est possible d’affecter le taux de variation dans une animation en utilisant un *interpolateur*. Un interpolateur rend possible pour les effets d’animation être accélérée, répétées ou decelerated. Le framework Android fournit plusieurs interpolateurs dès le départ, comme (liste non exhaustive) :

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; Ces interpolateurs augmenter ou diminuer le taux de variation dans une animation.

-   `BounceInterpolator` &ndash; la modification rebondit à la fin.

-   `LinearInterpolator` &ndash; le taux de modifications est constante.


Le code XML suivant montre un exemple d’un fichier d’animation qui combine certains de ces éléments :

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

Cette animation toutes les animations effectuer simultanément. La première animation de mise à l’échelle s’étirent l’image horizontalement et verticalement de le réduire, et ensuite l’image sera simultanément pivoté de 45 degrés sens antihoraire et réduire, disparaissent de l’écran.

L’animation par programme applicable à une vue par gonfler l’animation et l’appliquer ensuite à une vue. Android fournit la classe d’assistance `Android.Views.Animations.AnimationUtils` qui hébergera une ressource de l’animation de la majoration et retourner une instance de `Android.Views.Animations.Animation`. Cet objet est appliqué à une vue en appelant `StartAnimation` et en passant le `Animation` objet. L’extrait de code suivant montre un exemple de ceci :

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Maintenant que nous avons une compréhension fondamentale du fonctionnement des Animations de la vue, vous permet de déplacer vers des Animations de propriété.


### <a name="property-animations"></a>Animations de propriété

Animations de propriété sont une nouvelle API qui a été introduite dans 3.0 Android.
Ils fournissent une API plus extensible qui peut être utilisée pour animer n’importe quelle propriété sur n’importe quel objet.

Toutes les animations de propriété sont créées par les instances de la [animation](https://developer.xamarin.com/api/type/Android.Animation.Animator/) sous-classe. Applications n’utilisent pas directement cette classe, au lieu de cela, ils utilisent une de ses sous-classes :

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; cette classe est la classe la plus importante dans l’animation de propriétés entier API. Il calcule les valeurs des propriétés qui doivent être modifiées. Le `ViewAnimator` ne pas mettre à jour directement ces valeurs ; au lieu de cela, il déclenche des événements qui peuvent être utilisées pour mettre à jour des objets animés.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; cette classe est une sous-classe de `ValueAnimator` . Il est destiné à simplifier le processus de l’animation d’objets en acceptant un objet cible et la propriété à mettre à jour.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; cette classe est chargée pour orchestrer comment exécutent des animations dans une relation un à l’autre. Animations peuvent s’exécuter simultanément, séquentiellement ou dans un délai spécifié entre eux.


*Évaluateurs* sont des classes spéciales qui sont utilisées par les animations pour calculer les nouvelles valeurs pendant une animation. Dès le départ, Android fournit les évaluateurs suivantes :

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; calcule les valeurs de propriétés entières.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; calcule les valeurs de propriétés de type float.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; calcule les valeurs de propriétés de couleur.

Si la propriété est en cours d’animation n’est pas un `float`, `int` ou de la couleur, les applications peuvent créer leur propre évaluateur en implémentant le `ITypeEvaluator` interface. (L’implémentation des évaluateurs personnalisés est dépasse le cadre de cette rubrique.)

#### <a name="using-the-valueanimator"></a>À l’aide de la ValueAnimator

Il existe deux parties à toute animation : calculer des valeurs animées, puis en définissant ces valeurs sur les propriétés sur un objet. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) uniquement calculera les valeurs, mais il ne fonctionnera pas sur les objets directement. Au lieu de cela, les objets seront mis à jour à l’intérieur des gestionnaires d’événements qui seront appelés pendant la durée de vie de l’animation. Cette conception permet à plusieurs propriétés à mettre à jour à partir d’une seule valeur animée.

Pour obtenir une instance de `ValueAnimator` en appelant une des méthodes de fabrique suivantes :

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Une fois cette opération effectuée, le `ValueAnimator` instance doit avoir sa durée définie, et puis elle peut être démarrée. L’exemple suivant montre comment animer une valeur comprise entre 0 et 1 sur l’intervalle de 1 000 millisecondes :

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Mais lui-même, l’extrait de code ci-dessus n’est pas très utile &ndash; l’animation s’exécute, mais il n’existe aucune cible pour la valeur mise à jour. Le `Animator` classe déclenche l’événement de mise à jour lorsque celui-ci décide qu’il est nécessaire informer les écouteurs d’une nouvelle valeur. Les applications peuvent fournir un gestionnaire d’événements pour répondre à cet événement, comme illustré dans l’extrait de code suivant :

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

Maintenant que nous avons une compréhension de `ValueAnimator`, permet d’en savoir plus sur les `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>À l’aide de la ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) est une sous-classe de `ViewAnimator` qui combine le calcul de moteur et la valeur de minutage de la `ValueAnimator` avec la logique nécessaire pour associer des gestionnaires d’événements. Le `ValueAnimator` requiert des applications à associer explicitement un gestionnaire d’événements &ndash; `ObjectAnimator` s’occupera de cette étape pour nous.

L’API pour `ObjectAnimator` est très similaire à l’API pour `ViewAnimator`, mais nécessite que vous fournissiez l’objet et le nom de la propriété à mettre à jour. L’exemple suivant montre un exemple d’utilisation `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Comme vous pouvez le voir à partir de l’extrait de code précédent, `ObjectAnimator` peut réduire et simplifier le code qui est nécessaire pour animer un objet.


### <a name="drawable-animations"></a>Animations drawable

L’API d’animation finale est l’API d’Animation Drawable. Animations drawable charger une série de ressources Drawable un après l’autre et les afficher de manière séquentielle, similaire à un dessin animé retournement-it.

Ressources drawable sont définies dans un fichier XML qui a un `<animation-list>` élément en tant que l’élément racine et une série de `<item>` éléments qui définissent chaque trame dans l’animation. Ce fichier XML est stocké dans le `/Resource/drawable` dossier de l’application. Le code XML suivant est un exemple d’une animation drawable :

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

Cette animation s’exécute via six images. Le `android:duration` attribut déclare la durée pendant laquelle chaque image s’affichera. L’extrait de code suivant montre un exemple de création d’une animation Drawable et en le démarrant lorsque l’utilisateur clique sur un bouton sur l’écran :

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

À ce stade, nous avons traité les fondements de l’animation API disponibles dans une application Android.


## <a name="summary"></a>Récapitulatif

Cet article a présenté un grand nombre de nouveaux concepts et API pour aider à ajouter des graphiques à une application Android. Tout d’abord, il décrit les différents graphiques 2D de l’API et illustré comment Android permet aux applications dessiner directement à l’écran à l’aide d’un objet de zone de dessin. Nous avons également vu certaines techniques de remplacement qui permettent de graphique doit être créé façon déclarative à l’aide de fichiers XML. Puis nous expliquait les anciennes et nouvelles API pour la création d’animations dans Android.



## <a name="related-links"></a>Liens associés

- [Démonstration de l’animation (exemple)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animation et les graphiques](http://developer.android.com/guide/topics/graphics/index.html)
- [L’utilisation des Animations à donnez vie à vos applications mobiles](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Canvas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Animation de l’objet](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Animation de valeur](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
