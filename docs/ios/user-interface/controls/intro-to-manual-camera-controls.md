---
title: Contrôles de la caméra manuelle dans Xamarin.iOS
description: Ce document décrit comment le framework de AVFoundation iOS peut servir avec Xamarin.iOS pour activer les contrôles de la caméra manuelle. Contrôles de la caméra manuelle autoriser un utilisateur au focus du contrôle, équilibre et paramètres d’exposition.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 84c4b699ba2c046eeb70963f3df71ca9a4760f3b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104178"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Contrôles de la caméra manuelle dans Xamarin.iOS

Les contrôles de caméra manuel, fournis par le `AVFoundation Framework` dans iOS 8, autoriser une application mobile prendre le contrôle total sur l’appareil photo d’un appareil iOS. Ce niveau de granularité fin de contrôle peut être utilisé pour créer des applications professionnelles et de fournir des compositions d’artiste en ajustant les paramètres de l’appareil photo lors de la prise d’une image fixe ou une vidéo.

Ces contrôles peuvent également être utiles lors du développement d’applications scientifiques ou industrielles, où les résultats sont moins destinés à l’exactitude ou de la beauté de l’image et sont plus orientés vers la mise en surbrillance d’une fonctionnalité ou un élément de l’image en tenant.

## <a name="avfoundation-capture-objects"></a>Objets de Capture de AVFoundation

