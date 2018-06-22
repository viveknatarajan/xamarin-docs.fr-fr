---
title: Graphiques et des animations
description: Android fournit une infrastructure de très riche et diverses pour prendre en charge les animations et les graphiques 2D. Cette rubrique présente ces infrastructures et explique comment créer des graphiques personnalisés et les animations pour une utilisation dans une application Xamarin.Android.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 85a7cd2ac5094a4760c5465bd099ce2e3beeae79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773785"
---
# <a name="graphics-and-animation"></a>Graphiques et des animations

_Android fournit une infrastructure de très riche et diverses pour prendre en charge les animations et les graphiques 2D. Cette rubrique présente ces infrastructures et explique comment créer des graphiques personnalisés et les animations pour une utilisation dans une application Xamarin.Android._


## <a name="overview"></a>Vue d'ensemble

En dépit d’en cours d’exécution sur les appareils qui sont traditionnellement d’alimentation limitée, les applications mobiles nominales la plus élevées ont souvent un sophistiquées utilisateur expérience (UX), terminée avec des graphiques de haute qualité et les animations qui fournissent une interface intuitive, dynamique et réactive. Comme les applications mobiles d’obtenir plus d’informations et plus complexe, les utilisateurs ont commencé à attendre plus en plus d’applications.

Heureusement pour nous, les plateformes mobiles modernes ont des infrastructures très puissantes pour la création d’animations sophistiquées et des graphiques personnalisés tout en conservant la facilité d’utilisation. Cela permet aux développeurs d’ajouter une interactivité de pointe avec très peu d’effort.

Infrastructures d’interface utilisateur API Android à peu près peuvent être divisées en deux catégories : graphiques et l’Animation.

Graphiques sont divisés dans différentes approches pour créer des graphiques 2D et 3D. Graphiques 3D sont disponibles via un nombre d’infrastructures telles que OpenGL ES (version mobile de OpenGL spécifique) et d’infrastructures tierces telles que MonoGame (un toolkit inter-plateformes compatible avec la boîte à outils XNA). Bien que les graphiques 3D ne se trouvent pas dans le cadre de cet article, nous allons examiner les techniques de dessin 2D intégrés.

Android fournit deux API différentes pour créer des graphiques 2D. Un est une approche déclarative de haut niveau et l’autre une API de bas niveau par programmation :

-   **Ressources drawable** &ndash; ceux-ci servent à créer des graphiques personnalisés par programme ou (plus généralement) en incorporant des instructions de dessin dans des fichiers XML. Ressources drawable sont généralement définies en tant que fichiers XML qui contiennent des instructions ou des actions Android restituer un graphique 2D. 

-   **Zone de dessin** &ndash; il s’agit d’une API de bas niveau qui implique le dessin directement sur une image bitmap sous-jacent. Il fournit un contrôle précis sur ce qui est affiché. 

En plus de ces techniques graphiques 2D, Android fournit également plusieurs façons différentes pour créer des animations :

-   **Animations drawable** &ndash; Android prend également en charge les animations image par image appelées *Drawable Animation*. Il s’agit de l’API d’animation la plus simple. Android se charge de manière séquentielle et affiche les ressources Drawable en séquence (comme un dessin animé).

-   **Visualiser les Animations** &ndash; *vue Animations* sont l’animation d’origine de l’API dans Android et sont disponibles dans toutes les versions d’Android. Cette API est limitée, il fonctionne qu’avec les objets de vue et que vous ne pouvez effectuer des transformations simples sur ces vues.
    Affichage des animations sont généralement définies dans les fichiers XML se trouvent dans le `/Resources/anim` dossier.

-   **Propriété Animations** &ndash; Android 3.0 a introduit un nouvel ensemble de l’API d’animation appelé *Animations de propriété*. Ces nouvelles API a introduit un système extensible et flexible qui peut être utilisé pour animer les propriétés de n’importe quel objet, pas seulement d’afficher des objets. Cette flexibilité permet des animations d’être encapsulées dans des classes distinctes qui fera de partage de code.


