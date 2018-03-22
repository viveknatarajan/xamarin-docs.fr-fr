---
title: "Contrôles de la caméra manuelle"
description: "Le AVFoundation Framework plus facilement les utilisateurs peuvent prendre des photos excellentes en permettant aux contrôles de la caméra manuel. À l’aide de cette infrastructure, une application peut prendre un contrôle direct sur le focus de l’appareil photo, équilibre et paramètres d’exposition. Une application peut également utiliser des captures d’exposition entre crochets capture d’images avec des paramètres différents d’exposition. Cet article prendra un coup de œil rapide à l’utilisation des contrôles caméra manuel d’une application mobile iOS simple 8."
ms.topic: article
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f11305fcbf8a5b9bf6552fa31ecfa1c0e8e7a68f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="manual-camera-controls"></a>Contrôles de la caméra manuelle

_Le AVFoundation Framework plus facilement les utilisateurs peuvent prendre des photos excellentes en permettant aux contrôles de la caméra manuel. À l’aide de cette infrastructure, une application peut prendre un contrôle direct sur le focus de l’appareil photo, équilibre et paramètres d’exposition. Une application peut également utiliser des captures d’exposition entre crochets capture d’images avec des paramètres différents d’exposition. Cet article prendra un coup de œil rapide à l’utilisation des contrôles caméra manuel d’une application mobile iOS simple 8._

Les contrôles de caméra manuel, fournis par le `AVFoundation Framework` dans iOS 8, autoriser une application mobile afin de prendre le contrôle total sur la caméra d’un appareil iOS. Ce niveau de granularité fin de contrôle peut être utilisé pour créer des applications professionnelles et de fournir des compositions d’artiste en modifiant les paramètres de la caméra tout en tirant toujours une image ou une vidéo.

Ces contrôles peuvent également être utiles lors du développement d’applications scientifiques ou industrielles, où les résultats sont adressent moins l’exactitude ou la beauté de l’image et sont plus orientés vers la mise en surbrillance d’une fonctionnalité ou un élément de l’image en tenant.

## <a name="avfoundation-capture-objects"></a>Objets de Capture de AVFoundation

