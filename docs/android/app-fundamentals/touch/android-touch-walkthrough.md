---
title: Procédure pas à pas - à l’aide des fonctions tactiles dans Android
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: c4192f22ebd0ad1cde27745f5439c2d18a268ed3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111023"
---
# <a name="walkthrough---using-touch-in-android"></a>Procédure pas à pas - à l’aide des fonctions tactiles dans Android

Nous voir comment utiliser les concepts de la section précédente dans une application opérationnelle. Nous allons créer une application avec quatre activités. La première activité sera un menu ou un menu général qui lance les autres activités pour illustrer les différentes API. La capture d’écran suivante montre l’activité principale :

[![Capture d’écran de l’exemple avec Touch Me bouton](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

La première activité, exemple Touch, explique comment utiliser des gestionnaires d’événements pour toucher les vues. L’activité de reconnaissance de mouvement va vous montrer comment créer une sous-classe `Android.View.Views` et gérer des événements ainsi que montrent comment gérer des gestes de pincement. La troisième et dernière activité, **mouvement personnalisé**, sera montrent comment utiliser des mouvements personnalisés. Pour rendre les choses plus faciles à suivre et d’absorber, nous allons fractionner cette procédure pas à pas en sections, avec chaque section se concentre sur une des activités.

## <a name="touch-sample-activity"></a>Exemple d’activité tactile

-   Ouvrez le projet **TouchWalkthrough\_Démarrer**. Le **MainActivity** est prêt à aller &ndash; c’est à nous pour implémenter le comportement tactile dans l’activité. Si vous exécutez l’application, puis cliquez sur **exemple Touch**, l’activité suivante doit démarrer :

    [![Capture d’écran de l’activité avec Touch commence affiché](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Maintenant que nous avons confirmé que l’activité démarre, ouvrez le fichier **TouchActivity.cs** et ajoutez un gestionnaire pour le `Touch` événements de la `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Ensuite, ajoutez la méthode suivante à **TouchActivity.cs**:

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
        {
            case MotionEventActions.Down:
            case MotionEventActions.Move:
            message = "Touch Begins";
            break;

            case MotionEventActions.Up:
            message = "Touch Ends";
            break;

            default:
            message = string.Empty;
            break;
        }

        _touchInfoTextView.Text = message;
    }
    ```

Notez que, dans le code ci-dessus que nous utilisons le `Move` et `Down` action comme identiques. Il s’agit, car même si l’utilisateur ne peut pas soulevez son doigt le `ImageView`, il peut déplacer ou la pression exercée par l’utilisateur peut changer. Ces types de modifications générera un `Move` action.

Chaque fois que l’utilisateur touche le `ImageView`, le `Touch` événement sera déclenché et notre gestionnaire affiche le message **Touch commence** sur l’écran, comme illustré dans la capture d’écran suivante :

[![Capture d’écran de l’activité avec Touch commence](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Tant que l’utilisateur touche le `ImageView`, **Touch commence** s’affichera dans le `TextView`. Lorsque l’utilisateur touche n’est plus le `ImageView`, le message **Touch se termine** s’affichera dans le `TextView`, comme illustré dans la capture d’écran suivante :

[![Capture d’écran de l’activité avec Touch se termine](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Activité de module de reconnaissance de mouvement

Vous permet de mettre en œuvre de l’activité de reconnaissance de mouvement. Cette activité va vous montrer comment faire glisser une vue autour de l’écran et illustrer une manière d’implémenter le pincer-zoomer.

-   Ajouter une nouvelle activité à l’application nommée `GestureRecognizer`.
    Modifiez le code pour cette activité afin qu’il ressemble au code suivant :

    ```csharp
    public class GestureRecognizerActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            View v = new GestureRecognizerView(this);
            SetContentView(v);
        }
    }
    ```

-   Ajouter une nouvelle Android afficher au projet et nommez-le `GestureRecognizerView`. À cette classe, ajoutez les variables suivantes :

    ```csharp
    private static readonly int InvalidPointerId = -1;

    private readonly Drawable _icon;
    private readonly ScaleGestureDetector _scaleDetector;

    private int _activePointerId = InvalidPointerId;
    private float _lastTouchX;
    private float _lastTouchY;
    private float _posX;
    private float _posY;
    private float _scaleFactor = 1.0f;
    ```

-   Ajoutez le constructeur suivant à `GestureRecognizerView`. Ce constructeur ajoutera un `ImageView` à notre activité. À ce stade le code toujours ne compilera pas &ndash; nous devons créer la classe `MyScaleListener` qui aideront à redimensionner le `ImageView` lorsque l’utilisateur pinches il :

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Pour dessiner l’image sur notre activité, nous devons remplacer le `OnDraw` méthode de la classe de vue, comme indiqué dans l’extrait de code suivant. Ce code déplacera le `ImageView` à la position spécifiée par `_posX` et `_posY` ainsi que redimensionner l’image selon le facteur d’échelle :

    ```csharp
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        canvas.Save();
        canvas.Translate(_posX, _posY);
        canvas.Scale(_scaleFactor, _scaleFactor);
        _icon.Draw(canvas);
        canvas.Restore();
    }
    ```

-   Ensuite, nous devons mettre à jour la variable d’instance `_scaleFactor` en tant que l’utilisateur pinches le `ImageView`. Nous allons ajouter une classe appelée `MyScaleListener`. Cette classe doit écouter les événements de mise à l’échelle est déclenchée par Android lorsque l’utilisateur pinches le `ImageView`.
    Ajoutez la classe suivante interne à `GestureRecognizerView`. Cette classe est un `ScaleGesture.SimpleOnScaleGestureListener`. Cette classe est une classe de commodité que les écouteurs peuvent sous-classe lorsque vous êtes intéressé par un sous-ensemble des mouvements :

    ```csharp
    private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
    {
        private readonly GestureRecognizerView _view;

        public MyScaleListener(GestureRecognizerView view)
        {
            _view = view;
        }

        public override bool OnScale(ScaleGestureDetector detector)
        {
            _view._scaleFactor *= detector.ScaleFactor;

            // put a limit on how small or big the image can get.
            if (_view._scaleFactor > 5.0f)
            {
                _view._scaleFactor = 5.0f;
            }
            if (_view._scaleFactor < 0.1f)
            {
                _view._scaleFactor = 0.1f;
            }

            _view.Invalidate();
            return true;
        }
    }
    ```

-   La méthode suivante, nous devons remplacer dans `GestureRecognizerView` est `OnTouchEvent`. Le code suivant illustre l’implémentation complète de cette méthode. Il y a beaucoup de code ici, donc vous permet de prendre une minute et rechercher ce qui se passe ici. La première chose que cette méthode n’est mise à l’échelle de l’icône si nécessaire &ndash; Ceci est géré en appelant `_scaleDetector.OnTouchEvent`. Ensuite, nous essayons de déterminer quelle action appelé cette méthode :

    - Si l’utilisateur touche l’écran avec, nous enregistrer les positions X et Y et l’ID du premier pointeur qui touche l’écran.

    - Si l’utilisateur a déplacé leur tactile sur l’écran, nous Déterminez ensuite l’amplitude du déplacement du pointeur de l’utilisateur.

    - Si l’utilisateur a relâché son doigt en dehors de l’écran, puis nous allons nous arrêter les mouvements de suivi.

    ```csharp
    public override bool OnTouchEvent(MotionEvent ev)
    {
        _scaleDetector.OnTouchEvent(ev);

        MotionEventActions action = ev.Action & MotionEventActions.Mask;
        int pointerIndex;

        switch (action)
        {
            case MotionEventActions.Down:
            _lastTouchX = ev.GetX();
            _lastTouchY = ev.GetY();
            _activePointerId = ev.GetPointerId(0);
            break;

            case MotionEventActions.Move:
            pointerIndex = ev.FindPointerIndex(_activePointerId);
            float x = ev.GetX(pointerIndex);
            float y = ev.GetY(pointerIndex);
            if (!_scaleDetector.IsInProgress)
            {
                // Only move the ScaleGestureDetector isn't already processing a gesture.
                float deltaX = x - _lastTouchX;
                float deltaY = y - _lastTouchY;
                _posX += deltaX;
                _posY += deltaY;
                Invalidate();
            }

            _lastTouchX = x;
            _lastTouchY = y;
            break;

            case MotionEventActions.Up:
            case MotionEventActions.Cancel:
            // We no longer need to keep track of the active pointer.
            _activePointerId = InvalidPointerId;
            break;

            case MotionEventActions.PointerUp:
            // check to make sure that the pointer that went up is for the gesture we're tracking.
            pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
            int pointerId = ev.GetPointerId(pointerIndex);
            if (pointerId == _activePointerId)
            {
                // This was our active pointer going up. Choose a new
                // action pointer and adjust accordingly
                int newPointerIndex = pointerIndex == 0 ? 1 : 0;
                _lastTouchX = ev.GetX(newPointerIndex);
                _lastTouchY = ev.GetY(newPointerIndex);
                _activePointerId = ev.GetPointerId(newPointerIndex);
            }
            break;

        }
        return true;
    }
    ```

-   Maintenant, exécutez l’application et démarrer l’activité de reconnaissance de mouvement.
    Lorsqu’il démarre l’écran doit ressembler à la capture d’écran ci-dessous :

    [![Écran de démarrage de reconnaissance de mouvement avec icône Android](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Maintenant touchez l’icône et faites-le glisser dans l’écran. Essayez le geste de pincement pour zoomer. À un moment donné votre écran doit ressembler à la capture d’écran suivante :

    [![Icône de déplacement de mouvements autour de l’écran](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

À ce stade vous devez donner à vous-même un jeton pat à l’arrière : vous avez simplement implémenté pincer-zoomer dans une application Android ! Faites une pause rapide et vous permet de passer à la troisième et dernière activité dans cette procédure pas à pas &ndash; à l’aide des mouvements personnalisés.

## <a name="custom-gesture-activity"></a>Activité des mouvements personnalisés

Le dernier écran dans cette procédure pas à pas utilise des mouvements personnalisés.

Dans le cadre de cette procédure pas à pas, la bibliothèque de mouvements déjà été créée à l’aide du mouvement d’outil et ajoutée au projet dans le fichier **ressources/raw/gestes**. Avec ce peu de ménage disparaître, permet d’obtenir avec la dernière activité de la procédure pas à pas.

-   Ajoutez un fichier de disposition nommé **personnalisé\_mouvement\_layout.axml** au projet avec le contenu suivant. Le projet est déjà toutes les images de la **ressources** dossier :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
        <ImageView
            android:src="@drawable/check_me"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="3"
            android:id="@+id/imageView1"
            android:layout_gravity="center_vertical" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    </LinearLayout>
    ```

-   Ensuite, ajoutez une nouvelle activité au projet et nommez-le `CustomGestureRecognizerActivity.cs`. Ajoutez deux variables d’instance à la classe, comme dans les deux lignes de code suivantes :

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Modifier la `OnCreate` méthode de cette activité afin qu’elle ressemble au code suivant. Vous permet de prendre une minute pour expliquer ce qui se passait dans ce code. La première chose à faire est d’instancier un `GestureOverlayView` définissant comme l’affichage de l’activité racine.
    Nous avons également affecter un gestionnaire d’événements pour le `GesturePerformed` événement de `GestureOverlayView`. Ensuite nous majoration le fichier de disposition qui a été créé précédemment et l’ajouter en tant qu’une vue enfant de le `GestureOverlayView`. L’étape finale consiste à initialiser la variable `_gestureLibrary` et charger le fichier de mouvements dans les ressources d’application. Si le fichier de mouvements ne peut pas être chargé pour une raison quelconque, n’est pas une grande partie cette activité peut faire, donc il est arrêté :

    ```csharp
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
        SetContentView(gestureOverlayView);
        gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

        View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
        _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
        gestureOverlayView.AddView(view);

        _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
        if (!_gestureLibrary.Load())
        {
            Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
            Finish();
        }
    }
    ```

-   La dernière chose que nous devons implémenter la méthode `GestureOverlayViewOnGesturePerformed` comme indiqué dans l’extrait de code suivant. Lorsque le `GestureOverlayView` détecte un mouvement, qu’il rappelle à cette méthode. La première chose que nous tentons d’obtenir un `IList<Prediction>` objets qui correspondent au mouvement en appelant `_gestureLibrary.Recognize()`. Nous utilisons un peu de LINQ pour obtenir le `Prediction` ayant le score le plus élevé du geste.

    S’il n’y avait aucune mise en correspondance avec une haute du mouvement suffisamment score, puis le Gestionnaire d’événements s’arrête sans rien faire. Sinon, nous Vérifiez le nom de la prédiction et modifier l’image qui est affichée en fonction du nom du mouvement :

    ```csharp
    private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
    {
        IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
        orderby p.Score descending
        where p.Score > 1.0
        select p;
        Prediction prediction = predictions.FirstOrDefault();

        if (prediction == null)
        {
            Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
            return;
        }

        Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

        if (prediction.Name.StartsWith("checkmark"))
        {
            _imageView.SetImageResource(Resource.Drawable.checked_me);
        }
        else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
        {
            // Match one of our "erase" gestures
            _imageView.SetImageResource(Resource.Drawable.check_me);
        }
    }
    ```

-   Exécutez l’application et démarrent à l’activité de reconnaissance de mouvement personnalisée. Il doit ressembler à la capture d’écran suivante :

    [![Capture d’écran avec moi en vérifier l’image](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Dessiner maintenant une coche sur l’écran et la bitmap affichée doit ressembler à celui illustré dans les captures d’écran suivante :

    [![Coche dessiné, coche est reconnu.](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Enfin, dessinez un dessin à main levée sur l’écran. La case à cocher doit changer à son image d’origine, comme illustré dans ces captures d’écran :

    [![Scribble sur l’écran, l’image d’origine s’affiche.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Vous disposez maintenant comprendre comment intégrer touchers et gestes dans une application Android à l’aide de Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Android Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch finale (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
