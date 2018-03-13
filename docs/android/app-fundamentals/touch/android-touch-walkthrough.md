---
title: "Procédure pas à pas - à l’aide des fonctions tactiles dans Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: ec8d000f235a8e0da5198e425f9f1e630b2c662a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---using-touch-in-android"></a>Procédure pas à pas - à l’aide des fonctions tactiles dans Android

Nous expliquons comment utiliser les concepts de la section précédente dans une application. Nous allons créer une application avec quatre activités. La première activité à un menu ou à un menu général qui lance les autres activités pour illustrer les différentes API. La capture d’écran suivante montre l’activité principale :

[![Capture d’écran de l’exemple avec Touch Me bouton](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

La première activité, exemple Touch, affiche l’utilisation des gestionnaires d’événements pour toucher les vues. L’activité de reconnaissance de mouvement va vous montrer comment créer une sous-classe `Android.View.Views` et gérer les événements ainsi montrent comment gérer les mouvements de pincement. La troisième et dernière activité, **des mouvements personnalisés**, sera montrent comment utiliser des mouvements personnalisés. Pour simplifier les choses à suivre et absorber, nous allons scinder cette procédure pas à pas en sections, avec chaque section en mettant l’accent sur une des activités.

## <a name="touch-sample-activity"></a>Exemple d’activité tactile

-   Ouvrez le projet **TouchWalkthrough\_Démarrer**. Le **MainActivity** atteindre la valeur tous les &ndash; c’est à nous pour implémenter le comportement tactile dans l’activité. Si vous exécutez l’application et cliquez sur **toucher l’exemple**, l’activité suivante doit démarrer :

    [![Capture d’écran de l’activité commence Touch affiché](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Maintenant que nous avons vérifié que l’activité démarre, ouvrez le fichier **TouchActivity.cs** et ajoutez un gestionnaire pour le `Touch` l’événement de la `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Ensuite, ajoutez la méthode suivante à **TouchActivity.cs**:

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventArgs.Mask)
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

Notez que, dans le code ci-dessus que nous utilisons le `Move` et `Down` action comme identiques. Effet, même si l’utilisateur ne peut pas relâchez leurs le `ImageView`, elle peut déplacer ou la pression exercée par l’utilisateur peut modifier. Ces types de modifications générera un `Move` action.

Chaque fois que l’utilisateur touche le `ImageView`, le `Touch` événement sera déclenché et notre gestionnaire affichera le message **tactile commence** sur l’écran, comme illustré dans la capture d’écran suivante :

[![Capture d’écran de l’activité avec Touch commence](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Tant que l’utilisateur touche le `ImageView`, **tactile commence** s’affichera dans le `TextView`. Lorsque l’utilisateur n’est plus affecte le `ImageView`, le message **tactile se termine** s’affichera dans le `TextView`, comme illustré dans la capture d’écran suivante :

[![Capture d’écran de l’activité avec Touch se termine](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Activité de module de reconnaissance de mouvement

Vous permet de mettre en œuvre de l’activité de reconnaissance de mouvement. Cette activité va vous montrer comment faire glisser une vue autour de l’écran et illustrer un moyen d’implémenter le zoom par pincement.

-   Ajouter une nouvelle activité à l’application nommée `GestureRecognizer`.
    Modifier le code pour cette activité afin qu’il ressemble au code suivant :

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

-   Ajouter un nouveau Android afficher au projet et nommez-le `GestureRecognizerView`. Cette classe, ajoutez les variables suivantes :

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

-   Ajoutez le constructeur suivant à `GestureRecognizerView`. Ce constructeur ajoutera un `ImageView` à notre activité. À ce stade le code toujours compilera pas &ndash; nous devons créer la classe `MyScaleListener` qui aideront à redimensionner le `ImageView` lorsque l’utilisateur pinches il :

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Pour dessiner l’image sur notre activité, nous devons remplacer le `OnDraw` méthode de la classe de vue, comme indiqué dans l’extrait de code suivant. Ce code déplace le `ImageView` à la position spécifiée par `_posX` et `_posY` ainsi que redimensionner l’image selon le facteur d’échelle :

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

-   Ensuite, nous avons besoin mettre à jour la variable d’instance `_scaleFactor` en tant que l’utilisateur pinches le `ImageView`. Nous allons ajouter une classe appelée `MyScaleListener`. Cette classe doit écouter les événements de mise à l’échelle qui est déclenché par Android quand l’utilisateur pinches le `ImageView`.
    Ajoutez la classe suivante interne à `GestureRecognizerView`. Cette classe est un `ScaleGesture.SimpleOnScaleGestureListener`. Cette classe est une classe pratique que les écouteurs peuvent sous-classe lorsque vous êtes intéressé par un sous-ensemble des mouvements :

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

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   La méthode suivante, nous devons remplacer dans `GestureRecognizerView` est `OnTouchEvent`. Le code suivant montre l’implémentation complète de cette méthode. Il y a beaucoup de code ici, donc vous permet de prendre une minute et ressembler à ce qui se passe sur ici. La première chose que cette méthode n’est mise à l’échelle de l’icône si nécessaire &ndash; Ceci est géré en appelant `_scaleDetector.OnTouchEvent`. Ensuite, nous essayons de déterminer quelle action appelé cette méthode :

    - Si l’utilisateur touche l’écran, nous enregistre les positions X et Y et l’ID du premier pointeur qui touche l’écran.

    - Si l’utilisateur a déplacé leur tactile sur l’écran, nous déterminer l’amplitude l’utilisateur a déplacé le pointeur.

    - Si l’utilisateur a levé doigt hors de l’écran, puis nous allons nous arrêter les mouvements de suivi.

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

-   Maintenant sélectionnez l’icône et faites-le glisser dans l’écran. Essayez du mouvement de pincement pour zoomer. À un moment donné votre écran doit ressembler à la capture d’écran suivante :

    [![Icône de déplacement de mouvements autour de l’écran](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

À ce stade, vous devez donner vous-même un jeton d’accès personnel à l’arrière : vous venez d’implémenter deux doigts pour zoomer dans une application Android ! Faites une pause rapide et permet de passer à la troisième et dernière activité dans cette procédure pas à pas &ndash; à l’aide des mouvements personnalisés.

## <a name="custom-gesture-activity"></a>Activité des mouvements personnalisés

Le dernier écran dans cette procédure pas à pas utilise des mouvements personnalisés.

Dans le cadre de cette procédure pas à pas, la bibliothèque de mouvements déjà créée à l’aide du mouvement d’outil et ajoutée au projet dans le fichier **ressources/brut/mouvements**. Avec ce bit de gestion de, permet d’obtenir avec la dernière activité de la procédure pas à pas.

-   Ajoutez un fichier de disposition nommé **personnalisé\_mouvement\_layout.axml** au projet avec le contenu suivant. Le projet a déjà toutes les images le **ressources** dossier :

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

-   Ensuite ajouter une nouvelle activité pour le projet et nommez-le `CustomGestureRecognizerActivity.cs`. Ajoutez deux variables de l’instance à la classe, comme dans les deux lignes de code suivantes :

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Modifier la `OnCreate` méthode de cette activité afin qu’il ressemble au code suivant. Vous permet de prendre une minute pour expliquer ce qui se passe dans ce code. La première chose à faire est d’instancier un `GestureOverlayView` et le définit comme l’affichage de l’activité racine.
    Nous avons également affecter un gestionnaire d’événements pour le `GesturePerformed` l’événement de `GestureOverlayView`. Ensuite nous décompresser le fichier de disposition qui a été créé précédemment et équivaut à une vue enfant de la `GestureOverlayView`. L’étape finale consiste à initialiser la variable `_gestureLibrary` et charger le fichier de mouvements à partir des ressources d’application. Si le fichier de mouvements ne peut pas être chargé pour une raison quelconque, il n’est pas cette activité peut faire, il est arrêté :

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

-   La dernière chose que nous devons n’implémentent pas la méthode `GestureOverlayViewOnGesturePerformed` comme indiqué dans l’extrait de code suivant. Lorsque le `GestureOverlayView` détecte un mouvement, qu’il rappelle à cette méthode. La première chose que nous tente d’obtenir un `IList<Prediction>` objets qui correspondent à du mouvement en appelant `_gestureLibrary.Recognize()`. Nous utilisons un peu de LINQ pour obtenir le `Prediction` dont le score le plus élevé pour le mouvement.

    S’il y a aucune correspondance de mouvement avec un haut suffisamment score, puis le Gestionnaire d’événements s’arrête sans rien faire. Dans le cas contraire, nous Vérifiez le nom de la prédiction et modifier l’image qui est affichée en fonction du nom du mouvement :

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

-   Exécutez l’application et démarrer l’activité de reconnaissance de mouvement personnalisée. Il doit ressembler à la capture d’écran suivante :

    [![Capture d’écran avec Me vérifier l’image](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Maintenant dessiner une coche sur l’écran et la bitmap affichée doit ressembler à celui illustré dans les captures d’écran suivante :

    [![Dessiné coche](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)
    [![reconnues coche](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

    Enfin, dessinez un dessin à main levée sur l’écran. La case à cocher doit modifier à son image d’origine, comme indiqué dans les captures d’écran :

    [![Dessin à main levée dans l’écran](android-touch-walkthrough-images/image22.png)](android-touch-walkthrough-images/image22.png#lightbox)
    [![image d’origine s’affiche.](android-touch-walkthrough-images/image23.png)](android-touch-walkthrough-images/image23.png#lightbox)

Vous avez maintenant comprendre comment intégrer des fonctions tactiles et des mouvements dans une application Android à l’aide de Xamarin.Android.


## <a name="related-links"></a>Liens associés

- [Android Touch Démarrer (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch finale (exemple)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