Si prenant vidéo ou encore des images à l’aide de l’appareil photo sur un appareil iOS, le processus utilisé pour capturer ces images est essentiellement le même. Cela est vrai pour les applications qui utilisent les contrôles de caméra par défaut automatisée ou celles qui tirent parti des nouveaux contrôles de caméra manuel :

 [![](intro-to-manual-camera-controls-images/image1.png "Vue d’ensemble des objets de Capture AVFoundation")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Entrée est extraite d’un `AVCaptureDeviceInput` dans un `AVCaptureSession` par le biais d’un `AVCaptureConnection`. Le résultat est soit fourni sous la forme d’une image fixe ou un flux vidéo. L’ensemble du processus est contrôlé par un `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Manuel de contrôles fournis

À l’aide des nouvelles API fournies par iOS 8, l’application peut prendre le contrôle des fonctionnalités d’appareil photo suivantes :

-  **Mise au point manuelle** – en permettant à l’utilisateur final à prendre le contrôle du focus directement, une application peut fournir davantage de contrôle sur l’image effectuée.
-  **Exposition manuelle** – en fournissant l’exposition de contrôler manuellement, une application peut fournir davantage de liberté aux utilisateurs et les autoriser à obtenir une apparence stylisée.
-  **Manuel de l’équilibre blanc** – blanc solde est utilisé pour ajuster la couleur d’une image, souvent pour lui donner l’apparence réaliste. Les sources de lumière différents ont des températures de couleur différente et les paramètres de l’appareil photo utilisés pour capturer une image est ajustée pour compenser ces différences. Là encore, en permettant à l’utilisateur le contrôle sur l’équilibre des blancs, les utilisateurs peuvent effectuer des ajustements qui ne peut pas être effectuées automatiquement.


iOS 8 fournit des extensions et les améliorations apportées à iOS existant API pour fournir ce contrôle affiné sur l’image du processus de capture.

## <a name="bracketed-capture"></a>Capture entre crochets

La mise entre parenthèses de Capture est basée sur les paramètres de l’appareil photo manuel présentées ci-dessus et autorise l’application à un moment donné, de capture de plusieurs façons différentes.

En termes simples, entre parenthèses de Capture est une rafale de toujours images prises avec une variété de paramètres d’image à l’image.

## <a name="requirements"></a>Spécifications

Les éléments suivants sont requis pour terminer les étapes présentées dans cet article :

-  **Xcode 7 et iOS 8 ou version ultérieure** – Apple Xcode 7 et iOS 8 ou plus récente API doit être installés et configurés sur l’ordinateur du développeur.
-  **Visual Studio pour Mac** – la version la plus récente de Visual Studio pour Mac doit être installée et configurée sur l’appareil de l’utilisateur.
-  **iOS 8 appareil** – un appareil iOS exécutent la dernière version d’iOS 8. Fonctionnalités de l’appareil photo ne peut pas être testées dans le simulateur iOS.


## <a name="general-av-capture-setup"></a>Programme d’installation général AV Capture

Lors de l’enregistrement vidéo sur un appareil iOS, il existe un code de configuration générale qui est toujours requis. Cette section décrit le programme d’installation minimale qui est nécessaire pour enregistrer une vidéo à partir de l’appareil photo de l’appareil iOS et afficher que dans la vidéo en temps réel dans une `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Délégué de mémoire tampon de sortie exemple

Une des premières choses nécessaire sera un délégué pour surveiller la mémoire tampon de sortie de l’exemple et afficher une image saisie à partir de la mémoire tampon à un `UIImageView` dans l’interface utilisateur de l’application.

La routine suivante sera surveiller la mémoire tampon d’échantillon et mettre à jour de l’interface utilisateur :

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

Avec cette routine en place, le `AppDelegate` peut être modifié pour ouvrir une Session de Capture AV pour enregistrer un flux vidéo en direct.

### <a name="creating-an-av-capture-session"></a>Création d’une Session de Capture de l’antivirus

La session de Capture de la violation d’accès est utilisée pour contrôler l’enregistrement de la vidéo en direct à partir de l’appareil photo de l’appareil iOS et est nécessaire pour obtenir la vidéo dans une application iOS. Depuis l’exemple `ManualCameraControl` exemple d’application est à l’aide de la session de capture à différents endroits, il sera configuré dans le `AppDelegate` et sont accessibles à l’application entière.

Procédez comme suit pour modifier l’application `AppDelegate` et ajoutez le code nécessaire :


1. Double-cliquez sur le `AppDelegate.cs` fichier dans l’Explorateur de solutions pour ouvrir pour le modifier.
1. Ajoutez le code suivant à l’aide d’instructions au début du fichier :
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Ajoutez les variables privées suivantes et les propriétés calculées à la `AppDelegate` classe :
    
    ```
    #region Private Variables
    private NSError Error;
    #endregion
    
    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```
  
1. Substituez la méthode terminée et remplacez-la par :
    
    ```
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;
    
        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }
    
        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }
    
        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);
    
        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();
    
        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }
    
        // Attach input to session
        Session.AddInput (Input);
    
        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;
    
        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);
    
        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```  
  
1. Enregistrez les modifications dans le fichier.


Avec ce code en place, les contrôles de caméra manuel peut être facilement implémentées pour une expérimentation et le test.

## <a name="manual-focus"></a>Mise au point manuelle

En permettant à l’utilisateur final à prendre directement les contrôles de la vue, une application peut fournir un contrôle plus artistique sur l’image effectuée.

Par exemple, un photographe professionnel peut atténuer le focus d’une image pour obtenir un [Bokeh effet](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Un effet Bokeh")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Créer un [effet d’extraire le Focus](http://www.mediacollege.com/video/camera/focus/pull.html), telles que :

[![](intro-to-manual-camera-controls-images/image3.png "L’effet d’extraction de Focus")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Pour des chercheurs ou un générateur d’applications médicales, l’application peut souhaiter déplacer par programmation la thématique des essais. Dans les deux cas, la nouvelle API permet de mettre l’utilisateur final ou l’application de prendre le contrôle sur le focus au moment où l’image.

### <a name="how-focus-works"></a>Fonctionnement de Focus

Avant d’aborder les détails de contrôle du focus dans une application IOS 8. Jetons un œil rapide au fonctionnement du focus dans un appareil iOS :

[![](intro-to-manual-camera-controls-images/image4.png "Fonctionnement du focus dans un appareil iOS")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Passe à l’objectif de l’appareil photo sur l’appareil iOS et se concentre sur un capteur d’image. La distance de l’objectif du capteur contrôle où le Point Focal (la zone où l’image apparaît le plus nets) est en relation avec le capteur. Loin est de l’objectif du capteur, objets de distance semblent plus nets et plus près des objets semblent nettes.

Dans un appareil iOS, l’objectif est déplacé plus proche à ou éloigne le capteur par aimants et springs. Par conséquent, la position exacte de l’objectif est impossible, car elle varie d’un périphérique et peut être affecté par des paramètres tels que l’orientation de l’appareil ou l’âge de l’appareil et du ressort.

### <a name="important-focus-terms"></a>Termes du contrat de l’accent

Lors du traitement qui a le focus, il existe quelques termes que le développeur doit être familiarisé avec :

-  **Profondeur de champ** : la distance entre les objets le plus proche et le plus actif. 
-  **Macro** -il s’agit de l’extrémité proche du spectre des focus et la distance la plus proche à laquelle l’objectif peut se concentrer.
-  **L’infini** – il s’agit de l’autre extrémité du spectre le focus et la distance la plus éloignée à laquelle la thématique peut se concentrer.
-  **Hyperfocal Distance** : c’est le point dans le spectre de focus où l’objet plus éloigné dans le frame simplement à l’autre extrémité de focus. En d’autres termes, il s’agit de la position focale qui optimise la profondeur de champ. 
-  **Position de l’objectif** – c’est ce qui gère l’ensemble des éléments ci-dessus les termes. Cela correspond à la distance de l’objectif du capteur et, par conséquent le contrôleur de focus.


Avec ces termes du contrat et de la base de connaissances à l’esprit, les nouveaux contrôles de Focus manuel peut être implémentés correctement dans une application iOS 8.

### <a name="existing-focus-controls"></a>Contrôles de Focus existants

iOS 7 et versions antérieures, fourni des contrôles existants de Focus via `FocusMode`propriété en tant que :

-   `AVCaptureFocusModeLocked` – Le focus est verrouillé à un point unique de focus.
-   `AVCaptureFocusModeAutoFocus` – L’appareil photo balaye l’objectif via tous les points de référence jusqu'à ce qu’il trouve netteté et puis y reste.
-   `AVCaptureFocusModeContinuousAutoFocus` – L’appareil photo refocuses chaque fois qu’il détecte une condition hors du focus.


Les contrôles existants fournis également un point définissable d’intérêt via le`FocusPointOfInterest` propriété, afin que l’utilisateur peut appuyer pour vous concentrer sur une zone particulière. L’application permettre également suivre le mouvement en analysant le `IsAdjustingFocus` propriété.

En outre, la tranche a été fourni par le `AutoFocusRangeRestriction` propriété en tant que :

-   `AVCaptureAutoFocusRangeRestrictionNear` – Restreint la mise au point automatique à des profondeurs situés à proximité. Utile dans des situations telles que l’analyse d’un Code QR ou le code-barres.
-   `AVCaptureAutoFocusRangeRestrictionFar` – Restreint la mise au point automatique à des profondeurs géographiquement distincts. Utile dans les situations où les objets qui sont connus pour être non pertinentes sont dans le champ de vue (par exemple, un frame de fenêtre).


Enfin le `SmoothAutoFocus` propriété qui ralentit l’algorithme de focus automatique et étapes en incréments plus petits pour éviter le déplacement des artefacts lors de l’enregistrement vidéo.

### <a name="new-focus-controls-in-ios-8"></a>Nouveaux contrôles de Focus dans iOS 8

Outre les fonctionnalités fournies par iOS 7 et versions ultérieures, les fonctionnalités suivantes sont désormais disponibles pour contrôler le focus dans iOS 8 :

-  Plein contrôle manuel de la position de l’objectif lorsqu’il verrouille le focus.
-  Observation de la clé-valeur de la position de l’objectif dans n’importe quel mode focus.


Pour implémenter les fonctionnalités ci-dessus, le `AVCaptureDevice` classe a été modifiée pour inclure en lecture seule `LensPosition` propriété utilisée pour obtenir la position actuelle de l’objectif.

Pour prendre le contrôle manuel de la Position de l’objectif, le périphérique de Capture doit être en Mode Focus verrouillé. Exemple :

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

Le `SetFocusModeLocked` (méthode) du périphérique de Capture est utilisé pour ajuster la position de l’objectif. Une routine de rappel facultative peut fournir à recevoir la notification lorsque la modification prenne effet. Exemple :

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Comme indiqué dans le code ci-dessus, le périphérique de Capture doit être verrouillé pour la configuration avant de pouvoir apporter une modification de la Position de l’objectif. Les valeurs de Position de l’objectif valides sont compris entre 0,0 et 1,0.

### <a name="manual-focus-example"></a>Exemple de mise au point manuelle

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image5.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image5.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Un `UISegmentedControl` qui modifie le Mode Focus à partir d’automatique verrouillé.
-  Un `UISlider` qui permettra d’afficher et de mettre à jour la Position actuelle de la thématique.


Procédez comme suit pour la transmission du contrôleur d’affichage pour le contrôle du Focus manuel :


1. Ajoutez le code suivant à l’aide des instructions :

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Ajoutez les variables privées suivantes :

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```  
  
1. Ajoutez les propriétés calculées suivantes :

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Remplacer la `ViewDidLoad` méthode et ajoutez le code suivant :

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Remplacer la `ViewDidAppear` méthode et ajoutez le code suivant pour commencer l’enregistrement lorsque la vue de charge :

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Avec l’appareil photo dans le mode Auto, le curseur se déplace automatiquement que l’appareil photo ajuste le focus :

    [![](intro-to-manual-camera-controls-images/image6.png "Le curseur se déplace automatiquement que l’appareil photo ajuste le focus dans cet exemple d’application")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Appuyez sur le segment verrouillé et faites glisser le curseur de position pour ajuster la position de l’objectif manuellement :

    [![](intro-to-manual-camera-controls-images/image7.png "Manuellement ajuster la position de l’objectif")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Arrêtez l’application.


Le code ci-dessus a montré comment contrôler la position de l’objectif quand l’appareil est en mode automatique ou utiliser un curseur pour contrôler la position de la thématique lorsqu’il est en mode verrouillé.

## <a name="manual-exposure"></a>Exposition manuelle

Exposition fait référence à la luminosité d’une image par rapport à la luminosité de la source et est déterminée par la quantité de lumière atteint le capteur, pour la durée et par le niveau de gain de la sonde (mappage ISO). En fournissant l’exposition de contrôler manuellement, une application peut fournir davantage de liberté pour l’utilisateur final et les autoriser à obtenir une apparence stylisée.

À l’aide de contrôles exposition manuelle, l’utilisateur peut prendre une image à partir de réaliste clair à sombre et maussade :

[![](intro-to-manual-camera-controls-images/image8.png "Un exemple une image montrant l’exposition de la plus réaliste vive foncé et maussade")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Là encore, cela automatiquement à l’aide de contrôle par programmation pour les applications scientifiques ou via des contrôles manuels fournis par l’interface utilisateur des applications. Dans les deux cas, le nouveau iOS 8 exposition API permettent un contrôle précis sur les paramètres d’exposition de la caméra.

### <a name="how-exposure-works"></a>Fonctionne de l’exposition

Avant d’aborder les détails de contrôle de l’exposition d’une application IOS 8. Jetons un œil rapide au fonctionne de l’exposition :

[![](intro-to-manual-camera-controls-images/image9.png "Fonctionne de l’exposition")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Les trois éléments de base qui permettent de contrôler l’exposition sont :

-  **Vitesse d’obturation** : c’est la durée pendant laquelle le redimensionnement est ouvert pour vous permettre de lumière sur le capteur de l’appareil photo. Le plus court le temps que le redimensionnement est ouvert, moins la lumière est permettent, et le plus nette de l’image est (moins de flou directionnel). Plus le redimensionnement est ouvert, le plus clair est en et plus de mouvement flou qui se produit.
-  **Mappage de la norme ISO** – il s’agit d’un terme emprunté à partir de la photographie du film et fait référence à la sensibilité des produits chimiques du film à la lumière. Les valeurs de faible ISO de film ont moins de fragment et une reproduction des couleurs plus précie ; les valeurs ISO faibles sur les capteurs numériques ont moins de bruit capteur mais moins de luminosité. Plus la valeur ISO, l’image de la plus claire, mais avec plus de bruit de capteur. « ISO » sur un capteur numérique est une mesure de [gain électronique](http://en.wikipedia.org/wiki/Gain), pas une fonctionnalité physique. 
-  **Ouverture de l’objectif** : c’est la taille de l’ouverture de l’objectif. Sur tous les appareils iOS, l’objectif est fixe, afin de rendre les deux seules valeurs qui peuvent être utilisés pour ajuster l’exposition vitesse d’obturation et ISO.


### <a name="how-continuous-auto-exposure-works"></a>Comment continue Works d’exposition automatique

Avant d’apprendre comment exposition manuelle fonctionne, il est important de comprendre comment continue exposition auto fonctionne dans un appareil iOS.

[![](intro-to-manual-camera-controls-images/image10.png "Fonctionnement d’Exposition auto continue dans un appareil iOS")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Tout d’abord est le bloc d’exposition automatique, il dispose de la tâche de calcul exposition idéale et est transmis en continu les statistiques de contrôle. Il utilise ces informations pour calculer le mélange optimal ISO et vitesse d’obturation pour obtenir la scène bien allumée. Ce cycle correspond à la boucle AE.

### <a name="how-locked-exposure-works"></a>Comment verrouillé exposition fonctionne

Ensuite, nous allons examiner comment verrouillé works d’exposition dans les appareils iOS.

[![](intro-to-manual-camera-controls-images/image11.png "Comment verrouillé exposition fonctionne dans les appareils iOS")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Là encore, vous avez le bloc d’exposition automatique qui tente de calculer les e/s optimales et les valeurs de durée. Toutefois, dans ce mode, le bloc AE est déconnecté du moteur de contrôle des statistiques.

### <a name="existing-exposure-controls"></a>Exposition des contrôles existants

iOS 7 et au-dessus, fournissent des contrôles existants suivants exposition via le `ExposureMode` propriété :

-   `AVCaptureExposureModeLocked` – Exemples de la scène qu’une seule fois et utilise ces valeurs dans l’ensemble de la scène.
-   `AVCaptureExposureModeContinuousAutoExposure` – Exemples de la scène en permanence pour vous assurer qu’il est bien allumé.


Le `ExposurePointOfInterest` peut être utilisé pour appuyer pour exposer la scène en sélectionnant un objet cible pour exposer sur, et l’application peut surveiller le `AdjustingExposure` propriété pour voir quand exposition est répartie.

### <a name="new-exposure-controls-in-ios-8"></a>Nouveaux contrôles d’exposition dans iOS 8

Outre les fonctionnalités fournies par iOS 7 et versions ultérieures, les fonctionnalités suivantes sont désormais disponibles pour contrôler l’exposition dans iOS 8 :

-  Exposition de personnalisée entièrement manuelle.
-  Get, Set et clé-valeur observer IOS et la vitesse d’obturation (durée).


Pour implémenter les fonctionnalités ci-dessus, un nouveau `AVCaptureExposureModeCustom` mode a été ajouté. Lorsque l’appareil photo dans est le mode personnalisé, le code suivant peut être utilisé pour ajuster la durée d’exposition et ISO :

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

Dans les modes Auto et verrouillé, l’application peut ajuster le décalage de la routine d’exposition automatique utilisant le code suivant :

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Les plages minimale et maximale de paramètre dépendent de l’appareil de que l’application est en cours d’exécution, ils doivent jamais être codées en dur. Au lieu de cela, utilisez les propriétés suivantes pour obtenir les plages de valeurs minimale et maximale :

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Comme indiqué dans le code ci-dessus, le périphérique de Capture doit être verrouillé pour la configuration avant de pouvoir apporter une modification de l’exposition.

### <a name="manual-exposure-example"></a>Exemple d’exposition manuelle

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image12.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image12.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Un `UISegmentedControl` qui modifie le Mode Focus à partir d’automatique verrouillé.
-  Quatre `UISlider` contrôles qui seront affichent et mettre à jour le décalage, la durée, ISO et décalage.


Procédez comme suit pour câble du contrôleur d’affichage pour le contrôle de l’exposition manuelle :


1. Ajoutez le code suivant à l’aide des instructions :
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Ajoutez les variables privées suivantes :

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```  
  
1. Ajoutez les propriétés calculées suivantes :

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Remplacer la `ViewDidLoad` méthode et ajoutez le code suivant :

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;
    
        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;
    
        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });
    
            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });
    
            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });
    
            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        ISO.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        Bias.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Remplacer la `ViewDidAppear` méthode et ajoutez le code suivant pour commencer l’enregistrement lorsque la vue de charge :

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Avec l’appareil photo dans le mode Auto, les curseurs déplacera automatiquement comme la caméra ajuste l’exposition :

    [![](intro-to-manual-camera-controls-images/image13.png "Les curseurs seront déplace automatiquement comme la caméra ajuste l’exposition")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Appuyez sur le segment verrouillé et faites glisser le curseur de décalage pour ajuster le décalage de l’exposition automatique manuellement :

    [![](intro-to-manual-camera-controls-images/image14.png "Ajuster le décalage de l’exposition automatique manuellement")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Appuyez sur le segment personnalisé, puis faites glisser les curseurs ISO et la durée pour contrôler manuellement l’exposition :

    [![](intro-to-manual-camera-controls-images/image15.png "Faites glisser les curseurs ISO et la durée pour contrôler manuellement l’exposition")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Arrêtez l’application.


Le code ci-dessus a montré comment surveiller les paramètres de l’exposition lorsque la caméra est en mode automatique et comment utiliser les curseurs pour contrôler l’exposition lorsqu’il est en mode verrouillé ou personnalisé.

## <a name="manual-white-balance"></a>Équilibre manuel du blanc

Équilibre des blancs contrôles permettent aux utilisateurs d’ajuster le solde de colosr dans une image pour qu’elles apparaissent plus réaliste. Les sources de lumière différents ont des températures de couleur différente, et les paramètres de l’appareil photo utilisés pour capturer une image doivent être ajustées pour compenser ces différences. Là encore, en permettant à l’utilisateur le contrôle sur l’équilibre des blancs s’ajustements professionnels qui les routines automatique sont incapables de pour obtenir des effets artistiques.

[![](intro-to-manual-camera-controls-images/image16.png "Un exemple d’image montrant les réglages de l’équilibre des blancs manuel")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Par exemple, l’heure d’été a un cast blueish, tandis que les lumières incandescentes tungstène comportent une teinte warmer orange-jaune. (Qui prête à confusion, les couleurs « froides » ont des températures de couleur plus élevées que les couleurs « actif ». Couleur températures sont une mesure physique, pas perceptible dans un index.)

L’esprit humain est très efficace pour compenser les différences de température de couleur, mais il s’agit d’un élément qui a un appareil photo ne peut pas faire. L’appareil photo fonctionne en renforçant les couleurs sur le spectre opposé à des différences de couleur.

Le nouveau iOS 8 exposition API permet à l’application prendre le contrôle du processus et permettent un contrôle précis sur les paramètres de l’équilibre des blancs de la caméra.

### <a name="how-white-balance-works"></a>Comment fonctionne de solde

Avant d’aborder les détails de contrôle de l’équilibre des blancs dans une application IOS 8. Jetons un œil rapide comment équilibrer Works :

Dans l’étude de la perception de la couleur, la [CEI 1931 RVB des couleurs espace et l’espace de couleurs CEI 1931 XYZ](http://en.wikipedia.org/wiki/CIE_1931_color_space) sont le premier mathématiquement les espaces de couleurs. Ils ont été créés par la Commission internationale de l’éclairage (CIE) dans 1931.

[![](intro-to-manual-camera-controls-images/image17.png "Espace de couleurs de l’espace colorimétrique CEI 1931 RVB et CEI 1931 XYZ")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Le graphique ci-dessus montre toutes les couleurs visible à le œil humain, à partir des niveaux bleu vert vif rouge clair. N’importe quel point dans le diagramme peut être tracée avec une valeur de X et Y, comme indiqué dans le graphique ci-dessus.

Comme visible dans le graphique, il existe des valeurs X et Y peuvent être tracées sur le graphique qui est en dehors de la plage de la vision humaine, et par conséquent ces couleurs ne peut pas être reproduits par un appareil photo.

La courbe plus petite dans le graphique ci-dessus est appelée le [Planckian Locus](http://en.wikipedia.org/wiki/Planckian_locus), qui exprime la température de couleur (en degrés kelvin), avec les numéros les plus élevés sur le côté bleu (chaud) et inférieur des chiffres sur le côté rouge (refroidissement). Ils sont utiles pour des situations d’éclairage classique.

Dans les conditions d’éclairage mixte, les réglages de l’équilibre des blancs devez écarter le Planckian Locus pour apporter les modifications requises. Dans ces situations, que l’ajustement doit être décalés à vert ou rouge/magenta côté la CEI l’échelle.

les appareils iOS compensent des casts de couleur en renforçant le gain de couleur opposé. Par exemple, si une séquence comporte trop de bleu, le gain rouge augmenté pour compenser. Ces valeurs sont étalonnés pour des périphériques spécifiques afin qu’ils soient dépend du périphérique de gain.

### <a name="existing-white-balance-controls"></a>Contrôles de l’équilibre des blancs existants

iOS 7 et ultérieur fourni les contrôles blanc solde existants suivants via `WhiteBalanceMode` propriété :

-   `AVCapture WhiteBalance ModeLocked` – Exemples de la scène en une seule fois et à l’aide de ces valeurs dans l’ensemble de la scène.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – Exemples de la scène en permanence pour vous assurer qu’il est bien équilibré.


Et l’application peut surveiller le `AdjustingWhiteBalance` propriété pour voir quand exposition est répartie.

### <a name="new-white-balance-controls-in-ios-8"></a>Nouveaux contrôles de solde blanc dans iOS 8

Outre les fonctionnalités fournies par iOS 7 et versions ultérieures, les fonctionnalités suivantes sont désormais disponibles pour le contrôle blanc solde dans iOS 8 :

-  Prend le contrôle entièrement manuels de l’appareil RVB.
-  Get, Set et clé-valeur observer l’appareil RVB gains.
-  Prise en charge de solde blanc à l’aide d’une carte de gris.
-  Routines de conversion vers et depuis les espaces de couleurs indépendants de périphérique.


Pour implémenter les fonctionnalités ci-dessus, le `AVCaptureWhiteBalanceGain` structure a été ajoutée avec les membres suivants :

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Le gain de l’équilibre des blancs maximum est actuellement quatre (4) et peut être prêt à partir de la `MaxWhiteBalanceGain` propriété. Par conséquent, la plage autorisée est d’un (1) à `MaxWhiteBalanceGain` (4) actuellement.

Le `DeviceWhiteBalanceGains` propriété peut être utilisée pour observer les valeurs actuelles. Utilisez `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` pour ajuster le solde gains lorsque la caméra est en mode verrouillé équilibre des blancs.

#### <a name="conversion-routines"></a>Routines de conversion

Routines de conversion ont été ajoutés pour iOS 8 à l’aide de la conversion vers et à partir, les espaces de couleurs indépendants de périphérique. Pour implémenter les routines de conversion, la `AVCaptureWhiteBalanceChromaticityValues` structure a été ajoutée avec les membres suivants :

-   `X` -est une valeur comprise entre 0 et 1.
-   `Y` -est une valeur comprise entre 0 et 1.


Un `AVCaptureWhiteBalanceTemperatureAndTintValues` structure a également été ajoutée avec les membres suivants :

-   `Temperature` -flottante point la valeur en degrés Kelvin.
-   `Tint` -est un décalage de vert ou magenta de 0 à 150 avec des valeurs positives vers la direction de verte et négatifs vers le magenta.


Utilisez le `CaptureDevice.GetTemperatureAndTintValues`et `CaptureDevice.GetDeviceWhiteBalanceGains`méthodes de conversion entre la température et de teinte, de boutons et de RVB obtenir des espaces de couleurs.

> [!NOTE]
> Les routines de conversion sont plus précises, la valeur à convertir est proche au locus Planckian.




#### <a name="gray-card-support"></a>Prise en charge de la carte en gris

Apple utilise le terme gris World pour faire référence à la prise en charge de la carte de gris intégrée iOS 8. Il permet à l’utilisateur pour vous concentrer sur la carte physique gris qui couvre au moins 50 % du centre de l’image et l’utilise pour ajuster l’équilibre des blancs. L’objectif de la carte en gris est atteindre blanc apparaît neutre.

Cela peut être implémenté dans une application en invitant l’utilisateur à placer une carte grise physique devant l’appareil photo, analyse le `GrayWorldDeviceWhiteBalanceGains` propriété et en attente jusqu'à ce que les valeurs régler vers le bas.

L’application serait alors la verrouiller les gains de solde de blanc pour la `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` méthode en utilisant les valeurs à partir de la `GrayWorldDeviceWhiteBalanceGains` propriété pour appliquer les modifications.

Le périphérique de Capture doit être verrouillé pour la configuration avant de pouvoir apporter une modification de l’équilibre des blancs.

### <a name="manual-white-balance-example"></a>Exemple d’équilibre manuel du blanc

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image18.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image18.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Un `UISegmentedControl` qui modifie le Mode Focus à partir d’automatique verrouillé.
-  Deux `UISlider` contrôles qui seront affichent et mettre à jour la température et la teinte.
-  A `UIButton` utilisé pour définir le solde blanc à l’aide de ces valeurs et un espace gris carte (monde en gris).


Procédez comme suit pour câble du contrôleur de l’affichage de contrôle du solde blanc manuel :


1. Ajoutez le code suivant à l’aide des instructions :

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Ajoutez les variables privées suivantes :

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    #endregion
    ```
  
1. Ajoutez les propriétés calculées suivantes :

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Ajoutez la méthode privée suivante pour définir le nouveau blanc équilibrer la température et teinte :

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }
    
    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```   
  
1. Remplacer la `ViewDidLoad` méthode et ajoutez le code suivant :

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ``` 
  
1. Remplacer la `ViewDidAppear` méthode et ajoutez le code suivant pour commencer l’enregistrement lorsque la vue de charge :

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Enregistrer les modifications du code et exécuter l’application.
1. Avec l’appareil photo dans le mode Auto, les curseurs déplacera automatiquement que l’appareil photo règle blancs :

    [![](intro-to-manual-camera-controls-images/image19.png "Les curseurs seront déplace automatiquement comme la caméra ajuste l’équilibre des blancs")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Appuyez sur le segment verrouillé et faites glisser les curseurs Temp et teinte pour ajuster l’équilibre des blancs manuellement :

    [![](intro-to-manual-camera-controls-images/image20.png "Faites glisser les curseurs Temp et teinte pour ajuster l’équilibre des blancs manuellement")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Avec le segment verrouillé étant toujours sélectionné, placez une carte physique grise au premier plan de l’appareil photo et appuyez sur le bouton de la carte de gris pour ajuster l’équilibre des blancs au monde gris :

    [![](intro-to-manual-camera-controls-images/image21.png "Appuyez sur le bouton de la carte de gris pour ajuster l’équilibre des blancs au monde en gris")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Arrêtez l’application.

Le code ci-dessus a montré comment surveiller les paramètres de l’équilibre des blancs lorsque la caméra est en mode automatique, ou utilisez les curseurs pour contrôler l’équilibre des blancs lorsqu’il est en mode verrouillé.

## <a name="bracketed-capture"></a>Capture entre crochets

La mise entre parenthèses de Capture est basée sur les paramètres de l’appareil photo manuel présentées ci-dessus et autorise l’application à un moment donné, de capture de plusieurs façons différentes.

En termes simples, entre parenthèses de Capture est une rafale de toujours images prises avec une variété de paramètres d’image à l’image.

[![](intro-to-manual-camera-controls-images/image22.png "Comment fonctionne la mise entre parenthèses de Capture")](intro-to-manual-camera-controls-images/image22.png#lightbox)

À l’aide de la mise entre parenthèses de Capture dans iOS 8, une application peut prédéfinir une série de contrôles de la caméra manuelle, émettre une commande unique et un ont la séquence actuelle retourner une série d’images pour chacune des présélections manuelles.

### <a name="bracketed-capture-basics"></a>Principes de base entre parenthèses de Capture

Là encore, la mise entre parenthèses de Capture est une rafale de toujours images prises avec différents paramètres à partir de l’image à l’image. Les types de mise entre parenthèses de Capture disponibles sont :

-  **Auto exposition crochet** – où toutes les images ont une durée de décalage variée.
-  **Crochet d’exposition manuelle** – où toutes les images ont une vitesse d’obturation (durée) et l’ISO variés quantité.
-  **Crochet de croître simple** – une série d’images fixes prises de suite.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Nouvelle mise entre parenthèses de Capture des contrôles dans iOS 8

Toutes les commandes de mise entre parenthèses de Capture sont implémentées dans la `AVCaptureStillImageOutput` classe. Utilisez le `CaptureStillImageBracket`pour obtenir une série d’images avec le tableau donné de paramètres de méthode.

Deux nouvelles classes ont été implémentées pour gérer les paramètres :

-   `AVCaptureAutoExposureBracketedStillImageSettings` : Il possède une propriété, `ExposureTargetBias`, utilisé pour définir le décalage pour un crochet d’Exposition auto. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` : Il a deux propriétés, `ExposureDuration` et `ISO`, utilisé pour définir la vitesse d’obturation et ISO pour un crochet exposition manuelle. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>Contrôle de Capture entre crochets choses à faire et

#### <a name="dos"></a>Choses à faire

Voici une liste d’éléments qui doivent être effectués lors de contrôles à l’aide de la mise entre parenthèses de Capture dans iOS 8 :

-  Préparer l’application pour le cas le plus défavorable capture en appelant le `PrepareToCaptureStillImageBracket` (méthode).
-  Supposons que les mémoires tampons exemple allez proviennent du même pool partagé.
-  Pour libérer la mémoire allouée par un appel précédent de la préparation, appelez `PrepareToCaptureStillImageBracket` à nouveau et l’envoyer à un tableau d’un objet.


#### <a name="donts"></a>Avertissements

Voici une liste d’éléments qui ne doit pas être effectuée lors de contrôles à l’aide de la mise entre parenthèses de Capture dans iOS 8 :

-  Ne mélangez pas mise entre parenthèses de Capture des types de paramètres dans une seule capture.
-  Ne pas demander plus de `MaxBracketedCaptureStillImageCount` images dans une seule capture.


### <a name="bracketed-capture-details"></a>Détails de la Capture entre crochets

Les informations suivantes doivent être prises en considération lorsque vous travaillez avec la mise entre parenthèses de Capture dans iOS 8 :

-  Remplacement temporaire des paramètres entre crochets le `AVCaptureDevice` paramètres.
-  Flash et paramètres de stabilisation d’image sont ignorés.
-  Toutes les images doivent utiliser le même format de sortie (jpeg, png, etc.).
-  Aperçu vidéo peut supprimer des images.
-  Capture entre crochets est prise en charge sur tous les appareils compatibles avec iOS 8.


Avec ces informations à l’esprit, examinons un exemple d’utilisation de la mise entre parenthèses de Capture dans iOS 8.

### <a name="bracket-capture-example"></a>Exemple de Capture de support

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image23.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image23.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Trois `UIImageViews` qui affiche les résultats de la capture.
-  A `UIScrollView` pour héberger le flux vidéo et des vues de résultat.
-  A `UIButton` utilisé pour prendre une Capture mise entre parenthèses avec des paramètres prédéfinis.


Procédez comme suit pour la transmission du contrôleur d’affichage pour la Capture de la mise entre parenthèses :


1. Ajoutez le code suivant à l’aide des instructions :

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```  
  
1. Ajoutez les variables privées suivantes :

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```    
  
1. Ajoutez les propriétés calculées suivantes :

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. Ajoutez la méthode privée suivante pour générer les vues d’image de sortie :

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {
    
        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));
    
        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");
    
        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);
    
        // Add to scrolling view
        ScrollView.AddSubview (imageView);
    
        // Return new image view
        return imageView;
    }
    #endregion
    ```  
  
1. Remplacer la `ViewDidLoad` méthode et ajoutez le code suivant :
    
    ```
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);
    
        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));
    
        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };
    
        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;
    
            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });
    
            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);
    
                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);
    
                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```
    
  
1. Remplacer la `ViewDidAppear` méthode et ajoutez le code suivant :

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
        }
    }
    
    ```  
    