Affichage des Animations sont plus appropriées pour les applications qui doivent prendre en charge les plus anciens Android préliminaire 3.0 API (API niveau 11). Sinon, les applications doivent utiliser la plus récente propriété API d’Animation pour les raisons mentionnées ci-dessus.

Toutes ces infrastructures sont viables toutefois lorsque cela est possible, préférence doit être accordée aux Animations de la propriété, car il s’agit d’une API plus souple pour travailler avec des options. Autorisent les Animations de propriété pour la logique d’animation d’être encapsulées dans des classes distinctes qui rend le partage de code et simplifie la maintenance du code.


## <a name="accessibility"></a>Accessibilité

Graphiques et les animations permettant de rendre des applications Android attrayant et agréable à utiliser ; Toutefois, il est important de se rappeler que certaines interactions se produisent via screenreaders, autres périphériques d’entrée, ou avec zoom assistée.
En outre, certaines peuvent se produire sans fonctionnalités audio.

Les applications ne sont plus utilisables dans ces situations si elles ont été conçues avec l’accessibilité à l’esprit : fournissant des indicateurs et assistance de navigation dans l’interface utilisateur et en garantissant le contenu de texte ou des descriptions pour les éléments de représentation graphique de l’interface utilisateur existe.

Reportez-vous à [Guide d’accessibilité de Google](http://developer.android.com/guide/topics/ui/accessibility/) pour plus d’informations sur la façon d’utiliser les API d’accessibilité d’Android.



## <a name="2d-graphics"></a>Graphiques 2D

Ressources drawable constituent une technique courante dans les applications Android. Comme avec d’autres ressources, ressources Drawable sont déclaratives &ndash; qu’ils sont définis dans des fichiers XML. Cette approche permet une séparation nette de code à partir des ressources. Cela peut simplifier le développement et maintenance, car il n’est pas nécessaire de modifier le code pour mettre à jour ou changer les graphismes dans une application Android. Toutefois, alors que les ressources Drawable sont utiles pour les nombreuses exigences graphiques simples et courantes, ils ne disposent pas de l’alimentation et le contrôle de l’API de la zone de dessin.

L’autre technique, à l’aide de la [canevas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) d’objet, est très similaire à d’autres infrastructures de API traditionnelles telles que System.Drawing ou de dessin de noyaux d’iOS. À l’aide de l’objet de la zone de dessin fournit un contrôle optimal des graphiques 2D comment sont créés. Il est appropriée pour les situations dans lesquelles une ressource Drawable ne fonctionne pas ou qu’il sera difficile à utiliser. Par exemple, il peut être nécessaire pour dessiner un contrôle slider personnalisé dont l’aspect change selon les calculs reliées à la valeur du curseur.

Examinons tout d’abord les ressources Drawable. Ils sont plus simples et couvrent les scénarios les plus courants de dessin personnalisés.


### <a name="drawable-resources"></a>Ressources drawable

Ressources drawable sont définies dans un fichier XML dans le répertoire `/Resources/drawable`. Contrairement à l’incorporation PNG ou de JPEG, il n’est pas nécessaire de fournir des versions spécifiques de la densité des ressources Drawable.
Lors de l’exécution, une application Android charge ces ressources et les instructions contenues dans ces fichiers XML permet de créer des graphiques 2D.
Android définit plusieurs types de ressources Drawable :

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; il s’agit d’un objet Drawable qui dessine une primitive de forme géométrique et s’applique à un ensemble limité d’effets graphiques cette forme. Ils sont très utiles pour les éléments tels que la personnalisation des boutons ou la définition de l’arrière-plan d’éléments TextViews. Nous allons voir un exemple montrant comment utiliser un `ShapeDrawable` plus loin dans cet article.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; il s’agit d’une ressource Drawable qui modifiera l’apparence en fonction de l’état d’un contrôle de widget /. Par exemple, un bouton peut modifier son apparence selon si elle est activée ou non.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; cette ressource Drawable qui sera s’empilent plusieurs drawables une autre. Un exemple d’un *LayerDrawable* est indiqué dans la capture d’écran suivante :

    ![Exemple de LayerDrawable](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; il s’agit d’un *LayerDrawable* , mais avec une différence. A *TransitionDrawable* est en mesure de les animer une couche affiche par-dessus un autre.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; cela est très similaire à un *StateListDrawable* car il affiche une image en fonction de certaines conditions. Cependant, contrairement à un *StateListDrawable*, le *LevelListDrawable* affiche une image basée sur une valeur entière. Un exemple d’un *LevelListDrawable* serait d’afficher la puissance d’un signal Wi-Fi. Comme la force des modifications de signal Wi-Fi, le drawable affiché change en conséquence.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; comme leur nom l’indique, ces Drawables fournissent une mise à l’échelle et extrait les fonctionnalités. Le *ScaleDrawable* mise à l’échelle un autre while Drawable, le *ClipDrawable* fait correspondre un autre Drawable.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; Drawable ce appliquera marges sur les côtés d’une autre ressource Drawable. Il est utilisé lorsqu’une vue doit un arrière-plan est inférieur aux limites réelles de la vue.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; ce fichier est un ensemble d’instructions, au format XML, qui doivent être effectuées sur une image bitmap réelle. Certaines actions Android peut effectuer sont les limites, tramage et l’anticrénelage. Une des utilisations courantes de ce consiste à répartir une image bitmap de l’arrière-plan d’une mise en page.


#### <a name="drawable-example"></a>Exemple drawable

Examinons un exemple rapide de la création d’un graphique 2D à l’aide un `ShapeDrawable`. Un `ShapeDrawable` peut définir une des quatre formes de base : rectangle, ovale, ligne et en anneau. Il est également possible d’appliquer des effets de base, telles que le dégradé de couleur et taille. Le code XML suivant est un `ShapeDrawable` qui se trouvent dans le *AnimationsDemo* projet d’accompagnement (dans le fichier `Resources/drawable/shape_rounded_blue_rect.xml`).
Elle définit un rectangle avec un arrière-plan dégradé violet et des angles arrondis :

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

Nous pouvons faire référence à cette ressource Drawable de façon déclarative dans une disposition ou d’autres Drawable comme indiqué dans le code XML suivant :

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

Ressources drawable peuvent également être appliquées par programme. L’extrait de code suivant montre comment définir par programme l’arrière-plan d’un contrôle TextView :

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Pour voir à quoi il ressemblera, exécutez le *AnimationsDemo* de projet et sélectionnez l’élément forme Drawable dans le menu principal. Nous devons voir quelque chose de similaire à la capture d’écran suivante :

![TextView avec un arrière-plan personnalisé, drawable avec un dégradé et arrondi après](graphics-and-animation-images/image1.png)

Pour plus d’informations sur les éléments XML et la syntaxe de ressources Drawable, consultez [documentation de Google](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>À l’aide de l’API de dessin de zone de dessin

Drawables sont puissants mais ont leurs limites. Certains éléments sont très complexe ou n’est pas possible (par exemple : application d’un filtre à une image qui a été effectuée par un appareil photo sur l’appareil). Il serait très difficile d’appliquer la réduction des yeux rouges à l’aide d’une ressource Drawable.
Au lieu de cela, l’API de la zone de dessin permet à une application d’avoir un contrôle très précis pour modifier les couleurs dans une partie spécifique de l’image de manière sélective.

Une classe qui est généralement utilisée avec la zone de dessin est la [peinture](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) classe. Cette classe contient des informations de style et de couleur sur le dessin. Il est utilisé pour fournir des éléments tels une couleur et la transparence.

L’API de la zone de dessin utilise le *modèle du peintre* pour dessiner des graphiques 2D.
Les opérations sont appliquées dans les couches successives unes sur les autres. Chaque opération couvre une zone de l’image bitmap sous-jacent. Lorsque la zone chevauche une zone de peinture précédemment, la nouvelle peinture sera partiellement ou complètement masquer l’ancien. Il s’agit de la même façon que le travail de nombreuses autres API de dessin tels que System.Drawing et graphiques de base d’iOS.

Il existe deux façons d’obtenir un `Canvas` objet. La première méthode consiste à définir un [Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) objet, puis l’instanciation d’un `Canvas` objet avec lui. Par exemple, l’extrait de code suivant crée une nouvelle zone avec une image bitmap sous-jacent :

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Le moyen d’obtenir un `Canvas` objet est en le [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) méthode de rappel qui est fourni le [vue](https://developer.xamarin.com/api/type/Android.Views.View/) classe de base. Android appelle cette méthode lorsqu’il décide une vue doit dessiner lui-même et passe un `Canvas` objet pour l’affichage à utiliser.

La classe de zone de dessin expose des méthodes pour fournir par programme les instructions de dessin. Par exemple :

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; remplit les bitmap de tout le canevas avec la peinture spécifiée.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; Dessine la forme géométrique spécifiée à l’aide de la peinture spécifiée.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; Dessine le texte dans la zone de dessin avec la couleur spécifiée. Le texte est dessiné à l’emplacement `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>Avec l’API de la zone de dessin

Examinons un exemple de l’API de la zone de dessin en action. L’extrait de code suivant montre comment dessiner une vue :

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

Ce code ci-dessus crée d’abord une peinture rouge et un objet de peinture verte. Il remplit le contenu de la zone de dessin en rouge et indique à la zone de dessin pour dessiner un rectangle vert est 25 % de la largeur de la zone de dessin. Un exemple de ce peut être vu par dans `AnimationsDemo` projet est inclus dans le code source pour cet article. En démarrant l’application et en sélectionnant l’élément de dessin dans le menu principal, nous devons un écran semblable à la suivante :

![Écran de peinture rouge et d’objets de peinture verte](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animation

Les utilisateurs tels que les éléments qui se déplacent dans leurs applications. Les animations sont un excellent moyen d’améliorer l’expérience utilisateur d’une application et l’aider à ressortir. Les meilleures animations sont ceux que les utilisateurs ne remarquent parce qu’elles naturelles. Android fournit le trois API suivante pour les animations :

-   **Afficher l’Animation** &ndash; c’est l’API d’origine. Ces animations sont attachées à une vue spécifique et peuvent effectuer des transformations simples sur le contenu de la vue. En raison de sa simplicité, cette API toujours utile pour des opérations telles que les animations alphanumériques, rotations et ainsi de suite.

-   **Animation de la propriété** &ndash; animations de propriété ont été introduites dans 3.0 Android. Ils permettent à une application animer presque rien. Animations de propriété peuvent servir à modifier n’importe quelle propriété de n’importe quel objet, même si cet objet n’est pas visible à l’écran.

-   **Animation drawable** &ndash; cela une ressource Drawable spéciale qui est utilisée pour appliquer une animation simple effet aux mises en page.

En règle générale, animation de la propriété est le système par défaut à utiliser lorsqu’il est plus souple et offre davantage de fonctionnalités.


### <a name="view-animations"></a>Affichage des Animations

Vue animations sont limitées aux vues et peuvent exécuter uniquement des animations sur les valeurs de début et de points de terminaison, de taille, de rotation et de transparence.
Ces types d’animations sont généralement appelés *interpolation animations*. Animations de la vue peuvent être définies de deux façons &ndash; par programmation dans le code ou à l’aide de fichiers XML. Fichiers XML sont la meilleure façon de déclarer des animations de la vue, car ils sont plus lisible et plus facile à gérer.

Les fichiers XML de l’animation seront stockées dans le `/Resources/anim` répertoire d’un projet Xamarin.Android. Ce fichier doit avoir un des éléments suivants en tant que l’élément racine :

-   `alpha` &ndash; Une animation de fondu ou disparaître progressivement.

-   `rotate` &ndash; Une animation de rotation.

-   `scale` &ndash; Une animation de redimensionnement.

-   `translate` &ndash; Un mouvement horizontal ou vertical.

-   `set` &ndash; Un conteneur qui peut contenir un ou plusieurs des autres éléments de l’animation.

Par défaut, toutes les animations d’un fichier XML seront appliquées simultanément. Pour rendre les animations se produisent de manière séquentielle, définissez la `android:startOffset` attribut sur l’un des éléments définis ci-dessus.

Il est possible d’affecter le taux de changement d’une animation en utilisant un *interpolateur*. Un interpolateur rend possible pour les effets d’animation pour être accéléré, répétée ou decelerated. L’infrastructure Android fournit plusieurs interpolateurs prêtes à l’emploi, telles que (liste non exhaustive) :

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; Ces interpolateurs augmentent ou diminuer le taux de changement d’une animation.

-   `BounceInterpolator` &ndash; la modification rebondit à la fin.

-   `LinearInterpolator` &ndash; le taux de modifications est constant.


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

Cette animation peut exécuter toutes les animations simultanément. La première animation de la mise à l’échelle s’étirer l’image horizontalement réduire verticalement et ensuite l’image sera simultanément pivoté de 45 degrés aiguilles et réduire, disparaissent de l’écran.

L’animation par programme applicable à une vue par l’animation de gonfler et de les appliquer ensuite à une vue. Android fournit la classe d’assistance `Android.Views.Animations.AnimationUtils` qui permettra d’augmenter une ressource de l’animation et de retourner une instance de `Android.Views.Animations.Animation`. Cet objet est appliqué à une vue en appelant `StartAnimation` et en passant le `Animation` objet. L’extrait de code suivant montre un exemple :

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Maintenant que nous avons une connaissance fondamentale de fonctionnement de la vue des Animations, permet de déplacer les Animations de propriété.


### <a name="property-animations"></a>Animations de propriété

Les animations de propriété sont une nouvelle API qui a été introduite dans 3.0 Android.
Ils fournissent une API plus extensible qui peut être utilisée pour animer une propriété sur n’importe quel objet.

Toutes les animations de propriété sont créées par les instances de la [animation](https://developer.xamarin.com/api/type/Android.Animation.Animator/) sous-classe. Les applications n’utilisent pas directement de cette classe, au lieu de cela, ils utilisent une de ses sous-classes :

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; cette classe est la classe la plus importante de l’animation de la propriété entière API. Il calcule les valeurs des propriétés qui doivent être modifiées. Le `ViewAnimator` ne pas mettre à jour directement ces valeurs ; au lieu de cela, il déclenche des événements qui peuvent être utilisés pour mettre à jour des objets animés.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; cette classe est une sous-classe de `ValueAnimator` . Il est destiné à simplifier le processus d’animation d’objets en acceptant un objet cible et la propriété à mettre à jour.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; cette classe est responsable de l’orchestration comment les animations s’exécutent par rapport à un autre. Animations peuvent s’exécuter simultanément, séquentiellement ou dans un délai spécifié entre eux.


*Évaluateurs* sont des classes spéciales qui sont utilisés par des animations pour calculer les nouvelles valeurs pendant une animation. L’emploi, Android fournit les évaluateurs suivantes :

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; calcule les valeurs de propriétés de l’entier.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; calcule les valeurs de propriétés de type float.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; calcule les valeurs de propriétés de couleur.

Si la propriété est en cours d’animation n’est pas un `float`, `int` ou de la couleur, les applications peuvent créer leur propres évaluateur en implémentant le `ITypeEvaluator` interface. (L’implémentation des évaluateurs personnalisés est dépasse le cadre de cette rubrique.)

#### <a name="using-the-valueanimator"></a>À l’aide de la ValueAnimator

Pour les animations en deux parties : calculer des valeurs animées, puis en définissant ces valeurs sur les propriétés sur un objet. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) uniquement calculera les valeurs, mais il ne fonctionne pas directement sur des objets. Au lieu de cela, les objets seront mise à jour à l’intérieur des gestionnaires d’événements qui seront appelés pendant la durée de vie de l’animation. Cette conception permet à plusieurs propriétés à mettre à jour à partir d’une valeur animée.

Pour obtenir une instance de `ValueAnimator` en appelant une des méthodes de fabrique suivantes :

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Ensuite, la `ValueAnimator` doit avoir sa durée est définie, et il peut ensuite être démarré. L’exemple suivant montre comment animer une valeur comprise entre 0 et 1 sur la durée de 1000 millisecondes :

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Mais, l’extrait de code ci-dessus n’est pas très utile &ndash; l’animation s’exécutera, mais il n’existe aucune cible pour la valeur mise à jour. La `Animator` classe déclenche l’événement de mise à jour lorsqu’elle décide qu’il est nécessaire informer les écouteurs d’une nouvelle valeur. Les applications peuvent fournir un gestionnaire d’événements pour répondre à cet événement, comme indiqué dans l’extrait de code suivant :

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

Maintenant que nous avons une compréhension des `ValueAnimator`, permet d’en savoir plus sur les `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>À l’aide de la ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) est une sous-classe de `ViewAnimator` qui combine le calcul de moteur et la valeur de minutage de la `ValueAnimator` avec la logique nécessaire pour des gestionnaires d’événements. Le `ValueAnimator` exige des applications afin d’associer un gestionnaire d’événements explicitement &ndash; `ObjectAnimator` se charge de cette étape pour nous.

L’API pour `ObjectAnimator` est très similaire à l’API pour `ViewAnimator`, mais vous devez fournir l’objet et le nom de la propriété à mettre à jour. L’exemple suivant montre un exemple d’utilisation `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Comme vous pouvez le voir à partir de l’extrait de code précédent, `ObjectAnimator` permet de réduire et de simplifier le code qui est nécessaire pour animer un objet.


### <a name="drawable-animations"></a>Animations drawable

L’API d’animation finale est l’API d’Animation Drawable. Animations drawable charger une série de ressources Drawable un après l’autre et les afficher de manière séquentielle, similaire à un personnage de retournement d’informatique.

Ressources drawable sont définies dans un fichier XML qui a un `<animation-list>` élément en tant que l’élément racine et une série de `<item>` éléments qui définissent chaque image de l’animation. Ce fichier XML est stocké dans le `/Resource/drawable` dossier de l’application. Le code XML suivant est un exemple d’une animation drawable :

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

Cette animation s’exécutera à six images. Le `android:duration` attribut déclare la durée pendant laquelle chaque image s’affiche. L’extrait de code suivant montre un exemple de création d’une animation Drawable et en le démarrant lorsque l’utilisateur clique sur un bouton à l’écran :

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

À ce stade, nous avons couvert les bases de l’API disponibles dans une application Android de l’animation.


## <a name="summary"></a>Récapitulatif

Cet article a introduit un grand nombre de nouveaux concepts et les API pour aider à ajouter des graphiques à une application Android. Tout d’abord, il décrit les graphiques 2D diverses API et illustré comment Android permet aux applications dessiner directement à l’écran à l’aide d’un objet de la zone de dessin. Nous avons vu également certaines techniques de remplacement qui autorisent les graphiques de façon déclarative création à l’aide de fichiers XML. Nous avons puis pour présenter les anciennes et nouvelles API pour créer des animations dans Android.



## <a name="related-links"></a>Liens associés

- [Démonstration de l’animation (exemple)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animation et les graphiques](http://developer.android.com/guide/topics/graphics/index.html)
- [À l’aide des Animations pour donner vie des applications mobiles](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Canvas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Animation de l’objet](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Animation de la valeur](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