Si prend une vidéo ou encore des images à l’aide de l’appareil photo sur un appareil iOS, le processus utilisé pour capturer ces images est en grande partie les mêmes. Cela est vrai pour les applications qui utilisent les contrôles de la caméra automatisée par défaut ou celles qui tirent parti des nouveaux contrôles photo manuel :

 [![](intro-to-manual-camera-controls-images/image1.png "Vue d’ensemble des objets de Capture AVFoundation")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Entrée est extraite d’un `AVCaptureDeviceInput` dans un `AVCaptureSession` par l’intermédiaire d’un `AVCaptureConnection`. Le résultat est une sortie sous la forme d’une image fixe ou un flux vidéo. L’ensemble du processus est contrôlé par un `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Contrôles manuels fournis

À l’aide des nouvelles API fournis par iOS 8, l’application peut prendre le contrôle des fonctionnalités suivantes de l’appareil photo :

-  **Mise au point manuelle** – en autorisant l’utilisateur final à prendre le contrôle de la vue directement, une application peut fournir davantage de contrôle sur l’image extraite.
-  **Manuelle exposition** – en fournissant un contrôle manuel sur l’exposition, une application peut fournir davantage de liberté aux utilisateurs et leur permettre d’atteindre un aspect stylisé.
-  **Manuel de l’équilibre blanc** – solde de blanc est utilisé pour ajuster la couleur dans une image, souvent pour lui donner un aspect réaliste. Différentes sources de lumière ont des températures de couleur différente, et les paramètres de l’appareil photo utilisés pour capturer une image est ajustée pour compenser ces différences. Là encore, en permettant à l’utilisateur de contrôler l’équilibre des blancs, les utilisateurs peuvent effectuer des ajustements qui ne peut pas être effectuées automatiquement.


iOS 8 fournit des extensions et les processus de capture des améliorations apportées aux API pour fournir ce contrôle affiné sur l’image pour iOS existants.

## <a name="bracketed-capture"></a>Capture entre crochets

La mise entre parenthèses de Capture est basée sur les paramètres des contrôles photo manuel présenté ci-dessus et permet à l’application capturer un moment donné, dans un large éventail de différentes façons.

En termes simples, mise entre parenthèses de Capture est une rafale de toujours images prises avec un large éventail de paramètres d’image à l’image.

## <a name="requirements"></a>Configuration requise

Les éléments suivants sont requis pour effectuer les étapes présentées dans cet article :

-  **Xcode 7 + et iOS 8 ou version ultérieure** – du Apple Xcode 7 et iOS 8 ou API plus récentes doivent être installés et configurés sur l’ordinateur du développeur.
-  **Visual Studio pour Mac** – la dernière version de Visual Studio pour Mac doit être installée et configurée sur l’appareil de l’utilisateur.
-  **iOS 8 appareil** : un appareil iOS en cours d’exécution la dernière version d’iOS 8. Fonctionnalités de l’appareil photo ne peut pas être testées dans le simulateur iOS.


## <a name="general-av-capture-setup"></a>Programme d’installation général AV Capture

Lors de l’enregistrement vidéo sur un appareil iOS, il est un code de configuration générale qui est toujours requis. Cette section décrit le programme d’installation minimale qui est nécessaire pour enregistrer une vidéo à partir de l’appareil photo de l’appareil iOS et afficher cette vidéo en temps réel dans un `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Délégué de mémoire tampon de sortie exemple

Une des premières choses que nécessaire sera un délégué pour surveiller la mémoire tampon de sortie de l’exemple et afficher une image saisie à partir de la mémoire tampon à un `UIImageView` dans l’interface utilisateur de l’application.

La routine suivante sera surveiller la mémoire tampon d’exemple et mettre à jour de l’interface utilisateur :

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

Avec cette routine en place, le `AppDelegate` peut être modifié pour ouvrir une Session de Capture de violation d’accès pour enregistrer un flux vidéo en direct.

### <a name="creating-an-av-capture-session"></a>Création d’une Session de Capture de l’antivirus

La session de Capture de violation d’accès est utilisée pour contrôler l’enregistrement de la vidéo en direct à partir de l’appareil photo de l’appareil iOS et est nécessaire pour obtenir la vidéo dans une application iOS. Depuis l’exemple `ManualCameraControl` exemple d’application est à l’aide de la session de capture à différents endroits, il sera configuré dans le `AppDelegate` et mis à disposition à l’application entière.

Procédez comme suit pour modifier l’application `AppDelegate` et ajoutez le code nécessaire :


1. Double-cliquez sur le `AppDelegate.cs` fichier dans l’Explorateur de solutions pour l’ouvrir pour modification.
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
  
1. Substituez la méthode terminée et remplacez-le par :
    
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


Avec ce code en place, les contrôles de la caméra manuel peut être facilement implémentées pour l’expérimentation et de test.

## <a name="manual-focus"></a>Mise au point manuelle

En permettant à l’utilisateur final à prendre directement les contrôles du focus, une application peut fournir un contrôle plus artistique sur l’image extraite.

Par exemple, un photographe professionnel peut atténuer le focus d’une image pour obtenir un [Bokeh effet](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Un effet Bokeh")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Ou bien, créez un [effet d’extraire le Focus](http://www.mediacollege.com/video/camera/focus/pull.html), telles que :

[![](intro-to-manual-camera-controls-images/image3.png "L’effet d’extraction de Focus")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Pour les scientifiques ou un writer d’applications médicales, l’application peut souhaiter déplacer par programmation la glace des expériences. Dans les deux cas, la nouvelle API permet à l’utilisateur final ou l’application de prendre le contrôle sur le focus au moment où l’image est effectuée.

### <a name="how-focus-works"></a>Fonctionnement de Focus

Avant d’aborder les détails de contrôle du focus dans une application IOS 8. Jetons un œil rapide à la façon dont le focus fonctionne sur un appareil iOS :

[![](intro-to-manual-camera-controls-images/image4.png "Fonctionnement de focus sur un appareil iOS")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Lumière entre l’objectif de l’appareil photo sur l’appareil iOS et se concentre sur un capteur d’image. La distance du filtre à partir du capteur de contrôle où le Point Focal (la zone où l’image apparaît le plus prononcé) est en relation avec le capteur. Loin de l’objectif est à partir du capteur, objets de distance semblent plus prononcés et la plus proche de près les objets semblent plus prononcé.

Dans un appareil iOS, l’objectif est déplacé plus près à, ou plus éloigné, le capteur par aimants et springs. Par conséquent, la position exacte de l’objectif est impossible, car elle varie d’un périphérique à l’autre et peut être affectée par les paramètres tels que l’orientation de l’appareil ou l’âge de l’appareil et le ressort.

### <a name="important-focus-terms"></a>Termes du contrat de l’accent

Lors du traitement de focus, il existe quelques termes que le développeur doit connaître :

-  **Profondeur de champ** : distance entre les objets le plus proche et le plus actif. 
-  **Macro** -il s’agit de la proche de la fin de la gamme de focus et correspond à la distance le plus proche à laquelle le filtre peut se concentrer.
-  **L’infini** – il s’agit de l’autre extrémité du spectre le focus et est la distance la plus éloignée à laquelle le filtre peut se concentrer.
-  **Distance de Hyperfocal** – il s’agit du point dans le spectre de focus où l’objet plus éloigné dans le frame est seulement à l’autre extrémité de focus. En d’autres termes, il s’agit de la position focale qui optimise la profondeur de champ. 
-  **Position de l’objectif** – c’est ce qui contrôle tous les éléments ci-dessus les termes. Cela correspond à la distance du filtre à partir du capteur et consomme donc du contrôleur de focus.


Avec ces termes et les connaissances à l’esprit, les nouveaux contrôles de Focus manuel peut être implémentés avec succès dans une application iOS 8.

### <a name="existing-focus-controls"></a>Le Focus des contrôles existants

iOS 7 et versions antérieures, fourni des contrôles existants de Focus via `FocusMode`propriété en tant que :

-   `AVCaptureFocusModeLocked` – Le focus est verrouillé à un point unique de focus.
-   `AVCaptureFocusModeAutoFocus` – L’appareil photo balaye l’objectif via tous les points focal jusqu'à ce qu’il trouve la netteté et puis y reste.
-   `AVCaptureFocusModeContinuousAutoFocus` – L’appareil photo refocuses lorsqu’il détecte une condition hors du focus.


Les contrôles existants fournis également un point définissable d’intérêt via le`FocusPointOfInterest` propriété, afin que l’utilisateur peut appuyer pour se concentrer sur une zone particulière. L’application peut également suivre le mouvement de filtre en surveillant le `IsAdjustingFocus` propriété.

En outre, la restriction de plage a été fournie par le `AutoFocusRangeRestriction` propriété en tant que :

-   `AVCaptureAutoFocusRangeRestrictionNear` – Restreint l’autofocus à des profondeurs à proximité. Utile dans les situations telles que l’analyse d’un Code QR ou le code-barres.
-   `AVCaptureAutoFocusRangeRestrictionFar` – Restreint l’autofocus à des profondeurs éloignées. Utile dans les situations où les objets qui sont connus comme étant non pertinentes sont dans le champ de vision (par exemple, un frame de fenêtre).


Enfin le `SmoothAutoFocus` propriété qui ralentit l’algorithme de focus automatique et étapes en incréments plus petits afin d’éviter le déplacement des artefacts lors de l’enregistrement vidéo.

### <a name="new-focus-controls-in-ios-8"></a>Nouveaux contrôles de Focus dans iOS 8

Outre les fonctionnalités déjà fournies par iOS 7 et versions ultérieures, les fonctionnalités suivantes sont désormais disponibles pour contrôler le focus dans iOS 8 :

-  Contrôle entièrement manuelle de la position de l’objectif lorsqu’il verrouille le focus.
-  Observation clé-valeur de la position d’objectif dans n’importe quel mode focus.


Pour implémenter les fonctionnalités ci-dessus, le `AVCaptureDevice` classe a été modifiée pour inclure en lecture seule `LensPosition` propriété permettant d’obtenir la position actuelle de l’objectif.

Pour prendre le contrôle manuel de la Position de l’objectif, le périphérique de Capture doit être dans le Mode Focus verrouillé. Exemple :

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

Le `SetFocusModeLocked` méthode de l’appareil de Capture est utilisée pour ajuster la position de l’objectif. Une routine de rappel facultative peut fournir à recevoir la notification lorsque la modification prenne effet. Exemple :

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Comme indiqué dans le code ci-dessus, le périphérique de Capture doit être verrouillé pour la configuration avant d’effectuer une modification de la Position de l’objectif. Les valeurs de Position de filtre valides sont entre 0,0 et 1,0.

### <a name="manual-focus-example"></a>Exemple de mise au point manuelle

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image5.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image5.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Un `UISegmentedControl` qui modifie le Mode Focus à partir d’automatique sur verrouillé.
-  Un `UISlider` qui affiche et mettre à jour la Position actuelle de filtre.


Procédez comme suit pour AutoEventWireup le contrôleur d’affichage pour le contrôle du Focus manuel :


1. Ajoutez le code suivant à l’aide d’instructions :

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
  
1. Remplacer le `ViewDidLoad` méthode et ajoutez le code suivant :

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
  
1. Remplacer le `ViewDidAppear` méthode et ajoutez le code suivant pour démarrer l’enregistrement lorsque la vue chargée :

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
  
1. Avec l’appareil photo dans le mode Auto, le curseur passe automatiquement comme la caméra ajuste le focus :

    [![](intro-to-manual-camera-controls-images/image6.png "Le curseur se déplace d’automatiquement lorsque l’appareil photo ajuste le focus dans cet exemple d’application")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Appuyez sur le segment verrouillé et faites glisser le curseur de position pour ajuster la position de l’objectif manuellement :

    [![](intro-to-manual-camera-controls-images/image7.png "Ajuster manuellement la position de l’objectif")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Arrêter l’application.


Le code ci-dessus a montré comment faire pour surveiller la position de filtre lors de l’appareil photo est en mode automatique ou utiliser un curseur pour contrôler la position de lentille lorsqu’il est en mode verrouillé.

## <a name="manual-exposure"></a>Exposition manuelle

Exposition fait référence à la luminosité d’une image par rapport à la luminosité de la source et est déterminée par la quantité de lumière atteint le capteur, pour la durée et par le niveau de gain de la sonde (mappage ISO). En fournissant un contrôle manuel sur l’exposition, une application peut fournir davantage de liberté pour l’utilisateur final et les autoriser à obtenir une apparence stylisée.

À l’aide de contrôles manuelles exposition, l’utilisateur de prendre une image à partir d’anormalement clair à sombre et maussade :

[![](intro-to-manual-camera-controls-images/image8.png "Un exemple image illustrant exposition à des anormalement lumineux vers foncé et maussade")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Là encore, cela est possible automatiquement à l’aide de contrôle par programmation pour les applications scientifiques ou via les contrôles manuels fournis par l’interface utilisateur des applications. Dans les deux cas, la nouvelle iOS 8 exposition API fournissent un contrôle précis sur les paramètres d’exposition de la caméra.

### <a name="how-exposure-works"></a>Fonctionne de l’exposition

Avant d’aborder les détails de contrôle de l’exposition d’une application IOS 8. Jetons un œil rapide au fonctionne de l’exposition :

[![](intro-to-manual-camera-controls-images/image9.png "Fonctionne de l’exposition")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Les trois éléments de base qui sont combinent pour contrôler l’exposition sont :

-  **Vitesse d’obturation** – il s’agit la durée pendant laquelle l’obturateur est ouvert pour vous permettre de lumière sur le capteur de caméra. La plus courte lors de que l’obturateur est ouvert, moins la lumière est permettent et la plus nette de l’image est (moins de flou de mouvement). Plus l’obturateur est ouvert, le plus clair est en et plus de mouvement flou qui se produit.
-  **Mappage de ISO** – il s’agit d’un terme emprunté à partir de la photographie du film et fait référence à la sensibilité des produits chimiques du film à la lumière. Les valeurs de faible ISO de film ont moins de granularité et plus fine reproduction de couleur ; des valeurs faibles ISO sur capteurs numériques ont moins de bruit capteur mais moins luminosité. Plus la valeur ISO, plus l’image sera lumineux, mais avec plus de bruit de capteur. « ISO » sur un capteur numérique est une mesure de [gain électronique](http://en.wikipedia.org/wiki/Gain), pas une fonctionnalité physique. 
-  **Ouverture de l’objectif** – c’est la taille de l’ouverture de l’objectif. Sur tous les appareils iOS, l’objectif est fixe, par conséquent, les deux seules valeurs qui peuvent être utilisées pour ajuster l’exposition sont la vitesse d’obturation et ISO.


### <a name="how-continuous-auto-exposure-works"></a>Comment continue fonctionne d’exposition automatique

Avant d’apprendre comment manuelle exposition fonctionne, il est important de comprendre comment continue exposition automatique fonctionne dans un appareil iOS.

[![](intro-to-manual-camera-controls-images/image10.png "Fonctionnement d’exposition automatique continue dans un appareil iOS")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Tout d’abord est le bloc d’exposition automatique, il a le travail de calcul exposition idéale et est chargé en continu les statistiques de contrôle. Il utilise ces informations pour calculer le mélange optimal de ISO et vitesse d’obturation pour obtenir la scène bien allumée. Ce cycle est appelé la boucle AE.

### <a name="how-locked-exposure-works"></a>Comment verrouillé exposition Works

Ensuite, nous allons examiner comment verrouillé exposition fonctionne dans les appareils iOS.

[![](intro-to-manual-camera-controls-images/image11.png "Comment verrouillé exposition fonctionne dans les appareils iOS")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Là encore, vous avez le bloc d’exposition automatique qui tente de calculer les iOS optimale et les valeurs de durée. Toutefois, dans ce mode, le bloc AE est déconnecté à partir du moteur de statistiques de contrôle.

### <a name="existing-exposure-controls"></a>Exposition des contrôles existants

iOS 7 et fournissez ci-dessus, les contrôles d’exposition existants suivants via le `ExposureMode` propriété :

-   `AVCaptureExposureModeLocked` – Exemples une fois la scène et utilise ces valeurs tout au long de la scène.
-   `AVCaptureExposureModeContinuousAutoExposure` – Exemples de la scène en continu pour vous assurer qu’il est bien allumé.


Le `ExposurePointOfInterest` peut être utilisé pour appuyer pour exposer la scène en sélectionnant un objet cible pour exposer sur, et l’application peut surveiller le `AdjustingExposure` propriété pour voir quand exposition est répartie.

### <a name="new-exposure-controls-in-ios-8"></a>Nouveaux contrôles d’exposition dans iOS 8

Outre les fonctionnalités déjà fournies par iOS 7 et versions ultérieures, les fonctionnalités suivantes sont désormais disponibles pour contrôler l’exposition dans iOS 8 :

-  Entièrement manuelle exposition personnalisée.
-  Get, Set et clé-valeur observer IOS et la vitesse d’obturation (durée).


Pour implémenter les fonctionnalités ci-dessus, un nouveau `AVCaptureExposureModeCustom` mode a été ajouté. Lorsque l’appareil photo dans est le mode personnalisé, le code suivant peut être utilisé pour ajuster la durée d’exposition et ISO :

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

Dans les modes Auto et verrouillé, l’application peut ajuster le décalage de la routine d’exposition automatique en utilisant le code suivant :

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Les plages minimale et maximale de paramètre dépendent de l’appareil de que l’application est en cours d’exécution, ils doivent jamais être codée en dur. Au lieu de cela, utilisez les propriétés suivantes pour obtenir les plages de valeurs minimale et maximale :

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Comme indiqué dans le code ci-dessus, le périphérique de Capture doit être verrouillé pour la configuration avant d’effectuer une modification de l’exposition.

### <a name="manual-exposure-example"></a>Exemple d’exposition manuelle

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image12.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image12.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Un `UISegmentedControl` qui modifie le Mode Focus à partir d’automatique sur verrouillé.
-  Quatre `UISlider` contrôles qui seront affichent et mettre à jour le décalage, la durée, l’ISO et l’écart.


Procédez comme suit pour AutoEventWireup le contrôleur d’affichage pour le contrôle de l’exposition manuelle :


1. Ajoutez le code suivant à l’aide d’instructions :
    
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
  
1. Remplacer le `ViewDidLoad` méthode et ajoutez le code suivant :

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
  
1. Remplacer le `ViewDidAppear` méthode et ajoutez le code suivant pour démarrer l’enregistrement lorsque la vue chargée :

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

    [![](intro-to-manual-camera-controls-images/image13.png "Les curseurs passera automatiquement comme la caméra ajuste l’exposition")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Appuyez sur le segment verrouillé et faites glisser le curseur de décalage pour ajuster le décalage de l’exposition automatique manuellement :

    [![](intro-to-manual-camera-controls-images/image14.png "Ajustement de l’écart de l’exposition automatique manuellement")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Appuyez sur le segment personnalisé et faites glisser les curseurs de durée et ISO pour contrôler l’exposition manuellement :

    [![](intro-to-manual-camera-controls-images/image15.png "Faites glisser les curseurs de durée et ISO pour contrôler l’exposition manuellement")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Arrêter l’application.


Le code ci-dessus a montré comment faire pour surveiller les paramètres de l’exposition lors de l’appareil photo est en mode automatique et comment utiliser les curseurs pour contrôler l’exposition lorsqu’il est dans les modes verrouillé ou personnalisé.

## <a name="manual-white-balance"></a>Équilibre manuel du blanc

Solde de blanc contrôles permettent aux utilisateurs d’ajuster le solde de colosr dans une image à leur donner l’aspect plus réaliste. Différentes sources de lumière ont des températures de couleur différente, et les paramètres de l’appareil photo utilisés pour capturer une image doivent être ajustées pour compenser ces différences. Là encore, en permettant à l’utilisateur de contrôler l’équilibre des blancs qu’ils peuvent effectuer des ajustements professionnels les routines automatique sont incapables d’atteindre de nouveaux effets artistiques.

[![](intro-to-manual-camera-controls-images/image16.png "Un exemple d’image montrant les réglages de l’équilibre des blancs manuel")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Par exemple, l’heure d’été a un cast blueish, tandis que les lumières incandescence tungsten comportent une teinte plus chaude jaune-orange. (Il s’agisse, les couleurs « froides » ont des températures de couleur plus élevées que les couleurs « à chaud ». Couleur températures sont une mesure physique, pas perceptuelles une).

L’esprit humain est très efficace pour compenser les différences de température de couleur, mais il s’agit d’un élément qui ne peut pas un appareil photo. L’appareil photo fonctionne en insistant couleur de la gamme opposée à des différences de couleur.

La nouvelle iOS 8 exposition API permet à l’application prendre le contrôle du processus et de fournir un contrôle précis sur les paramètres de l’équilibre des blancs de la caméra.

### <a name="how-white-balance-works"></a>Comment équilibrer Works

Avant d’aborder les détails de contrôle de l’équilibre des blancs dans une application IOS 8. Jetons un œil rapide à blanc comment équilibrer works :

Dans l’étude de la perception de la couleur, la [CIE 1931 RVB des couleurs espace et espace de couleurs CIE 1931 XYZ](http://en.wikipedia.org/wiki/CIE_1931_color_space) sont des espaces de couleurs mathématiquement pour la première. Ils ont été créés par la Commission internationale de l’éclairage (CIE) dans 1931.

[![](intro-to-manual-camera-controls-images/image17.png "Espace de couleurs de l’espace de couleurs RVB de CIE 1931 et CIE 1931 XYZ")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Le graphique ci-dessus nous montre toutes les couleurs visible à le œil humain, à partir des niveaux bleu, vert vif rouge clair. N’importe quel point dans le diagramme peut être tracée avec une valeur X et Y, comme indiqué sur le graphique ci-dessus.

Visible dans le graphique, il existe des valeurs X et Y peuvent être tracées sur le graphique qui serait en dehors de la plage de la vision humaine, et par conséquent ces couleurs ne peut pas être reproduits par un appareil photo.

La courbe la plus petite dans le graphique ci-dessus est appelée le [Planckian emplacement](http://en.wikipedia.org/wiki/Planckian_locus), qui exprime la température de couleur (en degrés kelvin), les plus élevées sur le côté bleu (plus chaud) et des chiffres d’inférieur sur le côté rouge (REFROIDISSEUR). Ils sont utiles pour les situations d’éclairage classique.

Dans les conditions d’éclairage mixte, les ajustements de l’équilibre des blancs devrez s’écarter de l’emplacement Planckian pour apporter les modifications requises. Dans ces situations, que l’ajustement devra être décalés soit sur le vert ou mettre à l’échelle côté rouge/magenta de la CIE.

les appareils iOS compensent les casts de couleur en insistant le gain de couleur opposé. Par exemple, si une scène a trop de bleu, le gain rouge optimisé pour compenser. Ces valeurs sont étalonnés pour des appareils spécifiques afin qu’ils soient dépend du périphérique de gain.

### <a name="existing-white-balance-controls"></a>Équilibre des contrôles existants

iOS 7 et versions ultérieures fourni les contrôles de solde de blanc existants suivants via `WhiteBalanceMode` propriété :

-   `AVCapture WhiteBalance ModeLocked` – Exemples de la scène en une seule fois et à l’aide de ces valeurs tout au long de la scène.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – Exemples de la scène en continu pour vous assurer qu’il est bien équilibré.


Et l’application peut surveiller le `AdjustingWhiteBalance` propriété pour voir quand exposition est répartie.

### <a name="new-white-balance-controls-in-ios-8"></a>Nouveaux contrôles de solde de blanc dans iOS 8

Outre les fonctionnalités déjà fournies par iOS 7 et versions ultérieures, les fonctionnalités suivantes sont désormais disponibles pour contrôler la Balance des blancs dans iOS 8 :

-  Prend le contrôle entièrement manuels de l’appareil RVB.
-  Get, Set et clé-valeur observer l’appareil RVB gains.
-  Prise en charge pour équilibrer blanc à l’aide d’une carte de gris.
-  Routines de conversion vers et depuis les espaces de couleurs indépendants de périphérique.


Pour implémenter les fonctionnalités ci-dessus, le `AVCaptureWhiteBalanceGain` structure a été ajoutée avec les membres suivants :

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Le gain de solde blanc maximal est actuellement quatre (4) et peut être prêt à partir de la `MaxWhiteBalanceGain` propriété. La plage légale consiste donc à partir d’un (1) à `MaxWhiteBalanceGain` (4) actuellement.

Le `DeviceWhiteBalanceGains` propriété peut être utilisée pour observer les valeurs actuelles. Utilisez `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` pour ajuster le solde gagne lors de l’appareil photo est en mode verrouillé équilibre des blancs.

#### <a name="conversion-routines"></a>Routines de conversion

Routines de conversion ont été ajoutées à iOS 8 pour faciliter la conversion vers et à partir, les espaces de couleurs indépendants de périphérique. Pour implémenter les routines de conversion, le `AVCaptureWhiteBalanceChromaticityValues` structure a été ajoutée avec les membres suivants :

-   `X` -est une valeur comprise entre 0 et 1.
-   `Y` -est une valeur comprise entre 0 et 1.


Un `AVCaptureWhiteBalanceTemperatureAndTintValues` structure a également été ajoutée avec les membres suivants :

-   `Temperature` -flottante valeur à virgule en degrés Kelvin.
-   `Tint` -est un décalage à partir de vert ou magenta à partir de 0 à 150 avec des valeurs positives vers la direction de vert et négatif vers dans le magenta.


Utilisez le `CaptureDevice.GetTemperatureAndTintValues`et `CaptureDevice.GetDeviceWhiteBalanceGains`des méthodes pour convertir entre la température et teinte, chromatiques RVB et obtenir des espaces de couleurs.

> [!NOTE]
> Les routines de conversion sont plus précises, la valeur à convertir est proche à l’emplacement Planckian.




#### <a name="gray-card-support"></a>Prise en charge de la carte en gris

Apple utilise le terme gris monde pour faire référence à la prise en charge de la carte de gris intégrée à iOS 8. Il permet à l’utilisateur pour vous concentrer sur une carte gris physique qui couvre au moins 50 % du centre de l’image et l’utilise pour ajuster le solde de blanc. L’objectif de la carte gris consiste à atteindre blanc apparaît neutre.

Cela peut être implémenté dans une application en invitant l’utilisateur à placer une carte physique grise devant la caméra, analyse le `GrayWorldDeviceWhiteBalanceGains` propriété et en attente jusqu'à ce que les valeurs régler vers le bas.

L’application serait alors la verrouiller les gains de solde de blanc pour la `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` méthode en utilisant les valeurs à partir de la `GrayWorldDeviceWhiteBalanceGains` propriété pour appliquer les modifications.

Le périphérique de Capture doit être verrouillé pour la configuration avant d’effectuer une modification dans la Balance des blancs.

### <a name="manual-white-balance-example"></a>Exemple d’équilibre manuel du blanc

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image18.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image18.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Un `UISegmentedControl` qui modifie le Mode Focus à partir d’automatique sur verrouillé.
-  Deux `UISlider` contrôles qui seront affichent et mettre à jour la température et la teinte.
-  Un `UIButton` utilisé pour échantillonner un espace de carte de gris (gris World) et de définir un équilibre blanc à l’aide de ces valeurs.


Procédez comme suit pour AutoEventWireup le contrôleur d’affichage de contrôle du solde blanc manuel :


1. Ajoutez le code suivant à l’aide d’instructions :

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
  
1. Ajoutez la méthode privée suivante pour définir le blanc nouvelle équilibrer la température et teinte :

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
  
1. Remplacer le `ViewDidLoad` méthode et ajoutez le code suivant :

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
  
1. Remplacer le `ViewDidAppear` méthode et ajoutez le code suivant pour démarrer l’enregistrement lorsque la vue chargée :

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
  
1. Enregistrer les modifications du code et d’exécuter l’application.
1. Avec l’appareil photo dans le mode Auto, les curseurs déplacera automatiquement comme la caméra règle blancs :

    [![](intro-to-manual-camera-controls-images/image19.png "Les curseurs passera automatiquement comme la caméra règle blancs")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Appuyez sur le segment verrouillé et faites glisser les curseurs Temp et teinte pour ajuster l’équilibre des blancs manuellement :

    [![](intro-to-manual-camera-controls-images/image20.png "Faites glisser les curseurs Temp et teinte pour ajuster l’équilibre des blancs manuellement")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Avec le segment verrouillé étant toujours sélectionné, placez une carte physique gris au premier plan de l’appareil photo et appuyez sur le bouton gris carte pour ajuster l’équilibre des blancs au monde gris :

    [![](intro-to-manual-camera-controls-images/image21.png "Appuyez sur le bouton gris carte pour ajuster l’équilibre des blancs au monde gris")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Arrêter l’application.

Le code ci-dessus a montré comment faire pour surveiller les paramètres de l’équilibre des blancs lorsque l’appareil photo est en mode automatique ou utiliser des curseurs pour contrôler l’équilibre des blancs lorsqu’il est en mode verrouillé.

## <a name="bracketed-capture"></a>Capture entre crochets

La mise entre parenthèses de Capture est basée sur les paramètres des contrôles photo manuel présenté ci-dessus et permet à l’application capturer un moment donné, dans un large éventail de différentes façons.

En termes simples, mise entre parenthèses de Capture est une rafale de toujours images prises avec un large éventail de paramètres d’image à l’image.

[![](intro-to-manual-camera-controls-images/image22.png "Comment fonctionne la mise entre parenthèses de Capture")](intro-to-manual-camera-controls-images/image22.png#lightbox)

À l’aide de la mise entre parenthèses de Capture dans iOS 8, une application peut prédéfinir une série de contrôles manuels de caméra, émettre une commande unique et un ont la scène actuelle retourner une série d’images pour chacun des présélections manuelles.

### <a name="bracketed-capture-basics"></a>Principes de base Capture entre crochets

Là encore, la mise entre parenthèses de Capture est une rafale de toujours images prises avec divers paramètres à partir de l’image sur l’image. Les types de mise entre parenthèses de Capture disponibles sont :

-  **Auto exposition crochet** – où toutes les images ont une quantité de décalage variée.
-  **Manuelles exposition crochet** – où toutes les images ont une vitesse d’obturation (durée) et l’ISO variées quantité.
-  **Crochet de rafale simple** – une série d’images fixes prises en succession rapide.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Nouvelle mise entre parenthèses capturer les contrôles dans iOS 8

Toutes les commandes de mise entre parenthèses de Capture sont implémentées dans le `AVCaptureStillImageOutput` classe. Utilisez le `CaptureStillImageBracket`méthode pour obtenir une série d’images avec le tableau donné de paramètres.

Deux nouvelles classes ont été implémentées pour gérer les paramètres :

-   `AVCaptureAutoExposureBracketedStillImageSettings` : Il possède une propriété, `ExposureTargetBias`, utilisé pour définir le décalage pour un crochet d’exposition automatique. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` : Il a deux propriétés, `ExposureDuration` et `ISO`, utilisé pour définir la vitesse d’obturation et ISO pour un crochet manuelles exposition. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>Contrôle de Capture entre crochets choses à faire et

#### <a name="dos"></a>Choses à faire

Voici une liste des éléments qui doit être effectuée lorsque des contrôles sont à l’aide de la mise entre parenthèses de Capture dans iOS 8 :

-  Préparer l’application pour la capture pire situation en appelant le `PrepareToCaptureStillImageBracket` (méthode).
-  Supposons que les mémoires tampons exemple vont proviennent du même pool partagé.
-  Pour libérer la mémoire allouée par un appel précédent de la préparation, appelez `PrepareToCaptureStillImageBracket` à nouveau et l’envoyer à un tableau d’un objet.


#### <a name="donts"></a>Avertissements

Voici une liste des éléments qui ne doit pas être effectuée lorsque des contrôles sont à l’aide de la mise entre parenthèses de Capture dans iOS 8 :

-  Ne mélangez pas encadrée de capturer des types de paramètres dans une seule capture.
-  Ne pas demander plus de `MaxBracketedCaptureStillImageCount` images dans une seule capture.


### <a name="bracketed-capture-details"></a>Détails de Capture entre crochets

Les informations suivantes doivent être prises en considération lorsque vous travaillez avec la mise entre parenthèses de Capture dans iOS 8 :

-  Les paramètres entre crochets remplacent temporairement le `AVCaptureDevice` paramètres.
-  Flash et paramètres de stabilisation d’image sont ignorés.
-  Toutes les images doivent utiliser le même format de sortie (jpeg, png, etc.).
-  Aperçu vidéo peut-être supprimer des images.
-  Capture entre crochets est prise en charge sur tous les appareils compatibles avec iOS 8.


Avec ces informations à l’esprit, jetons un œil à un exemple d’utilisation de la mise entre parenthèses de Capture dans iOS 8.

### <a name="bracket-capture-example"></a>Exemple de Capture de crochet

Avec le code de configuration de Capture AV générale en place, un `UIViewController` peut être ajouté à la table de montage séquentiel de l’application et configuré comme suit :

[![](intro-to-manual-camera-controls-images/image23.png "Un UIViewController peut être ajouté aux table de montage séquentiel d’applications et configuré comme illustré ici")](intro-to-manual-camera-controls-images/image23.png#lightbox)

La vue contient les éléments principaux suivants :

-  Un `UIImageView` qui affiche le flux vidéo.
-  Trois `UIImageViews` qui affiche les résultats de la capture.
-  Un `UIScrollView` pour héberger les flux vidéo et les vues de résultat.
-  Un `UIButton` utilisé pour prendre une Capture entre crochets avec certains paramètres prédéfinis.


Procédez comme suit pour AutoEventWireup le contrôleur d’affichage pour la Capture de la mise entre parenthèses :


1. Ajoutez le code suivant à l’aide d’instructions :

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
  
1. Ajoutez la méthode privée suivante pour générer les vues d’image de sortie requis :

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
  
1. Remplacer le `ViewDidLoad` méthode et ajoutez le code suivant :
    
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
    
  
1. Remplacer le `ViewDidAppear` méthode et ajoutez le code suivant :

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
    
1. Enregistrer les modifications du code et d’exécuter l’application.
1. Frame d’une scène et appuyez sur le bouton capturer le crochet :

    [![](intro-to-manual-camera-controls-images/image24.png "Une scène de trame et appuyez sur le bouton capturer le crochet")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Effectuez un balayage de droite à gauche pour afficher les trois images prises par la mise entre parenthèses de Capture :

    [![](intro-to-manual-camera-controls-images/image25.png "Effectuez un balayage de droite à gauche pour afficher les trois images prises par la mise entre parenthèses de Capture")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Arrêter l’application.


Le code ci-dessus a montré comment configurer et prendre une exposition automatique entre crochets capturer dans iOS 8.

## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons couvert une introduction aux nouveaux contrôles d’appareil photo manuel fourni par iOS 8 et abordé les principes de base de ce qu’ils font et comment elles fonctionnent. Nous avons fourni des exemples de point manuelle, exposition manuelle et équilibre des blancs manuel. Enfin, nous a donné un exemple en prenant une mise entre parenthèses Capture l’utilisation des contrôles photo manuelle décrite précédemment

## <a name="related-links"></a>Liens associés

- [ManualCameraControls (exemple)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Introduction à iOS 8](~/ios/platform/introduction-to-ios8.md)