1. Enregistrer les modifications du code et exécuter l’application.
1. Image d’une scène et appuyez sur le bouton capturer le crochet :

    [![](intro-to-manual-camera-controls-images/image24.png "Image d’une scène et appuyez sur le bouton capturer le crochet")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Effectuez un balayage de droite à gauche pour voir les trois images pris par la mise entre parenthèses de Capture :

    [![](intro-to-manual-camera-controls-images/image25.png "Effectuez un balayage de droite à gauche pour voir les trois images pris par la mise entre parenthèses de Capture")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Arrêtez l’application.


Le code ci-dessus a montré comment configurer et prendre une exposition automatique mise entre parenthèses capturer dans iOS 8.

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons couvert une introduction aux nouveaux contrôles d’appareil photo manuel fourni par iOS 8 et couvert les principes fondamentaux de ce qu’elles font et comment elles fonctionnent. Nous avons fourni des exemples point manuelle, exposition manuelle et équilibre des blancs manuel. Enfin, nous avons attribué à un exemple qui prend un mise entre parenthèses de Capture avec les contrôles de caméra manuelle décrite précédemment

## <a name="related-links"></a>Liens associés

- [ManualCameraControls (exemple)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Introduction à iOS 8](~/ios/platform/introduction-to-ios8.md)
