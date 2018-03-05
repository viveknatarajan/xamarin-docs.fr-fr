---
title: "Recherche et les améliorations de Widget d’écran d’accueil"
description: "Cet article décrit les améliorations Qu'apple a apportées au système de Widget dans iOS 10."
ms.topic: article
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a6749ca9d8a793372ec088433780d622f2f05b41
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>Recherche et les améliorations de Widget d’écran d’accueil

_Cet article décrit les améliorations Qu'apple a apportées au système de Widget dans iOS 10._


Apple a introduit plusieurs améliorations au système de Widget pour vous assurer que les widgets apparaîtront sur n’importe quel arrière-plan qui existe sur le nouveau iOS 10 écran de verrouillage. En outre, les widgets contiennent désormais un [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) propriété qui permet au développeur décrire la quantité de contenu est disponible et permet à l’utilisateur développer et réduire le contenu.

Widgets (également appelé aujourd'hui Extensions) sont un type spécial d’e/s Extension afficher une petite quantité d’informations utiles ou exposer des fonctionnalités spécifiques de l’application en temps voulu. Par exemple, l’application de News a un widget qui affiche les gros titres et l’application Calendrier fournit deux widgets différents : un pour afficher aujourd'hui événements et l’autre pour afficher les événements à venir.

Widgets sont hautement personnalisables et peuvent contenir des éléments d’interface utilisateur tels que le texte, images, boutons, etc. En outre, le développeur peut personnaliser davantage la disposition de leurs widgets.

[ ![](widgets-images/widgets01.png "Widgets d’exemple")](widgets-images/widgets01.png)

Il existe deux emplacements principales qu’un utilisateur peut afficher et interagir avec les Widgets d’une application :

- **L’écran de recherche** -les utilisateurs peuvent ajouter des Widgets ils trouver plus utiles dans l’écran de recherche. L’écran de recherche est accessible en passant à droite sur les écrans d’accueil et de verrouillage.
- **L’écran d’accueil** -à partir de l’écran d’accueil, l’utilisateur peut utiliser 3D Touch pour ouvrir la liste des Actions rapides en appliquant la pression d’icône de l’application. Les Widgets d’une application seront affiche au-dessus de la liste des actions rapides. Veuillez consulter notre [présentation 3D Touch](~/ios/platform/3d-touch.md) documentation pour plus d’informations.

## <a name="widgets-developer-suggestions"></a>Suggestions de développeur de widgets

Dans l’idéal, le développeur doit toujours essayer et concevoir des Widgets que l’utilisateur souhaite ajouter à des écrans de recherche. À cette fin, Apple a les suggestions suivantes :

- **Créer une bonne, parcourez-les expérience** -l’utilisateur souhaitez Widgets qui fournissent des informations brèves, parcourez-les des mises à jour de l’état ou les autoriseront à exécuter plus rapidement des tâches simples. Ainsi, en fournissant la quantité exacte d’informations et d’un élément essentiel de l’interactivité. Chaque fois que possible, autoriser l’utilisateur à effectuer une tâche donnée d’un seul clic. En outre, étant donné que les Widgets ne prennent pas en charge le panoramique ou le défilement, cela aura prendre en considération dans la conception du son.
- **Afficher rapidement le contenu** -Widgets sont conçues pour être parcourez-les, l’utilisateur ne doit jamais avoir à attendre le contenu à charger une fois qu’un Widget s’affiche. Widgets doivent mettre en cache leur contenu localement afin qu’ils peuvent afficher le contenu récent pendant le chargement de contenu actualisé en arrière-plan.
- **Fournir des marges et remplissage approprié** -Widgets ne doit jamais rechercher encombrés, évitez d’extension de contenu sur les bords de vue d’un Widget. Il doit toujours y avoir une marge de large plusieurs pixels entre les bords et le contenu. Apple suggère également à l’aide de l’icône de l’application, affichée en haut du Widget, comme un repère d’alignement. Si le Widget présente une disposition en grille, vérifiez qu’il existe approprié de remplissage entre les éléments dans la grille et essayer de limiter le nombre d’éléments à quatre max.
- **Utiliser des dispositions adaptables** -largeur d’un Widget varie en fonction de l’appareil, il s’exécute sur et l’orientation du périphérique. Hauteur d’un Widget peut également varier selon si elle est affichée dans un état de l’étendue (non pris en charge par tous les Widgets) ou le réduit (la valeur par défaut). Un Widget réduit a une hauteur des lignes de table à peu près deux iOS standard. Le développeur peut demander la taille d’un Widget de développé, mais il doit idéalement être inférieure à la hauteur de l’écran. Dans l’état réduit, le Widget doit afficher des informations essentielles uniquement et autonomes. Lorsque les étendu, le Widget doit afficher des informations supplémentaires qui améliorent le contenu principal est indiqué dans l’état réduit. Widgets indiquées dans la liste des actions rapides seront dans l’état réduit.
- **Ne pas personnaliser arrière-plan son** -Widgets sont affichés sur un arrière-plan clair, Flou fourni par le système. Pour cela, pour promouvoir la cohérence entre les Widgets et améliorer la lisibilité de leur contenu. Évitez d’utiliser une image comme arrière-plan Widget, car il peut entrer en conflit avec peints de verrou et l’écran d’accueil de l’utilisateur.
- **Utiliser la police système en noir ou en gris foncé** - lors de l’affichage du texte dans un Widget, le fonctionnement de la police système meilleures. La police doit être dans une couleur grise ou noir en évidence sur l’arrière-plan de Widget clair, flou.
- **Fournir application accès lorsque approprié** -Widget doit toujours fonctionner séparément à partir de leur application, toutefois, si une fonctionnalité plus approfondie est requise, le Widget doit être en mesure de lancer l’application pour afficher ou modifier une information spécifique. Ne jamais inclure un bouton « Ouvrir l’application », simplement permettre à l’utilisateur de cliquer sur le contenu proprement dit et ne jamais ouvrir un 3e partie application.
- **Sélectionnez un clair, concis nom du Widget** -icône de l’application et du Widget s’affichent toujours sur le contenu du Widget. Apple suggère à l’aide du nom de l’application pour ses Widget principal et un nom clair et concis, pour tous les autres qu’il fournit. Lorsque vous fournissez un titre de Widget personnalisé, ils doivent avoir pour préfixe avec le nom de l’application (par exemple, les mappages à proximité, Maps Restaurants, etc.).
- **Informer lorsque l’authentification ajoute la valeur** : si des fonctionnalités supplémentaires ou des informations est disponible uniquement lorsque l’utilisateur est authentifié et connecté, il présente à l’utilisateur. Par exemple, porté mon dites « Sign in to porté de livre » application de partage.
- **Sélectionnez un Widget de liste Action rapide** -si l’application fournit plus d’un Widget, le développeur doit choisir celle qui doit être présent lorsque l’utilisateur affiche la liste des actions rapides en appliquant la pression à l’icône de l’application à l’aide de la 3D Touch.

Pour plus d’informations sur l’utilisation des widgets, veuillez consulter notre [Introduction aux Extensions](~/ios/platform/extensions.md), [présentation 3D Touch](~/ios/platform/3d-touch.md) documentation et Apple [Guide de programmation de l’Extension application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Utilisation de dynamisme de notre place

Dynamisme de notre place garantit que le texte d’un Widget reste lisible lorsque présentés sur lumière du Widget, arrière-plan flou (fourni par le système). Avant d’iOS 10, le développeur utiliserait un [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) pour dynamisme de notre place du Widget. Exemple :

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Cela a déconseillés dans iOS 10 et doit être remplacé par un [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) ou un [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Exemple :

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Utilisation des Widgets développés et réduites

Nouveau pour iOS 10, les widgets contiennent désormais un [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) propriété qui permet au développeur décrire la quantité de contenu est disponible et permet à l’utilisateur développer et réduire le contenu.

Lorsqu’un Widget est initialement affiché, elle est dans un état réduit. Un Widget réduit a une hauteur des lignes de table à peu près deux iOS standard. Le développeur peut demander la taille d’un Widget de développé, mais il doit idéalement être inférieure à la hauteur de l’écran. 

Dans l’état réduit, le Widget doit afficher des informations essentielles uniquement et autonomes. Lorsque les étendu, le Widget doit afficher des informations supplémentaires qui améliorent le contenu principal est indiqué dans l’état réduit. Par exemple, l’application Météo affiche la météo réduite et ajoute l’horaire prévision développée.

Widgets indiquées dans la liste des actions rapides seront dans l’état réduit. Si l’application fournit plus d’un Widget, le développeur doit choisir celle qui doit être présent lorsque l’utilisateur affiche la liste des actions rapides en appliquant la pression à l’icône de l’application à l’aide de la 3D Touch.

L’exemple suivant est une simple aujourd'hui Extension (Widget) qui gère les États réduit et développé :

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

Examinons le code spécifique du Mode d’affichage de Widget en détail. Pour indiquer au système que ce Widget prend en charge l’état étendu, il utilise :

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Pour obtenir le mode d’affichage actuel du Widget, il utilise :

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Pour obtenir la taille maximale pour l’état développé ou réduit, il utilise :

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

Et pour gérer l’état de modification (Mode d’affichage), il utilise :

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

Outre la définition de la taille demandée pour chaque état (réduit ou développé), il met également à jour le contenu affiché pour correspondre à la nouvelle taille.

## <a name="summary"></a>Récapitulatif

Cet article a couvert les améliorations Qu'apple a apportées au système de Widget dans iOS 10 et montre comment les implémenter dans Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Présentation des Extensions](~/ios/platform/extensions.md)
- [Introduction à la 3D Touch](~/ios/platform/3d-touch.md)
- [Guide de programmation d’Extension d’application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
