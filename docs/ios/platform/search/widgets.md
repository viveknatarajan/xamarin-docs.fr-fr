---
title: Recherche et l’écran d’accueil des améliorations Widget dans iOS 10
description: Ce document décrit les améliorations apportées par Apple pour les Widgets dans iOS 10, y compris les mises à jour des widgets de l’écran d’accueil et de recherche.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f693b480fff141c177ed135ced60afd65abd77de
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082173"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>Recherche et l’écran d’accueil des améliorations Widget dans iOS 10

_Cet article aborde les améliorations apportées par Apple dans le système de Widget dans iOS 10._

Apple a introduit plusieurs améliorations du système de Widget pour vous assurer que les widgets belles sur n’importe quel arrière-plan qui existe sur le nouveau iOS 10 écran de verrouillage. En outre, les widgets contiennent désormais un [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) propriété qui permet au développeur décrire la quantité de contenu est disponible et permet à l’utilisateur développer et réduire le contenu.

Widgets (également appelé aujourd'hui Extensions) sont un type spécial d’e/s Extension afficher une petite quantité d’informations utiles ou exposer des fonctionnalités spécifiques aux applications en temps voulu. Par exemple, l’application News comporte un widget qui affiche les gros titres et l’application Calendrier fournit deux gadgets différents : un pour afficher les événements et actuels un élément pour afficher les événements à venir.

Widgets sont hautement personnalisables et peuvent contenir des éléments d’interface utilisateur tels que le texte, images, boutons, etc. En outre, le développeur peut personnaliser davantage la disposition de leurs widgets.

[![](widgets-images/widgets01.png "Widgets d’exemple")](widgets-images/widgets01.png#lightbox)

Il existe deux emplacements principales qu’un utilisateur peut afficher et interagir avec les Widgets d’une application :

- **L’écran de recherche** -les utilisateurs peuvent ajouter les Widgets qu’ils trouver plus utiles à leur écran de recherche. L’écran de recherche est accessible en faisant glisser vers la droite sur les écrans d’accueil et de verrouillage.
- **L’écran d’accueil** -à partir de l’écran d’accueil, l’utilisateur peut utiliser 3D Touch pour ouvrir la liste des Actions rapides en appliquant la pression pour l’icône de l’application. Les Widgets d’une application seront affiche au-dessus de la liste Action rapide. Consultez notre [présentation 3D Touch](~/ios/platform/3d-touch.md) documentation pour plus d’informations.

## <a name="widgets-developer-suggestions"></a>Suggestions de développeur de widgets

Dans l’idéal, le développeur doit toujours essayer et concevoir des gadgets que l’utilisateur souhaite ajouter à leurs écrans de recherche. Dans ce but, Apple a les suggestions suivantes :

- **Créer une bonne, expérience Glanceable** -l’utilisateur souhaitez Widgets qui fournissent des informations brèves et glanceable des mises à jour de l’état ou de lui permettent d’effectuer rapidement des tâches simples. Ainsi, en fournissant la bonne quantité d’informations et un élément essentiel de l’interactivité. Si possible, autorisez l’utilisateur à effectuer une tâche donnée en un simple clic. En outre, étant donné que les Widgets ne prennent pas en charge de panoramique ou le défilement, cela aura prendre en considération dans la conception du Widget.
- **Afficher le contenu rapidement** -Widgets sont conçus pour être glanceable, afin que l’utilisateur ne doit jamais avoir à attendre le contenu à charger une fois un Widget s’affiche. Widgets doivent mettre en cache leur contenu localement afin qu’ils peuvent afficher le contenu récent pendant le chargement du nouveau contenu en arrière-plan.
- **Fournir des marges et remplissage approprié** -Widgets ne doit jamais rechercher encombrés, par conséquent, évitez l’extension de contenu sur les bords de vue d’un Widget. Il doit toujours y avoir une marge de large plusieurs pixels entre les bords et le contenu. Apple suggère également à l’aide de l’icône de l’application, affiché en haut du Widget, comme un repère d’alignement. Si le Widget présente une disposition en grille, assurez-vous qu’il est approprié de remplissage entre les éléments dans la grille et recommencez limiter le nombre d’éléments à quatre max.
- **Utiliser des mises en page Adaptable** -la largeur d’un Widget peut varier en fonction de l’appareil, il s’exécute et l’orientation du périphérique. Hauteur d’un Widget peut également varier en fonction de si elle est affichée dans un état de développé (non pris en charge par tous les Widgets) ou le réduit (la valeur par défaut). Un Widget réduit a une hauteur des lignes de table à peu près deux iOS standard. Le développeur peut demander la taille pour un Widget développé, mais il doit idéalement être inférieure à la hauteur de l’écran. Dans l’état réduit, le Widget doit afficher des informations essentielles uniquement et autonomes. Lorsque l’extension, le Widget doit afficher des informations supplémentaires qui améliorent le contenu principal affiché dans l’état réduit. Widgets indiqués dans la liste d’actions rapide sera uniquement dans un état réduit.
- **Ne pas personnaliser d’arrière-plan du Widget** -Widgets sont affichés sur un arrière-plan clair, Flou fourni par le système. Cela vise à favoriser la cohérence entre les Widgets et améliorer la lisibilité de leur contenu. Évitez d’utiliser une image comme arrière-plan Widget, car il peut entrer en conflit avec peints de verrou et l’écran d’accueil de l’utilisateur.
- **Utiliser la police système en noir ou en gris foncé** - lors de l’affichage de texte dans un Widget, de la police système fonctionne mieux. La police doit être dans une couleur gris ou noir pour se démarquer sur l’arrière-plan de Widget clair, floue.
- **Fournir application Access lorsque approprié** -Widget doit toujours fonctionner séparément à partir de leur application, toutefois, si les fonctionnalités plus approfondies sont requises, le Widget doit être en mesure de lancer l’application pour afficher ou modifier une information spécifique. Ne jamais inclure un bouton « Ouvrir application », simplement permettre à l’utilisateur d’appuyer sur le contenu proprement dit et n’ouvrez jamais un 3e tiers application.
- **Sélectionnez un clair, concis nom du Widget** -icône de l’application et nom son sont toujours affichés sur le contenu du Widget. Apple suggère à l’aide du nom de l’application pour sa Widget principal et un nom clair et concis pour tous les autres qu’il fournit. Lorsque vous fournissez un titre de Widget personnalisé, il doivent être précédés de nom de l’application (par exemple, les cartes à proximité, cartes Restaurants, etc.).
- **Informer lorsque l’authentification ajoute la valeur** : si des fonctionnalités supplémentaires ou des informations est disponible uniquement lorsque l’utilisateur est authentifié et connecté, présenter à l’utilisateur. Par exemple, une application de partage de course peut indiquer « Connexion à tour de livre ».
- **Sélectionnez un Widget de liste d’Action rapide** -si l’application fournit plusieurs Widget, le développeur doit choisir celui dans lequel présenter lorsque l’utilisateur fait apparaître la liste d’actions rapides en appliquant la pression pour l’icône de l’application à l’aide de 3D Touch.

Pour plus d’informations sur l’utilisation des widgets, veuillez consulter notre [Introduction aux Extensions](~/ios/platform/extensions.md), [présentation 3D Touch](~/ios/platform/3d-touch.md) documentation et Apple [Guide de programmation de l’Extension application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Utilisation de dynamisme de notre place

Dynamisme de notre place garantit que le texte d’un Widget reste lisible sur light du Widget, arrière-plan flou (fourni par le système). Avant d’iOS 10, le développeur utiliserait un [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) pour dynamisme de notre place du Widget. Exemple :

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Cela a déconseillés dans iOS 10 et doit être remplacé par un [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) ou un [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Exemple :

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Utilisation de Widgets développés et réduites

Nouveau à iOS 10, widgets contiennent désormais un [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) propriété qui permet au développeur décrire la quantité de contenu est disponible et permet à l’utilisateur développer et réduire le contenu.

Lorsqu’un Widget s’affiche initialement, il est dans un état réduit. Un Widget réduit a une hauteur des lignes de table à peu près deux iOS standard. Le développeur peut demander la taille pour un Widget développé, mais il doit idéalement être inférieure à la hauteur de l’écran. 

Dans l’état réduit, le Widget doit afficher des informations essentielles uniquement et autonomes. Lorsque l’extension, le Widget doit afficher des informations supplémentaires qui améliorent le contenu principal affiché dans l’état réduit. Par exemple, l’application Météo affiche les conditions météorologiques actuelles lorsque réduite et ajoute le toutes les heures de prévision développée.

Widgets indiqués dans la liste d’actions rapide sera uniquement dans un état réduit. Si l’application fournit plusieurs Widget, le développeur doit choisir celui dans lequel présenter lorsque l’utilisateur fait apparaître la liste d’actions rapides en appliquant la pression pour l’icône de l’application à l’aide de 3D Touch.

L’exemple suivant est d’une simple aujourd'hui Extension de (widgets) qui gère les États réduit et développé :

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

Examinons le code spécifique du Mode d’affichage de Widget en détail. Pour informer le système que ce Widget prend en charge l’état développé, il utilise :

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

Cet article a couvert les améliorations apportées par Apple dans le système de Widget dans iOS 10 et illustré comment les implémenter dans Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Introduction aux Extensions](~/ios/platform/extensions.md)
- [Introduction à la 3D Touch](~/ios/platform/3d-touch.md)
- [Guide de programmation d’Extension d’application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
